# Setting Up Swan Testnet Environment

#### 1.1 Setting Up Your Swan Testnet Environment

Main-net too costly? Just want to do initial testing? No problem! Swan Testnet is the right choice for you!

**Steps:**

* **Obtain an API Key**:
  * Open the [SWAN testnet dashboard](https://orchestrator-test.swanchain.io/provider-status).
  * Connect your wallet using your preferred tool.
  * Make sure you are on the testnet.
    *   After signing in, change the env to Proxima Testent.

        <figure><img src="../../../.gitbook/assets/Proxima.png" alt=""><figcaption></figcaption></figure>
  * Retrieve your API Key:
    *   After signing in, click on the profile icon and select "Show API Key".

        <figure><img src="../../../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>
    *   Click "New API Key" to generate a new key, which will appear in the list.

        <figure><img src="../../../.gitbook/assets/login-api-key-2.png" alt=""><figcaption></figcaption></figure>
    *   Go to [Bridge](https://superbridge.app/swan-chain) to get gas

        <figure><img src="../../../.gitbook/assets/Bridge-test.png" alt=""><figcaption></figcaption></figure>
    * Go to our [discord channel](https://discord.com/invite/swanchain) to get the SwanToken for testnet

**Install Swan SDK**

You can install the Swan SDK using one of the following methods:

**Note: Make sure you are installing the latest version of the SDK, Current version is:** [![PyPI version](https://img.shields.io/pypi/v/swan-sdk)](https://pypi.org/project/swan-sdk/)

*   **Via PyPI (Recommended)**:

    ```bash
    pip install swan-sdk
    ```
* **From GitHub**:
  *   Clone the repository:

      ```bash
      git clone https://github.com/swanchain/python-swan-sdk.git
      ```
  *   Create and activate a virtual environment (optional):

      ```bash
      python3 -m venv venv
      source venv/bin/activate
      ```
  *   Install the requirements:

      ```bash
      pip install -r requirements.txt
      ```
  *   Install the SDK:

      ```bash
      pip install .
      ```

      or if you want to change the source code:

      ```bash
      pip install -e .
      ```

**Note**: Ensure that `web3.py` version is between 6.15 and 7.10 to avoid potential errors. (`pip install web3>=6.15,<7.10`)

**Test Your Environment**

You can test your environment by running the following code snippet (Same as [setting up your Swan environment](./), just change the network to testnet):

```python
import swan
import json
swan_orchestrator = swan.resource(api_key="<your_api_key>", network='testnet', service_name='Orchestrator')
result = swan_orchestrator.create_task(
    app_repo_image='hello_world', # This "app_repo_image" is a special name-repo mapping made by Swan, it's DEMO ONLY
    wallet_address='<WALLET_ADDRESS>',
    private_key='<PRIVATE_KEY>'
)
task_uuid = result['id']
task_info = swan_orchestrator.get_deployment_info(task_uuid=task_uuid)
print(json.dumps(task_info, indent=2))
```

if you see the deployment information like this, your environment is set up correctly:

```json

{
  "data":{
    "computing_providers":[
      ...
    ],
    "jobs":[
      ...
    ],
    "task":{
      ...
    },
    "space":{
      ...
    }
  },
  "message":"fetch task info for task_uuid='dfb9c94a-dcb1-4b92-a54b-046ea7d745cc' successfully",
  "status":"success"
}
```

#### Check Status(Optional)

You can now run this code to check the status of your deployment.

* **Note**: You should copy the task uuid from the previous code response and comment out the previous code, otherwise, you will create a new deployment.

```python
result_url = swan_orchestrator.get_real_url(task_uuid)
print(result_url)
```

you can click the url to see the result, It will be something like this:

<figure><img src="../../../.gitbook/assets/hello_world.png" alt=""><figcaption></figcaption></figure>
