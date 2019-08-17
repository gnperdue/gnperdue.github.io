---
layout: post
title: Neural Architecture Search
date: 2018-07-17 12:00:01
categories: ml
---

I'm working on a project now that is using an evolutionary algorithm to perform neural network architecture optimization on the [Titan supercomputer](https://www.olcf.ornl.gov/olcf-resources/compute-systems/titan/) at [Oak Ridge National Laboratory](https://www.olcf.ornl.gov). You can read more about the algorithm [here](http://doi.acm.org/10.1145/2834892.2834896) and [here](http://doi.acm.org/10.1145/3146347.3146355).

[Neural Architecture Search](https://arxiv.org/abs/1611.01578), or NAS, is an attempt to solve the same problem - namely, automatically design a neural network - but with a different approach, namely using reinforcement learning instead of an evolutionary algorithm. There is another, follow-on paper to the original NAS paper that adds a few restrictions to the sort of networks the controller network can create, but in exchange gets a truly _massive_ increase in performance - this is [Efficient Neural Architecture Search](https://arxiv.org/abs/1802.03268), or ENAS. While NAS is very cool, it is ENAS that feels like a real game-changer to me (although there are some restrictions).

# Original NAS paper

## Main ideas

The goal is to _automate_ network architecture design, with the argument being that even if this process is computationally expensive, it can be, in real costs, much cheaper overall because it saves (potentially) a great deal of human time. This argument is sort of fundamental to the concept of productivity and growth, so I think it is reasonable to assert it.

At any rate, the function of the NAS is built around the observation that a network can be specified using a string of tokens to specify layers. If we view these tokens as "actions", we can think of building a network as an analogous problem to a controller solving some task in a reinforcement learning context. The actions effectively assemble the net, and then we may train it, and use the hold-out set accuracy as a reward signal for training based on policy gradients.

In the paper they build both a convolutional neural net for solving CIFAR-10 and a recurrent neural network targeting the Penn-Treebank problem. Personally, I haven't done much NLP work at this point, so while I basically know what is going on in terms of reinforcement learning with policy gradients, I'm not yet familiar with the task associated with Penn-Treebank. Someday.

Assembling a convolutional network structure is fairly straightforward - the only tricky piece is skip connections. They had a set of rules they used to manage shape mismatches. Skip connections here are not residual (why not?) but instead are depth-wise concatenation (like U-net). In general, there is more than one way to do this sort of construction and the specifics are not super-important. They also trained a recurrent cell and this was more complex, but again, their algorithm for assembling a recurrent cell was just one of many possible approaches.

The performance test sections of the paper were straightforward, although there was no explanation of the controller network architecture and no justification for its design. They place some limits on network growth as a function of iteration that seem reasonable, but they are not quantitatively justified in the paper.

The reward signal they use is the best accuracy in the last 5 epochs (out of 50 for convolutional nets, 35 for recurrent nets), cubed (this is a good idea - it helps to differentiate networks with close scores, although this modification is not justified quantitatively, i.e., why cubed and not raised to the fourth power, or exponentiated, or what have you). They did not employ early stopping (it would be interesting to see if there could be some computational savings in that case, but their decision may have been motivated by their distributed training strategy).

They make the point late in the paper that their found-network is at some local optimum after being created, which is what you would expect, but I'm not sure I fully buy their justification. They claim the network performance degrades under perturbations in the structure, but their example "perturbations" are fairly severe and not really the sort of small changes you might think of when hearing that phrase.

The authors do some nice generalization tests on their new recurrent cell and find good behavior, which, again, is what you would expect, but it is still a nice check to see that their recurrent cell can guide networks though problems aside from the specific one it was built using. It is also nice to see a state of the art result with the new cell on a task that was different from the one it was built for.

They have a couple of other nice controls tests, for example, testing with a larger search space and comparing to random search. (Comparing to random search is always important in studies like this - it is such a natural baseline.)


## Big takeaways

It is somewhat impressive this works - one could imagine NAS creating networks that didn’t perform well or getting stuck in bad parts of phase space for computationally prohibitive time. Theoretically, one expects _some_ search strategy to be able to do this, but it isn't obvious _a priori_ that reinforcement learning is efficient enough to make this work. Maybe it is obvious if you try, but it seems impressive they are able to always generate semantically valid networks - it would be interesting to study how they mapped controller network "outputs" to tokens, and how they made sure the ordering of the tokens was always valid, etc. Did they ever produce broken networks? Did they have a good algorithm for checking whether an architecture was valid without having to run a feed-forward pass through the network.

It isn’t clear how important the details of their strategy for distributing the computation are. It also isn’t clear how the computational efficiency compares, say, to an evolutionary strategy - they call evolutionary strategies "slow", but don’t quantify this assertion. (And, as someone who works on evolutionary strategies, I would like to see some justification, ha!)

It seems fairly obvious that it would have been possible to have different rules for building up networks - it would be interesting to understand some of the different strategies they considered. In general, there is no real attention devoted to "lessons learned" in the process of optimization, or analysis of differences in "second best" networks. Broadly - there are lots of papers on automatic network optimization but almost nothing analyzing the optimized networks. This, of course, is because that is a super-hard problem. It is thorny and messy and tied to the data and all sorts of things, but it would be really nice to start _trying_ to do that. After all, the first thing a science does is name everything.

One wonders if Google ran this process on a much larger scale at some point to build up even better networks... why not? Naturally, one would use something like NAS to design a better controller for NAS, that one then used to design a better controller for NAS, and so on... (okay, obviously not really!)


# Efficient NAS paper

## Main ideas

The central idea here is sort of mind-blowing - they build one _huge_ graph of layers, then _sample_ from it to produce child models. The authors phrased the NAS process as sampling sub-graphs of a much larger graph, or as sub-DAGs of a huge parent DAG, and ENAS is utilizing superpositions of these graphs. They then share parameters between child models (all part of the same big graph, after all) - and this sharing is the key to the massive speed ups they get here. Basically, NAS would train models to convergence, then toss them out once the reward signal was collected on the hold-out validation set, and then start all over. By keeping the same weights around, training is much faster. They also share parameters between recurrent cells when training RNNs, which is possibly even more surprising than sharing weights across different convolutional kernels.

The controller network they used was an LSTM with 100 hidden units (why not a [NASCell](https://www.tensorflow.org/api_docs/python/tf/contrib/rnn/NASCell)?). There were two sets of learnable parameters at work - the controller LSTM parameters, and the shared parameters of all the child models. The authors alternated training child models and the controller. First, they would fix the controller policy and perform stochastic gradient descent on the child model parameters. Then, they would fix the child model parameters and train the controller using policy gradients. When training the shared parameters of the child models, the gradient was computed by averaging over models that were sampled and - a _very surprising point_ - one only needs to sample one model to get good convergence!

New architectures are derived by sampling several models from the trained policy, evaluating _a single minibatch_ (emphasis by the authors), picking the model with the highest validation accuracy, and retraining only that model from scratch. It is surprising they only use one minibatch to make this evaluation, and that choice isn't justified.

They performed two versions of convolutional architecture search - one over the full space and another over convolutional cells linked together in a fixed overall architecture. I think of this second approach as a search for a new Inception module.

The authors close by spending some time thinking about whether ENAS is good at searching model space or wether they have good success because they designed a good search space to work with in the first place. They do the classic test here, which is to compare to a random network. They also disable training of the ENAS controller, so network evolution is conducted using a random policy, which seems like a similar test. In both cases they find significantly worse performance than when they use and train the ENAS controller.

## Big takeaways

In exchange for limiting the architecture space in some important aspects - for example, one cannot arbitrarily evolve the kernel shaps in ENAS because we need many sub-graphs to be compatible with each other - we get a very large increase in effective speed. This is made possible by the surprising facts that learned weights in one sub-graph transfer nicely to other sub-graphs and, furthermore, we only need to train one sub-graph per iteration to evaluate the performance of multiple sub-graphs (these observations are closely related, of course).
