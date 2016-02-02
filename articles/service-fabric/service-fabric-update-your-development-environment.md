<properties
   pageTitle="Service Fabric 開発環境の更新 | Microsoft Azure"
   description="Service Fabric 開発環境を更新して、最新のランタイム、SDK、およびツールを使用します。"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/30/2015"
   ms.author="seanmck"/>


# Service Fabric 開発環境の更新

 Service Fabric は定期的に、ローカルでの開発に使用するランタイム、SDK、ツールの新しいリリースを提供します。 これらのリリースでローカル開発環境を常に更新しておくと、アプリケーションをローカルで構築してテストする際に、いつでも最新の機能とバグの修正を利用し、パフォーマンスを高めることができます。

## ローカル クラスターのクリーンアップ

 現在 Service Fabric では、ローカル クラスターの実行中には、Service Fabric ランタイムのアップグレードをサポートしていません。 クリーンなアップグレードを実行するには、まず、ローカル クラスターをクリーンアップすることが重要です。
 > [AZURE.NOTE] ローカル クラスターをクリーンアップすると、デプロイ済みのすべてのアプリケーションとそのデータが削除されます。

 次のように、ローカル クラスターをクリーンアップできます。


 1. その他のすべての PowerShell ウィンドウを閉じて、管理者として新しいウィンドウを起動します。

 2. 使用してクラスター セットアップ ディレクトリに移動 `cd"$env:path: ProgramW6432\Microsoft sdks \service Fabric \clustersetup"`

 3. 実行 `.\CleanCluster.ps1`


## ランタイム、SDK、およびツールの更新

 既存のクラスターを正常にクリーンアップした後に、次のようにアップグレードを続行できます。


 1. Web Platform Installer を起動 [[1] の新しいリリースに更新][1]します。

 2. 完了すると、新しい PowerShell ウィンドウを管理者として起動し、使用してクラスター セットアップ ディレクトリに移動 `cd"$env:path: \microsoft SDKs\ServiceFabric\ClusterSetup"`します。

 3. 実行 `.\DevClusterSetup.ps1` 、ローカル クラスターを設定します。

これで完了です。 Visual Studio を起動し、Service Fabric アプリケーションの構築を続行できます。
>[AZURE.NOTE] 既定のプロジェクトの構造は、このリリースで変更されました。 既存のプロジェクトは、Visual Studio で開いて実行することができます。 ただし、アプリケーションのビルド、配置、またはデバッグに問題が発生した場合は、新しいプロジェクトを作成して、コードを移行することを検討してください。


[1]: http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric "WebPI link"

