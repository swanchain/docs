---
description: Get started with Swan SDK
---

# Swan SDK

## Overview

The Swan SDK is a toolkit designed to simplify interactions with the Swan Chain Network Resource. It provides a streamlined interface for creating and managing computational tasks, retrieving hardware information, processing payments, and monitoring task statuses.

### Chain Node Web Application

In this example, you will deploy a simple web application on the distributed computing provider network using Swan SDK. At the end of this example, you will have a Chain Node Frontend application running on the Swan network.

#### Create Task and Deploy Application Instances

{% tabs %}
{% tab title="Python" %}
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
{% endtab %}

{% tab title="Go" %}
```go
import "github.com/swanchain/go-swan-sdk"

task, err := client.CreateTask(&CreateTaskReq{
    PrivateKey:   "<PRIVATE_KEY>",
    RepoUri:      "https://github.com/swanchain/awesome-swanchain/tree/main/ChainNode",
    Duration:     2 * time.Hour,
    InstanceType: "C1ae.small", 
})

taskUUID := task.Task.UUID

// Get task deployment info
resp, err := client.TaskInfo(taskUUID)

//Get application instances URL
appUrls, err := client.GetRealUrl(taskUUID)
if err != nil {
	log.Fatalln(err)
}
log.Printf("%v", appUrls)
```
{% endtab %}
{% endtabs %}

Sample URL output:

```
['https://0sz7wqp79q.dev2.crosschain.computer', 'https://grxfl2u0cu.cp.filezoo.com.cn', 'https://0ux851gqmz.pvm.nebulablock.com']
```

#### View Running Application

Screenshot:

<figure><img src="../../../.gitbook/assets/sdk_app_running.png" alt="Chain Node App"><figcaption></figcaption></figure>

## Documentation and Support

More resources about swan SDK can be found here:

* [Swan Console platform](https://console.swanchain.io)
* [Deploying with Swan SDK](https://docs.swanchain.io/start-here/readme/deploying-with-swan-sdk)
* [Python-swan-sdk](https://github.com/swanchain/python-swan-sdk)
* [Go-swan-sdk](https://github.com/swanchain/go-swan-sdk)
* [Python-swan-sdk-samples](https://github.com/swanchain/python-swan-sdk)
* [Go-swan-sdk-samples](https://github.com/swanchain/go-swan-sdk-samples)



***
