Building the Training Data Set
======

### How to download the data

I will use flickr as the source of my images. The images that I am downloading were released  into the public domain under the creative commons license (more info [here](https://www.flickr.com/creativecommons/)). Downloading images with a given "tag" is fairly straightforward with the python program in this folder (see [here](/flickr_download.py)). The challenging part is determining what images to download in order to build an effective training set for our task.


### Discussion: building a good training set.

The most useful training samples for classification with convolutional neural networks are those that lie near the edge of the class distributions in feature space. This concept is illustrated in the below figure from [here](https://github.com/JBed/Fire_Findr/blob/master/3_Training_Data/intel_training.pdf).

![png](https://raw.githubusercontent.com/JBed/Fire_Findr/master/3_Training_Data/training.png)

Therefore, as the authors [here](https://github.com/JBed/Fire_Findr/blob/master/3_Training_Data/intel_training.pdf) state:

>The unequalness of training samples in terms of their value for fitting an appropriate set of decision boundaries should be considered in the design of training data acquisition and refinement strategies for a classification analysis.

Below I analyze the important border cases.

---

**For an image with visible flames the algorithm needs to understand that the object on fire is only important sometimes.**

That is the model need to not just detect flames but the context of the flames, and how this context relates to our goal. For example houses and cars should never be on fire but matches, lanterns, candles all should be on fire. Simply put object that is "burning" need to be recognized. To accomplish this I incorporated these words into my training set:

* positive: house on fire, car on fire, wildfire, brush fire

* negative: match on fire, lantern, candle, fireplace, burning cigarette.

---

**The algorithm needs to understand the importance of smoke.**

For example black smoke is likely from a fire but white "smoke" is likely just a cloud. This distinction is further complicated in images where there are no visible flames. in such a case it would be impossible to determine the source of the flame (eg if this is an unwanted fire). I incorporated this into my training set by using:

* positive: black smoke, fire smoke, 

* negative: clouds, white cloud + blue sky

Distinguishing smoke from a fire from a normal cloud formation can be quite difficult as is shown in the image below.

![jpg](https://raw.githubusercontent.com/JBed/Fire_Findr/master/3_Training_Data/smoke.jpg)

---

**The algorithm needs to be able to recognize damage that was uniquely caused by fire**

For example a fire can damage a house but so also can earthquakes and tornados. While fire will likely occur after an earthquake or tornado (although fires often occur after an earthquake or tornado). I incorporated this into my training set by using:

* positive: fire damage

* negative: earthquake damage, tornado damage


For example distinguishing these two images would need to correctly labeled.

positive:

![jpg](https://raw.githubusercontent.com/JBed/Fire_Findr/master/3_Training_Data/fire_damage.jpg)

negative:

![jpg](https://raw.githubusercontent.com/JBed/Fire_Findr/master/3_Training_Data/tornado_damage.jpg)


---

### Conclusions

The take home message from this section is that

* The individual training samples may vary greatly in importance.

* Complete description of each class in feature space is not required for an accurate classification.

* This relative importance of the training sites is a function of the specific purpose of the classifier.

* Sometimes the most informative training samples can only be identified after an initial training and test round. Therefore, training data set is an iterative process.

