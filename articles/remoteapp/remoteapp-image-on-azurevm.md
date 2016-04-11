<properties 
    pageTitle="Azure VM に基づく Azure RemoteApp イメージの作成 | Microsoft Azure"
    description="Azure 仮想マシンを使用することで Azure RemoteApp のイメージを作成する方法について説明します。" 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2015" 
    ms.author="elizapo" />



# Azure 仮想マシンに基づく Azure RemoteApp イメージの作成

Azure 仮想マシンから (コレクションで共有するアプリを保持する) Azure RemoteApp のイメージを作成できます。 また、Azure VM イメージ ギャラリーに追加された、Azure RemoteApp イメージのすべての要件を満たす仮想マシン イメージを使用することもできます。必要に応じて、この VM イメージを独自の VM の開始点として使用することができます。 ライブラリにある "Windows Server Remote Desktop Session Host" イメージを探してみましょう。

Azure VM に基づく独自のイメージを作成するには、イメージの作成と、Azure VM ライブラリから Azure RemoteApp へのイメージのアップロードという 2 つの手順があります。

## Azure VM に基づくカスタム イメージの作成

次の手順を使用して、Azure VM に基づくイメージを作成します。

1. Azure 仮想マシンを作成します。 Azure 仮想マシン イメージ ギャラリーの “Windows Server Remote Desktop Session Host” イメージまたは "Windows Server Remote Desktop Session Host with Microsoft Office 365 ProPlus" イメージを使用できます。 このイメージは、Azure RemoteApp テンプレート イメージのすべての要件を満たしています。 

    詳細については、「 [Windows を実行する VM の作成](virtual-machines-windows-tutorial.md)します。

2. VM に接続し、RemoteApp を使用して共有するアプリをインストールして構成します。 アプリに必要な追加の Windows の構成も実行してください。 

    詳細については、「 [Windows Server を実行している仮想マシンにログオンする方法](virtual-machines-log-on-windows-server.md)します。 

3. いずれかの Windows Server リモート デスクトップ セッション ホストのイメージを使用している場合は、VM が RemoteApp の前提条件を満たしていることを確認するための検証スクリプトが含まれています。 スクリプトを実行するにはダブルクリック **ValidateRemoteAppImage** デスクトップにします。 次の手順に進む前に、スクリプトによって報告されたエラーはすべて修正します。

4. SYSPREP は、イメージを一般化してキャプチャします。 参照してください [をテンプレートとして使用する Windows 仮想マシンをキャプチャする方法](../virtual-machines-capture-image-windows-server.md) についてです。

 

## Azure RemoteApp イメージ ライブラリへのイメージのインポート

次の手順を使用して、新しいイメージを Azure RemoteApp にインポートします。

1.  **テンプレート イメージ** ] タブをクリックします。
    - 既存のイメージがない、クリックして **アップロードまたはテンプレート イメージをインポート**します。 
    - 少なくとも 1 つのイメージが既にがある場合] をクリックして **+** 新しいイメージを追加します。

2. 選択 **、仮想マシンからイメージをインポート** ライブラリ、およびクリック **次**します。

3. 次のページで、一覧からカスタム イメージを選択し、イメージを作成したときに表示された手順に従っていることを確認します。 クリックして **次**します。
4. 新しい RemoteApp イメージの名前を入力して場所を選択したら、インポート プロセスを開始するチェックマークをクリックします。

> [AZURE.NOTE] Azure RemoteApp でサポートされている Azure の場所に Azure 仮想マシンがサポートされている Azure の場所からイメージをインポートすることができます。 インポート時間は場所によって異なり、最大 25 分かかります。

か、新しいコレクションを作成する準備が整いました、 [クラウド](remoteapp-create-cloud-deployment.md) コレクションまたは [ハイブリッド](remoteapp-create-hybrid-deployment.md), ニーズによって異なります。
 
