<properties
    pageTitle="ユニバーサル Windows アプリからの Azure BLOB ストレージへのイメージのアップロード | Microsoft Azure"
    description=".NET バックエンド モバイル サービスを使用してイメージを Azure BLOB ストレージにアップロードし、そのイメージにユニバーサル Windows アプリからアクセスする方法を説明します。"
    documentationCenter="windows"
    authors="ggailey777"
    services="mobile-services,storage"
    manager="dwrede"
    editor=""/>


<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="12/01/2015" 
    ms.author="glenga"/>


# モバイル サービスを使用した Azure ストレージへのイメージのアップロード

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-upload-data-blob-storage](../../includes/mobile-services-selector-upload-data-blob-storage.md)]

## 概要

このトピックでは、Azure モバイル サービスを使用して、ユーザーが生成したイメージをアプリケーションが Azure ストレージにアップロードおよび保存する方法を紹介します。 モバイル サービスでは、SQL データベースを使用してデータを保存します。 ただし、BLOB (Binary Large Object) データは、Azure BLOB ストレージ サービスに、より効率的に保存されます。

BLOB ストレージ サービスにデータを安全にアップロードするために必要な資格情報をクライアント アプリケーションで安全に配信できません。 代わりに、これらの資格情報をモバイル サービスに保存し、それらを使用して、新しいイメージをアップロードするために使用される SAS (Shared Access Signature) を生成する必要があります。 SAS は有効期間が短い資格情報 (この場合は 5 分間) であり、モバイル サービスによりクライアント アプリケーションに安全に返されます。 アプリケーションは、この一時的な資格情報を使用してイメージをアップロードします。 この例では、BLOB サービスからのダウンロードはパブリックです。

このチュートリアルでは、モバイル サービスにより生成された SAS を使用して、写真を撮影してイメージを Azure にアップロードする機能を、モバイル サービス クイック スタート アプリケーションに追加します。

## 前提条件

このチュートリアルには、次のものが必要です。

+ Microsoft Visual Studio 2013 Update 3 以降のバージョン。
+ [Azure ストレージ アカウント](../storage-create-storage-account.md)
+ コンピューターに接続された、カメラなどのイメージ キャプチャ デバイス

このチュートリアルは、Mobile Services のクイック スタートに基づいています。 このチュートリアルを開始する前に完了しておく必要 [を使ってみるモバイル サービス] です。

[AZURE.INCLUDE [mobile-services-dotnet-backend-configure-blob-storage](../../includes/mobile-services-dotnet-backend-configure-blob-storage.md)]

[AZURE.INCLUDE [mobile-services-windows-universal-dotnet-upload-to-blob-storage](../../includes/mobile-services-windows-universal-dotnet-upload-to-blob-storage.md)]

## 次のステップ

モバイル サービスを Blob サービスと統合することによりイメージを安全にアップロードできました。他のバックエンド サービスおよび統合のトピックを参照してください。

+ [モバイル サービスでのバックエンド ジョブをスケジュールします。](../mobile-services-dotnet-backend-schedule-recurring-tasks.md)

     モバイル サービスのジョブ スケジューラ機能を使用して、定義したスケジュールに従って実行されるサーバー スクリプト コードを定義する方法について説明します。

+ [モバイル サービス .NET の使用方法の概念リファレンス](../mobile-services-windows-dotnet-how-to-use-client-library.md)

     .NET で Mobile Services を使用する方法について説明します






[install the storage client library]: #install-storage-client 
[update the client app to capture images]: #add-select-images 
[install the storage client in the mobile service project]: #storage-client-server 
[update the todoitem definition in the data model]: #update-data-model 
[update the table controller to generate an sas]: #update-scripts 
[upload images to test the app]: #test 
[next steps]: #next-steps 
[get started with mobile services]: ../mobile-services-windows-store-dotnet-get-started.md 
[how to create a storage account]: ../storage-create-storage-account.md 
[azure storage client library for store apps]: http://go.microsoft.com/fwlink/p/?LinkId=276866 

