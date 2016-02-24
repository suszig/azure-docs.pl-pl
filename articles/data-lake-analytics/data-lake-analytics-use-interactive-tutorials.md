<properties 
   pageTitle="Azure ポータルの対話型チュートリアルを使用する Data Lake Analytics と U-SQL の学習 | Azure" 
   description="Data Lake Analytics と U-SQL について学習するためのクイック スタートです。 " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2015"
   ms.author="jgao"/>


# Azure Data Lake Analytics の対話型チュートリアルの使用

Azure ポータルでは、Data Lake Analytics を使用するための対話型チュートリアルが提供されます。 この記事では、Web サイト ログを分析するためのチュートリアルを読み進める方法を示します。


>[AZURE.NOTE] Visual Studio を使用して同じチュートリアルを読み進めるを確認する場合 [Data Lake 分析を使用して web サイト ログ分析](data-lake-analytics-analyze-weblogs.md)します。
>ポータルにはさらに対話型チュートリアルが追加されます。


他のチュートリアルについては、以下を参照してください。

- [Azure ポータルで Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-portal.md)
- [Azure PowerShell で Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-powershell.md)
- [.NET SDK で Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-net-sdk.md)
- [Data Lake Tools for Visual Studio を使用する U-SQL スクリプトの開発](data-lake-analytics-data-lake-tools-get-started.md) 

**前提条件**

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Data Lake 分析アカウント**します。  参照してください [Azure ポータルを使用して Azure Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md)します。

##Data Lake Analytics アカウントを作成する 

ジョブを実行するには、Data Lake Analytics アカウントが必要です。

Data Lake 分析アカウントごとに、 [Azure Data Lake ストア](data-lake-store-overview.md) 依存関係を考慮します。  このアカウントを参照します。
既定のデータストア湖アカウントです。  湖のデータ ストアのアカウントを作成するには、あらかじめまたは作成する場合 
Data Lake 分析アカウントです。 このチュートリアルでは、分析の湖のデータ ストアのアカウントを作成します 
アカウント

**Data Lake 分析アカウントを作成するには**

1. サインイン、 [Azure ポータル](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute)します。
2. クリックして **Microsoft Azure** を開くには、スタート画面左上隅にあります。
3. クリックして、 **Marketplace** を並べて表示します。  
3. 型 **Azure Data Lake 分析** の検索ボックスに、 **すべて** ブレードで、およびキーを押して **ENTER**します。 表示されます **Azure Data Lake 分析** 一覧にします。
4. クリックして **Azure Data Lake 分析** リストからです。
5. クリックして **作成** ブレードの下部にあります。
6. 次の項目を入力または選択します。

    ![Azure Data Lake Analytics ポータルのブレード](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **名前**: Analytics アカウントの名前します。
    - **Data Lake ストア**: 各 Data Lake 分析アカウント依存データ湖ストア アカウントを持っています。 Data Lake Analytics アカウントと従属する Data Lake Store アカウントは、同じ Azure データ センターに配置する必要があります。 以下の指示に従って、新しい Data Lake Store アカウントを作成するか、既存のものを選択します。
    - **サブスクリプション**: Analytics アカウントに使用される Azure サブスクリプションを選択します。
    - **リソース グループ**します。 既存の Azure リソース グループを選択するか、新しいグループを作成します。 アプリケーションは通常、Web アプリケーション、データベース、データベース サーバー、ストレージ、サード パーティのサービスなどの、複数のコンポーネントで構成されます。 Azure リソース マネージャー (ARM) を使用すると、アプリケーション内の複数のリソースを 1 つのグループ (Azure リソース グループと呼ばれます) と見なして作業できます。 アプリケーションのこれらすべてのリソースを、1 回の連携した操作でデプロイ、更新、監視、または削除できます。 デプロイメントにはテンプレートを使用しますが、このテンプレートは、テスト、ステージング、運用環境などのさまざまな環境に使用できます。 グループ全体のロールアップ コストを表示すると、組織の課金ついて明確に把握できます。 詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview.md)します。 
    - **場所**します。 Data Lake Analytics アカウントの Azure データ センターを選択します。 
7. 選択 **スタート画面にピン**します。 このチュートリアルに従う場合はこれが必要です。
8. クリックして **作成**します。 ポータルのスタート画面が表示されます。 新しいタイルはホーム ページに追加され、"Azure Data Lake Analytics のデプロイ" を示すラベルが付けられます。 Data Lake 分析アカウントを作成するのにはしばらくをかかります。 アカウントが作成されると、ポータルの新しいブレードにアカウントが開きます。

    ![Azure Data Lake Analytics ポータルのブレード](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##Web サイト ログ分析の対話型チュートリアルの実行

**Web サイト ログ分析の対話型チュートリアルを開くには**

1. ポータルで、 **Microsoft Azure** を開くには、スタート画面の左側のメニューからです。
2. Data Lake Analytics アカウントにリンクされているタイルをクリックします。
3. クリックして **対話型チュートリアルを探索** から、 **Essentials** バー。

    ![Data Lake Analytics の対話型チュートリアル](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. オレンジ色示す警告が「しない設定のサンプルをクリックして.」, をクリックを表示する場合は **サンプル データをコピー** 既定湖のデータ ストアのアカウントにサンプル データをコピーします。 対話型チュートリアルを実行するにはデータが必要です。
5.  **対話型チュートリアル** ブレードで、をクリックして **web サイト ログ分析**します。 ポータルの新しいポータル ブレードにチュートリアルが開きます。
5. クリックして **1「Introduction** し、指示に従います

##関連項目

- [Microsoft Azure Data Lake Analytics の概要](data-lake-analytics-overview.md)
- [Azure ポータルで Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-portal.md)
- [Azure PowerShell で Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-powershell.md)
- [Data Lake Tools for Visual Studio を使用する U-SQL スクリプトの開発](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure Data Lake Analytics を使用する Web サイト ログの分析](data-lake-analytics-analyze-weblogs.md)

