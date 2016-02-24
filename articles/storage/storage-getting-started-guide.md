<properties 
    pageTitle="Azure Storage を 5 分で使い始める | Microsoft Azure" 
    description="Azure QuickStarts、Visual Studio、Azure ストレージ エミュレーターを使用して、Microsoft Azure の BLOB、テーブル、キューをすばやく導入する方法について説明します。 初めての Azure Storage アプリケーションを 5 分で実行します。" 
    services="storage" 
    documentationCenter=".net" 
    authors="tamram" 
    manager="adinah" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="11/12/2015" 
    ms.author="tamram;selcint"/>

# Azure Storage を 5 分で使い始める 

## 概要

Azure SDK には、Microsoft Visual Studio から Azure Storage を使用するためのクイック スタートのテンプレートが含まれています。 Visual Studio Online アカウントは 

Azure Storage の開発は簡単です。 このチュートリアルでは、Azure Storage アプリケーションを短時間で稼動させる方法について説明します。 
Azure Storage で簡単に準備できる 2 つのシナリオを紹介します。

コードに入る前に Azure Storage の詳細については、「場合 [次のステップ](#next-steps)します。

## 前提条件

開始する前に、以下の前提条件を確認します。

1. コンパイルして、アプリケーションをビルドのバージョンが必要があります [Visual Studio](https://www.visualstudio.com/) 、コンピューターにインストールします。 
2. 最新バージョンをインストール [Azure SDK for .NET](http://azure.microsoft.com/downloads/)します。 SDK には、Azure QuickStart サンプル プロジェクト、Azure ストレージ エミュレーターが含まれています。 および [.NET 用 Azure ストレージ クライアント ライブラリ](https://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)します。
3. あることを確認 [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) このチュートリアルで使用する Azure QuickStart サンプル プロジェクトに必要なコンピューターにインストールされています。 お使いのコンピューターにインストールされる .NET Framework のバージョンが不明な場合は、次を参照してください。 [方法: 決定 .NET Framework のバージョンがインストールされている](https://msdn.microsoft.com/vstudio/hh925568.aspx)します。 またはキーを押します、 **開始** ] ボタンをクリックするかを入力、Windows キー **コントロール パネルの [**します。 クリックして **プログラム** > **プログラムと機能**, と .NET Framework 4.5 がインストールされているプログラムの一覧に表示するかどうかを判断します。

最新バージョンの Azure ストレージ クライアント ライブラリのバイナリは [NuGet](https://www.nuget.org/packages/WindowsAzure.Storage/)します。


## Azure ストレージ エミュレーターで最初の Azure Storage アプリケーションをローカルに実行する

Azure ストレージを使用するアプリケーションを開発する際にに対して行うことができます、 [Azure ストレージ エミュレーター](storage-use-emulator.md)します。 ストレージ エミュレーターでは、Azure の BLOB、キュー、テーブル サービスを開発用にエミュレートするローカル環境が利用できます。 ストレージ エミュレーターを使用すると、Azure サブスクリプションまたはストレージ アカウントを作成せずにストレージ アプリケーションをローカルでテストでき、コストもかかりません。

それでは、Visual Studio で Azure Quick Starts サンプル プロジェクトの 1 つを使用する簡単な Azure Storage アプリケーションを作成しましょう。 このチュートリアルの重点、 **Azure Blob ストレージ**, 、**Azure テーブル ストレージ**, 、および **Azure キュー ストレージ** サンプル プロジェクト。

1. Visual Studio を起動します。
2.  **ファイル** ] メニューをクリックして **新しいプロジェクト**します。
3.  **新しいプロジェクト** ダイアログ ボックスで、をクリックして **インストールされている** > **テンプレート** > **Visual c#** > **クラウド** > **クイック スタート** > **Data Services**します。
    - 3.a.  次のテンプレートのいずれかを選択します。Azure BLOB Storage、Azure Table Storage、または Azure Storage Queue。 
    - 3.b. 確認して **.NET Framework 4.5** ターゲット フレームワークとして選択します。   
    - 3.c. プロジェクトの名前を指定し、次のように新しい Visual Studio ソリューションを作成します。
    
    ![Azure QuickStarts][Image1]

アプリケーションを実行する前に、ソース コードを確認できます。 コードを確認するには、選択 **ソリューション エクスプ ローラー** 上、 **ビュー** Visual Studio のメニュー。 その後、Program.cs ファイルをダブルクリックします。 

次に、Azure ストレージ エミュレーターでサンプル アプリケーションを実行します。

1.  キーを押して、 **開始** ] ボタンをクリックするかを検索、Windows キー *Azure ストレージ エミュレーター*, 、アプリケーションを起動します。
2.  Visual Studio で、[ **ソリューションのビルド** 上、 **ビルド** メニュー。 
3.   **デバッグ** ] メニューの [キーを押して **f11 キーを押します** ソリューションを段階を実行するか、[ **f5 キーを押して** ソリューションを最初から最後まで実行します。

## クラウドの Azure Storage で最初の Azure Storage アプリケーションを実行する

クラウドの Azure Storage に対して実行するには、Azure サブスクリプションとストレージ アカウントが必要です (既に持っていない場合)。 

- Azure のサブスクリプションを取得するには、次を参照してください。 [無料評価版](http://azure.microsoft.com/pricing/free-trial/), 、[購入に関するオプション](http://azure.microsoft.com/pricing/purchase-options/), 、および [メンバー プラン](http://azure.microsoft.com/pricing/member-offers/) (MSDN、Microsoft Partner Network、BizSpark、および他の Microsoft プログラムのメンバー) 用です。
- Azure でストレージ アカウントを作成するを参照してください。 [を作成、管理、またはストレージ アカウントを削除する方法](storage-create-storage-account.md)します。

アカウントができたら、Visual Studio で Azure QuickStarts サンプル プロジェクトの 1 つを使用する簡単な Azure Storage アプリケーションを作成できます。 このチュートリアルの重点 **Azure Blob ストレージ**, 、**Azure テーブル ストレージ**, 、および **Azure キュー ストレージ** サンプル プロジェクト。

1. Visual Studio を起動します。
2.  **ファイル** ] メニューをクリックして **新しいプロジェクト**します。
3.  **新しいプロジェクト** ダイアログ ボックスで、をクリックして **インストールされている** > **テンプレート** > **Visual c#** > **クラウド** > **クイック スタート** > **Data Services**します。
    - 3.a. 次のテンプレートのいずれかを選択します。Azure BLOB Storage、Azure Table Storage、または Azure Storage Queue。
    - 3.b. 確認して **.NET Framework 4.5** ターゲット フレームワークとして選択します。
    - 3.c. プロジェクトの名前を指定し、新しい Visual Studio ソリューションを作成します。 

アプリケーションを実行する前に、ソース コードを確認できます。 コードを確認するには、選択 **ソリューション エクスプ ローラー** 上、 **ビュー** Visual Studio のメニュー。 その後、Program.cs ファイルをダブルクリックします。 

次に、サンプル アプリケーションを実行します。

1.  Visual Studio で、次のように選択します。 **ソリューション エクスプ ローラー** 上、 **ビュー** メニュー。 App.config ファイルをダブルクリックし、次の Azure SDK ストレージ エミュレーターの接続文字列をコメント アウトします。

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Azure Storage サービスの接続文字列をコメント解除し、次のようにストレージ アカウント名とアクセス キーを App.config ファイルに入力します。
    `<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    ストレージ アカウント名とアクセス キーを検索するには、次を参照してください。 [ストレージ アカウントとは](storage-whatis-account.md)です。

3.  App.config ファイルでストレージ アカウント名とアクセス キーを入力した後、 **ファイル** ] メニューのをクリックして **すべて保存** すべてのプロジェクト ファイルを保存します。
4.   **ビルド** ] メニューのをクリックして **ソリューションのビルド**します。
5.   **デバッグ** ] メニューの [キーを押して **F11** ソリューション ステップ バイ ステップの実行またはキーを押します **f5 キーを押して** ソリューションを実行します。


## 次のステップ

Azure Storage の詳細については、次のリソースをご覧ください。

* [Microsoft Azure Storage の概要](storage-introduction.md)
* [How to use Blob Storage from .NET (.NET から BLOB ストレージを使用する方法)](storage-dotnet-how-to-use-blobs.md)
* [How to use Table Storage from .NET (.NET からテーブル ストレージを使用する方法)](storage-dotnet-how-to-use-tables.md)
* [How to use Queue Storage from .NET (.NET からキュー ストレージを使用する方法)](storage-dotnet-how-to-use-queues.md)
* [AzCopy コマンド ライン ユーティリティを使ったデータの転送](storage-use-azcopy)
* [Azure ストレージのドキュメント](http://azure.microsoft.com/documentation/services/storage/)
* [Azure Storage Client Library](https://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
* [Azure Storage REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
 

