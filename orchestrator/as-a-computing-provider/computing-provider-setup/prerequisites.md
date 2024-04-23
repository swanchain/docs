# Prerequisites

Before you install the Computing Provider, you need to know there are some resources required:

* ✅ Possess a public IP
* ✅ Have a domain name (\*.example.com)
* ✅ Have an SSL certificate
* ✅ Have at least one GPU
* ✅ At least 8 vCPUs
* ✅ Minimum 50GB SSD storage
* ✅ Minimum 32GB memory
* ✅ Minimum 50MB bandwidth
* ✅ `Go` version must be 1.21+, you can refer here:

```bash
wget -c https://golang.org/dl/go1.21.7.linux-amd64.tar.gz -O - | sudo tar -xz -C /usr/local

echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.bashrc && source ~/.bashrc
```

###
