### 1. Setting Up Your Swan Environment

In this tutorial, you will set up your SWAN account and development environment, allowing you to interact with your SWAN account and the Orchestrator backend.

#### Steps:

* **Obtain an API Key**:
    * Open the [SWAN dashboard](https://orchestrator.swanchain.io/provider-status).
    * Connect your wallet using your preferred tool.
    * Retrieve your API Key:
        - After signing in, click on the profile icon and select "Show API Key".

            <figure><img src="../../.gitbook/assets/image (185).png" alt=""><figcaption></figcaption></figure>

        - Click "New API Key" to generate a new key, which will appear in the list.

            <figure><img src="../../.gitbook/assets/login-api-key-2.png" alt=""><figcaption></figcaption></figure>
        
        - Go to [Bridge](https://superbridge.app/swan-chain) to get gas and [Faucet](https://faucet.swanchain.io/) to get SwanCreditToken. 
            <figure><img src="../../.gitbook/assets/Bridge.png" alt=""><figcaption></figcaption></figure>
            <figure><img src="../../.gitbook/assets/faucet.png" alt=""><figcaption></figcaption></figure>

#### Install Swan SDK

You can install the Swan SDK using one of the following methods:

- **Via PyPI**:
  ```bash
  pip install swan-sdk
  ```
- **From GitHub**:
  ```bash
  git clone https://github.com/swanchain/python-swan-sdk.git
  ```

**Note**: Ensure that `web3.py` version 6.15 or later is installed to avoid potential errors. (```bash pip install web3>=6.15```)

#### Test Your Environment
You can test your environment by running the following code snippet:

```python
import swan
swan_orchestrator = swan.resource(api_key="<your_api_key>", network='mainnet', service_name='Orchestrator')
result = swan_orchestrator.create_task(
    app_repo_image='hello_world',
    wallet_address='<WALLET_ADDRESS>',
    private_key='<PRIVATE_KEY>'
)
task_uuid = result['id']
task_info = swan_orchestrator.get_deployment_info(task_uuid=task_uuid)
print(task_info)
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
               },
     ...
      }
   },
   "message":"fetch task info for task_uuid='dfb9c94a-dcb1-4b92-a54b-046ea7d745cc' successfully",
   "status":"success"
}
```
**Optional**: You can now run this code to check the status of your deployment.
```Python
result_url = swan_orchestrator.get_real_url(task_uuid)
print(result_url)
```
you can click the url to see the result.