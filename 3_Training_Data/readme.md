Building the Training Data Set
======

### How to download the data

I will use flickr as the source of my images. The images that I am downlaoding were realased into the public domain under the crative commons license (more info [here](https://www.flickr.com/creativecommons/)). Downloading images with a given "tag" is fairly straightforward with the python program in this folder (see [here](/flickr_download.py)). The challenging part is determining what images to download in order to build an effective training set for our task.


### Discussion: building a good training set.

After reviewing the literature the most useful training samples for classification with a neural networks are those that lie on the edge of the class distributions in feature space. This concept is ullustrated in the below figire form [here](https://github.com/JBed/Fire_Findr/blob/master/3_Training_Data/intel_training.pdf).

![png](https://raw.githubusercontent.com/JBed/Fire_Findr/master/3_Training_Data/training.png some text)



Consistent with:

---

**For an image with visible flames the algorithm need to understand that the object on fire is only important sometimes.**

For example houses and cars should never be on fire but matches, lanterns, candles both should be on fire. Simply put object that is "burning" need to be recognized. To accomplish this I incorporated these words into my asdf:

* positive: house on fire, car on fire, wildfire, brush fire

* negative: match on fire, lantern, candle, fireplace, cigarette.

---

**The algorithm needs to understand the importance of smoke.**

For example black smoke is likely form a fire but white "smoke" is likely just a cloud. This distinction is further complicated in images where there are no visible flames. in such a case it would be impossible to unrepentant if the source of the flame is of concern. I incorporated this into my training set by using:

* positive: black smoke, fire smoke, 

* negative: clouds, white cloud blue sky

Distinguishing smoke from a fire from a normal cloud formation can be quite difficult as is shown in the image below.

![jpg](asdf)

---

**The algorithm needs to damage that was uniquely caused by fire**

For example a fire can damage a house but so also can earthquakes and tornados. While fire will likely occur after an earthquake or tornado...

for example distinguishing these two images 


* positive: fire damage

* negative: earthquake damage, tornado damage


---


### Conclusions








