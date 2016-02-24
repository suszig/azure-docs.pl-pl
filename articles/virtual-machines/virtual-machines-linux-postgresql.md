<properties
    pageTitle="PostgreSQL を Linux VM にセットアップする | Microsoft Azure"
    description="Azure の Linux 仮想マシンに PostgreSQL をインストールして構成する方法を説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/10/2015"
    ms.author="mingzhan"/>


# Azure での PostgreSQL のインストールと構成

PostgreSQL は、Oracle や DB2 に似た高機能のオープン ソース データベースです。 PostgreSQL には、完全な ACID 準拠、信頼性の高いトランザクション処理、複数バージョンの同時実行制御など、エンタープライズ対応の機能が含まれます。 また、ANSI SQL や SQL/MED などの標準をサポートします (Oracle、MySQL、MongoDB、その他多くの外部データ ラッパーを含みます). 12 を超える手続き型言語、GIN および GIST のインデックス、空間データ、および JSON またはキーと値に基づくアプリケーションに対する NoSQL に似た複数の機能などのサポートにより、高度な拡張を行えます。

この記事では、Linux を実行している Azure Virtual Machine に PostgreSQL をインストールして構成する方法を説明します。


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## PostgreSQL のインストール

> [AZURE.NOTE] このチュートリアルを完了するために Linux を実行する Azure の仮想マシンはすでに必要があります。 作成を続行する前に、Linux VM の設定は、次を参照してください、。
[Azure Linux VM チュートリアル](virtual-machines-linux-tutorial.md)します。

この例では、PostgreSQL ポートとしてポート 1999 を使用します。  

PuTTY を使用して作成した Linux VM に接続します。 初めて Azure Linux VM を使用している場合を参照してください。 [ssh Azure 上の Linux の使用方法](virtual-machines-linux-use-ssh-key.md) を PuTTY を使用して Linux 仮想マシンに接続する方法を参照してください。

1. 次のコマンドを実行して、ルート (admin) に切り替えます。

        # sudo su -

2. 一部のディストリビューションには、PostgreSQL をインストールする前にインストールする必要がある依存アプリケーションがあります。 次の一覧でお使いのディストリビューションを確認し、適切なコマンドを実行してください。

    - Red Hat ベースの Linux:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Debian ベースの Linux:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - Suse Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. PostgreSQL をルート ディレクトリにダウンロードし、パッケージを解凍します。

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    上に示したのは例です。 詳細なダウンロード アドレスを見つけることができます、 [/pub/ソース/のインデックス](https://ftp.postgresql.org/pub/source/)します。

4. ビルドを開始するには、以下のコマンドを実行します。

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. すべてのビルドできるドキュメント (HTML およびマニュアル ページ) やその他のモジュール (contrib) などをビルドする場合は、次のコマンドを実行します。

        # gmake install-world

    次の確認メッセージが表示されます。

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## PostgreSQL の構成

1. (省略可能) シンボリック リンクを作成して、バージョン番号を含まないように PostgreSQL の参照を短縮します。

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. データベース用のディレクトリを作成します。

        # mkdir -p /opt/pgsql_data

3. root 以外のユーザーを作成し、そのユーザーのプロファイルを変更します。 次に、この新しいユーザーに切り替えます (と呼ばれる *postgres* この例では)。

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] セキュリティ上の理由は、PostgreSQL は、初期化、開始、またはデータベースをシャット ダウンするルートではないユーザーを使用します。


4. 編集、 *bash_profile* ファイルを次のコマンドを入力します。 これらの行の末尾に追加されます、 *bash_profile* ファイル。

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. 実行、 *bash_profile* ファイル。

        $ source .bash_profile

6. 次のコマンドを使用してインストールを検証します。

        $ which psql

    インストールが成功した場合は、次の応答が表示されます。

        /opt/pgsql/bin/psql

7. PostgreSQL のバージョンをチェックすることもできます。

        $ psql -V

8. ディスクを初期化します。

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    次の出力が表示されます。

![image](./media/virtual-machines-linux-postgresql/no1.png)

## PostgreSQL のセットアップ

<!--    [postgres@ test ~]$ exit -->

次のコマンドを実行します。

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

/etc/init.d/postgresql ファイルの 2 つの変数を変更します。 PostgreSQL のインストール パスにプレフィックスが設定されている: **/選択時**します。 PostgreSQL のデータの保存パスに設定されている PGDATA: **/opt/pgsql_data**します。

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![image](./media/virtual-machines-linux-postgresql/no2.png)

実行可能なようにファイルを変更します。

    # chmod +x /etc/init.d/postgresql

PostgreSQL を開始します。

    # /etc/init.d/postgresql start

PostgreSQL のエンドポイントがオンかどうかを確認します。

    # netstat -tunlp|grep 1999

次の出力が表示されます。

![image](./media/virtual-machines-linux-postgresql/no3.png)

## Postgres データベースへの接続

postgres ユーザーに再び切り替えます。

    # su - postgres

Postgres データベースを作成します。

    $ createdb events

先に作成したイベント データベースに接続します。

    $ psql -d events

## Postgres テーブルの作成および削除

データベースに接続しているため、そこにテーブルを作成できます。

たとえば、サンプルの Postgres テーブルを新しく作成するには、次のコマンドを実行します。

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

次の列名と制限で 4 列のテーブルを設定しました。

1. “name” 列は、VARCHAR コマンドによって、20 文字以下に制限されています。
2. "food" 列は各自が持ってくる食品アイテムを示します。 VARCHAR によってこのテキストは 30 文字以下に制限されます。
3. “confirmed” 列は、ユーザーが料理に対する RSVP を持っているかどうかを記録します。 許容される値は、"Y" と "N" です。
4. “date” 列は、イベントにサインアップした日付を示します。 Postgres では、日付を yyyy-mm-dd と記述する必要があります。

テーブルが正常に作成された場合、次が表示されます。

![image](./media/virtual-machines-linux-postgresql/no4.png)

次のコマンドを使用して、テーブルの構造をチェックすることもできます。

![image](./media/virtual-machines-linux-postgresql/no5.png)

### テーブルにデータを追加する

最初に、行に情報を挿入します。

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

次のように出力されます。

![image](./media/virtual-machines-linux-postgresql/no6.png)

テーブルにさらに人を追加することもできます。 オプションをいくつか次に示します。独自に作成することもできます。

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### テーブルの表示

テーブルを表示するには、次のコマンドを使用します。

    select * from potluck;

出力は次のようになります。

![image](./media/virtual-machines-linux-postgresql/no7.png)

### テーブルのデータの削除

テーブル内のデータを削除するには、次のコマンドを使用します。

    delete from potluck where name=’John’;

これにより、"John" 列内のすべての情報が削除されます。 出力は次のようになります。

![image](./media/virtual-machines-linux-postgresql/no8.png)

### テーブルのデータの更新

テーブル内のデータを更新するには、次のコマンドを使用します。 この例では、Sandy が参加を確定したため、RSVP を "N" から "Y" に変更します。

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##PostgreSQL の詳細情報
PostgreSQL を Azure Linux VM にインストールする作業が完了したため、Azure でそれを使用できるようになりました。 PostgreSQL に関する詳細についてにアクセスしてください、 [PostgreSQL の web サイト](http://www.postgresql.org/)します。

