# Traffic Lights Classifier

In this project, you’ll use your knowledge of computer vision techniques to build a classifier for images of traffic lights! You'll be given a dataset of traffic light images in which one of three lights is illuminated: red, yellow, or green.

| ![all_lights](images/all_lights.png) |
|:---:|
| **Images from the dataset. Left to right: red, green, and yellow traffic lights** |

## Classification Steps

In the provided notebook, you'll pre-process these images, extract features that will help distinguish the different types of images, and use those features to classify the traffic light images into three categories: red, yellow, or green. The tasks will be broken down into a few sections:

1. Loading and visualizing the data. The first step in any classification task is to be familiar with your data; you'll need to load in the images of traffic lights and visualize them!

2. Pre-processing. The input images and output labels need to be standardized; that is, all the input should be of the same type of data and of the same size, and the output should be a numerical label. This way, you can analyze all the input images using the same procedures, and you know what output to expect when you eventually classify a new image.

| ![processing_steps](images/processing_steps.png) |
|:---:|
| **Pre-processed, standardized images** |

3. Feature extraction. Next, you'll extract some features from each image that will be used to distinguish and classify these images. This is where you have a lot of creativity; features should be 1D vectors or even single values that provide some information about an image that can help classify it as a red, yellow, or green traffic light.

| ![feature_ext_steps](images/feature_ext_steps.png) |
|:---:|
| **An example of feature extraction steps** |

4. Classification and visualizing error. Finally, you'll write one function that uses your features to classify any traffic light image. This function will take in an image and output a label. You'll also be given code to classify a test set of data, compare your predicted label with the true label, and determine the accuracy of your classification model.

5. Evaluate your model. To pass this project, your classifier must be >90% accurate and never classify any red lights as green; it's likely that you'll need to improve the accuracy of your classifier by changing existing features or adding new features. I'd also encourage you to try to get as close to 100% accuracy as possible!

Next, read through the instructions and get ready to build a classifier!

---

## Algorithm

1. Resize the images to 32 x 32 shape.
2. I converted the image to HSV format from RGB format.
3. Then I separated the value channel of the image.
4. I analyzed the features based on the "value" channel of the HSV color image.
5. Assuming that the traffic lights are always positioned in the order of Red at the top, yellow at middle and green at the bottom, I partitioned my 32 x 32 image into three different sections along the vertical direction - Top, Middle and Bottom.
6. Based on this partition, I am checking the maximum brightness level in the image by summing up the .
7. At the location of maximum brightness, there is higher probability that I will have the traffic light ON. Judging by the location of light (top, bottom or middle), I can get an estimate of the light label.

Here is the code within the ***create_feature()*** function which takes as input one image and calculates the sum of all pixel values within the top, middle and bottom sections of the image:

```python
# Isolate the value component
v = hsv_im[:,:,2]

# Sum the V components over every column for all columns (axis = 0)
v_sum = np.sum(v[:,:], axis=0)
# calculate the sum of top section of columns between vertical 0-11 and horizonal 5:26
top_sum = np.sum(v[2:12,6:26], axis = 1)
#calculate total sum of top area
top_brightness = sum(top_sum)

middle_sum = np.sum(v[11:21,6:26], axis = 1)
#calculate total sum of middle area
middle_brightness = sum(middle_sum)

bottom_sum = np.sum(v[20:30,6:26], axis = 1)
#calculate total sum of bottom area
bottom_brightness = sum(bottom_sum)

# Assigning values to feature vector
feature.append(top_brightness)
feature.append(middle_brightness)
feature.append(bottom_brightness)
```

---

## Results

The classification algorithm is able to classify 94.61% of the images correctly with this approach. When tested on a set of 297 images, the algorithm correctly classified 281 out of 297 images. This methodology is sufficiently robust to not classify any red light as a green light.

---

### Algorithm Weaknesses

1. The algorithm used sections of high brightness as the only measure to distinguish traffic lights. This fails in case of the traffic lights not being sufficiently iluminated or blurred becasue of rain or mist. Also, if the image quality is not good (like in the case of first missclassified image), it fails.

2. The algorithm does not use color identification schemes to count the number of red, yellow or green pixels which will be a more straight forward way of estimating the traffic lights.

3. We can also use a feature line detection feature to better identify arrow lights.

---
