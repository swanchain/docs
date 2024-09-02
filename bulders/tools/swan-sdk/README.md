---
description: Get started with Swan SDK
---

# Swan SDK

## Overview

The Swan SDK is a Python toolkit designed to simplify interactions with the Swan Chain Network Resource. It provides a streamlined interface for creating and managing computational tasks, retrieving hardware information, processing payments, and monitoring task statuses.

## Quick Example

### Chain Node Web Application

In this example, you will deploy a simple web application on the distributed computing provider network using Swan SDK. At the end of this example, you will have a Chain Node Frontend application running on the Swan network.

#### Create Task and Deploy Application Instances

```python
import swan
import json

api_key = '<your_api_key>'
wallet_address = '<WALLET_ADDRESS>'
private_key = '<PRIVATE_KEY>'

swan_orchestrator = swan.resource(
    api_key=api_key, 
    service_name='Orchestrator'
)

result = swan_orchestrator.create_task(
    repo_uri='https://github.com/swanchain/awesome-swanchain/tree/main/ChainNode',
    wallet_address=wallet_address,
    private_key=private_key,
    auto_pay=True,
    instance_type='C1ae.medium',
)

task_uuid = result['task_uuid']
instance_type = result['instance_type']
task_info = swan_orchestrator.get_deployment_info(task_uuid=task_uuid)
print(json.dumps(task_info, indent=2))

### get real url (if no url, please wait for a while, then check again)
result_url = swan_orchestrator.get_real_url(task_uuid)
print(result_url)
```

Sample URL output:

```
['https://0sz7wqp79q.dev2.crosschain.computer', 'https://grxfl2u0cu.cp.filezoo.com.cn', 'https://0ux851gqmz.pvm.nebulablock.com']
```

#### View Running Application

Screenshot:

<figure><img src="../../../.gitbook/assets/sdk_app_running.png" alt="Chain Node App"><figcaption></figcaption></figure>

## Documentation and Support

For comprehensive guides and support, visit the [Swan SDK GitHub page](https://github.com/swanchain/python-swan-sdk). The repository includes detailed documentation, usage examples, and community support resources.

***
