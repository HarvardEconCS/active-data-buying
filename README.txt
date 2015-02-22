Code for paper "Actively Purchasing Data for Learning"
Jacob Abernethy
Yiling Chen
Chien-Ju Ho
Bo Waggoner  <bwaggoner@fas.harvard.edu>


------------------------------------------
-- Prerequisites

- python2.7.
- numpy
- pandas (version 15)
- scikit-learn (for downloading the data set conveniently)
- matplotlib for plotting results of simulations

Note for linux: some package managers have outdated versions of
pandas that are not compatible with this code.
The easiest way to get a recent version is probably to install via e.g.
$ apt-get install python-pip
$ pip install numpy
$ pip install pandas
$ pip install scikit-learn


-----------------------------------------
-- Running

$ python run.py

The first time you run it, downloads the MNIST training dataset to ./mldata/mnist-original.mat
After running, writes the results to the file "plot.py".

$ python plot.py

visualizes the results.

-----------------------------------------
-- Overview of the files

run.py: Do some number of trials for each mechanism, each method of generating costs, and each budget. Prints the data to plot.py, which can also be run as a python file to visualize the data.

run.py interacts with three types of classes: Algorithms (currently only gradient descent), Mechanisms (can be initialized with access to any algorithm), and Cost Generators (draw costs for a given data point according to some distribution).

Algorithms:
   -- implement methods: reset(eta), test_error(X,Y), norm_grad_loss(x,y), data_update(x,y,importance_wt), null_update().
  grad_desc.py: implementation of gradient descent algorithm.

Mechanisms:
  -- implement methods: reset(eta, T, B, cmax=1.0), train_and_get_err(costs,Xtrain,Ytrain,Xtest,Ytest)
  baseline.py: has unlimited budget, so buys every data point
  naive.py: offer fixed price to each data point until budget is exhausted
  rs12.py: implementation of generalization of RothSchoenebeck12 assuming that costs are uniform[0,cmax]
  ours.py: our posted-price and learning mechanism

Cost-generating:
  -- implementing methods normalize(num_examples[]), draw_cost(label)
	unif_costgen.py: costs are uniform [0,1] marginal. Can specify some labels as "expensive" and others as "cheap", and then expensive label costs are uniform [a,1] and cheap labels uniform [0,a] for some a.
	bernoulli_costgen.py: costs are free with probability 1-p and cost 1 with probability p. Can specify some labels as expensive and others as cheap, and then expensive labels will cost 1 when possible subject to the (p,1-p) distribution.
