# ECP FAQ

#### Q: How can I verify whether the ECP environment is installed successfully?

```bash
curl -k --location --request GET 'http://<PublicIp>:<port>/api/v1/computing/cp' \
--header 'Content-Type: application/json'

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
