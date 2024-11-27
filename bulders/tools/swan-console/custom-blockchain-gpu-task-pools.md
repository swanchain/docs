# Custom Blockchain GPU Task Pools

In this section, we will guide you through the process of setting up custom Blockchain GPU task pools on Swan Chain. This feature allows you to deploy your preferred Blockchain GPU task pool configurations with the same seamless experience as our official pools.



### 1. Accessing the Marketplace <a href="#id-01f0" id="id-01f0"></a>

The Swan Chain marketplace provides access to various Blockchain GPU task pools. To begin:

* Select “Marketplace” from the left panel
* Navigate to the Mining section
* click the "+" icon

<figure><img src="../../../.gitbook/assets/image (195).png" alt=""><figcaption></figcaption></figure>

### 2. Configuration Settings

#### Basic Configuration

1. Create a unique instance name
   * No specific naming rules apply
   * Choose a name that helps you identify this deployment

#### Docker Image Setup

1. In the Docker Image/OS field, enter your Blockchain GPU task container's Docker image
2. Configure essential environment variables, which typically include:

```
MINER_URL: [Your pool's stratum URL]
ACCOUNTNAME: [Your registered pool account name]
WORKERNAME: [A unique identifier for this worker]
```

> **Important**: Ensure your Docker image is properly configured and tested before deployment. Incorrect configurations may result in Blockchain GPU task interruptions.



<figure><img src="../../../.gitbook/assets/image (196).png" alt=""><figcaption></figcaption></figure>

## 3. Provider Selection <a href="#a831" id="a831"></a>

After setting up your environment variables, you’ll need to choose the hardware specifications for your deploy operation.

<figure><img src="https://docs.swanchain.io/~gitbook/image?url=https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A700%2F0*zc0_x7GTIc6XlZa6&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=58e63629&#x26;sv=1" alt=""><figcaption></figcaption></figure>

Swan Chain offers two distinct approaches for selecting a Computing Provider (CP):

1. **Automatic Matching**

* The system automatically matches you with suitable Computing Providers
* Recommended for new users

**2. Manual Provider Selection**

* Filter providers based on Geographic location, Hardware pricing and Provider account ID
* **PRO TIP**: Check provider performance metrics via their account ID in the provider dashboard
* **NOTE**: This option gives you more control but requires understanding of provider metrics

<figure><img src="https://docs.swanchain.io/~gitbook/image?url=https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A700%2F0*_FJH5EGnuiluoYgc&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=3ff7fe95&#x26;sv=1" alt=""><figcaption></figcaption></figure>

### 4. Final Deployment <a href="#ab5d" id="ab5d"></a>

* Review all configurations thoroughly
* Click “Deploy Now” to initiate the Blockchain GPU task

**Note:** If you see the “Not sufficient balance” notification, please follow [this guide](getting-started.md) to manage your accounts and transfer SWANU.

<figure><img src="https://docs.swanchain.io/~gitbook/image?url=https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A700%2F0*DuZLFLjsRD5tXhUm&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=f6f9d373&#x26;sv=1" alt=""><figcaption></figcaption></figure>

***

### Contributing to Official Blockchain GPU task Pools

Want to see your Docker configuration added to our official Blockchain GPU task pools? We welcome community contributions!

1. Visit our GitHub repository: [awesome-swanchain](https://github.com/swanchain/awesome-swanchain/tree/main)
2. Follow the contribution guidelines
3. Submit a Pull Request with your Blockchain GPU task pool configuration

