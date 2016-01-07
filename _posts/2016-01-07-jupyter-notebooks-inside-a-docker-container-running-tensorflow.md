---
layout: post
title: Jupyter notebooks inside a Docker container running TensorFlow
date: 2016-01-07 15:22:22
categories: docker tensorflow
---
This was the procedure I followed on my Mac (may be slightly different on
Linux or Windows).

First we map 8888 (Jupyter notebook) on the "inside" to 5000 on the "outside"
and run the image containing TensorFlow:

    docker run -it -v "$PWD":/mnist -p 5000:8888 b.gcr.io/tensorflow/tensorflow:latest-devel    
                                                                                                
(This also maps the current directory to a mount named `/mnist` that lives at
the root level in the docker container.) Then, we launch `ipython notebook` 
inside the image. Next, we get the correct host IP address with:

    docker-machine ls                                                                           
                                                                                                
This will provide a URL, e.g. `tcp://192.168.XYZ.XYZ:ABCD`. Point a browser to
`192.168.XYZ.XYZ:5000` and we will see the notebook.
