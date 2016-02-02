<properties
    pageTitle="Windows ストア アプリでの Azure ストレージの使用 | Microsoft Azure"
    description="Azure BLOB、キュー、テーブルを使用して Windows ストア アプリのデータを格納する方法を説明します。"
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="adinah" />
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/03/2015"
    ms.author="tamram"/>

# Windows ストア アプリでの Azure Storage の使用方法

## 概要

このガイドでは、Azure ストレージを利用する Windows ストア アプリケーションの開発に着手する方法を紹介します。

## 必要なツールのダウンロード

- [Visual Studio 2012](http://msdn.microsoft.com/library/windows/apps/br211384) ビルド、デバッグ、ローカライズ、パッケージ化、および Windows ストア アプリの配置を簡単になります。
- [Azure ストレージ クライアント ライブラリの Windows ランタイム](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/11/05/windows-azure-storage-client-library-for-windows-runtime.aspx) Azure ストレージを操作するためのクラス ライブラリを提供します。
- [WCF データ サービス ツール Windows ストア アプリ用](http://www.microsoft.com/download/details.aspx?id=30714) サービス参照の追加操作は、Visual Studio 2012 以降の Windows ストア アプリ用のクライアント側 OData サポート性を拡張します。

## アプリケーションの開発

### 開発の準備

Visual Studio 2012 以降で、新しい Windows ストア アプリケーション プロジェクトを作成します。

![store-apps-storage-vs-project][store-apps-storage-vs-project]

次に、Azure Storage クライアント ライブラリへの参照を追加します。そのためには、**[参照]** を右クリックし、**[参照の追加]** をクリックして、ダウンロードした Windows ランタイム用 Storage クライアント ライブラリに移動します。

![store-apps-storage-choose-library][store-apps-storage-choose-library]

### BLOB およびキュー サービスでのライブラリの使用

この時点で、アプリケーションは、Azure BLOB およびキュー サービスを呼び出すことができる状態になっています。 Azure ストレージの型を直接参照できるようにするために、次の **using** ステートメントを追加します。

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

次に、ページにボタンを追加します。 次のコードを追加、 **クリックして** イベントを使用して、イベント ハンドラー メソッドを変更し、 [async キーワード](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

このコードでは、2 種類の文字列変数、*accountName* および *accountKey* があると想定しています。 これらは、ストレージ アカウントの名前と、そのアカウントに関連付けられたアカウント キーを表します。

アプリケーションをビルドし、実行します。 ボタンをクリックすると、*container1* というコンテナーがアカウントに存在するかどうかがチェックされ、存在しない場合は作成されます。

### ライブラリとテーブル サービスの使用

Azure Table サービスとの通信に使用される型は、Windows ストア アプリケーション ライブラリ用の WCF Data Services に依存します。 ここでは、Package Manager Console を使用して、必要な WCF ライブラリへの参照を追加します。

![store-apps-storage-package-manager][store-apps-storage-package-manager]

次のコマンドを使用して、パッケージ マネージャーをコンピューター上の場所に指定します。

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

このコマンドにより、必要なすべての参照が自動的にプロジェクトに追加されます。 パッケージ マネージャー コンソールを使用しないようにする場合は、パッケージ ソースの一覧に、ローカル コンピューターに WCF Data Services NuGet フォルダーを追加およびの説明に従って、UI から、参照を追加し、 [を管理する NuGet パッケージ ダイアログを使用して、](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)します。

WCF Data Services NuGet パッケージを参照したら、ボタンの **Click** イベントのコードを変更します。

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

このコードで、"*table1*" というテーブルがアカウントに存在するかどうかがチェックされ、存在しない場合は作成されます。

ダウンロードした同じパッケージに含まれている Microsoft.WindowsAzure.Storage.Table.dll への参照を追加することもできます。 このライブラリには、リフレクション ベースのシリアル化や汎用クエリなどの追加機能が含まれます。 ただし、このライブラリは JavaScript をサポートしていないことに注意してください。




[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png 
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png 
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png 

