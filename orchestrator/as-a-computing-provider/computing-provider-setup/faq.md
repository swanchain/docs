# FAQ

#### Q: What is the current version of Computing Provider, and are there any tutorials?

**A:**

The latest version is [v0.4.1](https://github.com/swanchain/go-computing-provider/releases/tag/v0.4.1)&#x20;

Check the dashboard here : [https://orchestrator.swanchain.io/provider-status](https://orchestrator.swanchain.io/provider-status)

Tutorials&#x20;

* [Deploy your CP](./)
* [Connect to the Orchestrator and Collateral](../connect-to-orchestrator.md)
* [Config and receive the UBI task](config-and-receive-ubi-tasks-optional.md):&#x20;

#### Q: How can I verify if my Computing Provider is set up to receive UBI tasks?

**A**:

1.  Replace the UbiEnginePk in the **`$CP_PATH/config.toml`** file with the `ownerAddress`:

    ```toml
    [UBI]
    UbiEnginePk ="0xxxxx"
    ```
2. Restart `computing-provider`.
3.  Generate the signature using the following command:

    ```bash
    computing-provider wallet sign <ownerAddress> <nodeid+id>
    ```

    For example, if nodeid is `abcd` and taskid is `11`:

    ```bash
    computing-provider wallet sign <ownerAddress> abcd11
    ```
4.  Prepare raw data for the ubi-task test task:

    ```json
    {
        "id": 11,
        "name": "test",
        "type": 1,
        "zk_type": "fil-c2-512M",
        "input_param": "https://286cb2c989.acl.multichain.storage/ipfs/QmYg4CfA5E2zR4ktb5B3PafAeCWyEEXiKUVS4g2UE9occ5",
        "resource": {"cpu": "2", "gpu": "1", "memory": "5.00 GiB", "storage": "1.00 GiB"},
        "signature": "Signing_nodeid_and_taskid_with_ownerAddress"
    }

    ```
5.  Submit the `ubi-task` using the following command(using your public IP and port ):

    ```bash
    curl -k --location --request POST 'https://<public_IP>:<port>/api/v1/computing/cp/ubi' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "id": 11,
        "name": "test-ubi",
        "type": 1,
        "zk_type": "fil-c2-512M",
        "input_param": "https://286cb2c989.acl.multichain.storage/ipfs/QmYg4CfA5E2zR4ktb5B3PafAeCWyEEXiKUVS4g2UE9occ5",
        "resource": {"cpu": "2", "gpu": "1", "memory": "5.00 GiB", "storage": "1.00 GiB"},
        "signature": "Signing_nodeid_and_taskid_using_the_owner's_address."
    }'
    ```
6.  After running ubi-task, check if the task status is success:

    ```bash
    computing-provider ubi-task list
    ```
7. If the test is successful, restore the `UbiEnginePk` in the `config.toml` file to its original value

#### Q: How can I know if the status of the computing provider is normal?

**A**:&#x20;

Run the following command:

{% hint style="info" %}
_**\*Note: Please replace**** ****`<YOUR_MULTI_ADDRESS_IP>:<PORT>`**** ****with your actual multi-address IP and port.**_
{% endhint %}

```
curl --location --request POST 'http://<YOUR_MULTI_ADDRESS_IP>:<PORT>/api/v1/computing/lagrange/jobs' \
--header 'Content-Type: application/json' \
--data-raw '{
"uuid": "5641877b-dc94-469a-bb3b-ecab6d10f7dd",
"name": "Job-5641877b-dc94-469a-bb3b-ecab6d10f7dd",
"status": "Submitted",
"duration": 900,
"job_source_uri": "https://api.lagrangedao.org/spaces/51d6abbb-f928-43e4-91fd-79e93e2b276f",
"storage_source": "lagrange",
"task_uuid": "92cd5595-9789-4af3-9100-7c7e4aacb456"
}'
```

After running this command, wait for 3-5 minutes, and then execute&#x20;

<pre><code><strong>kubectl get ing -n ns-0x6091b2f5678952cafbf02755d78973ebff302e11
</strong></code></pre>

Find the hosts corresponding to the name `ing-minesweeper` and ensure that the domain can be accessed in a browser to confirm its normal status.

<img src="broken-reference" alt="" data-size="original">

#### Q: My node has been running for so long, yet the uptime is 0%.

**A**:

1\. Run the following command:

{% hint style="info" %}
_**Ensure that****  ****`<YOUR_MULTI_ADDRESS_IP>`**** ****is the Public IP.**_
{% endhint %}

```bash
curl -k https://<YOUR_MULTI_ADDRESS_IP>:<PORT>/api/v1/computing/host/info
```

2\. Compare the returned result with the example provided below. If they are different, you should review your port mappings.

Example result:

```json
{"status":"success","code":"","data":{"swan_miner_version":"","operating_system":"linux","architecture":"amd64","cpu_cores":48}}
```

3\. If your port mappings are correct and the result matches the example, then proceed to check the configuration file of the computing provider.&#x20;

Ensure that the `MultiAddress` is set exactly as `"/ip4/<public_ip>/tcp/<port>"`.

#### Q: Which ports need to be mapped?&#x20;

**A**: Here are the ports you need to map

1\. You need to map the CP's internal IP and its port (default 8085), as well as the public IP and port.

2\. Map your wildcard domain (\*.example.com) to your public IP.&#x20;

3\. Additionally, you need to map port 80 of your internal IP to port 80 of your public IP, as well as port 443 of your internal IP to port 443 of your public IP.

#### Q: Where should I create the API key?

**A**: you must use the API of [https://multichain.storage,](https://multichain.storage,) and login in it using Polygon mainnet wallet.

#### Q: What are the requirements for SSL certificates needed in CP?&#x20;

**A:** Please use certificates issued by trusted Certificate Authorities (CA). Currently, certificates generated by Certbot are not functioning properly.&#x20;

Otherwise, the application won't be displayed correctly on the Space App page.

#### **Q: Is it possible to use a port other than 80 and 443 in the wildcard domain(\*.exmaple.com)?**

**A**: No, it is not possible.

#### Q: Is the "`pod"` used for communication, and "`Calico`" is used to manage this communication within the cluster?&#x20;

**A**: Both are used for intra-cluster communication. You can use one of these approaches.

#### Q: If someone didn't apply for early bird, can they still join and run the computing provider tasks?

**A**: Of course, they can also follow the [instruction](broken-reference) to set up a Computing Provider.

#### Q: Can I move my computing provider to a new one while maintaining my previous server? Will this reset my uptime?

**A**: Yes, you need to move  `.swan_node` to the new server. The uptime will not be reset.
