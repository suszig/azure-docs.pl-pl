<properties 
   pageTitle="Visual Studio の接続済みサービスを使用して Azure Storage を追加する | Microsoft Azure"
   description="Visual Studio の [接続済みサービスの追加] ダイアログ ボックスを使用してアプリに Azure Storage を追加する"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="08/12/2015"
   ms.author="tarcher" />

# Visual Studio 接続済みサービスを使用した Azure ストレージの追加

## 概要

Visual Studio 2015 で接続できます任意の c# クラウド サービス、.NET バックエンド モバイル サービス、ASP.NET web サイトまたはサービス、ASP.NET 5 サービス、または Azure web ジョブのサービスを Azure Storage を使用して、 **[接続済みサービス** ダイアログ。 接続済みサービス機能は、必要なすべての参照と接続コードを追加し、構成ファイルを適切に変更します。 また、ダイアログでは、次の手順が BLOB ストレージ、キュー、およびテーブルの開始であることを知らせるドキュメントが示されます。

## サポートされているプロジェクトの種類

[接続済みサービス] ダイアログを使用して、次の種類のプロジェクトで Azure Storage に接続することができます。

- ASP.NET Web プロジェクト

- ASP.NET 5 プロジェクト

- .NET クラウド サービスの Web ロールと worker ロール プロジェクト

- .NET Mobile Services プロジェクト

- Azure WebJob プロジェクト


## [接続済みサービス] ダイアログを使用して Azure Storage に接続する

1. Azure アカウントがあることを確認します。 サインアップすることができます、Azure アカウントを取得していない場合、 [無料評価版](http://go.microsoft.com/fwlink/?LinkId=518146)します。 Azure アカウントを取得したら、ストレージ アカウントを作成し、モバイル サービスを作成して、Azure Active Directory を構成することができます。

1. Visual Studio でプロジェクトを開き、コンテキスト メニューを開き、 **参照** ソリューション エクスプ ローラーでノードを選択し **[接続済みサービス**します。

    ![Adding a connected service](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1.  **[接続済みサービス** ] ダイアログ ボックスで、選択 **Azure Storage**, を選択し、 **構成** ] ボタンをクリックします。 Azure にまだログインしていない場合は、ログインを促すメッセージが表示される場合があります。

    ![Add Connected Service dialog box - Storage](./media/vs-azure-tools-connected-services-storage/IC796703.png)

1.  **Azure Storage** ] ダイアログ ボックスで、既存のストレージ アカウントを選択し、をクリックし、 **追加**します。

    新しいストレージ アカウントを作成する必要がある場合は、次の手順に進みます。 必要ない場合は、手順 6 に進みます。

    ![Azure Storage dialog box](./media/vs-azure-tools-connected-services-storage/IC796704.png)

1. 新しいストレージ アカウントを作成するには 

    1. 選択、 **新しいストレージ アカウントの作成** Azure Storage] ダイアログ ボックスの下部にあるボタンをクリックします。

    1. 記入、 **ストレージ アカウントの作成** ] ダイアログ ボックスを選択し、 **作成** ] ボタンをクリックします。
    
        ![Azure Storage dialog box](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)

        チェックインする場合、 **Azure Storage** ダイアログ ボックスで、一覧に新しい記憶域が表示されます。

    1. 一覧に新しい記憶域を選択し、クリックして **追加**します。

1. Web ジョブ プロジェクトの [サービス参照] ノードの下に接続されているストレージ サービスが表示されます。

    ![Azure storage in web jobs project](./media/vs-azure-tools-connected-services-storage/IC796705.png)

1. 表示された作業の開始ページを確認し、プロジェクトがどのように変更されたかを確認します。 作業の開始ページは、接続済みサービスを追加したかどうかに関係なく、ブラウザーに表示されます。 推奨される次の手順とコード例を確認したり、[変更内容] ページに切り替えて、プロジェクトに追加された参照と、コードと構成ファイルがどのように変更されたかを確認したりすることができます。

## プロジェクトを変更する方法

ダイアログが完了すると、Visual Studio は参照を追加し、特定の構成ファイルを変更します。 特定の変更はプロジェクトの種類によって異なります。 

 - ASP.NET プロジェクトの場合、次を参照してください。 [何が起こったか – ASP.NET プロジェクト](http://go.microsoft.com/fwlink/p/?LinkId=513126)します。 
 - ASP.NET 5 プロジェクトでは、次を参照してください。 [何が起こったか – ASP.NET 5 プロジェクト](http://go.microsoft.com/fwlink/p/?LinkId=513124)します。 
 - クラウド サービス プロジェクト (web ロールとワーカー ロール) を参照してください [何が起こったか – クラウド サービス プロジェクト](http://go.microsoft.com/fwlink/p/?LinkId=516965)します。 
 - Web ジョブ プロジェクトでは、次を参照してください [内容。
web ジョブ プロジェクトが発生しました -](vs-storage-webjobs-what-happened/)です。

## 次のステップ

1. 作業の開始のコード サンプルを使用して、必要なストレージの種類を作成し、ストレージ アカウントにアクセスするためのコードを記述し始めます。

1. 質問してヘルプを表示します。
     - [MSDN フォーラム: Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)

     - [Azure のストレージ チーム ブログ](http://blogs.msdn.com/b/windowsazurestorage/)

     - [Storage (azure.microsoft.com)](http://azure.microsoft.com/services/storage)

     - [Storage のドキュメント (azure.microsoft.com)](http://azure.microsoft.com/documentation/services/storage/)


