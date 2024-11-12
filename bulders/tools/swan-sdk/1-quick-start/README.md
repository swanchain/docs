# Python Swan SDK

This guide details the steps needed to install or update the SWAN SDK for Python. The SDK is a comprehensive toolkit designed to facilitate seamless interactions with the SwanChain API.

## Installation

To use Swan SDK, you first need to install it and its dependencies. Before installing Swan SDK, install Python 3.8 or later and web3.py(==6.15.1).

Install the latest Swan SDK release via **pip**:

```bash
pip install swan-sdk
```

Or install via GitHub:

```bash
git clone https://github.com/swanchain/python-swan-sdk.git
cd python-swan-sdk
pip install .
```

## Get Swan API Key

To use `swan-sdk`, an Swan API key is required. Steps to get an API Key:

* Go to [Swan Console](https://console.swanchain.io/), switch network to [Swan Chain Mainnet](https://docs.swanchain.io/network-reference/readme).
* Login with your wallet.
* Click `API Keys` -> `Generate API Key`

## Using Swan

To use Swan SDK, you must first import it and indicate which service you're going to use:

```python
import swan

swan_orchestrator = swan.resource(api_key='<SWAN_API_KEY>', service_name='Orchestrator')
```

Now that you have an `Orchestrator` service, you can create and deploy instance applications as an Orchestrator task with the service.

```python
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

```python
# Get task deployment info
task_deployment_info = swan_orchestrator.get_deployment_info(task_uuid=task_uuid)
print(task_deployment_info)

# Get application instances URL
app_urls = swan_orchestrator.get_real_url(task_uuid)
print(app_urls)
```
