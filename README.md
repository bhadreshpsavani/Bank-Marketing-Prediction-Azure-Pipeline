# Bank Marketing Prediction Azure Pipeline 

We are having BankMarketing Data about customers and effectiveness of marketing on customer. We will create and run an AutoML Experiment using Azure Pipeline. 

In this project, We will continue to work with the Bank Marketing dataset. We will use Azure to configure a cloud-based machine learning production model, deploy it, and consume it. We will also create, publish, and consume a pipeline. 

## Architectural Diagram

In this project, We will following the below steps:

1. Automated ML Experiment
2. Deploy the best model
3. Enable logging
4. Swagger Documentation
5. Consume model endpoints
6. Create and publish a pipeline

![architecture](/images/Architecture.png)

## Key Steps:

1. Automated ML Experiment : Here we selected and upload [BankMarketing Dataset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) and created an automl experiment with classification type. This is classification problem and we want to completed it within 30 mins so we specific parameters in the Notebook in the configuration class object of automl with specific compute object.
    Below Image shows that data has been registered
    ![registered_dataset](images/DatasetRegistered.PNG)
    After Registering Data, We completed an automl Run
    ML studio showing the scheduled run
    ![run_completed1](images/run_completed.PNG)
    ![run_completed2](images/run_completed2.PNG))

2. Deploy the best model: After the experiment run completes, a summary of all the models and their metrics are shown, including explanations. 
The Best Model will be shown in the Details tab. In the Models tab, it will come up first (at the top).
    ![best_model](images/best_model.PNG)
    ![best_models](images/best_models.PNG)
    Deploying the Best Model will allow to interact with the HTTP API service and interact with the model by sending data over POST requests.
    ![BestModelDeployment](images/BestModelDeployment.PNG)

3. Enable logging:Now that the Best Model is deployed, enable Application Insights and retrieve logs. Although this is configurable at deploy time with a check-box, it is useful to be able to run code that will enable it for us. When we run the log.py file by specifing rest endpoint URL in the Experiment 
![enable_app_insight](images/enable_app_insight.PNG)
![app_insight_enabled](images/app_insight_enabled.PNG)

4. Swagger Documentation: A swagger UI is great way to make inference and accessing Rest API. We can demo it to someone without making a production ready UI.
For creating Swagger UI for Rest End of Azure deployed model. We need to download one file named `swagger.json`. This file can be downloaded from detail page of deployed model endpoint in Azure Python SDK. 

    Note: We need to place swagger.json in swagger folder of this repository.

    We need to create a docker container which can pull and run swagger image locally. The code is present in swagger.sh file in swagger folder. When we run it we will be able to see simple swagger Home screen like below.
    ![swagger](images/swagger_home.PNG)

    We need to run serve.py present in the swagger folder which will enable us to view experiment specific UI. Below are the the screen that we will get after that.
    ![swagger1](images/swagger1.PNG)
    ![swagger2](images/swagger2.PNG)
    ![swagger3](images/swagger3.PNG)

5. Consume model endpoints: once the model is deployed, use the endpoint.py script provided to interact with the trained model. In this step, you need to run the script, modifying both the `scoring_uri` and the `key` to match the key for your service and the URI that was generated after deployment
    ![endpoint](images/endpoint_results.PNG)
    When we run the above code it will generate `data.json` file

    It is always best practice to check benchmarking on the deployed model once end point is tested. We can do that by running benchmark.py file present in the same directory. It will gave output like below. Its about Time per Request(Inference Time), Request per Time(Throughput) etc. 
    ![benchmark](images/BechmarkResults.PNG)

6. Create and publish a pipeline: We have a jupyter Notebook. When we run it first few cell is to create an MLStep config object that can be used in Pipeline. We created Pipeline Object. We submitted that Pipeline. We also published the pipeline.

    The Advantage of Publishing pipeline is we can run job remotely with simple rest api call.

    The code for running pipeline is also present in jupyter lab
    ![pipeline_created1](images/pipeline_created1.PNG)
    ![pipeline_created2](images/pipeline_created2.PNG)
    ![pipeline_created3](images/pipeline_created3.PNG)
    ![pipeline_created4](images/pipeline_created4.PNG)
    
    The `Published Pipeline overview`, showing a REST endpoint and a status of `ACTIVE`
    ![pipeline_active](images/pipeline_status_active.PNG)
    
    ML studio showing the pipeline endpoint as Active
    ![pipeline_active](images/pipeline_published_run_active.PNG)
    

## Screen Recording
https://youtu.be/lEFy_f39_wQ

## Standout Suggestions:
* Train it for longer Time
* Check if data is balaced if not perform Upsampling/Downsampling or use Different Matrics while training
* Do data analysis and discard unappropriate features
