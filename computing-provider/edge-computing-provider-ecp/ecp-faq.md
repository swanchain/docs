# ECP FAQ

### Q: How can I verify whether the ECP environment is installed successfully?

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



### **Q: What are `ownerAddress`, `workerAddress`, and `beneficiaryAddress`?**

**A:** These are three different types of accounts used in the system for security and separation of concerns:

* `ownerAddress`: This is the owner account of the CP account. The owner has permission to change account information such as the multi-address, worker address, and beneficiary address. In most situations, the private key of the `ownerAddress` does not need to be present on the server for security reasons.
* `workerAddress`: This is the actual working address used for submitting proofs (`submitProof`). It needs to be funded with a certain amount of ETH to pay for gas fees when submitting proofs.
* `beneficiaryAddress`: This is the address where all earnings from the CP account will be sent. It is solely used for receiving funds. For security purposes, the private key of the `beneficiaryAddress` should not be stored on the server to maintain isolation.

By separating these accounts, the system ensures that only the necessary `workerAddress` private key is present on the server, while the more sensitive `ownerAddress` and `beneficiaryAddress` private keys are kept separate, enhancing the overall security of the system.



### Q: How to upgrade resource-exporter in ECP

**A**:

1. Delete container `resource-exporter` and `filswan/resource-exporter` local image

```
docker rm -f resource-exporter
docker rmi -f filswan/resource-exporter:v11.3.2
```

2. CP will automatically pull the image and run the resource-exporter container



### **Q: How to verify if ECP is running properly?**

* **Step 1: Check Collateral**:

```
computing-provider --repo info
```

[![image](https://private-user-images.githubusercontent.com/102578774/403743479-92e14ebd-cb46-4b3e-98f7-aa50ad544f0c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzczNjU4NDUsIm5iZiI6MTczNzM2NTU0NSwicGF0aCI6Ii8xMDI1Nzg3NzQvNDAzNzQzNDc5LTkyZTE0ZWJkLWNiNDYtNGIzZS05OGY3LWFhNTBhZDU0NGYwYy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMTIwJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDEyMFQwOTMyMjVaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT04ZmE5N2MyYzU0MTZiN2ViZmQ2ZDM4NTNiNmUxYmU2MWFlMWQyMDcyYzhlMDFmZTE5MTQ3MzEzZTU4M2VhMjA1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.3Fa97VNSN0WgnTFigMG0RbFtWEGOx_xmvisfuTnIkJM)](https://private-user-images.githubusercontent.com/102578774/403743479-92e14ebd-cb46-4b3e-98f7-aa50ad544f0c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzczNjU4NDUsIm5iZiI6MTczNzM2NTU0NSwicGF0aCI6Ii8xMDI1Nzg3NzQvNDAzNzQzNDc5LTkyZTE0ZWJkLWNiNDYtNGIzZS05OGY3LWFhNTBhZDU0NGYwYy5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMTIwJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDEyMFQwOTMyMjVaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT04ZmE5N2MyYzU0MTZiN2ViZmQ2ZDM4NTNiNmUxYmU2MWFlMWQyMDcyYzhlMDFmZTE5MTQ3MzEzZTU4M2VhMjA1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.3Fa97VNSN0WgnTFigMG0RbFtWEGOx_xmvisfuTnIkJM)

Ensure that the sum of `Collateral` and `Escrow` under the `ECP Balance` is greater than the calculated collateral amount for proper staking.

* **Step 2:Check Versions**:
  * **CP Version**:

```
computing-provider -v
```

Verify the CP version by checking the commit number in the official [go-computing-provider](https://github.com/swanchain/go-computing-provider) repository to confirm if your version is up-to-date or the recommended stable version.

* **Resource-Exporter Version**:

```
kubectl get daemonset resource-exporter-ds -n kube-system -o yaml | grep image:
```

Verify the version of `resource-exporter` by checking the required version in the [official repository](https://github.com/swanchain/go-computing-provider/tree/releases?tab=readme-ov-file#Install-the-Hardware-resource-exporter).

* **Step 3: Check Resource Endpoint**:

```
curl http://<cp-ip>:<port>/api/v1/computing/cp
```

> **Note**: Replace `<cp-ip>` and `<port>` with the corresponding information from your `CP_REPO` configuration file, where `MultiAddress = "/ip4/<ip>/tcp/<port>"`.

* **Step 4: Self-check by submitting a job**:

```
curl --location --request POST 'http://<cp-ip>:<port>/api/v1/computing/cp/ubi' \
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

* **Dashboard Status Check**:\
  In the [Provider Dashboard](https://provider.swanchain.io/), locate your CP using the `cpAccount`. Check that the status is `Online`, and ensure collateral is sufficient (as shown in the example). If so, your CP is running properly.

[![image](https://private-user-images.githubusercontent.com/102578774/403743659-087a96f3-a1c3-4607-b802-b343a51246e6.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzczNjU4NDUsIm5iZiI6MTczNzM2NTU0NSwicGF0aCI6Ii8xMDI1Nzg3NzQvNDAzNzQzNjU5LTA4N2E5NmYzLWExYzMtNDYwNy1iODAyLWIzNDNhNTEyNDZlNi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMTIwJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDEyMFQwOTMyMjVaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05ZjZkZWU2NjM4Mjc2NDg0NmU4OWFjYjM4N2Y3YzBmMzM1NGM2MGMxMThhNzA5NDhhNGVjNWNmZjVjZjMyN2NhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.Op8W10YxMo1lnZ-x1-NFKcQZ-dnSlFFEF1obDO4Z-k8)](https://private-user-images.githubusercontent.com/102578774/403743659-087a96f3-a1c3-4607-b802-b343a51246e6.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzczNjU4NDUsIm5iZiI6MTczNzM2NTU0NSwicGF0aCI6Ii8xMDI1Nzg3NzQvNDAzNzQzNjU5LTA4N2E5NmYzLWExYzMtNDYwNy1iODAyLWIzNDNhNTEyNDZlNi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMTIwJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDEyMFQwOTMyMjVaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05ZjZkZWU2NjM4Mjc2NDg0NmU4OWFjYjM4N2Y3YzBmMzM1NGM2MGMxMThhNzA5NDhhNGVjNWNmZjVjZjMyN2NhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.Op8W10YxMo1lnZ-x1-NFKcQZ-dnSlFFEF1obDO4Z-k8)



### **Q: How do I withdraw collateral from ECP?**

To withdraw collateral from ECP, use the following command:

```bash
computing-provider --repo <YOUR_CP_PATH> collateral withdraw --ecp --owner <YOUR_OWNER_WALLET_ADDRESS> --account <YOUR_CP_ACCOUNT> <AMOUNT>
```

