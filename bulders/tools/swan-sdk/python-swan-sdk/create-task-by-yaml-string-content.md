# YAML String Deployment Feature

## Overview
The YAML String Deployment feature allows you to define and deploy applications by passing a YAML configuration directly as a string parameter instead of referencing a file. This provides flexibility for dynamic configurations and programmatic deployment workflows.

## Usage

### Creating a Task with YAML String

To deploy an application using a YAML string, follow these steps:

1. Define your YAML configuration as a string variable:

```python
YAML_CONTENT = """
version: "2.0"
services:
 image: alex6nbai/gpu_benchmark:20250219-4
 envs:
  - NCCL_P2P_DISABLE=1
  - NCCL_SHM_DISABLE=1
 expose_port:
  - 8000
"""
```

2. Create a task specification using `TaskSpecFactory.build_yaml_task()`:

```python
yaml_task_spec = TaskSpecFactory.build_yaml_task(
    hardware_spec=HardwareSpec(
        cpu=2,
        memory=4,
        storage=30,
        gpus=[
            GpuSpec(
                gpu_model="NVIDIA 3080",
                count=2,
            )
        ]
    ),
    yaml_content=YAML_CONTENT,
)
```

3. Deploy the task using the orchestrator:

```python
result: TaskCreationResult = orchestrator.create_task(
    base_task_spec=yaml_task_spec,
    wallet_address=os.getenv("WALLET_ADDRESS"),
    private_key=os.getenv("PRIVATE_KEY"),
    duration=3600,
)
```

## Parameters

The `build_yaml_task()` method accepts the following parameters:

| Parameter | Type | Description |
|-----------|------|-------------|
| `hardware_spec` | `HardwareSpec` | Specification of required hardware resources |
| `yaml_content` | `str` | YAML configuration as a string |

### Hardware Specification

The `HardwareSpec` object defines the resources required for your task:

```python
HardwareSpec(
    cpu=2,                # Number of CPU cores
    memory=4,             # Memory in GB
    storage=30,           # Storage in GB
    gpus=[                # List of GPU specifications (optional)
        GpuSpec(
            gpu_model="NVIDIA 3080",  # GPU model
            count=2,                  # Number of GPUs
        )
    ]
)
```

## YAML Configuration Format

The YAML string should follow this structure:

```yaml
version: "2.0"
services:
 image: <docker-image>
 envs:
  - <environment-variable-1>
  - <environment-variable-2>
 expose_port:
  - <port-number>
```

## Complete Example

```python
import json
import logging
import os

import dotenv
from swan.object import TaskCreationResult, TaskDeploymentInfo
from swan.object.task_spec import HardwareSpec, GpuSpec, TaskSpecFactory

from base import ExampleBase

# Define YAML configuration as a string
YAML_CONTENT = """
version: "2.0"
services:
 image: alex6nbai/gpu_benchmark:20250219-4
 envs:
  - NCCL_P2P_DISABLE=1
  - NCCL_SHM_DISABLE=1
 expose_port:
  - 8000
"""

class YamlStringDeployment(ExampleBase):

    def deploy(self):
        # Create task specification with YAML string
        yaml_task_spec = TaskSpecFactory.build_yaml_task(
            hardware_spec=HardwareSpec(
                cpu=2,
                memory=4,
                storage=30,
                gpus=[
                    GpuSpec(
                        gpu_model="NVIDIA 3080",
                        count=2,
                    )
                ]
            ),
            yaml_content=YAML_CONTENT,
        )

        # Deploy the task
        result: TaskCreationResult = self.orchestrator.create_task(
            base_task_spec=yaml_task_spec,
            wallet_address=os.getenv("WALLET_ADDRESS"),
            private_key=os.getenv("PRIVATE_KEY"),
            duration=3600,
        )
        
        # Process result
        self.task_uuid = result.task_uuid if result else None
        self.tx_hash = result.tx_hash if result else None
        
        if not self.task_uuid:
            logging.error("Failed to initialize task.")
            return
            
        if not self.tx_hash:
            logging.error("Failed to create task, transaction hash not found.")
            return
            
        task_info: TaskDeploymentInfo = self.orchestrator.get_deployment_info(task_uuid=self.task_uuid)
        logging.info(task_info)
        logging.info(f"task_uuid: {self.task_uuid}")
        
        return result

if __name__ == "__main__":
    dotenv.load_dotenv()
    logging.basicConfig(level=logging.INFO)

    deployment = YamlStringDeployment()
    deployment.deploy()
```

## Benefits of Using YAML String Deployment

- **Dynamic Configuration**: Generate or modify YAML configurations programmatically
- **Simplified Deployment**: No need to create temporary YAML files
- **Integration Flexibility**: Easily integrate with web services or applications that generate configurations dynamically
- **Version Control**: Store multiple configuration versions in your code

## Notes

- Ensure your YAML string follows the proper indentation and format
- For complex configurations, consider validating the YAML string before deployment
- Environment variables in your YAML configuration will be applied to the deployed container
