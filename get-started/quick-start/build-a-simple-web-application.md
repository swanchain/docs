### 4. Build a simple web application

In this tutorial, you will learn how to deploy a simple web application on the distributed computing provider network using Swan SDK. At the end of this tutorial, you will have a Chain Node Frontend application running on the Swan network.

**Prerequisites**:

* A Swan API Key: If you do not have one, follow the [Setting Up Your Swan Environment](../quick-start/setting-up-your-swan-environment.md) tutorial.
* Python (version 3.8 or later) and `pip install web3>=6.15` (version 6.15 or later) installed in your environment.
* **Optional** A repo containing the application code. For this tutorial, we will use the ChainNode application from the [SwanChain Official GitHub repository](https://github.com/swanchain/awesome-swanchain/tree/main/ChainNode)

**Note** : [Click here to see the full demo code](https://github.com/swanchain/python-swan-sdk/blob/main/examples/ex1_webapp.py)
#### 1. Log in to Orchestrator via SDK

To use `swan-sdk`, log in to Orchestrator with your API Key. (Wallet login is not supported.)

```python
import swan
import json
swan_orchestrator = swan.resource(api_key="<your_api_key>", network='mainnet', service_name='Orchestrator')
```

#### 2. Create a Task

A task is created through Swan Orchestrator to send deployment orders to the distributed computing provider network.

Here, we deploy a ChainNode application with a free configuration and a one-hour duration (these settings are the default).

**Notes**: Even it's a free configuration, you still need to have sufficient ETH in your wallet to pay for the gas fee. Click [swan bridge](https://superbridge.app/swan-chain) to bridge the ETH.

```python
result = swan_orchestrator.create_task(
    repo_uri='https://github.com/swanchain/awesome-swanchain/tree/main/ChainNode',
    wallet_address='<WALLET_ADDRESS>',
    private_key='<PRIVATE_KEY>',
    auto_pay=True
)
task_uuid = result['id']
```
#### 3. Review the Deployment Result

After obtaining the task UUID from the previous step, you can track the deployment result using the Orchestrator interface `get_deployment_info`:

```python
task_info = swan_orchestrator.get_deployment_info(task_uuid=task_uuid)
print(json.dumps(task_info, indent=2))
```

<details>
<summary>Sample output (click to expand)</summary>

```json

{
  "data":{
    "computing_providers":[
      {
        "beneficiary":"0xC2522AE0392c6AFc61C7f3B2e4dF3c5E8A69a794",
        "cp_account_address":"0xE974b17d9D730CAe75a228Df7eCa452e31E06276",
        "created_at":1723741012,
        "freeze_online":"None",
        "id":111,
        "lat":45.5075,
        "lon":-73.5887,
        "multi_address":[
          "/ip4/38.80.81.161/tcp/8085"
        ],
        "name":"swancp.pvm.nebulablock.com",
        "node_id":"04da2df41b0bc7804c6fe92205ee00a919412b74cf63647a340baee75e3b89c3ca1cf0b80163c18e36be57885aa7b6af011c813e8ec4b4559a4732293119e6b670",
        "online":1,
        "owner_address":"0xC2522AE0392c6AFc61C7f3B2e4dF3c5E8A69a794",
        "region":"Quebec-CA",
        "task_types":"[1, 2, 3, 4]",
        "updated_at":1724283112,
        "version":"2.0",
        "worker_address":"0xC2522AE0392c6AFc61C7f3B2e4dF3c5E8A69a794"
      },
      {
        "beneficiary":"0x9A5D8Ac48Eb205eCf0B45428bF19DC1ADC1BC186",
        "cp_account_address":"0x6f43E3e5B70aa5BF5818c56D509BDd092D0907E0",
        "created_at":1722488655,
        "freeze_online":1,
        "id":100,
        "lat":45.5075,
        "lon":-73.5887,
        "multi_address":[
          "/ip4/38.140.46.60/tcp/8086"
        ],
        "name":"test244-seq",
        "node_id":"04241e19381a8fad4cc98ef6de0a7e417e6d662ff49d8096cff9ec4b08798eeb96687ff5c7b4bde1adb8ccdbb579f16ac0f2c4e0853406282a37285582879dde49",
        "online":1,
        "owner_address":"0x9A5D8Ac48Eb205eCf0B45428bF19DC1ADC1BC186",
        "region":"Quebec-CA",
        "task_types":"[1, 2, 3, 4]",
        "updated_at":1724283128,
        "version":"2.0",
        "worker_address":"0x9A5D8Ac48Eb205eCf0B45428bF19DC1ADC1BC186"
      },
      {
        "beneficiary":"0xBdDe0ffED293638De69ABD0fCf42237AD3F2cf94",
        "cp_account_address":"0x2bd6a6f41b37152677F8b4946490580F63494abD",
        "created_at":1722488518,
        "freeze_online":1,
        "id":99,
        "lat":35.8639,
        "lon":-78.535,
        "multi_address":[
          "/ip4/40.143.96.125/tcp/10011"
        ],
        "name":"new-cp-001",
        "node_id":"04d5b210591aa5aff5b4e49ad6a3ec57b72aefcdc99cd7888fff80b5991452d8a8dce099312cfb7e78637e04e9824a7274160e49176a00394745701ed450a113e2",
        "online":1,
        "owner_address":"0xBdDe0ffED293638De69ABD0fCf42237AD3F2cf94",
        "region":"North Carolina-US",
        "task_types":"[1, 2, 3, 4]",
        "updated_at":1724283128,
        "version":"2.0",
        "worker_address":"0xBdDe0ffED293638De69ABD0fCf42237AD3F2cf94"
      }
    ],
    "jobs":[
      {
        "build_log":"wss://log.pvm.nebulablock.com:8085/api/v1/computing/lagrange/spaces/log?space_id=54d57b7f-4fec-4b7a-9174-8d053e69aa28&type=build",
        "comments":"None",
        "container_log":"wss://log.pvm.nebulablock.com:8085/api/v1/computing/lagrange/spaces/log?space_id=54d57b7f-4fec-4b7a-9174-8d053e69aa28&type=container",
        "cp_account_address":"0xE974b17d9D730CAe75a228Df7eCa452e31E06276",
        "created_at":1724283127,
        "duration":172800,
        "ended_at":"None",
        "hardware":"C1ae.small",
        "id":399,
        "job_real_uri":"https://ds4556zqav.pvm.nebulablock.com",
        "job_result_uri":"None",
        "job_source_uri":"https://swanhub-cali.swanchain.io/spaces/54d57b7f-4fec-4b7a-9174-8d053e69aa28",
        "name":"Job-a888f0ef-6aa4-4b6b-943e-6031823e1b13",
        "node_id":"04da2df41b0bc7804c6fe92205ee00a919412b74cf63647a340baee75e3b89c3ca1cf0b80163c18e36be57885aa7b6af011c813e8ec4b4559a4732293119e6b670",
        "start_at":1724283128,
        "status":"Submitted",
        "storage_source":"swanhub",
        "task_uuid":"7c8510fd-db82-440c-9a3b-708aaa091eb7",
        "type":"None",
        "updated_at":1724283128,
        "uuid":"a888f0ef-6aa4-4b6b-943e-6031823e1b13"
      },
      {
        "build_log":"wss://log.dev2.crosschain.computer:8086/api/v1/computing/lagrange/spaces/log?space_id=54d57b7f-4fec-4b7a-9174-8d053e69aa28&type=build",
        "comments":"None",
        "container_log":"wss://log.dev2.crosschain.computer:8086/api/v1/computing/lagrange/spaces/log?space_id=54d57b7f-4fec-4b7a-9174-8d053e69aa28&type=container",
        "cp_account_address":"0x6f43E3e5B70aa5BF5818c56D509BDd092D0907E0",
        "created_at":1724283128,
        "duration":172800,
        "ended_at":"None",
        "hardware":"C1ae.small",
        "id":400,
        "job_real_uri":"https://qfoqep159o.dev2.crosschain.computer",
        "job_result_uri":"None",
        "job_source_uri":"https://swanhub-cali.swanchain.io/spaces/54d57b7f-4fec-4b7a-9174-8d053e69aa28",
        "name":"Job-2751b4f7-0d16-4aa2-b427-ea7f8a764fba",
        "node_id":"04241e19381a8fad4cc98ef6de0a7e417e6d662ff49d8096cff9ec4b08798eeb96687ff5c7b4bde1adb8ccdbb579f16ac0f2c4e0853406282a37285582879dde49",
        "start_at":1724283128,
        "status":"Submitted",
        "storage_source":"swanhub",
        "task_uuid":"7c8510fd-db82-440c-9a3b-708aaa091eb7",
        "type":"None",
        "updated_at":1724283128,
        "uuid":"2751b4f7-0d16-4aa2-b427-ea7f8a764fba"
      },
      {
        "build_log":"wss://log.cp.filezoo.com.cn:10011/api/v1/computing/lagrange/spaces/log?space_id=54d57b7f-4fec-4b7a-9174-8d053e69aa28&type=build",
        "comments":"None",
        "container_log":"wss://log.cp.filezoo.com.cn:10011/api/v1/computing/lagrange/spaces/log?space_id=54d57b7f-4fec-4b7a-9174-8d053e69aa28&type=container",
        "cp_account_address":"0x2bd6a6f41b37152677F8b4946490580F63494abD",
        "created_at":1724283128,
        "duration":172800,
        "ended_at":"None",
        "hardware":"C1ae.small",
        "id":401,
        "job_real_uri":"https://qxdi11nif0.cp.filezoo.com.cn",
        "job_result_uri":"None",
        "job_source_uri":"https://swanhub-cali.swanchain.io/spaces/54d57b7f-4fec-4b7a-9174-8d053e69aa28",
        "name":"Job-2207f9d3-7392-4060-8246-99941e600607",
        "node_id":"04d5b210591aa5aff5b4e49ad6a3ec57b72aefcdc99cd7888fff80b5991452d8a8dce099312cfb7e78637e04e9824a7274160e49176a00394745701ed450a113e2",
        "start_at":1724283128,
        "status":"Submitted",
        "storage_source":"swanhub",
        "task_uuid":"7c8510fd-db82-440c-9a3b-708aaa091eb7",
        "type":"None",
        "updated_at":1724283128,
        "uuid":"2207f9d3-7392-4060-8246-99941e600607"
      }
    ],
    "task":{
      "comments":"None",
      "created_at":1724283109,
      "end_at":1724455909,
      "id":325,
      "leading_job_id":"a888f0ef-6aa4-4b6b-943e-6031823e1b13",
      "name":"None",
      "refund_amount":"None",
      "refund_wallet":"0xC9180616B2b797385Ad64d09BF8730D74E3b5a41",
      "source":"v2",
      "start_at":1724283109,
      "start_in":300,
      "status":"completed",
      "task_detail":{
        "amount":0.0,
        "bidder_limit":3,
        "created_at":1724283109,
        "dcc_node_job_source_uri":"None",
        "dcc_selected_cp_list":"None",
        "duration":172800,
        "end_at":1724455909,
        "hardware":"C1ae.small",
        "job_result_uri":"None",
        "job_source_uri":"https://swanhub-cali.swanchain.io/spaces/54d57b7f-4fec-4b7a-9174-8d053e69aa28",
        "price_per_hour":"0.0",
        "requirements":{
          "hardware":"None",
          "hardware_type":"CPU",
          "memory":"2",
          "preferred_cp_list":"None",
          "region":"global",
          "storage":"None",
          "update_max_lag":"None",
          "vcpu":"2"
        },
        "space":{
          "activeOrder":{
            "config":{
              "description":"CPU only · 2 vCPU · 2 GiB",
              "hardware":"CPU only",
              "hardware_id":0,
              "hardware_type":"CPU",
              "memory":2,
              "name":"C1ae.small",
              "price_per_hour":0.0,
              "vcpu":2
            }
          },
          "name":"0",
          "uuid":"2439be1a-c5f2-44bf-9b71-1b8bea0fbaeb"
        },
        "start_at":1724283109,
        "status":"paid",
        "storage_source":"swanhub",
        "type":"None",
        "updated_at":1724283109
      },
      "task_detail_cid":"https://plutotest.acl.swanipfs.com/ipfs/QmbwxwFdTCGtZFcg9yxFczji56jp2PLUDViDciDur9vAjr",
      "tx_hash":"None",
      "type":"None",
      "updated_at":1724283132,
      "user_id":2,
      "uuid":"7c8510fd-db82-440c-9a3b-708aaa091eb7"
    }
  },
  "message":"fetch task info for task_uuid='7c8510fd-db82-440c-9a3b-708aaa091eb7' successfully",
  "status":"success"
}

```

</details>

#### 4. Access the Deployed Application

Retrieve the deployed application's URL:

```python
result_url = swan_orchestrator.get_real_url(task_uuid)
print(result_url)
```

Sample output:

```
['https://1cesbzlmow.cp.altafser.com', 'https://7h8g16arc1.1.3242.cn', 'https://v20bdns36f.1.3242.cn']
```

Once the job status is `Running`, you can access the application via the URL in your web browser.

Sample screenshot:

<figure><img src="../../.gitbook/assets/Chainnode.png" alt=""><figcaption></figcaption></figure>