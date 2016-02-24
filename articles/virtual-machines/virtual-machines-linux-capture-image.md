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

下にあるイメージを格納する azure **イメージ**します。 アップロードしたすべてのイメージがこの場所に格納されます。 イメージの詳細については、次を参照してください。 [Azure の仮想マシン イメージについて] []します。

## 開始する前に

これらの手順は、すでにクラシック デプロイ モデルを使用して Azure 仮想マシンを作成し、データ ディスクの接続を含め、オペレーティング システムの構成が完了していることを前提としています。 まだ完了していない場合は、下記の手順を確認してください。

- [How to Create a Virtual Machine Running Linux (Linux を実行する仮想マシンの作成方法)] []


## 仮想マシンをキャプチャする

1. 任意の SSH クライアントを使用して、仮想マシンに接続します。 詳細については、「 [を実行している Linux の仮想マシンにログオンする方法] []します。

2. SSH のウィンドウで、次のコマンドを入力します。  `waagent` からの出力はこのユーティリティのバージョンによって多少異なる場合があることに注意してください。

    `sudo waagent -deprovision`

    このコマンドは、システムをクリーンアップし、再プロビジョニングに適した状態にします。 この操作では次のタスクが実行されます。

    - すべての SSH ホスト キーの削除 (構成ファイルで Provisioning.RegenerateSshHostKeyPair が 'y' の場合)
    - /etc/resolv.conf 内のネームサーバー構成の消去
    - /etc/shadow の `root` ユーザーのパスワードの削除 (構成ファイルで Provisioning.DeleteRootPassword が 'y' の場合)
    - キャッシュされた DHCP クライアントのリースの削除
    - ホスト名を localhost.localdomain にリセット
    - 削除前回がプロビジョニングされたユーザー アカウント (/var/lib/waagent から取得) **および関連データ**します。

    >[AZURE.NOTE] イメージを「一般化」する目的でファイルを削除してデータをプロビジョニング解除されます。 このコマンドは、必ず新しいイメージ テンプレートとしてキャプチャする仮想マシン上で実行してください。 プロビジョニング解除により、イメージからすべての機密情報が削除されることや、イメージが第三者への再配布に適した状態になることが保証されるわけではありません。


3. 型 **y** を続行します。 `-force` パラメーターを追加すると、この確認手順を省略できます。

4. 型 **終了** SSH クライアントを閉じます。


    >[AZURE.NOTE] The next steps assume you have already [installed the Azure CLI](../xplat-cli-install.md) on your client computer. All the steps below can also be done in the [Azure classic portal] [].

5. クライアント コンピューターから Azure CLI を開き、Azure サブスクリプションにログインします。 詳細については、読み取り [Azure CLI から Azure サブスクリプションへの接続](../xplat-cli-connect.md)します。

6. サービス管理モードであることを確認します。

    `azure config mode asm`

7. 次のコマンドを実行して、上記の手順で既にプロビジョニングが解除された仮想マシンを終了します。

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] 使用して、サブスクリプション内で作成されたすべての仮想マシンを調べることができます。 `azure vm list`

8. 仮想マシンが停止したら、次のコマンドを実行してイメージをキャプチャします。

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    代わりにイメージ名を入力 _新しいイメージ名_します。 このコマンドでは、一般化された OS イメージが作成されます。 `-t` サブコマンドによって、元の仮想マシンが削除されます。

9.  新しいイメージが、新しい仮想マシンの構成に使用できるイメージの一覧に表示されるようになりました。 次のコマンドを実行すると、新しいイメージを表示できます。

    `azure vm image list`

     [Azure クラシック ポータル] [], に表示される、 **イメージ** ] ボックスの一覧です。

    ![イメージのキャプチャの成功](./media/virtual-machines-linux-capture-image/VMCapturedImageAvailable.png)


## 次のステップ
イメージを使用して仮想マシンを作成する準備ができました。 Azure CLI コマンド `azure vm create` を使用して、先ほど作成したイメージの名前を指定することができます。 参照してください [Mac、Linux、および Azure サービス管理で Windows 用 Azure CLI の使用](virtual-machines-command-line-tools.md) コマンドに関する詳細。 また、使用して、 [Azure クラシック ポータル] [] を使用してカスタムの仮想マシンを作成する、 **ギャラリーから** メソッドを作成したイメージを選択します。 参照してください [カスタム仮想マシンを作成する方法] [] 詳細です。

**関連項目:** [Azure Linux エージェント ユーザー ガイド](virtual-machines-linux-agent-user-guide.md)

[Azure classic portal]: http://manage.windowsazure.com
[How to Log on to a Virtual Machine Running Linux]: virtual-machines-linux-how-to-log-on.md
[About Virtual Machine Images in Azure]: virtual-machines-images.md
[How to Create a Custom Virtual Machine]: virtual-machines-linux-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: storage-windows-attach-disk.md
[How to Create a Virtual Machine Running Linux]: virtual-machines-linux-tutorial.md

