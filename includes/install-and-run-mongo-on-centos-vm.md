次のステップに従って、MongoDB を CentOS Linux が実行されている仮想マシンにインストールして実行します。

> [AZURE.WARNING] 認証、IP アドレス バインドなどの MongoDB セキュリティ機能は、既定では無効です。 MongoDB を運用環境に展開する前に、セキュリティ機能を有効にすることをお勧めします。  参照してください [セキュリティと認証](http://www.mongodb.org/display/DOCS/Security+and+Authentication) の詳細。

1. MongoDB をインストールできるようにパッケージ管理システム (YUM) を構成します。 作成、 */etc/yum.repos.d/10gen.repo* ファイルをリポジトリの情報を保持し、次を追加します。

        [10gen]
        name=10gen Repository
        baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64
        gpgcheck=0
        enabled=1

2. repo ファイルを保存し、次のコマンドを実行して、ローカル パッケージ データベースを更新します。

        $ sudo yum update

3. パッケージをインストールするには、次のコマンドを実行し、MongoDB の最新の安定バージョンと関連ツールをインストールします。

        $ sudo yum install mongo-10gen mongo-10gen-server

    MongoDB がダウンロードおよびインストールされるまで待ちます。

4. データ ディレクトリを作成します。 既定では MongoDB にデータを保存、 */データ/db* ディレクトリがこのディレクトリは作成する必要があります。 作成するには、次のように実行します。

        $ sudo mkdir -p /srv/datadrive/data
        $ sudo chown `id -u` /srv/datadrive/data

    Linux での MongoDB のインストールの詳細については、次を参照してください。 [クイック スタート Unix][QuickstartUnix]します。

5. データベースを開始するには、次を実行します。

        $ mongod --dbpath /srv/datadrive/data --logpath /srv/datadrive/data/mongod.log

    すべてのログ メッセージが表示されます、 */srv/datadrive/data/mongod.log* ファイル、MongoDB サーバーが開始して、事前のジャーナル ファイルに割り当てるとします。 MongoDB がジャーナル ファイルを事前に割り当てて、接続のリッスンを開始するには、数分かかる場合があります。

6. MongoDB 管理シェルを開始するには、個別の SSH または PuTTY ウィンドウを開いて実行します。

        $ mongo
        > db.foo.save ( { a:1 } )
        > db.foo.find()
        { _id : ..., a : 1 }
        > show dbs  
        ...
        > show collections  
        ...  
        > help  

    データベースは挿入によって作成されます。

7. MongoDB をインストールしたら、MongoDB にリモートでアクセスできるように、エンドポイントを構成する必要があります。 管理ポータルで、クリックして **仮想マシン**, を新しい仮想マシンの名前をクリックしてクリックして **エンドポイント**します。
    
    ![エンドポイント][Image7]

8. クリックして **エンドポイントの追加** ページの下部にあります。
    
    ![エンドポイント][Image8]

9. 次の設定で、エンドポイントを追加します。

 - **名前**: Mongo
 - **プロトコル**: TCP
 - **パブリック ポート**: 27017
 - **プライベート ポート**: 27017
 
 これにより、MongoDB へのリモート アクセスが可能になります。



[QuickStartUnix]: http://www.mongodb.org/display/DOCS/Quickstart+Unix


[Image7]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint.png
[Image8]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint2.png

