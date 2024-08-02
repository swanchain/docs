# Build a Basic Web Application

In this part, you will learn to deploy a simple web application to the powerful distributed computing provider network using Swan SDK.

**Prerequisites** Before starting this tutorial, you will need:

* An Swan API Key: if you don't already have one follow the Setting Up Your Swan Environment tutorial.
* Installed on your environment: `Python` (3.8 or later) and `web3.py` (6.15 or later).
* (Optional) a docker deployable application on GitHub repository.

### 1. Login into Orchestrator Through SDK

To use swan-sdk you will need to login to Orchestrator using API Key. (Wallet login is not supported)

```python
import swan

# default use testnet
swan_orchestrator = swan.resource(api_key="<your_api_key>", service_name='Orchestrator')

# To use mainnet
swan_orchestrator = swan.resource(api_key="<your_api_key>", network='mainnet', service_name='Orchestrator')
```

### 2. Create task

Task is created through Swan Orchestrator to send appication deployment order to the distributed computing provider network. In the simplified the example, the `auto_pay` option is by default set.

```python
result = orchestrator.create_task(
    app_repo_image='Tetris',
    wallet_address='<WALLET_ADDRESS>',
    private_key='<PRIVATE_KEY>',
)
task_uuid = result['id']
```

### 3. Review the deployment result

After getting a task uuid from the previous step, the deployment result can be tracked using the Orchestrator interface `get_deployment_info`:

```py
task_info = orchestrator.get_deployment_info(task_uuid=task_uuid)
print(task_info)
```

<details>

<summary>Sample output (click to expand)</summary>

```

{
  "data": {
    "computing_providers": [
      {
        "beneficiary": "0xBdDe0ffED293638De69ABD0fCf42237AD3F2cf94",
        "cp_account_address": "0x2bd6a6f41b37152677F8b4946490580F63494abD",
        "created_at": 1722488518,
        "freeze_online": null,
        "id": 99,
        "lat": 35.8639,
        "lon": -78.535,
        "multi_address": [
          "/ip4/40.143.96.125/tcp/10011"
        ],
        "name": "new-cp-001",
        "node_id": "04d5b210591aa5aff5b4e49ad6a3ec57b72aefcdc99cd7888fff80b5991452d8a8dce099312cfb7e78637e04e9824a7274160e49176a00394745701ed450a113e2",
        "online": 1,
        "owner_address": "0xBdDe0ffED293638De69ABD0fCf42237AD3F2cf94",
        "region": "North Carolina-US",
        "task_types": "[1, 3]",
        "updated_at": 1722544641,
        "version": "2.0",
        "worker_address": "0xBdDe0ffED293638De69ABD0fCf42237AD3F2cf94"
      },
      {
        "beneficiary": "0x9A5D8Ac48Eb205eCf0B45428bF19DC1ADC1BC186",
        "cp_account_address": "0x6f43E3e5B70aa5BF5818c56D509BDd092D0907E0",
        "created_at": 1722488655,
        "freeze_online": null,
        "id": 100,
        "lat": 45.5075,
        "lon": -73.5887,
        "multi_address": [
          "/ip4/38.140.46.60/tcp/8086"
        ],
        "name": "test244-seq",
        "node_id": "04241e19381a8fad4cc98ef6de0a7e417e6d662ff49d8096cff9ec4b08798eeb96687ff5c7b4bde1adb8ccdbb579f16ac0f2c4e0853406282a37285582879dde49",
        "online": 1,
        "owner_address": "0x9A5D8Ac48Eb205eCf0B45428bF19DC1ADC1BC186",
        "region": "Quebec-CA",
        "task_types": "[1, 4, 3]",
        "updated_at": 1722544641,
        "version": "2.0",
        "worker_address": "0x9A5D8Ac48Eb205eCf0B45428bF19DC1ADC1BC186"
      }
    ],
    "jobs": [
      {
        "build_log": "wss://log.cp.filezoo.com.cn:10011/api/v1/computing/lagrange/spaces/log?space_id=QmR7SP2ANxW55w9u6JuxvRs2wAD7asEibn9n6DKsykwR3U&type=build",
        "comments": "Running(downloadSource). downloadSource: no job_result_uri from api. downloadSource(Submitted).",
        "container_log": "wss://log.cp.filezoo.com.cn:10011/api/v1/computing/lagrange/spaces/log?space_id=QmR7SP2ANxW55w9u6JuxvRs2wAD7asEibn9n6DKsykwR3U&type=container",
        "cp_account_address": "0x2bd6a6f41b37152677F8b4946490580F63494abD",
        "created_at": 1722544628,
        "duration": 3600,
        "ended_at": null,
        "hardware": "C1ae.small",
        "id": 5,
        "job_real_uri": "https://g7dlk8hii5.cp.filezoo.com.cn",
        "job_result_uri": null,
        "job_source_uri": "https://plutotest.acl.swanipfs.com/ipfs/QmR7SP2ANxW55w9u6JuxvRs2wAD7asEibn9n6DKsykwR3U",
        "name": "Job-cb9e9afc-f51c-4fb3-9f70-384e9342e516",
        "node_id": "04d5b210591aa5aff5b4e49ad6a3ec57b72aefcdc99cd7888fff80b5991452d8a8dce099312cfb7e78637e04e9824a7274160e49176a00394745701ed450a113e2",
        "start_at": 1722544628,
        "status": "Running",
        "storage_source": "swanhub",
        "task_uuid": "f6e81501-4d59-44fe-9ce9-85f8ccc86529",
        "type": null,
        "updated_at": 1722544659,
        "uuid": "cb9e9afc-f51c-4fb3-9f70-384e9342e516"
      },
      {
        "build_log": "wss://log.dev2.crosschain.computer:8086/api/v1/computing/lagrange/spaces/log?space_id=QmR7SP2ANxW55w9u6JuxvRs2wAD7asEibn9n6DKsykwR3U&type=build",
        "comments": "Running(downloadSource). downloadSource: no job_result_uri from api. downloadSource(Submitted).",
        "container_log": "wss://log.dev2.crosschain.computer:8086/api/v1/computing/lagrange/spaces/log?space_id=QmR7SP2ANxW55w9u6JuxvRs2wAD7asEibn9n6DKsykwR3U&type=container",
        "cp_account_address": "0x6f43E3e5B70aa5BF5818c56D509BDd092D0907E0",
        "created_at": 1722544628,
        "duration": 3600,
        "ended_at": null,
        "hardware": "C1ae.small",
        "id": 6,
        "job_real_uri": "https://cobgjxtu2x.dev2.crosschain.computer",
        "job_result_uri": null,
        "job_source_uri": "https://plutotest.acl.swanipfs.com/ipfs/QmR7SP2ANxW55w9u6JuxvRs2wAD7asEibn9n6DKsykwR3U",
        "name": "Job-128d1de6-51f9-49d6-a5b2-5a40ad2209d4",
        "node_id": "04241e19381a8fad4cc98ef6de0a7e417e6d662ff49d8096cff9ec4b08798eeb96687ff5c7b4bde1adb8ccdbb579f16ac0f2c4e0853406282a37285582879dde49",
        "start_at": 1722544629,
        "status": "Running",
        "storage_source": "swanhub",
        "task_uuid": "f6e81501-4d59-44fe-9ce9-85f8ccc86529",
        "type": null,
        "updated_at": 1722544659,
        "uuid": "128d1de6-51f9-49d6-a5b2-5a40ad2209d4"
      }
    ],
    "task": {
      "comments": null,
      "created_at": 1722544608,
      "end_at": 1722548208,
      "id": 3,
      "leading_job_id": "cb9e9afc-f51c-4fb3-9f70-384e9342e516",
      "name": null,
      "refund_amount": null,
      "refund_wallet": "0xaA5812Fb31fAA6C073285acD4cB185dDbeBDC224",
      "source": "v2",
      "start_at": 1722544608,
      "start_in": 300,
      "status": "completed",
      "task_detail": {
        "amount": 0.0,
        "bidder_limit": 3,
        "created_at": 1722544608,
        "dcc_selected_cp_list": null,
        "duration": 3600,
        "end_at": 1722548208,
        "hardware": "C1ae.small",
        "job_result_uri": null,
        "job_source_uri": "https://plutotest.acl.swanipfs.com/ipfs/QmR7SP2ANxW55w9u6JuxvRs2wAD7asEibn9n6DKsykwR3U",
        "price_per_hour": "0.0",
        "requirements": {
          "hardware": "None",
          "hardware_type": "CPU",
          "memory": "2",
          "preferred_cp_list": null,
          "region": "global",
          "storage": null,
          "update_max_lag": null,
          "vcpu": "2"
        },
        "space": {
          "activeOrder": {
            "config": {
              "description": "CPU only \u00b7 2 vCPU \u00b7 2 GiB",
              "hardware": "CPU only",
              "hardware_id": 0,
              "hardware_type": "CPU",
              "memory": 2,
              "name": "C1ae.small",
              "price_per_hour": 0.0,
              "vcpu": 2
            }
          },
          "name": "0",
          "uuid": "1770b0a6-929f-4e50-aa53-2e1614459ae0"
        },
        "start_at": 1722544608,
        "status": "paid",
        "storage_source": "swanhub",
        "type": "None",
        "updated_at": 1722544608
      },
      "task_detail_cid": "https://plutotest.acl.swanipfs.com/ipfs/QmSoWh97T8xUKQMd6HEKhWiuHHeSjXgpY5yFpauW5v1Yo1",
      "tx_hash": null,
      "type": "None",
      "updated_at": 1722544632,
      "user_id": 4,
      "uuid": "f6e81501-4d59-44fe-9ce9-85f8ccc86529"
    }
  },
  "message": "fetch task info for task_uuid='f6e81501-4d59-44fe-9ce9-85f8ccc86529' successfully",
  "status": "success"
}
```

</details>

### 4. Get the deployed application

```py
result_url = orchestrator.get_real_url(task_uuid)
print(result_url)
```

Sample output:

```
['https://m477nntm4f.dev2.crosschain.computer', 'https://aiuro3tlza.cp.filezoo.com.cn']
```

If the job status becomes `Running`, the application can be accessed by the real url through web browser.

Sample screenshot:

<figure><img src="../../.gitbook/assets/image (184).png" alt=""><figcaption></figcaption></figure>
