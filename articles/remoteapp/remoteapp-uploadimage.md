
<properties
    pageTitle="Azure RemoteApp のカスタム イメージをアップロードする | Microsoft Azure"
    description="Azure RemoteApp のカスタム イメージをアップロードする方法について説明します。"
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/05/2015"
    ms.author="ericor" />



# Azure RemoteApp のカスタム イメージをアップロードする

カスタム テンプレート イメージの作成、または変更と更新ができたら、そのイメージを Azure RemoteApp イメージ ライブラリにアップロードする必要があります。 次の手順を使用します。


## 開始する前に

1.      Verify your custom image meets the [image requirements](remoteapp-imagereqs.md) and [application requirements](remoteapp-appreqs.md).
2.      Install the [Azure PowerShell module](../install-configure-powershell.md).

## カスタム イメージのアップロード方法のステップ バイ ステップ ガイド

1.      Open Azure Management Portal and navigate to the RemoteApp page.
2.      On the **Template images** tab, click **Upload** at the bottom of the page.
4.      Enter a friendly name for your image and specify the storage account location. Ensure the location is the same location as your RemoteApp collection or a location where you want to create one.
5.      When prompted, download the script to your local PC.
6.      Copy the command parameters in the text box to your clipboard.
7.      Open an elevated Windows PowerShell window.
8.      From the elevated Windows PowerShell window, navigate to the same directory where you downloaded the script.
9.      Paste the copied command and press **Enter**.

    アップロード プロセスが開始されます。所要時間は、ネットワークの速度やイメージのサイズなど、さまざまな要因によって異なります。

11.    ネットワーク接続の中断などの理由でアップロードが成功しなかった場合は、開始したアップロード プロセスをいつでも再開できます。 アップロードを再開するには、同じコマンド ラインを使用して、スクリプトを再度実行します。

> [AZURE.WARNING] アップロード スクリプトを変更できません。 アップロードするイメージがイメージの要件とアプリケーションの要件を満たしていることを確認するための特定のチェックが実装されています。

## 一般的な問題

- Azure PowerShell ではなく、Windows PowerShell を使用してください。 Azure PowerShell モジュールをインストールする必要があります。アップロード プロセス中に特定のモジュールが必要になるためです。
- スクリプトを変更しないでください。スクリプトには検証が含まれています。
- アップロード中に vhd ファイルがロックアウトされる場合は、ファイルを新しい場所にコピーまたは移動して、再度アップロードを実行してください。 一部の Windows プロセスがアップロードを阻んでいる可能性があります。  

