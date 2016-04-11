
1. 権限をエスカレートするには、次のように入力します。

        sudo -s

    パスワードを入力します。

2. MySQL Community Server エディションをインストールするには、次のように入力します。

        # zypper install mysql-community-server

    MySQL がダウンロードおよびインストールされるまで待ちます。

3. 次回システム起動時に MySQL を起動するように設定するには、次のように入力します。

        # insserv mysql

4. 次のコマンドで MySQL デーモン (mysqld) を手動で開始します。

        # rcmysql start

    MySQL デーモンの状態を確認するには、次のように入力します。

        # rcmysql status

    MySQL デーモンを停止するには、次のように入力します。

        # rcmysql stop

    > [AZURE.IMPORTANT] インストール後に、MySQL ルート パスワードは既定では空です。 実行することをお勧め **mysql \_secure\_installation**, 、MySQL の保護に役立つスクリプトです。 スクリプトを実行すると、MySQL ルート パスワードの変更、匿名のユーザー アカウントの削除、リモート ルート ログインの無効化、テスト データベースの削除、および権限テーブルの再読み込みを行うように求められます。 これらのオプションすべてに対して "はい" と答えて、ルート パスワードを変更することをお勧めします。

5. 次のように入力して、MySQL インストール スクリプトを実行します。

        $ mysql_secure_installation

6. 実行後、MySQL にログインできます。

        $ mysql -u root -p

    先ほど変更した MySQL ルート パスワードを入力すると、データベースを操作するための SQL ステートメントを実行できるプロンプトが表示されます。

7. 新しい MySQL ユーザーを作成するで次のコマンドを実行、 **mysql >** プロンプト。

        mysql> CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';

    行末のセミコロン (;) は、コマンドの終わりを示すために必要です。

8. データベースを作成し、このデータベースに対するアクセス許可を `mysqluser` ユーザーに付与するには、次のコマンドを実行します。

        mysql> CREATE DATABASE testdatabase;
        mysql> GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';

    データベースのユーザー名とパスワードは、データベースに接続するスクリプトのみが使用します。  データベース ユーザー アカウントは、システム上の実際のユーザー アカウントを表しているとは限りません。

9. 別のコンピューターからログインするには、次のように入力します。

        mysql> GRANT ALL ON testdatabase.* TO 'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';

    `ip-address` は、MySQL への接続元コンピューターの IP アドレスです。

10. MySQL データベース管理ユーティリティを終了するには、次のように入力します。

        quit

11. MySQL をインストールしたら、MySQL にリモートでアクセスできるように、エンドポイントを構成する必要があります。 ログイン、 [Azure ポータル][AzurePortal]します。 クリックして **仮想マシン**, を新しい仮想マシンの名前をクリックして、クリックして **エンドポイント**します。

12. クリックして **追加** ページの下部にあります。

13. 名前「MySQL」プロトコル エンドポイントを追加 **TCP**, 、および **パブリック** と **プライベート** ポートに「3306」に設定します。

14. お使いのコンピューターから仮想マシンにリモートで接続するには、次のように入力します。

        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net

    たとえば、このチュートリアルで作成した仮想マシンを使用する場合、次のコマンドを入力します。

        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png

