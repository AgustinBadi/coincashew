---
description: >-
  On Ubuntu/Debian, this guide will illustrate how to install and configure a
  Cardano stake pool from source code on a two node setup with 1 block producer
  node and 1 relay node.
---

# How to Set Up a Cardano Stake Pool

## :wrench: About This Guide

The *How to Set Up a Cardano Stake Pool* guide aims to give you complete, step-by-step instructions to implement and maintain a secure Cardano stake pool operating on the Cardano mainnet blockchain using the currently recommended software versions.

The guide includes the following parts:

- **Part I - Installation** describes how to secure the Linux computers hosting your Cardano stake pool, as well as how to install Cardano node software and dependent software packages.

- **Part II - Configuration** explains how to set up Cardano nodes to create a stake pool.

- **Part III - Operation** discusses how to create your stake pool.

- **Part IV - Administration & Maintenance** provides procedures that you need to manage your stake pool.

- **Part V - Tips** contains additional procedures to simplify managing your stake pool.

## :tada: Introduction

{% hint style="info" %}
If you want to support this free educational Cardano content or found this helpful, visit [cointr.ee to find our donation ](https://cointr.ee/coincashew)addresses. Much appreciated in advance. :pray:
{% endhint %}

{% hint style="success" %}
As of October 30 2021, this is **guide version 5.0.0** and written for **cardano mainnet **with **release v.1.30.1** :grin:
{% endhint %}

## :page\_facing\_up: Changelog - **Update Notes -** **October 30 2021**

* Re-organized content to improve loading speed.
* Contribution by [Change Pool](https://change.paradoxicalsphere.com) - Improved Table of Contents
* Added high-level explanation of Topology API.
* Increased the cardano-node service unit file timeout from 2 to 300 seconds.
* Added a [collection of projects](./see-also.md#projects) built by this amazing community.
* Added cardano-node RTS flags to reduce chance of missed slot leader checks.
* Added Leaderlog changes and improvements
* Increased minimum RAM requirements to 12GB.
* Updated for Alonzo release 1.29.0.
* Various fixes to testnet  / alonzo / storage requirements / cli commands
* Updated CNCLI's Leaderlog command with the [stake-snapshot approach](./part-iii-operation/configuring-slot-leader-calculation.md)
* Added [CNCLI tool](./part-iii-operation/configuring-slot-leader-calculation.md) for sending slot to Pooltool and for LeaderLog scripts
* Updated guide for release cardano-node/cli v1.27.0 changes
* Added [Stake Pool Operator's Best Practices Checklist](./appendix-best-practices-checklist.md)
* Contribution By [Billionaire Pool](https://www.billionairepool.com) - [Guide to monitoring your node security with OSSEC server and Slack.](./part-v-tips/monitoring-node-security-using-ossec-server-and-slack.md)
* Added how to [Secure your pool pledge with a 2nd pool owner using a hardware wallet](./part-iii-operation/securing-your-stake-pool-using-a-hardware-wallet.md)