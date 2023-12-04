# Blob-Triggered-Azure-Function V2
### Copy a blob with Client libraries
This function gets triggered when a new blob is created in the source container and makes a copy of the new blob in the destination container with the help of **Client Libraries**. 

I've borrowed the code from @yamchi, his blog about this project: https://medium.com/@yamchi/python-blob-triggered-azure-function-to-copy-a-newly-created-blob-to-a-backup-container-5f5a82a24ad4

**Note** that this code is limited to copying between containers within the same storage account.

# Local Testing:
* Ensure you have installed the Azure Functions CLI, described here: `https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=linux%2Cisolated-process%2Cnode-v4%2Cpython-v2%2Chttp-trigger%2Ccontainer-apps&pivots=programming-language-python`
* Ensure you have a local azure emulator like Azurite. The easiest way to do this is with the VSCode extension - that's explained well here `https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azurite?tabs=visual-studio`
* Assuming you have set it up, make sure you start it - if you don't, you'll get a lot of connection refused errors. You also need the queue emulator for this code to work.
* This code *won't* work locally with `"blob_conn_string": "UseDevelopmentStorage=true",` - it needs the full "fake" string, defined here: `https://learn.microsoft.com/en-us/azure/storage/common/storage-use-emulator?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#authenticating-requests-against-the-storage-emulator`
* To run locally, create local.settings.json in the root. You can use the template. `cp example.local.settings.json local.settings.json`
* Then, install the requirements and start the function:
```
$ pip install -r requirements.txt 
$ func start
```

# Production

To get the connection string for an Azure Storage Account go to the storage account and from the left pane select “Access Keys” and then click on “Show” and copy the value.
![Untitled1](https://user-images.githubusercontent.com/84933778/187779260-a68254d2-00e7-4cac-9b54-14b75e5068dc.png)
### Troubleshoot 1:
For any certificate problem, refer to the following links:
https://stackoverflow.com/questions/27835619/urllib-and-ssl-certificate-verify-failed-error/42334357#42334357:~:text=On%20Windows%2C%20Python%20does%20not%20look%20at%20the%20system%20certificate%2C%20it%20uses%20its%20own%20located%20at%20%3F%5Clib%5Csite%2Dpackages%5Ccertifi%5Ccacert.pem.

https://pypi.org/project/pip-system-certs/

### Troubleshoot 2:
If you had an authorization problem, you most probably need SAS Token for the copy process. 

# Testing The Function in Azure:
To test the function in Azure, you need to store the connection values in the Function App's `Application Settings`:
![Untitled3](https://user-images.githubusercontent.com/84933778/187781361-e9f60fc0-0c82-4eb9-9df1-43253145da96.png)

For more info refer to the following links:

https://azuresdkdocs.blob.core.windows.net/$web/python/azure-storage-blob/12.0.0/azure.storage.blob.html#


