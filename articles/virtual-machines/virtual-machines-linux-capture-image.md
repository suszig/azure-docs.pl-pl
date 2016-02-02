<properties
    pageTitle="Linux VM のイメージをキャプチャする | Microsoft Azure"
    description="クラシック デプロイ モデルで作成された Linux ベースの Azure の仮想マシン (VM) のイメージをキャプチャする方法について説明します。"
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
    ms.date="10/14/2015"
    ms.author="dkshir"/>



# 従来の Linux 仮想マシンをイメージとしてキャプチャする方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] [リソース マネージャー モデル](virtual-machines-linux-capture-image-resource-manager.md)します。


ここでは、Linux を実行する従来の Azure 仮想マシンをキャプチャして、他の仮想マシンを作成する際にイメージとして使用する方法を示します。 このイメージには、仮想マシンに接続された OS ディスクやデータ ディスクが含まれます。 ネットワーク構成は含まれないため、イメージから他の仮想マシンを作成するときは、ネットワーク構成を行う必要があります。

Azure では、イメージは **[イメージ]** に格納されます。 アップロードしたすべてのイメージがこの場所に格納されます。 イメージの詳細については、次を参照してください。 [[] で Azure 仮想マシン イメージについて][]します。

## 開始する前に

これらの手順は、すでにクラシック デプロイ モデルを使用して Azure 仮想マシンを作成し、データ ディスクの接続を含め、オペレーティング システムの構成が完了していることを前提としています。 まだ完了していない場合は、下記の手順を確認してください。

- [Linux を実行する仮想マシンを作成する方法][]


## 仮想マシンをキャプチャする

1. 任意の SSH クライアントを使用して、仮想マシンに接続します。 詳細については、「 [Linux の仮想マシンを実行している [] にログインする方法][]します。

2. SSH のウィンドウで、次のコマンドを入力します。 `waagent` からの出力はこのユーティリティのバージョンによって多少異なる場合があることに注意してください。

    `sudo waagent -deprovision`

    このコマンドは、システムをクリーンアップし、再プロビジョニングに適した状態にします。 この操作では次のタスクが実行されます。

    - すべての SSH ホスト キーの削除 (構成ファイルで Provisioning.RegenerateSshHostKeyPair が 'y' の場合)
    - /etc/resolv.conf 内のネームサーバー構成の消去
    - 削除、 `ルート` /etc/シャドウ (Provisioning.DeleteRootPassword が 'y'、構成ファイル内の場合) からユーザーのパスワード
    - キャッシュされた DHCP クライアントのリースの削除
    - ホスト名を localhost.localdomain にリセット
    - (/var/lib/waagent から取得した) 前回プロビジョニングされたユーザー アカウント**および関連データ**の削除
    >[AZURE.NOTE] プロビジョニング解除では、イメージを "一般化" する目的でファイルとデータが削除されます。 このコマンドは、必ず新しいイメージ テンプレートとしてキャプチャする仮想マシン上で実行してください。 プロビジョニング解除により、イメージからすべての機密情報が削除されることや、イメージが第三者への再配布に適した状態になることが保証されるわけではありません。

3. 「**y**」と入力して続行します。 `-force` パラメーターを追加すると、この確認手順を省略できます。

4. 「**Exit**」と入力して、SSH クライアントを閉じます。

    >[AZURE.NOTE] 次のステップでは、既にあると仮定 [Azure CLI をインストールして](../xplat-cli-install.md) 、クライアント コンピューターにします。 次の手順行うこともできます、 [Azure クラシック ポータルの [][]します。

5. クライアント コンピューターから Azure CLI を開き、Azure サブスクリプションにログインします。 詳細については、読み取り [Azure CLI から Azure サブスクリプションへの接続](../xplat-cli-connect.md)します。

6. サービス管理モードであることを確認します。

    `azure config mode asm`

7. 次のコマンドを実行して、上記の手順で既にプロビジョニングが解除された仮想マシンを終了します。

    `azure vm のシャット ダウン < your、仮想のマシンの名前 >`
    >[AZURE.NOTE] 使用して、サブスクリプション内で作成されたすべての仮想マシンを調べることができます `azure vm 一覧`

8. 仮想マシンが停止したら、次のコマンドを実行してイメージをキャプチャします。

    `azure vm のキャプチャ-t < your の仮想マシン-名 >< 新しいのイメージ名 >`

    _new-image-name_ を使用するイメージ名に置き換えて入力します。 このコマンドでは、一般化された OS イメージが作成されます。  `-T` サブコマンドは、元の仮想マシンを削除します。

9.  新しいイメージが、新しい仮想マシンの構成に使用できるイメージの一覧に表示されるようになりました。 次のコマンドを実行すると、新しいイメージを表示できます。

    `azure vm イメージ リスト`

    [Azure クラシック ポータル][], に表示される、 **イメージ** ] ボックスの一覧です。

    ![イメージのキャプチャの成功](./media/virtual-machines-linux-capture-image/VMCapturedImageAvailable.png)


## 次のステップ

イメージを使用して仮想マシンを作成する準備ができました。 Azure CLI コマンドを使用して `azure 仮想マシンの作成` して作成したイメージ名を入力します。 参照してください [Mac、Linux、および Azure サービス管理で Windows 用 Azure CLI の使用](virtual-machines-command-line-tools.md) コマンドに関する詳細。 また、使用して、 [Azure クラシック ポータル][] を使用してカスタムの仮想マシンを作成する、 **ギャラリーから** メソッドを作成したイメージを選択します。 参照してください [カスタム仮想マシンのを作成する方法][] 詳細です。

**関連項目:** [Azure Linux エージェント ユーザー ガイド](virtual-machines-linux-agent-user-guide.md)


[azure classic portal]: http://manage.windowsazure.com 
[how to log on to a virtual machine running linux]: virtual-machines-linux-how-to-log-on.md 
[about virtual machine images in azure]: virtual-machines-images.md 
[how to create a custom virtual machine]: virtual-machines-linux-create-custom.md 
[how to attach a data disk to a virtual machine]: storage-windows-attach-disk.md 
[how to create a virtual machine running linux]: virtual-machines-linux-tutorial.md 

