# sum of experiences on DL experiments
summarize of the experiences on deep learning experiments

It is too often these days that one spent several hours to implement a model but several days on implementation details to get it actually work. Most of the time are spent on such time-costing iterations, usually due to either lack of background knowledge or lack of enough details from the author or institution. The best way when one fall into such difficulties is to directly check the official implementation and try to modify all the possible different places and see what happens. Here I woulld like to summarize things that seems small but greatly affect whether the model works correctly.

1. Data set description and format
  The very first steps when you touch a new data set include the order and dimension of how data is stored. I remember getting cifar10/100 images in the wrong way the first time I touch it. I thought the 3072=1024*3=32*32*3 and directly reshape it into (32,32,3). But it was shown in the official document that the data is actually stored like (1024 B,1024 G,1024 R), which is channel first. So carefully check out how data is stored and described in the official document is the vital first step.
2. Preprocess
  Checking how the data are preprocessed by the author of a model/paper/project is also important. This typically include scaling, normalizing, the axis/dimension of normalizing, resizing, down-samplings like cropping (for images), onehot encoding for labels etc.
  
3. value scale consistency of inputs, weights and labels
  This is one of the most important thing to notice and verify. The final "boss of bug" I found in two projects end up to be value scale problem. For DCGAN, the network doesn't converge on cifar10 when the input random vector is in the scale (0,1), but easily converge when I change the inputs' scale to (-1,1) when I was using leaky_relu and tanh. For binary connect, the network doesn't converge when I was using relu as activation, because the network only have -1 and 1 weights for forward and backward step and using relu kills every cell coming from -1 connection. The network somehow need minus operation to get to the right place. Such kind of coherence of scale is vital for some models even to converge.

4. proper learning rate and batch_size
  These two do not affect convergence greatly except you set a too large learning rate. But it can be misleading and make you suspect about implementation when the network converges too slow. The do exist a good learning rate, smaller then which brings too slow convergence and larger then which the model doesn't move forward(loss doesn't going down). For batch size, set it as large as several hundreds if the memory allows.

-------------------
to be updated


