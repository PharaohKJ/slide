## プライベート Docker
## コンテナの運用について

　

<div align="right">
2014 DataCenter とソフトウェア開発ワークショップ
</br>
石川ハイテク交流センター
</br>
2014-11-21 PhalanXware 加藤 真透
</div>

--

# 第一部
## Docker とはなにか

--

### Docker とはなにか

　

Docker社が開発するオープンソースのコンテナー管理ソフトウェアの1つである。

LXC(LinuxContainers)によりLinux上に隔離された環境を作り出し、ホストの環境に影響を及ぼさずにゲストOS、アプリケーションソフトの実行ができる。

OSやアプリケーションの導入済みLinux環境を丸ごとイメージとして保存、共有する機能がある。

--

### Docker Hubとは

　

イメージの保存、共有するサービスが **Docker Hub** である。

https://hub.docker.com/

Docker社自体が、上記URLで提供するサービス。

基本的にはこれらのイメージは **public** で、誰でも利用することができる。

カスタムイメージ構築時、基礎となる元のイメージはここからダウンロードされるよう作られている。

--

### Docker Hub の問題点

　

前述のとおり、基本的には **public** であるため、独自のアプリケーションや設定を導入したカスタムイメージを公開することになってしまう。

そのため、カスタムイメージを運用する場合には、 **private** なRepositoryを用意する必要がある。その方法を以下に示す。

  - Docker Hub の 有料プランを契約する。
  - Docker Registry を自LAN内で運用する。

--

### 方法1 有料プラン(1)

　

Docker社はサービスの利用に有料プランを設けており、そちらで **private** なRepositoryを提供している。

https://registry.hub.docker.com/plans/ を参照。以下抜粋

 - 1 Repository $0/Month
 - 10 Repository $12/Month
 - 50 Repository $50/Month

--

### 方法1 有料プラン(2)

　

- メリット
 - 初期Dockerイメージ構築時と運用時の構成をそのまま使うことができる。
 - イメージ保存のディスク容量を気にしなくて良い。

　

- デメリット
 - インターネットを経由した Docker Hub へのアクセス・認証を必要とする。
 - LAN外のシステム Docker Hub の死活にサービスが左右される。
 - 有料となる。

--

### 方法2 Docker Registry の運用(1)

　

Docker社が、自ネットワーク内で Docker Hub を運用できるように、 **Docker Registry** というDockerイメージを公開している。

　

これを用いることで、ローカルな環境上に Docker Hub 相当の、イメージを共有するサービスを作ることができる。

--

### 方法2 Docker Registry の運用(2)

　

- メリット
  - インターネットを経由しない。認証不要。
  - 無料で用いることができる。

　

- デメリット
  - ディスク容量を使う。

--

### Docker Registry セットアップ手順

　

今回はDocker Hub Regisryに依存せず、自LAN内にDockerイメージを持つことを想定し、Docker Registryをセットアップする。

Docker Registry 自体も Docker イメージとして提供されている。以下のコマンドでpullし同時に動作させることができる。

```
docker run -d -p 5000:5000 registry
```

  - -d : デーモンモード
  - -p : ポートの転送設定
  - registry : docker image の名前

--

### Docker Registry push手順

　

前述した方法でRepositoryに、 カスタムしたイメージを push するコマンドを示す。

以下で、ホスト名とポートを指定し、そのRepositoryへイメージを push、後にpullして各Docker Hostでそれぞれ pull して使うことができる。

```
docker push www1.local:5000/webapp_service
```
　

  - **www1.local**:*5000*/**webapp_service**
    - **www1.local** : 前述の docker run registry したホスト名
    - *5000* : 前述の docker run registry したポート番号
    - **webapp_service** : 保存するイメージ名


--

### Docker Registry 運用図

　

　(DockerRegistryを動かす1台と、それを参照するDockerVM複数台、その上に動くコンテナを図示)


--

# 第二部
## docker運用のポイント

--

### docker pull ボトルネック

![図](./image.png "docker pull ボトルネック")


全Docker Hostが1つのRegistryよりDockerイメージをpullするため、以下の点が問題になる。

- networkリソースを使う。
- docker run までのレイテンシが増える。

--

### 改善案1 pre-pull モデル

 ![図](./image2.png "docker pull ボトルネック")

Docker Registryを各Docker Hostにもたせる。
あらかじめ、Dockerイメージ、アップデートをpullしておく。

- メリット
  - 各Docker Hostがイメージを持つため、pull(run)時、1台のDocker Repositoryにアクセスが集中しない。
  - 1台のDocker Repositoryに依存しないため、故障耐性が高い。

- デメリット
  - 各自のDockerRegistryが必要なのでディスク容量が必要となる。

--

### 改善案2 shared disk モデル

 ![図](./image3.png "docker pull ボトルネック")

- Docker Registryを作成し、そこにDockerイメージ、アップデートをpullしておく。
- そのイメージが収められているファイルシステムを各Docker Hostにマウントさせる。

- メリット
  - ディスクのマウントなので、実質使用しているのは1台のディスク容量のみとなる。
  - 1度イメージを更新すれば、全てのDocker Host上のイメージを更新することができる。

- デメリット
  - ディスクに障害があった場合、全てのDocker Hostに影響がある。

--

## まとめ

　

- Docker と Docker Registry について解説した。
- private な Docker Registry の必要性について解説し、作成方法を提示した。
　

- Docker イメージの配布方法の改善について考察した。
  - ボトルネックが存在することを示し、解決方法の以下2種を示した
    - 改善案1. pre-pull モデル
    - 改善案2. shared-disk モデル
