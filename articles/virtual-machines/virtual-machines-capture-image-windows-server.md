<properties
    pageTitle="Azure Windows VM のイメージをキャプチャする | Microsoft Azure"
    description="クラシック デプロイ モデルを使用して作成された Azure Windows 仮想マシンのイメージをキャプチャする"
    services="virtual-machines"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/05/2015"
    ms.author="cynthn"/>

#クラシック デプロイ モデルを使用して作成された Azure Windows 仮想マシンのイメージをキャプチャする

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


ここでは、Windows を実行する Azure 仮想マシンをキャプチャして、他の仮想マシンを作成する際にイメージとして使用する方法を示します。 このイメージには、オペレーティング システム ディスクと、仮想マシンに接続されているすべてのデータ ディスクが含まれます。 ネットワーク構成は含まれないため、イメージを使用する他の仮想マシンを作成するときは、ネットワーク構成を行う必要があります。

下にあるイメージを格納する azure **マイ イメージ**します。 この場所は、アップロードしたイメージが格納される場所と同じです。 イメージの詳細については、「 [仮想マシンのイメージについて](virtual-machines-images.md)します。

##開始する前に##

これらの手順は、すでに Azure 仮想マシンを作成し、データ ディスクの接続を含め、オペレーティング システムの構成が完了していることを前提としています。 まだ完了していない場合は、下記の手順を確認してください。

- [イメージから仮想マシンを作成](virtual-machines-create-custom.md)
- [データ ディスクを仮想マシンに接続する方法](storage-windows-attach-disk.md)

> [AZURE.WARNING] このプロセスは、キャプチャされた後に、元の仮想マシンを削除します。 

これは仮想マシンのバックアップする方法としてではありません。 バックアップ方法の 1 つに Azure Backup があります。これは、特定の地域でプレビューとして提供されています。 詳細については、「 [Azure の仮想マシンをバックアップ](../backup/backup-azure-vms.md)します。 その他のソリューションは認定パートナーから利用できます。 利用可能なソリューションについては、Azure Marketplace を検索して確認してください。


##仮想マシンをキャプチャする

1.  [Azure クラシック ポータル](http://manage.windowsazure.com), 、**接続** 仮想マシンにします。 手順については、次を参照してください。 [Windows Server を実行する仮想マシンへのサインイン方法] []します。

2.  管理者としてコマンド プロンプト ウィンドウを開きます。

3.  ディレクトリを `%windir%\system32\sysprep` に変更し、sysprep.exe を実行します。

4.   **システム準備ツール** ] ダイアログ ボックスが表示されます。 以下の手順を実行します。

    -  **システム クリーンアップ アクション**, を選択 **[システムの既定の Experience (OOBE)** ことを確認し、 **Generalize** がオンになっています。 詳細については、Sysprep を使用して、次を参照してください。 [sysprep の使用方法: An Introduction][]します。

    -  **シャット ダウン オプション**, [ **シャット ダウン**します。

    - Click **OK**.

    ![Sysprep を実行する](./media/virtual-machines-capture-image-windows-server/SysprepGeneral.png)

7.  Azure クラシック ポータルで仮想マシンの状態を変更する仮想マシンをシャット ダウン Sysprep **Stopped**します。

8.  Azure クラシック ポータルで、クリックして **仮想マシン** をキャプチャする仮想マシンを選択します。

9.  コマンド バー] をクリックして **キャプチャ**します。

    ![仮想マシンをキャプチャする](./media/virtual-machines-capture-image-windows-server/CaptureVM.png)

     **仮想マシンをキャプチャ** ] ダイアログ ボックスが表示されます。

10.  **イメージ名**, 、新しいイメージの名前を入力します。

11. 一連のカスタム イメージを Windows Server のイメージを追加する前に、前の手順で指示されたとおりに Sysprep を実行して一般化する必要があります。 をクリックして **仮想マシンで Sysprep を実行しました** をこのようにしたを示します。

12. チェック マークをクリックしてイメージをキャプチャします。 新しいイメージは、「 **イメージ**します。

    ![イメージのキャプチャの成功](./media/virtual-machines-capture-image-windows-server/VMCapturedImageAvailable.png)

##次のステップ

イメージを使用して仮想マシンを作成する準備ができました。 使用して仮想マシンを作成するには、 **ギャラリーから** メニュー項目を作成したイメージを選択します。 手順については、次を参照してください。 [イメージから仮想マシンを作成](virtual-machines-create-custom.md)します。



[How to sign in to a virtual machine running Windows Server]: virtual-machines-log-on-windows-server.md
[How to Use Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-capture-image-windows-server/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-capture-image-windows-server/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png

