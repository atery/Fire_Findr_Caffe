Examples
===

### ILSVRC12 Example

The first example involves the network architecture from Krizhevsky et el submitted to Advances in Neural Information Processing Systems (NIPS) 2012 (ILSVRC12). The original paper is available [here](http://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks).

The use of this example is that guides one in:

* Reproducing their results from ILSVRC12.

* Incorporating your own images to make a classifier specific to your own task.

I will go over the specifics in how this is done to fill in the instructions given [here](http://caffe.berkeleyvision.org/gathered/examples/imagenet.html).


**reproducing ILSVRC12**

First download the auxiliary files `train.txt` and `val.txt ` by running.

```
./get_ilsvrc_aux.sh
```

Register with NIPS and download the images from [here](google.com). Read the data into leveldb files that will be used by caffe. To do this the authors have included a the script `create_imagenet.sh`. You will need to modify `RESIZE=false` to `RESIZE=true`. Assuming you have the data in `${caffe_root}/data/ilsvrc12` you can run.

```
./examples/imagenet/create_imagenet.sh
```

This should create two leveldb files `ilsvrc12_val_leveldb` and `ilsvrc12_val_leveldb`. Create the training set image mean file by running.

```
./examples/imagenet/create_imagenet_mean.sh
```

The network definition is in `${caffe_root}/models/bvlc_reference_caffenet/train_val.prototxt` prototxt is the google protocol buffer file format (more info [here](https://developers.google.com/protocol-buffers/)) (it's similar to the json format). This file 

```
include { phase: TRAIN } or include { phase: TEST } 
```

This file also contain the various learning rates and the decay function. In addition, this file specifies the location of the mean pixel density file and trinan lmdv and val lmdb. Finally `solver.partotxt` contains information about the optimization method to be used and points to the `train_val.partotxt` file. Finally we can train by running.

```
./build/tools/caffe train --solver=models/bvlc_reference_caffenet/solver.prototxt
```

This will run and generate 

Additionally, it will generate a series of checkpoint files which contain the model weights and momentum. Having the checkpoint files allows one to restart at that point in the optimisation if the process gets interrupted (for example if you get kicked off your spot instance). The command to restart a computation is.

```
./build/tools/caffe train --solver=models/bvlc_reference_caffenet/solver.prototxt --snapshot=models/bvlc_reference_caffenet/caffenet_train_10000.solverstate
```

**Incorporating your own images**

Generating the lmdb files from your images is done is a similar way as described above. Since likely have a different number of output layers for the original model you will need to manually edit the `train_val.prototet` file to change the number of notes in the last layer.


**Classifying Images**

Once you have generated your `.caffemodel` file from the above you can classify new unseen images. One of the easiest ways  to do this is by making use the caffe python model. The authors provide a ipython notebook on this. I decided to adapt this into a python program with the added functionality of printing the predicted class name instead just the class number. Take a look at it [here](/class.py)

For example this image:

![jpg](https://raw.githubusercontent.com/JBed/Fire_Findr/master/2_Examples/cat.jpg)

is correctly classified as:

```
predicted class: 281
```

### Flickr Style Example

This examples illustrates how caffe can be used to do Transfer Learning (TL). TL is a way to make use of a network trained for one purpose in a different task. Assembling the necessary data files for this example is done by running provided python script.

```
python examples/finetune_flickr_style/assemble_data.py --workers=-1 --images=2000 --seed 831486
```

This should download `solver.prototxt` and `bvlc_reference_caffenet.caffemodel`. Now we can start the training with

```
./build/tools/caffe train -solver models/finetune_flickr_style/solver.prototxt -weights models/bvlc_reference_caffenet/bvlc_reference_caffenet.caffemodel -gpu 0
```


### R-CNN detector Example

Regional convolutional neural networks (R-CNN) classify regions in an image. For the full details of the R-CNN system and model, refer to its project site and the paper [here](https://github.com/rbgirshick/rcnn). This specific example deals with classifying regions in the image below.

![jpg](https://raw.githubusercontent.com/JBed/Fire_Findr/master/2_Examples/fish-bike.jpg)

Here we are making use of the ILSVRC12 example model. The only real difference is that instead of declaring an entire image belonging to one of the 1000 classes we are searching for regions of an image (possible multiple reasons). This can be run using the rcnn.py file wiht:

```
python rcnn.py
```

This should output

![jpg](https://raw.githubusercontent.com/JBed/Fire_Findr/master/2_Examples/fish-bike-detected.jpg)

