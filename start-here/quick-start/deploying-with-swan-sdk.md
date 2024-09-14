# Deploying with Swan SDK

The Swan SDK is a Python toolkit designed to simplify interactions with the Swan Chain Network Resource. It provides a streamlined interface for creating and managing computational tasks, retrieving hardware information, processing payments, and monitoring task statuses.

{% hint style="info" %}
_**For more sample tutorial, please refer to**_ [_**Python SDK Docs Samples**_](https://github.com/swanchain/python-sdk-docs-samples)
{% endhint %}

### Installation <a href="#installation" id="installation"></a>

To use Swan SDK, you first need to install it and its dependencies. Before installing Swan SDK, install Python 3.8 or later and web3.py(==6.15.1).

Install the latest Swan SDK release via **pip**:

```
pip install swan-sdk
```

Or install via GitHub:

```
git clone https://github.com/swanchain/python-swan-sdk.git
cd python-swan-sdk
pip install .
```

### Get Orchestrator API Key <a href="#get-orchestrator-api-key" id="get-orchestrator-api-key"></a>

To use `swan-sdk`, an Orchestrator API key is required.

Steps to get an API Key:

* Go to [Orchestrator Dashboard](https://orchestrator.swanchain.io/provider-status), switch network to Mainnet.
* Login through MetaMask.
* Click the user icon on the top right.
* Click 'Show API-Key' -> 'New API Key'
* Store your API Key safely, do not share with others.

### Using Swan <a href="#using-swan" id="using-swan"></a>

To use Swan SDK, you must first import it and indicate which service you're going to use:

```
import swan

swan_orchestrator = swan.resource(api_key='<SWAN_API_KEY>', service_name='Orchestrator')
```

Now that you have an `Orchestrator` service, you can create and deploy instance applications as an Orchestrator task with the service.

```
result = swan_orchestrator.create_task(
    repo_uri='https://github.com/swanchain/awesome-swanchain/tree/main/hello_world',
    wallet_address='<WALLET_ADDRESS>',
    private_key='<PRIVATE_KEY>',
    instance_type='C1ae.small',
    auto_pay=True
)
task_uuid = result['task_uuid']
```

Then you can follow up task deployment information and the URL for running applications.

```
# Get task deployment info
task_deployment_info = swan_orchestrator.get_deployment_info(task_uuid=task_uuid)
print(json.dumps(task_deployment_info, indent=2))

# Get application instances URL
app_urls = swan_orchestrator.get_real_url(task_uuid)
print(app_urls)
```
