# Lab 2: Intelligent PDF Summarize

## Durable Function

In this lab we are building an PDF Summarizer application using the Durable Functions and other Azure resources. This program is used to take a pdf file and make a summary in a txt file using the AI resources. 

### Trigger function

The Trigger function (blob_trigger) only run if there is something submitted in the 'input' container. This is where the user can input the pdf file in the container and the durable functions will take it and run processes on it.

### Orchestrator function

The Orchestrator function (process_document) calls other 3 functions to process each of the results of the pdf file

### Activity functions

There are 3 different activity functions where each of them is connected to an Azure resource and process each of the inputs and return a generated output. In the first function (analyze_pdf) is connected to the Cognitive Service resource where is takes the pdf file as the input and generates an analysis report as the output. In the second function (summarize_text) is connected to gpt-4 model in the OpenAI resources where it gives a summary of the analysis report in a txt file. In the final function (write_doc) send its summary file to the output container of the blob storage where users can receive it.

## Deploying the application

### Configure Azurite
In order to deploy the application, you first need to press `F1` and run this command
```txt
Azurite: Start
```
to run the processes.

### create the Function resource in Azure
After configuring Azurite, we will create the function in Azure by pressing `F1` and run this command
```txt
Azure Functions: Create Function App in Azure
```
and then enter the following prompts:
1. (your subscription)
2. PDF_summarize
3. East US
4. Python 3.10
5. Secrets

After that, a blob storage will be created for you including a resource group where we will create all the other functions.

### Deploying the Function resource
Now we need to deploy the application by pressing `F1` and run this command:

```txt
Azure Functions: Deploy to Function App
```

and then enter the following prompts:
1. (your subscription)
2. (the function you created in azure)

after that, all the changes will be uploaded to Azure.

### Run the program
Now press `F5` to start running the program.

## Blob Storage

After the function is deploy, you need to make two containers which are 'input' and 'output'. Go to the storage resource and go to the containers section on the left. After that click on `create +` twice and make two new containers. Ones the function is running, submit a pdf file in the 'input' container and when the functions are finished running, the output container contains the summary of that pdf file. Go to the Access keys in the Security + networking section and get the first connection string and paste it in the BLOB_STORAGE_ENDPOINT environment variable in local.settings.json file. 

## Cognitive Services

To create this type of resource, search up AI Foundation and go to Document intelligence section and press '+ Create Document intelligence'. Enter your subscription and the resource group that the function created. Fill in the rest of the stuff and create the application and you have the Cognitive Service resource. After that, go back to Overview and get the endpoint and paste it in the COGNITIVE_SERVICES_ENDPOINT environment variable in local.settings.json file. 

## Open AI

### Set up the Azure resource

After you made the AKF Cluster, make the OpenAI resource. Go to Azure and search up OpenAI and fill in the details:

- **Subscription**: Select your subscription.
- **Resource group**: Choose the function group you created,
- **Region**: Choose `East US`.
- **OpenAI name**: `OpenAISummaryAnalysis`.
- **Pricing tier**: `Standard SO`

Keep clicking `next` and select `Create`. After it finish deploying, go into the application by selecting `Go to Azure OpenAI Studio` and go to the Model catalog tab in Get started selection. After that, add GPT-4 and name it gpt-4-deployment. When deploying, select Global Standard on GPT-4. 

### Get the environment values

After that, note down the **name** and **Endpoint URL** in a separate note book. Also note down the **Keys** such as **API Key 1** and the **Azure OpenAI Endpoint URL** as well.

In local.settings.json fill in:

- `AZURE_OPENAI_DEPLOYMENT_NAME`: **deployment name for GPT-4**
- `AZURE_OPENAI_KEY`: **Enter the endpoint URL for OpenAI**
- `CHAT_MODEL_DEPLOYMENT_NAME`: gpt-4-deployment

# Deploying again

After you created and connected the rescores, deploy and run the function again.

# Changes

Change the summarize_text function so that it uses the endpoint string and key to connect to gpt model in OpenAI instead of using genetics that uses the deployment model name to connect.
