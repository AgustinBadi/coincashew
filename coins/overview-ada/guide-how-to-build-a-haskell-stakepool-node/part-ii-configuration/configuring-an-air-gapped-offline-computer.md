# Configuring an Air-gapped, Offline Computer

Store and safeguard the sensitive secret (private) keys for your stake pool using an air-gapped, offline computer. The most effective technique to prevent private key exposure is to guarantee that a necessary private key is never held for any length of time on any Internet-connected computer, also known as a hot node. Your air-gapped, offline computer may also be referred to as a cold environment.

Your air-gapped, offline computer:

- Protects against key-logging attacks; malware- or virus-based attacks; and, other firewall or security exploits
- Must not have a wired or wireless network connection
- Is not a virtual machine (VM) on a computer having a network connection
- Is physically isolated from the rest of your network

Read more about requirements to [Air Gap](https://en.wikipedia.org/wiki/Air_gap_(networking)).

## System Requirements

The system requirements for the air-gapped, offline computer that you use to support your stake pool operation are minimal. The computer must support the same operating system that you install on your hot nodes. For example, you may use a Raspberry Pi or an upcycled older computer or laptop.

Your cold environment requires a USB port to facilitate transporting files to and from your block-producing node using a USB stick or other removable media.

## Copying the cardano-cli Binary

Copy the `cardano-cli` binary that you produced when [Compiling Cardano Node](../part-i-installation/compiling-cardano-node.md) to your air-gapped, offline computer.

**To copy the cardano-cli binary to your cold environment:**

1. Insert the removable media that you want to use to transfer files into a hot node where you compiled the `cardano-cli` binary.

2. If you followed the Coin Cashew guide, then copy the `cardano-cli` binary located in the folder `/usr/local/bin/` to the removable media that you inserted in step 1  
{% hint style="info" %}
If you do not know the location of the `cardano-cli` binary, then type `which cardano-cli`
{% endhint %}

3. Eject the removable media from your hot node, and then insert the removable media into your air-gapped, offline computer.

4. On your air-gapped, offline computer, copy the `cardano-cli` binary from the removable media to the `/usr/local/bin/` folder.

5. To give execute permissions to the `cardano-cli` binary, type:
```bash
sudo chmod +x /usr/local/bin/cardano-cli
```

## Setting the NODE_HOME Environment Variable

For convenience when following the Coin Cashew guide, create a `NODE_HOME` environment variable on your air-gapped, offline computer set to the same file path that you set on your block-producing and relay nodes when [Installing GHC and Cabal](../part-i-installation/installing-ghc-and-cabal.md).

**To create a NODE_HOME environment variable:**

1. On your air-gapped, offline computer, open the file `$HOME/.bashrc` using a text editor, and then add the following line at the end of the file:
```bash
export NODE_HOME="$HOME/cardano-my-node"
```

2. To create the folder set for the `NODE_HOME` environment variable in the `$HOME/.bashrc` file, type:
```bash
mkdir $HOME/cardano-my-node
```

3. To reload your shell profile, type:
```bash
source $HOME/.bashrc
```

## <a name="libsecp"></a>Installing libsecp256k1

To use the `cardano-cli` binary on your air-gapped, offline computer you must also install the `libsecp256k1` library that you installed on your hot nodes. Use the following procedure to install `libsecp256k1` without connecting your air-gapped, offline computer to the Internet.

**To install the `libsecp256k1` library on your air-gapped, offline computer:**

1. On the air-gapped, ofline computer, open the `$HOME/.bashrc` file using a text editor, and then add the following lines at the end of the file:
```bash
export LD_LIBRARY_PATH="/usr/local/lib:$LD_LIBRARY_PATH"
export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH"
```

2. Save and close the `$HOME/.bashrc` file.

3. To reload the `$HOME/.bashrc` file, type:
```bash
source $HOME/.bashrc
```

4. Using removable media, copy the following three files from the `/usr/local/lib` folder on a block-producing or relay node where you installed `libsecp256k1` to the `/usr/local/lib` folder on your air-gapped, offline computer:
```bash
libsecp256k1.a
libsecp256k1.la
libsecp256k1.so.0.0.0
```

5. To set file permissions and ownership for the `libsecp256k1` library files on the air-gapped, offline computer, type the following commands using a terminal window:
```bash
cd /usr/local/lib
sudo chown root:root libsecp256k1.*
sudo chmod 644 libsecp256k1.a
sudo chmod 755 libsecp256k1.la
sudo chmod 755 libsecp256k1.so.0.0.0
```

6. To create symbolic links, type:
```bash
sudo ln -s libsecp256k1.so.0.0.0 libsecp256k1.so
sudo ln -s libsecp256k1.so.0.0.0 libsecp256k1.so.0
```

7. Type `ls -la` and then confirm that in step 6 you created the following symbolic links:
```bash
lrwxrwxrwx root root libsecp256k1.so -> libsecp256k1.so.0.0.0
lrwxrwxrwx root root libsecp256k1.so.0 -> libsecp256k1.so.0.0.0
```

8. To update available symbolic links for currently shared libraries, type:
```bash
sudo ldconfig
```