# MLFinalProject
Final project for Machine Learning class.
## Running Instructions

Import project into R Studio as a "New Project." Select "Version Control" from the next menu, and choose to import from git. Paste the url of repository into the url field. 

Once the project is in R Studio, make sure all libraries are installed: keras, ggplot2, tidyr, tibble, and dplyr. 

To run the code, select "Run All" from the "Run" dropdown menu.

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

There was about an equal amount of fires extinguished to fires not extinguished and every fuel type was evenly tested for every size of fuel used, distance away, decibel, and frequency.

We removed every row after 15390 because the fuel type of LPG was a gas and wasnt measured for size in the same way as the liquids.
The status variable is the label, and there are 15,390 elements in the dataset.

There are 3 different types of fuel: gasoline, thinner, and kerosene. 
We converted all of them into an int so gasoline become 0 and so on so the network could work with them.

We normalized all numberic categories on a min-max scale to get them all on a range from 0-1 in their respective categories. After the changes, the data looks like:

| Size | Fuel     | Distance  | Decibel    | Airflow   | Frequency | Status |
| :--: |:--------:| :--------:| :--------: | :-------: | :-------: | :----: |
| 1    | 0        | 10        | 0.05263158 | 0.0000000 | 1.0000000 | 0      |
| 1    | 0        | 10        | 0.05263158 | 0.0000000 | 0.9600000 | 1      |
| 1    | 0        | 10        | 0.05263158 | 0.1529412 | 0.9333333 | 1      |


![corr](https://user-images.githubusercontent.com/77691466/166080324-f505a4e6-bcfe-44f9-aa3c-c1b1cdcbe121.png)

Looking at the correlation matrix, everything is at least somewhat correlated with our label, status.

## Goals and Hypothesis
The goal of this project is to be able to predict whether or not the fire was put out given the 6 variables to a reasonable degree of accuracy. Our hypothesis is that we should get very good accuracy looking at the correlation plot. Since there is such a low correlation between the type of fuel and the result, removing it from the data might not hurt the accuracy that much.

## Methods
We use tensorflow in R to create a sequential keras model.   


## Training
We randomly select 12,000 entries in the data to use as training data, and leave the rest to use as testing data. We also set a random seed in order to make this process reproducible.

## Process
We first created a model with two hidden layers of 64 nodes using relu as an activation function. The output layer uses sigmoid as the activation function, since the result will always be binary; either the fire was put out or it wasn't.  

The loss was calculated using binary crossentropy, and the model uses the adam algorithm for optimization. It was run for 35 epochs.

We then created a model with all the same setup as before, but using two hidden layers of 128 nodes and a dropout of 0.1 on one of the layers to see if that would help improve the accuracy of our model.

Finally, we created a model using the same amount of layers and nodes as the dropout model, but this time using L2 kernel regularization of 0.001 on each layer to see if that would help improve accuracy.


## Results
* Model with no dropout and no regularization: 

![runGraph](https://user-images.githubusercontent.com/77516389/166330047-6b111763-7109-42e0-bf65-4a4354a9145e.png)

* Model with dropout:

![dropoutGraph](https://user-images.githubusercontent.com/77516389/166331552-821b7651-11f7-4273-8cb9-9d08510ebe39.png)

* Model with L2 regularization:

![l2Graph](https://user-images.githubusercontent.com/77516389/166331742-3ede6c06-6916-442a-aad5-6c2935e5b925.png)

All three of the testing loss and testing accuracy results: 

![totalResults](https://user-images.githubusercontent.com/77516389/166331846-c9014fdf-d41e-42cd-85cb-54956a87f2da.png)


From this we can see that the dropout model was the most accurate in predicting if the fires were put out, followed by the regular model, followed by the L2 model

We confirmed that our hypothesis was correct that since the data is closely correlated, our model is able to predict fires being put out with around 94% accuracy. We also tried to remove different columns of data to see if the accuracy would be afftected. Everything except fuel being removed caused the model to go below 80% accuracy while fuel would drop the accuracy about 2%. From this we decided all columns of the data would be necessary.

## More Tests
After the first tests we experimented with different numbers of nodes, more hidden layers, and more epochs for the layers to see if we could make the models even better.
These were the results:
| Model Type | Hidden Layers     | Nodes for each layer  | Epochs | Testing loss | Testing accuracy |
| :--: |:--------:| :--------:| :-----: | :-----: | :-------: |
| Regular    |  4        | 64      | 35 | 0.0916 | 0.9575 |
| Dropout(0.2)    |  4        | 64      | 35 | 0.0925 | 0.9631 |
| Regular    |  4        | 128      |  35 | 0.1241 | 0.9631 |
| Regular    |  4        | 128      |  50 | 0.0711 | 0.9714 |
| Dropout(0.1)    |  4        | 128      |   50 | 0.0775 | 0.9673 |
| L2    |  6        | 128      | 50 | 0.1149 | 0.9649 |

From these tests we have concluded that having 4 hidden layers with 128 nodes in each hidden layer was the most accurate. It out performed dropout where dropout had out performed it in the first tests we ran. There were tests with less nodes in the hidden layers but they were always of lower accuracy unless a lot of epochs were run on them.


## Challenges and Future Work
One challenge we faced was figuring out specifically what changes to make to the data. Once we normalized our data, that improved accuracy. We also needed to be able to increase the accuracy on an already pretty accurate model. Since the data is pretty heavily correlated we think it would be possible to get an even higher accuracy probably around 99%.

