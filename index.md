Kubernetes を利用した RTC 開発フレームワーク
===
　クラウドをサービスを構築するために使われるクラスタ構築ツールである Kubernetes を活用し、
ロボットシステム開発・運用を効率化するワークフローとそれを補助するツールについて紹介します。
RTC やロボットシステムの開発だけでなく、運用面での効率化にも貢献します。

目次
===
* [フレームワークが実現すること](#%E3%83%95%E3%83%AC%E3%83%BC%E3%83%A0%E3%83%AF%E3%83%BC%E3%82%AF%E3%81%8C%E5%AE%9F%E7%8F%BE%E3%81%99%E3%82%8B%E3%81%93%E3%81%A8)
* [クラウド関連技術とロボットシステム開発への応用](#%E3%82%AF%E3%83%A9%E3%82%A6%E3%83%89%E9%96%A2%E9%80%A3%E6%8A%80%E8%A1%93%E3%81%A8%E3%83%AD%E3%83%9C%E3%83%83%E3%83%88%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0%E9%96%8B%E7%99%BA%E3%81%B8%E3%81%AE%E5%BF%9C%E7%94%A8)
  * [クラスター](#%E3%82%AF%E3%83%A9%E3%82%B9%E3%82%BF%E3%83%BC)
  * [Kubernetes](#kubernetes)
  * [OS コンテナ](#os-%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A)
  * [Pod](#pod)
* [作成したツール](#%E4%BD%9C%E6%88%90%E3%81%97%E3%81%9F%E3%83%84%E3%83%BC%E3%83%AB)
* [ワークフロー](#%E3%83%AF%E3%83%BC%E3%82%AF%E3%83%95%E3%83%AD%E3%83%BC)
* [サンプルを用いた実践](#%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%82%92%E7%94%A8%E3%81%84%E3%81%9F%E5%AE%9F%E8%B7%B5)
  * [インストール手順](#%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%89%8B%E9%A0%86)
    * [Docker のインストール](#docker-%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)
    * [Kubernetes のインストール](#kubernetes-%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)
  * [クラスター構築手順](#%E3%82%AF%E3%83%A9%E3%82%B9%E3%82%BF%E3%83%BC%E6%A7%8B%E7%AF%89%E6%89%8B%E9%A0%86)
    * [マスターノード](#%E3%83%9E%E3%82%B9%E3%82%BF%E3%83%BC%E3%83%8E%E3%83%BC%E3%83%89)
    * [ワーカーノード](#%E3%83%AF%E3%83%BC%E3%82%AB%E3%83%BC%E3%83%8E%E3%83%BC%E3%83%89)
    * [管理 PC](#%E7%AE%A1%E7%90%86-pc)
    * [ワーカーにラベルを設定](#%E3%83%AF%E3%83%BC%E3%82%AB%E3%83%BC%E3%81%AB%E3%83%A9%E3%83%99%E3%83%AB%E3%82%92%E8%A8%AD%E5%AE%9A)
  * [RTC の作成、削除、バージョン変更手順](#rtc-%E3%81%AE%E4%BD%9C%E6%88%90%E5%89%8A%E9%99%A4%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E5%A4%89%E6%9B%B4%E6%89%8B%E9%A0%86)
* [付録](#%E4%BB%98%E9%8C%B2)
  * [Docker ビルドの高速化](#docker-%E3%83%93%E3%83%AB%E3%83%89%E3%81%AE%E9%AB%98%E9%80%9F%E5%8C%96)
  * [マルチアーキテクチャ対応の Docker ビルド](#%E3%83%9E%E3%83%AB%E3%83%81%E3%82%A2%E3%83%BC%E3%82%AD%E3%83%86%E3%82%AF%E3%83%81%E3%83%A3%E5%AF%BE%E5%BF%9C%E3%81%AE-docker-%E3%83%93%E3%83%AB%E3%83%89)
    * [初期設定](#%E5%88%9D%E6%9C%9F%E8%A8%AD%E5%AE%9A)
    * [ビルド手順](#%E3%83%93%E3%83%AB%E3%83%89%E6%89%8B%E9%A0%86)
  * [Docker リポジトリの構築](#docker-%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%81%AE%E6%A7%8B%E7%AF%89)
  * [Docker イメージのサイズ](#docker-%E3%82%A4%E3%83%A1%E3%83%BC%E3%82%B8%E3%81%AE%E3%82%B5%E3%82%A4%E3%82%BA)
  * [コンテナネットワーク](#%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF)
* [改訂履歴](#%E6%94%B9%E8%A8%82%E5%B1%A5%E6%AD%B4)
* [注記](#%E6%B3%A8%E8%A8%98)

# フレームワークが実現すること
　はじめに、このページで紹介するツール・仕組みを導入することで何が実現されるのかを列挙しながら説明します。

- RTC のビルド環境・手順の構築

  　RTC のビルドは cmake やコンパイラ等の開発ツールや使用するライブラリのインストールが必要です。
  こうしたビルド手順をテキスト記述することで、ツールによりビルドが自動化され、再現可能になります。  
  　例えば、複数人で開発する場合に開発環境を複数準備する場合に、人的ミス無く効率的に構築できます。

- 独立した RTC 実行環境の構築

  　一つの PC で複数のアプリケーションを動作させる場合、ライブラリのバージョンやポート番号の衝突が起こることがあります。
  RTC を独立した実行環境で実行することで、こうした問題を解決し、RTC を動作させる PC を自由に変更できます。

- RTC の CPU アーキテクチャ対応

  　例えば、開発した RTC を PC (x86) で動かしたい場合と Rasberry Pi (armhf, arm32v7) で動かしたい場合があります。
  これを解決する基本的な手段はそれぞれの環境用でビルドすることです。
  前述のスクリプトを適切に記述することで、自動的に複数のアーキテクチャの RTC をビルドすることができます。  

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69402728-0b7a8880-0d3c-11ea-88c1-324890afbf5a.png" width="500px">
</div>


- ビルドされた RTC のバージョン管理

  　前述の通りに RTC やライブラリなどが一つのイメージとして管理されます。
  このイメージには名前をつけることができ、リポジトリに登録することでバージョンともに履歴が管理できます。
  開発から時間が経過して、ビルド環境や必要ライブラリが開発終了となっていても、
  リポジトリからイメージを取り出すだけで実行可能です。
  ```
  myRTC:1.0
  myRTC:1.1
  ...
  myRTC:20190101
  myRTC:20190102
  ...
  ```

- リモート PC への RTC 配置と実行

  　一般的には RTC は開発用 PC でビルドされ、ロボットシステムに搭載された別の PC に配置して実行します。
  こうしたスタイルの場合、 PC や RTC の数に応じて配置のコスト（手間や人的ミス）が増加します。
  配置する RTC イメージ名、バージョン名と実行する PC 名をテキスト記述することで、
  ツールを通して配置可能となります。  
  　また、開発用 PC はロボットシステムと同じネットワークに所属する必要は無いため、遠隔地でのロボット開発も可能です。

-  プロトタピングやデバッグの効率化

  　前述のリモート PC への配置と実行を利用することで、プログラム修正と実行の反復作業は効率的になります。
  プロトタイピングにおいては、アルゴリズム修正やパラメータ変更などが容易になります。
  デバッグ時にいおいても同様に、修正した RTC の動作を手早く確認できます。
  また、デバッグに使われる rtshell や OpenRTP などのツールはこの仕組みの中でも使用可能です。

-  RTC のインテグレーション作業の効率化

  　ロボットシステムは複数の RTC をインテグレーションすることで構築されます。
  テキスト記述を行うことで、ロボットシステム内の各 PC に対して、どの RTC をバージョンやオプションを指定
  しながら実行できます。テキスト記述であるため、このテキスト自体のバージョン管理は git 等で容易であり、
  各バージョンごとに RTC の動かし方を残すことができます。また、設計・実装者が意図した通りのシステムを、
  テスト担当者やサービス運用担当者がミス無く再現でき、コミュニケーションに関するロスを軽減します。

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69770408-46613e00-11cc-11ea-993c-3442e1958a1a.png" width="500px">
</div>

# クラウド関連技術とロボットシステム開発への応用
　ロボットシステムにクラウド関連技術を適応することで、前節に記述した内容を実現します。
本節では、ロボットシステムに適応する前に、その理解を助ける目的でクラウド構築について解説します。

## クラスター
　クラウドのサービスを実現しているのは、計算機を束ねて構築される **クラスター** です。ユーザーがクラウドサービスにアクセスすることは、
それを実現しているクラスターのいずれかの計算機にアクセスすることになります。

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69405212-6b742d80-0d42-11ea-8c56-ce77f5fb31d9.png" width="500px">
</div>

サーバー上へのサービス構築比べて、クラスターにする利点として以下があげられます。
  - ユーザー数の増加に応じて、計算機を増やして性能（レスポンス等）を維持できる
  - ユーザー数の減少に応じて、計算機を減らすことでコストを節約できる
  - 計算機の故障が発生しても、故障していない計算機にユーザーを振り分ければ、サービスは止まらない
  - サービスを止めること無く、ソフトウェアをバージョンアップができる

 　本稿では、ロボットシステム開発のワークフローに上記の最後の項目を活用します。

## Kubernetes
　次にクラスター構築に近年使われることが増えてきた **Kubernetes** (https://kubernetes.io/ja/) について解説します。
Kubernetes ではクラスターに所属する計算機のことを **ノード** と呼びます。
ノードには、**マスターノード** と **ワーカーノード** の二種類が存在します。
マスターノードは、クラスターを管理するノードで、クラスター管理者の指示を受け付けるインターフェースとしての役割も担います。
ワーカーノードは、マスターノードの指示に従い、サービス実現のためにソフトウェアを実行するノードです。
管理者は、計算機をマスターノードに登録することでワーカーノードを増やすことができます。  
　ノードにはラベルを(複数)つけることができ、どのワーカーノードでどのソフトウェアを実行するかを識別します。これを利用することで、
ロボットシステムに搭載される複数の PC に対して、動作させるアプリケーション（制御ソフトウェア）を指定することができます。

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69703160-267e3b80-1134-11ea-96da-7485a526ebe9.png" width="500px">
</div>

 Kubenetes の概要は Wikipedia (https://ja.wikipedia.org/wiki/Kubernetes) も参考になります。

## OS コンテナ
　Kubernetes はどのノードでソフトウェアを実行しても同じ動作を保証するために、**OS コンテナ** (あるいは単にコンテナ) を内部で使用します。
OS コンテナとは、アプリケーションとそれが必要とするファイル (ライブラリ、プログラム、設定ファイル、...) をセットにしたものです。
コンテナはノード上で実行されますが、ノードが持つファイルとは独立しています。
これによりアプリケーション間で競合するライブラリが存在する場合でも、コンテナごとにライブラリを格納することで競合を起こさずに実行できます。  
　本稿では OS コンテナとして採用されることの多い **Docker** を用います。
Docker は **Dockerfile** というテキストファイルにビルド手順を記述すると、図のような **Docker イメージ** を生成します。
また、イメージを実行したプロセス（とファイル）を **Docker コンテナ** と呼びます。

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69770216-9c81b180-11cb-11ea-9e9e-1d4b34b8f0f4.png" width="500px">
</div>

## Pod
　Kubenetes はワーカーノードの中に **Pod** という仮想マシン (のようなもの) を作り、その中で OS コンテナを実行します。
マニフェストファイルというテキストファイルに、Pod の名前、動かすコンテナ名、動かすノードを決めるラベル名を記述すると、
Kubernetes は適切なワーカーノードを選択し、その中で Pod を作り、コンテナを実行します。  
　本稿では、わかりやすさのために Pod に対して一つの OS コンテナを割り当て、OS コンテナには一つの RTC を実行する形とします。

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69770222-9f7ca200-11cb-11ea-8da7-21128272f20c.png" width="350px">
</div>

# 作成したツール

　今回のワークフロー構築に当たり、作成したツールを以下に示します。これらの次章以降で使用します。

- Docker/Kubenetes テンプレート対応 OpenRTP

  https://github.com/r-kurose/OpenRTP-aist/tree/docker_template

- ネームサーバー
  + Docker イメージ (linux/amd64, arm)

    https://hub.docker.com/repository/docker/kuroseaist/rtm-nameserver

  + Dockerfile と Kubenetes マニフェストファイル

    https://github.com/r-kurose/rtm-namaserver-image

- rtshell

  + Docker イメージ (linux/amd64, 386, arm, arm64).

    https://hub.docker.com/repository/docker/kuroseaist/rtshell

  + Dockerfile と Kubenetes マニフェストファイル

    https://github.com/r-kurose/rtshell-image

- ビルド用 Docker イメージ (ビルドキャッシュ用）

  ビルド済み omniORB, OpenRTM-aist のイメージです。
  RTC のビルドを行う際に、これらのイメージが使われます。
  OpenRTM-aist は開発版 (2.0 予定版) です。

  + omniORB 4.2.3 ビルドイメージ on Alpine 3.10 (linux/amd64, arm).

    https://hub.docker.com/repository/docker/kuroseaist/omniorb-alpine

    https://github.com/r-kurose/omniorb-alpine-image

  + OpenRTM-aist on Alpine ビルドイメージ (linux/amd64, arm).

    https://hub.docker.com/repository/docker/kuroseaist/openrtm-alpine

    https://github.com/r-kurose/openrtm-alpine-image

  + OpenRTM-aist on Ubuntu ビルドイメージ (linux/amd64, 386, arm, arm64).

    https://hub.docker.com/repository/docker/kuroseaist/openrtm-ubuntu

    https://github.com/r-kurose/openrtm-ubuntu-image

# ワークフロー
　ワークフローの全体像をツールやそこで扱われるデータを元に2枚の図で示します。  図上の青文字は新規に作成したものです。

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69770263-c509ab80-11cb-11ea-9c48-fb7ba1b4b0df.png" width="500px">
</div>

　開発者は、RTC Builder を用いて Docker イメージを作成するための Dockerfile と Kubernetes のマニフェストファイルをテンプレートとして出力します。
これまでと同様に RTC のソースコードを記述します。
この時、ビルド手順である Dockerfile と実行手順である Kubenetes のマニフェストファイルも自身のソフトウェアに合わせて修正します。
後は Docker を使って RTC をビルドして、イメージとソースコードをリポジトリへ登録します。

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69770267-c89d3280-11cb-11ea-8d87-6a467ad2fea4.png" width="500px">
</div>

　次にリポジトリに登録されたソフトウェアをロボット上の PC に配信して、実行します。事前にロボットに搭載する PC をワーカーノードとして登録
しておきます。クラスタの管理 PC でコマンドを実行すると、ロボット上の PC にソフトウェアが配信され、実行されます。RTC のアクティベーション
やコネクターの接続は、RTC の起動オプションなどを用いて自動で行うか、rtshell などをリモート実行することで手作業もできます。  
　プログラム開発時は、「開発 PC」=「管理 PC」とすることでビルドしたソフトウェアがすぐにロボット上で動作させることができ、
アルゴリズムの評価やデバッグを簡単に行えます。
またテスト時は、テスト担当者にソースコードリポジトリを通して、 Kubenetes マニフェストファイルを渡します。
これにより、手違い無く正しいバージョンのソフトウェアをロボット上の (複数の) PC 上で、正しい起動オプションで実行することができ、
コミュニケーションエラーやヒューマンエラーが入り込むことを防ぎます。

# サンプルを用いた実践
　本章では、OpenRTM-aist の初学者におなじみの ConsoleIn と ConsoleOut のサンプルを用いて、
ワークフローを実行する手順を示します。これを通して一通りの作業を体験できます。ここで構築するシステムを以下の図に示します。

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69773320-deaff080-11d5-11ea-8fd9-af9dbe4970ee.png" width="500px">
</div>

上記を実践する場合、5台の PC (Raspberry Pi を含んでも良い) が必要です。
難しい場合は、下の図のようにいくつかの役割を1台の PC で兼務するか、仮想マシンを使って下さい。  
　なお、マスターノードにワーカーノードを兼務させることもできますが、本稿では説明しません。

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69773322-df488700-11d5-11ea-9348-2743b0ac9181.png" width="400px">
</div>

## インストール手順
　下記の表はそれぞれがインストールしなければならないソフトウェアを示します。
なお、マスターノードとワーカーノードを各一つが基本の最小構成です。
管理 PC とはクラスタを管理するもののことで、RTC の実行指示をします。
開発 PC とは、RTC のソースコード作成とビルドをする PC です。

| | Kubernetes | Docker
:----:|:----:|:----:|
| マスターノード | ○ | ○ |
| ワーカーノード | ○ | ○ |
| 管理 PC       | ○ | - |
| 開発 PC       | - | ○ |

### Docker のインストール

　ディストリビューションのパッケージを利用する方法と、Docker 公式パッケージ (docker-ce) を利用する方法があります。
最新版を使用したい場合は、後者を利用してください。

- a. ディストリビューションのパッケージを利用する場合

   Ubuntu: `apt install docker.io`

- b. Docker 公式による docker-ce を利用する場合

  https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-engine---community

  　上記は Ubuntu のものです。左側メニューに他のディストリビューションもあります。
  Raspberry Pi で Raspbian を使用している場合は Debian のページを見てください。

### Kubernetes のインストール

  https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl

## クラスター構築手順
　ここではクラスターの構築を行います。まずマスターノードを用意し、その後にワーカーノードをマスターノードに登録してください。
具体的なやり方を下記に示します。

### マスターノード
　下記の初期化手順をおまじないとして実行してください。
```
kubeadm init --pod-network-cidr=10.244.0.0/16
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
### ワーカーノード
```
kubeadm join <マスターノードの kubeadm init 実行で表示されるパラメーター>
```
確認方法: マスターノードで `kubectl get nodes` を実行するとノードが見える。

### 管理 PC
以下の実行で、マスターノードに対して指示を出すことができるようになります。
```
mkdir -p $HOME/.kube
sudo cp -i <マスターノードの admin.conf> $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
なお、マスターノードの conf ファイルは `/etc/kubernetes/admin.conf` にあります。

確認方法: 管理 PC で `kubectl get nodes` を実行するとノードが見える。

### ワーカーにラベルを設定
管理 PC からワーカーにラベルを設定します。
このラベルは、ソフトウェア（Docker イメージ）を動かすワーカーノードを指定するのに使用されます。
```
kubectl label node worker-2 consolein=true
kubectl label node worker-1 consoleout=true
kubectl label node worker-1 rtshell=rue
kubectl label node worker-1 nameserver=true
```
確認方法: `kubectl get nodes --show-labels` で設定した値が見える。

ラベルの削除法は、 ` kubectl label nodes worker-1 consoleout-` です。

## RTC の実行
　ここではツールを用いて RTC の起動やバージョン変更の仕方を説明します。
この章の作業を通して Docker / Kubenetes の使用方法、RTC の起動スクリプト (=Kubernetes マニフェストファイル) や
実行イメージ (=ビルド済 RTC, Docker イメージ)のリポジトリへの登録方法を知ることができます。

1. ネームサーバーと rtshell を起動します。
   ネームサーバー用のイメージとマニフェストファイルを用意しました。以下を実行して下さい。
   ```
   kubectl apply -f https://raw.githubusercontent.com/r-kurose/rtm-namaserver-image/master/nameserver.yaml
   kubectl apply -f https://raw.githubusercontent.com/r-kurose/rtshell-image/master/rtshell.yaml
   ```

   確認方法: `kubectl get pods` で二つの Pod が Running になれば成功です。
   
1. ワーカーに RTC を起動します。
   ConsoleIn/ConsoleOut のイメージとマニフェストファイルを用意しました。以下を実行して下さい。
   ```
   kubectl apply -f https://raw.githubusercontent.com/r-kurose/kube-simpleio-sample/master/ConsoleOut/ConsoleIn.yaml
   kubectl apply -f https://raw.githubusercontent.com/r-kurose/kube-simpleio-sample/master/ConsoleOut/ConsoleOut.yaml
   ```

   確認方法: `kubectl get pods` で確認の上、`kubectl exec <rtshell node ip> rtls -R localhost` で RTC が見えれば成功です。
 
## RTC のアップデート
1. ConsoleIn/ConsoleOut のソースコードを取得する。
   以下から、ソースコードをダウンロードして下さい。
   https://github.com/r-kurose/kube-simpleio-sample

   なお、自分でコンポーネントを作りたい場合は、作成ツールの一覧にある RTC Builder でテンプレートを作成して下さい。

1. RTC のソースコードを変更してビルドします。
   お試しに入力した値を +1 する変更を加えてみます。ConsoleIn.cpp#L105 に `m_out.data += 1` を追加して下さい。

   次に RTC をビルドします。
   テンプレートの Dockerfile は、Ubuntu 用と Alpine 用があります。
   どちらを採用するかは、イメージサイズに関してまとめた付録の章も参考にして下さい。
   amd64 向けと arm (Raspberry Pi) 向けを作りたい場合の方法も付録に記載しています。
   ```
   docker build -f Dockerfile.alpine -t ConsoleIn:1.0 .
   ```
   確認方法: `docker images` でイメージの存在が見えたら成功です。

1. イメージを Docker リポジトリにアップロードします。
   ```
   docker tag ConsoleIn:1.0 <リポジトリ名>/ConsoleIn:1.0
   docker push <リポジトリ名>/ConsoleIn:1.0
   ```
   　push先に DockerHub を使う場合は事前にアカウントを取得し、ログインして下さい。
   ローカル環境にリポジトリを構築する場合は、付録を参照して下さい。その場合、ログインは不要です。
   ```
   docker login <リポジトリ名>
   ```
1. Kubernetes のマニフェストファイル (.yaml) を開きイメージ名を変更します。

   開発時は、ここで作成したマニフェストファイルを git などのソースコードリポジトリで管理することをお勧めします。

1. RTC を停止します。
   ```
   kubectl delete -f ConsoleIn.yaml
   ```
   　停止までに時間がかかることもありますが、 rtshell などで RTC が停止したことが確認できます。
   `kubectl get pods` で pod が終了したこともわかると思います。

1. バージョンアップした RTC を起動します。
   ```
   kubectl apply -f ConsoleIn.yaml
   ```
   RTC の動作を確認すると、値が一つ増えていると思います。

1. RTC をバージョンダウンします。

   RTC を元のものに戻してみます。 delete せずにそのまま `kubectl apply` してみて下さい。
   しばらくの間、二つのバージョンが同時に動作するかもしれません。
   先ほどのように delete -f で起動中の RTC を止めることが一つの解決法です。

# 付録
## Docker ビルドの高速化
　Docker のビルドを高速化するために、 BUILDKIT を有効にすると便利です。
Docker 1809 以降をインストールの上、 `/etc/docker/daemon.json` に `{"features":{"buildkit": true}}` を加えてください。
追記後に Docker サービスの再起動も必要です。

## マルチアーキテクチャ対応の Docker ビルド
　PC 用と Raspberry Pi 用 など、複数の CPU アーキテクチャに対応した RTC を準備したいときなど、
Docker buildx (https://docs.docker.com/buildx/working-with-buildx/) プラグインを利用すると便利です。

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69770452-7e688100-11cc-11ea-932a-55aa6439d5bf.png" width="500px">
</div>

### 初期設定
   1. Docker 1903 以降をインストールします。
   1. Docker buildx プラグイン (https://github.com/docker/buildx/releases) をダウンロードして、
   1. `~/.docker/cli-plugins/` に `docker-buildx` にリネームして置きます。
   1. ビルド用のドライバーを ` docker buildx create --use` で作成します。

### ビルド手順
   1. qemu を有効にします。 (この作業は OS 起動後に1度必要です)
      ```
      docker run --rm --privileged multiarch/qemu-user-static:register
      ```

   1. ビルド用ドライバーに対応プラットフォームを追加します。(この作業は OS 起動後に1度必要です)
      ```
      docker buildx inspect --bootstrap
      ```

   1. 念のためにビルド可能な CPU アーキテクチャを確認します。
      ```
      docker buildx ls
      ```

   1. ビルドします。

      　ビルドして、結果をリポジトリに登録する方法は以下になります。事前にリポジトリにログイン `docker login` して下さい。
      ```
      docker buildx build -f <Dockerfile> -t <login name>/<image name>:<version> --platform=linux/amd64,linux/arm/v7 --push .
      ```
      　なお、｀docker build` 同様に結果を PC に登録する場合は `--push` の代わりに `--load` を用います。
      ただし、アーキテクチャは一つのみの指定になります。
      ```
      docker buildx build -f <Dockerfile> -t <image_name>:<version> --platform=linux/arm/v7 --load .
      ```

## Docker リポジトリの構築
　Docker イメージのオープンなリポジトリとして Docker Hub (https://hub.docker.com) があります。
しかし、配布できないプログラムを含むプログラムがある場合や、通信環境をインターネットと切り離したい場合など、
プライベートなリポジトリを構築する際には、Docker registry (https://hub.docker.com/_/registry) を用います。  
　なお、証明書の作成・登録を行わないと https によるイメージ登録 (docker push) はできません。
証明書を使わずに (= 暗号化なしに) イメージ登録をする場合は、それを許可するオプションが必要です。

## Docker イメージのサイズ

Docker イメージのサイズが大きいと実行時の取得に時間がかかります。
特に配置する回数の多い RTC の Dockerイメージは小さくあるべきです。
以下に、参考値として Raspberry Pi 用 Docker イメージの容量と取得時間 (docker pull) を計測した結果を示します。
使用したコンポーネントは、 SineWaveComp です。

| イメージ | 容量 | 取得時間 | パッケージ管理 | 備考 |
:----:|:----:|:----:|:----:|:----:|
| Ubuntu + OpenRTM         | 500 MB  | 4m 52s | apt | コンパイラ等含む環境 |
| Ubuntu + OpenRTM runtime | 52 MB   | 20s    | apt | |
| Alpine + OpenRTM runtime | 11.7 MB | 5s     | apk | |
| Busybox (静的リンク)      | 8.2 MB  | 未計測 | 無  | |
| OS無し (静的リンク）       | 7.0 MB  | 未計測 | 無 | ファイル使用不可 (rtc.conf, ログファイル) |

## コンテナネットワーク

　Kubenetes は、Pod のネットワークを物理ネットワークから抽象化するために **クラスターネットワーク** (CNI)を用います。
例えば下記の図のように、Pod には青文字の IP が仮想的に割り当てられます。
　Kubernetes の注意点として Pod が起動するまでは基本的に IP がわからないことがあります。図の例では、
RTC はネームサーバーに自身を登録したいのですが、ネームサーバーの IP (10.96.0.4) を知ることはできません。
このような場合に、**ClusterIP** または **NodePort** あるいは総称して **サービス** という機能を用います。
図上ではサービスに 10.96.0.100 が割り当てられていますが、Pod ではありません。
ただし、クラスター内部からこの特別な IP (10.96.0.100) に対してアクセスすると、ネームサーバー (10.96.0.4) に転送されます。
サービスの IP はマニフェストファイルに記述するため、この IP は事前に知ることができます。  
　もう一つの注意点として、クラスター内部の IP とクラスター外部の IP の二種類があるため、クラスター外からクラスター内の RTC への
アクセスは設定が必要となります。最も簡単な解決方法は、Pod を動かすかにかかわらず、すべてのマシンをクラスターに登録することです。

<div align="center">
<img src="https://user-images.githubusercontent.com/45954537/69773299-cdff7a80-11d5-11ea-9bdc-b3b0932b40c4.png" width="500px">
</div>

# 改訂履歴
  - 2019-11-28: 初版

# 注記
　本記事は第20回計測自動制御学会システムインテグレーション部門講演会 (SI2019) のミドルウェアコンテスト作品です。
* コンテスト作品掲載ページ: https://tmp.openrtm.org/openrtm/ja/project/k8s-rtc
* SI2019 公式ページ: https://sice-si.org/conf/si2019/

---
国立研究開発法人 産業技術総合研究所  
ロボットイノベーション研究センター  
ロボットソフトウェアプラットフォーム研究チーム  
黒瀬 竜一 \<kurose.ryoichi@aist.go.jp\>

パナソニック アドバンストテクノロジー株式会社  
イノベーション基盤開発室 第二課  
矢後聡識 \<yago.akinori@.jp.panasonic.com\>
