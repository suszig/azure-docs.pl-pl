<properties
    pageTitle="ディスクを Linux VM にアタッチする | Microsoft Azure"
    description="データ ディスクを Linux を実行する Azure 仮想マシンに接続し、初期化して、使用できる状態にする方法について説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2015"
    ms.author="dkshir"/>

# データ ディスクを Linux 仮想マシンに接続する方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


空のディスクと、データが含まれているディスクのどちらも接続できます。 どちらの場合も、ディスクは、実際には、Azure ストレージ アカウントに配置されている .vhd ファイルです。 また、ディスクを接続した後に、初期化して、使用できる状態にする必要があります。 この記事では、クラシック デプロイ モデルを使用して作成した仮想マシンを参照します。

> [AZURE.NOTE] 1 つまたは複数の個別のディスクを使用して仮想マシンのデータを格納することをお勧めします。 Azure の仮想マシンを作成するとき、オペレーティング システム ディスクと一時ディスクが表示されます。 **データを格納する一時ディスクを使わないでください。**名前が示すとおり、D ドライブは一時的なストレージのみを提供します。 Azure Storage に配置されていないため、冗長性やバックアップは提供しません。
> 一時ディスクが通常、Azure Linux エージェントによって管理されに自動的にマウント **/mnt/retention/リソース** (または **/mnt** Ubuntu イメージで)。 一方で、データ ディスクには Linux カーネルによって `/dev/sdc` のような名前が付けられる場合があります。その場合、このリソースをパーティション分割し、フォーマットしてからマウントする必要があります。 [Azure Linux エージェント ユーザー ガイド] [エージェント] の詳細を参照してください。

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## 方法: Linux での新しいデータ ディスクの初期化

1. 仮想マシンへの接続 手順については、[Linux を実行する仮想マシンにログオンする方法] を参照してください。 [ログオン] です。



2. 次に、データ ディスクの初期化のためにデバイスの ID を検索する必要があります。 この作業を実行する 2 つの方法があります。

    a) SSH のウィンドウで、次のコマンドを入力し、仮想マシンを管理するために作成したアカウントのパスワードを入力します。

            $sudo grep SCSI /var/log/messages

    最新の Ubuntu ディストリビューションの場合は、`/var/log/messages` へのログ記録が既定で無効になっていることがあるため、`sudo grep SCSI /var/log/syslog` の使用が必要になる場合があります。

    表示されたメッセージで、最後に追加されたデータ ディスクの識別子を確認できます。

    ![ディスク メッセージに取得](./media/virtual-machines-linux-how-to-attach-disk/DiskMessages.png)

    または

    b) を使用して、 `lsscsi` デバイス id を確認するコマンドです。 `lsscsi` いずれかでインストールできる `yum install lsscsi` (Red Hat でベースの配布) または `apt-get install lsscsi` (Debian をベースのディストリビューション)。 探しているディスクを見つけることができます、 _lun_ または **論理ユニット番号**です。 たとえば、 _lun_ に関連付けられているディスクに簡単に示されているようにの `azure vm disk list <virtual-machine>` として。

            ~$ azure vm disk list ubuntuVMasm
            info:    Executing command vm disk list
            + Fetching disk images
            + Getting virtual machines
            + Getting VM disks
            data:    Lun  Size(GB)  Blob-Name                         OS
            data:    ---  --------  --------------------------------  -----
            data:         30        ubuntuVMasm-2645b8030676c8f8.vhd  Linux
            data:    1    10        test.VHD
            data:    2    30        ubuntuVMasm-76f7ee1ef0f6dddc.vhd
            info:    vm disk list command OK

    これを、同じサンプル仮想マシンに対する `lsscsi` の出力と比較します。

            adminuser@ubuntuVMasm:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
            [5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd
            [5:0:0:2]    disk    Msft     Virtual Disk     1.0   /dev/sde

    各行の組の最後の数字は、 _lun_します。 詳細については、「`man lsscsi`」を参照してください。

3. SSH のウィンドウで、次のコマンドを入力して新しいデバイスを作成し、アカウントのパスワードを入力します。

        $sudo fdisk /dev/sdc

    >[AZURE.NOTE] この例では、使用する必要があります `sudo -i` /sbin または/usr/sbin がにない場合、一部のディストリビューションで、 `$PATH`です。


4. 求められたら、[ **n** 新しいパーティションを作成します。


    ![Create new device](./media/virtual-machines-linux-how-to-attach-disk/DiskPartition.png)

5. 求められたら、[ **p** するには、パーティションをプライマリ パーティションにするには、入力 **1** 行うことのできる、最初のパーティションをシリンダーの既定値を受け入れるように入力を入力します。


    ![Create partition](./media/virtual-machines-linux-how-to-attach-disk/DiskCylinder.png)



6. 型 **p** がパーティション分割されたディスクに関する詳細が表示されます。


    ![List disk information](./media/virtual-machines-linux-how-to-attach-disk/DiskInfo.png)



7. 型 **w** 、ディスクの設定を記述します。


    ![Write the disk changes](./media/virtual-machines-linux-how-to-attach-disk/DiskWrite.png)

8. 新しいパーティションにファイル システムを作成します。 たとえば、次のコマンドを入力し、アカウントのパスワードを入力します。

        # sudo mkfs -t ext4 /dev/sdc1

    ![Create file system](./media/virtual-machines-linux-how-to-attach-disk/DiskFileSystem.png)

    >[AZURE.NOTE] SUSE Linux Enterprise 11 システムのみサポートされている読み取り専用アクセス ext4 ファイル システムに注意してください。  これらのシステムには ext4 ではなく ext3 として新しいファイル システムの書式設定することをお勧めします。


9. 新しいファイル システムをマウントするディレクトリを作成します。 たとえば、次のコマンドを入力し、アカウントのパスワードを入力します。

        # sudo mkdir /datadrive


10. 次のコマンドを入力してドライブをマウントします。

        # sudo mount /dev/sdc1 /datadrive

    データ ディスクとしてを使用する準備ができました **/datadrive**します。


11. 新しいドライブを /etc/fstab に追加します。

    再起動後にドライブを自動的に再マウントするために、そのドライブを /etc/fstab ファイルに追加する必要があります。 また、ドライブを参照する際に、デバイス名 (つまり /dev/sdc1) だけではなく、UUID (汎用一意識別子) を /etc/fstab で使用することもお勧めします。 使用する新しいドライブの UUID を確認する、 **blkid** ユーティリティ。

        # sudo -i blkid

    次のような出力が表示されます。

        /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
        /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
        /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


    >[AZURE.NOTE] 不適切に編集、 **/etc/fstab** ファイルは、システムが起動できなくなる可能性があります。 編集方法がはっきりわからない場合は、このファイルを適切に編集する方法について、ディストリビューションのドキュメントを参照してください。 編集する前に、/etc/fstab ファイルのバックアップを作成することもお勧めします。

    次に、開く、 **/etc/fstab** テキスト エディターでファイルです。 /etc/fstab is はシステム ファイルです。したがってこのファイルを編集するには `sudo` を使用する必要があります。たとえば、次のようになります。

        # sudo vi /etc/fstab

    新しいに対して UUID 値を使用してこの例では **/dev/sdc1** 前の手順で、マウント ポイントとして作成したデバイス **/datadrive**します。 末尾に次の行を追加、 **/etc/fstab** ファイル。

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2

    または、SUSE Linux に基づいたシステムでは、わずかに異なる形式を使用する必要がある場合があります。

        /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults   1   2

    テストするだけのマウントを解除して再マウントし、ファイル システム、つまり、ファイル システムが正しくマウントされるようになりました 例を使用してマウント ポイント `/datadrive` 前の手順で作成します。

        # sudo umount /datadrive
        # sudo mount /datadrive

    `mount` コマンドでエラーが発生した場合、/etc/fstab ファイルの構文が正しいかどうかを確認してください。 追加のデータ ドライブやパーティションを作成する場合、それらも /etc/fstab に個別に入力する必要があります。

    これらのコマンドを使用して、ドライブを書き込み可能にする必要があります。
        # cd/datadrive
        # sudo chmod 移動 + w/datadrive

>[AZURE.NOTE] 後、fstab を編集することがなく、データ ディスクを削除すると、VM が起動に失敗する可能性があります。 これが頻繁に発生する場合、大部分のディストリビューションでは `nofail` または `nobootwait` fstab オプションが提供されます。これによって、起動時にディスクのマウントが失敗してもシステムを起動することができます。 これらのパラメーターの詳細については、使用しているディストリビューションのドキュメントを参照してください。

## その他のリソース
[Linux を実行する仮想マシンにログオンする方法][ログオン]

[Linux 仮想マシンからディスクを切断する方法 ](virtual-machines-linux-how-to-detach-disk.md)

[サービス管理 API での Azure CLI の使用](virtual-machines-command-line-tools.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-how-to-log-on.md

