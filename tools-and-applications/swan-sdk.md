# Swan SDK

### Overview

The Swan SDK is a Python toolkit designed to simplify interactions with the Swan Chain Network Resource. It provides a streamlined interface for creating and managing computational tasks, retrieving hardware information, processing payments, and monitoring task statuses.

### Installation

To install the Swan SDK, use the following command:

```bash
pip install swan-sdk
```

Alternatively, you can clone the repository from GitHub:

```bash
git clone https://github.com/swanchain/python-swan-sdk.git
```

### Key Features

* **API Client Integration**: Easily connect to the SwanChain API.
* **Data Models**: Utilize pre-defined models for seamless data handling.
* **Task Management**: Create, monitor, and manage computational tasks.
* **Payment Processing**: Handle payments for computational resources.
* **Status Monitoring**: Track the status of your tasks in real-time.

### Getting Started

Here’s a basic example to help you get started with the Swan SDK:

```python
import os
import swan

swan_orchestrator = swan.resource(api_key='<SWAN_API_KEY>', service_name='Orchestrator')

result = swan_orchestrator.create_task(
    app_repo_image='hello-world',
    wallet_address='<WALLET_ADDRESS>',
    private_key='<PRIVATE_KEY>',
)
task_uuid = result['id']
# Get task info
task_info = swan_orchestrator.get_deployment_info(task_uuid=task_uuid)
print(task_info)
```

A sample output:

```
['https://krfswstf2g.anlu.loveismoney.fun', 'https://l2s5o476wf.cp162.bmysec.xyz', 'https://e2uw19k9uq.cp5.node.study']
```

The "hello-world" task is deployed to 3 computing providers, open any link in the browser&#x20;

```
Hello World
```

For more detailed information and advanced usage, refer to the [official documentation](https://github.com/swanchain/python-swan-sdk).

### Documentation and Support

For comprehensive guides and support, visit the [Swan SDK GitHub page](https://github.com/swanchain/python-swan-sdk). The repository includes detailed documentation, usage examples, and community support resources.

***
