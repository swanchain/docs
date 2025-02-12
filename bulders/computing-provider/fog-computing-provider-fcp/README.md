# Fog Computing Provider(FCP)

Fog Computing Provider(FCP)**:** Offers a layered network that extends cloud capabilities to the edge of the network, providing services such as AI model training and deployment. This provider utilizes infrastructure like Kubernetes (K8S) to support scalable, distributed computing tasks.

**FCP hardware requirements:**

* Possess a public IP
* Have a domain name (\*.example.com)
* Have an SSL certificate
* Have at least one GPU
* At least 8 vCPUs
* Minimum 100GB SSD storage
* Minimum 64GB memory
* Minimum 50MB bandwidth

#### FCP (Fog Computing Provider) Status:

The FCP (Fog Computing Provider) status indicates the current operational state of the provider:

* **Online**: Has an FCP taskType, query is successful, sufficient collateral, and not rejecting tasks (Normal operation)
* **Offline**: Has an FCP taskType, but the query is unsuccessful
* **Version Too Low**: CP version and resource-exporter version need to be upgraded. Current latest versions are CP (v1.1.1) and resource-exporter (v12.0.0)
* **Cheating**: CP resource information collection is incorrect and fails verification
* **Sibyl**: Multiple CPs are running on the same server, indicating Sybil behavior.&#x20;
