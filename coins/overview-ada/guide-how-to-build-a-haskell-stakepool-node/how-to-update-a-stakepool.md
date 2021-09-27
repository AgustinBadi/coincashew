# How to update a Stakepool

## 🎉 ∞ Pre-Announcements

{% hint style="info" %}
🎊 This latest update brought to you by the generous donations by [**BEBOP stake pool**](https://bebopadapool.com/). If you found this helpful, consider using [cointr.ee to find our donation ](https://cointr.ee/coincashew)addresses. 🙏 
{% endhint %}

{% hint style="success" %}
As of August 27 2021, this guide is written for **mainnet** with **release v1.29.0** 😁 
{% endhint %}

## 📡 1. How to perform an update

From time to time, there will be new versions of `cardano-node`. Follow the [Official Cardano-Node Github Repo](https://github.com/input-output-hk/cardano-node) by enabling **notifications** with the watch functionality.

{% hint style="danger" %}
Read the patch notes for any other special updates or dependencies that may be required for the latest release.
{% endhint %}

{% tabs %}
{% tab title="v1.29.0 Notes" %}
**Full release notes:** [**https://github.com/input-output-hk/cardano-node/releases/tag/1.29.0**](https://github.com/input-output-hk/cardano-node/releases/tag/1.29.0)\*\*\*\*

This release is an important update to the node that provides the functionality that is needed following the Alonzo hard fork.  
**All users, including stake pool operators, must upgrade to this version \(or a later version\) of the node.**

The release includes features that will enable the use of the node in the Alonzo era, allowing the on-chain execution of Plutus scripts,  
including extended CLI commands to support the construction of transactions that include Plutus scripts, datums and redeemers.  
It incorporates several improvements, including a new `transaction build` command that calculates transaction fees and Plutus script execution units, and a new version of the `query tip` command that provides additional information, including node synchronisation progress. The `transaction build` command requires a local instance of the node in order to check Plutus script validity and to provide information that is used by the fee calculation. The Shelley specification has also been updated with respect to rewards calculation.

Note that this release changes the log format of traces configured by `TraceChainSyncHeaderServer` and `TraceChainSyncClient` . See [\#2746](https://github.com/input-output-hk/cardano-node/pull/2746) for more detail.

### 🛑 Release Dependencies

#### 1. If using cncli for leaderlogs and sendslots, update to `cncli version 3.15` is required.

```bash
RELEASETAG=$(curl -s https://api.github.com/repos/AndrewWestberg/cncli/releases/latest | jq -r .tag_name)
VERSION=$(echo ${RELEASETAG} | cut -c 2-)
echo "Installing release ${RELEASETAG}"
curl -sLJ https://github.com/AndrewWestberg/cncli/releases/download/${RELEASETAG}/cncli-${VERSION}-x86_64-unknown-linux-gnu.tar.gz -o /tmp/cncli-${VERSION}-x86_64-unknown-linux-gnu.tar.gz
```

```bash
sudo tar xzvf /tmp/cncli-${VERSION}-x86_64-unknown-linux-gnu.tar.gz -C /usr/local/bin/
```

#### Checking that cncli is properly updated

```text
cncli -V
```

It should return the updated version number.

**2. Download `mainnet-alonzo-genesis.json` file**

```bash
cd $NODE_HOME
wget -N https://hydra.iohk.io/build/7416228/download/1/mainnet-alonzo-genesis.json
```

**3. Download new`mainnet-config.json` file to with alonzo configurations.**

{% hint style="info" %}
If you have any custom mainnet configurations, be sure to backup and re-apply your settings.
{% endhint %}

```bash
cd $NODE_HOME
wget -N https://hydra.iohk.io/build/7416228/download/1/mainnet-config.json
sed -i mainnet-config.json \
    -e "s/TraceBlockFetchDecisions\": false/TraceBlockFetchDecisions\": true/g" \
    -e "s/127.0.0.1/0.0.0.0/g"
```

Verify that your **mainnet-config.json** contains the following two new lines.

```text
  "AlonzoGenesisFile": "mainnet-alonzo-genesis.json",
  "AlonzoGenesisHash": "7e94a15f55d1e82d10f09203fa1d40f8eede58fd8066542cf6566008068ed874",
```

View your **mainnet-config.json**

```bash
cat mainnet-config.json
```

Example of what it should look like with the two new lines.

```bash
{
  "AlonzoGenesisFile": "mainnet-alonzo-genesis.json",
  "AlonzoGenesisHash": "7e94a15f55d1e82d10f09203fa1d40f8eede58fd8066542cf6566008068ed874",
  "ApplicationName": "cardano-sl",
  "ApplicationVersion": 1,
  "ByronGenesisFile": "byron-genesis.json",
  "ByronGenesisHash": "5f20df933584822601f9e3f8c024eb5eb252fe8cefb24d1317dc3d432e940ebb",
  "LastKnownBlockVersion-Alt": 0,
  "LastKnownBlockVersion-Major": 3,
  "LastKnownBlockVersion-Minor": 0,
  "MaxKnownMajorProtocolVersion": 2,
  "Protocol": "Cardano",
  "RequiresNetworkMagic": "RequiresNoMagic",
  "ShelleyGenesisFile": "genesis.json",
  "ShelleyGenesisHash": "1a3be38bcbb7911969283716ad7aa550250226b76a61fc51cc9a9a35d9276d81",
```

**\[ Optional Troubleshooting \]** 4. In case your node does not start up properly, refresh `mainnet-shelley-genesis.json`

```bash
cd $NODE_HOME
wget -N https://hydra.iohk.io/build/7416228/download/1/mainnet-shelley-genesis.json
```
{% endtab %}

{% tab title="v1.27.0 Notes" %}
**Full release notes:** [**https://github.com/input-output-hk/cardano-node/releases/tag/1.27.0**](https://github.com/input-output-hk/cardano-node/releases/tag/1.27.0)\*\*\*\*

Node version 1.27.0 provides important new functionality, including supporting new CLI commands that have been requested by stake pools, providing garbage collection metrics.  
It includes the performance fixes for the epoch boundary calculation that were released in node version [1.26.2](https://github.com/input-output-hk/cardano-node/releases/tag/1.26.2), plus a number of bug fixes and code improvements.  
It also includes many fundamental changes that are needed to prepare for forthcoming feature releases \(notably Plutus scripts in the Alonzo era\).  
Note that this release includes breaking changes to the API and CLI commands, and that compilation using GHC version 8.6.5 is no longer supported.

### 🛑 Release Dependencies

#### 1. If using cncli for leaderlogs and sendslots, update to `cncli version 2.10` is required.

```bash
RELEASETAG=$(curl -s https://api.github.com/repos/AndrewWestberg/cncli/releases/latest | jq -r .tag_name)
VERSION=$(echo ${RELEASETAG} | cut -c 2-)
echo "Installing release ${RELEASETAG}"
curl -sLJ https://github.com/AndrewWestberg/cncli/releases/download/${RELEASETAG}/cncli-${VERSION}-x86_64-unknown-linux-gnu.tar.gz -o /tmp/cncli-${VERSION}-x86_64-unknown-linux-gnu.tar.gz
```

```bash
sudo tar xzvf /tmp/cncli-${VERSION}-x86_64-unknown-linux-gnu.tar.gz -C /usr/local/bin/
```

#### Checking that cncli is properly updated

```text
cncli -V
```

It should return the updated version number.
{% endtab %}

{% tab title="v1.26.2 Notes" %}
**Full release notes:** [**https://github.com/input-output-hk/cardano-node/releases/tag/1.26.2**](https://github.com/input-output-hk/cardano-node/releases/tag/1.26.2)\*\*\*\*

This point release is a recommended upgrade for all stake pool operators. It is not required for relays or other passive nodes. It ensures that block producing nodes do not unnecessarily re-evaluate the stake distribution at the epoch boundary.

{% hint style="info" %}
It is possible to upgrade from v1.25.1 but for a smooth update, ensure you have completed the v1.26.1 release dependencies, notably the ghc and cabal updates. Also note the database migration can take up to 2 hours.
{% endhint %}
{% endtab %}

{% tab title="v1.26.1 Notes" %}
**Full release notes:** [**https://github.com/input-output-hk/cardano-node/releases/tag/1.26.1**](https://github.com/input-output-hk/cardano-node/releases/tag/1.26.1)\*\*\*\*

This release includes significant performance improvements and numerous other minor improvements and feature additions. In particular the reward calculation pause is eliminated, and the CPU load for relays handling lots of incoming transactions should be significantly reduced.  
The focus of the current development work is on completing and integrating support for the Alonzo era. This release includes many of the internal changes but does not yet include support for the new era.

* Note that this release will automatically perform a DB migration on the first startup after the update. The **migration should take 10-120 minutes** however it can take up to 60 minutes depending on your CPU. If this is a problem for your use, then see below for steps to mitigate this.
* The format of the `cardano-cli query tip` query has changed - see release notes for the CLI below.
* The `cardano-cli query ledger-state` query now produces a binary file if `--out-file` is specified. If required \(e.g. for use in `cncli`\), JSON output can be obtained by omitting this parameter, and redirecting the standard output to a file

#### Steps to mitigate downtime for this update

This release will automatically perform a DB migration on the first startup after the update. The migration should take 10-20 minutes however it can take up to 60 minutes depending on your CPU.

To mitigate downtime:

1. Update a non-production mainnet node
2. Take a DB snapshot
3. Stop production node
4. Backup and replace DB with snapshot
5. Restart production node on 1.26.1
6. Repeat steps 3-5 for all production nodes

### 🛑 Release v1.26.1 New Dependencies

#### 1. Install GHC 8.10.4 and Cabal 3.4.0.0 and update dependencies

```bash
sudo apt-get -y install pkg-config libgmp-dev libssl-dev libtinfo-dev libsystemd-dev zlib1g-dev
```

```bash
sudo apt-get -y install build-essential curl libgmp-dev libffi-dev libncurses-dev libtinfo5
```

```bash
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```

Answer **NO** to installing haskell-language-server \(HLS\).

Answer **YES** to automatically add the required PATH variable to ".bashrc".

```bash
source ~/.bashrc
ghcup upgrade
ghcup install ghc 8.10.4
ghcup set ghc 8.10.4
# Verify 8.10.4 is installed
ghc --version  
```

```bash
ghcup install cabal 3.4.0.0
ghcup set cabal 3.4.0.0
# Verify 3.4.0.0 is installed
cabal --version 
```

#### 2. Update variables for topologyUpdater.sh on relay nodes

`.blockNo` was changed to `.block`

```bash
cd $NODE_HOME
sed -i topologyUpdater.sh \
  -e "s/jq -r .blockNo/jq -r .block/g"
```

**3. Update gLiveView**

```bash
cd ${NODE_HOME}
curl -s -o gLiveView.sh https://raw.githubusercontent.com/cardano-community/guild-operators/master/scripts/cnode-helper-scripts/gLiveView.sh
curl -s -o env https://raw.githubusercontent.com/cardano-community/guild-operators/master/scripts/cnode-helper-scripts/env
chmod 755 gLiveView.sh
# Update env file with the stake pools configuration.
sed -i env \
    -e "s/\#CONFIG=\"\${CNODE_HOME}\/files\/config.json\"/CONFIG=\"\${NODE_HOME}\/mainnet-config.json\"/g" \
    -e "s/\#SOCKET=\"\${CNODE_HOME}\/sockets\/node0.socket\"/SOCKET=\"\${NODE_HOME}\/db\/socket\"/g"
```

#### 4. If used, update qcpolsendmytip.sh on block producer core node

Various variables changed.

```bash
cd $NODE_HOME
sed -i qcpolsendmytip.sh \
  -e "s/jq -r '.slotNo, .headerHash, .blockNo'/jq -r '.slot, .hash, .block'/g"
sudo systemctl restart qcpolsendmytip
```
{% endtab %}
{% endtabs %}

### Compiling the new binaries

To update with `$HOME/git/cardano-node` as the current binaries directory, clone a new git repo named `cardano-node2` so that you have a backup in case of rollback. Remove the old binaries.

```bash
cd $HOME/git
rm -rf cardano-node-old/
git clone https://github.com/input-output-hk/cardano-node.git cardano-node2
cd cardano-node2/
```

Run the following command to pull and build the latest binaries. Change the checkout **tag** or **branch** as needed.

```bash
cd $HOME/git/cardano-node2
cabal update
git fetch --all --recurse-submodules --tags
git checkout $(curl -s https://api.github.com/repos/input-output-hk/cardano-node/releases/latest | jq -r .tag_name)
cabal configure -O0 -w ghc-8.10.4
echo -e "package cardano-crypto-praos\n flags: -external-libsodium-vrf" > cabal.project.local
cabal build cardano-node cardano-cli
```

{% hint style="info" %}
Build process may take a few minutes up to a few hours depending on your computer's processing power.
{% endhint %}

Verify your **cardano-cli** and **cardano-node** were updated to the expected version.

```bash
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") version
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") version
```

{% hint style="danger" %}
Stop your node before updating the binaries.
{% endhint %}

{% tabs %}
{% tab title="systemd" %}
```
sudo systemctl stop cardano-node
```
{% endtab %}

{% tab title="cnode" %}
```
sudo systemctl stop cnode
```
{% endtab %}

{% tab title="block producer node" %}
```bash
killall -s 2 cardano-node
```
{% endtab %}

{% tab title="relaynode1" %}
```
killall -s 2 cardano-node
```
{% endtab %}
{% endtabs %}

Copy **cardano-cli** and **cardano-node** files into bin directory.

```bash
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") /usr/local/bin/cardano-cli
```

```bash
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") /usr/local/bin/cardano-node
```

Verify your **cardano-cli** and **cardano-node** were copied successfully and updated to the expected version.

```bash
cardano-node version
cardano-cli version
```

{% hint style="success" %}
Now restart your node to use the updated binaries.
{% endhint %}

{% tabs %}
{% tab title="systemd" %}
```
sudo systemctl start cardano-node
```
{% endtab %}

{% tab title="cnode" %}
```
sudo systemctl start cnode
```
{% endtab %}

{% tab title="block producer node" %}
```bash
cd $NODE_HOME
./startBlockProducingNode.sh
```
{% endtab %}

{% tab title="relaynode1" %}
```bash
cd $NODE_HOME
./startRelayNode1.sh
```
{% endtab %}
{% endtabs %}

Finally, the following sequence will switch-over to your newly built cardano-node folder while keeping the old directory for backup.

```bash
cd $HOME/git
mv cardano-node/ cardano-node-old/
mv cardano-node2/ cardano-node/
```

{% hint style="danger" %}
\*\*\*\*🤖 **Important Reminder**: Don't forget to update your **air-gapped offline machine \(cold environment\)** with the new **Cardano CLI** binaries.
{% endhint %}

{% hint style="success" %}
Congrats on completing the update. ✨ 

Did you find our guide useful? Send us a signal with a tip and we'll keep updating it. 

It really energizes us to keep creating the best crypto guides. 

Use [cointr.ee to find our donation ](https://cointr.ee/coincashew)addresses. 🙏 

Any feedback and all pull requests much appreciated. 🌛 

Hang out and chat with fellow stake pool operators on Discord @

[https://discord.gg/w8Bx8W2HPW](https://discord.gg/w8Bx8W2HPW) 😃 

Hang out and chat with our stake pool community on Telegram @ [https://t.me/coincashew](https://t.me/coincashew)
{% endhint %}

## 🤯 2. In case of problems

### 🛣 4.1 Forked off

Forget to update your node and now your node is stuck on an old chain?

Reset your database files and be sure to grab the [latest genesis, config, topology json files](https://hydra.iohk.io/job/Cardano/cardano-node/cardano-deployment/latest-finished/download/1/index.html).

```bash
cd $NODE_HOME
rm -rf db
```

### 📂 4.2 Roll back to previous version from backup

{% hint style="danger" %}
Stop your node before updating the binaries.
{% endhint %}

{% tabs %}
{% tab title="systemd" %}
```
sudo systemctl stop cardano-node
```
{% endtab %}

{% tab title="cnode" %}
```
sudo systemctl stop cnode
```
{% endtab %}

{% tab title="block producer node" %}
```bash
killall -s 2 cardano-node
```
{% endtab %}

{% tab title="relaynode1" %}
```
killall -s 2 cardano-node
```
{% endtab %}
{% endtabs %}

Restore the old repository.

```bash
cd $HOME/git
mv cardano-node/ cardano-node-rolled-back/
mv cardano-node-old/ cardano-node/
```

Copy the binaries to `/usr/local/bin`

```bash
sudo cp $(find $HOME/git/cardano-node/dist-newstyle/build -type f -name "cardano-cli") /usr/local/bin/cardano-cli
sudo cp $(find $HOME/git/cardano-node/dist-newstyle/build -type f -name "cardano-node") /usr/local/bin/cardano-node
```

Verify the binaries are the correct version.

```bash
/usr/local/bin/cardano-cli version
/usr/local/bin/cardano-node version
```

{% hint style="success" %}
Now restart your node to use the updated binaries.
{% endhint %}

{% tabs %}
{% tab title="systemd" %}
```
sudo systemctl start cardano-node
```
{% endtab %}

{% tab title="cnode" %}
```
sudo systemctl start cnode
```
{% endtab %}

{% tab title="block producer node" %}
```bash
cd $NODE_HOME
./startBlockProducingNode.sh
```
{% endtab %}

{% tab title="relaynode1" %}
```bash
cd $NODE_HOME
./startRelayNode1.sh
```
{% endtab %}
{% endtabs %}

### 🤖 4.3 Last resort: Rebuild from source code

Follow the steps in [How to build a Stakepool.](./)

