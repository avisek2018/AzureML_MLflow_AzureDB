# Create, Run, and Azure Databricks Experiments in Azure Machine Learning and MLflow
MLflow is an open-source product designed to manage the Machine Learning development lifecycle. It allows data scientists to train models, register those models, deploy the models, and manage model updates. MLflow has four components:

-	MLflow Tracking
-	MLflow Projects
-	MLflow Models
-	MLflow Model Registry

The MLflow Tracking component is an API and UI for logging parameters, code versions, metrics, and output files when running your machine learning code and for later visualizing the results.
We can use Azure Machine Learning as the backend of MLflow experiments while trigerring it from the Azure databricks. Azure Machine Learning workspace that provides a centralized, secure, and scalable location to store training metrics and artifacts.

## Create Databricks Cluster:

![Databricks Cluster](Images/DB_Cluster.JPG?raw=true)

 ## Install required packages: 
 
We need to install the required packages.

![Install Libraries](Images/DB_Cluster_Libraries.JPG?raw=true)

## Import the Data and Create Table:

I used the famous Penguin dataset [Penguin Dataset](https://www.kaggle.com/code/parulpandey/penguin-dataset-the-new-iris/data) as an example here. 

![Penguin Dataset](Images/Penguin.JPG?raw=true)

You can download the dataset and create table using UI inside Azure Databricks.

![Create Table](Images/create_table.JPG?raw=true)


## Configure MLflow tracking to use AML: 

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

```
experiment_name = 'Penguin-ADB-MLflow-AML'
mlflow.set_experiment(experiment_name)
```
## Run Experiment:

```
with mlflow.start_run() as run:
    ...
    ...
```

## Check Model Metrices and Artifacts

![Model Metrics](Images/model_metric.JPG?raw=true)

![onfusion Matrix](Images/conf_mat.JPG?raw=true)

![Classification Report](Images/class_report.JPG?raw=true)

## Register the Model in Azure ML

```
from azureml.core import Model

model = Model.register(workspace=ws, 
                       model_name='penguin-classification',
                       model_path=model_file_path, # local path
                       description='Model to predict Penguin Species.')
```

