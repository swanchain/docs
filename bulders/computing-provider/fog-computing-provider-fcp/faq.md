# FCP FAQ

#### Q: What is the current version of Computing Provider, and are there any tutorials?

**A:**

The latest version is v1.0.1: [h](https://github.com/swanchain/go-computing-provider/releases/tag/v1.0.0)[https://github.com/swanchain/go-computing-provider/releases/tag/v1.0.1](https://github.com/swanchain/go-computing-provider/releases/tag/v1.0.1)

Check the dashboard here : [https://orchestrator.swanchain.io/provider-status](https://orchestrator.swanchain.io/provider-status)

Tutorials&#x20;

* [Deploy your ECP](../edge-computing-provider-ecp/ecp-setup.md)
* [Depoy your FCP](computing-provider-setup.md)



**Q: How can I verify whether the FCP environment is installed successfully?**

```
curl -k https://<PUBLIC_IP>:<PORT>/api/v1/computing/cp
```

out:

```

{
    "node_id": "04f4af161995c1cc0f8a4c1dd316a64b809c879e1cc7",
    "region": "North Carolina-US",
    "cluster_info": [
        {
            "machine_id": "4c4c4544-0046-3510-8058-b1c04f504433",
            "cpu_name": "AMD",
            "cpu": {
                "total": "192",
                "used": "16",
                "free": "176"
            },
            "memory": {
                "total": "2003 GiB",
                "used": "62 GiB",
                "free": "1822 GiB"
            },
            "gpu": {
                "driver_version": "535.104.05",
                "cuda_version": "12020",
                "attached_gpus": 1,
                "details": [
                    {
                        "product_name": "NVIDIA 3080",
                        "status": "available",
                        "fb_memory_usage": {
                            "total": "10240 MiB",
                            "used": "235 MiB",
                            "free": "10004 MiB"
                        },
                        "bar1_memory_usage": {
                            "total": "256 MiB",
                            "used": "2 MiB",
                            "free": "253 MiB"
                        }
                    }
                ]
            },
            "storage": {
                "total": "877 GiB",
                "used": "456 GiB",
                "free": "376 GiB"
            }
        }
    ],
    "multi_address": "/ip4/provider.cp.cn/tcp/9085",
    "node_name": "fcp-001"
}
```



#### Q: How can I know if the status of the computing provider is normal?

**A**:&#x20;

Set the `[HUB].VerifySign` in the **`$CP_PATH/config.toml`** file&#x20;

```
[HUB]
VerifySign = false
```

Run the following command:

{% hint style="info" %}
_**\*Note: Please replace****&#x20;****`<YOUR_MULTI_ADDRESS_IP>:<PORT>`****&#x20;****with your actual multi-address IP and port.**_
{% endhint %}

```
curl -k --location --request POST 'https://<YOUR_MULTI_ADDRESS_IP>:<PORT>/api/v1/computing/lagrange/jobs' \
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



**Q: How can I verify if my Computing Provider is set up to receive UBI tasks?**

**A**:

1.  Replace the UbiEnginePk in the **`$CP_PATH/config.toml`** file with the `ownerAddress`:

    Copy

    ```
    [UBI]
    UbiEnginePk ="0xxxxx"
    ```
2. Restart `computing-provider`.
3.  Generate the signature using the following command:

    Copy

    ```
    computing-provider wallet sign <ownerAddress> <nodeid+contract_addr>
    ```

    For example, if nodeid is abcd and task contract\_addr is 11:

    Copy

    ```
    computing-provider wallet sign <ownerAddress> abcd11
    ```
4.  Prepare raw data for the ubi-task test task:

    Copy

    ```
    {

    "id": 26384,
    "name": "1000-0-7-26381",
    "type": 1,
    "resource_type": 0,
    "deadline": 2000000,
    "check_code": "check_code-demo",
    "input_param": "https://286cb2c989.acl.swanipfs.com/ipfs/QmTgoX6LkzZTsTjSjXvujzgJEHBLTEg3KMUadQGnyTrNFG",
    "verify_param": "https://286cb2c989.acl.swanipfs.com/ipfs/QmTgoX6LkzZTsTjSjXvujzgJEHBLTEg3KMUadQGnyTrNFG.verify",
    "resource":{"cpu":"10","memory":"5.00 GiB","storage":"10.00 GiB"}
    "signature":"Signing_cpAccountAddress_and_id_with_ownerAddress"

    }
    ```
5.  Submit the `ubi-task` using the following command(using your public IP and port ):

    Copy

    ```
    curl --location --request POST 'http://<public_IP>:<port>/api/v1/computing/cp/ubi' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "id": 26384,
        "name": "1000-0-7-26381",
        "type": 1,
        "resource_type": 0,
        "deadline": 2000000,
        "check_code": "check_code-demo",
        "input_param": "https://286cb2c989.acl.swanipfs.com/ipfs/QmTgoX6LkzZTsTjSjXvujzgJEHBLTEg3KMUadQGnyTrNFG",
        "verify_param": "https://286cb2c989.acl.swanipfs.com/ipfs/QmTgoX6LkzZTsTjSjXvujzgJEHBLTEg3KMUadQGnyTrNFG.verify",
        "resource":{"cpu":"10","memory":"5.00 GiB","storage":"10.00 GiB"}
        "signature":"Signing_cpAccountAddress_and_id_with_ownerAddress"
    }'
    ```
6.  After running ubi-task, check if the task status is success:

    Copy

    ```
    computing-provider ubi list
    ```
7. If the test is successful, restore the `UbiEnginePk` in the `config.toml` file to its original value\


#### Q: How to upgrade resource-exporter in FCP

**A**:&#x20;

1. Stop the computing-provider service.
2. Delete component `resource-exporter` and `filswan/resource-exporter` local image

```
kubectl delete ds -n kube-system resource-exporter-ds

docker rmi -f filswan/resource-exporter:v11.3.0
```

3. Reinstall `resource-exporter` by the command:

```
cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: DaemonSet
metadata:
 namespace: kube-system
 name: resource-exporter-ds
 labels:
   app: resource-exporter
spec:
 selector:
   matchLabels:
     app: resource-exporter
 template:
   metadata:
     labels:
       app: resource-exporter
   spec:
     containers:
     - name: resource-exporter
       image: filswan/resource-exporter:v11.3.1
       imagePullPolicy: IfNotPresent
EOF
```

4. Check the `resource-exporter` version:

```
kubectl describe po -n kube-system resource-exporter-ds |grep "Image:"
```

It should be:

```
Image:          filswan/resource-exporter:v11.3.1
```

and ensure it is running by `kubectl get po -n kube-system`

<figure><img src="../../../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

5. Start `computing-provider` with v1.0.1: [https://github.com/swanchain/go-computing-provider/releases/tag/v1.0.1](https://github.com/swanchain/go-computing-provider/releases/tag/v1.0.1)

#### Q: How to upgrade resource-exporter in ECP

**A**:&#x20;

1. Delete container `resource-exporter` and `filswan/resource-exporter` local image

```
docker rm -f resource-exporter
docker rmi -f filswan/resource-exporter:v11.3.0
```

2. CP will automatically pull the image and run the resource-exporter container

#### Q: Which ports need to be mapped?&#x20;

**A**: Here are the ports you need to map

1\. You need to map the CP's internal IP and its port (default 8085), as well as the public IP and port.

2\. Map your wildcard domain (\*.example.com) to your public IP.&#x20;

3\. Additionally, you need to map port 80 of your internal IP to port 80 of your public IP, as well as port 443 of your internal IP to port 443 of your public IP.

####

#### Q: Where should I create the API key?

**A**: you must use the API of [https://multichain.storage,](https://multichain.storage,) and login in it using Polygon mainnet wallet.

####

#### Q: What are the requirements for SSL certificates needed in CP?&#x20;

**A:** Please use certificates issued by trusted Certificate Authorities (CA). Currently, certificates generated by Certbot are not functioning properly.&#x20;

Otherwise, the application won't be displayed correctly on the Space App page.

####

#### **Q: Is it possible to use a port other than 80 and 443 in the wildcard domain(\*.exmaple.com)?**

**A**: No, it is not possible.

####

#### Q: Is the "`pod"` used for communication, and "`Calico`" is used to manage this communication within the cluster?&#x20;

**A**: Both are used for intra-cluster communication. You can use one of these approaches.

####

#### Q: If someone didn't apply for early bird, can they still join and run the computing provider tasks?

**A**: Of course, they can also follow the [instruction](broken-reference) to set up a Computing Provider.

####

#### Q: Can I move my computing provider to a new one while maintaining my previous server? Will this reset my uptime?

**A**: Yes, you need to move  `.swan_node` to the new server. The uptime will not be reset.

####

#### **Q: How can I migrate my CP (Computing Provider) to a new environment?**

**A:** To migrate your CP to a new environment, follow these steps:

1. **Backup CP Repo**: Copy all files under the old CP directory (`$CP_PATH`) to a directory on the new server, for example: `/data/swan`.
2. **Set Environment Variable**: Set the environment variable `CP_PATH` to the directory where you copied the CP files on the new server. For example, you can do this by running the command: `export CP_PATH=/data/swan`.
3. **Start CP Service**: Once the files are copied and the environment variable is set, start the CP service process on the new server.

By following these steps, you'll successfully migrate your CP to the new environment, ensuring that it operates smoothly in the new environment.



**Q: How can I resolve the error `(error) MISCONF Redis is configured to save RDB snapshots`seen in the FCP logs?**

**A:** MISCONF Redis is configured to save RDB snapshots but is currently not able to persist on disk, Commands that may modify the data set are disabled.&#x20;

You can try to change the setting:

```
$ redis-cli
> config set stop-writes-on-bgsave-error no
```



#### Q: **How do I withdraw collateral from FCP?**

To withdraw collateral from FCP, use the following command:

```bash
computing-provider --repo <YOUR_CP_PATH> collateral withdraw --fcp --owner <YOUR_OWNER_WALLET_ADDRESS> --account <YOUR_CP_ACCOUNT> <AMOUNT>
```



#### Q: How to Upgrade Resource-Exporter in FCP

**A:**

**Step 1:** Stop the computing-provider service.

**Step 2:** Delete the current `resource-exporter` component:

```sh
kubectl delete ds -n kube-system resource-exporter-ds
```

**Step 3:** Reinstall the `resource-exporter` by using the following command:

```sh
cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: DaemonSet
metadata:
  namespace: kube-system
  name: resource-exporter-ds
  labels:
    app: resource-exporter
spec:
  selector:
    matchLabels:
      app: resource-exporter
  template:
    metadata:
      labels:
       app: resource-exporter
    spec:
      containers:
      - name: resource-exporter
        image: filswan/resource-exporter:v11.2.9
        imagePullPolicy: IfNotPresent
        securityContext:
          privileged: true
EOF
```

**Step 4:** Check the version of the `resource-exporter`:

```sh
kubectl describe po -n kube-system resource-exporter-ds | grep "Image:"
```

It should display:

```
Image:          filswan/resource-exporter:v11.2.9
```

Ensure the `resource-exporter` is running by executing:

```sh
kubectl get po -n kube-system
```

**Step 5:** Start the computing-provider with the latest version.
