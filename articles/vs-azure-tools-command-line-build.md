<properties
   pageTitle="Azure のコマンド ライン ビルド"
   description="Azure のコマンド ライン ビルド"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2015"
   ms.author="tarcher" />

# Azure のコマンド ライン ビルド

## 概要

コマンド プロンプトで MSBuild を実行すると、Azure のデプロイ用パッケージを作成できます。 一部のビルド プロセスを自動化するだけでなく、デバッグ、ステージング、および運用向けのビルドを構成、定義できます。


## Microsoft Build Engine (MSBuild)

Microsoft Build Engine (MSBuild) を使用すると、Visual Studio がインストールされていないビルド ラボ環境で製品のビルドを構築できます。 MSBuild は、拡張可能で Microsoft が完全にサポートしている XML 形式をプロジェクト ファイルに使用します。 このファイル形式では、1 つまたは複数のプラットフォームや構成でビルドが必要な項目を記述できます。

MSBuild はコマンド プロンプトで実行することもできます。このトピックでは、その方法について説明します。 コマンド プロンプトでプロパティを設定すると、プロジェクトの特定の構成を作成できます。 同様に、MSBuild コマンドをビルドするターゲットを定義することもできます。 コマンド ライン パラメーターおよび MSBuild の詳細については、次を参照してください。 [MSBuild コマンド ライン リファレンス](https://msdn.microsoft.com/library/ms164311.aspx)します。

## インストール

MSBuild を使用して Azure パッケージを作成するには、ソフトウェアおよびツールをビルド サーバーにインストールする必要があります。次の手順で説明します。

1. .NET Framework 4 以降をインストールします。この中に MSBuild が含まれています。

1. インストール、 [Azure Authoring Tools](http://go.microsoft.com/fwlink/?LinkId=394615) (microsoftazureauthoringtools-x64.msi または microsoftazureauthoringtools-x86.msi を探します。

1. インストール、 [Azure Libraries for .NET](http://go.microsoft.com/fwlink/?LinkId=394616) (microsoftazurelibsfornet-x64.msi または Microsoftazurelibs-x86.msi を検索を探します。

1. 別のコンピューターの Visual Studio がインストールされている場所にある Microsoft.WebApplication.targets ファイルをコピーします。

    このファイルは、C:\Program Files (x86)\MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (Visual Studio 2012 の場合は v11.0 ディレクトリ) に含まれています。このファイルをビルド サーバーの同じディレクトリにコピーする必要があります。

1. インストール、 [Azure Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616)します。

    Visual Studio 2013 プロジェクトをビルドするには、WindowsAzureTools.vs120.exe を探します。

## MSBuild パラメーター

パッケージを作成する最も簡単な方法は、`/t:Publish` オプションを指定して MSBuild を実行することです。 既定では、このコマンドは、ProjectDir\bin\Configuration\app.publish\ など、プロジェクトのルート フォルダーに関連するディレクトリを作成します。 Azure プロジェクトをビルドすると、パッケージ ファイルとそれに対応する構成ファイルの 2 つのファイルが生成されます。

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

既定では、各 Azure プロジェクトにはローカル (デバッグ) ビルド用に 1 つ、クラウド (ステージングまたは運用) ビルド用に 1 つのサービス構成ファイルが含まれていますが、必要に応じてサービス構成ファイルを追加または削除できます。 Visual Studio 内でパッケージをビルドするときは、パッケージに含めるサービス構成ファイルを指定するよう求められます。 MSBuild を使用してパッケージをビルドする場合、既定でローカル サービス構成ファイルが含められます。 別のサービス構成ファイルを含めるには、MSBuild コマンドの `TargetProfile` プロパティ を設定します (`MSBuild /t:Publish /p:TargetProfile=ProfileName`)。

パッケージと構成ファイルの格納に別のディレクトリを使用する場合は、`/p:PublishDir=Directory\` オプションを使用してパスを設定します。その際、末尾に円記号の区切り記号を含めます。

## デプロイ

パッケージは、ビルド後に Azure にデプロイできます。 このプロセスについて説明するチュートリアルについては、Azure の Web サイトを参照してください。 そのプロセスを自動化する方法については、次を参照してください。 [Azure でクラウド サービスの継続的な配信](../cloud-services/cloud-services-dotnet-continuous-delivery)します。

