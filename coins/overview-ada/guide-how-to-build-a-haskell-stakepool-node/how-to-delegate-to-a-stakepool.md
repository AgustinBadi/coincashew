# How to delegate to a Stakepool

## ✅ 0. Pre-requisites

* Both a payment and a stake key pair. Payment key should contain some ADA.

## 👩💻 1. Register Stake Address

Create a certificate, `stake.cert`, using the `stake.vkey`

```text
cardano-cli shelley stake-address registration-certificate \
    --staking-verification-key-file stake.vkey \
    --out-file stake.cert
```

You need to find the **tip** of the blockchain to set the **ttl** parameter properly.

```
export CARDANO_NODE_SOCKET_PATH=~/cardano-my-node/db/socket
cardano-cli shelley query tip --testnet-magic 42
```

Example **tip** output:

> `Tip (SlotNo {unSlotNo = 510000})`

{% hint style="info" %}
You will want to set your **ttl** value greater than the current tip. In this example, we use 2000000. 
{% endhint %}

Calculate the current minimum fee:

```text
cardano-cli shelley transaction calculate-min-fee \
    --tx-in-count 1 \
    --tx-out-count 1 \
    --ttl 2000000 \
    --testnet-magic 42 \
    --signing-key-file pay.skey \
    --signing-key-file stake.skey \
    --certificate stake.cert \
    --protocol-params-file params.json
```

Result of current minimum fee:

> `runTxCalculateMinFee: 171309`

Build your transaction which will register your stake address.

```text
cardano-cli shelley query utxo \
    --address $(cat pay.addr) \
    --testnet-magic 42
```

Example output:

```text
                 TxHash                         Ix        Lovelace
--------------------------------------------------------------------
81acd93...                                        0      100000000000
```

{% hint style="info" %}
Notice the TxHash and Ix \(index\). Will use this data shortly.
{% endhint %}

Calculate your transaction's change

```text
expr 1000000000 - 400000 - 171309
> 999428691
```

{% hint style="info" %}
Registration of a stake address certificate costs 400000 lovelace.
{% endhint %}

Run the build-raw transaction command

{% hint style="info" %}
Pay close attention to **tx-in**. The data should in the format`<TxHash>#<Ix number>`from above.
{% endhint %}

```text
cardano-cli shelley transaction build-raw \
    --tx-in 81acd93...#0 \
    --tx-out $(cat pay.addr)+999428691\
    --ttl 2000000 \
    --fee 171309 \
    --tx-body-file tx.raw \
    --certificate stake.cert
```

Sign the transaction with both the payment and stake secret keys.

```text
cardano-cli shelley transaction sign \
    --tx-body-file tx.raw \
    --signing-key-file pay.skey \
    --signing-key-file stake.skey \
    --testnet-magic 42 \
    --tx-file tx.signed
```

Send the signed transaction.

```text
cardano-cli shelley transaction submit \
    --tx-file tx.signed \
    --testnet-magic 42
```

## 📄 2. Create a delegation certificate

Given the **stake pool verification key file** `node.vkey` from your stakepool, run the following:

```text
cardano-cli shelley stake-address delegation-certificate \
    --staking-verification-key-file stake.vkey \
    --stake-pool-verification-key-file node.vkey \
    --out-file deleg.cert
```

Next, build then sign and submit your delegation transaction.

```text
cardano-cli shelley transaction calculate-min-fee \
    --tx-in-count 1 \
    --tx-out-count 1 \
    --ttl 2000000 \
    --testnet-magic 42 \
    --signing-key-file pay.skey \
    --signing-key-file stake.skey \
    --certificate deleg.cert \
    --protocol-params-file params.json

> runTxCalculateMinFee: 172805

cardano-cli shelley query utxo \
    --address $(cat pay.addr) \
    --testnet-magic 42

                 TxHash                       TxIx        Lovelace
--------------------------------------------------------------------
32cd839...                                        0       499243830

expr 499243830 - 172805
> 499071025

########################
# Update tx-in with your TxHash and TxIx
########################

cardano-cli shelley transaction build-raw \
    --tx-in 32cd839...#0 \
    --tx-out $(cat pay.addr)+499071025\
    --ttl 2000000 \
    --fee 172805 \
    --out-file tx.raw \
    --certificate deleg.cert

cardano-cli shelley transaction sign \
    --tx-body-file tx.raw \
    --signing-key-file pay.skey \
    --signing-key-file stake.skey \
    --testnet-magic 42 \
    --tx-file tx.signed

cardano-cli shelley transaction submit \
    --tx-file tx.signed \
    --testnet-magic 42
```

{% hint style="success" %}
Congratulations! Your ADA is now successfully delegated to your chosen stakepool.
{% endhint %}

