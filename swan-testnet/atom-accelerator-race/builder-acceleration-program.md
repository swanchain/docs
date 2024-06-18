# Builder Acceleration Program

The Builder Acceleration Program is an exciting opportunity for developers and builders to contribute to the growth of the Swan Chain ecosystem. This program aims to attract more AI models and Zero-Knowledge (ZK) applications, enriching the network's capabilities and fostering innovation.

### Duration

April 29th , 00:00 (EST) - June 29th, 2024, 23:59 (EST)

### How to Participate

To participate in the Builder Acceleration Program, follow these steps:

1. **Build Lagrange LDL Configs or Dockerfiles**: Participants will be required to build [Lagrange LDL configs](https://docs.lagrangedao.org/spaces/intro/lagrange-definition-language-ldl) (and container images/Dockerfiles if needed) for specific AI models or ZK applications.
2. **Contribute to the Open-Source Repository**: Commit your Lagrange LDL configs or Dockerfiles to the open-source code repository hosted on GitHub [here](https://github.com/swanchain/awesome-swanchain). Check [Submission Guidelines here](builder-acceleration-program.md#submission-guidelines).
3. **Deploy and Run**: participants are required to deploy their Lagrange LDL configs or Dockerfiles in Lagrange, follow this [video tutorial](https://www.youtube.com/watch?v=xDHNHbIxDYY) or the [documentation](https://docs.lagrangedao.org/spaces) and ensure they are running correctly.
4. **Fill Out the Form:** Remember to fill out this [form](https://docs.google.com/forms/d/e/1FAIpQLSf93f3COyR4OS0256HqK4DJ3j6Rb2eX61XcfcGlo7\_AcEJtVw/viewform?usp=sf\_link) when you finish this task. If you are contributing multiple AI models, please submit this form separately for each AI model.

{% hint style="info" %}
**Examples of LDL Configs**

Participants can build LDL configs for various AI models and ZK applications, including but not limited to:

* **AI Models**: [Stable Diffusion (SD)](https://lagrangedao.org/spaces/0x231fe9090f4d45413474BDE53a1a0A3Bd5C0ef03/Stable-Diffusion-Base-LoRA/app), [MusicGen](https://lagrangedao.org/spaces/0x231fe9090f4d45413474BDE53a1a0A3Bd5C0ef03/MusicGen/app), and other popular AI models.
* **ZK Applications**: Filecoin C2, Aleo, and other Zero-Knowledge applications.
{% endhint %}

### Rules and Eligibility

* The program is open to developers and builders globally.
* Participants must comply with all applicable laws and regulations in their respective jurisdictions.
* Swan Chain reserves the right to modify or terminate the program at any time, with or without notice.
* Rewards will be distributed in the form of credits.

### Submission Guidelines

**1. Create a New Branch**

* For every contribution of LDL configs (and container images, if needed) for specific AI models or applications, please create a new branch.
* Name the new branch after the AI model you are contributing.

Example:

```bash
git checkout -b <AI_model_name>
```

{% hint style="info" %}
**Note:** If you are contributing multiple AI models, please submit separate Pull Requests (PRs) for each model to facilitate team review.
{% endhint %}

**2. Contributing Files**

* Your contribution should include LDLs and a README file. Chek additional documentation on [how to build LDLs here](https://docs.lagrangedao.org/spaces/intro/lagrange-definition-language-ldl).
* The README must contain a screenshot confirming the deployment of the model on any available GPU providers in Lagrange. The screenshot should display the wallet address and space link.

Example LDL yaml file:

```
version: "2.0"

services:
  db:
    image: postgres:11.6-alpine
    env:
      - POSTGRES_USER=codimd
      - POSTGRES_PASSWORD=rootadmin
      - POSTGRES_DB=codimd
    expose:
        - port: 5432
          as: 5432
          to:
            - service: db
    ready-cmd:
        - "psql"
        - "-w"
        - "-U"
        - "codimd"
        - "-d"
        - "codimd"
        - "-c"
        - "SELECT 1"
  codimd:
    image: hackmdio/hackmd:2.4.1
    env:
      - CMD_DB_URL=postgres://codimd:rootadmin@127.0.0.1:5432/codimd
      - CMD_USECDN=false
    depends-on:
      - db
    expose:
        - port: 3000
          as: 3000
          to:
            - global: true

deployment:
  db:
    lagrange:
      count: 1
  codimd:
    lagrange:
      count: 1
```

Example README file:

```markdown
# <AI Model Name> LDL

## Intro about the AI Model

## Deployment Confirmation
![Deployment Screenshot](path/to/screenshot.png)

- **Wallet Address**: 0x1234abcd...
- **Space Link**: [Lagrange Space](https://lagrange.space/link-to-deployment)
```

**3. Update the README Directory**

* Update the directory in the README file [here](https://github.com/swanchain/awesome-swanchain/blob/main/README.md).
* Add your contributed files to the appropriate category:
  * AI-CPU
  * AI-GPU
  * Zero-knowledge Service
  * Tools
  * DeFi
  * Game-CPU
  * Game-GPU
* If your contributed model does not fit any of the existing categories, you may create a new one.

Example update to README:

```markdown
## AI-GPU

- [AI Model Name](path/to/your/LDL/yaml)
```

**4. Fill Out the Form**

* Remember to fill out this [form](https://docs.google.com/forms/d/e/1FAIpQLSf93f3COyR4OS0256HqK4DJ3j6Rb2eX61XcfcGlo7\_AcEJtVw/viewform?usp=sf\_link).

{% hint style="info" %}
**Note:** If you are contributing multiple AI models, please submit this form multiple times, once for each AI model.
{% endhint %}
