# Lab 2: Intelligent PDF Summarize

## Durable Function

In this lab we are building an PDF Summarizer application using the Durable Functions and other Azure resorces.

## Deploying the application

In order to deploy the application, you first need to press `F1` and run this command
```txt
Azurite: Start
```
to run the proccesses.

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

Now we need to deploy the application by pressing `F1` and run this command:

```txt
Azure Functions: Deploy to Function App
```

and then enter the following prompts:
1. (your subscription)
2. (the function you created in azure)

after that all the changes will be uploaded to Azure.

Now press `F5` to start running the program.

## Blob Storage

After the function is deploy, you need to make two containers which are 'input' and 'output'. Go to the storage resorce and go to the containers section on the left. After that click on `create +` twice and make two new containers. Ones the function is running, submit a pdf file in the 'input' container and when the functions are finished running, the output container contains the summary of that pdf file. Get the endpoint string and paste it in the BLOB_STORAGE_ENDPOINT enviroment variable. 

## Cognitive Services

In order to create this type of resorces, search up AI Foundation and go to Document intelligence section and press '+ Create Document intelligence'. Enter your subscription and the recorce group that the function created and fill in the rest of the stuff. Create the application and you have the Cognitive Service resource. After that, get the endpoint and paste it in the COGNITIVE_SERVICES_ENDPOINT enviroment variable. 

## Open AI

### Set up the Azure resource

After you made the AKF Cluster, make the OpenAI resource. Go to Azure and search up OpenAI and fill in the details:

- **Subscription**: Select your subscription.
- **Resource group**: Choose `BestBuyStore`.
- **Region**: Choose `East US`.
- **OpenAI name**: `OpenAI`.
- **Pricing tier**: `Standard SO`

Keep clicking `next` and select `Create`. After it finish deploying, go into the application by selecting `Go to Azure OpenAI Studio` and go to the Model catalog tab in Get started selection. After that, add GPT-4 and name it gpt-4-deployment. When deploying, select Global Standard on GPT-4. 

After that, note down the **name** and **Endpoint URL** in a separate note book. Also note down the **Keys** such as **API Key 1** and the **Azure OpenAI Endpoint URL** as well.

In local.settings.json fill in:

- `AZURE_OPENAI_DEPLOYMENT_NAME`: **deployment name for GPT-4**
- `AZURE_OPENAI_KEY`: **Enter the endpoint URL for OpenAI**
- `CHAT_MODEL_DEPLOYMENT_NAME`: gpt-4-deployment