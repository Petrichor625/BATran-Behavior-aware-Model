# BATran: Behavior-aware Model

This repository is the code of Achieving Human-Like Trajectory Prediction in Autonomous Vehicles-A Behavior-Aware Approach.

[2023.5.25] ***The NGSIM dataset is uploaded in the Releases in this repository, for access to other datasets (HighD and RounD) please see README .***



## Introduction

This project introduce a behavior-aware model for trajectory prediction that incorporates theories and findings from the fields of human behavior, decision-making, theory of mind, etc. This model is comprised of behavior-aware, interaction-aware, priority-aware, and position-aware modules that analyze and interpret a variety of inputs, perceive and comprehend underlying interactions, and take into account uncertainty and variability in prediction. We evaluate the performance of our model using the NGSIM dataset and show that it outperforms current state-of-the-art baselines in terms of prediction accuracy and efficiency. Even when trained on a reduced portion of the training data, specifically 25%, our model outperforms all baselines, demonstrating its robustness and efficiency in predicting future vehicle trajectories and the potential to lower the amount of data required for training autonomous vehicles, particularly in corner cases.


## Our model

This model comprises of four innovative modules - a behavior-aware module, an interaction-aware module, a priority-aware module, and a position-aware module - each designed to enhance the sophistication and nuance of the model's understanding of driver behavior and vehicle interactions on the road.

**The behavior-aware module**, in particular, is a key component of this model. Instead of resorting to a simplistic classification of behaviors into two or three distinct typologies, it utilizes a continuous representation of behavioral information, rooted in dynamic geometric graph theory, to offer unparalleled flexibility and scalability in dynamic driving contexts. This allows autonomous vehicles to anticipate and respond to the actions of other drivers in a more intricate and elaborate manner. 

**The interaction-aware module**, on the other hand, takes into account the interactions between the AV and other vehicles in the environment, utilizing Long Short-Term Memory Networks (LSTMs) encoder to process historical track information for the ego vehicle and surrounding vehicles, thus enabling the ego vehicle to have a better understanding of the potential interactions with other vehicles. 

**The priority-aware module** evaluates the importance of different vehicles and allows the ego vehicle to prioritize its attention and response to certain vehicles over others, based on their relevance to the ego vehicle's trajectory. 

**The position-aware module** encodes and learns the dynamic location of the ego vehicle, providing additional context for the prediction of the ego vehicle's trajectory. Furthermore, we introduce a Polar coordinate system to accommodate the relative distance among various vehicles and scenes, which provides a flexible way to adapt to heterogeneous input data.



![image](https://github.com/Petrichor625/BATran-Behavior-aware-Model/blob/main/framework.png)


## Install

The model install in Ubuntu 20.04, cuda11.7

**1. Clone this repository**: clone our model use the following code 

```
git clone https://github.com/Petrichor625/BATraj-Behavior-aware-Model.git
cd Behavior_aware file
```

**2. Implementation Environment**: The model is implemented by using Pytorch. We share our anaconda environment in the folder 'environments', then use this command to implement your environment.

```
cd environments
conda env create -f environment.yml
```

If this command cannot implement your conda environment well, try to install again the specific package separately.

```
conda create -n Behaivor_aware python=3.8
conda activate Behavior-aware
conda install pytorch==1.8.0 cudatoolkit=11.1 -c pytorch -c conda-forge
```



## Download and Pre-processing Datasets

Before starting the training process,   you can choose one of the datasets to train the model,  and you need to prepare the dataset and follow the steps below.



**a. NGSIM (Next Generation Simulation) Dataset**

<u>The esteemed NGSIM dataset is available at the Releases in this repository.</u>

###### **Note**

1. [2023.5.25] Like the baselines, the NGSIM dataset in our work is segmented in the same way as in the most widely used work [Deo and Trivedi, 2018], so that comparisons can be made.

2. [2023.5.25] The maneuver-based test set is only a true subset of the overall test set. We select well-defined maneuvers from the overall test set and divide them into the maneuver-based test set, omitting some unknown maneuvers such as zigzag driving, tentative driving, etc. 

   

**b. Highway Drone (HighD) Dataset**

HighD is a new dataset of naturalistic vehicle trajectories recorded on German highways. Using a drone, typical limitations of established traffic data collection methods such as occlusions are overcome by the aerial perspective. Traffic was recorded at six different locations and included more than 110 500 vehicles. It provides both the full dataset and the sample version of the dataset for testing purposes. We also train and test our models in the highD dataset.

Due to the policy requirements of this dataset, please request and download the **HighD** dataset from the  HighD official [website](https://www.highd-dataset.com/). Normally, applications take 7-14 working days to be approved.

After downloading the dataset,  please put all your dataset files in the directory named `HgihD` (in the folder 'dataset'), and then you then run the following MATLAB script to process the raw data:

```
HighD_preprocess.m
```



###### Node

[2023.5.25] The HighD dataset in our work is segmented in the same way as the work ([stdan](https://ieeexplore.ieee.org/document/9767719))



**c.  Roundabout Drone (RounD) Dataset**

The rounD dataset is a new dataset of naturalistic road user trajectories recorded at German roundabouts. Using a drone, typical limitations of established traffic data collection methods like occlusions are overcome. Traffic was recorded at three different locations. The trajectory for each road user and its type is extracted.

To further evaluate the performance of our model in some complex and non-regularized scenes, such as roundabouts, and irregular intersections, we also train our model in the rounD dataset.



Due to the policy requirements of this dataset, please request and download the **RounD** dataset from the  RounD official [website](https://www.round-dataset.com/). Normally, applications also take 7-14 working days to be approved.

After downloading the dataset,  please put all your dataset files in the directory named `RounD` (in the folder 'dataset'), and then you then run the following MATLAB script to process the raw data:

```
rounD_preprocess.m
```



## Train 

In this section, we will explain how to train the model.

Please keep your model in a file named `models` and change your hyperparameter in models.py

###### Note

You can view or change the network parameters from the `model_args.py` file

##### Begin to Train

```
python3 train.py
```

The training logs will keep in the file name `Train_logs`, you can use tensorboard to check your training process.

```
tensorboard --logdir /YourPath/Train_log/yourmodel
```

**Save trained model**

In addition, the trained model will be stored in the path you set, which can be set from the `model_fname` in the `train.py` file 

```
model_fname = 'path/to/save/model'
```



## Evaluation

This step helps you assess how well the model generalizes to unseen data and allows you to make any necessary adjustments or improvements. If the model does not meet your desired performance criteria, you can iterate on the training process by adjusting hyperparameters, modifying the model architecture, or collecting additional data.

Once the model has been trained, it is important to evaluate its performance on a separate validation or test set. This step helps assess how well the model generalizes to unseen data. You can choose to evaluate the model using either the validation set (val) or the test set (the overall tested set or the maneuver-based dataset) by setting the test_dataset_files and test_cases . 

```
test_dataset_files = ['TestSet', 'TestSet_keep', 'TestSet_merge', 'TestSet_left',  'TestSet_right']
test_cases = ['overall', 'keep', 'merge', 'left', 'right']
```

Please set the path of your trained model in net_fname in evlauate.py

```
net_fname = 'path/to/save/model'
```

To evaluate the model on the validation or test set, run the following command:

```
python evaluate.py 
```

Finally, the evaluation results will be saved as a .csv file and stored in the path you have predefined in eval_fname

```
eval_fname ='path/to/save/results'
```



## Qualitative results

We are preparing a script for generating these visualizations:

 ````
 Code for qualitative results coming soon
 ````
 ![image](https://github.com/Petrichor625/BATraj-Behavior-aware-Model/blob/main/Figures/Qualitative%20results.gif)


## Baseline

**[S-LSTM](https://ieeexplore.ieee.org/document/7780479)**: This method integrates a social pooling mechanism to the LSTM model.

**[S-GAN:](https://ieeexplore.ieee.org/document/8578338)** This baseline presents a pooling strategy based on generative adversarial networks (GAN).

**[CS-LSTM](https://ieeexplore.ieee.org/document/8575356):** This approach uses a convolutional layer for the pooling mechanism with a maneuver classifier provided.

**[MATF-GAN](https://ieeexplore.ieee.org/document/8953520):** This model employs convolutional fusion to capture the spatial interaction between agents and scene context, and utilizes GAN to generate predicted trajectories. 

**[NLS-LSTM](https://ieeexplore.ieee.org/document/8813829):** The non-local social pooling scheme captures the social interaction by combining both local and non-local operations to produce context vectors for social pooling.

**[PiP](https://link.springer.com/chapter/10.1007/978-3-030-58589-1_36):** The planning-informed prediction model integrates trajectory prediction with target vehicle planning to predict the trajectory by using candidate target vehicle trajectories as a fundamental component.

**[MHA-LSTM](https://ieeexplore.ieee.org/document/9084255):** The multi-head attention social pooling scheme uses the multi-head dot product attention mechanism to predict the trajectory

**[TS-GAN](https://ieeexplore.ieee.org/document/9151374):** This approach uses a social convolution mechanism and recurrent social module for vehicle trajectory prediction.

**[STDAN](https://ieeexplore.ieee.org/document/9767719):** The model employs a spatial-temporal dynamic attention network to predict trajectory prediction.



## Main contribution

This work aims to introduce a novel **lightweight and map-free model** that eliminates the need for labor-intensive high-definition (HD) maps and costly manual annotation by using only historical trajectory data in the polar coordinate system and a DGG-based method to capture continuous driving behavior. It can be summarized as follows:

1. We use a **continuous representation of behavioral information that enables higher-level learning and flexibility while avoiding the categorization of driving behaviors into fixed, discrete groups**. This approach is more adaptive and eliminates the need for manual labeling in the training process, while also addressing potential issues with continuously changing behavior labels and selecting appropriate time windows.
2. We present an efficient map-free framework that eliminates the need for expensive HD maps and drastically reduces the number of parameters. Since HD maps are typically designed for specific regions and may not be available or accurate for other locations, where dynamic changes in road conditions or traffic patterns can quickly render HD or vectorized maps obsolete. **Our model can adapt to changing conditions without relying on HD maps.**
3. This work proposes an innovative pooling mechanism by **defining the motion of vehicles in polar coordinates and using radial motion velocity to extract the relative positions of vehicles**, avoiding reliance on absolute positions and individual grid cells. Compared to the Cartesian coordinate system, it provides an easier way to represent directions and distances relative to the vehicle, which can help account for the curvature of the road using the angle, radius, and radial velocity of motion. Thus, it is consistent with human perception and cognition and can be used for complex and non-regularized scenes, such as roundabouts, and irregular intersections. This can make it easier to incorporate other factors that may affect the motion of the vehicle, such as the friction of the road surface or the virtual effects of interaction between ego vehicles and surrounding vehicles.
4. We trained our model **on a reduced portion of the training set, specifically 25%,** the evaluation results indicate that our model achieves significantly lower RMSE and NLL values compared to most of the baselines, even when using a much smaller amount of training data. Remarkably, it maintains competitive performance **even with 25% missing data**, demonstrating its robustness in a variety of traffic scenes.


