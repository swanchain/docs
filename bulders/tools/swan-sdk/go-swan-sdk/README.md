# Go Swan SDK

This guide details the steps needed to install or update the SWAN SDK for Go. The SDK is a comprehensive toolkit designed to facilitate seamless interactions with the SwanChain API.

## Getting started

**Go Version**

`go-swan-sdk` requires [Go](https://go.dev/) version [1.21](https://go.dev/doc/devel/release#go1.21.0) or above.

**Swan API Key**

To use `swan-sdk`, an Swan API key is required. Steps to get an API Key:

* Go to [Swan Console](https://console.swanchain.io/), switch network to [Swan Chain Mainnet](https://docs.swanchain.io/network-reference/readme).
* Login with your wallet.
* Click `API Keys` -> `Generate API Key`

**Using go-swan-sdk**

With [Go's module support](https://go.dev/wiki/Modules#how-to-use-modules), `go [build|run|test]` automatically fetches the necessary dependencies when you add `import`in your project:

```go
import "github.com/swanchain/go-swan-sdk"
```

To update the SDK use `go get -u` to retrieve the latest version of the SDK:

```sh
go get -u github.com/swanchain/go-swan-sdk
```

## Quickstart

To use `go-swan-sdk`, you must first import it, and you can create and deploy instance applications quickly.

```go
package main

import (
	"github.com/swanchain/go-swan-sdk"
	"log"
	"time"
)

func main() {
	client, err := swan.NewAPIClient("<YOUR_API_KEY>")
	if err != nil {
		log.Fatalf("failed to init swan client, error: %v \n", err)
	}
	task, err := client.CreateTask(&swan.CreateTaskReq{
		PrivateKey:   "<PRIVATE_KEY>",
		RepoUri:      "https://github.com/swanchain/awesome-swanchain/tree/main/hello_world",
		Duration:     2 * time.Hour,
		InstanceType: "C1ae.small",
	})
	taskUUID := task.Task.UUID

	// Get task deployment info
	resp, err := client.TaskInfo(taskUUID)
	if err != nil {
		log.Fatalln(err)
	}
	log.Printf("task info: %+v \n", resp)

	//Get application instances URL
	appUrls, err := client.GetRealUrl(taskUUID)
	if err != nil {
		log.Fatalln(err)
	}
	log.Printf("app urls: %v \n", appUrls)
}
```
