# Upgrading a Node

## :tada: Introduction

{% hint style="success" %}
If you want to support this free educational Cardano content or found the content helpful, visit [cointr.ee to find our donation addresses](https://cointr.ee/coincashew). Much appreciated in advance. :pray:

Technical writing by Paradoxical Sphere and [Change Stake Pool \[CHG\]](https://change.paradoxicalsphere.com)
{% endhint %}

Input-Output (IOHK) regularly releases new versions of Cardano Node via the `cardano-node` [GitHub repository](https://github.com/input-output-hk/cardano-node). Carefully review release notes available in the repository for new features, known issues, technical specifications, related downloads, documentation, changelogs, assets and other details of each new release.

{% hint style="info" %}
To receive notifications related to activity in the Cardano Node GitHub repository, configure [Watch](https://docs.github.com/en/account-and-profile/managing-subscriptions-and-notifications-on-github/setting-up-notifications/configuring-notifications#automatic-watching) functionality.
{% endhint %}

The *Upgrading a Node* topic describes how to upgrade a Cardano node to the latest version. Complete the following procedures on each block producer and relay node comprising your stake pool, as needed to complete the upgrade.

{% hint style="info" %}
For instructions on upgrading your stake pool to a previous Cardano version, see the [archive](./upgrading-a-node-archive.md).
{% endhint %}

## :octagonal\_sign: Upgrading Third-party Software

### CNCLI

If you use Andrew Westberg's [CNCLI](https://github.com/AndrewWestberg/cncli) command line utilities, then upgrade to the latest version if a newer version is available.

{% hint style="danger" %}
Do not confuse Andrew Westberg's CNCLI utilities with the [`cncli.sh`](https://cardano-community.github.io/guild-operators/Scripts/cncli/) companion script for stake pool operators maintained by [Guild Operators](https://cardano-community.github.io/guild-operators/).
{% endhint %}

**To upgrade Andrew Westberg's CNCLI binary:**

1. In a terminal window, type the following commands:
```bash
RELEASETAG=$(curl -s https://api.github.com/repos/AndrewWestberg/cncli/releases/latest | jq -r .tag_name)
VERSION=$(echo ${RELEASETAG} | cut -c 2-)
echo "Installing release ${RELEASETAG}"
curl -sLJ https://github.com/AndrewWestberg/cncli/releases/download/${RELEASETAG}/cncli-${VERSION}-x86_64-unknown-linux-gnu.tar.gz -o /tmp/cncli-${VERSION}-x86_64-unknown-linux-gnu.tar.gz
sudo tar xzvf /tmp/cncli-${VERSION}-x86_64-unknown-linux-gnu.tar.gz -C /usr/local/bin/
```

2. To confirm that the new version of the CNCLI binary is installed, type:
```
cncli -V
```

If the command displays the latest version number, then the upgrade is successful.

### Guild LiveView

[Guild Operators](https://cardano-community.github.io/guild-operators/) maintains a set of tools to simplify operating a Cardano stake pool, including [Guild LiveView](https://cardano-community.github.io/guild-operators/Scripts/gliveview/). If you use the gLiveView script, then ensure that you are using the latest version prior to upgrading your Cardano node.

{% hint style="info" %}
In the [Common `env`](https://cardano-community.github.io/guild-operators/Scripts/env/) file for Guild Operator scripts, by default the `UPDATE_CHECK` user variable is set to check for updates to scripts when you start `gLiveView.sh`. If your implementation displays the message `Checking for script updates` when gLiveView starts and you install available updates when prompted, then you do NOT need to complete the following procedure.
{% endhint %}

**To upgrade the Guild LiveView tool manually:**

1. To navigate to the folder where Guild LiveView is located, type the following command in a terminal window:
```bash
cd ${NODE_HOME}
```

2. To back up existing Guild LiveView script files, type:
```bash
mv gLiveView.sh gLiveView.sh.bak
mv env env.bak
```

3. To download the latest Guild LiveView script files, type:
```bash
curl -s -o gLiveView.sh https://raw.githubusercontent.com/cardano-community/guild-operators/master/scripts/cnode-helper-scripts/gLiveView.sh
curl -s -o env https://raw.githubusercontent.com/cardano-community/guild-operators/master/scripts/cnode-helper-scripts/env
```

4. To set file permissions on the `gLiveView.sh` file that you downloaded in step 3, type:
```bash
chmod 755 gLiveView.sh
```

5. To set the `CONFIG` and `SOCKET` user variables in the `env` file that you downloaded in step 3, type:
```bash
sed -i env \
    -e "s/\#CONFIG=\"\${CNODE_HOME}\/files\/config.json\"/CONFIG=\"\${NODE_HOME}\/mainnet-config.json\"/g" \
    -e "s/\#SOCKET=\"\${CNODE_HOME}\/sockets\/node0.socket\"/SOCKET=\"\${NODE_HOME}\/db\/socket\"/g"
```
{% hint style="info" %}
For details on setting the `NODE_HOME` environment variable, see the topic [Installing Cabal and GHC](../part-i-installation/installing-cabal-and-ghc.md)
{% endhint %}

6. As needed to configure Guild LiveView for your stake pool, use a text editor to transfer additional user variable definitions from the `env.bak` file that you created in step 2 to the `env` file that you downloaded in step 3.

7. To test the upgrade, type:
```bash
gLiveView.sh
```

If the upgrade is successful, then the terminal window displays the Guild LiveView dashboard having the version number of the latest release.

## <a name="SetGCVersions"></a>Setting GHC and Cabal Versions

For each Cardano Node release, Input-Output recommends compiling binaries using specific versions of GHC and Cabal. For example, refer to [Installing cardano-node and cardano-cli from source](https://developers.cardano.org/docs/get-started/installing-cardano-node/) in the [Cardano Developer Portal](https://developers.cardano.org/docs/get-started/) to determine the GHC and Cabal versions required for the current Cardano Node release. _Table 1_ lists GHC and Cabal version requirements for the current Cardano Node release.

_Table 1 Current Cardano Node Version Requirements_

|  Release Date  |  Cardano Node Version  |  GHC Version   | Cabal Version  |
|:--------------:|:----------------------:|:--------------:|:--------------:|
|  March 7, 2022 |         1.34.1         |     8.10.7     |    3.6.2.0     |

**To upgrade the GHCup installer for GHC and Cabal to the latest version:**

- In a terminal window, type:
```
ghcup upgrade
ghcup --version
```

**To install other GHC versions:**

- In a terminal window, type the following commands where `<GHCVersionNumber>` is the GHC version that you want to install and use:
```
ghcup install ghc <GHCVersionNumber>
ghcup set ghc <GHCVersionNumber>
ghc --version
```

**To install other Cabal versions:**

- In a terminal window, type the following commands where `<CabalVersionNumber>` is the Cabal version that you want to install and use:
```
ghcup install cabal <CabalVersionNumber>
ghcup set cabal <CabalVersionNumber>
cabal --version
```

{% hint style="info" %}
To set GHCup, GHC and Cabal versions using a graphical user interface, type `ghcup tui` in a terminal window.
{% endhint %}

## Downloading New Configuration Files



## <a name="BuildingCN"></a>Building Cardano Node Binaries

**To build binaries for a new Cardano Node version:**

1. To create a clone of the Cardano Node [GitHub repository](https://github.com/input-output-hk/cardano-node), type the following commands in a terminal window on the computer you want to upgrade where `<NewFolderName>` is the name of a folder that does not exist:
```bash
# Navigate to the folder where you want to clone the repository
cd $HOME/git
# Download the Cardano Node repository to your local computer
git clone https://github.com/input-output-hk/cardano-node.git ./<NewFolderName>
```  
{% hint style="info" %}
Cloning the GitHub repository to a new folder allows you to roll back the upgrade, if needed, by re-installing on your computer the `cardano-node` and `cardano-cli` binaries from the folder where you compiled a previous version of Cardano Node packages.
{% endhint %}

2. To build Cardano Node binaries using the source code that you downloaded in step 1, type the following commands where `<NewFolderName>` is the name of the folder you created in step 1 and `<GHCVersionNumber>` is the GHC version that you set in the section [Setting GHC and Cabal Versions](./upgrading-a-node.md#SetGCVersions):
```bash
# Navigate to the folder where you cloned the Cardano Node repository
cd $HOME/git/<NewFolderName>
# Update the list of available packages
cabal update
# Download all branches and tags from the remote repository
git fetch --all --recurse-submodules --tags
# Switch to the branch of the latest Cardano Node release
git checkout $(curl -s https://api.github.com/repos/input-output-hk/cardano-node/releases/latest | jq -r .tag_name)
# Adjust the project configuration to disable optimization and use the recommended compiler version
cabal configure -O0 -w ghc-<GHCVersionNumber>
# Append the cabal.project.local file in the current folder to avoid installing the custom libsodium library
echo -e "package cardano-crypto-praos\n flags: -external-libsodium-vrf" >> cabal.project.local
# Compile the cardano-node and cardano-cli packages found in the current directory
cabal build cardano-node cardano-cli
```  
<!-- References:
https://stackoverflow.com/questions/67748740/what-is-the-difference-between-git-clone-git-fetch-and-git-pull
https://cabal.readthedocs.io/en/3.4/cabal-project.html#package-configuration-options
https://iohk.zendesk.com/hc/en-us/articles/900001951646-Building-a-node-from-source -->  
{% hint style="info" %}
The time required to compile the `cardano-node` and `cardano-cli` packages may be a few minutes to hours, depending on the specifications of your computer.
{% endhint %}

3. When the compiler finishes, to verify the version numbers of the new `cardano-node` and `cardano-cli` binaries type the following commands where `<NewFolderName>` is the folder where you cloned the Cardano Node GitHub repository in step 1:
```bash
$(find $HOME/git/<NewFolderName>/dist-newstyle/build -type f -name "cardano-node") version
$(find $HOME/git/<NewFolderName>/dist-newstyle/build -type f -name "cardano-cli") version
```

## Installing New Cardano Node Binaries

**To install new `cardano-node` and `cardano-cli` binaries:**

1. To stop your Cardano node, type the following command where `<CardanoServiceName>` is the name of the systemd service running your Cardano node:
```bash
sudo systemctl stop <CardanoServiceName>.service
```
{% hint style="info" %}
If you follow the [Coin Cashew](https://www.coincashew.com/) instructions for [Creating Startup Scripts](../part-ii-configuration/creating-startup-scripts.md), then `<CardanoServiceName>` is `cardano-node`
{% endhint %}  

2. To replace the existing `cardano-node` and `cardano-cli` binaries, type the following commands where `<NewFolderName>` is the folder where you cloned the Cardano Node GitHub respository in the section [Building Cardano Node Binaries](./upgrading-a-node.md#BuildingCN) and `<DestinationPath>` is the absolute file path to the folder where you install Cardano Node binaries on your local computer:
```bash
sudo cp $(find $HOME/git/<NewFolderName>/dist-newstyle/build -type f -name "cardano-node") <DestinationPath>/cardano-node
sudo cp $(find $HOME/git/<NewFolderName>/dist-newstyle/build -type f -name "cardano-cli") <DestinationPath>/cardano-cli
```
{% hint style="info" %}
If you follow the [Coin Cashew](https://www.coincashew.com/) instructions for [Compiling Source Code](../part-i-installation/compiling-source-code.md), then `<DestinationPath>` is `/usr/local/bin`
{% endhint %}

3. To verify that you installed the new Cardano Node binaries successfully, type:
```bash
cardano-node version
cardano-cli version
```

4. Optionally, to install the latest versions of all previously installed packages on your computer, and then reboot the computer, type:
```
sudo apt-get update && sudo apt-get upgrade -y && sudo reboot
```

5. If you need to restart your Cardano node manually, then type the following command where `<CardanoServiceName>` is the name of the systemd service running your Cardano node:
```bash
sudo systemctl start <CardanoServiceName>.service
```  

6. Copy the new `cardano-node` and `cardano-cli` binaries to the air-gapped, offline computer that you use to sign transactions for your stake pool.

## Verifying the Upgrade

To verify that the upgrade is successful, open gLiveView, journactl logs or Grafana dashboard.

{% tabs %}
{% tab title="gLiveView" %}
```bash
cd ${NODE_HOME}
./gLiveView.sh
```
{% endtab %}

{% tab title="journalctl" %}
```
journalctl -fu cardano-node
```
{% endtab %}
{% endtabs %}

Starting a node may take up to 30 minutes. Be patient.

{% hint style="success" %}
Congrats on completing the update. :sparkles:

Did you find our guide useful? Send us a signal with a tip and we'll keep updating it.

It really energizes us to keep creating the best crypto guides.

Use [cointr.ee](https://cointr.ee/coincashew) to find our donation addresses. :pray:

Any feedback and all pull requests much appreciated. :first\_quarter\_moon\_with\_face:

Hang out and chat with our stake pool community on Telegram @ [https://t.me/coincashew](https://t.me/coincashew)
{% endhint %}

## :exploding\_head: Troubleshooting

If your upgrade is unsuccessful, then try [Fixing a Corrupt Blockchain](../part-v-tips/fixing-a-corrupt-blockchain.md) and [Resetting an Installation](../part-v-tips/resetting-an-installation.md), as needed.