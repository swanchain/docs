# Dockerfile String Deployment Feature

## Overview
The Dockerfile String Deployment feature enables you to create and deploy applications by passing a Dockerfile configuration directly as a string parameter. This allows for dynamic Dockerfile generation and eliminates the need for physical Dockerfile files in your deployment workflow.

## Usage

### Creating a Task with Dockerfile String

To deploy an application using a Dockerfile string, follow these steps:

1. Define your Dockerfile configuration as a string variable:

```python
DOCKERFILE_CONTENT = """
# Use a lightweight Python image as the base
FROM python:3.9-slim

# Set the working directory to /app
WORKDIR /app

# Create the index.html file with the desired content
RUN echo "dockerfile testing success" > index.html

# Expose port 8000 for the HTTP server
EXPOSE 8000

# Start the HTTP server when the container launches
CMD ["python3", "-m", "http.server", "8000"]
"""
```

2. Create a task specification using `TaskSpecFactory.build_dockerfile_task()`:

```python
dockerfile_task_spec = TaskSpecFactory.build_dockerfile_task(
    hardware_spec=HardwareSpec(
        cpu=2,
        memory=4,
        storage=30,
        gpus=[
            GpuSpec(
                gpu_model="NVIDIA 3080",
                count=1,
            )
        ]
    ),
    dockerfile_content=DOCKERFILE_CONTENT,
)
```

3. Deploy the task using the orchestrator:

```python
result: TaskCreationResult = orchestrator.create_task(
    base_task_spec=dockerfile_task_spec,
    wallet_address=os.getenv("WALLET_ADDRESS"),
    private_key=os.getenv("PRIVATE_KEY"),
    duration=3600,
)
```

## Parameters

The `build_dockerfile_task()` method accepts the following parameters:

| Parameter | Type | Description |
|-----------|------|-------------|
| `hardware_spec` | `HardwareSpec` | Specification of required hardware resources |
| `dockerfile_content` | `str` | Dockerfile configuration as a string |

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
            count=1,                  # Number of GPUs
        )
    ]
)
```

## Dockerfile Format

The Dockerfile string should follow standard Dockerfile syntax:

```dockerfile
FROM <base-image>
WORKDIR <working-directory>
COPY <source> <destination>
RUN <command>
EXPOSE <port>
CMD ["executable", "param1", "param2"]
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

# Define Dockerfile configuration as a string
DOCKERFILE_CONTENT = """
# Use a lightweight Python image as the base
FROM python:3.9-slim

# Set the working directory to /app
WORKDIR /app

# Create the index.html file with the desired content
RUN echo "dockerfile testing success" > index.html

# Expose port 8000 for the HTTP server
EXPOSE 8000

# Start the HTTP server when the container launches
CMD ["python3", "-m", "http.server", "8000"]
"""

class DockerfileStringDeployment(ExampleBase):

    def deploy(self):
        # Create task specification with Dockerfile string
        dockerfile_task_spec = TaskSpecFactory.build_dockerfile_task(
            hardware_spec=HardwareSpec(
                cpu=2,
                memory=4,
                storage=30,
                gpus=[
                    GpuSpec(
                        gpu_model="NVIDIA 3080",
                        count=1,
                    )
                ]
            ),
            dockerfile_content=DOCKERFILE_CONTENT,
        )

        # Deploy the task
        result: TaskCreationResult = self.orchestrator.create_task(
            base_task_spec=dockerfile_task_spec,
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

    deployment = DockerfileStringDeployment()
    deployment.deploy()
```

## Benefits of Using Dockerfile String Deployment

- **Dynamic Image Building**: Generate or modify Dockerfile configurations programmatically
- **Custom Environments**: Create specialized environments tailored to specific application requirements
- **No External Dependencies**: Build and deploy without relying on pre-built Docker images
- **Versioning Control**: Embed Dockerfile configurations directly in your code, ensuring version consistency
- **Simplified CI/CD Integration**: Generate Dockerfiles on-the-fly during deployment pipelines

## Notes

- The Dockerfile string must follow standard Dockerfile syntax
- Complex builds may take longer to deploy as they need to be built from the Dockerfile
- Consider optimizing the Dockerfile to minimize build times
- Multi-stage builds are supported for creating optimized production images
