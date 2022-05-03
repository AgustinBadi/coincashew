# Configuring Stake Pool Topology

## Configuring a Block-producing Node

{% hint style="info" %}
A block producer node will be configured with various key-pairs needed for block generation (cold keys, KES hot keys and VRF hot keys). It can only connect to its relay nodes.
{% endhint %}

{% hint style="info" %}
A relay node will not be in possession of any keys and will therefore be unable to produce blocks. It will be connected to its block-producing node, other relays and external nodes.
{% endhint %}

![](../../../../.gitbook/assets/producer-relay-diagram.png)

{% hint style="success" %}
For the purposes of this guide, we will be building **two nodes** on two **separate servers**. One node will be designated the **block producer node**, and the other will be the relay node, named **relaynode1**.
{% endhint %}

{% hint style="danger" %}
Configure **topology.json** file so that

* relay node(s) connect to public relay nodes (like IOHK) and your block-producer node
* block-producer node **only** connects to your relay node(s)
{% endhint %}

On your **block-producer** node, run the following. Update the **addr** with your relay node's IP address.

{% hint style="warning" %}
**What IP address to use?**

* If both block producer and relay nodes are a local network, then use the relay node's local IP address. `i.e. 192.168.1.123`
* If you are running all nodes in the cloud/VPS , then use the relay node's public IP address. `i.e. 173.45.12.32`
* If your block producer node is local and relay node is in the cloud, then use the relay node's public IP address. `i.e. 173.45.12.32`
{% endhint %}

{% tabs %}
{% tab title="block producer node" %}
```bash
cat > $NODE_HOME/${NODE_CONFIG}-topology.json << EOF 
 {
    "Producers": [
      {
        "addr": "<RELAYNODE1'S IP ADDRESS>",
        "port": 6000,
        "valency": 1
      }
    ]
  }
EOF
```
{% endtab %}
{% endtabs %}

## Configuring a Relay Node

{% hint style="warning" %}
:construction: On your other server that will be designed as your relay node or what we will call **relaynode1** throughout this guide, carefully **repeat steps in Part 1  Installation** in order to build the cardano binaries.
{% endhint %}

{% hint style="info" %}
You can have multiple relay nodes as you scale up your stake pool architecture. Simply create **relaynodeN** and adapt the guide instructions accordingly.
{% endhint %}

On your **relaynode1**, run the following after updating with your block producer's IP address.

{% hint style="warning" %}
**What IP address to use?**

* If both block producer and relay nodes are on a local network, then use the local IP address. `i.e. 192.168.1.123`
* If you are running on a cloud or VPS configuration, then use the block producer's public IP address. `i.e. 173.45.12.32`
* If your block producer node is local and relay node is in the cloud, then use the block producer's public IP address (also known as your local network's public IP address). `i.e. 173.45.12.32`
{% endhint %}

{% tabs %}
{% tab title="relaynode1" %}
```bash
cat > $NODE_HOME/${NODE_CONFIG}-topology.json << EOF 
 {
    "Producers": [
      {
        "addr": "<BLOCK PRODUCER NODE'S IP ADDRESS>",
        "port": 6000,
        "valency": 1
      },
      {
        "addr": "relays-new.cardano-mainnet.iohk.io",
        "port": 3001,
        "valency": 2
      }
    ]
  }
EOF
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Valency tells the node how many connections to keep open. Only DNS addresses are affected. If value is 0, the address is ignored.
{% endhint %}

{% hint style="danger" %}
:sparkles: **Port Forwarding Tip:** You'll need to forward and open ports 6000 to your nodes. Check with [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) or [https://canyouseeme.org/](https://canyouseeme.org) .
{% endhint %}