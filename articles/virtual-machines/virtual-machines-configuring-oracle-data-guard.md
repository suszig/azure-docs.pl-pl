<properties
    pageTitle="VM で Oracle Data Guard を構成する |Microsoft Azure"
    description="高可用性と障害復旧のために Oracle Data Guard を Azure 仮想マシンにセットアップして実装するチュートリアルの手順。"
    services="virtual-machines"
    authors="bbenz"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="06/22/2015"
    ms.author="bbenz" />


# Azure 用の Oracle データ保護の構成

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


このチュートリアルでは、高可用性と障害復旧のための Azure Virtual Machines 環境に  Oracle Data Guard をセットアップ、実装する方法について説明します。 このチュートリアルでは、RAC 以外の Oracle データベースの 1 ウェイレプリケーションの方法について説明します。

Oracle Data Guard は Oracle データベースのデータの保護および障害復旧をサポートします。 シンプルで高パフォーマンスな、障害復旧、データの保護、および Oracle データベース全体の高可用性のすぐに使えるソリューションです。

このチュートリアルでは、Oracle データベースの高可用性と障害復旧の概念に関する理論的かつ実用的な知識があることを前提としています。 詳細については、次を参照してください。、 [Oracle の web サイト](http://www.oracle.com/technetwork/database/features/availability/index.html) 、さらに、 [Oracle データ保護の概念と管理に関するガイド](http://docs.oracle.com/cd/E11882_01/server.112/e17022/create_ps.htm)します。

また、このチュートリアルでは、次の前提条件が既に実装されていることを前提としています。

- 高可用性と障害回復に関する考慮事項のセクションを既に確認していること、 [Oracle 仮想マシン イメージ - 他の考慮事項](virtual-machines-miscellaneous-considerations-oracle-virtual-machine-images.md) トピックです。 Azure では、スタンドアロンの Oracle Database インスタンスをサポートしていますが、Oracle Real Application Cluster (Oracle RAC) をサポートしていないことにご注意ください。

- 同じプラットフォームで提供される Windows Server 上の Oracle Enterprise Edition のイメージを使用して、Azure に 2 つの Virtual Machines (VM) を作成している。 詳細については、次を参照してください。 [Azure で Oracle Database 12c 仮想マシンを作成する](virtual-machines-creating-oracle-webLogic-server-12c-virtual-machine.md) と [Azure の仮想マシン](http://azure.microsoft.com/documentation/services/virtual-machines/)します。 仮想マシンがであることを確認、 [同じクラウド サービス](virtual-machines-load-balance.md) あり、同じ [仮想ネットワーク](azure.microsoft.com/documentation/services/virtual-network/) に永続的なプライベート IP アドレスを介して相互にアクセスできることを確認します。 また、同じ Vm を配置お勧め [可用性セット](virtual-machines-manage-availability.md) Azure が別個のフォールト ドメインに配置し、アップグレード ドメインを許可するようにします。 Oracle Data Guard は Oracle Database Enterprise Edition でのみ使用可能となっていますのでご注意ください。 それぞれのマシンには、少なくとも 2 GB のメモリと 5 GB のディスク領域が必要です。 VM のサイズを提供するプラットフォームに関する最新情報についてを参照してください。 [Azure の仮想マシンのサイズ](http://msdn.microsoft.com/library/dn197896.aspx)します。 VM に追加のディスク ボリュームが必要な場合は、追加のディスクをアタッチすることができます。 詳細については、次を参照してください。 [にデータ ディスクを仮想マシンに接続する方法](storage-windows-attach-disk.md)します。

- Azure クラシック ポータルでは、仮想マシンの名前は、プライマリ VM が「Machine1」として、スタンバイ VM が「Machine2」として設定されています。

- 設定する、 **ORACLE_HOME** など、プライマリおよびスタンバイ仮想マシン内で同じ oracle ルート インストール パス] をポイントする環境変数 `C:\OracleDatabase\product\11.2.0\dbhome_1\database`します。

- Windows サーバーに、**Administrators**グループのメンバーまたは **ORA_DBA** グループのメンバーとしてログオンしている。

このチュートリアルでは、次のことについて説明します。

物理スタンバイ データベース環境の展開

1. プライマリ データベースの作成

2. スタンバイ データベースを作成るためのプライマリ データベースの準備

    1. 強制ログ記録の有効化

    2. パスワード ファイルの作成

    3. スタンバイ redo ログの構成

    4. アーカイブの有効化

    5. プライマリ データベースの初期化パラメーターの設定

物理スタンバイデータベースの作成

1. スタンバイ データベースの初期化パラメーターのファイルを準備

2. プライマリおよびスタンバイのマシンでデータベースをサポートするためのリスナーと tnsnames の構成

    1. 両方のデータベースのエントリを保持するために両方のサーバーで listener.ora を構成

    2. プライマリおよびスタンバイの両方のデータベースのエントリを保持するために、プライマリおよびスタンバイの Virtual Machines で tnsnames.ora を構成

    3. リスナーを開始し、両方のサービスの両方の Virtual Machines で tnsping を確認します。

3. マウント状態でスタンバイインスタンスを起動

4. RMAN を使用して、データベースのクローンとスタンバイ データベースを作成

5. 物理スタンバイ データベースを管理回復モードで起動

6. 物理スタンバイデータベースの検証

> [AZURE.IMPORTANT] このチュートリアルは、次のハードウェアおよびソフトウェア構成に対してセットアップ、テストされています。
>
>|                     | **プライマリ データベース**                      | **スタンバイ データベース**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Oracle リリース**   |Oracle11g Enterprise リリース (11.2.0.4.0) |Oracle11g Enterprise リリース (11.2.0.4.0) |
>| **マシン名**     |Machine1                                  |Machine2                                  |
>| **オペレーティング システム** |Windows 2008 R2 |Windows 2008 R2 |
>| **Oracle SID**       |TEST                                      |TEST\_STBY                                |
>| **Memory**           |Min 2 GB                                  |Min 2 GB                                  |
>| **ディスク領域**       |Min 5 GB                                  |Min 5 GB                                  |

Oracle データベースおよび Oracle Data Guard の今後のリリースでは、追加の変更を実装する必要が生じる可能性があります。 最新バージョンについては、次を参照してください。 [Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) と [Oracle データベース](http://www.oracle.com/us/corporate/features/database-12c/index.html) Oracle の web サイトのドキュメントです。

## 物理スタンバイ データベース環境の展開

### 1.プライマリ データベースの作成

- プライマリ仮想マシンでプライマリ データベース "TEST" を作成 詳細は、「Oracle データベースの作成と構成」を参照してください。
- SQL で SYSDBA ロールを持つsql では、SYS ユーザーとしてデータベースに接続 * さらにコマンド プロンプトおよびデータベースの名前を表示するには、次のステートメントを実行します。

      SQL> select name from v$database;
    
      The result will display like the following:
    
      NAME
      ---------
      TEST

- 次に、dba_data_files システム ビューからデータベース ファイルの名前をクエリします。

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF


### 2.スタンバイ データベースを作成るためのプライマリ データベースの準備

スタンバイ データベースを作成する前に、プライマリ データベースが正しく構成されていることを確認することを推奨します。 実行する必要のある手順の一覧を次に示します。

1. 強制ログ記録の有効化
2. パスワード ファイルの作成
3. スタンバイ redo ログの構成
4. アーカイブの有効化
5. プライマリ データベースの初期化パラメーターの設定

#### 強制ログ記録の有効化

スタンバイ データベースを展開するには、プライマリ データベースでの「強制ログ」を有効にする必要があります。 このオプションにより、イベントで 'nologging' 操作が完了した時も、強制ログが優先されすべての操作は、redo ログに記録されます。 そのため、プライマリ データベース内のすべてがログに記録され、スタンバイへレプリケーションには、プライマリ データベースでのすべての操作が含まれていることを確認します。 強制ログを有効にするには、alter database ステートメントを実行します。

    SQL> ALTER DATABASE FORCE LOGGING;
    
    Database altered.

#### パスワード ファイルの作成

プライマリサーバーからスタンバイサーバーまでアーカイブされたログを送り適用するは、sys パスワードはプライマリおよびスタンバイサーバーの両方で同一でなくてはなりません。 そのため、プライマリ データベースでパスワード ファイルを作成し、スタンバイ サーバーにコピーします。
>[AZURE.IMPORTANT] Oracle Database 12 c を使用する場合は、Oracle Data Guard の管理に使用できる新しいユーザー**SYSDG**があります。 詳細については、次を参照してください。 [Oracle Database 12c Release 変更](http://docs.oracle.com/cd/E16655_01/server.121/e10638/release_changes.htm)します。

さらに、ORACLE\_HOME 環境が既に Machine1 で定義されていることを確認します。 そうでない場合は「環境変数」ダイアログ ボックスを使用して環境変数として定義します。 このダイアログ ボックスにアクセスするには、**コントロール パネル**の「システム」アイコンをダブルクリックして**システム**ユーティリティを開始します。次に**詳細設定**タブをクリックし**環境変数**を選択します。 **システム変数**で**新規**ボタンをクリックして環境変数を設定します。 環境変数を設定した後に、既存の Windows コマンド プロンプトを閉じ、新しいものを開きます。

C:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\database などの Oracle\_Home ディレクトリに切り替えるには、次のステートメントを実行します。

    cd %ORACLE_HOME%\database

次に、パスワード ファイル作成ユーティリティを使用してパスワード ファイルを作成 [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm)します。 同じ Machine1 Windows のコマンド プロンプトで**SYS**のパスワードのパスワード値を設定することにより、次のコマンドを実行します。

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

このコマンドでは、ORACLE\_HOME\\database ディレクトリ内に pwdtest.ora という名前のパスワード ファイルを作成します。 このファイルは、手動で Machine2 で %ORACLE\_HOME%\\database ディレクトリにコピーする必要があります。

#### スタンバイ redo ログの構成

次に、スタンバイ状態になったときに、プライマリが再実行を受信正しくできるように、スタンバイの redo ログを構成する必要があります。 ここで事前作成すると、スタンバイ状態で redo ログも自動的に作成できます。 オンライン redo ログと同じサイズでスタンバイ redo ログ (SRL) を構成することが重要です。 現在のスタンバイ redo ログ ファイルのサイズは、現在のプライマリ データベースのオンライン redo ログ ファイルのサイズと正確に一致しなくてはなりません。

Sql \~server で、次のステートメントを実行する * PLUS コマンド プロンプト Machine1 でします。 v$logfile は redo ログ ファイルに関する情報を含むシステム ビューです。

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800

52428800 が 50 メガバイトであることに注意してください。

次に、sql \~server * Plus ウィンドウ、スタンバイ データベースに新しいスタンバイ redo ログ ファイル グループを追加して、グループ句を使用してグループを識別する番号を指定するには、次の文を実行します。 グループ番号を使用するとスタンバイ redo ログのファイル グループを簡単に管理することができます。

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

次に、 redo ログ ファイルについての情報の一覧するために次のシステム ビューを実行します。 この操作では、スタンバイ redo ログ ファイルのグループが作成されたことを確認できます。

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### アーカイブの有効化

次に、プライマリ データベースを ARCHIVELOG モードにして自動保存を有効にするには、次のステートメントを実行してアーカイブを有効にします。 データベースをマウントすることにより、アーカイブ ログ モードを有効にし、archivelog コマンドを実行します。

まず、sysdba としてログインします。 Windows のコマンド プロンプトで次を実行します。

    sqlplus /nolog
    
    connect / as sysdba

Sql \~server でデータベースをシャット ダウンし、* Plus コマンド プロンプト。

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

次に、データベースをマウントするために、 startup mount コマンドを実行します。 これにより、Oracle で指定されたデータベースでインスタンスを関連付けます。

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

次に、以下を実行します。

    SQL> alter database archivelog;
    Database altered.

次に、Alter database ステートメントをオープンクローズで実行、データベースを通常の使用に使用できるようにします。

    SQL> alter database open;
    
    Database altered.

#### プライマリ データベースの初期化パラメーターの設定

Data Guard を構成するには、正規 pfile (初期化パラメーターのテキスト ファイル) で、スタンバイのパラメーターを作成して構成する必要があります。  Pfile の準備ができたら、server パラメーター ファイル (SPFILE) に変換する必要があります。

INIT.ORA パラメーターを使用して、Data Guard 環境を制御できます。 このチュートリアルに従うと、プライマリ データベースの INIT.ORA を更新する必要があります。そのためプライマリ状態またはスタンバイ状態の両方のロールを保持できます。

    SQL> create pfile from spfile;
    File created.

次に、スタンバイのパラメーターを追加するには pfile を編集する必要があります。そのためには、INITTEST を開きます。%Oracle\_home%\\database の場所に ORA ファイルです。次に、INITTEST.ora ファイルに次のステートメントを追加します。なお、INIT の名前付け規則。ORA ファイルは INIT\<YourDatabaseName\>します。ORA します。

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------

前のステートメント ブロックには、次の 3 つの重要なセットアップ項目が含まれています。
-   **LOG_ARCHIVE_CONFIG...:** このステートメントを使用する一意のデータベースの id を定義します。
-   **LOG_ARCHIVE_DEST_1...:** このステートメントを使用してローカルのアーカイブ フォルダーの場所を定義します。 データベースのアーカイブが必要な用の新しいディレクトリを作成し、Oracle の既定フォルダー %oracle_home%\database\archive ORACLE_HOME%\database\archive を使用するのではなく、このステートメントを明示的に使用するローカルのアーカイブ場所を指定することをお勧めします。
-   **LOG_ARCHIVE_DEST_2.LGWR:** 、非同期ログライター プロセス (LGWR) をトランザクション redo データを収集し、スタンバイ変換先に転送を定義します。 ここでは、DB_UNIQUE_NAME は転送先のスタンバイ サーバーにあるデータベースの一意の名前を指定します。

新規パラメーターファイルが準備できたら、ここから spfile を作成する必要があります。

最初に、データベースをシャット ダウンします:

    SQL> shutdown immediate;
    
    Database closed.
    
    Database dismounted.
    
    ORACLE instance shut down.

次に、スタートアップ マウント コマンドを次のように実行します。

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

次に、spfile を作成します。

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';
    
    File created.

次に、データベースをシャット ダウンします:

    SQL> shutdown immediate;
    
    ORA-01507: database not mounted

次に、インスタンスを起動するスタートアップ コマンドを使用します。

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

## 物理スタンバイデータベースの作成

このセクションでは、物理スタンバイ データベースを準備するために Machine2 で実行しなくてはならない手順について説明します。

まず、Azure クラシック ポータルを使用して Machine2 にリモート デスクトップをポイントする必要があります。

次に、スタンバイ サーバー (Machine2) では、[C:\\\ などのスタンバイ データベースに必要なすべてのフォルダーを作成<YourLocalFolder\>\\TEST します。このチュートリアルに従いながら、controlfile、datafiles, redologfiles、udump、bdump cdump ファイルなどのすべての必要なファイルを保持するために、ファイル構造が Machine1 上のフォルダー構造と一致していることを確認します。さらに、Machine2 で、ORACLE\_HOME と ORACLE\_BASE 環境変数を定義します。そうでない場合は環境変数ダイアログ ボックスを使用して環境変数として定義します。このダイアログ ボックスにアクセスするには、**コントロール パネル**の「システム」アイコンをダブルクリックして**システム**ユーティリティを開始します。次に**詳細設定**タブをクリックし**環境変数**を選択します。**システム変数**で**新規**ボタンをクリックして環境変数を設定します。環境変数を設定した後に、既存の Windows コマンド プロンプトを閉じ、新しいものを開いて変更を確認します。

次に、次の手順に従います。

1. スタンバイ データベースの初期化パラメーターのファイルを準備

2. プライマリおよびスタンバイのマシンでデータベースをサポートするためのリスナーと tnsnames の構成

    1. 両方のデータベースのエントリを保持するために両方のサーバーで listener.ora を構成

    2. プライマリおよびスタンバイの両方のデータベースのエントリを保持するために、プライマリおよびスタンバイの Virtual Machines で tnsnames.ora を構成

    3. リスナーを開始し、両方のサービスの両方の Virtual Machines で tnsping を確認します。

3. マウント状態でスタンバイインスタンスを起動

4. RMAN を使用して、データベースのクローンとスタンバイ データベースを作成

5. 物理スタンバイ データベースを管理回復モードで起動

6. 物理スタンバイデータベースの検証

### 1.スタンバイ データベースの初期化パラメーターのファイルを準備

このセクションでは、初期化パラメーターのファイルのスタンバイ データベースを準備する方法を示します。 これには、まず INITTEST.ORA を Machine 1 から Machine2 へ手動でコピーします。 INITTEST を表示することができます。2 台のコンピューターで %ORACLE\_HOME%\\database フォルダーに ORA ファイルです。 次に、スタンバイ ロールを以下のように設定するには、Machine2 で INITTEST.ora ファイルを変更します。

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'
    
    
    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30

前のステートメント ブロックには、2 つの重要なセットアップ項目が含まれています。

-   **\*.LOG_ARCHIVE_DEST_1:** Machine2 で手動で c:\OracleDatabase\TEST_STBY\archives フォルダーを作成する必要があります。
-   **\*.LOG_ARCHIVE_DEST_2:** これは省略可能な手順です。 この設定が必要になるため、プライマリ マシンがメンテナンス中の場合にスタンバイマシンがプライマリ データベースになる場合に必要となる場合があるため、これを設定します。

次に、スタンバイインスタンスを開始する必要があります。 スタンバイデータベースサーバーで、新しい Windows サービスを作成することにより Oracle インスタンスを作成するために Windows コマンド プロンプトで次のコマンドを入力します。

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

**Oradim**コマンドは Oracle インスタンスを作成しますが、開始しません。 C:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\BIN ディレクトリ内には、それを検索します。

## プライマリおよびスタンバイのマシンでデータベースをサポートするためのリスナーと tnsnames の構成

スタンバイ データベースを作成する際、構成内のプライマリ データベースとスタンバイ データベースが互いに通信しあえることを確認する必要があります。 この場合は、手動でまたはネットワーク構成ユーティリティ NETCA を使用してリスナーと TNSNames の両方を構成する必要があります。 Recovery Manager ユーティリティ (RMAN) を使用する場合、これは必須のタスクです。

### 両方のデータベースのエントリを保持するために両方のサーバーで listener.ora を構成

リモート デスクトップから Machine1 へ、および以下で指定した listener.ora ファイルを編集します。 listener.ora ファイルを編集するときには、常にかっこひらきと閉じかっこが同じ列に並んでいることを確認します。 次のフォルダーに listener.ora ファイルを見つけることができます: c:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\NETWORK\\ADMIN\\ です。

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora
    
    # Generated by Oracle configuration tools.
    
    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )
    
    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

次に、リモート デスクトップを次のように、listener.ora ファイル Machine2 を編集するため:
    # listener.ora ネットワーク構成ファイル: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.
    
    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )
    
    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

### プライマリおよびスタンバイの両方のデータベースのエントリを保持するために、プライマリおよびスタンバイの Virtual Machines で tnsnames.ora を構成

リモート デスクトップから Machine1 へ、および以下で指定した tnsnames.ora ファイルを編集します。 次のフォルダーに tnsnames.ora ファイルを見つけることができます: c:\\OracleDatabase\\product\\11.2.0\\dbhome\_1\\NETWORK\\ADMIN\\ です。

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )
    
    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

リモート デスクトップから Machine 2 へ、および次のように tnsnames.ora ファイルを編集します:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )
    
    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

### リスナーを開始し、両方のサービスの両方の Virtual Machines で tnsping を確認します。

プライマリおよびスタンバイの両方の Virtual Machines で、新しい Windows のコマンド プロンプトを開き、次のステートメントを実行します。

    C:\Users\DBAdmin>tnsping test
    
    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)
    
    
    C:\Users\DBAdmin>tnsping test_stby
    
    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)

## マウント状態でスタンバイインスタンスを起動

スタンバイ状態の仮想マシン (machine 2) にスタンバイ データベースをサポートするために、環境をセットアップする必要があります。

まず、パスワード ファイルを、プライマリ コンピューター (Machine1) からスタンバイ マシン (machine 2) に手動でコピーします。 これは**sys**パスワードは両方のコンピューターで同じである必要があるため必要となるものです。

次に、Machine2 で、Windows コマンド プロンプトを開き、次のようにスタンバイ データベースをポイントするよう環境変数を設定します。

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

次に、nomount 状態でスタンバイ データベースを起動して spfile を生成します。

データベースを開始します:

    SQL>shutdown immediate;
    
    SQL>startup nomount
    ORACLE instance started.
    
    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes

## RMAN を使用して、データベースのクローンとスタンバイ データベースを作成

Recovery Manager ユーティリティ (RMAN) を使用して、物理スタンバイ データベースを作成するために、プライマリ データベースの任意のバックアップ コピーを取ることができます。

リモートデスクトップからスタンバイ仮想マシン (MACHINE2) へ、および TARGET (プライマリ データベース、 Machine1) および AUXILLARY (スタンバイ データベース, Machine2) インスタンスの両方に完全な接続文字列を指定することで RMAN ユーティリティを実行します。
>[AZURE.IMPORTANT] スタンバイ サーバーマシンにはデータベースがまだないため、オペレーティング システムの認証は使用しないでください。

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY
    
    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

## 物理スタンバイ データベースを管理回復モードで起動

このチュートリアルでは、物理スタンバイデータベースを作成する方法について説明します。 論理スタンバイ データベースを作成する方法の詳細については、Oracle 文書を参照してください。

Sql \~server を開いて * Plus コマンド プロンプトおよびスタンバイの仮想マシンまたはサーバー (MACHINE2) 上で Data Guard を有効にする、次のようにします。

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

スタンバイ データベースを**MOUNT**モードで開いた時、アーカイブ ログ配布は続行され、管理されている回復プロセスはスタンバイ データベースに適用するログを続行します。 これにより、スタンバイ データベースをプライマリ データベースともに最新の状態に保ちます。 スタンバイ データベースはこの期間にレポートの目的ではアクセスできないことに注意してください。

**READ ONLY**モードでプライマリ データベースを開いた時、アーカイブ ログ配布は続行されます。 ただし、管理されている回復プロセスは停止されます。 これにより、管理されている回復プロセスが再開されるまで、スタンバイデータベースはどんどん古くなっていきます。 この期間中には、レポート目的でスタンバイ データベースにアクセスできますが、データには最新の変更が反映されない場合があります。

一般的には、万一プライマリ データベースに不具合が発生した場合に、データをスタンバイ データベースで最新に保つため、スタンバイ データベースは**マウント** モードで保持することが推奨されます。 ただし、アプリケーションの要件によっては、スタンバイデータベースは**READ ONLY**モードでレポート目的で保持できます。 以下の手順は、sql \~server を使用して、読み取り専用のモードで Data Guard を有効にする方法を示して * とします。

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;

## 物理スタンバイデータベースの検証

このセクションでは、管理者として高可用性構成を確認する方法を示します。

Sql \~server を開いて * Plus コマンド プロンプト ウィンドウ チェックのアーカイブ redo ログに、スタンバイ仮想マシン (Machine2)。

    SQL> show parameters db_unique_name;
    
    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY
    
    SQL> SELECT NAME FROM V$DATABASE
    
    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;
    
    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

SQL * Plus コマンド プロンプト ウィンドウを開き、プライマリマシン (Machine1) でログファイルを切り替えます。

    SQL> alter system switch logfile;
    System altered.
    
    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

アーカイブ済みの redo ログ、スタンバイ仮想マシンで (マシン 2) を確認します。

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;
    
    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES
    
    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

スタンバイ仮想マシンで (マシン 2) ギャップを確認します。

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

別の確認方法には、スタンバイ データベースへのフェールオーバーと、可能な場合にプライマリ データベースへのフェールバックをテストします。 プライマリ データベースとしてスタンバイ データベースを有効にするには、次のステートメントを使用します:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

元のプライマリ データベースで後記を有効にしていない場合、元のプライマリ データベースを削除して、スタンバイ データベースとして再作成することが推奨されます。

プライマリ データベースと、スタンバイ データベースでフラッシュバックデータベースを有効にすることをお勧めします。 フェールオーバーが発生した場合、プライマリ データベースはフェールオーバーの前にフラッシュバックされ、スタンバイ データベースにすばやく変換されます。

## その他のリソース

[Azure の oracle 仮想マシン イメージ](virtual-machines-oracle-list-oracle-virtual-machine-images.md)





