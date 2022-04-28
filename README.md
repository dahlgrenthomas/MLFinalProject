# MLFinalProject
Final project for ML class.
## Design Matrix Description
This data set for this project is located at https://www.kaggle.com/datasets/muratkokludataset/acoustic-extinguisher-fire-dataset?resource=download. This data set is from a study where sound waves were used to extinguish fires started with multiple different fuels. A few rows of the data are as follows:
| Size | Fuel     | Distance  | Decibel | Airflow | Frequency | Status |
| :--: |:--------:| :--------:| :-----: | :-----: | :-------: | :----: |
| 1    | gasoline | 10        | 96      | 0       | 75        | 0      |
| 1    | gasoline | 10        | 96      | 0       | 72        | 1      |
| 1    | gasoline | 10        | 96      | 2.6     | 70        | 1      |

A short description of each variable:
* Size: The size of the fire.
* Fuel: The type of fuel used to start the fire.
* Distance: Distance in centimeters of the fuel from the speaker.
* Decibel: Volume of the audio played through the speaker.
* Airflow: Measurement of air flow created by the speaker.
* Frequency: Frequency of the audio played through the speaker.
* Status: Whether or not the fire was put out. 0 for no, 1 for yes. 

The status variable is the label, and there are 17,400 (?) elements in the dataset.

## Goals and Hypothesis
The goal of this project is to be able to predict whether or not the fire was put out given the 6 variables to a reasonable degree of accuracy. Hypothesis?

## Methods
We use tensorflow in R to create a sequential keras model.   

## Training
We randomly select 14,000 entries in the data to use as training data, and leave the rest to use as testing data. We also set a random seed in order to make this process reproducible.

## Process
We first created a model with two hidden layers using relu as an activation function. The output layer uses sigmoid as the activation function, since the result will always be binary; either the fire was put out or it wasn't.  

We then created a model using a dropout of 0.2 to see if that would help improve the accuracy of our model.

Finally, we created a model using kernel regularization of 0.001 on each layer to see if that would help improve accuracy.

## Results

![Screenshot (27)](https://user-images.githubusercontent.com/77691466/165845746-aeccb078-b3e8-46e1-b721-21827b68687c.png)

![comparison](https://user-images.githubusercontent.com/77691466/165845685-a85485ce-1f39-431d-b702-553bedaa7e8a.png)

## Challenges and Future Work
