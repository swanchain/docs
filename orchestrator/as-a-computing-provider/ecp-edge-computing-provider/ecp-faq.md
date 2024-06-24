# ECP FAQ

#### Q: How can I verify whether the ECP environment is installed successfully?

```bash
curl http://<PUBLIC_IP>:<PORT>/api/v1/computing/cp
```

out:

```json

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
    "node_name": "ecp-001"
}
```



#### **Q: What are `ownerAddress`, `workerAddress`, and `beneficiaryAddress`?**

**A:** These are three different types of accounts used in the system for security and separation of concerns:

* `ownerAddress`: This is the owner account of the CP account. The owner has permission to change account information such as the multi-address, worker address, and beneficiary address. In most situations, the private key of the `ownerAddress` does not need to be present on the server for security reasons.
* `workerAddress`: This is the actual working address used for submitting proofs (`submitProof`). It needs to be funded with a certain amount of sETH to pay for gas fees when submitting proofs.
* `beneficiaryAddress`: This is the address where all earnings from the CP account will be sent. It is solely used for receiving funds. For security purposes, the private key of the `beneficiaryAddress` should not be stored on the server to maintain isolation.

By separating these accounts, the system ensures that only the necessary `workerAddress` private key is present on the server, while the more sensitive `ownerAddress` and `beneficiaryAddress` private keys are kept separate, enhancing the overall security of the system.

#### **Q: How do I update the resource-exporter in ECP?**

**A:**&#x20;

Step 1: Stop the computing-provider process.

Step 2: Execute the following commands:

```bash
docker rm -f resource-exporter
docker rmi filswan/hardware-exporter:v2.0
```

Step 3: Restart the computing-provider process.

Step 4: Check resource-exporter logs:

Verify that the resource-exporter is functioning correctly by monitoring its logs:

```bash
docker logs -f resource-exporter
```

#### **Q: How do I withdraw collateral from ECP?**

To withdraw collateral from ECP, use the following command:

```bash
computing-provider --repo <YOUR_CP_PATH> collateral withdraw --ecp --owner <YOUR_OWNER_WALLET_ADDRESS> <AMOUNT>
```
