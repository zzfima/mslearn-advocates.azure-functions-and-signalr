## In Azure site:
* Run the following command in Cloud Shell to define a name for your Azure Storage account.  
    
    export STORAGE_ACCOUNT_NAME=mslsigrstorage$(openssl rand -hex 5)
    
    echo "Storage Account Name: $STORAGE_ACCOUNT_NAME"

* Run the following az storage account create command to create a storage account for your function and static website.
    az storage account create \
    --name $STORAGE_ACCOUNT_NAME \
    --resource-group xxx \
    --kind StorageV2 \
    --sku Standard_LRS

* Run the following az cosmosdb create command in Cloud Shell to create a new Azure Cosmos DB account in your sandbox resource group.
    az cosmosdb create  \
    --name msl-sigr-cosmos-$(openssl rand -hex 5) \
    --resource-group xxx

* Run the following commands in Cloud Shell to get the connection strings for the resources we created in this exercise.
    STORAGE_CONNECTION_STRING=$(az storage account show-connection-string \
    --name $(az storage account list \
    --resource-group xxx \
    --query [0].name -o tsv) \
    --resource-group xxx \
    --query "connectionString" -o tsv)

    COSMOSDB_ACCOUNT_NAME=$(az cosmosdb list \
        --resource-group xxx \
        --query [0].name -o tsv)

    COSMOSDB_CONNECTION_STRING=$(az cosmosdb list-connection-strings  \
    --name $COSMOSDB_ACCOUNT_NAME \
    --resource-group xxx \
    --query "connectionStrings[?description=='Primary SQL Connection String'].connectionString" -o tsv)

    COSMOSDB_MASTER_KEY=$(az cosmosdb list-keys \
    --name $COSMOSDB_ACCOUNT_NAME \
    --resource-group xxx \
    --query primaryMasterKey -o tsv)

    printf "\n\nReplace <STORAGE_CONNECTION_STRING> with:\n$STORAGE_CONNECTION_STRING\n\nReplace <COSMOSDB_CONNECTION_STRING> with:\n$COSMOSDB_CONNECTION_STRING\n\nReplace <COSMOSDB_MASTER_KEY> with:\n$COSMOSDB_MASTER_KEY\n\n"

## In VSCode
* In local.settings.json, update the variables AzureWebJobsStorage, AzureCosmosDBConnectionString, and AzureCosmosDBMasterKey with the values listed in the Cloud Shell and save the file. The local.settings.json file should only exist on your local computer.

* npm install

* F5

* To run the web application on your machine, open a second integrated terminal instance and run the following command to start the web app.
    npm start