<properties
    pageTitle="負荷分散セットを使用して MySQL をクラスター化する |Microsoft Azure"
    description="Azure 上のクラシック デプロイ モデルを使用して作成された負荷分散された可用性の高い Linux MySQL クラスターをセットアップする"
    services="virtual-machines"
    documentationCenter=""
    authors="bureado"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/14/2015"
    ms.author="jparrel"/>


# 負荷分散セットを使用して Linux の MySQL をクラスター化する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


Linux ベースの高可用性サービスを Microsoft Azure 上にデプロイするには、さまざまな方法が利用できます。この記事の目的はそれぞれの方法を詳細に説明することですが、MySQL サーバーの高可用性についての初歩的な知識も説明します。 この方法を示したビデオは [Channel 9](http://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL)します。

ここでは、DRBD、Corosync、Pacemaker をベースとする、シェアード ナッシング、2 ノード、1 マスターの MySQL 高可用性ソリューションについて概説します。 MySQL を同時に実行しているノードは 1 つだけです。 また、DRBD リソースに対して同時に読み書きできるノードも 1 つに制限されます。

Microsoft Azure の負荷分散セットを使用すると、ラウンド ロビン、エンドポイント検出、VIP の削除と適切な回復などの機能が提供されるため、LVS のような VIP ソリューションは必要ありません。 VIP はグローバルにルーティング可能な IPv4 アドレスで、最初にクラウド サービスを作成するときに、Microsoft Azure によって割り当てられます。

その他の NBD Cluster、Percona、Galera など、少なくとも 1 つを含むいくつかのミドルウェア ソリューションなどの MySQL 用アーキテクチャがある VM として利用可能な [VM Depot](http://vmdepot.msopentech.com)します。 これらのソリューションが、(マルチキャストまたはブロードキャストではなく) ユニキャストでのレプリケーションが可能で、共有ストレージや複数のネットワーク インターフェイスに依拠しないものであれば、Microsoft Azure 上で簡単にデプロイできます。

もちろん、これらのクラスタリング アーキテクチャは、他の PostgreSQL や OpenLDAP などの製品にも同様の方法で拡張できます。 たとえば、ここで挙げたシェアード ナッシング方式の負荷分散手順は、マルチマスターの OpenLDAP でのテストに成功しています。その内容は、マイクロソフトの Channel 9 ブログで見ることができます。

## 準備

作業に入る前に、少なくとも 2 つの VM (この例では、XS を使用)、1 つのネットワーク、1 つのサブネット、1 つのアフィニティ グループ、1 つの可用性セットを作成できるサブスクリプションを取得している Microsoft Azure のアカウントが必要です。また、クラウド サービスと同じリージョンに新しい VHD を作成し、それを Linux の VM に接続できるようにしておくことも必要です。

### テスト環境

- Ubuntu 13.10
  - DRBD
  - MySQL サーバー
  - Corosync および Pacemaker

### アフィニティ グループ

Azure クラシック ポータルにログインし、下にスクロールして、[設定] でこのソリューション用の新しいアフィニティ グループを作成します。 後で作成したリソースは、このアフィニティ グループに割り当てられます。

### ネットワーク

ネットワークを作成し、その中にサブネットを作成します。 ここでは、中に /24 のサブネットを 1 つだけ含む 10.10.10.0/24 ネットワークを選択します。

### 仮想マシン

最初の Ubuntu 13.10 の VM は Ubuntu ギャラリーの動作保証済みイメージを使用して作成されと呼ばれる `hadb01`します。 このとき、hadb という名前の新しいクラウド サービスを作成します。 このような名前にしたのは、リソースを追加するときにこのサービスが持つ共有、負荷分散という性質を表すためです。 作成 `hadb01` とは簡単に、ポータルを使用します。 SSH エンドポイントは自動的に作成されます。ネットワークは、先ほど作成したネットワークを選択します。 VM 用の新しい可用性セットの作成も選択します。

2 つ目の VM を作成したら (厳密に言えば、クラウド サービスの作成時に)、最初の VM を作成すると `hadb02`します。 目の vm はも使用します Ubuntu 13.10 の VM ポータルを使用してギャラリーからは既存のクラウド サービスを使用するように選択して `hadb.cloudapp.net`, 、代わりに、新しいレコードを作成します。 ネットワークと可用性セットは自動的に選択されます。 SSH エンドポイントも作成されます。

SSH ポートのメモが取得され、両方の Vm を作成した後 `hadb01` (TCP 22) および `hadb02` (Azure によって自動的に割り当て済み)

### ストレージの接続

両方の VM に新しいディスクを接続し、それぞれに新しい 5 GB のディスクを作成します。 これらのディスクは、オペレーティング システムのメイン ディスクとして使用されている VHD コンテナーでホストされます。 ディスクが作成され、接続されているため、Linux カーネルでは、新しいデバイスを表示しますを再起動する必要はありません (通常 `/デベロッパー/sdc`, 、チェックする `dmesg` 出力の)。

新しいパーティションの作成という名前の各 VM で `cfdisk` (プライマリ、Linux パーティション) と、新しいパーティション テーブルを作成します。 **このパーティションには、ファイルシステムを作成しないでください。**

## クラスターのセットアップ

両方の Ubuntu VM に Corosync、Pacemaker、および DRBD をインストールするには、APT を使用する必要があります。 使用して `apt get`:

    sudo apt-get install corosync pacemaker drbd8-utils.

**この時点では MySQL をインストールしないでください。** Debian および Ubuntu のインストール スクリプトが MySQL のデータ ディレクトリを初期化は `/var/lib/mysql`, 、ディレクトリは DRBD のファイルシステムに置き換わる、のでを後でこれを行うには必要があります。

この時点で確認する必要も (を使用して `ためには、ifconfig`) 両方の Vm で 10.10.10.0/24 サブネットのアドレスと、ping を送信できる他の名前を使用していること。 使用することも必要な場合は `で問題` と `ssh コピー-id` 両方の Vm は、パスワードを求めることがなく SSH 経由で通信できるかどうかを確認します。

### DRBD のセットアップ

基になるを使用する DRBD リソースを作成して、 `/dev/sdc1` パーティションを `/dev/drbd1` リソースことはできません ext3 を使用してフォーマットし、プライマリとセカンダリの両方のノードで使用します。 これを行うには、開く `/etc/drbd.d/r0.res` し、次のリソース定義をコピーします。 両方の VM でこの操作を行います。

    resource r0 {
      on `hadb01` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.4:7789;
        meta-disk internal;
      }
      on `hadb02` {
        device  /dev/drbd1;
        disk   /dev/sdc1;
        address  10.10.10.5:7789;
        meta-disk internal;
      }
    }

その後で、初期化を使用して、リソース `drbdadm` 両方の Vm で。

    sudo drbdadm -c /etc/drbd.conf role r0
    sudo drbdadm up r0

最後に、プライマリ (`hadb01`)、DRBD リソースの所有権 (プライマリ) を強制適用します。

    sudo drbdadm primary --force r0

Proc/drbd の内容を確認する場合 (`sudo cat/proc/drbd`) 両方の Vm に表示されます `プライマリ/セカンダリ` に `hadb01` と `セカンダリ/プライマリ` に `hadb02`, 、この時点でのソリューションと一致します。 5 GB ディスクは、お客様への課金なしで 10.10.10.0/24 ネットワーク上で同期されます。

ファイル システムを作成するにはディスクが同期 `hadb01`します。 テストのために ext2 を使用しましたが、次の手順を実行すると、ext3 ファイルシステムが作成されます。

    mkfs.ext3 /dev/drbd1

### DRBD リソースのマウント

`Hadb01` DRBD リソースをマウントする準備ができました。 Debian 系および使用 `/var/lib/mysql` MySQL のデータ ディレクトリとして。 まだ MySQL をインストールしていないため、このディレクトリを次のようにして作成し、DRBD リソースをマウントします。  `Hadb01`:

    sudo mkdir /var/lib/mysql
    sudo mount /dev/drbd1 /var/lib/mysql

## MySQL のセットアップ

これで、MySQL をインストールする準備ができたら `hadb01`:

    sudo apt-get install mysql-server

`Hadb02`, 、2 つのオプションがあります。 1 つは、MySQL サーバーをインストールすることです。これにより、/var/lib/mysql が作成されるため、このディレクトリを新しいデータ ディレクトリに割り当てて、その中にあるものを削除します。  `Hadb02`:

    sudo apt-get install mysql-server
    sudo service mysql stop
    sudo rm –rf /var/lib/mysql/*

2 番目のオプションは、フェールオーバーする `hadb02` し、そこに mysql サーバーをインストール (インストール スクリプトは、既存のインストールを確認し、それ)

`Hadb01`:

    sudo drbdadm secondary –force r0

`Hadb02`:

    sudo drbdadm primary –force r0
    sudo apt-get install mysql-server

ここで、DRBD のフェールオーバーを計画していない場合、1 つ目の選択肢は簡単ですがスマートさに欠けるかもしれません。 このセットアップを行うと、次のように、MySQL データベースの操作を開始できます。  `Hadb02` (または、drbd のどちらかのサーバーがアクティブで)。

    mysql –u root –p
    CREATE DATABASE azureha;
    CREATE TABLE things ( id SERIAL, name VARCHAR(255) );
    INSERT INTO things VALUES (1, "Yet another entity");
    GRANT ALL ON things.\* TO root;

**警告**: この最後のステートメントは、このテーブルのルート ユーザーに対する認証を無効にします。 この部分は、製品レベルの GRANT ステートメントに置き換えて、単なる説明用として含める必要があります。

また、この記事の目的は VM の外部からクエリを実行することですが、その場合には、MySQL 用のネットワークを有効にする必要があります。 両方の Vm で開く `/etc/mysql/my.cnf` を見つけて `バインド アドレス`, 、それを 127.0.0.1 から 0.0.0.0 に変更します。 ファイルを保存して、発行、 `sudo service mysql 再起動` 、現在のプライマリにします。

### MySQL 負荷分散セットの作成

私たちは、ポータルに戻ってを見つけて、 `hadb01` VM、さらにエンドポイント。 ここで、新しいエンドポイントを作成します。ドロップダウンから MySQL (TCP 3306) をクリックし、*[新しい負荷分散セットを作成する]* チェック ボックスをオンにします。 次に、負荷分散されたエンドポイントを呼び出します `lb mysql`します。 時間は最低値の 5 (秒) に減らしますが、それ以外のほとんどのオプションはそのままにします。

エンドポイントを作成したらへと向かう `hadb02`, 、エンドポイントとは、新しいエンドポイントを作成しますが `lb mysql`, 、ドロップダウン メニューから MySQL を選択します。 この手順では、Azure CLI も使用できます。

これで、クラスターの手動操作に必要なものはすべて揃いました。

### 負荷分散セットのテスト

テストは、外部のマシンから、MySQL クライアントやアプリケーション (たとえば、Azure Website を実行している phpMyAdmin) を使用して実行できます。ここでは、別の Linux ボックスで MySQL のコマンド ライン ツールを使用します。

    mysql azureha –u root –h hadb.cloudapp.net –e "select * from things;"

### 手動によるフェールオーバー

フェールオーバーのシミュレーションができる状態になりました。MySQL をシャットダウンし、DRBD のプライマリを切り替え、MySQL を再起動します。

hadb01 の場合:

    service mysql stop && umount /var/lib/mysql ; drbdadm secondary r0

hadb02 の場合:

    drbdadm primary r0 ; mount /dev/drbd1 /var/lib/mysql && service mysql start

手動でフェールオーバーすると、リモート クエリを繰り返すことができます。正しく動作することを確認します。

## Corosync のセットアップ

Corosync は、クラスターの基盤となるインフラストラクチャで、Pacemaker を動作させるために必要です。 Heartbeat v1 と v2 のユーザー (および他の Ultramonkey などの方法) から見れば、Corosync には Heartbeat の CRM の一部の機能しかありません。一方、Pacemaker には、より Hearbeat に近い機能が残されています。

Azure で Corosync を使用する場合、Microsoft Azure のネットワークがユニキャスト通信しかサポートしないのに対して、Corosync は、ユニキャストよりもブロードキャスト、ブロードキャストよりもマルチキャストを優先するという大きな制約があります。

ただし、Corosync にはユニキャスト動作モードがあり、実際上の制約は、各ノードのノード間通信が*自動的には*行われないため、各ノードの IP アドレスを含む構成ファイルでノードを定義する必要があることです。 Corosync サンプル ファイルを使用してには、ユニキャストおよび変更するだけバインド アドレス、ノード リスト、およびログ ディレクトリ (Ubuntu を使用して `/var/log/corosync` 、サンプル ファイルを使用して `/var/log/cluster`) クォーラム ツールを有効にします。

* * 注意してください、 `トランスポート: udpu` ディレクティブ、およびノード * の手動で定義した IP アドレスです。

`/Etc/corosync/corosync.conf` 両方のノード。

    totem {
      version: 2
      crypto_cipher: none
      crypto_hash: none
      interface {
        ringnumber: 0
        bindnetaddr: 10.10.10.0
        mcastport: 5405
        ttl: 1
      }
      transport: udpu
    }
    
    logging {
      fileline: off
      to_logfile: yes
      to_syslog: yes
      logfile: /var/log/corosync/corosync.log
      debug: off
      timestamp: on
      logger_subsys {
        subsys: QUORUM
        debug: off
        }
      }
    
    nodelist {
      node {
        ring0_addr: 10.10.10.4
        nodeid: 1
      }
    
      node {
        ring0_addr: 10.10.10.5
        nodeid: 2
      }
    }
    
    quorum {
      provider: corosync_votequorum
    }

両方の VM でこの構成ファイルをコピーし、両方のノードで Corosync を起動します。

    sudo service start corosync

サービスを開始するとすぐに、現在のリング内にクラスターが構築される必要があります。また、クォーラムも構成される必要があります。 この機能をチェックするには、ログを確認するかまたは次のコードで確認します。

    sudo corosync-quorumtool –l

次のイメージに近い出力が必要です。

![corosync-quorumtool -l sample output](media/virtual-machines-linux-mysql-cluster/image001.png)

## Pacemaker のセットアップ

Pacemaker は、クラスターを利用してリソースを監視し、プライマリがダウンしたときにリソースをセカンダリに切り替える時期を定義します。 リソースは、利用できるスクリプト セット、LSB (init 風) スクリプトなどの選択肢から定義できます。

Pacemaker が DRBD リソース、マウント ポイント、および MySQL サービスを "所有" するようにします。 プライマリに不具合が発生したとき、Pacemaker によって DRBD の有効/無効、マウント/マウントの解除、MySQL の起動/停止を正しい順序で切り替えることができれば、セットアップは完了したことになります。

初めて Pacemaker をインストールする場合には、次のような簡単な構成にします。

    node $id="1" hadb01
      attributes standby="off"
    node $id="2" hadb02
      attributes standby="off"

実行してチェック `sudo crm 構成ショー`します。 ここで、ファイルを作成 (と答えると、 `/tmp/cluster.conf`)、次のリソースを使用します。

    primitive drbd_mysql ocf:linbit:drbd \
          params drbd_resource="r0" \
          op monitor interval="29s" role="Master" \
          op monitor interval="31s" role="Slave"
    
    ms ms_drbd_mysql drbd_mysql \
          meta master-max="1" master-node-max="1" \
            clone-max="2" clone-node-max="1" \
            notify="true"
    
    primitive fs_mysql ocf:heartbeat:Filesystem \
          params device="/dev/drbd/by-res/r0" \
          directory="/var/lib/mysql" fstype="ext3"
    
    primitive mysqld lsb:mysql
    
    group mysql fs_mysql mysqld
    
    colocation mysql_on_drbd \
           inf: mysql ms_drbd_mysql:Master
    
    order mysql_after_drbd \
           inf: ms_drbd_mysql:promote mysql:start
    
    property stonith-enabled=false
    
    property no-quorum-policy=ignore

次に、このファイルを前述の構成に読み込みます (1 つのノードで行うだけで十分です)。

    sudo crm configure
      load update /tmp/cluster.conf
      commit
      exit

両方のノードで、ブート時に Pacemaker が起動することも確認します。

    sudo update-rc.d pacemaker defaults

数秒するを使用して、 `sudo crm_mon – L`, 、ノードの 1 つが、クラスターのマスターなりことのすべてのリソースが実行されていることを確認します。 mount と ps を使用して、リソースが実行中であることを確認できます。

次のスクリーン ショット `crm_mon` 1 つのノードが停止しました (exit コントロール C を使用して)

![crm_mon node stopped](media/virtual-machines-linux-mysql-cluster/image002.png)

次のスクリーンショットでは、両方のノードのうち、1 つがマスター、もう 1 つがスレーブとなっています。

![crm_mon operational master/slave](media/virtual-machines-linux-mysql-cluster/image003.png)

## テスト

これで、自動フェールオーバー シミュレーションの準備が完了しました。 このシミュレーションを実行する場合、ソフト方式とハード方式の 2 つの方法があります。 ソフト方式は、クラスターのシャット ダウン機能を使用して: `crm_standby-u 'uname-n'-v で`します。 マスターでこの機能を使用し、スレーブが引き継ぎます。 この機能は、必ずオフに戻してください (そうしないと、crm_mon は、1 つのノードがスタンバイ中であると通知します)。

ハード方式は、ポータル経由でプライマリ VM (hadb01) をシャットダウンするか、この VM の実行レベル (停止、シャットダウンなど) を変更します。さらに、マスターがダウンしているという通知を Corosync と Pacemaker に送る必要があります。 このテストはメンテナンス時に有用ですが、単に VM をフリーズさせるだけでも同じ状況を強制的に作り出すこともできます。

## STONITH

物理デバイスを制御する STONITH スクリプトの代わりに、Azure CLI 経由で VM のシャットダウンを発行することができます。 使用する `/usr/lib/stonith/plugins/external/ssh` としてベース アドレスとクラスターの構成で STONITH を有効にします。 Azure CLI をグローバルにインストールし、発行設定/プロファイルをクラスターのユーザー用に読み込む必要があります。

サンプル コードで使用可能なリソースの [GitHub](https://github.com/bureado/aztonith)します。 次のコードを追加することで、クラスターの構成を変更する必要があります `sudo crm 構成`:。

    primitive st-azure stonith:external/azure \
      params hostlist="hadb01 hadb02" \
      clone fencing st-azure \
      property stonith-enabled=true \
      commit

**注:** このスクリプトは、up/down のチェックは実行しません。 元の SSH リソースには、15 の ping チェックがありますが、Azure VM の復旧時間はさらに大きな可変性を持ちます。

## 制限事項

次の制限事項が適用されます。

- DRBD Pacemaker はリソースとして管理する linbit DRBD リソース スクリプト `ダウン drbdadm` シャット ダウンする場合、ノードでも、ノードには、スタンバイを始めたばかりです。 マスターへの書き込み中はスレーブは DRBD リソースと同期しないため、このような動作は適切ではありません。 マスターのフェールオーバーが正しく行われなかった場合、スレーブは古いファイルシステムの状態を引き継ぐ可能性があります。 こうした状況は、次の 2 つの方法により解決する可能性があります。
  - 適用する、 `drbdadm up r0` ローカルの (クラスター化されていない) ウォッチドッグを介してすべてのクラスター ノードで、または、
  - Linbit DRBD スクリプトを編集しことを確認します `ダウン` が呼び出されない `/usr/lib/ocf/resource.d/linbit/drbd`します。
- 負荷分散の応答には、少なくとも 5 秒必要なため、アプリケーションはクラスター対応であると共に、タイムアウトに寛容である必要があります。アプリケーション内キュー、クエリ ミドルウェアなど、他のアーキテクチャが役立つ可能性もあります。
- 適切なペースで書き込みが実行されるためには MySQL の調整が必要です。また、メモリ損失を最小化するため、できるだけ頻繁にキャッシュをディスクにフラッシュします。
- DRBD はデバイスをリプリケートするためのメカニズムとして、仮想スイッチで VM を相互に接続しているため、書き込み性能は、仮想スイッチでの接続状態に左右されます。





