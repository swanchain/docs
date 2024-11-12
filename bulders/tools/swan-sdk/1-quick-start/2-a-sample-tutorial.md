
# A Sample Tutorial

For more detailed samples, consult [SDK Samples](https://github.com/swanchain/python-sdk-docs-samples).

For detailed description of functions, please check [Key Functions](https://github.com/swanchain/python-swan-sdk/blob/main/docs/key_functions.md).

## Orchestrator

Orchestrator allows you to create task to run application instances to the powerful distributed computing providers network.

### Fetch available instance resources

Before using Orchestrator to deploy task, it is necessary to know which instance resources are available. Through `get_instance_resources` you can get a list of available instance resources including their `region` information. From the output list, you can choose an `instance_type` by checking the description for the hardware configuration requirements.

```python
import json
import swan

swan_orchestrator = swan.resource(api_key='<SWAN_API_KEY>', service_name='Orchestrator')

available_resources = swan_orchestrator.get_instance_resources()
print(available_resources)
```

Sample output:

```
[InstanceResource({
  "hardware_id": 0,
  "instance_type": "C1ae.small",
  "description": "CPU only \u00b7 2 vCPU \u00b7 2 GiB",
  "type": "CPU",
  "region": [
    "Quebec-CA",
    "North Carolina-US"
  ],
  "price": "1.2",
  "status": "available",
  "snapshot_id": 1731441600,
  "expiry_time": 1731442218
}), ...]
```


### Create and deploy a task

Deploy a simple application with Swan SDK:

```python
import json
import swan

swan_orchestrator = swan.resource(api_key='<SWAN_API_KEY>', service_name='Orchestrator')

result = swan_orchestrator.create_task(
    repo_uri='https://github.com/swanchain/awesome-swanchain/tree/main/hello_world',
    wallet_address='<WALLET_ADDRESS>',
    private_key='<PRIVATE_KEY>',
    instance_type='C1ae.small',
    auto_pay=True
)
task_uuid = result['task_uuid']
# Get task deployment info
task_deployment_info = swan_orchestrator.get_deployment_info(task_uuid=task_uuid)
print(task_deployment_info)
```

It may take up to 5 minutes to get the deployment result:

```python
# Get application instances URL
app_urls = swan_orchestrator.get_real_url(task_uuid)
print(app_urls)
```
A sample output:

```
['https://krfswstf2g.anlu.loveismoney.fun', 'https://l2s5o476wf.cp162.bmysec.xyz', 'https://e2uw19k9uq.cp5.node.study']
```

It shows that this task has three applications. Open the URL in the web browser you will view the application's information if it is running correctly.

### Check information of an existing task

With Orchestrator, you can check information for an existing task to follow up or view task deployment.

```python
import json
import swan

swan_orchestrator = swan.resource(api_key='<SWAN_API_KEY>', service_name='Orchestrator')

# Get an existing task deployment info
task_deployment_info = swan_orchestrator.get_deployment_info(<task_uuid>)
print(task_deployment_info)
```

### Access application instances of an existing task

With Orchestrator, you can easily get the deployed application instances for an existing task.

```python
import json
import swan

swan_orchestrator = swan.resource(api_key='<SWAN_API_KEY>', service_name='Orchestrator')

# Get application instances URL
app_urls = swan_orchestrator.get_real_url(<task_uuid>)
print(app_urls)
```

### Renew an existing task

If you have already submitted payment for the renewal of a task, you can use the `tx_hash` with `renew_task` to extend the task.

```python
import json
import swan

swan_orchestrator = swan.resource(api_key='<SWAN_API_KEY>', service_name='Orchestrator')

renew_result = swan_orchestrator.renew_task(
    task_uuid=<task_uuid>, 
    duration=3600, # Optional: default 3600 seconds (1 hour)
    auto_pay=True, 
    private_key=<PRIVATE_KEY>,
    instance_type=<instance_type> 
)
print(renew_result)
```

### Terminate an existing task

You can also early terminate an existing task and its application instances. By terminating task, you will stop all the related running application instances and thus you will get refund of the remaining task duration.

```python
import json
import swan

swan_orchestrator = swan.resource(api_key='<SWAN_API_KEY>', service_name='Orchestrator')

# Terminate an existing task (and its application instances)
swan_orchestrator.terminate_task(<task_uuid>)
```

