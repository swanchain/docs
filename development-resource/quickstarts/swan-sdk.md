# Swan SDK

### Overview

The Swan SDK is a Python toolkit designed to simplify interactions with the SwanChain API. It provides a streamlined interface for creating and managing computational tasks, retrieving hardware information, processing payments, and monitoring task statuses.

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
from swan_sdk import SwanClient

# Initialize the client
client = SwanClient(api_key='your_api_key')

# Retrieve hardware information
hardware_info = client.get_hardware()

# Create a new task
task = client.create_task(
    hardware_id=hardware_info[0]['id'],
    script='print("Hello, SwanChain!")'
)

# Monitor the task status
status = client.get_task_status(task['id'])
print(status)
```

For more detailed information and advanced usage, refer to the [official documentation](https://github.com/swanchain/python-swan-sdk).

### Documentation and Support

For comprehensive guides and support, visit the [Swan SDK GitHub page](https://github.com/swanchain/python-swan-sdk). The repository includes detailed documentation, usage examples, and community support resources.

***
