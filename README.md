# ml_bank_marketing

ML Model for Bank Marketing Dataset

Data used for this project is taken from UC Irvine Machine Learning Repository https://archive.ics.uci.edu/ml/datasets/Bank+Marketing

Code with the data exploration and is presented in the jupyter notebook "notebook.ipynb"

For this project I have used Cross Industry Standard Process for Data Mining Framework (CRISP-DM) and went throught the following stages:

1. Business Understanding
    
To understand the Business objective which needs to be solved by the model which is to classify or predict the client subsribstion to the bank services based on client's characteristics

2. Data Understanding

During this stage I have explored and visualized the data to evaluate what kind of data I am working with, see any patterns and relationships.
From the graphs presented in the notebook it can be seen how massively duration of the compaigns affects the end result of customer subsribtion.

3. Data Preparation

Here I have addressed the missing values, unbalance in the dataset and normalizing the data.
I have used scikit learn preprocessing to replace missing values with the most common values in the dataset. Dropping samples with missing data is not an option since I will lose over 26% of data which is already limited.
In addition to that I have normalized numerical values to bring them into range of 0 to 1 and encode categorical data to improve the learning of the models. For this I have developed a simple funciton which would be used for the data passed for inference and finalizing prediction.
Since I am dealing with imbalanced data I have decided to use oversampling technique called SMOTE to address that and generate new instances of the misrepresented class.
NOTE: Oversampling data with SMOTE gives me the same results for AUC metrics as on data without oversampling.

4. Modeling

During this step I have build couple of models using Scikit Learn and Tensorflow Keras libraries.
Linear SVC  - no specific hyperparameters were configured and was used as default 
XGBoost - no specific hyperparameters were configured and was used as default
Neural Network - build simple Neural Network with a Single Dense Layer and 10 nodes, I have used relu activation function on the nodes for this layer, in addition to that there is a single output layer (also Dense) with a single node and sigmoid activation function used because of the binary classification problem

5. Evaluation

Since the dataset is highly imbalanced the Accuracy metrics will not adequately represent the model performance 
Since we are caring about false negative we can optimize on recall or Area Under the Curve in ROC.
   
6. Deployment

Deployment can be done in cloud, on prem or in hybrid environment.
I would use docker with configured environment and with training, data preprocessing scripts to build the model and saved it in artifactory, or an S3 bucket.
Then build another container image for predictions which downloads the build image during build and runs flask with configured POST request to accept the data required for predictions, it will use the data preparation script to make sure the data is available for the inference and will use the loaded model to return the predicted value. 
This container image would be user to create one or many containers, using Ansible on prem servers and/or on EC2. The load balancer would need to be configured as well to load the traffic between the servers and containers on them. When deploying a new model, we can deploy the newly build inference image alongside the old one and manage the traffic by adjusting load balancer configuration.

Alternatively, I can use Amazon SageMaker not only to build the model but also to serve it. It uses the similar approach but would require less manual steps and configurations as well as it will not required building orchestration scripts.
In addition to that with Amazon SageMaker we can address the future model updates easily by deploying new model alongside the previous one and managing the amount of traffic that goes to each of those models. Using SageMaker would allow us to use build in metrics to monitor the performance of the model.

