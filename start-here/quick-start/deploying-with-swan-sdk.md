# Deploying with Swan SDK

The Swan SDK is a toolkit designed to simplify interactions with the Swan Chain Network Resource. It provides a streamlined interface for creating and managing computational tasks, retrieving hardware information, processing payments, and monitoring task statuses.

{% hint style="info" %}
_**For more sample tutorial, please refer to**_ [_**Python Swan SDK Samples**_](https://github.com/swanchain/python-sdk-docs-samples) _**or**_ [_**Go Swan SDK Samples**_](https://github.com/swanchain/go-swan-sdk-samples)_**.**_
{% endhint %}

### Get Swan API Key <a href="#get-orchestrator-api-key" id="get-orchestrator-api-key"></a>

To use `swan-sdk`, an Swan API key is required.

Steps to get an API Key:

* Go to [Orchestrator Dashboard](https://orchestrator.swanchain.io/provider-status), switch network to Mainnet.
* Login with your Wallet.
* Click the user icon on the top right.
* Click `Show API-Key` -> `New API Key`

## Quick Start <a href="#installation" id="installation"></a>

{% tabs %}
{% tab title="Python" %}
### Installation <a href="#quickstart" id="quickstart"></a>

To use Python Swan SDK, you first need to install it and its dependencies. Before installing Swan SDK, install Python 3.8 or later and web3.py(==6.15.1).

Install the latest Swan SDK release via **pip**:

```sh
pip install swan-sdk
```

Or install via GitHub:

```sh
git clone https://github.com/swanchain/python-swan-sdk.git
cd python-swan-sdk
pip install .
```

### Using Python Swan SDK <a href="#quickstart" id="quickstart"></a>

To use Python Swan SDK, you must first import it and indicate which service you're going to use:

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
print(json.dumps(task_deployment_info, indent=2))

# Get application instances URL
app_urls = swan_orchestrator.get_real_url(task_uuid)
print(app_urls)
```
{% endtab %}

{% tab title="Go" %}
### Installation <a href="#quickstart" id="quickstart"></a>

**Go Version**

`go-swan-sdk` requires [Go](https://go.dev/) version [1.21](https://go.dev/doc/devel/release#go1.21.0) or above.

With [Go's module support](https://go.dev/wiki/Modules#how-to-use-modules), `go [build|run|test]` automatically fetches the necessary dependencies when you add `import`in your project:

```go
import "github.com/swanchain/go-swan-sdk"
```

To update the SDK use `go get -u` to retrieve the latest version of the SDK:

```go
go get -u github.com/swanchain/go-swan-sdk
```

### Using Go Swan SDK <a href="#quickstart" id="quickstart"></a>

To use `go-swan-sdk`, you must first import it, and you can create and deploy instance applications quickly.

```go
import (
	"log"
	"time"
	
	"github.com/swanchain/go-swan-sdk"
)

client, err := swan.NewAPIClient(apiKey)
if err != nil {
	log.Fatalf("failed to init swan client, error: %v \n", err)
}
task, err := client.CreateTask(&CreateTaskReq{
    PrivateKey:   "<PRIVATE_KEY>",
    RepoUri:      "https://github.com/swanchain/awesome-swanchain/tree/main/hello_world",
    Duration:     2 * time.Hour,
    InstanceType: "C1ae.small", 
})

taskUUID := task.Task.UUID

// Get task deployment info
resp, err := client.TaskInfo(taskUUID)

// Get application instances URL
appUrls, err := client.GetRealUrl(taskUUID)
if err != nil {
	log.Fatalln(err)
}
log.Printf("app urls: %v", appUrls)
```
{% endtab %}
{% endtabs %}

### Documentation and Support <a href="#documentation-and-support" id="documentation-and-support"></a>

More resources about swan SDK can be found here:

* [Swan Console platform](https://console.swanchain.io/)
* [Deploying with Swan SDK](https://docs.swanchain.io/start-here/readme/deploying-with-swan-sdk)
* [Python-swan-sdk](https://github.com/swanchain/python-swan-sdk)
* [Go-swan-sdk](https://github.com/swanchain/go-swan-sdk)
* [Python-swan-sdk-samples](https://github.com/swanchain/python-swan-sdk)
* [Go-swan-sdk-samples](https://github.com/swanchain/go-swan-sdk-samples)
