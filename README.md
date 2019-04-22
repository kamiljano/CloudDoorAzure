# About

CloudDoor is a minimalistic serverless CnC application based on Azure Functions and Azure IoT Hub.
The application allows to register new IoT devices, perform basic queries on them and issue remote operations.

# TODO

* Figure out how to make the HTTP paths look legit... not like what I have now - "route": "products/{category:alpha}/{id:int?}"
* Store the device creation date (might require parsing the IoT Hub event)
* Create a resource template and make sure that the whole thing can be deployed with just one command
* Figure out the authentication for everything except for the registration function
* Create an endpoint for issuing the new commands
* Add automatic client version updates
* Make sure that the admin api of the IoT Hub can only be accessed from the cloud. Might need to put it into some vNet or something

# Endpoints

## Register

    GET /api/register

    Example output:

    {
        "connectionString": "HostName=**********;SharedAccessKey=**************",
        "deviceId": "****************"
    }

## Search for devices

    GET /api/findBots

    Example output:

    [
        {
            "id": "c12535a0-a673-4510-8bbc-c1b3bb80281f",
            "os": {
                "version": "Microsoft Windows NT 6.2.9200.0",
                "type": "Win32NT"
            },
            "currentUser": "DF\\janoka"
        }
    ]


# Install Dependencies

All dependencies can be installed with the simple command

```
npm install -g azure-functions-core-tools
npm install
```

# Deploy

## The hard, but detailed way

1. Create a resource group - `az group create --name CloudDoorDevResourceGroup --location northeurope`
2. Deploy the resources - `az group deployment create --resource-group CloudDoorDevResourceGroup --template-file "./azuredeploy.json"`.
Optional parameters for the command:

    IoT related:

    * `--skuName` - IoT Hub's SKU name. By default it's 'F1' (free IoT Hub)
    * `--capacityUnits` - IoT Hub's capacity units. By default 1