---
description: 検証者になり、eth2、プルーフ・オブ・ステーク・ブロックチェーンの安全性を確保するのに役立ちます。 ETH32をお持ちの方なら誰でも参加できます。
---

# ガイド\| ETH2メインネットステーキングでバリデーターを設定する方法

{% hint style="success" %}
2020年12月5日現在、このガイドは **mainnet 用に更新されています。**😁
{% endhint %}

🎊 **2020-12 Update**: We're on [Gitcoin](https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew), where you can contribute via [quadratic funding](https://vitalik.ca/general/2019/12/07/quadratic.html) and make a big impact. **1 DAI** のコントリビューションは **23 DAI** の試合に相当します。

[をご確認ください](https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew). ありがとうございます!🙏

{% embed url="https://gitcoin.co/grants/1653/eth2-staking-guides-by-constashew" caption="" %}

## 🏁 0. 前提条件

### 💻eth2 バリデータとビーコンノードを操作するためのスキル

eth2 のバリデータとして、通常、以下のような能力があります。

* eth2ビーコンノードと検証器の設置方法、実行、維持方法に関する運用上の知識
* 24時間365日体制で検証を維持するという長期的な取り組みです
* 基本的なオペレーティングシステムのスキル
* has known beginners for Beginners' by Superphiz ['Intro to Eth2 & Staking for Beginners' by Superphiz](https://www.youtube.com/watch?v=tpkpW031RCI)
* [Eth2 Study Master course](https://ethereumstudymaster.com/) に合格または登録されています
* そして、 [8 Things Every Eth2 validatorが知っておくべきである](https://medium.com/chainsafe-systems/8-things-every-eth2-validator-should-know-before-staking-94df41701487)

### 🎗 **最低設定要件**

* **オペレーティングシステム:** 64 ビット Linux \(i.\) Ubuntu 20.04 LTSサーバまたはデスクトップ\)
* **プロセッサ:** デュアルコア CPU、Intel Core i5–760 または AMD FX-8100 以上
* **Memory:** 8GB RAM
* **ストレージ:** 20GB SSD
* **インターネット:** 少なくとも1 Mbpsの速度でブロードバンドのインターネット接続。
* **電力:** 信頼性の高い電力。
* **ETH残高:** 少なくとも32 ETHといくつかのETHをデポジット取引手数料として
* **ウォレット**: メタマスクがインストールされました

### 🏋 推奨ハードウェア設定

* **オペレーティングシステム:** 64 ビット Linux \(i.\) Ubuntu 20.04 LTSサーバまたはデスクトップ\)
* **プロセッサ:** クワッドコア CPU、Intel Core i7-4770 または AMD FX-8310 以上
* **メモリ:** 16GB RAM 以上
* **ストレージ:** 1TB SSD 以上
* **インターネット:** データ制限のない10Mbps以上の速度でブロードバンドインターネット接続。
* **電源:** 無停電電源\(UPS\) による信頼性の高い電力
* **ETH残高:** 少なくとも32 ETHといくつかのETHをデポジット取引手数料として
* **ウォレット**: メタマスクがインストールされました

{% hint style="success" %}
✨ **Pro Validator Tip**: OS、VM、またはマシンの新しいインスタンスから始めることを強くお勧めします。 テストネットキー、ウォレット、データベースを再利用しないことで、頭痛を避けることができます。
{% endhint %}

### 🔓 推奨されるeth2バリデータセキュリティのベストプラクティス。

バリデータを保護するためのアイデアやリマインダーが必要な場合は、こちらを参照してください

### 🛠 Setup Ubuntu

Ubuntu Serverをインストールする必要がある場合は、

{% embed url="https://ubuntu.com/tutorials/install-ubuntu-server\#1-overview" caption="" %}

またはUbuntuデスクトップ [https://www.coincashew.com/coins/overview-xtz/guide-how-to-setup-a-baker/install-ubuntu](https://www.coincashew.com/coins/overview-xtz/guide-how-to-setup-a-baker/install-ubuntu)

### 🎭 メタマスクの設定

メタマスクをインストールする必要がある場合は、[https://www.coincashew.com/wallets/browser-wallets/metamask-ethereum](https://www.coincashew.com/wallets/browser-wallets/metamask-ethereum)

## 🌱 1. ETHの購入/交換/統合

{% hint style="info" %}
あなたが所有するすべての32ETHは、1バリデータを作成することができます。 ビーコンノードで何千ものバリデータを実行できます。
{% endhint %}

ETH \(または32 ETHの倍数\) は、Metamask でアクセス可能な単一のアドレスに統合する必要があります。

ETHを購入/交換する必要がある場合、もしくはETHを32の倍数に補充する必要がある場合は、以下をご覧ください: [https://www.coincashew.com/coins/overview-eth/guide-how-to-buy-eth](https://www.coincashew.com/coins/overview-eth/guide-how-to-buy-eth)

## 👩💻 2. Launchpadでバリデータにサインアップする

1. 依存関係、イーサリアム財団デポジットツールをインストールし、キーペアのあなたの2セットを生成します。

{% hint style="info" %}
各バリデータには2つのキーペアがあります。 **署名キー** と **引き出しキー** これらのキーは 1 つのニーモニックフレーズから派生します。 [キーについての詳細はこちら](https://blog.ethereum.org/2020/05/21/keys/)
{% endhint %}

ビルド済みの [イーサリアム財団デポジットツール](https://github.com/ethereum/eth2.0-deposit-cli) をダウンロードするか、ソースからビルドするかを選択できます。

{% tabs %}
{% tab title="ソースコードからビルド" %}
依存関係のインストール

```text
sudo apt update
sudo apt install python3-pip git -y
```

ソースコードをダウンロードしてインストールします。

```text
cd $HOME
git clone https://github.com/eth2.0-deposit-cli.git eth2deposit-cli
cd eth2deposit-cli
sudo ./deposit.sh install
```

新しいニーモニックを作ります。

```text
./deposit.sh new-mnemonic --chain mainnet
```
{% endtab %}

{% tab title="ビルド済みのeth2deposit-cli" %}
eth2deposit-cliをダウンロード

```bash
cd $HOME
wget https://github.com/eth2.0-deposit-cli/releases/download/v1.1.0/eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz
```

SHA256 チェックサムが、 [リリース ページ](https://github.com/ethereum/eth2.0-deposit-cli/releases/tag/v1.0.0) のチェックサムと一致することを確認します。

```bash
echo "2107f26f954545f423530e3501ae616c222b6bf77774a4f2743effb8fe4bcbe7 *eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz" | shasum -a 256 --check
```

アーカイブを展開します。

```text
tar -xvf eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz
mv eth2deposit-cli-ed5a6d3-linux-amd64 eth2deposit-cli
rm eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz
cd eth2deposit-cli
```

新しいニーモニックを作ります。

```text
./deposit new-mnemonic --chain mainnet
```
{% endtab %}

{% tab title="高度-最も安全" %}
{% hint style="warning" %}
🔥**\[** オプション**\]** セキュリティのヒント: **eth2deposit-cli ツール** を実行し、 **mnemonic seed** を作成します。 **usb から起動されたエアーギャップ付きオフラインマシンの** 上で、バリデータキーを生成します。
{% endhint %}

起動可能な usb を作る際に、この [ethstaker.cc](https://ethstaker.cc/) をローダウン専用にフォローしてください。

### パート1 - Ubuntu 20.04 USBブート可能なドライブを作成

{% embed url="https://www.youtube.com/watch?v=DTR3PzRRtYU" %}

### パート2 - USBドライブからUbuntu 20.04をインストールします

{% embed url="https://www.youtube.com/watch?v=C97\_6MrufCE" caption="" %}

USBキーを介して、USBから起動されたエアラップされたオフラインマシンにオンラインマシンから、事前に構築されたeth2deposit-cliバイナリをコピーすることができます。 イーサネットケーブルおよび/またはWi-Fiを取り外してください。
{% endtab %}
{% endtabs %}

2. 指示に従って、パスワードを選択します。 あなたのニーモニックを書き留めて、この安全と **オフライン**を維持してください。

3. [https://launchpad.ethereum.org/](https://launchpad.ethereum.org/) の手順に従って、完了したばかりの手順をスキップします。 eth2フェーズ0の概要材料を研究する。 eth2を理解することが成功の鍵です!

4.`validator_keys` ディレクトリにある `deposit_data-#########.json` をアップロードします。

5. メタマスクウォレット、レビューと受け入れ条件でランチパッドに接続します。

6. トランザクションを確認\(s\). バリデーターごとに32ETHのデポジットトランザクションがあります。

{% hint style="info" %}
あなたの取引はあなたのETHを [公式ETH2入金契約アドレスに送金します。 ](https://blog.ethereum.org/2020/11/04/eth2-quick-update-no-19/)

**Check**, _double-check_, _**triple-check**_ that the official Eth2 deposit contract address is correct.[`0x00000000219ab540356cBB839Cbe05303d7705Fa`](https://etherscan.io/address/0x00000000219ab540356cbb839cbe05303d7705fa)
{% endhint %}

{% hint style="danger" %}
🔥 **重要な暗号リマインダー:** **あなたの記憶を保ち、ETHを保ちましょう。**🚀

* ニーモニックシード **オフライン**を書き留めてください。 _電子メールではありません。 クラウドではありません。_
* 複数のコピーが良い。 _\_ \[_メタルシードに最も保存されている。\_\]\([https://jlopp.github.io/metal-bitcoin-storage-reviews/](https://jlopp.github.io/metal-bitcoin-storage-reviews/)\)
* 今後、このニーモニックから引き出しキーが生成されます。
* **\*\*ディレクトリの** `validator_keys`\*\* などのオフラインバックアップ \` を作成します。

  &lt;/p&gt;
{% endhint %}

## 🛸 3. Install a ETH1 node

{% hint style="info" %}
 Ethereum 2.0は、32 ETHバリデータデポジットを監視するためにEthereum 1.0への接続が必要です。 独自のEthereum 1.0ノードをホストすることは、分散化を最大化し、Infuraなどの第三者への依存を最小限に抑える最良の方法です。
{% endhint %}

{% hint style="warning" %}
その後の手順は、 ベストプラクティスのセキュリティガイドが完了したことを前提としています。🛑 **ROOT** ユーザとしてプロセスを実行しないでください。 😱
{% endhint %}

どちらかを選択 [**OpenEthereum**](https://www.parity.io/ethereum/)**,** [**Geth**](https://geth.ethereum.org/)**,** [**Besu**](https://besu.hyperledger.org/)**,** [**Nethermind**](https://www.nethermind.io/), [**Infura**](https://infura.io/) **or** [**Chainstack**](https://chainstack.com/)**.**

{% tabs %}
{% tab title="OpenEthereum" %}
{% hint style="info" %}
 **OpenEthereum** - \*\*ゴールは **Rust プログラミング言語** を使用して、最速で最も軽く、最も安全なイーサリアムクライアントになることです。 OpenEthereumはGPLv3の下でライセンスされており、すべてのEthereumの必要性に使用できます。
{% endhint %}

#### ⚙ 依存関係のインストール

```text
sudo apt-get update
sudo apt-get install curl jq unzip -y
```

#### 🤖 Install OpenEthereum

最新のリリースは [https://github.com/openetherum/openetherum/releases](https://github.com/openethereum/openethereum/releases)

最新のLinuxリリース、zip解除、実行権限の追加、クリーンアップを自動的にダウンロードします。

```bash
mkdir $HOME/openethereum
cd $HOME/openethereum
curl -s https://api.github.com/repos/openethereum/openethereum/releases/latest | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux | xargs wget -q --show-progress
unzip -o openethereum*.zip
chmod +x openethereum
rm openethereum*.zip
```

 ⚙ **systemdの設定と設定**

**eth1.service** の設定を定義する `ユニットファイル` を作成するには、以下を実行します。

単純に以下をコピー/ペーストします。

```bash
cat > $HOME/eth1.service << EOF 
[Unit]
Description     = openethereum eth1 service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/openethereum/openethereum --metrics --metrics-port=6060
Restart         = on-failure
RestartSec      = 3

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="info" %}
**Nimbus**特定の構成: **ExecStart** ラインに次のフラグを追加します。

```bash
--ws-origins=all
```
{% endhint %}

ユニットファイルを `/etc/systemd/system` に移動し、アクセス許可を与えます。

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

起動時に自動起動を有効にするには、以下を実行します。

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1
```

#### ⛓ OpenEthereumを開始

```text
sudo systemctl start eth1
```
{% endtab %}

{% tab title="Geth" %}
{% hint style="info" %}
**Geth** - Go Ethereumは、Ethereumプロトコルの\(C++とPythonとともに\)3つの元の実装の1つです。 それは **Go**で書かれており、完全にオープンソースであり、GNU LGPL v3 の下でライセンスされています。
{% endhint %}

[https://github.com/etherum/go-ethereum/releases](https://github.com/ethereum/go-ethereum/releases)

#### 🧬 リポジトリからインストール

```text
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update -y
sudo apt-get install ethereum -y
```

⚙ **systemdの設定と設定**

**eth1.service** の設定を定義する `ユニットファイル` を作成するには、以下を実行します。

単純に以下をコピー/ペーストします。

```bash
cat > $HOME/eth1.service << EOF 
[Unit]
Description     = geth eth1 service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = /usr/bin/geth --http --metrics --pprof
Restart         = on-failure
RestartSec      = 3
TimeoutSec      = 300

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="info" %}
**Nimbus Specific Configuration**: **ExecStart** ラインに次のフラグを追加します。

```bash
--ws
```
{% endhint %}

ユニットファイルを `/etc/systemd/system` に移動し、アクセス許可を与えます。

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

起動時に自動起動を有効にするには、以下を実行します。

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1
```

#### ⛓ 開始 geth

```text
sudo systemctl start eth1
```
{% endtab %}

{% tab title="Besu" %}
{% hint style="info" %}
**Hyperledger Besu** はオープンソースのイーサリアムクライアントで、プライベートネットワークで安全で高性能なトランザクション処理を必要とする要求の厳しいエンタープライズアプリケーション向けに設計されています。 Apache 2.0 ライセンスの下で開発され、 **Java** で書かれています。
{% endhint %}

#### 🧬 Java の依存関係をインストール

```text
sudo apt update
sudo apt install openjdk-11-jdk -y
```

#### 🌜 Besuをダウンロードして解凍します。

最新のリリースは [https://github.com/hyperledger/besu/releases](https://github.com/hyperledger/besu/releases)

ファイルは [https://dl.bintray.com/hyperledger-org/besu-repo からダウンロードできます](https://dl.bintray.com/hyperledger-org/besu-repo)

```text
cd
wget -O besu.tar.gz https://dl.bintray.com/hyperledger-org/besu-repo/besu-20.10.1.tar.gz
tar -xvf besu.tar.gz
rm besu.tar.gz
mv besu* besu
```

⚙ **systemdの設定と設定**

**eth1.service** の設定を定義する `ユニットファイル` を作成するには、以下を実行します。

単純に以下をコピー/ペーストします。

```bash
cat > $HOME/eth1.service << EOF 
[Unit]
Description     = besu eth1 service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/besu/bin/besu --metrics-enabled --rpc-http-enabled --data-path="$HOME/.besu"
Restart         = on-failure
RestartSec      = 3

[Install]
WantedBy    = multi-user.target
EOF
```

ユニットファイルを `/etc/systemd/system` に移動し、アクセス許可を与えます。

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

起動時に自動起動を有効にするには、以下を実行します。

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
**Nethermind** は、パフォーマンスと柔軟性に関する主要なイーサリアムクライアントです。 Nethermindは、広く普及しているエンタープライズフレンドリーなプラットフォームである.NETコア上に構築されており、安定性、信頼性、データの整合性、セキュリティを見失うことなく、既存のインフラストラクチャとの統合を簡単にします。
{% endhint %}

#### ⚙ 依存関係のインストール

```text
sudo apt-get update
sudo apt-get install curl libsnappy-dev libc6-dev jq libc6 unzip -y
```

#### 🌜 ネザーマインドをダウンロードして解凍する

最新のリリースは [https://github.com/NethermindEth/nethermind/releases で確認してください](https://github.com/NethermindEth/nethermind/releases)

最新のLinuxリリース、ZIP解除、クリーンアップを自動的にダウンロードします。

```bash
mkdir $HOME/nethermind 
cd $HOME/nethermind
curl -s https://api.github.com/repos/NethermindEth/nethermind/releases/latest | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux  | xargs wget -q --show-progress
unzip -o nethermind*.zip
rm nethermind*linux*.zip
```

⚙ **systemdの設定と設定**

**eth1.service** の設定を定義する `ユニットファイル` を作成するには、以下を実行します。

単純に以下をコピー/ペーストします。

```bash
cat > $HOME/eth1.service << EOF 
[Unit]
Description     = nethermind eth1 service
Wants           = network-online.target
After           = network-online.target 

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/nethermind/Nethermind.Runner --baseDbPath $HOME/.nethermind --Metrics.Enabled true --JsonRpc.Enabled true --Sync.DownloadBodiesInFastSync true --Sync.DownloadReceiptsInFastSync true --Sync.AncientBodiesBarrier 11052984 --Sync.AncientReceiptsBarrier 11052984
Restart         = on-failure
RestartSec      = 3

[Install]
WantedBy    = multi-user.target
EOF
```

ユニットファイルを `/etc/systemd/system` に移動し、アクセス許可を与えます。

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

起動時に自動起動を有効にするには、以下を実行します。

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1
```

#### ⛓ Start Nethermind

```text
sudo systemctl start eth1
```
{% endtab %}

{% tab title="最小限のハードウェアセットアップ" %}
{% hint style="info" %}
無限のディスク領域の設定に適しています。 可能な場合は、常に独自のフルeth1ノードを実行します。
{% endhint %}

[https://infura.io/](https://infura.io/) の API アクセスキーにサインアップ

1. 無料アカウントにサインアップします。
2. メールアドレスを確認してください。
3. ダッシュボード [https://infura.io/dashboard](https://infura.io/dashboard) をご覧ください
4. プロジェクトを作成し、名前を付けます。
5. ENDPOINTとして **Mainnet** を選択します
6. 以下にあるeth2クライアントの特定の設定に従ってください。

{% hint style="success" %}
代わりに、 [https://etherumnodes.com/](https://ethereumnodes.com/) で無料のイーサリアムノードを使用してください。
{% endhint %}

## ニンバス特有の設定

1. システムの **ユニットファイル**を作成するときは、このエンドポイントで `--web-url` パラメータを更新します。
2. WebSocket のエンドポイントをコピーします。 `wss://` で開始
3. これをステップ4で保存し、eth2ノードを設定します。

```bash
# 例
--web3-url=<your wss:// infura endpoint>
```

## 鉄工固有の構成

1. `/etc/teku/teku.yaml` にある `teku.yaml`を作成した後、このエンドポイントで `--eth1-endpoint` パラメータを更新します。
2. http エンドポイントをコピーします。 `http://`で開始
3. これをステップ4で保存し、eth2ノードを設定します。

```bash
# 例
eth1-endpoint: <your https:// infura endpoint>
```

## 灯台固有の構成

1. **beacon chain systemd** **unit file**を作成するときは、このエンドポイントで `--eth1-endpoint` パラメータを追加します。
2. **https** エンドポイントをコピーします。 `https://` で始まります
3. これをステップ4で保存し、eth2ノードを設定します。

```bash
# 例
--eth1-endpoint=<your https:// infura endpoint>
```

## プライズム固有の設定

1. **beacon chain systemd unit file**を作成するときは、このエンドポイントで `--http-web3provider` パラメータを更新してください。
2. **https** エンドポイントをコピーします。 `https://` で始まります
3. これをステップ4で保存し、eth2ノードを設定します。

```bash
# 例
--http-web3provider=<your https:// infura endpoint>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
eth1ノードの同期には最大1週間かかることがあります。 ギガビットインターネットを搭載したハイエンドマシンでは、同期に1日以内かかることを期待しています。
{% endhint %}

{% hint style="success" %}
eth1 ノードは、これらのイベントが発生したときに完全に同期されます。

* **`OpenEthereum:`** `Imported #<block number>`
* **`Geth:`** `新しいチェーンセグメントをインポート`
* **`Besu:`** `インポート済み #<block number>`
* **`Nethermind:`** `古いヘッダを同期しなくなりました`
{% endhint %}

#### 🛠 eth1.service commands

  🗒 **eth1ログを表示してフォローする**

```text
journalctl -u eth1 -f
```

🗒 **eth1 サービスを停止するには**

```text
sudo systemctl stop eth1
```

## 🌜 4. ETH2ビーコンチェーンノードとバリデータを設定する

Lighthouse、Nimbus、Teku、Prysm、Lodestarのいずれかをお選びください。

{% tabs %}
{% tab title="Lighthouse" %}
{% hint style="info" %}
[灯台](https://github.com/sigp/lighthouse) は Eth2.0 クライアントで、スピードとセキュリティに重点を置いています。 背後にいるチーム [シグマ・プライム](https://sigmaprime.io/), は、Ethereum財団、Consensysys、および個人とともにLighthouseに資金を提供した情報セキュリティおよびソフトウェアエンジニアリング会社です。 LighthouseはRustに構築され、Apache 2.0ライセンスの下で提供されています。
{% endhint %}

## ⚙ 4.1. Rust の依存関係をインストール

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

デフォルトのインストールを続行するには'1'を入力してください。

環境変数を更新します。

```bash
echo export PATH="$HOME/.cargo/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
```

錆の依存関係をインストールします。

```text
sudo apt-get update
sudo apt install -y git gcc g++ make cmake pkg-config libssl-dev
```

## 💡 4.2. ソースから灯台を作る

```bash
mkdir ~/git
cd ~/git
git clone https://github.com/sigp/lighthouse.git
cd lighthouse
git fetch --all && git checkout stable && git pull
make
```

{% hint style="info" %}
コンパイルエラーの場合は、次のシーケンスを実行します。

```text
rustup update
cargo clean
make
```
{% endhint %}

{% hint style="info" %}
このビルドプロセスには数分かかることがあります。
{% endhint %}

バージョン番号を確認して灯台が正しくインストールされていることを確認します。

```text
lighthouse --version
```

## 🎩 4.3. バリデータキーをインポート

{% hint style="info" %}
Lighthouseにキーをインポートすると、バリデータ署名キー\(s\)は `$HOME/.lighthouse/mainnet/validators` フォルダに保存されます。
{% endhint %}

以下のコマンドを実行して、validator キーを eth2deposit-cli ツールディレクトリからインポートします。

アカウントをインポートするキーストアのパスワードを入力します。

```bash
lighthouse account validator import --network mainnet --directory=$HOME/eth2deposit-cli/validator_keys
```

アカウントが正常にインポートされたことを確認します。

```bash
lighthouse account_manager validator list --network mainnet
```

{% hint style="danger" %}
**警告**: 他のリンクを有効にするためには、元のキーを使用しないでください。さもないと、SLASHEDを取得します。
{% endhint %}

## 🔥 4.4. ポート転送やファイアウォールの設定

ネットワーク設定またはクラウドプロバイダ設定に固有の [バリデータのファイアウォールポートがオープンで到達可能であることを確認してください。](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **灯台ビーコンチェーン** は、tcpとudpのためにポート9000が必要
* **eth1** 節点は、tcpとudp のポート30303が必要です

{% hint style="info" %}
✨ **ポート転送 ヒント:** ポートをバリデータに転送して開く必要があります。 [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) または [https://canyouseeme.org/](https://canyouseeme.org/) で動作していることを確認してください。
{% endhint %}

## ⛓ 4.5. ビーコンチェーンを開始する

#### 🍰 ビーコンチェーンにsystemdを使用する利点 <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. メンテナンス、停電などによるコンピュータの再起動時にビーコンチェーンを自動起動します。
2. クラッシュしたビーコンチェーンプロセスを自動的に再起動します。
3. ビーコンチェーンの稼働時間とパフォーマンスを最大化します。

#### 🛠 Systemd のセットアップ手順

**beacon-chain.service** の設定を定義するために、次のように`ユニットファイル` を作成します。 コピーして貼り付けるだけです。

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
ExecStart       = $(which lighthouse) bn --staking --validator-monitor-auto --metrics --network mainnet
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="info" %}
🔥 **Lighthouse ヒント:** **ExecStart** 行で `--eth1-endpoints` フラグを追加すると、冗長な eth1 ノードが使用できます。 カンマで区切ってください。 エンドポイントが末尾のスラッシュで終わらないこと、または`/` それを削除してください。

```bash
# 例:
--eth1-endpoints http://localhost:8545,https://nodes.mewapi.io/rpc/eth,https://mainnet.eth.cloud.ava.do,https://mainnet.infura.io/v3/xxx
```
{% endhint %}

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

ファイル権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

起動時に自動起動を有効にし、ビーコンノードサービスを開始するには、以下を実行します。

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="info" %}
**よくある問題のトラブルシューティング**:

_ビーコンチェーンは:8545サービスに接続できませんでしたか?_

* \[Service\]の下のビーコンチェーンユニットファイルに追加します。 "`ExecStartPre = /bin/slep 30`" " 接続する前に eth1 ノードが起動するまで 30 秒待つようにします。

_CRIT 無効な eth1 チェーン id . 正しいチェーンIDに切り替えてください。_

* eth1 ノードが mainnet に完全に同期することを許可します。
{% endhint %}

{% hint style="success" %}
よくやった。 あなたのビーコンチェーンは、システムの信頼性と堅牢性によって管理されています。 systemdを使用するためのコマンドを以下に示します。
{% endhint %}

### 🛠 systemd コマンドをいくつか使います。

#### ✅ビーコンチェーンがアクティブかどうかを確認します

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 ビーコンチェーンの状態を表示

```text
sudo systemctl status beacon-chain
```

#### 🔄 ビーコンチェーンを再起動する

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 ビーコンチェーンを停止します

```text
sudo systemctl stop beacon-chain
```

#### :file\_cabiet: ログの表示とフィルタリングを行う

```bash
#view and follow the log
journalctl --unit=beacon-chain -f
#view log since yesterday
journctl --unit=beacon-chain --since=today
#view log since today
journalctl --unit=beacon-chain --since=today
#view log between a date
journctl ----unit=beacon-chain --since='202020-12-02 12:00:00:00' -since='20202020-12-02 12:00:00:00'
```

## 🧬 4.6. バリデータを開始する

#### 🚀 グラフィティとポイントのセットアップ

`落書き`をセットアップし、バリデータが正常に提案し、POAPトークンを獲得するブロックに含まれるカスタムメッセージを設定します。 [ここでイーサリアム1.0アドレスを指定してPOAP文字列を生成します。](https://beaconcha.in/poap)

`MY_GRAFFITI` 変数を設定するには、次のコマンドを実行します。 `<my POAP string or message>` を単一引用符で置き換えます。

```bash
MY_GRAFFITI== '<my POAP string or message>'
# 例
# MY_GRAFFITI='poapAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
[POAP - 出席証明トークンの詳細について。 ](https://www.poap.xyz/)
{% endhint %}

#### 🍰 バリデータに systemd を使用する利点 <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. メンテナンス、停電などによりコンピュータが再起動すると、バリデータを自動的に開始します。
2. クラッシュしたバリデータプロセスを自動的に再起動します。
3. バリデータの稼働時間とパフォーマンスを最大化します。

#### 🛠 Systemd のセットアップ手順

**validator.service** の設定を定義する`ユニットファイル` を作成するには、以下を実行します。 コピーして貼り付けるだけです。

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
ExecStart       = $(which lighthouse) vc --network mainnet --graffiti "${MY_GRAFFITI}" --metrics 
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

ファイル権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```

起動時に自動起動を有効にし、バリデータを開始するには、以下を実行します。

```text
sudo systemctl daemon-reload
sudo systemctl enable validator
sudo systemctl start validator
```

{% hint style="success" %}
よくやった。 現在、バリデータはシステムの信頼性と堅牢性によって管理されています。 systemdを使用するためのコマンドを以下に示します。
{% endhint %}

### 🛠 systemd コマンドをいくつか使います。

#### ✅バリデータが有効かどうかを確認します

```text
sudo systemctl is-active validator
```

#### 🔎 バリデータの状態を表示

```text
sudo systemctl status validator
```

#### 🔄 バリデータを再起動する

```text
sudo systemctl reload-or-restart validator
```

#### 🛑 バリデータの停止

```text
sudo systemctl stop validator
```

#### :file\_cabiet: ログの表示とフィルタリングを行う

```bash
#view and follow the log
journalctl --unit=validator -f
#view log since yesterday
journctl --unit=validator --since=today
#view log since today
journalctl --unit=validator --since=today
#view log between a date
journctl ---unit=validator --since='202020-12-01 00-01 00:00:00' -until='20202020-12-02 12:00:00:00:00:00'
```
{% endtab %}

{% tab title="Nimbus" %}
{% hint style="info" %}
[Nimbus](https://our.status.im/tag/nimbus/) は、イーサリアム2の研究プロジェクトとクライアント実装です。 リソースに制限がある古いスマートフォンを含む、組込みシステムやパーソナルモバイルデバイスでうまく動作するように設計されています。 Nimbusチームは [ステータス](https://status.im/about/) の出身で、 [メッセージングapp/wallet/Web3ブラウザ](https://status.im/) で最もよく知られています。 Nimbus \(Apache 2\) は C にコンパイルする Python のような構文を持つ言語である Nimで書かれています。
{% endhint %}

## ⚙ 4.1. ソースから Nimbus をビルド

依存関係のインストール

```text
sudo apt-get update
sudo apt-get install curl build-essential git -y
```

Nimbus をインストールしてビルドします。

```bash
mkdir ~/git 
cd ~/git
git clone https://github.com/status-im/imbus-eth2
cd nimbus-eth2
make NIMFLAGS="-d:insecure" nimbus_beacon_node
```

{% hint style="info" %}
ビルドプロセスには数分かかることがあります。
{% endhint %}

ヘルプを表示してNimbusが正しくインストールされていることを確認します。

```bash
cd $HOME/git/nimbus-eth2/build
./nimbus_beacon_node --help
```

バイナリファイルを `/usr/bin` にコピーします

```bash
sudo cp $HOME/git/nimbus-eth2/build/nimbus_beacon_node /usr/bin
```

## 🎩 4.2. バリデータキーをインポート <a id="6-import-validator-key"></a>

ニンバスデータを格納するディレクトリ構造を作成します。

```bash
sudo mkdir -p /var/lib/nimbus
```

このディレクトリの所有権を取得し、正しい権限レベルを設定します。

```bash
sudo chown $(whoami):$(whoami) /var/lib/nimbus
sudo chmod 700 /var/lib/nimbus
```

以下のコマンドはバリデータキーをインポートします。

アカウントをインポートするキーストアのパスワードを入力します。

```bash
cd $HOME/git/nimbus-eth2
build/nimbus_beacon_node deposits import --data-dir=/var/lib/nimbus $HOME/eth2deposit-cli/validator_keys
```

これで、ディレクトリ一覧を行うことでアカウントが正常にインポートされたことを確認できます。

```bash
ll /var/lib/nimbus/validators
```

それぞれのバリデータのpubkeyにちなんで名付けられたフォルダが表示されます。

{% hint style="info" %}
キーをNimbusにインポートすると、検証ツールの署名キーが/ var / lib / nimbusフォルダーのシークレットと検証ツールの下に保存されます。

`secrets` フォルダには、すべてのバリデータキーにアクセスできる共通の秘密が含まれています。

`バリデータ` フォルダには、署名する keystore\(s\) \(暗号化された鍵\) が含まれています。 キーストアは、キーを交換するためのメソッドとしてバリデータによって使用されます。

キーとキーストアの詳細については、こちら  を参照してください。
{% endhint %}

{% hint style="danger" %}
**警告**: 他のリンクを有効にするためには、元のキーを使用しないでください。さもないと、SLASHEDを取得します。
{% endhint %}

## 🔥 4.3. ポート転送やファイアウォールの設定

ネットワーク設定またはクラウドプロバイダ設定に固有の [バリデータのファイアウォールポートがオープンで到達可能であることを確認してください。](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Nimbus beacon chain node** 使用しますport 9000 for tcp.
* **eth1** 節点は、tcpとudp のポート30303が必要です

{% hint style="info" %}
✨ **ポート転送 ヒント:** ポートをバリデータに転送して開く必要があります。 [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) または [https://canyouseeme.org/](https://canyouseeme.org/) で動作していることを確認してください。
{% endhint %}

## 🏂 4.4. ビーコンチェーンとバリデータを開始する

{% hint style="info" %}
Nimbusはビーコンチェーンとバリデータの両方を1つのプロセスに統合します。
{% endhint %}

#### 🚀 グラフィティとポイントのセットアップ

`落書き`をセットアップし、バリデータが正常に提案し、POAPトークンを獲得するブロックに含まれるカスタムメッセージを設定します。 [ここでイーサリアム1.0アドレスを指定してPOAP文字列を生成します。](https://beaconcha.in/poap)

`MY_GRAFFITI` 変数を設定するには、次のコマンドを実行します。 `<my POAP string or message>` を単一引用符で置き換えます。

```bash
MY_GRAFFITI== '<my POAP string or message>'
# 例
# MY_GRAFFITI='poapAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
[POAP - 出席証明トークンの詳細について。 ](https://www.poap.xyz/)
{% endhint %}

#### 🍰 あなたのビーコンチェーンとバリデータに systemd を使用する利点 <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. メンテナンス、停電などによるコンピュータの再起動時にビーコンチェーンを自動起動します。
2. クラッシュしたビーコンチェーンプロセスを自動的に再起動します。
3. ビーコンチェーンの稼働時間とパフォーマンスを最大化します。

#### 🛠 セットアップ手順

**beacon-chain.service** の設定を定義するために、次のように`ユニットファイル` を作成します。 コピーして貼り付けるだけです。

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
ExecStart       = /bin/bash -c '/usr/bin/nimbus_beacon_node --network=mainnet --graffiti="${MY_GRAFFITI}" --data-dir=/var/lib/nimbus --web3-url=ws://127.0.0.1:8546 --metrics --metrics-port=8008 --rpc --rpc-port=9091 --validators-dir=/var/lib/nimbus/validators --secrets-dir=/var/lib/nimbus/secrets --log-file=/var/lib/nimbus/beacon.log'
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="warning" %}
Nimbus は ETH1 ノードの websocket 接続 \("ws://" と "wss://"\' のみをサポートしています。 Geth, OpenEthereum, Infura and Chainstack ETH1 nodes are verified compatible.
{% endhint %}

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

ファイル権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

起動時に自動起動を有効にし、ビーコンノードサービスを開始するには、以下を実行します。

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
よくやった。 あなたのビーコンチェーンは、システムの信頼性と堅牢性によって管理されています。 systemdを使用するためのコマンドを以下に示します。
{% endhint %}

### 🛠 systemd コマンドをいくつか使います。

#### ✅ビーコンチェーンがアクティブかどうかを確認します

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 ビーコンチェーンの状態を表示

```text
sudo systemctl status beacon-chain
```

#### 🔄 ビーコンチェーンを再起動する

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 ビーコンチェーンを停止します

```text
sudo systemctl stop beacon-chain
```

#### :file\_cabiet: ログの表示とフィルタリングを行う

```bash
#view and follow the log
journalctl --unit=beacon-chain -f
#view log since yesterday
journctl --unit=beacon-chain --since=today
#view log since today
journalctl --unit=beacon-chain --since=today
#view log between a date
journctl ----unit=beacon-chain --since='202020-12-02 12:00:00:00' -since='20202020-12-02 12:00:00:00'
```
{% endtab %}

{% tab title="Teku" %}
{% hint style="info" %}
[PegaSys Teku](https://pegasys.tech/teku/) \(旧 Artemis\)は、制度的なニーズとセキュリティ要件を満たすように設計されたJavaベースのEthereum 2.0クライアントです。 & PegaSys は [ConsenSys](https://consensys.net/) の部門であり、エンタープライズ対応のクライアントとコアイーサリアムプラットフォームとやり取りするためのツールを構築することに専念しています。 Teku is Apache 2 licensed and written in Java, a language notable for its materity & ubiquity.
{% endhint %}

## ⚙ 4.1 ソースからTekuをビルド

インストール git.

```text
sudo apt-get install git -y
```

Java 11をインストールします。

**Ubuntu 20.x**の場合、以下を使用してください

```text
sudo apt update
sudo apt install openjdk-11-jdk -y
```

**Ubuntu 18.x**の場合、以下を使用してください

```text
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-set-default -y
```

Java 11以降がインストールされていることを確認します。

```bash
Java --version
```

Tekuをインストールしてビルドします。

```bash
mkdir ~/git
cd ~/git
git clone https://github.com/ConsenSys/teku.git
cd teku
./gradlew distTar installDist
```

{% hint style="info" %}
このビルドプロセスには数分かかることがあります。
{% endhint %}

バージョンを表示することでTekuが正しくインストールされていることを確認します。

```bash
cd $HOME/git/teku/build/install/teku/bin
./teku --version
```

tekuバイナリファイルを `/usr/bin/teku`にコピーします

```bash
sudo cp -r $HOME/git/teku/build/install/teku /usr/bin/teku
```

## 🔥 4.2. ポート転送やファイアウォールの設定

ネットワーク設定またはクラウドプロバイダ設定に固有の [バリデータのファイアウォールポートがオープンで到達可能であることを確認してください。](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Tekuビーコンチェーンノード** はポート9000をtcpとudpに使用します
* **eth1** 節点は、tcpとudp のポート30303が必要です

{% hint style="info" %}
\*\*\*✨ **ポート転送のヒント:** ポートをバリデータに転送して開く必要があります。 [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) または [https://canyouseeme.org/](https://canyouseeme.org/) で動作していることを確認してください。
{% endhint %}

## 🏂 4.3. ビーコンチェーンとバリデータを設定する

{% hint style="info" %}
Tekuはビーコンチェーンとバリデータの両方を1つのプロセスに統合します。
{% endhint %}

鉄工用のディレクトリ構造を設定します。

```bash
sudo mkdir -p /var/lib/teku
sudo mkdir -p /etc/teku
sudo chown $(whoami):$(whoami) /var/lib/teku
```

`validator_files` ディレクトリを上記で作成したデータディレクトリにコピーし、余分なデポジット\_dataファイルを削除します。

```bash
cp -r $HOME/eth2deposit-cli/validator_keys /var/lib/teku
rm /var/lib/teku/validator_keys/deposit_data*
```

{% hint style="danger" %}
**警告**: 他のリンクを有効にするためには、元のキーを使用しないでください。さもないと、SLASHEDを取得します。
{% endhint %}

バリデータのパスワードをファイルに保存します。

`echo` の後に引用符間のパスワードを更新します。

```bash
echo 'my_password_goes_here' > $HOME/validators-password.txt
sudo mv $HOME/validators-password.txt /etc/teku/validators-password.txt
sudo chmod 600 /etc/teku/validators-password.txt
```

#### 🚀 グラフィティとポイントのセットアップ

`落書き`をセットアップし、バリデータが正常に提案し、POAPトークンを獲得するブロックに含まれるカスタムメッセージを設定します。 [ここでイーサリアム1.0アドレスを指定してPOAP文字列を生成します。](https://beaconcha.in/poap)

`MY_GRAFFITI` 変数を設定するには、次のコマンドを実行します。 `<my POAP string or message>` を単一引用符で置き換えます。

```bash
MY_GRAFFITI== '<my POAP string or message>'
# 例
# MY_GRAFFITI='poapAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
[POAP - 出席証明トークンの詳細について。 ](https://www.poap.xyz/)
{% endhint %}

Teku設定ファイルを生成します。 コピーして貼り付けるだけです。

```bash
cat > $HOME/teku.yaml << EOF
# network
network: "mainnet"

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

設定ファイルを `/etc/teku`に移動する

```bash
sudo mv $HOME/teku.yaml /etc/teku/teku.yaml
```

## 🎩 4.4 インポートバリデータキー

{% hint style="info" %}
バリデータキーのディレクトリを指定する場合、Tekuは同じ名前のキーストアとパスワードファイルを見つけることを期待します。

例えば `keystore-m_12221_3600_1_0_11222333.json` と `keystore-m_12221_3600_1_0_11222333.txt`
{% endhint %}

すべてのバリデータに対応するパスワードファイルを作成します。

```bash
for f in /var/lib/teku/validator_keys/keystore*.json; do cp /etc/teku/validators-password.txt /var/lib/teku/validator_keys/$(basename $f .json).txt; done
```

バリデータのキーストアとバリデータのパスワードが存在することを確認するには、以下のディレクトリを確認します。

```bash
ll /var/lib/teku/validator_keys
```

## 🏁 4.5. ビーコンチェーンとバリデータを開始する

**systemd** を使用して、tekuの開始と停止を管理します。

#### 🍰 あなたのビーコンチェーンとバリデータに systemd を使用する利点 <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. メンテナンス、停電などによるコンピュータの再起動時にビーコンチェーンを自動起動します。
2. クラッシュしたビーコンチェーンプロセスを自動的に再起動します。
3. ビーコンチェーンの稼働時間とパフォーマンスを最大化します。

#### 🛠 セットアップ手順

**beacon-chain.service** の設定を定義するために、次のように`ユニットファイル` を作成します。

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
WantedBy	= multi-user.target
EOF
```

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

ファイル権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

起動時に自動起動を有効にし、ビーコンノードサービスを開始するには、以下を実行します。

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
よくやった。 あなたのビーコンチェーンは、システムの信頼性と堅牢性によって管理されています。 systemdを使用するためのコマンドを以下に示します。
{% endhint %}

### 🛠 systemd コマンドをいくつか使います。

#### ✅ビーコンチェーンがアクティブかどうかを確認します

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 ビーコンチェーンの状態を表示

```text
sudo systemctl status beacon-chain
```

#### 🔄 ビーコンチェーンを再起動する

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 ビーコンチェーンを停止します

```text
sudo systemctl stop beacon-chain
```

#### :file\_cabiet: ログの表示とフィルタリングを行う

```bash
#view and follow the log
journalctl --unit=beacon-chain -f
#view log since yesterday
journctl --unit=beacon-chain --since=today
#view log since today
journalctl --unit=beacon-chain --since=today
#view log between a date
journctl ----unit=beacon-chain --since='202020-12-02 12:00:00:00' -since='20202020-12-02 12:00:00:00'
```
{% endtab %}

{% tab title="Prysm" %}
{% hint style="info" %}
[Prysm](https://github.com/prysmaticlabs/prysm) は、使いやすさ、セキュリティ、および信頼性に焦点を当てた、Ethereum 2.0プロトコルのGo実装です。 Prysmは、クライアントの開発に唯一の焦点を当てている [Prysmatic Labs](https://prysmaticlabs.com/)によって開発されました。 PrismはGoに書かれており、GPL-3.0ライセンスの下でリリースされています。
{% endhint %}

## ⚙ 4.1. Install Prysm

```bash
mkdir ~/prysm && cd ~/prysm 
curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output prysm.sh && chmod +x prysm.sh 
```

## 🔥 4.2. ポート転送やファイアウォールの設定

ネットワーク設定またはクラウドプロバイダ設定に固有の [バリデータのファイアウォールポートがオープンで到達可能であることを確認してください。](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Prysm beacon chain node** は udp にポート 12000 を tcp に使用する
* **eth1** 節点は、tcpとudp のポート30303が必要です

{% hint style="info" %}
✨ **ポート転送 ヒント:** ポートをバリデータに転送して開く必要があります。 [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) または [https://canyouseeme.org/](https://canyouseeme.org/) で動作していることを確認してください。
{% endhint %}

## 🎩 4.3. バリデータキーをインポート

利用規約に同意し、デフォルトのウォレットの場所に同意し、新しいパスワードを入力してウォレットを暗号化し、インポートしたアカウントのパスワードを入力します。

```bash
$HOME/prysm/prysm.sh validator accounts import --mainnet --keys-dir=$HOME/eth2deposit-cli/validator_keys
```

インポートされたバリデータの確認に成功しました。

```bash
$HOME/prysm/prysm.sh validator accounts list --mainnet
```

バリデータのパブキーがリストされていることを確認します。

> \#出力例:
>
> 1つのバリデータアカウントを表示する \`validator accounts list --show-deposit-data を実行して、アカウントの eth1 デポジットトランザクションデータを表示する
>
> Account 0 \| pens-brother-heat  
> \[validating public key\] 0x2374.....7121

{% hint style="danger" %}
**警告**: 他のリンクを有効にするためには、元のキーを使用しないでください。さもないと、SLASHEDを取得します。
{% endhint %}

## 🏂 4.4. ビーコンチェーンを開始する

#### 🍰 あなたのビーコンチェーンとバリデータに systemd を使用する利点 <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. メンテナンス、停電などによるコンピュータの再起動時にビーコンチェーンを自動起動します。
2. クラッシュしたビーコンチェーンプロセスを自動的に再起動します。
3. ビーコンチェーンの稼働時間とパフォーマンスを最大化します。

#### 🛠 セットアップ手順

**beacon-chain.service** の設定を定義するために、次のように`ユニットファイル` を作成します。 コピーして貼り付けるだけです。

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
ExecStart       = $(echo $HOME)/prysm/prysm.sh beacon-chain --mainnet --p2p-max-peers=45 --http-web3provider=http://127.0.0.1:8545 --accept-terms-of-use 
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

ファイル権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

起動時に自動起動を有効にし、ビーコンノードサービスを開始するには、以下を実行します。

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
よくやった。 あなたのビーコンチェーンは、システムの信頼性と堅牢性によって管理されています。 systemdを使用するためのコマンドを以下に示します。
{% endhint %}

### 🛠 systemd コマンドをいくつか使います。

#### ✅ビーコンチェーンがアクティブかどうかを確認します

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 ビーコンチェーンの状態を表示

```text
sudo systemctl status beacon-chain
```

#### 🔄 ビーコンチェーンを再起動する

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 ビーコンチェーンを停止します

```text
sudo systemctl stop beacon-chain
```

#### 🗒 ログを表示およびフィルタリングする

```bash
#view and follow the log
journalctl --unit=beacon-chain -f
#view log since yesterday
journctl --unit=beacon-chain --since=today
#view log since today
journalctl --unit=beacon-chain --since=today
#view log between a date
journctl ----unit=beacon-chain --since='202020-12-02 12:00:00:00' -since='20202020-12-02 12:00:00:00'
```

## 🧬 4.5. バリデータを開始する <a id="9-start-the-validator"></a>

バリデータのパスワードをファイルに保存し、読み取り専用にします。

```bash
echo 'my_password_goes_here' > $HOME/.eth2validators/validators-password.txt
sudo chmod 600 $HOME/.eth2validators/validators-password.txt
```

#### 🚀 グラフィティとポイントのセットアップ

`落書き`をセットアップし、バリデータが正常に提案し、POAPトークンを獲得するブロックに含まれるカスタムメッセージを設定します。 [ここでイーサリアム1.0アドレスを指定してPOAP文字列を生成します。](https://beaconcha.in/poap)

`MY_GRAFFITI` 変数を設定するには、次のコマンドを実行します。 `<my POAP string or message>` を単一引用符で置き換えます。

```bash
MY_GRAFFITI== '<my POAP string or message>'
# 例
# MY_GRAFFITI='poapAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
[POAP - 出席証明トークンの詳細について。 ](https://www.poap.xyz/)
{% endhint %}

コマンドラインから手動で、またはsystemdで自動的にバリデータを実行することができます。

#### 🍰 バリデータに systemd を使用する利点 <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. メンテナンス、停電などによりコンピュータが再起動すると、バリデータを自動的に開始します。
2. クラッシュしたバリデータプロセスを自動的に再起動します。
3. バリデータの稼働時間とパフォーマンスを最大化します。

#### 🛠 systemd のセットアップ手順

**validator.service** の設定を定義する`ユニットファイル` を作成するには、以下を実行します。 コピーして貼り付けるだけです。

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
ExecStart       = $(echo $HOME)/prysm/prysm.sh validator --mainnet --graffiti "${MY_GRAFFITI}" --accept-terms-of-use --wallet-password-file $(echo $HOME)/.eth2validators/validators-password.txt
Restart         = on-failure

[Install]
WantedBy	= multi-user.target
EOF
```

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

ファイル権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```

起動時に自動起動を有効にし、バリデータを開始するには、以下を実行します。

```text
sudo systemctl daemon-reload
sudo systemctl enable validator
sudo systemctl start validator
```

### 🛠 systemd コマンドをいくつか使います。

#### ✅バリデータが有効かどうかを確認します

```text
sudo systemctl is-active validator
```

#### 🔎 バリデータの状態を表示

```text
sudo systemctl status validator
```

#### 🔄 バリデータを再起動する

```text
sudo systemctl reload-or-restart validator
```

#### 🛑 バリデータの停止

```text
sudo systemctl stop validator
```

#### :file\_cabiet: ログの表示とフィルタリングを行う

```bash
#view and follow the log
journalctl --unit=validator -f
#view log since yesterday
journctl --unit=validator --since=today
#view log since today
journalctl --unit=validator --since=today
#view log between a date
journctl ---unit=validator --since='202020-12-01 00-01 00:00:00' -until='20202020-12-02 12:00:00:00:00:00'
```
{% endtab %}

{% tab title="Lodestar" %}
{% hint style="info" %}
**Lodestar is a Typescript implementation** of the official [Ethereum 2.0 specification](https://github.com/ethereum/eth2.0-specs) by the [ChainSafe.io](https://lodestar.chainsafe.io/) team. ビーコンチェーンクライアントに加えて、チームは22のパッケージとライブラリにも取り組んでいます。 完全なリストは [こちら](https://hackmd.io/CcsWTnvRS_eiLUajr3gi9g) にあります。 最後に ロデスタチームは、軽いクライアントの研究開発においてEth2スペースを率いており、この目的のためにEFとMoloch DAOから資金を受けています。
{% endhint %}

## ⚙ 4.1 ソースからロデスタを作る

curl と git をインストールします。

```bash
sudo apt-get install gcc g++ make git curl -y
```

yarnをインストールします。

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install yarn -y
```

yarn が正しくインストールされていることを確認します。

```bash
yarn --version
# バージョンを出力する必要がある >= 1.22.4
```

Install nodejs.

```text
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

nodejsが正しくインストールされていることを確認します。

```bash
nodejs -v
# バージョンを出力する必要がある >= v12.18.3
```

Lodestarをインストールしてビルドします。

```bash
mkdir ~/git
cd ~/git
git clone https://github.com/chainsafe/lodestar.git
cd lodestar
yarn install --ignore-optional
yarn run build
```

{% hint style="info" %}
このビルドプロセスには数分かかることがあります。
{% endhint %}

ヘルプメニューを表示してロデスタが正しくインストールされていることを確認します。

```text
./lodestar --help
```

## 🔥 4.2. ポート転送やファイアウォールの設定

ネットワーク設定またはクラウドプロバイダ設定に固有の [バリデータのファイアウォールポートがオープンで到達可能であることを確認してください。](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Lodestar beacon chain node** は、tcpにポート30607、udp peer discoveryにポート9000を使用します。
* **eth1** 節点は、tcpとudp のポート30303が必要です

{% hint style="info" %}
\*\*\*✨ **ポート転送のヒント:** ポートをバリデータに転送して開く必要があります。 [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) または [https://canyouseeme.org/](https://canyouseeme.org/) で動作していることを確認してください。
{% endhint %}

## 🎩 4.3. バリデータキーをインポート

```bash
./lodestar account validator import \
  --network mainnet \
  --directory $HOME/eth2deposit-cli/validator_keys
```

アカウントをインポートするキーストアのパスワードを入力します。

キーが正しくインポートされたことを確認します。

```text
./lodestar account validator list --network mainnet
```

{% hint style="danger" %}
**警告**: 他のリンクを有効にするためには、元のキーを使用しないでください。さもないと、SLASHEDを取得します。
{% endhint %}

## 🏂 4.4. ビーコンチェーンとバリデータを開始する

システムでビーコンチェーンを自動的に実行します。

#### 🍰 ビーコンチェーンにsystemdを使用する利点 <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. メンテナンス、停電などによるコンピュータの再起動時にビーコンチェーンを自動起動します。
2. クラッシュしたビーコンチェーンプロセスを自動的に再起動します。
3. ビーコンチェーンの稼働時間とパフォーマンスを最大化します。

#### 🛠 セットアップ手順

**beacon-chain.service** の設定を定義するために、次のように`ユニットファイル` を作成します。 コピーして貼り付けるだけです。

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
ExecStart       = $(echo $HOME)/git/lodestar/lodestar beacon --network mainnet --eth1.providerUrl http://localhost:8545 --metrics.enabled true --metrics.serverPort 8008
Restart         = on-failure

[Install]
WantedBy	= multi-user.target
EOF
```

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

ファイル権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

起動時に自動起動を有効にし、ビーコンノードサービスを開始するには、以下を実行します。

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
よくやった。 あなたのビーコンチェーンは、システムの信頼性と堅牢性によって管理されています。 systemdを使用するためのコマンドを以下に示します。
{% endhint %}

### 🛠 systemd コマンドをいくつか使います。

#### ✅ビーコンチェーンがアクティブかどうかを確認します

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 ビーコンチェーンの状態を表示

```text
sudo systemctl status beacon-chain
```

#### 🔄 ビーコンチェーンを再起動する

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 ビーコンチェーンを停止します

```text
sudo systemctl stop beacon-chain
```

#### :file\_cabiet: ログの表示とフィルタリングを行う

```bash
#view and follow the log
journalctl --unit=beacon-chain -f
#view log since yesterday
journctl --unit=beacon-chain --since=today
#view log since today
journalctl --unit=beacon-chain --since=today
#view log between a date
journctl ----unit=beacon-chain --since='202020-12-02 12:00:00:00' -since='20202020-12-02 12:00:00:00'
```

## 🧬 4.5. バリデータを開始する

#### 🚀 グラフィティとポイントのセットアップ

`落書き`をセットアップし、バリデータが正常に提案し、POAPトークンを獲得するブロックに含まれるカスタムメッセージを設定します。 [ここでイーサリアム1.0アドレスを指定してPOAP文字列を生成します。](https://beaconcha.in/poap)

`MY_GRAFFITI` 変数を設定するには、次のコマンドを実行します。 `<my POAP string or message>` を単一引用符で置き換えます。

```bash
MY_GRAFFITI== '<my POAP string or message>'
# 例
# MY_GRAFFITI='poapAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
[POAP - 出席証明トークンの詳細について。 ](https://www.poap.xyz/)
{% endhint %}

systemdで自動的にバリデータを実行します。

#### 🍰 バリデータに systemd を使用する利点 <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. メンテナンス、停電などによりコンピュータが再起動すると、バリデータを自動的に開始します。
2. クラッシュしたバリデータプロセスを自動的に再起動します。
3. バリデータの稼働時間とパフォーマンスを最大化します。

#### 🛠 セットアップ手順

**validator.service** の設定を定義する`ユニットファイル` を作成するには、以下を実行します。 コピーして貼り付けるだけです。

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
ExecStart       = $(echo $HOME)/git/lodestar/lodestar validator --network mainnet --graffiti "${MY_GRAFFITI}"
Restart         = on-failure

[Install]
WantedBy	= multi-user.target
EOF
```

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

ファイル権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```

起動時に自動起動を有効にし、バリデータを開始するには、以下を実行します。

```text
sudo systemctl daemon-reload
sudo systemctl enable validator
sudo systemctl start validator
```

{% hint style="success" %}
よくやった。 現在、バリデータはシステムの信頼性と堅牢性によって管理されています。 systemdを使用するためのコマンドを以下に示します。
{% endhint %}

### 🛠 systemd コマンドをいくつか使います。

#### ✅バリデータが有効かどうかを確認します

```text
sudo systemctl is-active validator
```

#### 🔎 バリデータの状態を表示

```text
sudo systemctl status validator
```

#### 🔄 バリデータを再起動する

```text
sudo systemctl reload-or-restart validator
```

#### 🛑 バリデータの停止

```text
sudo systemctl stop validator
```

#### :file\_cabiet: ログの表示とフィルタリングを行う

```bash
#view and follow the log
journalctl --unit=validator -f
#view log since yesterday
journctl --unit=validator --since=today
#view log since today
journalctl --unit=validator --since=today
#view log between a date
journctl ---unit=validator --since='202020-12-01 00-01 00:00:00' -until='20202020-12-02 12:00:00:00:00:00'
```
{% endtab %}
{% endtabs %}

## 🕒5. 時刻同期

{% hint style="info" %}
ビーコンチェーンは認証を行い、ブロックを生成するために正確な時間に依存しています。 あなたのコンピュータの時間は0以内に実際のNTPまたはNTS時間に対して正確でなければなりません。 秒
{% endhint %}

以下のガイドで **Chrony** をセットアップしてください。

{% embed url="https://www.coincashew.com/coins/overview-ada/guide-how-to-build-a-haskell-stakepool-node/how-to-setup-chrony" %}

{% hint style="info" %}
chronyはNetwork Time Protocolの実装であり、コンピュータの時刻をNTPと同期させるのに役立ちます。
{% endhint %}

## 🔎6. GrafanaとPrometheusでバリデータを監視する

Prometheusは、これらのターゲットのメトリックHTTPエンドポイントをスクレイピングすることによって、監視対象からメトリックを収集する監視プラットフォームです。 [公式ドキュメントはこちらから入手できます。](https://prometheus.io/docs/introduction/overview/) Grafana は収集されたデータを視覚化するために使用されるダッシュボードです。

### 🐣 6.1 インストール

prometheusとprometheusノードエクスポーターをインストールします。

```text
sudo apt-get install -y prometheus prometheus-node-exporter
```

Grafanaをインストールします。

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" > grafana.list
sudo mv grafana.list /etc/apt/sources.list.d/grafana.list
sudo apt-get update && sudo apt-get install -y grafana
```

自動的に起動するようにサービスを有効にします。

```bash
sudo systemctl enable grafana-server.service prometheus.service prometheus-node-exporter.service
```

**prometheus.yml** 設定ファイルを作成します。 eth2クライアントのタブを選択します。 コピーして貼り付けるだけです。

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

**eth1 ノード** の prometheus をセットアップします。 **prometheus.yml** を編集することから始める

```bash
nano $HOME/prometheus.yml
```

eth1 ノードの適用可能なジョブスニペットを **prometheus.yml** の末尾に追加します。 ファイルを保存します。

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

オランダのモニタリングには [Prometheus Pushgateway](https://github.com/prometheus/pushgateway) が必要です。 以下のコマンドでインストールします。

```bash
sudo apt-get install -y prometheus-pushgateway
```

{% hint style="info" %}
Pushgateway は、ネザーマインドからポート9091のデータをリッスンします。
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
{% endtabs %}

`/etc/prometheus/prometheus.yml` に移動します。

```bash
sudo mv $HOME/prometheus.yml /etc/prometheus/prometheus.yml
sudo chmod 644 /etc/prometheus/prometheus.yml
```

最後に、サービスを再起動します。

```bash
sudo systemctl restart grafana-server.service prometheus.service prometheus-node-exporter.service
```

サービスが正常に実行されていることを確認します:

```text
sudo systemctl status grafana-server.service prometheus.service prometheus-node-exporter.service
```

{% hint style="info" %}
💡 **リマインダー**: 別のマシンから監視情報を表示する場合は、ファイアウォールやポート転送時にポート3000が開いていることを確認します。
{% endhint %}

### :antera\_bars: 6.2 Grafana ダッシュボードの設定

1. ローカルブラウザで [http://localhost:3000](http://localhost:3000) または [http://&lt;your](http://<your) validator's ip address&gt;:3000 を開きます。
2. **admin** / **admin** でログイン
3. パスワードの変更
4. **設定歯車** アイコンをクリックし、 **データソースを追加**
5. **Prometheus** を選択してください
6. **の名前** を **"Prometheus** " に設定する
7. **URL** を [http://localhost:9090 に設定](http://localhost:9090)
8. **保存 & テスト** をクリックします
9. **ETH2クライアントのjsonファイルを** ダウンロードして保存します。 \[ [Lighthouse BC ](https://raw.githubusercontent.com/sigp/lighthouse-metrics/master/dashboards/Summary.json)\| [Lighthouse VC](https://raw.githubusercontent.com/sigp/lighthouse-metrics/master/dashboards/ValidatorClient.json) \| [Teku ](https://grafana.com/api/dashboards/13457/revisions/2/download)\| [Nimbus ](https://raw.githubusercontent.com/status-im/nimbus-eth2/master/grafana/beacon_nodes_Grafana_dashboard.json)\| [Prysm ](https://raw.githubusercontent.com/GuillaumeMiralles/prysm-grafana-dashboard/master/less_10_validators.json)\| [Prysm &gt; 10 Validators](https://raw.githubusercontent.com/GuillaumeMiralles/prysm-grafana-dashboard/master/more_10_validators.json) \| Lodestar \]
10. ETH1クライアントのjsonファイルをダウンロードして保存します \[ [Geth](https://gist.githubusercontent.com/karalabe/e7ca79abdec54755ceae09c08bd090cd/raw/3a400ab90f9402f2233280afd086cb9d6aac2111/dashboard.json) \| [Besu ](https://grafana.com/api/dashboards/10273/revisions/5/download)\| [Nethermind ](https://raw.githubusercontent.com/NethermindEth/metrics-infrastructure/master/grafana/dashboards/nethermind.json)\| [OpenEthereum ](https://raw.githubusercontent.com/dappnode/DAppNodePackage-openethereum/master/openethereum-grafana-dashboard.json)\]
11. **Create +** icon &gt; **Import**
12. **アップロードJSONファイル**からETH2クライアントダッシュボードを追加
13. 必要に応じて、Prometheus を **Data Source** として選択します。
14. **インポート** ボタンをクリックします。
15. ETH1クライアントダッシュボードの手順11-14を繰り返します。

{% hint style="info" %}
🔥 **一般的なGrafanaの問題のトラブルシューティング**:

_ダッシュボードにはeth1ノードデータは表示されません。_

* /etc/systemd/system/eth1.serviceにあるeth1ユニットファイルで、レポートメトリックとpprof httpサーバーが有効になるように、eth1ノード/ gethが正しいパラメーターで開始されていることを確認します。
  * 例:`ExecStartPre = /usr/bin/geth --http --metrics --pprof`
{% endhint %}

#### ETH2クライアントごとのGrafana Dashboardの例。

{% tabs %}
{% tab title="Lighthouse" %}
![](../../.gitbook/assets/lhm.png)

![](../../.gitbook/assets/lighthouse-validator.png)

クレジット: [https://github.com/sigp/lighthouse-metrics/](https://github.com/sigp/lighthouse-metrics/)
{% endtab %}

{% tab title="Nimbus" %}
![](../../.gitbook/assets/nim_dashboard.png)

クレジット: [https://github.com/status-im/nimbus-eth2/](https://github.com/status-im/nimbus-eth2/)
{% endtab %}

{% tab title="Teku" %}
![](../../.gitbook/assets/teku.dash.png)

クレジット: [https://grafana.com/grafana/dashboards/13457](https://grafana.com/grafana/dashboards/13457)
{% endtab %}

{% tab title="Prysm" %}
![](../../.gitbook/assets/prysm_dash.png)

Credits: [https://github.com/GuillaumeMiralles/prysm-grafana-dashboard](https://github.com/GuillaumeMiralles/prysm-grafana-dashboard)
{% endtab %}

{% tab title="Lodestar" %}
作業中です。
{% endtab %}
{% endtabs %}

#### ETH1ノードごとのGrafana Dashboardの例。

{% tabs %}
{% tab title="Geth" %}
![](../../.gitbook/assets/geth-dash.png)

クレジット: [https://gist.github.com/karalabe/e7ca79abdec54755ceae09c08bd090cd](https://gist.github.com/karalabe/e7ca79abdec54755ceae09c08bd090cd)
{% endtab %}

{% tab title="Besu" %}
![](../../.gitbook/assets/besu-dash.png)

クレジット: [https://grafana.com/dashboards/10273](https://grafana.com/dashboards/10273)
{% endtab %}

{% tab title="Nethermind" %}
![](../../.gitbook/assets/nethermind-dash.png)

Credits: [https://github.com/NethermindEth/metrics-インフラストラクチャ](https://github.com/NethermindEth/metrics-infrastructure)
{% endtab %}

{% tab title="OpenEthereum" %}
![](../../.gitbook/assets/openethereum-dashboard.png)
{% endtab %}
{% endtabs %}

### ⚠ 6.3 アラート通知の設定

{% hint style="info" %}
バリデータがオフラインになった場合に通知を受け取るためのアラートを設定します。
{% endhint %}

バリデータに関する問題の通知を受け取ります。 電子メール、電報、discord、またはスラックのいずれかを選択します。

{% tabs %}
{% tab title="Email Notifications" %}
1. [https://beaconcha.in/](https://beaconcha.in/) をご覧ください
2. アカウントにサインアップします。
3. **メールアドレス** を確認してください
4. **バリデータの公開アドレス**を検索する
5. **ブックマーク** をクリックして、ウォッチリストにバリデーターを追加します。
{% endtab %}

{% tab title="Telegram Notifications" %}
1. Grafanaのメニューで、ベルアイコンの下にある **通知チャンネル** を選択します。
2. **Add channel** をクリックします。
3. 通知チャンネルに **の名前** を付けます。
4. タイプリストから **テレグラム** を選択します。
5. **Telegram API の設定**を完了するには、 **Telegram チャンネル** と **ボット** が必要です。 `@Botfather`でボットをセットアップする方法については、Telegramのドキュメントの [このセクション](https://core.telegram.org/bots#6-botfather) を参照してください。 BotのAPIトークンを作成する必要があります。
6. 新しい電報グループを作成します。
7. ボットを新しいグループに招待します。
8. グループに少なくとも1つのメッセージを入力して初期化します。
9. [をご覧ください`https://api.telegram.org/botXXX:YYY/getUpdates`](https://api.telegram.org/botXXX:YYY/getUpdates) ここで `XXX:YYY` はあなたのBOTAPIトークンです。
10. JSON 応答で、 **チャット ID** を見つけてコピーします。 **チャット** と **タイトル** の間で検索します。 _チャットIDの例_: `-1123123123`

    ```text
    "chat":{"id":-123123,"title":
    ```

11. **Grafana** の対応するフィールドに **チャット ID** を貼り付けます。
12. **アラートの通知チャンネルを** 保存してテストします。
13. ダッシュボードからカスタムアラートを作成できるようになりました。 [アラートの作成方法については、こちらをご覧ください。](https://grafana.com/docs/grafana/latest/alerting/create-alerts/)
{% endtab %}

{% tab title="Discord Notifications" %}
1. Grafanaのメニューで、ベルアイコンの下にある **通知チャンネル** を選択します。
2. **Add channel** をクリックします。
3. 通知チャンネルに **の名前** を追加します。
4. タイプリストから **Discord** を選択します。
5. 設定を完了するには、Discordサーバーの\(およびテキストチャンネルが利用可能\)とWebhookのURLが必要です。 Discordの Webhook の設定方法については、ドキュメントの [このセクション](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks) を参照してください。
6. Discordの通知設定パネルにWebhook **URL** を入力します。
7. **Send Test**をクリックすると、Discordチャンネルに確認メッセージが送信されます。
{% endtab %}

{% tab title="Slack Notifications" %}
1. Grafanaのメニューで、ベルアイコンの下にある **通知チャンネル** を選択します。
2. **Add channel** をクリックします。
3. 通知チャンネルに **の名前** を追加します。
4. **リストから** を選択します。
5. Slack の Incoming Webhooks のセットアップ方法については、ドキュメントの [このセクション](https://api.slack.com/messaging/webhooks) を参照してください。
6. Slack Incoming Webhook URL を **URL** フィールドに入力します。
7. **Send Test**をクリックすると、Slackチャンネルに確認メッセージが送信されます。
{% endtab %}
{% endtabs %}

### 🌊 6.4 アップタイムによるモニタリング Google Cloud によるチェック

{% hint style="info" %}
誰が見張りをしているのか。 Uptime Checkのような外部サードパーティツールを使用してください。 電源障害やハードウェア障害などの災害の際に検証器が機能していることを もっと確信させることができる これらのシナリオでは、PrometheusとGrafanaによる前述の監視も機能しなくなる可能性があります。

[Mohamed Mansourへの貢献は、このガイド](https://www.youtube.com/watch?v=txgOVDTemPQ) を鼓舞してくれた。
{% endhint %}

GoogleのUptime Checkという無償監視サービスのセットアップ方法は次のとおりです。

{% hint style="info" %}
ビデオのデモは、 [MohamedMansourのeth2教育ビデオ](https://www.youtube.com/watch?v=txgOVDTemPQ) をご覧ください。 彼の [GITCOIN助成金](https://gitcoin.co/grants/1709/video-educational-grant)を支援してください。 🙏
{% endhint %}

1. [cloud.google.com](https://cloud.google.com/) を訪問
2. 検索フィールドで **モニタリング** を検索します。
3. **モニタリングを開始するプロジェクトを選択** をクリックします。
4. **新規プロジェクト** をクリックします。
5. **\*\* プロジェクトに名前を付け、** 作成をクリックします。\*\*
6. format@@0メニューから、新しいプロジェクトを選択します。
7. 右側の列にモニタリングカードがあります。 _\*\*_ をクリックします。
8. 左のメニューで **アップタイムチェック** をクリックし、 **UPTIMEチェックを作成します。**
9. タイトルを入力してください。例: _**Geth ノード**_
10. プロトコルを _**TCP**_ として選択してください
11. パブリックIPアドレスとポート番号を入力します。 すなわち、ip=**7.55.6.3** and port=**30303**
12. ご希望の周波数を選択してください。例: **5分**
13. チェックする最も近い地域を選択してください。 「次へ」をクリックします。
14. 通知チャンネルを作成する。 **通知チャンネルの管理をクリックします。**
15. ご希望の設定を選択してください。 Slack、Webhook、Eメール、SMSのいずれかまたはすべてから選択します。
16. 「稼働時間を確認する」ウィンドウに戻ります。
17. format@@0フィールドで、format@@1ボタンをクリックして新しい通知チャンネルをロードします。
18. 希望する通知を選択します。
19. **TEST** をクリックして、通知が正しく設定されていることを確認します。
20. **CREATE** をクリックして終了します。

{% hint style="success" %}
🎉バリデーターの設定おめでとうございます！ あなたはeth2.0に行くのが良いです。

私たちのガイドが役に立ちましたか？ ヒントを教えてください。アップデートを続けます。

[coint.eeを使用して、寄付 ](https://cointr.ee/coincashew)アドレスを見つけてください。 🙏

どんなフィードバックとすべてのプルリクエストは非常に高く評価されます。 🌛

Discordで仲間のステーカーとチャットしましょう @

[https://discord.gg/w8Bx8W2HPW](https://discord.gg/w8Bx8W2HPW)😃
{% endhint %}

## 🧙7. Update a ETH2 client

新しいリリースがカットされた場合は、最新の安定リリースに更新したいと思います。 以下に、eth2ビーコンチェーンとバリデータを更新する方法を示します。

{% hint style="info" %}
更新前に **git ログを`git ログ`** または **リリース ノート** で常に確認してください。 注意が必要な変更があるかもしれません。
{% endhint %}

ETH2クライアントを選択します。

{% tabs %}
{% tab title="Lighthouse" %}
リリースノートを確認し、破損している変更/機能を確認してください。

[https://github.com/sigp/lighthouse/releases](https://github.com/sigp/lighthouse/releases)

最新のソースを取得してビルドします。

```bash
cd $HOME/git/lighthouse
git fetch --all && git checkout stable && git pull
make
```

新しいバージョン番号を確認してビルドが完了したことを確認します。

```bash
lighthouse --version
```

通常の操作手順に従ってビーコンチェーンとバリデータを再起動します。

```text
sudo systemctl reload-or-restart beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus" %}
リリースノートを確認し、破損している変更/機能を確認してください。

[https://github.com/status-im/nimbus-eth2/releases](https://github.com/status-im/nimbus-eth2/releases)

最新のソースを取得してビルドします。

```bash
cd $HOME/git/nimbus-eth2
git pull && make update
make NIMFLAGS="-d:insecure" nimbus_beacon_node
```

新しいバージョン番号を確認してビルドが完了したことを確認します。

```bash
cd $HOME/git/nimbus-eth2/build
./nimbus_beacon_node --version
```

通常の操作手順に従って、新しいバイナリをコピーし、ビーコンチェーンとバリデータを再起動します。

```bash
sudo systemctl stop beacon-chain
sudo rm /usr/bin/nimbus_beacon_node
sudo cp $HOME/git/nimbus-eth2/build/nimbus_beacon_node /usr/bin
sudo systemctl reload-or-restart beacon-chain
```
{% endtab %}

{% tab title="Teku" %}
リリースノートを確認し、破損している変更/機能を確認してください。

[https://github.com/ConsenSys/teku/releases](https://github.com/ConsenSys/teku/releases)

最新のソースを取得してビルドします。

```bash
cd $HOME/git/teku
git pull
./gradlew distTar installDist
```

新しいバージョン番号を確認してビルドが完了したことを確認します。

```bash
cd $HOME/git/teku/build/install/teku/bin
./teku --version
```

通常の操作手順に従ってビーコンチェーンとバリデータを再起動します。

```bash
sudo systemctl stop beacon-chain
sudo rm -rf /usr/bin/teku
sudo cp -r $HOME/git/teku/build/install/teku /usr/bin/teku
sudo systemctl reload-or-restart beacon-chain
```
{% endtab %}

{% tab title="Prysm" %}
リリースノートを確認し、破損している変更/機能を確認してください。 [https://github.com/prysmaticlabs/prysm/releases](https://github.com/prysmaticlabs/prysm/releases)

```bash
#プロセス
sudo systemctl reload-or-restart beacon-chain validator
```
{% endtab %}

{% tab title="Lodestar" %}
リリースノートを確認し、破損している変更/機能を確認してください。

[https://github.com/ChainSafe/lodestar/releases](https://github.com/ChainSafe/lodestar/releases)

最新のソースを取得してビルドします。

```bash
cd $HOME/git/lodestar
git pull
yarn install --ignore-optional
yarn run build
```

新しいバージョン番号を確認してビルドが完了したことを確認します。

```bash
./lodestar --version
```

通常の操作手順に従ってビーコンチェーンとバリデータを再起動します。

```text
sudo systemctl reload-or-restart beacon-chain validator
```
{% endtab %}
{% endtabs %}

ログを確認して、サービスが正常に動作していることを確認し、エラーがないことを確認してください。

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

## 🔥8. 追加の役に立つヒント

### 🛑 8.1 バリデータを任意に終了する

{% hint style="info" %}
このコマンドを使用して、バリデーターで検証を停止する意図を知らせます。 これは、もはやあなたのバリデータとステークしたくなくなり、ノードをオフにしたいことを意味します。

* 自発的な終了は、最低2048エポック\(または〜9日間\)かかります。 終了するキューがあり、バリデータが最終的に終了するまでの遅延があります。
* フェーズ0でバリデータを終了すると、元に戻すことができなくなり、再度バリデーションを再開することはできません。
* あなたの資金は、フェーズ1.5以降まで引き出しできません。
* あなたのバリデータが終了キューを離れ、本当に終了したら、ビーコンノードとバリデータをオフにしても安全です。
{% endhint %}

{% tabs %}
{% tab title="Lighthouse" %}
```bash
lighthouse account validator exit \
--keystore $HOME/.lighthouse/mainnet/validators \
--beacon-node http://localhost:5052 \
--network mainnet
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

### 🗝 8.2 ニーモニックフレーズを確認

eth2deposit-cli ツールを使用して、 `validator_keys` を復元して同じeth2キーペアを再生成できるようにします。

```bash
cd $HOME/eth2deposit-cli 
./deposit.sh existing-mnemonic --chain mainnet
```

{% hint style="info" %}
両方の **pubkey** が **キーストアファイル** で同一の **の場合、** これは、ニーモニックフレーズが非常に正しいことを意味します。 塩漬けのために他のフィールドは異なるでしょう。
{% endhint %}

### 🤖8.3 バリデーターを追加する

eth2deposit-cli ツールを使用して、新しい入金データファイルと `validator_keys` を作成することで、バリデータをさらに追加できます。

まず、既存の `validator_key` ディレクトリをバックアップし、最後に日付を追加します。

```bash
cd $HOME/eth2deposit-cli
mv validator_key validator_key_$(date +"%Y%d%m-%H%M%S")
```

たとえば、もともと3つのバリデータを作成しましたが、さらに5つのバリデータを追加したい場合は、以下のコマンドを使用できます。

```bash
cd $HOME/eth2deposit-cli
./deposit.sh existing-mnemonic --validator_start_index 3 --num_validators 5 --chain mainnet
```

`deposit_data-#########.json` を発射台のサイトにアップロードし、対応する32 ETHの入金取引を行う手順を完了します。

Finish by stopping your validator, importing the new validator key\(s\), restarting your validator and verifying the logs confirming everything still work without error.

### 💸 8.4 Eth2 クライアントをスラッシュ保護で切り替える / 移行

{% hint style="info" %}
このプロセスの重要なポイントは、同時に2つのeth2クライアントを実行することを避けることです。 あなたはエーテルの損失を引き起こすスラッシングペナルティによって罰せられることを避けたい。
{% endhint %}

#### 🛑 8.4.1 古いビーコンチェーンと古いバリデータを停止します。

スラッシング データベースをエクスポートするには、バリデータを停止する必要があります。

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

#### 💽 8.4.2 スラッシュデータベースのエクスポート \(オプション\)

{% hint style="info" %}
[EIP-3076](https://eips.ethereum.org/EIPS/eip-3076) は、eth2 クライアント間の安全移行バリデータ鍵の標準を実装している。 これは、スラッシングデータベースのエクスポートされた内容です。
{% endhint %}

エクスポート .json ファイルの場所と名前を更新します。

{% tabs %}
{% tab title="Lighthouse" %}
```bash
lighthouse account validator slashing-protection export <lighthouse_interchange.json>
```
{% endtab %}

{% tab title="Nimbus" %}
実装するには
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
./lodestar account validator slashing-protection export --network mainnet --file interchange.json
```
{% endtab %}
{% endtabs %}

#### 🚧 8.4.3 新しいバリデーター/ビーコンチェーンをセットアップしてインストール

新しいバリデータ **をセットアップ/インストールする必要がありますが、systemd プロセスの実行を開始しないでください**。 新しいバリデーターの [セクション4に従ってください。 ETH2ビーコンチェーンとバリデーターを設定します。](guide-or-how-to-setup-a-validator-on-eth2-testnet.md#4-configure-a-eth2-beacon-chain-node-and-validator) クライアントのビルド/インストール、ポート転送/ファイアウォール、および新しいsystemdユニットファイルが必要です。

{% hint style="warning" %}
✨ ヒント：検証キーを再インポートするプロセス中は、ペナルティが大幅に削減されるのを防ぐために、少なくとも13分または2エポック待機してください。 同じバリデータキーを持つ2つのeth2クライアントを同時に実行することは避ける必要があります。
{% endhint %}

{% hint style="danger" %}
🛑 重要なステップ：スラッシュデータベースをインポートするか、少なくとも13分または2エポック待つまで、systemdプロセスを開始しないでください。
{% endhint %}

#### 📂 8.4.4 スラッシング データベースをインポート \(オプション\'

新しいeth2クライアントを使用して、次のコマンドを実行し、2ステップ前からスラッシングデータベースをインポートするための関連するパスを更新します。

{% tabs %}
{% tab title="Lighthouse" %}
```bash
lighthouse account validator slashing-protection import <my_interchange.json>
```
{% endtab %}

{% tab title="Nimbus" %}
実装するには
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
./lodestar account validator slashing-protection import --network mainnet --file interchange.json
```
{% endtab %}
{% endtabs %}

#### 🌠 8.4.5 新しいバリデータと新しいビーコンチェーンを開始

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

#### 🔥 8.4.6 機能を確認する

ログを確認して、サービスが正常に動作していることを確認し、エラーがないことを確認してください。

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

最後に、検証者の認証がパブリックブロックエクスプローラで動作していることを確認してください。

[https://beaconcha.in/](https://beaconcha.in/)

ステータスを表示するには、バリデータのパブキーを入力してください。

#### 8.4.7 Prometheus と Grafana によるアップデート監視

[セクション 6](guide-or-how-to-setup-a-validator-on-eth2-testnet.md#6-monitoring-your-validator-with-grafana-and-prometheus) を確認し、 `prometheus.yml` を変更します。 prometheusが新しいeth2クライアントのメトリックポートに接続されていることを確認してください。 新しいeth2クライアントのダッシュボードをインポートすることもできます。

### 🖥 8.5 利用可能なすべてのLVMディスク領域を使用する

Ubuntu Serverのインストール中に、ハードドライブの空き容量が不足している場合に一般的な問題が発生します。

```bash
# ディスクドライブを見る
sudo -s lvm

# 必要に応じて論理ボリュームファイルシステムパスを変更する
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv

#exit lvextend
exit

# 論理ボリューム内の新しい空き容量を使用するようにファイルシステムをリサイズする
resize2fs /dev/ubuntu-vg/ubuntu-lv

## 新しい空き容量を確認する
df -h
```

**ソースリファレンス**:

{% embed url="https://askubuntu.com/questions/1106795/ubuntu-server-18-04-lvm-out-space-with-imper-default-partitioning" caption="" %}

### 🚦 8.6 ネットワーク帯域幅の使用量を削減

{% hint style="info" %}
独自のETH1ノードをホストすると、1日に数百ギガバイトのデータを消費することができます。 データプランは制限されたりコストがかかる可能性があるため、データ使用量を減らしたり、ネットワークへの良好な接続を維持したりする必要があるかもしれません。
{% endhint %}

eth1.service unit ファイルを編集します。

```bash
sudo nano /etc/systemd/system/eth1.service
```

`ExecStart` 行のピア数を制限するには、次のフラグを追加します。

{% tabs %}
{% tab title="Geth" %}
```bash
--maxpeers 10
# Example
# ExecStart       = /usr/bin/geth --maxpeers 10 --http --ws
```
{% endtab %}

{% tab title="OpenEthereum \(Parity\)" %}
```bash
--max-peers 10
# Example
# ExecStart       = <home directory>/openethereum/openethereum --max-peers 10
```
{% endtab %}

{% tab title="Besu" %}
```bash
--max-peers 10
# 例
# ExecStart = <home directory>/besu/bin/besu --max-peers 10 --rpc-http-enabled
```
{% endtab %}

{% tab title="Nethermind" %}
```bash
--Network.ActivePeersMaxCount 10
# Example
# ExecStart       = <home directory>/nethermind/Nethermind.Runner --Network.ActivePeersMaxCount 10 --JsonRpc.Enabled true
```
{% endtab %}
{% endtabs %}

最後に、新しいユニットファイルをリロードし、eth1 ノードを再起動します。

```bash
sudo systemctl daemon-reload
sudo systemctl restart eth1
```

### 📂 8.7 重要なディレクトリ場所

{% hint style="info" %}
バリデータキー、データベースディレクトリまたはその他の重要なファイルを見つける必要がある場合。
{% endhint %}

#### Eth2 Client ファイルと場所

{% tabs %}
{% tab title="Lighthouse" %}
```bash
# Validator Keys
~/.lighthouse/mainnet/validators

# Beacon Chain Data
~/.lighthouse/mainnet/beacon

# List of all validators and passwords
~/.lighthouse/mainnet/validators/validator_definitions.yml

#Slash protection db
~/.lighthouse/mainnet/validators/slashing_protection.sqlite
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
# バリデーターキー
/var/lib/nimbus/validators

# ビーコンチェーンデータ
/var/lib/nimbus/db

#Slash protection db
/var/lib/nimbus/validators/slashing_protector.sqlite3

#Logs
/var/lib/nimbus/beacon.log
```
{% endtab %}

{% tab title="Teku" %}
```bash
# バリデーターキー
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

#### Eth1 node ファイルと場所

{% tabs %}
{% tab title="Geth" %}
```bash
# データベースの場所
$HOME/.ethereum
```
{% endtab %}

{% tab title="OpenEthereum \(Parity\)" %}
```bash
# データベースの場所
$HOME/.local/share/openethereum
```
{% endtab %}

{% tab title="Besu" %}
```bash
# データベースの場所
$HOME/.besu/database
```
{% endtab %}

{% tab title="Nethermind" %}
```bash
#データベースの場所
$HOME/.nethermind/nethermind_db/mainnet
```
{% endtab %}
{% endtabs %}

### 🌏 8.8 ETH1ノードを別のマシンでホスティングする

{% hint style="info" %}
ビーコンチェーンとバリデータがどこにあるのかと異なるマシン上で独自のETH1ノードをホストすることで、モジュール性と柔軟性を高めることができます。
{% endhint %}

eth1 ノード マシンで、eth1.service unit ファイルを編集します。

```bash
sudo nano /etc/systemd/system/eth1.service
```

`ExecStart` 行で、リモートからの http や websocket API 要求を許可するために次のフラグを追加します。

{% hint style="info" %}
websocketを使用していない場合、wsパラメータを含める必要はありません。 Nimbus のみウェブソケットが必要です。
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
# ExecStart = <home directory>/openetereum/openethereum --jsonrpc-interface=all --ws-interface=all
```
{% endtab %}

{% tab title="Besu" %}
```bash
--rpc-http-host=0.0.0.0 --rpc-ws-enabled --rpc-ws-host=0.0.0.0
# 例
# ExecStart = <home directory>/besu/bin/besu --rpc-http-host=0.0.0.0 --rpc-ws-enabled ---rpc-ws-host=0.0.0.0 --rpc-http-enabled
```
{% endtab %}

{% tab title="Nethermind" %}
```bash
--JsonRpc.Host 0.0.0.0 --WebSocketsEnabled
# 例
# ExecStart = <home directory>/nethermind/Nethermind.Runner --JsonRpc.Host 0.0.0.0.0 --WebSocketsEnabled -JsonRpc.Enabled true
```
{% endtab %}
{% endtabs %}

新しいユニットファイルを再読み込みし、eth1ノードを再起動します。

```bash
sudo systemctl daemon-reload
sudo systemctl restart eth1
```

ビーコンチェーンをホストする別のマシンで、beacon-chain unit ファイルを eth1 ノードの IP アドレスで更新します。

{% tabs %}
{% tab title="Lighthouse" %}
```bash
# beacon-chain unit file
nano /etc/systemd/system/beacon-chain.service
# add the --eth1-endpoint parameter
# 例
# --eth1-endpoint=http://192.168.10.22
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
# beacon chain unit file
nano /etc/systemd/system/beacon-chain.service
# modify the --web-url parameter
# 例
# --web3-url=ws://192.168.10.22
```
{% endtab %}

{% tab title="Teku" %}
```bash
# edit teku.yaml
nano /etc/teku/teku.yaml
# change the eth1-endpoint
# 例
# eth1-endpoint: "http://192.168.10.20:8545"
```
{% endtab %}

{% tab title="Prysm" %}
```bash
# editbeacon-chain unit file
nano /etc/systemd/system/beacon-chain.service
# add the --http-web3provider parameter
# 例
# --http-web3provider=http://192.168.10.20:8545
```
{% endtab %}

{% tab title="Lodestar" %}
```bash
# edit beacon-chain unit file
nano /etc/systemd/system/beacon-chain.service
# add the --eth1.providerUrl parameter
# 例
# --eth1.providerUrl http://192.168.10.20:8545
```
{% endtab %}
{% endtabs %}

更新されたユニットファイルを再読み込みし、ビーコンチェーンを再起動します。

```bash
sudo systemctl daemon-reload
sudo systemctl restart beacon-chain
```

### 🎊 8.9 POAP グラフィティフラグを追加または変更

`落書き`をセットアップし、バリデータが正常に提案するブロックに含まれるカスタムメッセージを設定し、早期ビーコンチェーンのバリデータPOAPトークンを獲得します。 [ここでイーサリアム1.0アドレスを指定してPOAP文字列を生成します。](https://beaconcha.in/poap)

`MY_GRAFFITI` 変数を設定するには、次のコマンドを実行します。 `<my POAP string or message>` を単一引用符で置き換えます。

```bash
MY_GRAFFITI== '<my POAP string or message>'
# 例
# MY_GRAFFITI='poapAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
[POAP - 出席証明トークンの詳細について。 ](https://www.poap.xyz/)
{% endhint %}

{% tabs %}
{% tab title="Lighthouse" %}
**validator.service** の設定を定義するために`ユニットファイル` を再作成するには、以下を実行します。 コピーして貼り付けるだけです。

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
ExecStart       = $(which lighthouse) vc --network mainnet --graffiti "${MY_GRAFFITI}" 
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

ファイル権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```
{% endtab %}

{% tab title="Nimbus" %}
**beacon-chain.service** の設定を定義するために、次のように`ユニットファイル` を再作成します。 コピーして貼り付けるだけです。

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
ExecStart       = /usr/bin/nimbus_beacon_node --network=mainnet --graffiti="${MY_GRAFFITI}" --data-dir=/var/lib/nimbus --web3-url=ws://127.0.0.1:8546 --metrics --metrics-port=8008 --rpc --rpc-port=9091 --validators-dir=/var/lib/nimbus/validators --secrets-dir=/var/lib/nimbus/secrets --log-file=/var/lib/nimbus/beacon.log
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="warning" %}
Nimbus は ETH1 ノードの websocket 接続 \("ws://" と "wss://"\' のみをサポートしています。 Geth, OpenEthereum, Infura and Chainstack ETH1 nodes are verified compatible.
{% endhint %}

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

ファイル権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```
{% endtab %}

{% tab title="Teku" %}
Teku設定ファイルを再生成します。 コピーして貼り付けるだけです。

```bash
cat > $HOME/teku.yaml << EOF
# network
network: "mainnet"

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
metrics-categories: ["BEACON","LIBP2P","NETWORK"]
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

設定ファイルを `/etc/teku`に移動する

```bash
sudo mv $HOME/teku.yaml /etc/teku/teku.yaml
```
{% endtab %}

{% tab title="Prysm" %}
**validator.service** の設定を定義するために、`ユニットファイル` を再作成します。 コピーして貼り付けるだけです。

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
ExecStart       = $(echo $HOME)/prysm/prysm.sh validator --mainnet --graffiti "${MY_GRAFFITI}" --accept-terms-of-use --wallet-password-file $(echo $HOME)/.eth2validators/validators-password.txt
Restart         = on-failure

[Install]
WantedBy	= multi-user.target
EOF
```

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```
{% endtab %}

{% tab title="Lodestar" %}
**validator.service** の設定を定義するために`ユニットファイル` を再作成するには、以下を実行します。 コピーして貼り付けるだけです。

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
ExecStart       = $(echo $HOME)/git/lodestar/lodestar validator --network mainnet --graffiti "${MY_GRAFFITI}"
Restart         = on-failure

[Install]
WantedBy	= multi-user.target
EOF
```

ユニットファイルを `/etc/systemd/system に移動します`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

権限を更新します。

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```
{% endtab %}
{% endtabs %}

更新されたユニットファイルをリロードし、グラフィティを有効にするためにバリデータープロセスを再起動します。

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

### 📦 8.10 ETH1ノードを更新 - Geth / OpenEthereum / Besu / Nethermind

{% hint style="info" %}
随時、最新のETH1リリースにアップデートして、新たな改善や機能をお楽しみください。
{% endhint %}

eth1ノードプロセスを停止します。

```bash
# これは時間がかかることがあります。
sudo systemctl stop eth1
```

eth1 ノードパッケージまたはバイナリを更新します。

{% tabs %}
{% tab title="Geth" %}
[https://github.com/etherum/go-ethereum/releases](https://github.com/ethereum/go-ethereum/releases)

```bash
sudo apt update
sudo apt upgrade -y
```
{% endtab %}

{% tab title="OpenEthereum \(Parity\)" %}
最新のリリースは [https://github.com/openetherum/openetherum/releases](https://github.com/openethereum/openethereum/releases)

最新のLinuxリリース、zip解除、実行権限の追加、クリーンアップを自動的にダウンロードします。

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
最新のリリースは [https://github.com/hyperledger/besu/releases](https://github.com/hyperledger/besu/releases)

ファイルは [https://dl.bintray.com/hyperledger-org/besu-repo からダウンロードできます](https://dl.bintray.com/hyperledger-org/besu-repo)

上記のリポジトリから目的のファイルを手動で見つけて、URL で `wget` コマンドを変更します。

> 例
>
> wget -O besu.tar.gz [https://dl.bintray.com/hyperledger-org/besu-repo/besu-20.10.1.tar.gz](https://dl.bintray.com/hyperledger-org/besu-repo/besu-20.10.1.tar.gz)

```bash
cd $HOME
# backup previous besu version in case of rollback
mv besu besu_backup_$(date +"%Y%d%m-%H%M%S")
# download latest besu
wget -O besu.tar.gz <https URL to latest tar.gz linux file>
# untar
tar -xvf besu.tar.gz
# cleanup
rm besu.tar.gz
# rename besu to standard folder location
mv besu* besu
```
{% endtab %}

{% tab title="Nethermind" %}
最新のリリースは [https://github.com/NethermindEth/nethermind/releases で確認してください](https://github.com/NethermindEth/nethermind/releases)

最新のLinuxリリース、ZIP解除、クリーンアップを自動的にダウンロードします。

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
{% endtabs %}

eth1ノードプロセスを開始します。

```bash
sudo systemctl start eth1
```

ログを確認して、サービスが正常に動作していることを確認し、エラーがないことを確認してください。

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

最後に、検証者の認証がパブリックブロックエクスプローラで動作していることを確認してください。

[https://beaconcha.in/](https://beaconcha.in/)

ステータスを表示するには、バリデータのパブキーを入力してください。

## 🌇 9. DiscordとRedditのコミュニティに参加する

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

## 🧩10. 参照資料

このガイドを作成するための基礎となった以下のリンクで、立派な人々によって行われたハードワークを感謝します。

{% embed url="https://launchpad.etherum.org/" caption="" %}

{% embed url="https://pegasys.tech/teku-ethereum-2-for-enterprise/" caption="" %}

{% embed url="https://docs.teku.pegasys.tech/en/latest/HowTo/Get-Started/Build-From-Source/" caption="" %}

{% embed url="https://lighthouse-book.sigmaprime.io/intro.html" caption="" %}

{% embed url="https://status-im.github.io/nimbus-eth2/intro.html" caption="" %}

{% embed url="https://prylabs.net/participate" caption="" %}

{% embed url="https://docs.prylabs.network/docs/getting-started/" caption="" %}

{% embed url="https://chainsafe.github.io/lodestar/installation/" caption="" %}

## 🎉11. ボーナスリンク

### 🧱 ETH2 Block Explorers

{% embed url="https://beaconcha.in/" caption="" %}

{% embed url="https://beaconscan.com/" caption="" %}

### 🗒最新のEth2情報

{% embed url="https://hackmd.io/@benjaminion/eth2\_news/" caption="" %}

{% embed url="https://www.reddit.com/r/ethstaker" caption="" %}

{% embed url="https://blog.etherum.org/" caption="" %}

### 👨👩👧👦 追加のETH2コミュニティガイド

{% embed url="https://someresat.medium.com/" caption="" %}

{% embed url="https://github.com/metanull-operator/eth2-ubuntu" caption="" %}

{% embed url="https://agstakingco.gitbook.io/eth-2-0-staking-guide-medalla/" caption="" %}

#### ハードウェアステーキングガイド [https://www.reddit.com/r/ethstaker/comments/j3mlup/a\_slightly\_updated\_look\_at\_hardware\_for\_staking/](https://www.reddit.com/r/ethstaker/comments/j3mlup/a_slightly_updated_look_at_hardware_for_staking/)

{% embed url="https://medium.com/@RaymondDurk/how-to-stake-for-ethere-2-0-with-dappnode-231fa7689c02" caption="" %}

{% embed url="https://kb.beaconcha.in/" caption="" %}

