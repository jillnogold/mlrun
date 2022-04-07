# Setting up your development environment <!-- omit in toc -->

With MLRun, you can use your code on a local machine while running your functions either locally or on a remote cluster. This tutorial explains how to set up both options.

**In this section**
- [Prerequisites](#prerequisites)
- [Configure the environment](#configure-remote-environment)
    - [Set environmental variables by .env file](#set-environmental-variables-by-env-file)
    - [Set environment variables](#set-environment-variables)
- [IDE configuration](#ide-configuration)
- [Set the environment from PyCharm](#remote-environment-from-pycharm)
- [Set the environment from VSCode](#remote-environment-from-vscode)
  - [Create environment file](#create-environment-file)
  - [Create Python debug configuration](#create-python-debug-configuration)
  - [Set environment file in debug configuration](#set-environment-file-in-debug-configuration)
- [Set environment variables in a terminal](#set-environment-variables-in-a-terminal)

<a id="prerequisites"></a>
## Prerequisites

Before you begin, ensure that the following prerequisites are met:

1. Install MLRun locally. You need to install MLRun locally and make sure the that the MLRun version you install is the same as the MLRun service version.

   If you already installed a previous version of MLRun, uninstall it by running:

    ```sh
    pip uninstall -y mlrun
    ```

   If you are running the MLRun service remotely, Install a specific version using the following command; replace the `<version>`  placeholder with the MLRun version number (e.g., `1.0.0`):
 
    ```sh
    pip install mlrun==<version>
    ```
   If you are running the MLRun service locally, install the latest version by running:
 ```  
   pip install mlrun[api]
```    
   To specify an earlier version, run: `pip install mlrun[api]==x.y.z` where x.y.z is the version number.

2. If you are going to run the MLRun service locally, start the service by running, on a seperate terminal/console:
   ```
   mlrun db
   MLRUN_DBPATH=http://localhost:8080
   MLRUN_ARTIFACT_PATH=<full path to saved artifact dir>
   ```
   You can use template values in the artifact path. The supported values are:
   - `{{project}}` to include the project name in the path.
   - `{{run.uid}}` to include the specific run uid in the artifact path. 
   
   For example:

    ```ini
    MLRUN_ARTIFACT_PATH=/User/artifacts/{{project}}
    ```
    
   or:

    ```ini
    MLRUN_ARTIFACT_PATH=/User/artifacts/{{project}}/{{run.uid}}
    ```
    
2. If you are running the MLRun service remotely, ensure that you have access to your MLRun service (i.e., to the service URL on the remote Kubernetes cluster).

## Configure the environment

### Set environmental variables by .env file 

You can store the credentials and environmental settings, including project secrets, remote cluster credentials, addresses, etc in a 
standard .env file. Use the SDK or the env option to load the .env file when MLRun imports or starts. 

#### File format

The file has lines in the format `KEY=VALUE`. 

This sample file includes some basic settings for an MLRun service running remotely:
```
MLRUN_DBPATH=https://mlrun-api.default-tenant.app.xxx.iguazio-cd1.com
V3IO_USERNAME=admin
V3IO_ACCESS_KEY=MYKEY123
AWS_ACCESS_KEY_ID-XXXX
AWS_SECRET_ACCESS_KEY=YYYY
```

This sample file includes some basic settings for an MLRun service running locally:
```
MLRUN_ARTIFACT_PATH=<full path to saved artifact dir>
V3IO_USERNAME=admin
V3IO_ACCESS_KEY=MYKEY123
V3IO_USERNAME=admin
V3IO_API=https://webapi.default-tenant.app.xxx.iguazio-cd1.com
```

where:
* MLRUN_DBPATH: API endpoint of the MLRun APIs service endpoint 
* V3IO_USERNAME=username of a platform user with access to the MLRun service
* V3IO_ACCESS_KEY=platform access key
    
#### Usage
* To set the mlrun env from an .env file, use: `mlrun.set_env_from_file(env_path)`
* To load project secrets from an .env file, use: `project.set_secrets(file_path="secrets.env")`
* To load the .env file automatically on import, set the env var: `MLRUN_ENV_FILE` with the .env file path (useful if you're using CLI or 
want to skip `set_env_from_file()`
* To set the env vars from a file and return the results as a dict, use: `env_dict = mlrun.set_env_from_file(env_path, return_dict=True)`
   where:
      * `env_file`: path/url to .env file
      * `return_dict`: set to True to return the .env as a dict

If the remote service is on an instance of the Iguazio MLOps Platform (**"the platform"**), set the following environment variables as well; 
replace the `<...>` placeholders with the information for your specific platform cluster:

    ```ini
    V3IO_USERNAME=<username of a platform user with access to the MLRun service>
    V3IO_API=<API endpoint of the webapi service endpoint; e.g., "https://default-tenant.app.mycluster.iguazio.com:8444">
    V3IO_ACCESS_KEY=<platform access key>
    ```

These are typical settings used for data access to cloud storage:      
      
**AWS S3/services credentials** for use with S3 or Sagemaker:
   
   ```
   AWS_ACCESS_KEY_ID=<key-id>
   AWS_SECRET_ACCESS_KEY=<access-key>
   ```
   
**Azure connection string** points at a storage account. 

For example: DefaultEndpointsProtocol=https;AccountName=myAcct;AccountKey=XXXX;EndpointSuffix=core.windows.net
   ```    
   AZURE_STORAGE_CONNECTION_STRING=<connection-string>
   ```
   
**Google cloud**   
```
GCP_CREDENTIALS='{"type": "service_account", "project_id": "iguazio", "private_key_id": "a603f3e04b83c1097654cfdf41ef5727b2593ea2", 
    private_key": "-----BEGIN PRIVATE KEY-----\\123456789...\\-----END PRIVATE KEY-----\\n", "client_email": "mlrun-v1-
    warroom@iguazio.iam.gserviceaccount.com", "client_id": "115756219098901667372", "auth_uri": "https://accounts.google.com/o/oauth2/auth", 
    "token_uri": "https://oauth2.googleapis.com/token", "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs", 
    "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/mlrun-v1-warroom%40iguazio.iam.gserviceaccount.com"}'
```  
    
#### Access keys
You can get the platform access key from the platform dashboard: select the user-profile picture or icon from the top right corner of any page, and select **Access Keys** from the menu. In the **Access Keys** window, either copy an existing access key or create a new key and copy it. Alternatively, you can get the access key by checking the value of the `V3IO_ACCESS_KEY` environment variable in a web-shell or Jupyter Notebook service.


### Set environment variables

If you are not using an .env file, you can set individual environment variables to define your MLRun configuration using the variables described in [Set environmental variables by .env file](#set-environmental-variables-by-env-file). 


## IDE configuration

## Set the environment from PyCharm

You can use PyCharm with MLRun remote by changing the environment variables configuration.

1. From the main menu, choose **Run | Edit Configurations**.

    ![Edit configurations](../_static/images/pycharm/remote-pycharm-run_edit_configurations.png)

2. To set-up default values for all Python configurations, on the left-hand pane of the run/debug configuration dialog, expand the **Templates** node and select the **Python** node. The corresponding configuration template appears in the right-hand pane. Alternatively, you can edit a specific file configuration by choosing the corresponding file on the left-hand pane. Choose the **Environment Variables** edit box and expand it to edit the environment variables.

    ![Edit configuration screen](../_static/images/pycharm/remote-pycharm-edit_configurations_screen.png)

3. Add the environment variables and values of `MLRUN_DBPATH`, `MLRUN_ARTIFACT_PATH`, `V3IO_USERNAME`, `V3IO_API`, and `V3IO_ACCESS_KEY`.

    ![Environment variables](../_static/images/pycharm/remote-pycharm-environment_variables.png)

## Set the environment from VSCode

### Create environment file

Create an environment file called `mlrun.env` in your workspace folder. Copy-paste the configuration below; replace the `<...>` placeholders to identify your remote target:

``` ini
# Remote URL to mlrun service
MLRUN_DBPATH=<API endpoint of the MLRun APIs service endpoint; e.g., "https://mlrun-api.default-tenant.app.mycluster.iguazio.com">
# Root artifact path on the remote server
MLRUN_ARTIFACT_PATH=<remote path; e.g., "/User/artifacts/{{run.project}}">
# Iguazio platform username
V3IO_USERNAME=<username of a platform user with access to the MLRun service>
# V3IO data access API URL (copy from the services screen)
V3IO_API=<API endpoint of the webapi service endpoint; e.g., "https://default-tenant.app.mycluster.iguazio.com:8444">
# Iguazio V3IO data layer credentials (copy from your user settings)
V3IO_ACCESS_KEY=<platform access key>
```

```{admonition}Note
Make sure that you add `.env` to your `.gitignore` file. The environment file contains sensitive information that you should not store in your source control.
```

### Create Python debug configuration

Create a [debug configuration in VSCode](https://code.visualstudio.com/docs/python/debugging). Configurations are defined in a `launch.json` file that's stored in a `.vscode` folder in your workspace.

To initialize debug configurations, first select the Run view in the sidebar:

<img src="../_static/images/vscode/debug-icon.png" width="200" >

If you don't yet have any configurations defined, you'll see a button to Run and Debug, as well as a link to create a configuration (launch.json) file:

<img src="../_static/images/vscode/debug-start.png" width="400" >

To generate a `launch.json` file with Python configurations, do the following steps:

1. Click the **create a launch.json file** link (circled in the image above) or use the **Run** > **Open configurations** menu command.

2. A configuration menu opens from the Command Palette. Select the type of debug configuration you want for the opened file. For now, in the **Select a debug configuration** menu that appears, select **Python File**.
![Debug configurations menu](../_static/images/vscode/debug-configurations.png)

   > **Note** Starting a debugging session through the Debug Panel, **F5** or **Run > Start Debugging**, when no configuration exists will also bring up the debug configuration menu, but will not create a launch.json file.

3. The Python extension then creates and opens a `launch.json` file that contains a pre-defined configuration based on what you previously selected, in this case **Python File**. You can modify configurations (to add arguments, for example), and also add custom configurations.

   ![Configuration json](../_static/images/vscode/configuration-json.png)

### Set environment file in debug configuration

Add an `envFile` setting to your configuration with the value of `${workspaceFolder}/mlrun.env`

If you created a new configuration in the previous step, your `launch.json` would look as follows:

```javascript
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "envFile": "${workspaceFolder}/mlrun.env"
        }
    ]
}
```

## Set environment variables in a terminal

You can create a script that sets the desired environment variables before launching your IDE

Create a file `mlrun_env.sh`, and copy-paste the code below; replace the `<...>` placeholders to identify your remote target:

``` bash
#!/usr/bin/env bash

# Remote URL to mlrun service
export MLRUN_DBPATH=<API endpoint of the MLRun APIs service endpoint; e.g., "https://mlrun-api.default-tenant.app.mycluster.iguazio.com">
# Root artifact path on the remote server
export MLRUN_ARTIFACT_PATH=<remote path; e.g., "/User/artifacts/{{run.project}}">
# Iguazio platform username
export V3IO_USERNAME=<username of a platform user with access to the MLRun service>
# V3IO data access API URL (copy from the services screen)
export V3IO_API=<API endpoint of the webapi service endpoint; e.g., "https://default-tenant.app.mycluster.iguazio.com:8444">
# Iguazio V3IO data layer credentials (copy from your user settings)
export V3IO_ACCESS_KEY=<platform access key>
```

In your terminal session execute:

```sh
source mlrun_env.sh
```

Then launch your IDE from the same terminal session.
