![logo](./logo.png "とあるインフラエンジニアの開発環境")
 twitter
 github
 Web系のインフラエンジニア
 最近、興味あるもの
 その他
 普段使っているツールを紹介
 \*\*env系の簡易マネージャ
 設定がひとつに
 \*\*envのインストール
 \*\*envで任意のものをインストール
 Go はインストールされていない
 Python は System 標準のものを利用している
 Ruby は rbenv で 2.1.2 がインストールされている
 コマンドひとつで簡単にアップデートができます
 コマンドひとつで各リポジトリ内にある不要なオブジェクトを削除したりできます
 \*\*env rehash のようなサブコマンドをまとめて実行できます
 riywo/anyenv
 znz/anyenv-update
 anyenv-git
 anyenv-exec
 anyenvという\*\*env系の簡易マネージャを作った
 anyenvで開発環境を整える
 サーバ構成管理ツール
 ローカルホストからリモートホストに Push して実行するので、リモートホストにエージェントは不要です
 Python 製のツールだけど、実際に利用するためには yaml を書けてコマンドラインでサーバの作業ができれば大丈夫
 外部 DSL だけど自作の module を利用する機能があるので、自由度は高い
 できないこと
 やらないほうが良いこと
 module が処理していること
 標準で用意された module はこんな感じで使えます
 シェルスクリプトで書いていた処理に、実行に必要な引数を渡して処理結果が画面出力されてるものと考えればわかりやすいかも
 ansible → sh
 -m raw -a "uname -a" -i hosts → ./remote_host_uname.sh
 module などを利用する手順をまとめて yaml で書いたもの
 ansible-playbook ファイル名 で実行する
 設定ファイルを配置や置換したり、サービスの再起動などサーバを構築するうえで必要な手順を記載する
 yum module を利用して指定パッケージをインストールする場合、こんな感じで書けます
 一部抜粋した部分をサーバ構築の手順書として書いたものと比較するとわかりやすいかも
 Ansible Tutorial
 不思議の国のAnsible
 入門Ansible
 Qiita (tag:Ansible)
 書きたかったけど間に合わんかったｗ
 簡単に説明すると、 ssh 経由でサーバのコマンドを実行して、ファイルの存在や設定値、サービスの起動状態などを確認するためのツール
 anyenv
 Ansible
 Serverspec