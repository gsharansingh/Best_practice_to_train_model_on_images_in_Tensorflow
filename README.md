# Best practice to train model on images in Tensorflow
> Common questions:
> 
> Which is better ImageDataGenerator or tf.Data pipeline?
> 
> Is graph-type model any good in trying implement a deep learning model for image classificaton?
> 

These questions are reason for me to try these 3 possible methods for the training of an image classification model.

In this program, I have tried to compare 3 different methods to execute the same model. The model is executed:
1. with ImageDataGenerator
2. with tf.Data
3. with Autograph Model, instead of Eager Model

## Data and Model
The data is `Fruits 360` from `https://www.kaggle.com/moltean/fruits`
Amomg the 131 categories, I have used only 14. These classes are: 
```
['Avocado', 'Banana', 'Blueberry', 'Cauliflower', 'Corn', 'Guava', 'Kiwi', 'Mango', 'Orange', 'Pear', 'Pineapple', 'Pomegranate', 'Strawberry', 'Watermelon']
```
I used 80% of the images for training, and 20% for validation in all three cases.

`InceptionV3` model and its pretrained weights are used to create custom model with functional API.
All methods are taking full advantage of CPU and GPU's parallel execution.

## Physical Hardware
All 3 models are run on `Ryzen5 4800HS` and `Nvidia RTX2060 Max-Q`

## Expectation vs Reality

**Expectation**: 
After learning that we can squeeze extra bit of performance out of the code by implementing Autograph model. 
And with tf.Data pipeline, we can prefetch data using CPU and train model using GPU simultaneously, which should give performance boost over the Simple methods of fetching and training model.

| Method | Assumption | Reason |
|---|---|---|
| ImageDataGenerator | Slowest | Simple to design |
| tf.Data | Normal | Uses CPU and GPU simultaneously |
| Autograph Model | Fastest | Uses CPU and GPU simultaneously + TensorFlow was designed to program around graph mode |

**Reality**:
By the end of the program, I found that the tf.Data gives the best proformance, which was unexpected. 
The model with Graph Model should have proformed best, but it was the worst. It almost took 70% more than the rest two.
The model with ImageDataGenerator, the simplest to write, proformed relatively well. At only 2.5% slow than tf.Data provided that it comes with the ease of use, is the best value for time model.

## Results
| Method | Time taken(seconds) |
|---|---|
| ImageDataGenerator | 328 |
| tf.Data | 320 |
| Autograph Model | 511 |

## Conculsion
In conculsion, I would recommend to use ImageDataGenerator to train models as it was only 2.5% slower than tf.Data method at the same it was easy to design.
On top of that, while using ImageDataGenerator, it is simple to implement Data Augmentation.

