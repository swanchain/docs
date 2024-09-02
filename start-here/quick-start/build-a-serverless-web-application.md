# Build a Basic Web Application

In this tutorial, you will learn how to deploy a Serverless Web Application which can called by APIs using Swan SDK.

**Prerequisites**:

* A Swan API Key: If you do not have one, follow the [Setting Up Your Swan Environment](setting-up-your-swan-environment/) tutorial.
* Python (version 3.8 or later) and `pip install web3>=6.15` (version 6.15 or later) installed in your environment.
* **Optional** A repo containing the application code. For this tutorial, we will use a script build by Swan [SwanChain Official GitHub repository](https://github.com/swanchain/awesome-swanchain/tree/main/serverless-api)

**Note** : [Click here to see the full demo code](https://github.com/swanchain/python-swan-sdk/blob/main/examples/ex3\_webapp.py)

## 1. Log in to Orchestrator via SDK

To use `swan-sdk`, log in to Orchestrator with your API Key. (Wallet login is not supported.)

```python
import swan
import json
swan_orchestrator = swan.resource(api_key="<your_api_key>", network='mainnet', service_name='Orchestrator')
```

## 2. Create a Task

A task is created through Swan Orchestrator to send deployment orders to the distributed computing provider network.

Here, we deploy a serverless application with a free configuration and a one-hour duration (these settings are the default).

**Notes**: Even it's a free configuration, you still need to have sufficient ETH in your wallet to pay for the gas fee. Click [swan bridge](https://superbridge.app/swan-chain) to bridge the ETH.

```python
result = swan_orchestrator.create_task(
    repo_uri='https://github.com/swanchain/awesome-swanchain/tree/main/serverless-api',
    wallet_address='<WALLET_ADDRESS>',
    private_key='<PRIVATE_KEY>',
    auto_pay=True
)
task_uuid = result['id']
```

## 3. Review the Deployment Result

After obtaining the task UUID from the previous step, you can track the deployment result using the Orchestrator interface `get_deployment_info`:

* **Note**: (You should copy the task uuid from the previous code response and comment out the previous code, otherwise, you will create a new deployment.):

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

## 4. Access the Deployed Application

Retrieve the deployed application's URL

```python
result_url = swan_orchestrator.get_real_url(task_uuid)
print(result_url)
```

Sample output:

```
['https://mklief9bkq.dev2.crosschain.computer', 'https://epz3zdkwuv.pvm.nebulablock.com', 'https://ug9rrs2dzn.cp.filezoo.com.cn']
```

Once the job status is `Running`, you can try call the api in the [api doc](https://github.com/swanchain/awesome-swanchain/blob/main/serverless-api/Readme.md). Or try the curl below (change the result url to the one you get):

* Test the connection:

```Bash
curl --location '{{result_url}}' 
```

* Post a new item:

```Bash
curl --location '{{result_url}}/items/' \
--header 'Content-Type: application/json' \
--data '{
    "id":1,
    "name":"test",
    "description": "test description",
    "price":6.75,
    "tax":0.5

}'
```

* Get all items:

```Bash
curl --location '{{result_url}}/items'
```

* Get a specific item:

```Bash
curl --location '{{result_url}}/items/1'
```
