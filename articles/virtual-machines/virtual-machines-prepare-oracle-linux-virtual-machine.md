<properties 
pageTitle="Azure 用の Oracle Linux 仮想マシンの準備 | Microsoft Azure" 
description="Microsoft Azure で Linux を実行する Oracle 仮想マシンの構成手順を説明します。" 
services="virtual-machines" 
authors="bbenz" 
documentationCenter="virtual-machines"
tags="azure-service-management,azure-resource-manager"
/>

<tags 
ms.service="virtual-machines" 
ms.devlang="na" 
ms.topic="article" 
ms.tgt_pltfrm="vm-linux" 
ms.workload="infrastructure-services" 
ms.date="06/22/2015" 
ms.author="bbenz" />

#Azure 用の Oracle Linux 仮想マシンの準備

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


-   [Azure 用の Oracle Linux 6.4 以上の仮想マシンの準備](virtual-machines-linux-create-upload-vhd-oracle.md)

-   [Azure 用の Oracle Linux 7.0 以上の仮想マシンの準備](virtual-machines-linux-create-upload-vhd-oracle.md)

##前提条件
この記事では、既に Oracle Linux オペレーティング システムを仮想ハード ディスクにインストールしていることを前提にしています。 .vhd ファイルを作成するツールは、Hyper-V のような仮想化ソリューションなど複数あります。 手順については、次を参照してください。 [インストールの Hyper-v 仮想マシンの作成と](http://technet.microsoft.com/library/hh846766.aspx)です。

**Oracle Linux のインストールに関する注記**

- Oracle の Red Hat 互換カーネルとその UEK3 (Unbreakable Enterprise Kernel) は、両方とも Hyper-V と Azure でサポートされています。 最良の結果を得るために、Oracle Linux VHD を準備する際に、最新のカーネルに更新してください。

- Oracle の UEK2 は必要なドライバーを含んでいないため、Hyper-V と Azure ではサポートされていません。

- 新しい VHDX 形式は、Azure ではサポートされていません。 Hyper-V マネージャーまたは convert-vhd コマンドレットを使用して、ディスクを VHD 形式に変換できます。

- Linux システムをインストールする場合は、LVM (通常、多くのインストールで既定) ではなく標準パーティションを使用することをお勧めします。 これにより、特に OS ディスクをトラブルシューティングのために別の VM に接続する必要がある場合に、LVM 名と複製された VM の競合が回避されます。 LVM または [RAID](virtual-machines-linux-configure-raid.md) 優先する場合にデータ ディスク上で使用される可能性があります。

- さらに大きいサイズの VM では NUMA はサポートされていません。2.6.37 以下のバージョンの Linux カーネルにバグがあるためです。 この問題は、主に、アップストリームの Red Hat 2.6.32 カーネルを使用したディストリビューションに影響します。 Azure Linux エージェント (waagent) を手動でインストールすると、Linux カーネルの GRUB 構成で NUMA が自動的に無効になります。 このことに関する詳細については、次の手順を参照してください。

- OS ディスクにスワップ パーティションを構成しないでください。 Linux エージェントは、一時的なリソース ディスク上にスワップ ファイルを作成するよう構成できます。 このことに関する詳細については、次の手順を参照してください。

- すべての VHD のサイズは 1 MB の倍数であることが必要です。

##Oracle Linux 6.4+
Azure 上で実行する仮想マシンのオペレーティング システムで固有の構成手順を完了する必要があります。

1. Hyper-V マネージャーの中央のウィンドウで仮想マシンを選択します。

2. クリックして **接続** を仮想マシンのウィンドウを開きます。

3. 次のコマンドを実行して NetworkManager をアンインストールします。

        # sudo rpm -e --nodeps NetworkManager

    >[AZURE.NOTE]パッケージがまだインストールされていない場合、このコマンドは失敗してエラー メッセージが表示されます。 これは予期されることです。

4. という名前のファイルを作成する **ネットワーク** /etc/etc/sysconfig/ディレクトリに、次のテキストが含まれています。

    `NETWORKING=yes`  
    `HOSTNAME=localhost.localdomain`

5.  という名前のファイルを作成 **ifcfg-eth0** /etc/sysconfig/network-scripts/ディレクトリに、次のテキストが含まれています。

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
            TYPE=Ethernet
            USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

6.  udev ルールを移動 (または削除) して、イーサネット インターフェイスの静的ルールが生成されないようにします。 これらのルールは、Azure または Hyper-V で仮想マシンを複製する際に問題の原因となります。

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2\>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2\>/dev/null

7.  次のコマンドを実行して、起動時にネットワーク サービスが開始されるようにします。

        # chkconfig network on

8.  次のコマンドを実行して python-pyasn1 をインストールします。

        # sudo yum install python-pyasn1

9.  GRUB 構成でカーネルのブート行を変更して Azure の追加のカーネル パラメーターを含めます。 これを行うには、テキスト エディターで "/boot/grub/menu.lst" を開き、既定のカーネルに次のパラメーターが含まれていることを確認します。

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    これにより、すべてのコンソール メッセージが最初のシリアル ポートに送信され、メッセージを Azure での問題のデバッグに利用できるようになります。 これにより、Oracle の Red Hat 互換カーネルのバグが原因で NUMA が無効になります。

    そのほかにもことをお勧めする *削除* 、次のパラメーター。

        rhgb quiet crashkernel=auto

    クラウド環境では、すべてのログをシリアル ポートに送信するため、グラフィカル ブートおよびクワイエット ブートは役立ちません。

    必要に応じて `crashkernel` オプションは構成したままにすることができますが、このパラメーターにより、VM 内の使用可能なメモリ量が 128 MB 以上減少します。 このため、比較的小さなサイズの VM では問題となる可能性がある点に注意してください。

10.  SSH サーバーがインストールされており、起動時に開始するように構成されていることを確認します。 通常これが既定です。

11.  次のコマンドを実行して Azure Linux エージェントをインストールします。

        # sudo yum インストール WALinuxAgent

    Note that installing the WALinuxAgent package will remove the NetworkManager and NetworkManager-gnome packages if they were not already removed as described in step 2.

12.  OS ディスクにスワップ領域を作成しないでください。

    The Azure Linux Agent can automatically configure swap space by using the local resource disk that is attached to the VM after provisioning on Azure. Note that the local resource disk is a *temporary* disk, and it might be emptied when the VM is deprovisioned. After you install the Azure Linux Agent (see previous step), modify the following parameters in /etc/waagent.conf appropriately:

        ResourceDisk.Format=y

        ResourceDisk.Filesystem=ext4

        ResourceDisk.MountPoint=/mnt/resource

        ResourceDisk.EnableSwap=y

        ResourceDisk.SwapSizeMB=2048 ## NOTE: set this to whatever you need it to be.

13.  次のコマンドを実行して仮想マシンをプロビジョニング解除し、Azure でのプロビジョニング用に準備します。

        # sudo waagent-強制的 - プロビジョニング解除
        # エクスポート HISTSIZE = 0
        # ログアウト

14.  クリックして **アクション-\ > シャット ダウン** Hyper V マネージャーでします。 これで、Linux VHD を Azure にアップロードする準備が整いました。

##Oracle Linux 7.0 以上
**Oracle Linux 7 での変更**

Azure 用の Oracle Linux 7 仮想マシンを準備する手順は、Oracle Linux 6 の場合とほとんど同じです。 ただし、次のように、重要な違いがいくつかあります。

-   Red Hat 互換カーネルと Oracle の UEK3 は、両方とも Azure でサポートされています。 UEK3 カーネルをお勧めします。

-   NetworkManager パッケージが、Azure Linux エージェントと競合しなくなりました。 このパッケージは既定でインストールされます。このパッケージを削除しないことをお勧めします。

-   GRUB2 が、既定のブートローダーとして使用されるようになったため、カーネル パラメーターの編集手順が変更されました (以下を参照)。

-   XFS が既定のファイル システムになりました。 必要に応じて、引き続き ext4 ファイル システムを使用できます。

**構成の手順**

1.  Hyper-V マネージャーで仮想マシンを選択します。

2.  クリックして **接続** を仮想マシンのコンソール ウィンドウを開きます。

3.  という名前のファイルを作成する **ネットワーク** /etc/etc/sysconfig/ディレクトリに、次のテキストが含まれています。

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  という名前のファイルを作成 **ifcfg-eth0** /etc/sysconfig/network-scripts/ディレクトリに、次のテキストが含まれています。

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
            PEERDNS=yes
        IPV6INIT=no

5.  udev ルールを移動 (または削除) して、イーサネット インターフェイスの静的ルールが生成されないようにします。 これらのルールは、Microsoft Azure または Hyper-V で仮想マシンを複製する際に問題の原因となります。

        # sudo mkdir -m 0700 /var/lib/waagent
        # sudo mv /lib/udev/rules.d/75-persistent-net-generator.rules /var/lib/waagent/ 2>/dev/null
        # sudo mv /etc/udev/rules.d/70-persistent-net.rules /var/lib/waagent/ 2>/dev/null

6.  次のコマンドを実行して、起動時にネットワーク サービスが開始されるようにします。

        # sudo chkconfig network on

7.  次のコマンドを実行して python-pyasn1 パッケージをインストールします。

        # sudo yum install python-pyasn1

8.  次のコマンドを実行して、現在の yum メタデータをクリアし、更新をインストールします。

        # sudo yum clean all
        # sudo yum -y update

9.  GRUB 構成でカーネルのブート行を変更して Azure の追加のカーネル パラメーターを含めます。 開いている「//etc/default/grub」のテキスト エディターと例については、GRUB\_CMDLINE\_LINUX パラメーターの編集。

        GRUB\_CMDLINE\_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"

    これにより、すべてのコンソール メッセージが最初のシリアル ポートに送信され、メッセージを Azure での問題のデバッグに利用できるようになります。 そのほかにもことをお勧めする *削除* 、次のパラメーター。

        rhgb quiet crashkernel=auto

    クラウド環境では、すべてのログをシリアル ポートに送信するため、グラフィカル ブートおよびクワイエット ブートは役立ちません。

    必要に応じて `crashkernel` オプションは構成したままにすることができますが、このパラメーターにより、VM 内の使用可能なメモリ量が 128 MB 以上減少します。 このため、比較的小さなサイズの VM では問題となる可能性がある点に注意してください。

10.  "/etc/default/grub" の編集を終了したら、次のコマンドを実行して GRUB 構成をリビルドします。

        # sudo grub2 mkconfig-o/boot/grub2/grub.cfg

11.  SSH サーバーがインストールされており、起動時に開始するように構成されていることを確認します。 通常これが既定です。

12.  次のコマンドを実行して Azure Linux エージェントをインストールします。

        # sudo yum インストール WALinuxAgent

13.  OS ディスクにスワップ領域を作成しないでください。

    The Azure Linux Agent can automatically configure swap space by using the local resource disk that is attached to the VM after provisioning on Azure. Note that the local resource disk is a *temporary* disk, and it might be emptied when the VM is deprovisioned. After you install the Azure Linux Agent (see previous step), modify the following parameters in /etc/waagent.conf appropriately:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048 ## NOTE: Set this to whatever you need it to be.

14.  次のコマンドを実行して仮想マシンをプロビジョニング解除し、Azure でのプロビジョニング用に準備します。

        # sudo waagent-強制的 - プロビジョニング解除
        # エクスポート HISTSIZE = 0
        # ログアウト

15.  クリックして **アクション-\ > シャット ダウン** Hyper V マネージャーでします。 これで、Linux VHD を Azure にアップロードする準備が整いました。

