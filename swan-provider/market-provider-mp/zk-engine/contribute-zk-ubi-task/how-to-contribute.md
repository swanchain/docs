# How to Contribute

To contribute your zk-tasks to the UBI Task Pool, please follow these steps. For each task, submit a separate application. If you have multiple tasks, kindly submit individual applications for each.

### Step 1: Fill out Information:

Provide details below and create a new [GitHub Issue](https://github.com/swanchain/devgrants/issues).

**ZK-Task Information:**&#x20;

* Describe your zk-task, including task details, completion requirements, and minimal resource prerequisites.
* **Resource Requirements:**
  * RAM:
  * Storage:
  * vCPU numbers:
  * GPU:
* **Input Parameters:**
  * Specify input parameters required for your zk-task.
* **Task Completion:**
  * Clarify the method to complete your task (customized binary, or other).
* **Output:**
  * Describe the zk proof generated as output.
* **Verification:**
  * Explain how to verify your proof.
* **Additional Parameters:**
  * Mention any other necessary parameters.
* _**A complete example**: Include the task sample, proof, verify result etc._

> _Note: Repeat these steps for each zk-task if multiple tasks are being submitted._

**Example Application:**

> **ZK-Task Information:**
>
> * _Description:_ Implementing a customized binary for secure data processing.
> * _Resource Requirements:_
>   * RAM: 16GB
>   * Storage: 200GB
>   * vCPU numbers: 4
>   * GPU: Nvidia GTX 1080
> * _Input Parameters:_ JSON file with task specifications.
> * _Task Completion:_ Utilize the provided binary for data processing.
> * _Output:_ ZK proof generated after task completion.
> * _Verification:_ Refer to the attached verification guide.
> * &#x20;Additional Parameters: _Any additional information, requirements, or dependencies._
> * _A complete example: Include the task sample, proof, verify result etc._

**Submission Guidelines:**

1. Fill out the information above.
2. Create a new [GitHub Issue](https://github.com/swanchain/devgrants/issues). with this information.Once the issue is reviewed and approved, we will provide you with the "ubi\_engine\_base\_url" for you to submit your zk-task.
3. Label the issue with "zk-task" and any specific task identifiers.

We appreciate your contributions to elevate the UBI Task Pool with your innovative zk-tasks.

### Step 2: Submit your ZK-task to the UBI Task Pool

You can submit your zk-task to the pool through the API.

* Method: \[POST] {ubi\_hub\_base\_url}/v1/tasks
* Request Parameters:

```
//Parameters examples:
{
  "name": "example_task",
  "type": 1,
  "zk_type": "fil-c2-512M",
  "input_param": "example_input_zsdt_url",
  "verify_param": "example_verify_url",
  "resource_id": 1
}
```

| Parameter      | Type   | Description                                                                                                                            |
| -------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| `name`         | string | Task name                                                                                                                              |
| `type`         | int    | Task type 0: CPU; 1: GPU                                                                                                               |
| `zk_type`      | string | ZK type (e.g., fil-c2-512M、fil-c2-32G)                                                                                                 |
| `input_param`  | string | Network storage address of the input parameter file for executing UBI-task, compressed in zsdt format (recommended to use MCS storage) |
| `verify_param` | string | Network storage address of the input parameter file for verifying UBI-task results (recommended to use MCS storage)                    |
| `resource_id`  | int    | Resource ID 1,2,3,4                                                                                                                    |

* Response:&#x20;

```
// return the task_id
{
  "task_id": 1,
}
```

#### Request Demo

```
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
	"github.com/swanchain/ubi-benchmark/utils"
	"net/http"
)

func main() {
	var task = Task{
		Name:        "example_task",
		Type:        1,
		ZkType:      "fil-c2-512M",
		InputParam:  "example_input_zsdt_url",
		VerifyParam: "example_verify_url",
		ResourceID:  GPU512,
	}

	jsonData, err := json.Marshal(task)
	if err != nil {
		log.Errorf("JSON encoding failed: %v", err)
		return
	}

	resp, err := http.Post(url, "application/json", bytes.NewBuffer(jsonData))
	if err != nil {
		log.Fatal("POST request failed: %v", err)
		return
	}
	defer resp.Body.Close()

	if resp.StatusCode == http.StatusOK {
		fmt.Printf("Request successful, status code: %d \n", resp.StatusCode)
	} else {
		fmt.Printf("Request failed, status code: %d \n", resp.StatusCode)
	}
}

type Task struct {
	Name        string `json:"name"`
	Type        int    `json:"type"`
	ZkType      string `json:"zk_type"`
	InputParam  string `json:"input_param"`
	VerifyParam string `json:"verify_param"`
	ResourceID  int    `json:"resource_id"`
}

const (
	CPU512 = 1
	CPU32G = 2
	GPU512 = 3
	GPU32G = 4
)
```

<table><thead><tr><th width="87">resourceId</th><th width="87">type</th><th width="87">cpu</th><th width="87">memory_gb</th><th width="87">storage_gb</th></tr></thead><tbody><tr><td>1</td><td>CPU512</td><td>5</td><td>3</td><td>30</td></tr><tr><td>2</td><td>CPU32G</td><td>8</td><td>275</td><td>50</td></tr><tr><td>3</td><td>GPU512</td><td>5</td><td>3</td><td>30</td></tr><tr><td>4</td><td>GPU32G</td><td>8</td><td>275</td><td>50</td></tr></tbody></table>

_**Note**: If the configurations mentioned above (GPU/CPU) do not meet your requirements, please leave your specific requirements in the_ [_GitHub issue_](https://github.com/swanchain/devgrants/issues)_._

### Step 3: Get the ZK-Task Proof

You can request your zk-task's result from the pool through the API.

* Method: \[GET] {ubi\_hub\_base\_url}/v1/tasks/{taskId}
* Request Parameters:

```
// Parameters Examples:
{
  "id": 1,
  "type": 1,
  "zk_type": "fil-c2-512M",
  "proof": "xxxxx"
}
```

| Parameter | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| `id`      | string | Task id                                       |
| `type`    | int    | Task type                                     |
| `zk_type` | string | ZK type                                       |
| `proof`   | string | Proof/result after the completion of the task |

* Response:&#x20;

```
// return the task result.
{
  "proof": "xxxxx",
}
```

Thank you for your contribution, and we hope your zk-task brings added value to the Swan decentralized computing ecosystem!
