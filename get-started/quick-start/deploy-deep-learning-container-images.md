### 3. Deploy Deep Learning Container Images

In this tutorial, you will learn how to deploy a Llama3 application with just a few lines of code.

**Note** : [Click here to see the full demo code](https://github.com/swanchain/python-swan-sdk/blob/main/examples/ex2_modelapp.py)

**Prerequisites**:

Before starting, ensure you have completed the following tutorials:
* [Setting Up Your Swan Environment](../quick-start/setting-up-your-swan-environment.md)
* [Build a Basic Web Application](../quick-start/build-a-serverless-web-application.md)

#### 1. Log in to Orchestrator via SDK

(Same as the previous tutorial [Build a Basic Web Application](../quick-start/build-a-serverless-web-application.md))

```python
import swan
import json
swan_orchestrator = swan.resource(api_key="<your_api_key>", network='mainnet', service_name='Orchestrator')
```

#### 2. Create a Task

A task is created through Swan Orchestrator to deploy the application on the distributed computing provider network.

**Notes**:
- **Optional**: Here, `hardware_id` is set to 13, which corresponds to GPU hardware type (NVIDIA 3080). To check the hardware ID for other hardware types, use:
  ```python
  available_hardware = swan_orchestrator.get_hardware_config()
  print(json.dumps(available_hardware, indent=2))
  ```
- The `auto_pay` parameter is set to `True`, meaning the SDK will automatically deduct SwanCreditToken from your wallet address. Ensure you have sufficient ETH for gas and SwanCreditToken in your wallet.
- [Bridge to get the gas](https://superbridge.app/swan-chain)
- [Faucet to get the SwanCreditToken](https://faucet.swanchain.io/)

```python
result = swan_orchestrator.create_task(
    repo_uri='https://github.com/swanchain/awesome-swanchain/tree/main/Llama3-8B-LLM-Chat',
    wallet_address='<WALLET_ADDRESS>',
    private_key='<PRIVATE_KEY>',
    hardware_id=13,
    duration=3600,
    auto_pay=True
)
task_uuid = result['id']
```

#### 3. Review the Deployment Result

After obtaining the task UUID, track the deployment result using the Orchestrator interface `get_deployment_info`:
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
[https://jpz44b3hmq.computing.filezoo.com.cn]
```

Once the job status is `Running`, you can access the application via the URL in your web browser.

Sample screenshot:

<figure><img src="../../.gitbook/assets/llama3.png" alt=""><figcaption></figcaption></figure>