# A Sample Tutorial

**New client**

```go
client, err := swan.NewAPIClient("<SWAN_API_KEY>")
```

**Fetch all instance resources**

Through `InstanceResources` you can get a list of available instance resources including their region information. You can select one you want to use.

```go
instances, err := swan.InstanceResources(true)
```

> **Note:** All Instance type list can be found [here](https://github.com/swanchain/go-swan-sdk/blob/main/doc/instance.md).

**Create and deploy a task**

Deploy a application, if you have set `PrivateKey`, this task will be payed automaiclly, and deploy to computing providers on Swan Chain Network:

```go
task, err := client.CreateTask(&CreateTaskReq{
    PrivateKey:   "<YOUR_WALLET_ADDRESS_PRIVATE_KEY>",
    RepoUri:      "<YOUR_PROJECT_GITHUB_URL>",
    Duration:      2 * time.Hour,
    InstanceType: "C1ae.small", 
})

taskUUID := task.Task.UUID
log.Printf("taskUUID: %v", taskUUID)

```

**Access application instances of an existing task**

You can easily get the deployed application instances for an existing task.

```go
// Get application instances URL
appUrls, err := client.GetRealUrl("<TASK_UUID>")
if err != nil {
	log.Fatalln(err)
}
log.Printf("app urls: %v", appUrls)
```

A sample output:

```
['https://krfswstf2g.anlu.loveismoney.fun', 'https://l2s5o476wf.cp162.bmysec.xyz', 'https://e2uw19k9uq.cp5.node.study']
```

It shows that this task has three applications. Visit the URL in the web browser you will view the application's information if it is running correctly.

**Renew duration of an existing task**

`RenewTask` extends the duration of the task before completed

```go
resp, err := client.RenewTask("<TASK_UUID>", <Duration>,"<PRIVATE_KEY>")
```

**Terminate an existing task**

You can early terminate an existing task and its application instances. By terminating task, you will stop all the related running application instances and thus you will get refund of the remaining task duration.

```go
resp, err := client.TerminateTask("<TASK_UUID>")
```

**Check information of an existing task**

You can get the task details by the `taskUUID`

```go
resp, err := client.TaskInfo("<TASK_UUID>")
```

**Check all task list information belonging to a wallet address**

You can get all tasks deployed from one wallet address

```go
total, resp, err := client.Tasks(&TaskQueryReq{
    Wallet: "<WALLET_ADDRESS>",
    Page:   0,
    Size:   10,
})
```

### More Samples

For more pratical samples, consult [go-swan-sdk-samples](https://github.com/swanchain/go-swan-sdk-samples).

### More Resources

More resources about swan SDK can be found

* [Swan Console platform](https://console.swanchain.io)
* [Deploying with Swan SDK](https://docs.swanchain.io/start-here/readme/deploying-with-swan-sdk)
* [Python-swan-sdk](https://github.com/swanchain/python-swan-sdk)
* [Python-swan-sdk-samples](https://github.com/swanchain/python-swan-sdk)

### License

The `go-swan-sdk` is released under the **MIT** license, details of which can be found in the LICENSE file.
