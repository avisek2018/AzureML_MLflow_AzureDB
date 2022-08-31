
![Header Image](Images/Article_header.JPG?raw=true)


# Create, Run, and Track Azure Databricks Experiments in Azure Machine Learning using MLflow
MLflow is an open-source product designed to manage the Machine Learning development lifecycle. It allows data scientists to train models, register those models, deploy the models, and manage model updates. MLflow has four components:

-	MLflow Tracking
-	MLflow Projects
-	MLflow Models
-	MLflow Model Registry

The MLflow Tracking component is an API and UI for logging parameters, code versions, metrics, and output files when running your machine learning code and for later visualizing the results.
We can use Azure Machine Learning as the backend of MLflow experiments while triggering it from the Azure databricks. Azure Machine Learning workspace that provides a centralized, secure, and scalable location to store training metrics and artifacts.

![Image Copyright Microsoft](Images/mlflow-diagram.png?raw=true)

I assume you have the basic knowledge of creating Azure resources, storage account, Azure Databricks, and Azure ML workspace. In order to configure MLflow Tracking and connect Azure Machine Learning as the backend for MLFlow experiments, we need to follow these steps ....

## Create Databricks Cluster:

We need to create our own databricks compute cluster to run the experiments.

![Databricks Cluster](Images/DB_Cluster.JPG?raw=true)

 ## Install Required Packages: 
 
After we create the databricks cluster we need to install the required packages.

![Install Libraries](Images/DB_Cluster_Libraries.JPG?raw=true)

## Import the Data and Create Table:

I used the famous Penguin dataset [Penguin Dataset](https://www.kaggle.com/code/parulpandey/penguin-dataset-the-new-iris/data) as an example here. 

![Penguin Dataset](Images/Penguin.JPG?raw=true)

You can download the dataset and create table using UI inside Azure Databricks.

![Create Table](Images/create_table.JPG?raw=true)


## Configure MLflow tracking to use AML: 

We need to get your AML workspace object. From our AML workspace object, get the unique tracking URI address and then setup MLflow tracking URI to point to our AML workspace.

```
import mlflow
from azureml.core import Workspace

# Get your AML workspace
ws = Workspace.from_config()

# Get the unique tracking URI address to the AML workspace
tracking_uri = ws.get_mlflow_tracking_uri()

# Set up MLflow tracking URI to point to AML workspace
mlflow.set_tracking_uri(tracking_uri)
```

## Define and Configure a MLflow Experiment:

We need to define and set the experiment inside Azure Machine Learning.

```
experiment_name = 'Penguin-ADB-MLflow-AML'
mlflow.set_experiment(experiment_name)
```

## Run Experiment:

Once we define and create the experiment we can start our training run with ` start_run()`

```
with mlflow.start_run() as run:
    ...
    ...
```

## Check Model Metrics and Artifacts

Once the experiment runs to success we can navigate to Azure Machine Learning workspace and check the metrics and artifacts inside the specific run.

![AML Experiment](Images/aml_experiment.JPG?raw=true)

![Model Metrics](Images/model_metric.JPG?raw=true)

![onfusion Matrix](Images/conf_mat.JPG?raw=true)

![Classification Report](Images/class_report.JPG?raw=true)

## Register the Model in Azure ML

We can register the model from Azure Databricks to the Azure Machine Learning workspace.

```
from azureml.core import Model

model = Model.register(workspace=ws, 
                       model_name='penguin-classification',
                       model_path=model_file_path, # local path
                       description='Model to predict Penguin Species.')
```

After we execute the above code we can go to Azure ML workspace and verify the model registry.

![Model Registry](Images/model_registry.JPG?raw=true)


Thanks a ton to Microsoft Learning for some of the images and code segments. 
