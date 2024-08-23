### 4. Build a simple web application

In this tutorial, you will learn how to deploy a simple web application on the distributed computing provider network using Swan SDK. At the end of this tutorial, you will have a Chain Node Frontend application running on the Swan network.

**Prerequisites**:

* A Swan API Key: If you do not have one, follow the [Setting Up Your Swan Environment](../quick-start/setting-up-your-swan-environment.md) tutorial.
* Python (version 3.8 or later) and `pip install web3>=6.15` (version 6.15 or later) installed in your environment.
* **Optional** A repo containing the application code. For this tutorial, we will use the ChainNode application from the [SwanChain Official GitHub repository](https://github.com/swanchain/awesome-swanchain/tree/main/ChainNode)

**Note** : [Click here to see the full demo code](https://github.com/swanchain/python-swan-sdk/blob/main/examples/ex1_webapp.py)
#### 1. Log in to Orchestrator via SDK

To use `swan-sdk`, log in to Orchestrator with your API Key. (Wallet login is not supported.)

```python
import swan
import json
swan_orchestrator = swan.resource(api_key="<your_api_key>", network='mainnet', service_name='Orchestrator')
```

#### 2. Create a Task

A task is created through Swan Orchestrator to send deployment orders to the distributed computing provider network.

Here, we deploy a ChainNode application with a free configuration and a one-hour duration (these settings are the default).

**Notes**: Even it's a free configuration, you still need to have sufficient ETH in your wallet to pay for the gas fee. Click [swan bridge](https://superbridge.app/swan-chain) to bridge the ETH.

```python
result = swan_orchestrator.create_task(
    repo_uri='https://github.com/swanchain/awesome-swanchain/tree/main/ChainNode',
    wallet_address='<WALLET_ADDRESS>',
    private_key='<PRIVATE_KEY>',
    auto_pay=True
)
task_uuid = result['id']
```
#### 3. Review the Deployment Result

After obtaining the task UUID from the previous step, you can track the deployment result using the Orchestrator interface `get_deployment_info`:
- **Note**: (You should copy the task uuid from the previous code response and comment out the previous code, otherwise, you will create a new deployment.)

```python
task_info = swan_orchestrator.get_deployment_info(task_uuid=task_uuid)
print(json.dumps(task_info, indent=2))
```

<details>
<summary>Sample output (click to expand)</summary>

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

</details>

#### 4. Access the Deployed Application

Retrieve the deployed application's URL:

```python
result_url = swan_orchestrator.get_real_url(task_uuid)
print(result_url)
```

Sample output:

```
['https://1cesbzlmow.cp.altafser.com', 'https://7h8g16arc1.1.3242.cn', 'https://v20bdns36f.1.3242.cn']
```

Once the job status is `Running`, you can access the application via the URL in your web browser.

Sample screenshot:

<figure><img src="../../.gitbook/assets/Chainnode.png" alt=""><figcaption></figcaption></figure>