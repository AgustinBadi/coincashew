---
description: >-
  Become a validator and help secure eth2, a proof-of-stake blockchain. Anyone
  with 32 ETH can join.
---

# Guide \| How to setup a validator on ETH2 testnet PRATER

{% hint style="info" %}
🎊 **2021-09 Gitcoin Grant Round 11:** We improve this guide with your support! 

[Help fund us and earn a **POAP NFT**](https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew). Appreciate your support!🙏 
{% endhint %}

{% embed url="https://gitcoin.co/grants/1653/ethereum-staking-guides-by-coincashew-with-poap" %}

{% hint style="success" %}
As of Sept 26 2021, this guide is updated for **testnet PRATER.**

If you wish to test on **testnet PYRMONT**, [please click here](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-testnet).
{% endhint %}

{% hint style="info" %}
#### ⏩ [Mainnet guide](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet). Always test and practice on testnet first. 
{% endhint %}

### 📄 Changelog - **Update Notes -** **Sept 26 2021**

* Added erigon build dependencies.
* Added **Teku** Checkpoint Sync feature, the quickest way to sync a Ethereum beacon chain client.
* geth + erigon pruning / Altair hard fork changes / nimbus eth1 fallback
* lighthouse + prysm doppelganger protection enabled. Doppelganger protection intentionally misses an epoch on startup and listens for attestations to make sure your keys are not still running on the old validator client.
* OpenEthereum will no longer be supported post London hard fork. Gnosis, maintainers of OpenEthereum, suggest users migrate to their new **Erigon** Ethererum client. Added setup instructions for **Erigon** under eth1 node section.
* Added [Mobile App Node Monitoring by beaconcha.in](guide-or-how-to-setup-a-validator-on-eth2-mainnet/#6-5-mobile-app-node-monitoring-by-beaconcha-in)
* Updated [eth2.0-deposit-cli to v.1.2.0](https://github.com/ethereum/eth2.0-deposit-cli/releases/tag/v1.2.0) and added section on eth1 withdrawal address
* Added generating mnemonic seeds on **Tails OS** by [punggolzenith](https://github.com/punggolzenith)
* Iancoleman.io BLS12-381 Key Generation Tool [how-to added](guide-or-how-to-setup-a-validator-on-eth2-mainnet/#8-12-eip2333-key-generator-by-iancoleman-io)
* Testnet guide forked for [Prater testnet](guide-or-how-to-setup-a-validator-on-eth2-testnet-prater.md) staking
* [Geth pruning guide](guide-or-how-to-setup-a-validator-on-eth2-mainnet/how-to-free-up-eth1-node-disk-space.md) created
* Major changes to Lodestar guide
* Additional [Grafana Dashboards](guide-or-how-to-setup-a-validator-on-eth2-mainnet/#6-2-setting-up-grafana-dashboards) for Prysm, Lighthouse and Nimbus
* [Validator Security Best Practices added](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md)
* Translations now available for Japanese, Chinese and Spanish \(access by changing site language\)
* Generate keystore files on [Ledger Nano X, Nano S and Trezor Model T](guide-or-how-to-setup-a-validator-on-eth2-mainnet/#2-signup-to-be-a-validator-at-the-launchpad) with tool from [allnodes.com](https://twitter.com/Allnodes/status/1390020240541618177?s=20)
* [Batch deposit tool](guide-or-how-to-setup-a-validator-on-eth2-mainnet/#2-signup-to-be-a-validator-at-the-launchpad) by [abyss.finance](https://twitter.com/AbyssFinance/status/1379732382044069888) now added

### 📄 Latest Essential Ethereum Staking Reading

* [Modelling the Impact of Altair by pintail.xyz](https://pintail.xyz/posts/modelling-the-impact-of-altair/)

## 🏁 0. Prerequisites

### 👩🚀 Skills for operating a eth2 validator and beacon node

As a validator for eth2, you will typically have the following abilities:

* operational knowledge of how to set up, run and maintain a eth2 beacon node and validator continuously
* a long term commitment to maintain your validator 24/7/365
* basic operating system skills

### 👨💻 Experience required to be a successful validator

* have learned the essentials by watching ['Intro to Eth2 & Staking for Beginners' by Superphiz](https://www.youtube.com/watch?v=tpkpW031RCI)
* have passed or is actively enrolled in the [Eth2 Study Master course](https://ethereumstudymaster.com/)
* and have read the [8 Things Every Eth2 validator should know.](https://medium.com/chainsafe-systems/8-things-every-eth2-validator-should-know-before-staking-94df41701487)

### \*\*\*\*🍼 **Minimum Setup Requirements**

* **Operating system:** 64-bit Linux \(i.e. Ubuntu 20.04 LTS Server or Desktop\)
* **Processor:** Dual core CPU, Intel Core i5–760 or AMD FX-8100 or better
* **Memory:** 8GB RAM
* **Storage:** 128GB SSD
* **Internet:** Broadband internet connection with speeds at least 1 Mbps.
* **Power:** Reliable electrical power.
* **ETH balance:** at least 32 goerli ETH and some ETH for deposit transaction fees
* **Wallet**: Metamask installed

### 🏋♂ Recommended Hardware Setup

* **Operating system:** 64-bit Linux \(i.e. Ubuntu 20.04 LTS Server or Desktop\)
* **Processor:** Quad core CPU, Intel Core i7–4770 or AMD FX-8310 or better
* **Memory:** 16GB RAM or more
* **Storage:** 2TB SSD or more
* **Internet:** Broadband internet connections with speeds at least 10 Mbps without data limit. 
* **Power:** Reliable electrical power with uninterruptible power supply \(UPS\)
* **ETH balance:** at least 32 goerli ETH and some ETH for deposit transaction fees
* **Wallet**: Metamask installed

{% hint style="info" %}
💡 For examples of actual staking hardware builds, check out [RocketPool's hardware guide](https://github.com/rocket-pool/docs.rocketpool.net/blob/main/src/guides/node/local/hardware.md).
{% endhint %}

{% hint style="success" %}
🔥 **Pro Validator Tip**: Highly recommend you begin with a brand new instance of an OS, VM, and/or machine. Avoid headaches by NOT reusing testnet keys, wallets, or databases for your validator.
{% endhint %}

### 🔓 Recommended eth2 validator Security Best Practices

If you need ideas or a reminder on how to secure your validator, refer to

{% page-ref page="guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md" %}

### 🛠 Setup Ubuntu

If you need to install Ubuntu Server, refer to

{% embed url="https://ubuntu.com/tutorials/install-ubuntu-server\#1-overview" caption="" %}

or Ubuntu Desktop,

{% page-ref page="../overview-xtz/guide-how-to-setup-a-baker/install-ubuntu.md" %}

### 🎭 Setup Metamask

If you need to install Metamask, refer to

{% page-ref page="../../wallets/browser-wallets/metamask-ethereum.md" %}

### 🧩 High Level Validator Node Overview

{% hint style="info" %}
At the end of this guide, you will build a machine that hosts three main components: a validator client, a beacon chain client and an eth1 node.

**Validator client** - Responsible for producing new blocks and attestations in the beacon chain and shard chains.

**Beacon chain client** - Responsible for managing the state of the beacon chain, validator shuffling, and more.

**Eth1 node** - Supplies incoming validator deposits from the eth1 mainnet chain to the beacon chain client.

Note: Teku and Nimbus combines both clients into one process.
{% endhint %}

![](../../.gitbook/assets/eth2network.png)

## 🌱 1. Obtain testnet ETH

{% hint style="info" %}
Every 32 ETH you own allows you to make 1 validator. You can run thousands of validators with your beacon node. However on testnet, please only run 1 or 2 validators to keep the activation queue reasonably quick.
{% endhint %}

Join the [ethstaker Discord](https://discord.io/ethstaker) and send a request for ETH in the **`-request-goerli-eth channel`**

```text
!send <your metamask goerli network ETH address>
```

Otherwise, visit the 🚰[Goerli Authenticated Faucet](https://faucet.goerli.mudit.blog/).

## 🧙 2. Signup to be a validator at the Launchpad

1. Install dependencies, the ethereum foundation deposit tool and generate your two sets of key pairs.

{% hint style="info" %}
Each validator will have two sets of key pairs. A **signing key** and a **withdrawal key.** These keys are derived from a single mnemonic phrase. [Learn more about keys.](https://blog.ethereum.org/2020/05/21/keys/)
{% endhint %}

You have the choice of downloading the pre-built [ethereum foundation deposit tool](https://github.com/ethereum/eth2.0-deposit-cli) or building it from source. Alternatively, if you have a **Ledger Nano X/S or Trezor Model T**, you're able to generate deposit files with keys managed by a hardware wallet.

{% tabs %}
{% tab title="Hardware Wallet - Most Secure" %}
## How to generate validator keys with Ledger Nano X/S and Trezor Model T

{% hint style="info" %}
[Allnodes ](https://help.allnodes.com/en/articles/4664440-how-to-setup-ethereum-2-0-validator-node-on-allnodes)has created an easy to use tool to connect a Ledger Nano X/S and Trezor Model T and generate the deposit json files such that the withdrawal credentials remain secured by the hardware wallet. This tool can be used by any validator or staker.
{% endhint %}

1. Connect your hardware wallet to your PC/laptop 
2. If using a Ledger Nano X/S, open the "ETHEREUM" ledger app \(if missing, install from Ledger Live\)
3. Visit [AllNode's Deposit Generator Tool.](https://wallet.allnodes.com/eth2/generate)
4. Select network &gt; Gorli Testnet
5. Select your wallet &gt; then CONTINUE

![](../../.gitbook/assets/allnodes-menu-testnet.png)

6. From the dropdown, select your eth address with at least 32 ETH to fund your validators

7. On your hardware wallet, sign the ETH signature message to login to allnodes.com

8. Again on your hardware wallet, sign another message to verify your eth2 withdrawal credentials

{% hint style="info" %}
Double check that your generated deposit data file contains the same string as in withdrawal credentials and that this string includes your Ethereum address \(starting after 0x\)
{% endhint %}

![](../../.gitbook/assets/allnodes-3.png)

9. Enter the amount of nodes \(or validators you want\) 

10. Finally, enter a **KEYSTORE password** to encrypt the deposit json files. Keep this password safe and **offline**.

11. Confirm password and click **GENERATE**
{% endtab %}

{% tab title="Build from source code" %}
Install dependencies.

```text
sudo apt update
sudo apt install python3-pip git -y
```

Download source code and install.

```text
cd $HOME
git clone https://github.com/ethereum/eth2.0-deposit-cli.git eth2deposit-cli
cd eth2deposit-cli
sudo ./deposit.sh install
```

Make a new mnemonic.

```text
./deposit.sh new-mnemonic --chain prater
```

{% hint style="info" %}
**Advanced option**: Custom eth1 withdrawal address, often used for 3rd party staking.

```bash
# Add the following
--eth1_withdrawal_address <eth1 address hex string>
# Example
./deposit.sh new-mnemonic --chain prater --eth1_withdrawal_address 0x1...x
```

If this field is set and valid, the given Eth1 address will be used to create the withdrawal credentials. Otherwise, it will generate withdrawal credentials with the mnemonic-derived withdrawal public key in [EIP-2334 format](https://eips.ethereum.org/EIPS/eip-2334#eth2-specific-parameters).
{% endhint %}
{% endtab %}

{% tab title="Pre-built eth2deposit-cli" %}
Download eth2deposit-cli.

```bash
cd $HOME
wget https://github.com/ethereum/eth2.0-deposit-cli/releases/download/v1.2.0/eth2deposit-cli-256ea21-linux-amd64.tar.gz
```

Verify the SHA256 Checksum matches the checksum on the [releases page](https://github.com/ethereum/eth2.0-deposit-cli/releases/tag/v1.0.0).

```bash
echo "825035b6d6c06c0c85a38f78e8bf3e9df93dfd16bf7b72753b6888ae8c4cb30a *eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz" | shasum -a 256 --check
```

Example valid output:

> eth2deposit-cli-256ea21-linux-amd64.tar.gz: OK

{% hint style="danger" %}
Only proceed if the sha256 check passes with **OK**!
{% endhint %}

Extract the archive.

```text
tar -xvf eth2deposit-cli-256ea21-linux-amd64.tar.gz
mv eth2deposit-cli-256ea21-linux-amd64 eth2deposit-cli
rm eth2deposit-cli-256ea21-linux-amd64.tar.gz
cd eth2deposit-cli
```

Make a new mnemonic.

```text
./deposit new-mnemonic --chain prater
```

{% hint style="info" %}
**Advanced option**: Custom eth1 withdrawal address, often used for 3rd party staking.

```bash
# Add the following
--eth1_withdrawal_address <eth1 address hex string>
# Example
./deposit.sh new-mnemonic --chain prater --eth1_withdrawal_address 0x1...x
```

If this field is set and valid, the given Eth1 address will be used to create the withdrawal credentials. Otherwise, it will generate withdrawal credentials with the mnemonic-derived withdrawal public key in [EIP-2334 format](https://eips.ethereum.org/EIPS/eip-2334#eth2-specific-parameters).
{% endhint %}
{% endtab %}

{% tab title="Advanced - Most Secure" %}
{% hint style="warning" %}
🔥**\[ Optional \] Pro Security Tip**: Run the **eth2deposit-cli tool** and generate your **mnemonic seed** for your validator keys on an **air-gapped offline machine booted from usb**.
{% endhint %}

You will learn how to boot up a windows PC into an airgapped [Tails operating system](https://tails.boum.org/index.en.html).

The Tails OS is an _amnesic_ operating system, meaning it will save nothing and _leave no tracks behind_ each time you boot it.

## Part 0 - Prerequisites

You need:

* 2 storage mediums \(can be USB stick, SD cards or external hard drives\)
* One of them must be &gt; 8GB  
* Windows or Mac computer
* 30 minutes or longer depending on your download speed 

## Part 1 - Download Tails OS

Download the official image from the [Tails website](https://tails.boum.org/install/index.en.html). Might take a while, go grab a coffee.

Make sure you follow the guide on the Tails website to verify your download of Tails.

## Part 2 - Download and install the software to transfer your Tails image on your USB stick

For Windows, use one of

* [Etcher](https://tails.boum.org/etcher/Etcher-Portable.exe)
* [Win32 Disk Imager](https://win32diskimager.org/#download)
* [Rufus](https://rufus.ie/en_US/)

For Mac, download [Etcher](https://tails.boum.org/etcher/Etcher.dmg)

## Part 3 - Making your bootable USB stick

Run the above software. This is an example how it looks like on Mac OS with etcher, but other software should be similar.

![](../../.gitbook/assets/etcher_in_mac.png)

Select the Tails OS image that you downloaded as the image. Then select the USB stick \(the larger one\).

Then flash the image to the larger USB stick.

## Part 4 - Download and verify the eth2-deposit-cli

You can refer to the other tab on this guide on how to download and verify the eth2-deposit-cli.

Copy the file to the other USB stick.

## Part 5 - Reboot your computer and into Tails OS

After you have done all the above, you can reboot. If you are connected by a LAN cable to the internet, you can disconnect it manually.

Plug in the USB stick that has your Tails OS.

On Mac, press and hold the Option key immediately upon hearing the startup chime. Release the key after Startup Manager appears.

On Windows, it depends on your computer manufacturer. Usually it is by pressing F1 or F12. If it doesn't work, try googling "Enter boot options menu on \[Insert your PC brand\]"

Choose the USB stick that you loaded up with Tails OS to boot into Tails.

## Part 6 - Welcome to Tails OS

![](../../.gitbook/assets/grub.png)

You can boot with all the default settings.

## Part 7 - Run the eth2-deposit-cli

Plug in your other USB stick with the `eth2-deposit-cli` file.

You can then open your command line and navigate into the directory containing the file. Then you can continue the guide from the other tab.

Make a new mnemonic.

```text
./deposit.sh new-mnemonic --chain prater
```

If you ran this command directly from your non-Tails USB stick, the validator keys should stay on it. If it hasn't, copy the directory over to your non-Tails USB stick.

{% hint style="warning" %}
\*\*\*\*🔥 **Make sure you have saved your validator keys directory in your other USB stick \(non Tails OS\) before you shutdown Tails. Tails will delete everything saved on it after you shutdown.**.
{% endhint %}

{% hint style="success" %}
🎉 Congrats on learning how to use Tails OS to make an air gapped system. As a bonus, you can reboot into Tails OS again and connect to internet to surf the dark web or clear net safely!
{% endhint %}

Alternatively, follow this [ethstaker.cc](https://ethstaker.cc/) exclusive for the low down on making a bootable usb.

### Part 1 - Create a Ubuntu 20.04 USB Bootable Drive

{% embed url="https://www.youtube.com/watch?v=DTR3PzRRtYU" %}

### Part 2 - Install Ubuntu 20.04 from the USB Drive

{% embed url="https://www.youtube.com/watch?v=C97\_6MrufCE" %}

You can copy via USB key the pre-built eth2deposit-cli binaries from an online machine to an air-gapped offline machine booted from usb. Make sure to disconnect the ethernet cable and/or WIFI.
{% endtab %}
{% endtabs %}

2. If using **eth2deposit-cli**, follow the prompts and pick a **KEYSTORE password**. This password encrypts your keystore files. Write down your mnemonic and keep this safe and **offline**.

{% hint style="danger" %}
**Do not send real mainnet ETH during this process!** 🛑 Use only goerli ETH.
{% endhint %}

{% hint style="warning" %}
\*\*\*\*🚧 **Caution**: Only deposit the 32 ETH per validator if you are confident your ETH1 node and ETH2 validator will be fully synched and ready to perform validator duties. You can return later to launchpad with your deposit-data to finish the next steps.
{% endhint %}

3. Follow the steps at [https://prater.launchpad.ethereum.org](https://prater.launchpad.ethereum.org/en/) while skipping over the steps you already just completed. Study the eth2 phase 0 overview material. Understanding eth2 is the key to success!

{% hint style="info" %}
\*\*\*\*🐳 **Batch Depositing Tip**: If you have many deposits to make for many validators, consider using [Abyss.finance's eth2depositor tool.](https://abyss.finance/eth2depositor) This greatly improves the deposit experience as multiple deposits can be batched into one transaction, thereby saving gas fees and saving your fingers by minimizing Metamask clicking.

Make sure to switch to **GÖRLI** network.

Source: [https://twitter.com/AbyssFinance/status/1379732382044069888](https://twitter.com/AbyssFinance/status/1379732382044069888)
{% endhint %}

4. Back on the launchpad website, upload your`deposit_data-#########.json` found in the `validator_keys` directory.

5. Connect to the launchpad with your Metamask wallet, review and accept terms.

6. Confirm the transaction\(s\). There's one deposit transaction of 32 ETH for each validator. 

{% hint style="info" %}
For instance, if you want to run 3 validators you will need to have \(32 x 3\) = 96 goerli ETH plus some extra to cover the gas fees.
{% endhint %}

{% hint style="info" %}
Your transaction is sending and depositing your ETH to the prater ETH2 deposit contract address.

**Check**, _double-check_, _**triple-check**_ that the prater Eth2 deposit contract address is correct.

[`0xff50ed3d0ec03aC01D4C79aAd74928BFF48a7b2b`](https://goerli.etherscan.io/address/0xff50ed3d0ec03ac01d4c79aad74928bff48a7b2b)\`\`
{% endhint %}

{% hint style="danger" %}
🔥 **Critical Crypto Reminder:** **Keep your mnemonic, keep your ETH.**

* Write down your mnemonic seed **offline**. _Not email. Not cloud._
* Multiple copies are better. _Best stored in a_ [_metal seed._](https://jlopp.github.io/metal-bitcoin-storage-reviews/)
* The withdrawal keys will be generated from this mnemonic in the future.
* Make **offline backups**, such as to a USB key, of your **`validator_keys`** directory.
{% endhint %}

## 🛰 3. Install a ETH1 node

{% hint style="info" %}
Ethereum 2.0 requires a connection to Ethereum 1.0 in order to monitor for 32 ETH validator deposits. Hosting your own Ethereum 1.0 node is the best way to maximize decentralization and minimize dependency on third parties such as Infura.
{% endhint %}

{% hint style="warning" %}
The subsequent steps assume you have completed the [best practices security guide. ](https://www.coincashew.com/coins/overview-eth/guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node)

🛑 Do not run your processes as **ROOT** user. 😲 
{% endhint %}

Your choice of either [**OpenEthereum**](https://www.parity.io/ethereum/)**,** [**Geth**](https://geth.ethereum.org/)**,** [**Besu**](https://besu.hyperledger.org/)**,** [**Nethermind**](https://www.nethermind.io/) **or** [**Infura**](https://infura.io/)**.**

{% tabs %}
{% tab title="Geth" %}
{% hint style="info" %}
**Geth** - Go Ethereum is one of the three original implementations \(along with C++ and Python\) of the Ethereum protocol. It is written in **Go**, fully open source and licensed under the GNU LGPL v3.
{% endhint %}

#### 🧬 Install from the repository

```text
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update -y
sudo apt dist-upgrade -y
sudo apt-get install ethereum -y
```

⚙ **Setup and configure systemd**

Run the following to create a **unit file** to define your `eth1.service` configuration.

Simply copy/paste the following.

```bash
cat > $HOME/eth1.service << EOF 
[Unit]
Description     = geth eth1 service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = /usr/bin/geth --http --goerli --metrics --pprof
Restart         = on-failure
RestartSec      = 3
TimeoutSec      = 300

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="info" %}
**Nimbus Specific Configuration**: Add the following flag to the **ExecStart** line.

```bash
--ws
```
{% endhint %}

Move the unit file to `/etc/systemd/system` and give it permissions.

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

Run the following to enable auto-start at boot time.

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1
```

#### ⛓ Start geth

```text
sudo systemctl start eth1
```

{% hint style="info" %}
**Geth Tip**: When is my geth node synched?

1. Attach to the geth console with:`geth attach` [`http://127.0.0.1:8545`](http://127.0.0.1:8545)\`\`
2. Type the following:`eth.syncing`
3. If it returns false, your geth node is synched.
{% endhint %}
{% endtab %}

{% tab title="Besu" %}
{% hint style="info" %}
**Hyperledger Besu** is an open-source Ethereum client designed for demanding enterprise applications requiring secure, high-performance transaction processing in a private network. It's developed under the Apache 2.0 license and written in **Java**.
{% endhint %}

#### 🧬 Install java dependency

```text
sudo apt-get update
sudo apt install openjdk-11-jdk -y
```

#### 🌍 Download and unzip Besu

Review the latest release at [https://github.com/hyperledger/besu/releases](https://github.com/hyperledger/besu/releases)

File can be downloaded from [https://dl.bintray.com/hyperledger-org/besu-repo](https://dl.bintray.com/hyperledger-org/besu-repo)

```text
cd
wget -O besu.tar.gz https://dl.bintray.com/hyperledger-org/besu-repo/besu-20.10.1.tar.gz
tar -xvf besu.tar.gz
rm besu.tar.gz
mv besu* besu
```

⚙ **Setup and configure systemd**

Run the following to create a **unit file** to define your `eth1.service` configuration.

Simply copy/paste the following.

```bash
cat > $HOME/eth1.service << EOF 
[Unit]
Description     = besu eth1 service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/besu/bin/besu --metrics-enabled --rpc-http-enabled --network=goerli --data-path="$HOME/.besu_goerli"
Restart         = on-failure
RestartSec      = 3

[Install]
WantedBy    = multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system` and give it permissions.

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

Run the following to enable auto-start at boot time.

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1
```

#### ⛓ Start besu

```text
sudo systemctl start eth1
```
{% endtab %}

{% tab title="Nethermind" %}
{% hint style="info" %}
**Nethermind** is a flagship Ethereum client all about performance and flexibility. Built on **.NET** core, a widespread, enterprise-friendly platform, Nethermind makes integration with existing infrastructures simple, without losing sight of stability, reliability, data integrity, and security.
{% endhint %}

#### ⚙ Install dependencies

```text
sudo apt-get update
sudo apt-get install curl libsnappy-dev libc6-dev jq libc6 unzip -y
```

#### 🌍 Download and unzip Nethermind

Review the latest release at [https://github.com/NethermindEth/nethermind/releases](https://github.com/NethermindEth/nethermind/releases)

Automatically download the latest linux release, un-zip and cleanup.

```bash
mkdir $HOME/nethermind
chmod 775 $HOME/nethermind
cd $HOME/nethermind
curl -s https://api.github.com/repos/NethermindEth/nethermind/releases/latest | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux  | xargs wget -q --show-progress
unzip -o nethermind*.zip
rm nethermind*linux*.zip
```

⚙ **Setup and configure systemd**

Run the following to create a **unit file** to define your `eth1.service` configuration.

Simply copy/paste the following.

```bash
cat > $HOME/eth1.service << EOF 
[Unit]
Description     = nethermind eth1 service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/nethermind/Nethermind.Runner --config goerli --baseDbPath $HOME/.nethermind_goerli --Metrics.Enabled true --JsonRpc.Enabled true --Sync.DownloadBodiesInFastSync true --Sync.DownloadReceiptsInFastSync true --Sync.AncientBodiesBarrier 11052984 --Sync.AncientReceiptsBarrier 11052984
Restart         = on-failure
RestartSec      = 3

[Install]
WantedBy    = multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system` and give it permissions.

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

Run the following to enable auto-start at boot time.

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1
```

#### ⛓ Start Nethermind

```text
sudo systemctl start eth1
```

{% hint style="info" %}
**Note about Metric Error messages**: You will see these until prometheus pushergateway is setup in section 6. `Error in MetricPusher: System.Net.Http.HttpRequestException: Connection refused`
{% endhint %}
{% endtab %}

{% tab title="Erigon" %}
{% hint style="info" %}
**Erigon** - Successor to OpenEthereum, Erigon is an implementation of Ethereum \(aka "Ethereum client"\), on the efficiency frontier, written in Go.
{% endhint %}

#### ⚙ Install Go dependencies

```text
wget -O go.tar.gz https://golang.org/dl/go1.16.5.linux-amd64.tar.gz
```

```bash
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go.tar.gz
```

```bash
echo export PATH=$PATH:/usr/local/go/bin>> $HOME/.bashrc
source $HOME/.bashrc
```

Verify Go is properly installed and cleanup files.

```bash
go version
rm go.tar.gz
```

#### 🤖 Build and install Erigon

Install build dependencies.

```bash
sudo apt-get update
sudo apt install build-essential git
```

Review the latest release at [https://github.com/ledgerwatch/erigon/releases](https://github.com/ledgerwatch/erigon/releases)

```bash
cd $HOME
git clone --recurse-submodules -j8 https://github.com/ledgerwatch/erigon.git
cd erigon
make erigon && make rpcdaemon
```

Make data directory and update directory ownership.

```bash
sudo mkdir -p /var/lib/erigon
sudo chown $USER:$USER /var/lib/erigon
```

​ ⚙ **Setup and configure systemd**

Run the following to create a **unit file** to define your `eth1.service` configuration.

Simply copy/paste the following.

```bash
cat > $HOME/eth1.service << EOF 
[Unit]
Description     = erigon eth1 service
Wants           = network-online.target
After           = network-online.target 
Requires        = eth1-erigon.service

[Service]
Type            = simple
User            = $USER
ExecStart       = $HOME/erigon/build/bin/erigon --datadir /var/lib/erigon --chain goerli --private.api.addr=localhost:9089 --metrics --pprof --prune htc
Restart         = on-failure
RestartSec      = 3
KillSignal      = SIGINT
TimeoutStopSec  = 5

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="info" %}
 By default with Erigon, `--prune` deletes data older than 90K blocks from the tip of the chain \(aka, for if tip block is no. 12'000'000, only the data between 11'910'000-12'000'000 will be kept\).
{% endhint %}

```bash
cat > $HOME/eth1-erigon.service << EOF 
[Unit]
Description     = erigon rpcdaemon service
BindsTo	        = eth1.service
After           = eth1.service

[Service]
Type            = simple
User            = $USER
ExecStartPre	  = /bin/sleep 3
ExecStart       = $HOME/erigon/build/bin/rpcdaemon --private.api.addr=localhost:9089 --datadir /var/lib/erigon --http.api=eth,erigon,web3,net,debug,trace,txpool,shh
Restart         = on-failure
RestartSec      = 3
KillSignal      = SIGINT
TimeoutStopSec  = 5

[Install]
WantedBy    = eth1.service
EOF
```

Move the unit files to `/etc/systemd/system` and give it permissions.

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
sudo mv $HOME/eth1-erigon.service /etc/systemd/system/eth1-erigon.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
sudo chmod 644 /etc/systemd/system/eth1-erigon.service
```

Run the following to enable auto-start at boot time.

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1 eth1-erigon
```

#### ⛓ Start Erigon

```text
sudo systemctl start eth1
```
{% endtab %}

{% tab title="OpenEthereum \(Retired\)" %}
{% hint style="danger" %}
OpenEthereum will no longer be supported post London hard fork.  Gnosis, maintainers of OpenEthereum, suggest users migrate to their new **Erigon** Ethererum client.
{% endhint %}

{% hint style="info" %}
**OpenEthereum** - It's ****goal is to be the fastest, lightest, and most secure Ethereum client using the **Rust programming language**. OpenEthereum is licensed under the GPLv3 and can be used for all your Ethereum needs.
{% endhint %}

#### ⚙ Install dependencies

```text
sudo apt-get update
sudo apt-get install curl jq unzip -y
```

#### 🤖 Install OpenEthereum

Review the latest release at [https://github.com/openethereum/openethereum/releases](https://github.com/openethereum/openethereum/releases)

Automatically download the latest linux release, un-zip, add execute permissions and cleanup.

```bash
mkdir $HOME/openethereum
cd $HOME/openethereum
curl -s https://api.github.com/repos/openethereum/openethereum/releases/latest | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux | xargs wget -q --show-progress
unzip -o openethereum*.zip
chmod +x openethereum
rm openethereum*.zip
```

​ ⚙ **Setup and configure systemd**

Run the following to create a **unit file** to define your `eth1.service` configuration.

Simply copy/paste the following.

```bash
cat > $HOME/eth1.service << EOF 
[Unit]
Description     = openethereum eth1 service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/openethereum/openethereum --chain goerli --metrics --metrics-port=6060
Restart         = on-failure
RestartSec      = 3

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="info" %}
**Nimbus Specific Configuration**: Add the following flag to the **ExecStart** line.

```bash
--ws-origins=all
```
{% endhint %}

Move the unit file to `/etc/systemd/system` and give it permissions.

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

Run the following to enable auto-start at boot time.

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1
```

#### ⛓ Start OpenEthereum

```text
sudo systemctl start eth1
```
{% endtab %}

{% tab title="Minimum Hardware Setup \(Infura\)" %}
{% hint style="info" %}
Infura is suitable for limited disk space setups. Always run your own full eth1 node when possible.
{% endhint %}

Sign up for an API access key at [https://infura.io/](https://infura.io/)

1. Sign up for a free account.
2. Confirm your email address.
3. Visit your dashboard [https://infura.io/dashboard](https://infura.io/dashboard)
4. Create a project, give it a name.
5. Select **Goerli** as the ENDPOINT
6. Follow the specific configuration for your eth2 client found below.

{% hint style="success" %}
Alternatively use a free testnet Ethereum node such as [Chainstack ](https://chainstack.com)at [https://ethereumnodes.com/](https://ethereumnodes.com/)
{% endhint %}

## Nimbus Specific Configuration

1. When creating your systemd's **unit file**, update the `--web-url` parameter with this endpoint. 
2. Copy the websocket endpoint. Starts with `wss://`
3. Save this for step 4, configuring your eth2 node.

```bash
#example
--web3-url=<your wss:// infura endpoint>
```

## Teku Specific Configuration

1. After creating the `teku.yaml` located in `/etc/teku/teku.yaml`, update the `--eth1-endpoint` parameter with this endpoint. 
2. Copy the http endpoint. Starts with `http://`
3. Save this for step 4, configuring your eth2 node.

```bash
#example
eth1-endpoint: <your https:// infura endpoint>
```

## Lighthouse Specific Configuration

1. When creating your **beacon chain systemd** **unit file**, add the `--eth1-endpoint` parameter with this endpoint. 
2. Copy the **https** endpoint. Starts with `https://`
3. Save this for step 4, configuring your eth2 node.

```bash
#example
--eth1-endpoint=<your https:// infura endpoint>
```

## Prysm Specific Configuration

1. When creating your **beacon chain systemd unit file**, update the `--http-web3provider` parameter with this endpoint. 
2. Copy the **https** endpoint. Starts with `https://`
3. Save this for step 4, configuring your eth2 node.

```bash
#example
--http-web3provider=<your https:// infura endpoint>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Syncing an eth1 node can take up to 1 week. On high-end machines with gigabit internet, expect syncing to take less than a day.
{% endhint %}

{% hint style="success" %}
Your eth1 node is fully sync'd when these events occur.

* **`OpenEthereum:`** `Imported #<block number>`
* **`Geth:`** `Imported new chain segment`
* **`Besu:`** `Imported #<block number>`
* **`Nethermind:`** `No longer syncing Old Headers`
{% endhint %}

#### 🛠 Helpful eth1.service commands <a id="helpful-eth-1-service-commands"></a>

​​ 📄 **To view and follow eth1 logs**

```text
journalctl -fu eth1
```

​ 🛑 **To stop eth1 service**

```text
sudo systemctl stop eth1
```

## 🌜 4. Configure a ETH2 beacon chain node and validator

Your choice of [Lighthouse](https://github.com/sigp/lighthouse), [Nimbus](https://github.com/status-im/nimbus-eth2), [Teku](https://consensys.net/knowledge-base/ethereum-2/teku/), [Prysm](https://github.com/prysmaticlabs/prysm) or [Lodestar](https://lodestar.chainsafe.io/).

{% tabs %}
{% tab title="Lighthouse" %}
{% hint style="info" %}
[Lighthouse](https://github.com/sigp/lighthouse) is an Eth2.0 client with a heavy focus on speed and security. The team behind it, [Sigma Prime](https://sigmaprime.io/), is an information security and software engineering firm who have funded Lighthouse along with the Ethereum Foundation, Consensys, and private individuals. Lighthouse is built in Rust and offered under an Apache 2.0 License.
{% endhint %}

## ⚙ 4.1. Install rust dependency

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Enter '1' to proceed with the default install.

Update your environment variables.

```bash
echo export PATH="$HOME/.cargo/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
```

Install rust dependencies.

```text
sudo apt install -y git gcc g++ make cmake pkg-config libssl-dev
```

## 💡 4.2. Build Lighthouse from source

```bash
mkdir ~/git
cd ~/git
git clone https://github.com/sigp/lighthouse.git
cd lighthouse
git fetch --all && git checkout stable && git pull
make
```

{% hint style="info" %}
In case of compilation errors, run the following sequence.

```text
rustup update
cargo clean
make
```
{% endhint %}

{% hint style="info" %}
This build process may take a few minutes.
{% endhint %}

Verify lighthouse was installed properly by checking the version number.

```text
lighthouse --version
```

## 🎩 4.3. Import validator key

{% hint style="info" %}
When you import your keys into Lighthouse, your validator signing key\(s\) are stored in the `$HOME/.lighthouse/prymont/validators` folder.
{% endhint %}

Run the following command to import your validator keys from the eth2deposit-cli tool directory.

Enter your **keystore password** to import accounts.

```bash
lighthouse account validator import --network prater --directory=$HOME/eth2deposit-cli/validator_keys
```

Verify the accounts were imported successfully.

```bash
lighthouse account_manager validator list --network prater
```

{% hint style="danger" %}
**WARNING**: DO NOT USE THE ORIGINAL KEYSTORES TO VALIDATE WITH ANOTHER CLIENT, OR YOU WILL GET SLASHED.
{% endhint %}

## 🔥4.4. Configure port forwarding and/or firewall

Specific to your networking setup or cloud provider settings, [ensure your validator's firewall ports are open and reachable.](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Lighthouse beacon chain** requires port 9000 for tcp and udp
* **eth1** node requires port 30303 for tcp and udp

{% hint style="info" %}
**Port Forwarding Tip:** You'll need to forward and open ports to your validator. Verify it's working with [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) or [https://canyouseeme.org/](https://canyouseeme.org/) .
{% endhint %}

## ⛓ 4.5. Start the beacon chain

#### Benefits of using systemd for your beacon chain <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your beacon chain when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed beacon chain processes.
3. Maximize your beacon chain up-time and performance.

#### Setup Instructions

Run the following to create a **unit file** to define your`beacon-chain.service` configuration.

```bash
cat > $HOME/beacon-chain.service << EOF 
# The eth2 beacon chain service (part of systemd)
# file: /etc/systemd/system/beacon-chain.service 

[Unit]
Description     = eth2 beacon chain service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $USER
ExecStart       = $(which lighthouse) bn --staking --validator-monitor-auto --metrics --network prater
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="info" %}
🔥 **Lighthouse Pro Tip:** On the **ExecStart** line, adding the `--eth1-endpoints` flag allows for redundant eth1 nodes. Separate with comma. Make sure the endpoint does not end with a trailing slash or`/` Remove it.

```bash
# Example:
--eth1-endpoints http://localhost:8545,https://goerli.infura.io/v3/xxx
```

Signup for free goerli ethereum fallback nodes at [https://infura.io/](https://infura.io/)
{% endhint %}

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

Give it permissions.

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

Run the following to enable auto-start at boot time and then start your beacon node service.

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
Nice work. Your beacon chain is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

### 🛠 Some helpful systemd commands

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl -fu beacon-chain
```

```bash
#view log since yesterday
journalctl --unit=beacon-chain --since=yesterday
```

```bash
#view log since today
journalctl --unit=beacon-chain --since=today
```

```bash
#view log between a date
journalctl --unit=beacon-chain --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

#### ✅ Check whether the beacon chain is active

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 View the status of the beacon chain

```text
sudo systemctl status beacon-chain
```

#### 🔄 Restarting the beacon chain

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 Stopping the beacon chain

```text
sudo systemctl stop beacon-chain
```

## 🧬 4.6. Start the validator <a id="4-6-start-the-validator"></a>

#### ​🚀 Setup Graffiti and POAP <a id="setup-graffiti-and-poap"></a>

Setup your `graffiti`, a custom message included in blocks your validator successfully proposes, and earn a POAP token. [Generate your POAP string by supplying an Ethereum 1.0 address here.](https://beaconcha.in/poap)​

Run the following command to set the `MY_GRAFFITI` variable. Replace `<my POAP string or message>` between the single quotes.

```bash
MY_GRAFFITI='<my POAP string or message>'
# Examples
# MY_GRAFFITI='poapAAAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
Learn more about [POAP - The Proof of Attendance token. ](https://www.poap.xyz/)
{% endhint %}

#### ​ 🍰 Benefits of using systemd for your validator <a id="benefits-of-using-systemd-for-your-stake-pool-1"></a>

1. Auto-start your validator when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed validator processes.
3. Maximize your validator up-time and performance.

#### ​ 🛠 Setup Instructions for Systemd <a id="setup-instructions-for-systemd-1"></a>

Run the following to create a **unit file** to define your`validator.service` configuration. Simply copy and paste.

```bash
cat > $HOME/validator.service << EOF 
# The eth2 validator service (part of systemd)
# file: /etc/systemd/system/validator.service 

[Unit]
Description     = eth2 validator service
Wants           = network-online.target beacon-chain.service
After           = network-online.target 

[Service]
User            = $USER
ExecStart       = $(which lighthouse) vc --network prater --graffiti "${MY_GRAFFITI}" --metrics --enable-doppelganger-protection
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system` 

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```

Run the following to enable auto-start at boot time and then start your validator.

```text
sudo systemctl daemon-reload
sudo systemctl enable validator
sudo systemctl start validator
```

{% hint style="success" %}
Nice work. Your validator is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

### 🛠 Some helpful systemd commands

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl -fu validator
```

```bash
#view log since yesterday
journalctl --unit=validator --since=yesterday
```

```bash
#view log since today
journalctl --unit=validator --since=today
```

```bash
#view log between a date
journalctl --unit=validator --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

#### ✅ Check whether the validator is active

```text
sudo systemctl is-active validator
```

#### 🔎 View the status of the validator

```text
sudo systemctl status validator
```

#### 🔄 Restarting the validator

```text
sudo systemctl reload-or-restart validator
```

#### 🛑 Stopping the validator

```text
sudo systemctl stop validator
```
{% endtab %}

{% tab title="Nimbus" %}
{% hint style="info" %}
[Nimbus](https://our.status.im/tag/nimbus/) is a research project and a client implementation for Ethereum 2.0 designed to perform well on embedded systems and personal mobile devices, including older smartphones with resource-restricted hardware. The Nimbus team are from [Status](https://status.im/about/) the company best known for [their messaging app/wallet/Web3 browser](https://status.im/) by the same name. Nimbus \(Apache 2\) is written in Nim, a language with Python-like syntax that compiles to C.
{% endhint %}

{% hint style="info" %}
💡 **Noteworthy**: binaries for all the usual platforms as well as dockers for x86 and arm can be found below:

[https://github.com/status-im/nimbus-eth2/releases/](https://github.com/status-im/nimbus-eth2/releases/)[https://hub.docker.com/r/statusim/nimbus-eth2](https://hub.docker.com/r/statusim/nimbus-eth2)
{% endhint %}

## ⚙4.1. Build Nimbus from source

Install dependencies.

```text
sudo apt-get update
sudo apt-get install curl build-essential git -y
```

Install and build Nimbus.

```bash
mkdir ~/git 
cd ~/git
git clone https://github.com/status-im/nimbus-eth2
cd nimbus-eth2
make nimbus_beacon_node
```

{% hint style="info" %}
The build process may take a few minutes.
{% endhint %}

Verify Nimbus was installed properly by displaying the help.

```bash
cd $HOME/git/nimbus-eth2/build
./nimbus_beacon_node --help
```

Copy the binary file to `/usr/bin`

```bash
sudo cp $HOME/git/nimbus-eth2/build/nimbus_beacon_node /usr/bin
```

## 🎩 4.2. Import validator key <a id="6-import-validator-key"></a>

Create a directory structure to store nimbus data.

```bash
sudo mkdir -p /var/lib/nimbus
```

Take ownership of this directory and set the correct permission level.

```bash
sudo chown $(whoami):$(whoami) /var/lib/nimbus
sudo chmod 700 /var/lib/nimbus
```

The following command will import your validator keys.

Enter your **keystore password** to import accounts.

```bash
cd $HOME/git/nimbus-eth2
build/nimbus_beacon_node deposits import --data-dir=/var/lib/nimbus $HOME/eth2deposit-cli/validator_keys
```

Now you can verify the accounts were imported successfully by doing a directory listing.

```bash
ll /var/lib/nimbus/validators
```

You should see a folder named for each of your validator's pubkey.

{% hint style="info" %}
When you import your keys into Nimbus, your validator signing key\(s\) are stored in the `/var/lib/nimbus` folder, under `secrets` and `validators.`

The `secrets` folder contains the common secret that gives you access to all your validator keys.

The `validators` folder contains your signing keystore\(s\) \(encrypted keys\). Keystores are used by validators as a method for exchanging keys.

For more on keys and keystores, see [here](https://blog.ethereum.org/2020/05/21/keys/).
{% endhint %}

{% hint style="danger" %}
**WARNING**: DO NOT USE THE ORIGINAL KEYSTORES TO VALIDATE WITH ANOTHER CLIENT, OR YOU WILL GET SLASHED.
{% endhint %}

## 🔥 4.3. Configure port forwarding and/or firewall

Specific to your networking setup or cloud provider settings, [ensure your validator's firewall ports are open and reachable.](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Nimbus beacon chain node** will use port 9000 for tcp and udp
* **eth1** node requires port 30303 for tcp and udp

{% hint style="info" %}
✨ **Port Forwarding Tip:** You'll need to forward and open ports to your validator. Verify it's working with [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) or [https://canyouseeme.org/](https://canyouseeme.org/) .
{% endhint %}

## 👩🚀 4.4. Start the beacon chain and validator

{% hint style="info" %}
Nimbus combines both the beacon chain and validator into one process.
{% endhint %}

#### 🚀 Setup Graffiti and POAP

Setup your `graffiti`, a custom message included in blocks your validator successfully proposes, and earn a POAP token. [Generate your POAP string by supplying an Ethereum 1.0 address here.](https://beaconcha.in/poap)

Run the following command to set the `MY_GRAFFITI` variable. Replace `<my POAP string or message>` between the single quotes. 

```bash
MY_GRAFFITI='<my POAP string or message>'
# Examples
# MY_GRAFFITI='poapAAAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
Learn more about [POAP - The Proof of Attendance token. ](https://www.poap.xyz/)
{% endhint %}

#### 🍰 Benefits of using systemd for your beacon chain and validator <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your beacon chain when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed beacon chain processes.
3. Maximize your beacon chain up-time and performance.

#### 🛠 Setup Instructions

Run the following to create a **unit file** to define your`beacon-chain.service` configuration.

```bash
cat > $HOME/beacon-chain.service << EOF 
# The eth2 beacon chain service (part of systemd)
# file: /etc/systemd/system/beacon-chain.service 

[Unit]
Description     = eth2 beacon chain service
Wants           = network-online.target
After           = network-online.target 

[Service]
Type            = simple
User            = $(whoami)
WorkingDirectory= /var/lib/nimbus
ExecStart       = /bin/bash -c '/usr/bin/nimbus_beacon_node --network=prater --graffiti="${MY_GRAFFITI}" --data-dir=/var/lib/nimbus --web3-url=ws://127.0.0.1:8546 --metrics --metrics-port=8008 --rpc --rpc-port=9091 --validators-dir=/var/lib/nimbus/validators --secrets-dir=/var/lib/nimbus/secrets --log-file=/var/lib/nimbus/beacon.log'
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="warning" %}
Nimbus only supports websocket connections \("ws://" and "wss://"\) for the ETH1 node. Geth, OpenEthereum and Infura ETH1 nodes are verified compatible.
{% endhint %}

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

Run the following to enable auto-start at boot time and then start your beacon node service.

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
Nice work. Your beacon chain is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

### 🛠 Some helpful systemd commands

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl -fu beacon-chain
```

```bash
#view log since yesterday
journalctl --unit=beacon-chain --since=yesterday
```

```bash
#view log since today
journalctl --unit=beacon-chain --since=today
```

```bash
#view log between a date
journalctl --unit=beacon-chain --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

#### ✅ Check whether the beacon chain is active

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 View the status of the beacon chain

```text
sudo systemctl status beacon-chain
```

#### 🔄 Restarting the beacon chain

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 Stopping the beacon chain

```text
sudo systemctl stop beacon-chain
```
{% endtab %}

{% tab title="Teku" %}
{% hint style="info" %}
[PegaSys Teku](https://pegasys.tech/teku/) \(formerly known as Artemis\) is a Java-based Ethereum 2.0 client designed & built to meet institutional needs and security requirements. PegaSys is an arm of [ConsenSys](https://consensys.net/) dedicated to building enterprise-ready clients and tools for interacting with the core Ethereum platform. Teku is Apache 2 licensed and written in Java, a language notable for its materity & ubiquity.
{% endhint %}

## ⚙ 4.1 Build Teku from source

Install git.

```text
sudo apt-get install git -y
```

Install Java 11.

For **Ubuntu 20.x**, use the following

```text
sudo apt update
sudo apt install openjdk-11-jdk -y
```

For **Ubuntu 18.x**, use the following

```text
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-set-default -y
```

Verify Java 11+ is installed.

```bash
java --version
```

Install and build Teku.

```bash
mkdir ~/git
cd ~/git
git clone https://github.com/ConsenSys/teku.git
cd teku
./gradlew distTar installDist
```

{% hint style="info" %}
This build process may take a few minutes.
{% endhint %}

Verify Teku was installed properly by displaying the version.

```bash
cd $HOME/git/teku/build/install/teku/bin
./teku --version
```

Copy the teku binary file to `/usr/bin/teku`

```bash
sudo cp -r $HOME/git/teku/build/install/teku /usr/bin/teku
```

## 🔥 4.2. Configure port forwarding and/or firewall

Specific to your networking setup or cloud provider settings, [ensure your validator's firewall ports are open and reachable.](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Teku beacon chain node** will use port 9000 for tcp and udp
* **eth1** node requires port 30303 for tcp and udp

{% hint style="info" %}
✨ **Port Forwarding Tip:** You'll need to forward and open ports to your validator. Verify it's working with [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) or [https://canyouseeme.org/](https://canyouseeme.org/) .
{% endhint %}

## 🌍 4.3. Configure the beacon chain and validator

{% hint style="info" %}
Teku combines both the beacon chain and validator into one process.
{% endhint %}

{% hint style="warning" %}
If you participated in any of the prior test nets, you need to clear the database.

```bash
rm -rf $HOME/.local/share/teku/data
```
{% endhint %}

Setup a directory structure for Teku.

```bash
sudo mkdir -p /var/lib/teku
sudo mkdir -p /etc/teku
sudo chown $(whoami):$(whoami) /var/lib/teku
```

Copy your `validator_files` directory to the data directory we created above and remove the extra deposit\_data file.

```bash
cp -r $HOME/eth2deposit-cli/validator_keys /var/lib/teku
rm /var/lib/teku/validator_keys/deposit_data*
```

{% hint style="danger" %}
**WARNING**: DO NOT USE THE ORIGINAL KEYSTORES TO VALIDATE WITH ANOTHER CLIENT, OR YOU WILL GET SLASHED.
{% endhint %}

Store your **keystore password** in a file and make it read-only. This is required so that Teku can decrypt and load your validators.

Update your **keystore password** between the quotation marks after `echo`.

```bash
echo 'my_keystore_password' > $HOME/validators-password.txt
sudo mv $HOME/validators-password.txt /etc/teku/validators-password.txt
sudo chmod 600 /etc/teku/validators-password.txt
```

Clear the bash history in order to remove traces of keystore password.

```bash
shred -u ~/.bash_history && touch ~/.bash_history
```

#### 🚀 Setup Graffiti and POAP

Setup your `graffiti`, a custom message included in blocks your validator successfully proposes.

Run the following command to set the `MY_GRAFFITI` variable. Replace `<my POAP string or message>` between the single quotes. 

```bash
MY_GRAFFITI='<my POAP string or message>'
```

```bash
# Examples
# MY_GRAFFITI='poapAAAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
Learn more about [POAP - The Proof of Attendance token. ](https://www.poap.xyz/)
{% endhint %}

⏩ **Setup Teku Checkpoint Sync**

{% hint style="info" %}
Teku's Checkpoint Cync utilizes Infura to create the fastest syncing Ethereum beacon chain.
{% endhint %}

1. Sign up for [a free infura account](https://infura.io/register).

2. Create a project.

![](../../.gitbook/assets/inf1.png)

3. Copy your Project's ENDPOINT. Ensure the correct Network is selected with the dropdown box.

![](../../.gitbook/assets/inf2.png)

Replace `<my infura Project's ENDPOINT>` with your Infura endpoint and then run the following command to set the `INFURA_PROJECT_ENDPOINT` variable.

```bash
INFURA_PROJECT_ENDPOINT=<my Infura Project's ENDPOINT>
```

```bash
# Example
# INFURA_PROJECT_ENDPOINT=1Rjimg6q8hxGaRfxmEf9vxyBEk5n:c42acfe90bcae227f9ec19b22e733550@eth2-beacon-prater.infura.io
```

Confirm that your Infura Project Endpoint looks correct.

```bash
echo $INFURA_PROJECT_ENDPOINT
```

Generate your Teku Config file. Simply copy and paste.

```bash
cat > $HOME/teku.yaml << EOF
# network
network: "prater"
initial-state: "${INFURA_PROJECT_ENDPOINT}/eth/v1/debug/beacon/states/finalized" 

# p2p
p2p-enabled: true
p2p-port: 9000
# validators
validator-keys: "/var/lib/teku/validator_keys:/var/lib/teku/validator_keys"
validators-graffiti: "${MY_GRAFFITI}"

# Eth 1
eth1-endpoint: "http://localhost:8545"

# metrics
metrics-enabled: true
metrics-port: 8008

# database
data-path: "$(echo $HOME)/tekudata"
data-storage-mode: "archive"

# rest api
rest-api-port: 5051
rest-api-docs-enabled: true
rest-api-enabled: true

# logging
log-include-validator-duties-enabled: true
log-destination: CONSOLE
EOF
```

Move the config file to `/etc/teku`

```bash
sudo mv $HOME/teku.yaml /etc/teku/teku.yaml
```

## 🎩 4.4 Import validator key

{% hint style="info" %}
When specifying directories for your validator-keys, Teku expects to find identically named keystore and password files.

For example `keystore-m_12221_3600_1_0_0-11222333.json` and `keystore-m_12221_3600_1_0_0-11222333.txt`
{% endhint %}

Create a corresponding password file for every one of your validators.

```bash
for f in /var/lib/teku/validator_keys/keystore*.json; do cp /etc/teku/validators-password.txt /var/lib/teku/validator_keys/$(basename $f .json).txt; done
```

Verify that your validator's keystore and validator's passwords are present by checking the following directory.

```bash
ll /var/lib/teku/validator_keys
```

## 🏁 4.5. Start the beacon chain and validator

Use **systemd** to manage starting and stopping teku.

#### Benefits of using systemd for your beacon chain and validator <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your beacon chain when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed beacon chain processes.
3. Maximize your beacon chain up-time and performance.

#### Setup Instructions

Run the following to create a **unit file** to define your`beacon-chain.service` configuration.

```bash
cat > $HOME/beacon-chain.service << EOF
# The eth2 beacon chain service (part of systemd)
# file: /etc/systemd/system/beacon-chain.service 

[Unit]
Description     = eth2 beacon chain service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = /usr/bin/teku/bin/teku -c /etc/teku/teku.yaml
Restart         = on-failure
Environment     = JAVA_OPTS=-Xmx5g

[Install]
WantedBy    = multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system` and give it permissions.

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

Run the following to enable auto-start at boot time and then start your beacon node service.

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
Nice work. Your beacon chain is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

### 🛠 Some helpful systemd commands

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl -fu beacon-chain
```

```bash
#view log since yesterday
journalctl --unit=beacon-chain --since=yesterday
```

```bash
#view log since today
journalctl --unit=beacon-chain --since=today
```

```bash
#view log between a date
journalctl --unit=beacon-chain --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

#### ✅ Check whether the beacon chain is active

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 View the status of the beacon chain

```text
sudo systemctl status beacon-chain
```

#### 🔄 Restarting the beacon chain

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 Stopping the beacon chain

```text
sudo systemctl stop beacon-chain
```
{% endtab %}

{% tab title="Prysm" %}
{% hint style="info" %}
[Prysm](https://github.com/prysmaticlabs/prysm) is a Go implementation of Ethereum 2.0 protocol with a focus on usability, security, and reliability. Prysm is developed by [Prysmatic Labs](https://prysmaticlabs.com/), a company with the sole focus on the development of their client. Prysm is written in Go and released under a GPL-3.0 license.
{% endhint %}

## ⚙ 4.1. Install Prysm

```bash
mkdir ~/prysm && cd ~/prysm 
curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output prysm.sh && chmod +x prysm.sh
```

## 🔥 4.2. Configure port forwarding and/or firewall

Specific to your networking setup or cloud provider settings, [ensure your validator's firewall ports are open and reachable.](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Prysm beacon chain node** will use port 12000 for udp and port 13000 for tcp
* **eth1** node requires port 30303 for tcp and udp

{% hint style="info" %}
✨ **Port Forwarding Tip:** You'll need to forward and open ports to your validator. Verify it's working with [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) or [https://canyouseeme.org/](https://canyouseeme.org/) .
{% endhint %}

## 🎩 4.3. Import validator key

Accept terms of use, accept default wallet location, enter a new **prysm-only password** to encrypt your local prysm wallet and enter the **keystore password** for your imported accounts.

{% hint style="info" %}
If you wish, you can use the same password for the **keystore** and **prysm-only**.
{% endhint %}

```bash
$HOME/prysm/prysm.sh validator accounts import --prater --keys-dir=$HOME/eth2deposit-cli/validator_keys
```

Verify your validators imported successfully.

```bash
$HOME/prysm/prysm.sh validator accounts list --prater
```

Confirm your validator's pubkeys are listed.

> \#Example output:
>
> Showing 1 validator account View the eth1 deposit transaction data for your accounts by running \`validator accounts list --show-deposit-data
>
> Account 0 \| pens-brother-heat  
> \[validating public key\] 0x2374.....7121

{% hint style="danger" %}
**WARNING**: DO NOT USE THE ORIGINAL KEYSTORES TO VALIDATE WITH ANOTHER CLIENT, OR YOU WILL GET SLASHED.
{% endhint %}

## 🧬 4.4. Start the beacon chain

#### 🍰 Benefits of using systemd for your beacon chain and validator <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your beacon chain when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed beacon chain processes.
3. Maximize your beacon chain up-time and performance.

#### 🛠 Setup Instructions

Run the following to create a **unit file** to define your`beacon-chain.service` configuration.

```bash
cat > $HOME/beacon-chain.service << EOF 
# The eth2 beacon chain service (part of systemd)
# file: /etc/systemd/system/beacon-chain.service 

[Unit]
Description     = eth2 beacon chain service
Wants           = network-online.target
After           = network-online.target 

[Service]
Type            = simple
User            = $(whoami)
ExecStart       = $(echo $HOME)/prysm/prysm.sh beacon-chain --prater --p2p-max-peers=45 --monitoring-host="0.0.0.0" --http-web3provider=http://127.0.0.1:8545 --accept-terms-of-use 
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="info" %}
🔥 **Prysm Pro Tip:** On the **ExecStart** line, adding the `--fallback-web3provider` flag allows for a backup eth1 node. May repeat flag multiple times. Make sure the endpoint does not end with a trailing slash or`/` Remove it.

```bash
--fallback-web3provider=<http://<alternate eth1 provider one> --fallback-web3provider=<http://<alternate eth1 provider two>
# Example, repeat flag for multiple eth1 providers
# --fallback-web3provider=https://nodes.mewapi.io/rpc/eth --fallback-web3provider=https://mainnet.infura.io/v3/YOUR-PROJECT-ID
```

Sign up for free goerli ethereum fallback nodes at [https://infura.io/](https://infura.io/)
{% endhint %}

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

Give it permissions.

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

Run the following to enable auto-start at boot time and then start your beacon node service.

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
Nice work. Your beacon chain is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

###  🛠 Some helpful systemd commands <a id="some-helpful-systemd-commands-4"></a>

#### 🗄 Viewing and filtering logs <a id="viewing-and-filtering-logs-4"></a>

```bash
#view and follow the log
journalctl -fu beacon-chain
```

```bash
#view log since yesterday
journalctl --unit=beacon-chain --since=yesterday
```

```bash
#view log since today
journalctl --unit=beacon-chain --since=today
```

```bash
#view log between a date
journalctl --unit=beacon-chain --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

#### ​✅ Check whether the beacon chain is active <a id="check-whether-the-beacon-chain-is-active-3"></a>

```text
sudo systemctl is-active beacon-chain
```

#### ​🔎 View the status of the beacon chain <a id="view-the-status-of-the-beacon-chain-3"></a>

```text
sudo systemctl status beacon-chain
```

####  🔄 Restarting the beacon chain <a id="restarting-the-beacon-chain-3"></a>

```text
sudo systemctl reload-or-restart beacon-chain
```

####  🛑 Stopping the beacon chain <a id="stopping-the-beacon-chain-3"></a>

```text
sudo systemctl stop beacon-chain
```

## 🧬 4.5. Start the validator <a id="9-start-the-validator"></a>

Store your **prysm-only password** in a file and make it read-only. This is required so that Prysm can decrypt and load your validators.

```bash
echo 'my_prysm-only_password_goes_here' > $HOME/.eth2validators/validators-password.txt
sudo chmod 600 $HOME/.eth2validators/validators-password.txt
```

Clear the bash history in order to remove traces of keystore password.

```bash
shred -u ~/.bash_history && touch ~/.bash_history
```

#### 🚀 Setup Graffiti and POAP <a id="setup-graffiti-and-poap-3"></a>

Setup your `graffiti`, a custom message included in blocks your validator successfully proposes, and earn a POAP token. [Generate your POAP string by supplying an Ethereum 1.0 address here.](https://beaconcha.in/poap)​

Run the following command to set the `MY_GRAFFITI` variable. Replace `<my POAP string or message>` between the single quotes.

```bash
MY_GRAFFITI='<my POAP string or message>'
# Examples
# MY_GRAFFITI='poapAAAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

Learn more about [POAP - The Proof of Attendance token.](https://www.poap.xyz/)

Your choice of running a validator manually from command line or automatically with systemd.

#### 🍰 Benefits of using systemd for your validator <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your validator when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed validator processes.
3. Maximize your validator up-time and performance.

#### 🛠 Setup Instructions

Run the following to create a **unit file** to define your`validator.service` configuration.

```bash
cat > $HOME/validator.service << EOF 
# The eth2 validator service (part of systemd)
# file: /etc/systemd/system/validator.service 

[Unit]
Description     = eth2 validator service
Wants           = network-online.target beacon-chain.service
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/prysm/prysm.sh validator --prater --graffiti "${MY_GRAFFITI}" --accept-terms-of-use --wallet-password-file $(echo $HOME)/.eth2validators/validators-password.txt --enable-doppelganger
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system` 

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```

Run the following to enable auto-start at boot time and then start your validator.

```text
sudo systemctl daemon-reload
sudo systemctl enable validator
sudo systemctl start validator
```

###  🛠 Some helpful systemd commands

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl -fu validator
```

```bash
#view log since yesterday
journalctl --unit=validator --since=yesterday
```

```bash
#view log since today
journalctl --unit=validator --since=today
```

```bash
#view log between a date
journalctl --unit=validator --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

#### ✅ Check whether the validator is active

```text
sudo systemctl is-active validator
```

#### 🔎 View the status of the validator

```text
sudo systemctl status validator
```

#### 🔄 Restarting the validator

```text
sudo systemctl reload-or-restart validator
```

#### 🛑 Stopping the validator

```text
sudo systemctl stop validator
```

Verify that your **validator public key** appears in the logs.

```bash
journalctl --unit=validator --since=today
# Example below
# INFO Enabled validator       voting_pubkey: 0x2374.....7121
```
{% endtab %}

{% tab title="Lodestar" %}
{% hint style="info" %}
**Lodestar is a Typescript implementation** of the official [Ethereum 2.0 specification](https://github.com/ethereum/eth2.0-specs) by the [ChainSafe.io](https://lodestar.chainsafe.io/) team. In addition to the beacon chain client, the team is also working on 22 packages and libraries. A complete list can be found [here](https://hackmd.io/CcsWTnvRS_eiLUajr3gi9g). Finally, the Lodestar team is leading the Eth2 space in light client research and development and has received funding from the EF and Moloch DAO for this purpose.
{% endhint %}

## ⚙ 4.1 Build Lodestar from source

Install curl and git.

```bash
sudo apt-get install gcc g++ make git curl -y
```

Install yarn.

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install yarn -y
```

Confirm yarn is installed properly.

```bash
yarn --version
# Should output version >= 1.22.4
```

Install nodejs.

```text
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Confirm nodejs is installed properly.

```bash
nodejs -v
# Should output version >= v12.18.3
```

Install and build Lodestar.

```bash
cd ~/git
git clone https://github.com/chainsafe/lodestar.git
cd lodestar
yarn install --ignore-optional
yarn run build
```

{% hint style="info" %}
This build process may take a few minutes.
{% endhint %}

Verify Lodestar was installed properly by displaying the help menu.

```text
./lodestar --help
```

## 🔥 4.2. Configure port forwarding and/or firewall

Specific to your networking setup or cloud provider settings, [ensure your validator's firewall ports are open and reachable.](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Lodestar beacon chain node** will use port 30607 for tcp and port 9000 for udp peer discovery.
* **eth1** node requires port 30303 for tcp and udp

{% hint style="info" %}
 ✨ **Port Forwarding Tip:** You'll need to forward and open ports to your validator. Verify it's working with [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) or [https://canyouseeme.org/](https://canyouseeme.org/) .
{% endhint %}

## 🎩 4.3. Import validator key

```bash
./lodestar account validator import \
  --testnet prater \
  --directory $HOME/eth2deposit-cli/validator_keys
```

Enter your **keystore password** to import accounts.

Confirm your keys were imported properly.

```text
./lodestar account validator list --testnet prater
```

{% hint style="danger" %}
**WARNING**: DO NOT USE THE ORIGINAL KEYSTORES TO VALIDATE WITH ANOTHER CLIENT, OR YOU WILL GET SLASHED.
{% endhint %}

## 🛰 4.4. Start the beacon chain and validator

Run the beacon chain automatically with systemd.

#### 🍰 Benefits of using systemd for your beacon chain <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your beacon chain when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed beacon chain processes.
3. Maximize your beacon chain up-time and performance.

#### 🛠 Setup Instructions

Run the following to create a **unit file** to define your`beacon-chain.service` configuration.

```bash
cat > $HOME/beacon-chain.service << EOF 
# The eth2 beacon chain service (part of systemd)
# file: /etc/systemd/system/beacon-chain.service 

[Unit]
Description     = eth2 beacon chain service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
WorkingDirectory= $(echo $HOME)/git/lodestar
ExecStart       = $(echo $HOME)/git/lodestar/lodestar --testnet prater --eth1.providerUrl http://localhost:8545 --metrics.enabled true --metrics.serverPort 8008
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

Run the following to enable auto-start at boot time and then start your beacon node service.

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
Nice work. Your beacon chain is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

### 🛠 Some helpful systemd commands

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl -fu beacon-chain
```

```bash
#view log since yesterday
journalctl --unit=beacon-chain --since=yesterday
```

```bash
#view log since today
journalctl --unit=beacon-chain --since=today
```

```bash
#view log between a date
journalctl --unit=beacon-chain --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

#### ✅ Check whether the beacon chain is active

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 View the status of the beacon chain

```text
sudo systemctl status beacon-chain
```

#### 🔄 Restarting the beacon chain

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 Stopping the beacon chain

```text
sudo systemctl stop beacon-chain
```

## 🧬 4.5. Start the validator

#### ​🚀 Setup Graffiti and POAP <a id="setup-graffiti-and-poap-4"></a>

Setup your `graffiti`, a custom message included in blocks your validator successfully proposes, and earn a POAP token. [Generate your POAP string by supplying an Ethereum 1.0 address here.](https://beaconcha.in/poap)​

Run the following command to set the `MY_GRAFFITI` variable. Replace `<my POAP string or message>` between the single quotes.

```bash
MY_GRAFFITI='<my POAP string or message>'
# Examples
# MY_GRAFFITI='poapAAAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

Learn more about [POAP - The Proof of Attendance token.](https://www.poap.xyz/) ​

Run the validator automatically with systemd.

#### 🍰Benefits of using systemd for your validator <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your validator when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed validator processes.
3. Maximize your validator up-time and performance.

#### 🛠 Setup Instructions

Run the following to create a **unit file** to define your`validator.service` configuration.

```bash
cat > $HOME/validator.service << EOF 
# The eth2 validator service (part of systemd)
# file: /etc/systemd/system/validator.service 

[Unit]
Description     = eth2 validator service
Wants           = network-online.target beacon-chain.service
After           = network-online.target 

[Service]
User            = $(whoami)
WorkingDirectory= $(echo $HOME)/git/lodestar
ExecStart       = $(echo $HOME)/git/lodestar/lodestar validator --testnet prater --graffiti "${MY_GRAFFITI}"
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```

Run the following to enable auto-start at boot time and then start your validator.

```text
sudo systemctl daemon-reload
sudo systemctl enable validator
sudo systemctl start validator
```

{% hint style="success" %}
Nice work. Your validator is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

### 🛠 Some helpful systemd commands

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl -fu validator
```

```bash
#view log since yesterday
journalctl --unit=validator --since=yesterday
```

```bash
#view log since today
journalctl --unit=validator --since=today
```

```bash
#view log between a date
journalctl --unit=validator --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

#### ✅ Check whether the validator is active

```text
sudo systemctl is-active validator
```

#### 🔎 View the status of the validator

```text
sudo systemctl status validator
```

#### 🔄 Restarting the validator

```text
sudo systemctl reload-or-restart validator
```

#### 🛑 Stopping the validator

```text
sudo systemctl stop validator
```
{% endtab %}
{% endtabs %}

## 🕜 5. Time Synchronization

{% hint style="info" %}
Because beacon chain relies on accurate times to perform attestations and produce blocks, your computer's time must be accurate to real NTP or NTS time within 0.5 seconds.
{% endhint %}

Setup **Chrony** with the [following guide](https://www.coincashew.com/coins/overview-ada/guide-how-to-build-a-haskell-stakepool-node/how-to-setup-chrony).

{% hint style="info" %}
chrony is an implementation of the Network Time Protocol and helps to keep your computer's time synchronized with NTP.
{% endhint %}

{% hint style="warning" %}
Running multiple time synchronization services is known to cause issues. Ensure only either Chrony or only 1 NTP service is running.
{% endhint %}

## 🔎 6. Monitoring your validator with Grafana and Prometheus

Prometheus is a monitoring platform that collects metrics from monitored targets by scraping metrics HTTP endpoints on these targets. [Official documentation is available here.](https://prometheus.io/docs/introduction/overview/) Grafana is a dashboard used to visualize the collected data.

### 🐣 6.1 Installation

Install prometheus and prometheus node exporter.

```text
sudo apt-get install -y prometheus prometheus-node-exporter
```

Install grafana.

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" > grafana.list
sudo mv grafana.list /etc/apt/sources.list.d/grafana.list
sudo apt-get update && sudo apt-get install -y grafana
```

Enable services so they start automatically.

```bash
sudo systemctl enable grafana-server.service prometheus.service prometheus-node-exporter.service
```

Create the **prometheus.yml** config file. Choose the tab for your eth2 client. Simply copy and paste.

{% tabs %}
{% tab title="Lighthouse" %}
```bash
cat > $HOME/prometheus.yml << EOF
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
   - job_name: 'node_exporter'
     static_configs:
       - targets: ['localhost:9100']
   - job_name: 'nodes'
     metrics_path: /metrics    
     static_configs:
       - targets: ['localhost:5054']
   - job_name: 'validators'
     metrics_path: /metrics
     static_configs:
       - targets: ['localhost:5064']
EOF
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
cat > $HOME/prometheus.yml << EOF
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
   - job_name: 'node_exporter'
     static_configs:
       - targets: ['localhost:9100']
   - job_name: 'nodes'
     metrics_path: /metrics    
     static_configs:
       - targets: ['localhost:8008']
EOF
```
{% endtab %}

{% tab title="Teku" %}
```bash
cat > $HOME/prometheus.yml << EOF
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
   - job_name: 'node_exporter'
     static_configs:
       - targets: ['localhost:9100']
   - job_name: 'nodes'
     metrics_path: /metrics    
     static_configs:
       - targets: ['localhost:8008']
EOF
```
{% endtab %}

{% tab title="Prysm" %}
```bash
cat > $HOME/prometheus.yml << EOF
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
   - job_name: 'node_exporter'
     static_configs:
       - targets: ['localhost:9100']
   - job_name: 'validator'
     static_configs:
       - targets: ['localhost:8081']
   - job_name: 'beacon node'
     static_configs:
       - targets: ['localhost:8080']
   - job_name: 'slasher'
     static_configs:
       - targets: ['localhost:8082']
EOF
```
{% endtab %}

{% tab title="Lodestar" %}
```bash
cat > $HOME/prometheus.yml << EOF   
scrape_configs:
   - job_name: 'node_exporter'
     static_configs:
       - targets: ['localhost:9100']
   - job_name: 'Lodestar'
     metrics_path: /metrics    
     static_configs:
       - targets: ['localhost:8008']
EOF
```
{% endtab %}
{% endtabs %}

Setup prometheus for your **eth1 node**. Start by editing **prometheus.yml**

```bash
nano $HOME/prometheus.yml
```

Append the applicable job snippet for your eth1 node to the end of **prometheus.yml**. Save the file.

{% hint style="warning" %}
**Spacing matters**. Ensure all `job_name` snippets are in alignment.
{% endhint %}

{% tabs %}
{% tab title="Geth" %}
```bash
   - job_name: 'geth'
     scrape_interval: 15s
     scrape_timeout: 10s
     metrics_path: /debug/metrics/prometheus
     scheme: http
     static_configs:
     - targets: ['localhost:6060']
```
{% endtab %}

{% tab title="Besu" %}
```bash
   - job_name: 'besu'
     scrape_interval: 15s
     scrape_timeout: 10s
     metrics_path: /metrics
     scheme: http
     static_configs:
     - targets:
       - localhost:9545
```
{% endtab %}

{% tab title="Nethermind" %}
```bash
   - job_name: 'nethermind'
     scrape_interval: 15s
     scrape_timeout: 10s
     honor_labels: true
     static_configs:
       - targets: ['localhost:9091']
```

Nethermind monitoring requires [Prometheus Pushgateway](https://github.com/prometheus/pushgateway). Install with the following command.

```bash
sudo apt-get install -y prometheus-pushgateway
```

{% hint style="info" %}
Pushgateway listens for data from Nethermind on port 9091.
{% endhint %}
{% endtab %}

{% tab title="OpenEthereum" %}
```bash
   - job_name: 'openethereum'
     scrape_interval: 15s
     scrape_timeout: 10s
     metrics_path: /metrics
     scheme: http
     static_configs:
     - targets: ['localhost:6060']
```
{% endtab %}

{% tab title="Erigon" %}
```bash
   - job_name: 'erigon'
     scrape_interval: 10s
     scrape_timeout: 3s
     metrics_path: /debug/metrics/prometheus
     scheme: http
     static_configs:
       - targets: ['localhost:6060']
```
{% endtab %}
{% endtabs %}

Move it to `/etc/prometheus/prometheus.yml`

```bash
sudo mv $HOME/prometheus.yml /etc/prometheus/prometheus.yml
```

Update file permissions.

```bash
sudo chmod 644 /etc/prometheus/prometheus.yml
```

Finally, restart the services.

```bash
sudo systemctl restart grafana-server.service prometheus.service prometheus-node-exporter.service
```

Verify that the services are running properly:

```text
sudo systemctl status grafana-server.service prometheus.service prometheus-node-exporter.service
```

{% hint style="info" %}
💡 **Reminder**: Ensure port 3000 is open on the firewall and/or port forwarded if you intend to view monitoring info from a different machine.
{% endhint %}

### 📶 6.2 Setting up Grafana Dashboards

1. Open [http://localhost:3000](http://localhost:3000) or http://&lt;your validator's ip address&gt;:3000 in your local browser.
2. Login with **admin** / **admin**
3. Change password
4. Click the **configuration gear** icon, then **Add data Source**
5. Select **Prometheus**
6. Set **Name** to **"Prometheus**"
7. Set **URL** to [http://localhost:9090](http://localhost:9090)
8. Click **Save & Test**
9. **Download and save** your ETH2 Client's json file. More json dashboard options available below. \[ [Lighthouse](https://raw.githubusercontent.com/Yoldark34/lighthouse-staking-dashboard/main/Yoldark_ETH_staking_dashboard.json) \| [Teku ](https://grafana.com/api/dashboards/13457/revisions/2/download)\| [Nimbus ](https://raw.githubusercontent.com/status-im/nimbus-eth2/master/grafana/beacon_nodes_Grafana_dashboard.json)\| [Prysm ](https://raw.githubusercontent.com/GuillaumeMiralles/prysm-grafana-dashboard/master/less_10_validators.json)\| [Prysm &gt; 10 Validators](https://raw.githubusercontent.com/GuillaumeMiralles/prysm-grafana-dashboard/master/more_10_validators.json) \| Lodestar \] 
10. **Download and save** your ETH1 Client's json file \[ [Geth](https://gist.githubusercontent.com/karalabe/e7ca79abdec54755ceae09c08bd090cd/raw/3a400ab90f9402f2233280afd086cb9d6aac2111/dashboard.json) \| [Besu ](https://grafana.com/api/dashboards/10273/revisions/5/download)\| [Nethermind ](https://raw.githubusercontent.com/NethermindEth/metrics-infrastructure/master/grafana/dashboards/nethermind.json)\| [Erigon](https://raw.githubusercontent.com/ledgerwatch/erigon/devel/cmd/prometheus/dashboards/erigon.json) \| [OpenEthereum ](https://raw.githubusercontent.com/dappnode/DAppNodePackage-openethereum/master/openethereum-grafana-dashboard.json)\]
11. **Download and save** a [node-exporter dashboard](https://grafana.com/api/dashboards/11074/revisions/9/download) for general system monitoring
12. Click **Create +** icon &gt; **Import**
13. Add the ETH2 client's dashboard via **Upload JSON file**
14. If needed, select Prometheus as **Data Source**.
15. Click the **Import** button.
16. Repeat steps 12-15 for the ETH1 client dashboard.
17. Repeat steps 12-15 for the node-exporter dashboard.

{% hint style="info" %}
\*\*\*\*🔥 **Troubleshooting common Grafana issues**:

_The dashboards do not display eth1 node data._

* In the **eth1 unit file** under located at `/etc/systemd/system/eth1.service`,  make sure your eth1 node/geth is started with the correct parameters so that reporting metrics and pprof http server are enabled. 
  * Example: `ExecStartPre = /usr/bin/geth --http --metrics --pprof`
{% endhint %}

#### Example of Grafana Dashboards for each ETH2 client.

{% tabs %}
{% tab title="Lighthouse" %}
![Beacon chain dashboard by sigp](../../.gitbook/assets/lhm.png)

![Validator Client dashboard by sigp](../../.gitbook/assets/lighthouse-validator.png)

Beacon Chain JSON Download link: [https://raw.githubusercontent.com/sigp/lighthouse-metrics/master/dashboards/Summary.json](https://raw.githubusercontent.com/sigp/lighthouse-metrics/master/dashboards/Summary.json)

Validator Client JSON download link: [https://raw.githubusercontent.com/sigp/lighthouse-metrics/master/dashboards/ValidatorClient.json](https://raw.githubusercontent.com/sigp/lighthouse-metrics/master/dashboards/ValidatorClient.json)

Credits: [https://github.com/sigp/lighthouse-metrics/](https://github.com/sigp/lighthouse-metrics/)

![LH dashboard by Yoldark](../../.gitbook/assets/yoldark-lighthouse.png)

JSON Download link: [https://raw.githubusercontent.com/Yoldark34/lighthouse-staking-dashboard/main/Yoldark\_ETH\_staking\_dashboard.json](https://raw.githubusercontent.com/Yoldark34/lighthouse-staking-dashboard/main/Yoldark_ETH_staking_dashboard.json)

Credits: [https://github.com/Yoldark34/lighthouse-staking-dashboard](https://github.com/Yoldark34/lighthouse-staking-dashboard)
{% endtab %}

{% tab title="Nimbus" %}
![Dashboard by status-im](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-MLaFtX_4IFT3gsWUFJo%2F-MLaGFd6ZyPrECjDXjmm%2Fnim_dashboard.png?alt=media&token=390f67c9-534b-4fd5-a023-99834f66a6f7)

Credits: [https://github.com/status-im/nimbus-eth2/](https://github.com/status-im/nimbus-eth2/)​

![Nimbus dashboard by metanull-operator](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-MY1xtw2woRtAxaFYC7_%2F-MY20PiRgnpI3ViITbSW%2Feth2-grafana-dashboard-prysm.jpg?alt=media&token=8088df1a-3e86-4163-a47b-788b6a4d09ac)

JSON download link:

Credits: [https://github.com/metanull-operator/eth2-grafana/](https://github.com/metanull-operator/eth2-grafana/)
{% endtab %}

{% tab title="Teku" %}


![Teku by PegaSys Engineering](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-MMx2Q3_p75KOvwr8zmU%2F-MMx4rhP3N0_7z0tES08%2Fteku.dash.png?alt=media&token=2e0a90c9-cfc5-40d7-8485-8881fc276e8a)

Credits: [https://grafana.com/grafana/dashboards/13457](https://grafana.com/grafana/dashboards/13457)
{% endtab %}

{% tab title="Prysm" %}
![Prysm dashboard by GuillaumeMiralles](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-MLtAo4xmQIeMBUl4Wzz%2F-MLtCjxO392yO3NI4gFu%2Fprysm_dash.png?alt=media&token=03fb9d9a-a6c3-40c9-9ed9-a642595bf9ec)

Credits: [https://github.com/GuillaumeMiralles/prysm-grafana-dashboard](https://github.com/GuillaumeMiralles/prysm-grafana-dashboard)​

![Prysm dashboard by metanull-operator](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-MY1xtw2woRtAxaFYC7_%2F-MY20PiRgnpI3ViITbSW%2Feth2-grafana-dashboard-prysm.jpg?alt=media&token=8088df1a-3e86-4163-a47b-788b6a4d09ac)

JSON download link: [https://github.com/metanull-operator/eth2-grafana/raw/master/eth2-grafana-dashboard-single-source.json](https://github.com/metanull-operator/eth2-grafana/raw/master/eth2-grafana-dashboard-single-source.json)​

Credits: [https://github.com/metanull-operator/eth2-grafana/](https://github.com/metanull-operator/eth2-grafana/)
{% endtab %}

{% tab title="Lodestar" %}
Work in progress.
{% endtab %}
{% endtabs %}

#### Example of Grafana Dashboards for each ETH1 node.

{% tabs %}
{% tab title="Geth" %}
![Dashboard by karalabe](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-MNQ727XtpuSpc5rNv9T%2F-MNQ8xA4d1daR1qEezPP%2Fgeth-dash.png?alt=media&token=266a625f-f94d-4149-bccd-4766d2b71bde)

Credits: [https://gist.github.com/karalabe/e7ca79abdec54755ceae09c08bd090cd](https://gist.github.com/karalabe/e7ca79abdec54755ceae09c08bd090cd)
{% endtab %}

{% tab title="Besu" %}
![](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-MNQMiNKxWoPcnmx45Pz%2F-MNQOM8AjwyS1As7jZPY%2Fbesu-dash.png?alt=media&token=fe4aa1b1-808a-4432-ba66-57187949814d)

Credits: [https://grafana.com/dashboards/10273](https://grafana.com/dashboards/10273)
{% endtab %}

{% tab title="Nethermind" %}
![](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-MNQMiNKxWoPcnmx45Pz%2F-MNQOpWHG19DQ95eeIu8%2Fnethermind-dash.png?alt=media&token=30640066-cae8-40d2-b4ee-db7703470f00)

Credits: [https://github.com/NethermindEth/metrics-infrastructure](https://github.com/NethermindEth/metrics-infrastructure)
{% endtab %}

{% tab title="Erigon" %}
![](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-McIFGQr58950S1M17rL%2F-McIG-6X8EFEA_Qadgy0%2Ferigon-grafana.png?alt=media&token=37da3e98-43af-47ed-a2ad-95d321d50ae1)

Credits: [https://github.com/ledgerwatch/erigon/tree/devel/cmd/prometheus/dashboards](https://github.com/ledgerwatch/erigon/tree/devel/cmd/prometheus/dashboards)
{% endtab %}

{% tab title="OpenEthereum" %}
![Credits to dappnode](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-MQGWiFqY5Rf9x--yW0P%2F-MQGXfcYx2huFLikHG7d%2Fopenethereum-dashboard.png?alt=media&token=3dbd824c-501e-486d-b74a-d5cf3579f11e)
{% endtab %}
{% endtabs %}

#### Example of Node-Exporter Dashboard

{% tabs %}
{% tab title="Node-Exporter Dashboard by starsliao" %}
**General system monitoring**

Includes: CPU, memory, disk IO, network, temperature and other monitoring metrics.

![](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-MP6uHBGLt2-yzHS9Gpu%2F-MP6vagYpr_OYSaYWy3b%2Fnode-exporter.png?alt=media&token=ed51bbb8-653e-456e-bf6f-1e45f75ddc56)

![](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-MP6uHBGLt2-yzHS9Gpu%2F-MP6vh9VT1UsWCJWanik%2Fnode-exporter2.png?alt=media&token=23a67c44-3498-40ab-93f0-63a2849306f2)

Credits: [starsliao](https://grafana.com/grafana/dashboards/11074)
{% endtab %}
{% endtabs %}

### ⚠ 6.3 Setup Alert Notifications

{% hint style="info" %}
Setup alerts to get notified if your validators go offline.
{% endhint %}

Get notified of problems with your validators. Choose between email, telegram, discord or slack.

{% tabs %}
{% tab title="Email Notifications" %}
1. Visit [https://prater.beaconcha.in/](https://prater.beaconcha.in/)
2. Sign up for an account.
3. Verify your **email**
4. Search for your **validator's public address**
5. Add validators to your watchlist by clicking the **bookmark symbol**.
{% endtab %}

{% tab title="Telegram Notifications" %}
1. On the menu of Grafana, select **Notification channels** under the bell icon.  
2. Click on **Add channel**.
3. Give the notification channel a **name**.
4. Select **Telegram** from the Type list.
5. To complete the **Telegram API settings**, a **Telegram channel** and **bot** are required. For instructions on setting up a bot with `@Botfather`, see [this section](https://core.telegram.org/bots#6-botfather) of the Telegram documentation. You need to create a BOT API token.
6. Create a new telegram group.
7. Invite the bot to your new group.
8. Type at least 1 message into the group to initialize it.
9. Visit [`https://api.telegram.org/botXXX:YYY/getUpdates`](https://api.telegram.org/botXXX:YYY/getUpdates) where `XXX:YYY` is your BOT API Token.
10. In the JSON response, find and copy the **Chat ID**. Find it between **chat** and **title**. _Example of Chat ID_: `-1123123123`

    ```text
    "chat":{"id":-123123123,"title":
    ```

11. Paste the **Chat ID** into the corresponding field in **Grafana**.
12. **Save and test** the notification channel for your alerts.
13. Now you can create custom alerts from your dashboards. [Visit here to learn how to create alerts.](https://grafana.com/docs/grafana/latest/alerting/create-alerts/) 
{% endtab %}

{% tab title="Discord Notifications" %}
1. On the menu of Grafana, select **Notification channels** under the bell icon.  
2. Click on **Add channel**.
3. Add a **name** to the notification channel.
4. Select **Discord** from the Type list.
5. To complete the set up, a Discord server \(and a text channel available\) as well as a Webhook URL are required. For instructions on setting up a Discord's Webhooks, see [this section](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks) of their documentation.
6. Enter the Webhook **URL** in the Discord notification settings panel.
7. Click **Send Test**, which will push a confirmation message to the Discord channel.
{% endtab %}

{% tab title="Slack Notifications" %}
1. On the menu of Grafana, select **Notification channels** under the bell icon.  
2. Click on **Add channel**.
3. Add a **name** to the notification channel.
4. Select **Slack** from the Type list.
5. For instructions on setting up a Slack's Incoming Webhooks, see [this section](https://api.slack.com/messaging/webhooks) of their documentation.
6. Enter the Slack Incoming Webhook URL in the **URL** field.
7. Click **Send Test**, which will push a confirmation message to the Slack channel.
{% endtab %}
{% endtabs %}

### 🌊 6.4 Monitoring with Uptime Check by Google Cloud

{% hint style="info" %}
Who watches the watcher? With an external 3rd party tool like Uptime Check, you can have greater reassurance your validator is functioning in case of disasters such as power failure, hardware failure or internet outage. In these scenarios, the previously mentioned monitoring by Prometheus and Grafana would likely cease to function as well.

Credits to [Mohamed Mansour for inspiring this how-to guide](https://www.youtube.com/watch?v=txgOVDTemPQ).
{% endhint %}

Here's how to setup a no-cost monitoring service called Uptime Check by Google.

{% hint style="info" %}
For a video demo, watch [MohamedMansour's eth2 education videos](https://www.youtube.com/watch?v=txgOVDTemPQ). Please support his [GITCOIN grant](https://gitcoin.co/grants/1709/video-educational-grant). 🙏 
{% endhint %}

1. Visit [cloud.google.com](https://cloud.google.com/)
2. Search for **Monitoring** in the search field.
3. Click **Select a Project to Start Monitoring**.
4. Click **New Project.**
5. **Name** your project and click **Create.**
6. From the notifications menu, select your new project.
7. On the right column, there's a Monitoring Card. Click **Go to Monitoring**.
8. On the left menu, click **Uptime checks** and then **CREATE UPTIME CHECK.**
9. Type in a title i.e. _**Geth node**_ 
10. Select protocol as _**TCP**_
11. Enter your public IP address and port number. i.e. ip=**7.55.6.3** and port=**30303**
12. Select your desired frequency to check i.e. **5 minutes.**
13. Choose the region closest to you to check from. Click Next.
14. Create a Notification Channel. Click **Manage Notification Channels.**
15. Choose your desired settings. Pick from any or all of Slack, Webhook, Email or SMS
16. Go back to Create Uptime Check window. 
17. Within the notifications field, click the refresh button to load your new notification channels.
18. Select desired notifications.
19. Click **TEST** to verify your notifications are setup correctly.
20. Click **CREATE** to finish.

### ​ 📱 6.5 Mobile App Node Monitoring by beaconcha.in <a id="6-5-mobile-app-node-monitoring-by-beaconcha-in"></a>

Learn how to monitor your validator & beacon node on the [beaconcha.in mobile app.](https://beaconcha.in/mobile)​

{% hint style="info" %}
This feature currently works for Lighthouse, Nimbus and Prysm.
{% endhint %}

Refer to the official guide found here: [https://kb.beaconcha.in/beaconcha.in-explorer/mobile-app-less-than-greater-than-beacon-node](https://kb.beaconcha.in/beaconcha.in-explorer/mobile-app-less-than-greater-than-beacon-node)​

![beaconcha.in mobile app monitoring](https://gblobscdn.gitbook.com/assets%2F-M5KYnWuA6dS_nKYsmfV%2F-McIFGQr58950S1M17rL%2F-McIHFLjf-2V_mF_8ZqL%2Fgrafik.png?alt=media&token=317dd85a-42af-4efd-83bc-e0e5c078a630)

{% hint style="success" %}
Once your beacon chain is sync'd, validator up and running, you just wait for activation. This process can take 24+ hours. When you're assigned, your validator will begin creating and voting on blocks while earning staking rewards.

Use [https://prater.beaconcha.in/](https://prater.beaconcha.in/) to create alerts and track your validator's performance.
{% endhint %}

{% hint style="info" %}
Be sure to review the [Checklist \| How to confirm a healthy functional ETH2 validator.](guide-or-how-to-setup-a-validator-on-eth2-mainnet/checklist-or-how-to-confirm-a-healthy-functional-eth2-validator.md)
{% endhint %}

{% hint style="success" %}
🎊 Congrats on setting up your testnet validator! Once comfortable on testnet for a few weeks, you're good to go with staking on mainnet.

Did you find our guide useful? Send us a signal with a tip and we'll keep updating it.

Use [cointr.ee to find our donation ](https://cointr.ee/coincashew)addresses. 🙏 

Any feedback and all pull requests much appreciated. 🌛 

Hang out and chat with fellow stakers on Discord @ [https://discord.gg/w8Bx8W2HPW](https://discord.gg/w8Bx8W2HPW)
{% endhint %}

## 🧙♂ 7. Update a ETH2 client

When a new release is cut, you will want to update to the latest stable release. The following shows you how to update your eth2 beacon chain and validator.

{% hint style="info" %}
Always review the **git logs with command`git log`** or **release notes** before updating. There may be changes requiring your attention.
{% endhint %}

{% hint style="success" %}
\*\*\*\*🔥 **Pro tip**: Plan your update to overlap with the longest attestation gap. [Learn how here.](guide-or-how-to-setup-a-validator-on-eth2-mainnet/how-to-find-longest-attestation-slot-gap.md)
{% endhint %}

Select your ETH2 client.

{% tabs %}
{% tab title="Lighthouse" %}
Review release notes and check for breaking changes/features.

[https://github.com/sigp/lighthouse/releases](https://github.com/sigp/lighthouse/releases)

Pull the latest source and build it.

```bash
cd $HOME/git/lighthouse
git fetch --all && git checkout stable && git pull
make
```

{% hint style="info" %}
In case of compilation errors, update Rust with the following sequence.

```text
rustup update
cargo clean
make
```
{% endhint %}

Verify the build completed by checking the new version number.

```bash
lighthouse --version
```

Restart beacon chain and validator as per normal operating procedures.

```text
sudo systemctl reload-or-restart beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus" %}
Review release notes and check for breaking changes/features.

[https://github.com/status-im/nimbus-eth2/releases](https://github.com/status-im/nimbus-eth2/releases)

Pull the latest source and build it.

```bash
cd $HOME/git/nimbus-eth2
git pull && make update
make nimbus_beacon_node
```

Verify the build completed by checking the new version number.

```bash
cd $HOME/git/nimbus-eth2/build
./nimbus_beacon_node --version
```

Stop, copy new binary, and restart beacon chain and validator as per normal operating procedures.

```bash
sudo systemctl stop beacon-chain
sudo rm /usr/bin/nimbus_beacon_node
sudo cp $HOME/git/nimbus-eth2/build/nimbus_beacon_node /usr/bin
sudo systemctl reload-or-restart beacon-chain
```
{% endtab %}

{% tab title="Teku" %}
Review release notes and check for breaking changes/features.

[https://github.com/ConsenSys/teku/releases](https://github.com/ConsenSys/teku/releases)

Pull the latest source and build it.

```bash
cd $HOME/git/teku
git pull
./gradlew distTar installDist
```

Verify the build completed by checking the new version number.

```bash
cd $HOME/git/teku/build/install/teku/bin
./teku --version
```

Restart beacon chain and validator as per normal operating procedures.

```bash
sudo systemctl stop beacon-chain
sudo rm -rf /usr/bin/teku
sudo cp -r $HOME/git/teku/build/install/teku /usr/bin/teku
sudo systemctl reload-or-restart beacon-chain
```
{% endtab %}

{% tab title="Prysm" %}
Review release notes and check for breaking changes/features. [https://github.com/prysmaticlabs/prysm/releases](https://github.com/prysmaticlabs/prysm/releases)

```bash
#Simply restart the processes
sudo systemctl reload-or-restart beacon-chain validator
```
{% endtab %}

{% tab title="Lodestar" %}
Review release notes and check for breaking changes/features.

[https://github.com/ChainSafe/lodestar/releases](https://github.com/ChainSafe/lodestar/releases)

Pull the latest source and build it.

```bash
cd $HOME/git/lodestar
git pull
yarn install --ignore-optional
yarn run build
```

Verify the build completed by checking the new version number.

```bash
./lodestar --version
```

Restart beacon chain and validator as per normal operating procedures.

```text
sudo systemctl reload-or-restart beacon-chain validator
```
{% endtab %}
{% endtabs %}

Check the logs to verify the services are working properly and ensure there are no errors.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl status beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```text
sudo systemctl status beacon-chain
```
{% endtab %}
{% endtabs %}

## 🔥 8. Additional Useful Tips

### 🛑 8.1 Voluntary exit a validator

{% hint style="info" %}
Use this command to signal your intentions to stop validating with your validator. This means you no longer want to stake with your validator and want to turn off your node.

* Voluntary exiting takes a minimum of 2048 epochs \(or ~9days\). There is a queue to exit and a delay before your validator is finally exited.
* Once a validator is exited in phase 0, this is non-reversible and you can no longer restart validating again. 
* Your funds will not be available for withdrawal until phase 1.5 or later. 
* After your validator leaves the exit queue and is truely exited, it is safe to turn off your beacon node and validator.
{% endhint %}

{% tabs %}
{% tab title="Lighthouse" %}
```bash
lighthouse account validator exit \
--keystore $HOME/.lighthouse/prater/validators/<0x validator>/<keystore.json file> \
--beacon-node http://localhost:5052 \
--network prater
```
{% endtab %}

{% tab title="Teku" %}
```bash
teku voluntary-exit \
--epoch=<epoch number to exit> \
--beacon-node-api-endpoint=http://127.0.0.1:5051 \
--validator-keys=<path to keystore.json>:<path to password.txt file>
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
build/nimbus_beacon_node deposits exit --validator=<VALIDATOR_PUBLIC_KEY> --data-dir=/var/lib/nimbus
```
{% endtab %}

{% tab title="Prysm" %}
```bash
$HOME/prysm/prysm.sh validator accounts voluntary-exit
```
{% endtab %}

{% tab title="Lodestar" %}
```bash
./lodestar account validator voluntary-exit --publicKey 0xF00
```
{% endtab %}
{% endtabs %}

### 🗝 8.2 Verify your mnemonic phrase

Using the eth2deposit-cli tool, ensure you can regenerate the same eth2 key pairs by restoring your `validator_keys`

```bash
cd $HOME/eth2deposit-cli 
./deposit.sh existing-mnemonic --chain prater
```

{% hint style="info" %}
When the **pubkey** in both **keystore files** are **identical,** this means your mnemonic phrase is veritably correct. Other fields will be different because of salting.
{% endhint %}

### 🤖 8.3 Add additional validators

Backup and move your existing `validator_key` directory and append the date to the end.

```bash
# Adjust your eth2deposit-cli directory accordingly
cd $HOME/eth2deposit-cli
# Renames and append the date to the existing validator_key directory
mv validator_key validator_key_$(date +"%Y%d%m-%H%M%S")
# Optional: you can also delete this folder since it can be regenerated.
```

{% hint style="info" %}
Using the eth2deposit-cli tool, you can add more validators by creating a new deposit data file and `validator_keys`
{% endhint %}

1. For example, in case we originally created **3 validators** but now wish to **add 5 more validators**, we could use the following command. Select the tab depending on how you acquired [**eth2deposit tool**](https://github.com/ethereum/eth2.0-deposit-cli).

{% hint style="warning" %}
**Security recommendation reminder**: For best security practices, key management and other activities where you type your 24 word mnemonic seed should be completed on an air-gapped offline cold machine booted from USB drive.
{% endhint %}

{% hint style="danger" %}
Reminder to use the same **keystore password.**
{% endhint %}

{% tabs %}
{% tab title="Build from source code" %}
```bash
# Generate from an existing mnemonic 5 more validators when 3 were previously already made
./deposit.sh existing-mnemonic --validator_start_index 3 --num_validators 5 --chain prater
```
{% endtab %}

{% tab title="Pre-built eth2deposit-cli binaries" %}
```bash
# Generate from an existing mnemonic 5 more validators when 3 were previously already made
./deposit existing-mnemonic --validator_start_index 3 --num_validators 5 --chain prater
```
{% endtab %}

{% tab title="Advanced - Most Secure" %}
{% hint style="warning" %}
🔥**Pro Security Tip**: Run the **eth2deposit-cli tool** and generate your **mnemonic seed** for your validator keys on an **air-gapped offline machine booted from usb**.
{% endhint %}

Follow this [ethstaker.cc](https://ethstaker.cc/) exclusive for the low down on making a bootable usb.

### Part 1 - Create a Ubuntu 20.04 USB Bootable Drive

{% embed url="https://www.youtube.com/watch?v=DTR3PzRRtYU" %}

### Part 2 - Install Ubuntu 20.04 from the USB Drive

{% embed url="https://www.youtube.com/watch?v=C97\_6MrufCE" %}

You can copy via USB key the pre-built eth2deposit-cli binaries from an online machine to an air-gapped offline machine booted from usb. Make sure to disconnect the ethernet cable and/or WIFI.

Run the existing-mnemonic command in the previous tabs.
{% endtab %}
{% endtabs %}

3. Complete the steps of uploading the `deposit_data-#########.json` to the [prater Eth2 launch pad site](https://prater.launchpad.ethereum.org/en/) and making your corresponding 32 ETH deposit transactions.

Run the existing-mnemonic command in the previous tabs.

1. Complete the steps of uploading the `deposit_data-#########.json` to the [prater Eth2 launch pad site](https://prater.launchpad.ethereum.org/en/) and making your corresponding 32 ETH deposit transactions.
2. Finish by stopping your validator, importing the new validator key\(s\), restarting your validator and verifying the logs ensuring everything still works without error. Review steps 2 and onward of the main guide if you need a refresher.
3. Finally, verify your **existing** validator's attestations are working with public block explorer such as

[https://prater.beaconcha.in/](https://prater.beaconcha.in/)

Enter your validator's pubkey to view its status.

{% hint style="info" %}
Your additional validators are now in the activation queue waiting their turn.
{% endhint %}

### 🛰 8.4 Switch / change eth2 clients with slash protection

{% hint style="info" %}
The key takeaway in this process is to avoid running two eth2 clients simultaneously. You want to avoid being punished by a slashing penalty, which causes a loss of ether.
{% endhint %}

#### 🛑 8.4.1 Stop old beacon chain and old validator.

In order to export the slashing database, the validator needs to be stopped.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl stop beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```text
sudo systemctl stop beacon-chain
```
{% endtab %}
{% endtabs %}

#### 💽 8.4.2 Export slashing database \(Optional\)

{% hint style="info" %}
[EIP-3076](https://eips.ethereum.org/EIPS/eip-3076) implements a standard to safety migrate validator keys between eth2 clients. This is the exported contents of the slashing database.
{% endhint %}

Update the export .json file location and name.

{% tabs %}
{% tab title="Lighthouse" %}
```bash
lighthouse account validator slashing-protection export <lighthouse_interchange.json>
```
{% endtab %}

{% tab title="Nimbus" %}
To be implemented
{% endtab %}

{% tab title="Teku" %}
```bash
teku slashing-protection export --to=<FILE>
```
{% endtab %}

{% tab title="Prysm" %}
```bash
prysm.sh validator slashing-protection export --datadir=/path/to/your/wallet --slashing-protection-export-dir=/path/to/desired/outputdir
```
{% endtab %}

{% tab title="Lodestar" %}
```bash
./lodestar account validator slashing-protection export --network prater --file interchange.json
```
{% endtab %}
{% endtabs %}

#### 🚧 8.4.3 Setup and install new validator / beacon chain

Now you need to setup/install your new validator **but do not start running the systemd processes**. Be sure to thoroughly follow your new validator's instructions under section 4. You will need to build/install the client, configure port forwarding/firewalls, and new systemd unit files.

{% hint style="warning" %}
🔥 **Pro Tip**: During the process of re-importing validator keys, **wait at least 13 minutes** or two epochs to prevent slashing penalties. You must avoid running two eth2 clients with same validator keys at the same time.
{% endhint %}

{% hint style="danger" %}
🛑 **Critical Step**: Do not start any **systemd processes** until either you have **imported the slashing database** or you have **waited at least 13 minutes or two epochs**.
{% endhint %}

#### 🌱 8.4.4 Import slashing database \(Optional\)

Using your new eth2 client, run the following command and update the relevant path to import your slashing database from 2 steps ago.

{% tabs %}
{% tab title="Lighthouse" %}
```bash
lighthouse account validator slashing-protection import <my_interchange.json>
```
{% endtab %}

{% tab title="Nimbus" %}
To be implemented
{% endtab %}

{% tab title="Teku" %}
```bash
teku slashing-protection import --from=<FILE>
```
{% endtab %}

{% tab title="Prysm" %}
```bash
prysm.sh validator slashing-protection import --datadir=/path/to/your/wallet --slashing-protection-json-file=/path/to/desiredimportfile
```
{% endtab %}

{% tab title="Lodestar" %}
```bash
./lodestar account validator slashing-protection import --network prater --file interchange.json
```
{% endtab %}
{% endtabs %}

#### 🚀 8.4.5 Start new validator and new beacon chain

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl start beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```text
sudo systemctl start beacon-chain
```
{% endtab %}
{% endtabs %}

#### 🔥 8.4.6 Verify functionality

Check the logs to verify the services are working properly and ensure there are no errors.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl status beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```text
sudo systemctl status beacon-chain
```
{% endtab %}
{% endtabs %}

Finally, verify your validator's attestations are working with public block explorer such as

[https://prater.beaconcha.in/](https://prater.beaconcha.in/)

Enter your validator's pubkey to view its status.

#### 📶 8.4.7 Update Monitoring with Prometheus and Grafana

Review section 6 and change your `prometheus.yml`. Ensure prometheus is connected to your new eth2 client's metrics port. You will also want to import your new eth2 client's dashboard.

### 💻 8.5 Use all available LVM disk space

During installation of Ubuntu Server, a common issue arises where your hard drive's space is not fully available for use.

```bash
# View your disk drives
sudo -s lvm

# Change the logical volume filesystem path if required
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv

#exit lvextend
exit

# Resize file system to use the new available space in the logical volume
resize2fs /dev/ubuntu-vg/ubuntu-lv

## Verify new available space
df -h

# Example output of a 2TB drive where 25% is used
# Filesystem                         Size   Used Avail Use% Mounted on
# /dev/ubuntu-vg/ubuntu-lv           2000G  500G  1500G  25% /
```

**Source reference**:

{% embed url="https://askubuntu.com/questions/1106795/ubuntu-server-18-04-lvm-out-of-space-with-improper-default-partitioning" caption="" %}

### 🚦8.6 Reduce network bandwidth usage

{% hint style="info" %}
Hosting your own ETH1 node can consume hundreds of gigabytes of data per day. Because data plans can be limited or costly, you might desire to slow down data usage but still maintain good connectivity to the network.
{% endhint %}

Edit your eth1.service unit file.

```bash
sudo nano /etc/systemd/system/eth1.service
```

Add the following flag to limit the number of peers on the `ExecStart` line.

{% tabs %}
{% tab title="Geth" %}
```bash
--maxpeers 10
# Example
# ExecStart       = /usr/bin/geth --maxpeers 10 --http --goerli --ws
```
{% endtab %}

{% tab title="OpenEthereum \(Parity\)" %}
```bash
--max-peers 10
# Example
# ExecStart       = $(echo $HOME)/openethereum/openethereum --max-peers 10 --chain goerli
```
{% endtab %}

{% tab title="Besu" %}
```bash
--max-peers 10
# Example
# ExecStart       = <home directory>/besu/bin/besu --max-peers 10 --rpc-http-enabled --network=goerli
```
{% endtab %}

{% tab title="Nethermind" %}
```bash
--Network.ActivePeersMaxCount 10
# Example
# ExecStart       = <home directory>/nethermind/Nethermind.Runner --Network.ActivePeersMaxCount 10 --config goerli --JsonRpc.Enabled true
```
{% endtab %}

{% tab title="Erigon" %}
```
--maxpeers 10
```
{% endtab %}
{% endtabs %}

Finally, reload the new unit file and restart the eth1 node.

```bash
sudo systemctl daemon-reload
sudo systemctl restart eth1
```

### 📁 8.7 Important directory locations

{% hint style="info" %}
In case you need to locate your validator keys or database directories.
{% endhint %}

#### Eth2 Client files and locations

{% tabs %}
{% tab title="Lighthouse" %}
```bash
# Validator Keys
~/.lighthouse/prater/validators

# Beacon Chain Data
~/.lighthouse/prater/beacon

# List of all validators and passwords
~/.lighthouse/prater/validators/validator_definitions.yml

#Slash protection db
~/.lighthouse/prater/validators/slashing_protection.sqlite
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
# Validator Keys
/var/lib/nimbus/validators

# Beacon Chain Data
/var/lib/nimbus/db

#Slash protection db
/var/lib/nimbus/validators/slashing_protection.sqlite3

#Logs
/var/lib/nimbus/beacon.log
```
{% endtab %}

{% tab title="Teku" %}
```bash
# Validator Keys
/var/lib/teku

# Beacon Chain Data
~/tekudata/beacon

#Slash protection db
~/tekudata/validator/slashprotection
```
{% endtab %}

{% tab title="Prysm" %}
```bash
# Validator Keys
~/.eth2validators/prysm-wallet-v2/direct

# Beacon Chain Data
~/.eth2/beaconchaindata
```
{% endtab %}

{% tab title="Lodestar" %}
```bash
# Validator Keystores
$rootDir/keystores

# Validator Secrets
$rootDir/secrets

# Validator DB Data
$rootDir/validator-db
```
{% endtab %}
{% endtabs %}

#### Eth1 node files and locations

{% tabs %}
{% tab title="Geth" %}
```bash
# database location
$HOME/.ethereum
```
{% endtab %}

{% tab title="OpenEthereum \(Parity\)" %}
```bash
# database location
$HOME/.local/share/openethereum
```
{% endtab %}

{% tab title="Besu" %}
```bash
# database location
$HOME/.besu/database
```
{% endtab %}

{% tab title="Nethermind" %}
```bash
#database location
$HOME/.nethermind/nethermind_db/goerli
```
{% endtab %}

{% tab title="Erigon" %}
```bash
#database location
/var/lib/erigon
```
{% endtab %}
{% endtabs %}

###  🌍 8.8 Hosting ETH1 node on a different machine

{% hint style="info" %}
Hosting your own ETH1 node on a different machine than where your beacon-chain and validator resides, can allow some extra modularity and flexibility.
{% endhint %}

On the eth1 node machine, edit your eth1.service unit file.

```bash
sudo nano /etc/systemd/system/eth1.service
```

Add the following flag to allow remote incoming http and or websocket api requests on the `ExecStart` line.

{% hint style="info" %}
If not using websockets, there's no need to include ws parameters. Only Nimbus requires websockets.
{% endhint %}

{% tabs %}
{% tab title="Geth" %}
```bash
--http.addr 0.0.0.0 --ws.addr 0.0.0.0
# Example
# ExecStart       = /usr/bin/geth --http.addr 0.0.0.0 --ws.addr 0.0.0.0 --http --ws
```
{% endtab %}

{% tab title="OpenEthereum \(Parity\)" %}
```bash
--jsonrpc-interface=all --ws-interface=all
# Example
# ExecStart       = <home directory>/openethereum/openethereum --jsonrpc-interface=all --ws-interface=all
```
{% endtab %}

{% tab title="Besu" %}
```bash
--rpc-http-host=0.0.0.0 --rpc-ws-enabled --rpc-ws-host=0.0.0.0
# Example
# ExecStart       = <home directory>/besu/bin/besu --rpc-http-host=0.0.0.0 --rpc-ws-enabled --rpc-ws-host=0.0.0.0 --rpc-http-enabled
```
{% endtab %}

{% tab title="Nethermind" %}
```bash
--JsonRpc.Host 0.0.0.0 --WebSocketsEnabled
# Example
# ExecStart       = <home directory>/nethermind/Nethermind.Runner --JsonRpc.Host 0.0.0.0 --WebSocketsEnabled --JsonRpc.Enabled true
```
{% endtab %}
{% endtabs %}

Reload the new unit file and restart the eth1 node.

```bash
sudo systemctl daemon-reload
sudo systemctl restart eth1
```

On the separate machine hosting the beacon-chain, update the beacon-chain unit file with the eth1 node's IP address.

{% tabs %}
{% tab title="Lighthouse" %}
```bash
# edit beacon-chain unit file
nano /etc/systemd/system/beacon-chain.service
# add the --eth1-endpoints parameter
# example
# --eth1-endpoints=http://192.168.10.22
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
# edit beacon chain unit file
nano /etc/systemd/system/beacon-chain.service
# modify the --web-url parameter
# example
# --web3-url=ws://192.168.10.22
```
{% endtab %}

{% tab title="Teku" %}
```bash
# edit teku.yaml
nano /etc/teku/teku.yaml
# change the eth1-endpoint
# example
# eth1-endpoint: "http://192.168.10.20:8545"
```
{% endtab %}

{% tab title="Prysm" %}
```bash
# edit beacon-chain unit file
nano /etc/systemd/system/beacon-chain.service
# add the --http-web3provider parameter
# example
# --http-web3provider=http://192.168.10.20:8545
```
{% endtab %}

{% tab title="Lodestar" %}
```bash
# edit beacon-chain unit file
nano /etc/systemd/system/beacon-chain.service
# add the --eth1.providerUrl parameter
# example
# --eth1.providerUrl http://192.168.10.20:8545
```
{% endtab %}
{% endtabs %}

Reload the updated unit file and restart the beacon-chain.

```bash
sudo systemctl daemon-reload
sudo systemctl restart beacon-chain
```

### 🎊 8.9 Add or change POAP graffiti flag

Setup your `graffiti`, a custom message included in blocks your validator successfully proposes, and earn an early beacon chain validator POAP token. [Generate your POAP string by supplying an Ethereum 1.0 address here.](https://beaconcha.in/poap)

{% hint style="info" %}
Learn more about [POAP - The Proof of Attendance token. ](https://www.poap.xyz/)
{% endhint %}

{% tabs %}
{% tab title="Lighthouse" %}
Change the graffiti value on ExecStart line. Save your file.

```bash
sudo nano /etc/systemd/system/validator.service
```
{% endtab %}

{% tab title="Nimbus" %}
Change the graffiti value on ExecStart line. Save your file.

```bash
sudo nano /etc/systemd/system/beacon-chain.service
```
{% endtab %}

{% tab title="Teku" %}
Change the **validators-graffiti** value. Save your file.

```bash
sudo nano /etc/teku/teku.yaml
```
{% endtab %}

{% tab title="Prysm" %}
Change the graffiti value on ExecStart line. Save your file.

```bash
sudo nano /etc/systemd/system/validator.service
```
{% endtab %}

{% tab title="Lodestar" %}
Change the graffiti value on ExecStart line. Save your file.

```bash
sudo nano /etc/systemd/system/validator.service
```
{% endtab %}
{% endtabs %}

Reload the updated unit file and restart the validator process for your graffiti to take effect.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl daemon-reload
sudo systemctl restart validator
```
{% endtab %}

{% tab title="Teku \| Nimbus" %}
```text
sudo systemctl daemon-reload
sudo systemctl restart beacon-chain
```
{% endtab %}
{% endtabs %}

### 📦8.10 Update a ETH1 node - Geth / OpenEthereum / Besu / Nethermind / Erigon

{% hint style="info" %}
From time to time, be sure to update to the latest ETH1 releases to enjoy new improvements and features.
{% endhint %}

{% hint style="success" %}
\*\*\*\*🔥 **Pro tip**: Setup a failover eth1 node to overlap your maintenance. See section 8.11, strategy 2, eth1 redundancy.
{% endhint %}

Update your operating system and ensure it's on the latest long term \(LTS\) support version.

```bash
sudo apt update
sudo apt dist-upgrade -y
```

Stop your eth1 node process.

```bash
# This can take a few minutes.
sudo systemctl stop eth1
```

Update the eth1 node package or binaries.

{% tabs %}
{% tab title="Geth" %}
Review the latest release notes at [https://github.com/ethereum/go-ethereum/releases](https://github.com/ethereum/go-ethereum/releases)

```bash
# Already handled by previous commands.
# sudo apt update
# sudo apt dist-upgrade -y
```
{% endtab %}

{% tab title="OpenEthereum \(Parity\)" %}
Review the latest release at [https://github.com/openethereum/openethereum/releases](https://github.com/openethereum/openethereum/releases)

Automatically download the latest linux release, un-zip, add execute permissions and cleanup.

```bash
cd $HOME
# backup previous openethereum version in case of rollback
mv openethereum openethereum_backup_$(date +"%Y%d%m-%H%M%S")
# store new version in openethreum directory
mkdir openethereum && cd openethereum
# download latest version
curl -s https://api.github.com/repos/openethereum/openethereum/releases/latest | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux  | xargs wget -q --show-progress
# unzip
unzip openethereum*.zip
# add execute permission
chmod +x openethereum
# cleanup
rm openethereum*.zip
```
{% endtab %}

{% tab title="Besu" %}
Review the latest release at [https://github.com/hyperledger/besu/releases](https://github.com/hyperledger/besu/releases)

File can be downloaded from [https://dl.bintray.com/hyperledger-org/besu-repo](https://dl.bintray.com/hyperledger-org/besu-repo)

Manually find the desired file from above repo and modify the `wget` command with the URL.

> Example:
>
> wget -O besu.tar.gz [https://dl.bintray.com/hyperledger-org/besu-repo/besu-20.10.1.tar.gz](https://dl.bintray.com/hyperledger-org/besu-repo/besu-20.10.1.tar.gz)

```bash
cd $HOME
# backup previous besu version in case of rollback
mv besu besu_backup_$(date +"%Y%d%m-%H%M%S")
# download latest besu
wget -O besu.tar.gz <https URL to latest tax.gz linux file>
# untar
tar -xvf besu.tar.gz
# cleanup
rm besu.tar.gz
# rename besu to standard folder location
mv besu* besu
```
{% endtab %}

{% tab title="Nethermind" %}
Review the latest release at [https://github.com/NethermindEth/nethermind/releases](https://github.com/NethermindEth/nethermind/releases)

Automatically download the latest linux release, un-zip and cleanup.

```bash
cd $HOME
# backup previous nethermind version in case of rollback
mv nethermind nethermind_backup_$(date +"%Y%d%m-%H%M%S")
# store new version in nethermind directory
mkdir nethermind && cd nethermind 
# download latest version
curl -s https://api.github.com/repos/NethermindEth/nethermind/releases/latest | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux  | xargs wget -q --show-progress
# unzip
unzip -o nethermind*.zip
# cleanup
rm nethermind*linux*.zip
```
{% endtab %}

{% tab title="Erigon" %}
Review the latest release at [https://github.com/ledgerwatch/erigon/releases](https://github.com/ledgerwatch/erigon/releases)

```bash
cd $HOME/erigon
git pull
make erigon && make rpcdaemon
```
{% endtab %}
{% endtabs %}

Start your eth1 node process.

```bash
sudo systemctl start eth1
```

Check the logs to verify the services are working properly and ensure there are no errors.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl status eth1 status beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```text
sudo systemctl status eth1 beacon-chain
```
{% endtab %}
{% endtabs %}

Finally, verify your validator's attestations are working with public block explorer such as

[https://prater.beaconcha.in](https://prater.beaconcha.in/)

Enter your validator's pubkey to view its status.

### ✨ 8.11 How to improve validator attestation effectiveness

{% hint style="info" %}
Learn about [attestation effectiveness from Attestant.io](https://www.attestant.io/posts/defining-attestation-effectiveness/)
{% endhint %}

#### 👨👩👦👦 Strategy \#1: Increase eth2 beacon chain peer count

{% hint style="info" %}
This change will result in increased bandwidth and memory usage. Tweak and tailor appropriately for your hardware.

_Kudos to_ [_Rémy Roy_](https://www.reddit.com/user/remyroy/) _for this strat._
{% endhint %}

Edit your `beacon-chain.service` unit file \(except for Teku\).

```bash
sudo nano /etc/systemd/system/beacon-chain.service
```

Add the following flag to increase peers on the `ExecStart` line.

{% tabs %}
{% tab title="Lighthouse" %}
```bash
--target-peers 100
# Example
# lighthouse bn --target-peers 100 --staking --metrics --network prater
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
--max-peers=100
# Example
# /usr/bin/nimbus_beacon_node --network=prater --max-peers=100
```
{% endtab %}

{% tab title="Teku" %}
```bash
# Edit teku.yaml
sudo nano /etc/teku/teku.yaml

# add the following line to teku.yaml and save the file
p2p-peer-upper-bound: 100
```
{% endtab %}

{% tab title="Prysm" %}
```bash
--p2p-max-peers=100
# Example
# prysm.sh beacon-chain --prater --p2p-max-peers=100 --http-web3provider=http://127.0.0.1:8545 --accept-terms-of-use
```
{% endtab %}

{% tab title="Lodestar" %}
```bash
--network.maxPeers 100
# Example
# yarn run cli beacon --network.maxPeers 100 --network prater
```
{% endtab %}
{% endtabs %}

Reload the updated unit file and restart the beacon-chain process to complete this change.

```bash
sudo systemctl daemon-reload
sudo systemctl restart beacon-chain
```

#### 👩🚀 Strategy \#2: Eth1 node redundancy

{% hint style="info" %}
Especially useful during eth1 upgrades, when your primary node is temporarily unavailable.
{% endhint %}

Edit your `beacon-chain.service` unit file.

```bash
sudo nano /etc/systemd/system/beacon-chain.service
```

Add the following flag on the `ExecStart` line. 

{% tabs %}
{% tab title="Lighthouse" %}
```bash
--eth1-endpoints <http://alternate eth1 endpoints>
# Example, separate endpoints with commas.
# --eth1-endpoints http://localhost:8545,https://nodes.mewapi.io/rpc/eth,https://mainnet.eth.cloud.ava.do,https://mainnet.infura.io/v3/xxx
```
{% endtab %}

{% tab title="Prysm" %}
```bash
--fallback-web3provider=<http://<alternate eth1 provider one> --fallback-web3provider=<http://<alternate eth1 provider two>
# Example, repeat flag for multiple eth1 providers
# --fallback-web3provider=https://nodes.mewapi.io/rpc/eth --fallback-web3provider=https://mainnet.infura.io/v3/YOUR-PROJECT-ID
```
{% endtab %}

{% tab title="Teku" %}
```bash
--eth1-endpoints <http://alternate eth1 endpoints>,<http://alternate eth1 endpoints>
# Support for automatic fail-over of eth1-endpoints. Multiple endpoints can be specified with the new --eth1-endpoints CLI option.
# Example
# --eth1-endpoints http://localhost:8545,https://infura.io/
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
--web3-url <http://alternate eth1 endpoint>
# Multiple endpoints can be specified with additional parameters of --web3-url
# Example
# --web3-url http://localhost:8545 --web3-url wss://mainnet.infura.io/ws/v3/...
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
💸 Signup for free goerli ethereum fallback nodes at [https://infura.io/](https://infura.io/)
{% endhint %}

Reload the updated unit file and restart the beacon-chain process to complete this change.

```bash
sudo systemctl daemon-reload
sudo systemctl restart beacon-chain
```

#### ⚙ Strategy \#3: Perform updates or reboots during the longest attestation gap

Learn how to at the following quick guide.

{% page-ref page="guide-or-how-to-setup-a-validator-on-eth2-mainnet/how-to-find-longest-attestation-slot-gap.md" %}

#### ⛓ Strategy \#4: Beacon node redundancy

{% hint style="info" %}
Allows the VC \(validator client\) to connect to multiple BN \(beacon nodes\). This means your validator client can use multiple BNs. Whenever a BN fails to respond, the VC will try again with the next BN.

Must install a BN of the same eth2 client on another server.

Currently only works for Lighthouse.
{% endhint %}

Edit your `validator.service` unit file.

```bash
sudo nano /etc/systemd/system/validator.service
```

Add the following flag on the `ExecStart` line. 

{% tabs %}
{% tab title="Lighthouse" %}
```bash
--beacon-nodes <BEACON-NODE ENDPOINTS>
# Example, separate endpoints with commas.
# lighthouse vc --beacon-nodes http://localhost:5052,http://192.168.1.100:5052
# If localhost is not responsive (perhaps during an update), the VC will attempt to use 192.168.1.100 instead.
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
[Infura.io](https://infura.io) offers beacon-node endpoints.
{% endhint %}

Reload the updated unit file and restart the validator process to complete this change.

```bash
sudo systemctl daemon-reload
sudo systemctl restart validator
```

### 🧩 8.12 EIP2333 Key Generator by iancoleman.io

A [key generator tool by iancoleman](https://iancoleman.io/eip2333/) can generate EIP2333 keys from a BIP39 mnemonic, or a seed, or a master secret key.

This tool should be used **offline** and is useful for extracting **withdrawal keys** and **signing keys**.

{% hint style="info" %}
For more info see the [EIP2333 spec](https://eips.ethereum.org/EIPS/eip-2333).
{% endhint %}

{% embed url="https://iancoleman.io/eip2333/" %}

### 📀 8.13 Dealing with storage issues on the Eth1 node

{% hint style="info" %}
It is currently recommended to use a minimum 1TB hard disk.

_Kudos to_ [_angyts_](https://github.com/angyts) _for this contribution._
{% endhint %}

After running the Eth1 node for a while, you will notice that it will start to fill up the hard disk. The following steps might be helpful for you.

Firstly make sure you have a fallback Eth1 node: see [8.11 Strategy 2](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet#strategy-2-eth1-redundancy).

{% tabs %}
{% tab title="Manually Pruning Geth" %}
{% hint style="info" %}
Since Geth 1.10x version, the blockchain data can be regularly pruned to reduce it's size.
{% endhint %}

Reference: [https://gist.github.com/yorickdowne/3323759b4cbf2022e191ab058a4276b2](https://gist.github.com/yorickdowne/3323759b4cbf2022e191ab058a4276b2)

You will need to upgrade Geth to at least 1.10x. Other prerequisites are a fully synced Eth1 node and that a snapshot has been created.

Stop your Eth1 node

```text
sudo systemctl stop eth1
```

Prune the blockchain data

```text
geth --datadir ~/.ethereum snapshot prune-state
```

{% hint style="warning" %}
🔥 **Geth pruning Caveats**: 

* Pruning can take a few hours or longer \(typically 2 to 10 hours is common\) depending on your node's disk performance.
* There are three stages to pruning: **iterating state snapshot, pruning state data and compacting database.**
* "**Compacting database**" will stop updating status and appear hung. **Do not interrupt or restart this process.** Typically after an hour, pruning status messages will reappear.
{% endhint %}

Restart the Eth1 node

```text
sudo systemctl start eth1
```
{% endtab %}

{% tab title="Adding new hard disks and changing the data directory" %}
After you have installed your hard disk, you will need to properly format it and automount it. Consult the ubuntu guides on this.

I will assume that the new disk has been mounted onto `/mnt/eth1-data`. \(The name of the mount point is up to you\)

Handling file permissions.

You need to change ownership of the folder to be accessible by your `eth1 service`. If your folder is a different name, please change the `/mnt/eth1-data` accordingly.

```text
sudo chown $whoami:$whoami /mnt/eth1-data
```

```text
sudo chmod 755 /mnt/eth1-data
```

Stop your Eth1 node.

```text
sudo systemctl stop eth1
```

Edit the system service file to point to a new data directory.

```text
sudo nano /etc/systemd/system/eth1.service
```

At the end of this command starting with `/usr/bin/geth --http --metrics ....` add a space and the following flag `--datadir "/mnt/eth1-data"`.

`Ctrl-X` to save your settings.

Refresh the system service daemon to load the new configurations.

```text
sudo systemctl daemon-reload
```

Restart the Eth1 node.

```text
sudo systemctl start eth1
```

Make sure it is up and running by viewing the running logs.

```text
journalctl -fu eth1
```

\(**Optional**\) Delete original data directory

```text
rm -r ~/.ethereum
```
{% endtab %}
{% endtabs %}

## 🌇 9. Join the community on Discord and Reddit

### 📱 Discord

{% tabs %}
{% tab title="Lighthouse" %}
{% embed url="https://discord.gg/cyAszAh" %}
{% endtab %}

{% tab title="Nimbus" %}
{% embed url="https://discord.gg/XRxWahP" %}
{% endtab %}

{% tab title="Teku" %}
{% embed url="https://discord.gg/7hPv2T6" %}
{% endtab %}

{% tab title="Prysm" %}
{% embed url="https://discord.gg/XkyZSSk4My" %}
{% endtab %}

{% tab title="Lodestar" %}
{% embed url="https://discord.gg/aMxzVcr" %}
{% endtab %}

{% tab title="CoinCashew" %}
{% embed url="https://discord.gg/w8Bx8W2HPW" %}
{% endtab %}
{% endtabs %}

### 🌍 Reddit r/ethStaker

{% embed url="https://www.reddit.com/r/ethstaker/" caption="" %}

## 🧩 10. Reference Material

Appreciate the hard work done by the fine folks at the following links which served as a foundation for creating this guide.

{% embed url="https://prater.launchpad.ethereum.org/" caption="" %}

{% embed url="https://pegasys.tech/teku-ethereum-2-for-enterprise/" caption="" %}

{% embed url="https://docs.teku.pegasys.tech/en/latest/HowTo/Get-Started/Build-From-Source/" caption="" %}

{% embed url="https://lighthouse-book.sigmaprime.io/intro.html" caption="" %}

{% embed url="https://status-im.github.io/nimbus-eth2/intro.html" caption="" %}

{% embed url="https://prylabs.net/participate" caption="" %}

{% embed url="https://docs.prylabs.network/docs/getting-started/" caption="" %}

{% embed url="https://chainsafe.github.io/lodestar/installation/" caption="" %}

## 🙏 11. Bonus links

### 🧱 ETH2 Block Explorers

{% embed url="https://prater.beaconcha.in/" caption="" %}

{% embed url="https://beaconscan.com/" caption="" %}

### 📰 Latest Eth2 Info

{% embed url="https://hackmd.io/@benjaminion/eth2\_news/" caption="" %}

{% embed url="https://www.reddit.com/r/ethstaker" caption="" %}

{% embed url="https://blog.ethereum.org/" caption="" %}

###  👨👩👧👧 Additional ETH2 Community Guides

{% embed url="https://someresat.medium.com/" caption="" %}

{% embed url="https://github.com/metanull-operator/eth2-ubuntu" caption="" %}

{% embed url="https://agstakingco.gitbook.io/eth-2-0-staking-guide-prater-lighthouse/" %}

#### Hardware Staking Guide [https://www.reddit.com/r/ethstaker/comments/j3mlup/a\_slightly\_updated\_look\_at\_hardware\_for\_staking/](https://www.reddit.com/r/ethstaker/comments/j3mlup/a_slightly_updated_look_at_hardware_for_staking/)

{% embed url="https://medium.com/@RaymondDurk/how-to-stake-for-ethereum-2-0-with-dappnode-231fa7689c02" caption="" %}

{% embed url="https://kb.beaconcha.in/" caption="" %}

