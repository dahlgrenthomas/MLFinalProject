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

The status variable is the label, and there are 15,390 elements in the dataset.

There are originally 4 different types of fuel: gasoline, thinner, kerosene, and liquid petroleum gas (LPG). Since the measurements of the size of fires was different for LPG, we removed that from the data. 

We normalized all numberic categories on a min-max scale to get them all on a range from 0-1 in their respective categories. We also changed the fuel category to be categorical. After the changes, the data looks like:

| Size | Fuel     | Distance  | Decibel    | Airflow   | Frequency | Status |
| :--: |:--------:| :--------:| :--------: | :-------: | :-------: | :----: |
| 1    | 0        | 10        | 0.05263158 | 0.0000000 | 1.0000000 | 0      |
| 1    | 0        | 10        | 0.05263158 | 0.0000000 | 0.9600000 | 1      |
| 1    | 0        | 10        | 0.05263158 | 0.1529412 | 0.9333333 | 1      |

Looking at a correlation plot of the data, we found that the type of fuel had very little correlation with whether or not the fire was put out. Because of this, we removed the type of fuel from our data. 

![corr](https://user-images.githubusercontent.com/77691466/166080324-f505a4e6-bcfe-44f9-aa3c-c1b1cdcbe121.png)

With size removed, the final data set looks like:

| Size | Distance | Decibel    | Airflow   | Frequency | Status |
| :--: |:--------:| :--------: | :-------: | :-------: | :----: |
| 1    |10        | 0.05263158 | 0.0000000 | 1.0000000 | 0      |
| 1    |10        | 0.05263158 | 0.0000000 | 0.9600000 | 1      |
| 1    |10        | 0.05263158 | 0.1529412 | 0.9333333 | 1      |

## Goals and Hypothesis
The goal of this project is to be able to predict whether or not the fire was put out given the 6 variables to a reasonable degree of accuracy. Our hypothesis is that we should get very good accuracy looking at the correlation plot. Since there is such a low correlation between the type of fuel and the result, removing it from the data should not hurt our accuracy very much.

## Methods
We use tensorflow in R to create a sequential keras model.   


## Training
We randomly select 12,000 entries in the data to use as training data, and leave the rest to use as testing data. We also set a random seed in order to make this process reproducible.

## Process
We first created a model with two hidden layers using relu as an activation function. The output layer uses sigmoid as the activation function, since the result will always be binary; either the fire was put out or it wasn't.  

The loss was calculated using binary crossentropy, and the model uses the Adam algorithm for optimization. 

We then created a model with all the same setup as before, but using a dropout of 0.2 to see if that would help improve the accuracy of our model.

Finally, we created a model still using the same setup as the first model, but this time using kernel regularization of 0.001 on each layer to see if that would help improve accuracy.

## Results
* Model with no dropout and no regularization: 

![Original](https://user-images.githubusercontent.com/77691466/166080385-e919a71b-2732-435d-80d0-63a6ea9ff945.png)

* Model with dropout:

![Dropout](https://user-images.githubusercontent.com/77691466/166080401-d136ac48-8628-4366-9f1f-2fa11c4fe65d.png)

* Model with regularization and no dropout:

![regularization](https://user-images.githubusercontent.com/77691466/166080423-a4262076-1585-42e8-9936-377a2ac24231.png)

All three compared: 

![Screenshot (32)](https://user-images.githubusercontent.com/77691466/166080557-6fe866a5-5e2a-4d90-8d0f-b0f4f1ccbfc1.png)

Using either dropout or regularization did not affect accuracy numbers very much. All three are very close to each other, with our model with no dropout and no regularization achieving the highest degree of accuracy by .16%.

We confirmed that our hypothesis was correct that since the data is closely correlated, our model is able to predict fires being put out with around 91% accuracy. 

## Challenges and Future Work
One challenge we faced was figuring out specifically what changes to make to the data. Once we normalized our data, that improved accuracy. 

