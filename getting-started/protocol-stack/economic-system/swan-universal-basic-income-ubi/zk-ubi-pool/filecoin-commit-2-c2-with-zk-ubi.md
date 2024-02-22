# Filecoin Commit 2 (C2) with ZK-UBI

[Commit 2](https://docs.filecoin.io/storage-providers/architecture/sealing-pipeline#commit-2) (C2) is an intensive computational step in Filecoin's sector sealing pipeline requiring generation of a [zk-SNARK](https://docs.filecoin.io/reference/general/glossary#zero-knowledge-succinct-non-interactive-argument-of-knowledge-zk-snark) proof to validate storage commitment.

Swan's ZK-UBI mechanism incorporates such heavy workloads into an incentive design that allows [Computing Provider](../../../../../orchestrator/as-a-computing-provider/computing-provider-setup/) within Swan ecosystem to accelerate proofs while earning ZK-UBI rewards.

**Key Points about C2**

* C2 involves creating a zk-SNARK proof to validate the miner’s commitment to store sector data.
* It is a GPU-intensive computation, requiring significant compute resources (peak memory of 275GiB for 32GiB sectors).

**Receiving ZK-Tasks**

Computing providers on Swan can following this [guide](../../../../../orchestrator/as-a-computing-provider/computing-provider-setup/config-and-receive-ubi-tasks-optional.md) to configure themselves to receive and process outsourced Filecoin C2 sector sealing tasks.

Once set up, suitable C2 tasks get automatically assigned based on provider declared capacity.

**Contributing ZK-proof Workloads**

For Storage Providers interested in contributing tasks to this pool, please refer to this page: Contribute to ZK-UBI Task

If you have an independent zero-knowledge proof system that requires robust computational support from the Swan Network, you can directly apply for our DevGrant [here](https://github.com/swanchain/devgrants/issues/new/choose).\
