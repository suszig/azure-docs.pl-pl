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

- [Azure ポータルを使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md)
- [Azure PowerShell を使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-powershell.md)
- [.NET SDK を使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-net-sdk.md)
- [Visual Studio の Data Lake Tools を使用して U SQL スクリプトを開発します。](data-lake-analytics-data-lake-tools-get-started.md)

**前提条件**

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Data Lake Analytics アカウント**。 参照してください [Azure ポータルを使用して Azure Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md)します。

## Data Lake Analytics アカウントを作成する

ジョブを実行するには、Data Lake Analytics アカウントが必要です。

Data Lake 分析アカウントごとに、 [Azure Data Lake ストア](data-lake-store-overview.md) 依存関係を考慮します。 このアカウントを参照します。
既定のデータストア湖アカウントです。 湖のデータ ストアのアカウントを作成するには、あらかじめまたは作成する場合 
Data Lake 分析アカウントです。 このチュートリアルでは、分析の湖のデータ ストアのアカウントを作成します 
アカウント

**Data Lake 分析アカウントを作成するには**

1. サインイン、 [Azure ポータル](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute)します。
2. クリックして **Microsoft Azure** を開くには、スタート画面左上隅にあります。
3. **[Marketplace]** タイルをクリックします。
3. **[すべて]** ブレードの検索ボックスに「**Azure Data Lake Analytics**」と入力して、**Enter** キーを押します。 リストに **Azure Data Lake Analytics** が表示されます。
4. リストから **Azure Data Lake Analytics** をクリックします。
5. ブレードの下部にある **[作成]** をクリックします。
6. 次の項目を入力または選択します。

    ![Azure Data Lake Analytics ポータルのブレード](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **名前**: Analytics アカウントに名前を付けます。
    - **Data Lake Store**: 各 Data Lake Analytics アカウントには、従属する Data Lake Store アカウントがあります。 Data Lake Analytics アカウントと従属する Data Lake Store アカウントは、同じ Azure データ センターに配置する必要があります。 以下の指示に従って、新しい Data Lake Store アカウントを作成するか、既存のものを選択します。
    - **サブスクリプション**: Analytics アカウントに使用する Azure サブスクリプションを選択します。
    - **リソース グループ**。 既存の Azure リソース グループを選択するか、新しいものを作成します。 アプリケーションは通常、Web アプリケーション、データベース、データベース サーバー、ストレージ、サード パーティのサービスなどの、複数のコンポーネントで構成されます。 Azure リソース マネージャー (ARM) を使用すると、アプリケーション内の複数のリソースを 1 つのグループ (Azure リソース グループと呼ばれます) と見なして作業できます。 アプリケーションのこれらすべてのリソースを、1 回の連携した操作でデプロイ、更新、監視、または削除できます。 デプロイメントにはテンプレートを使用しますが、このテンプレートは、テスト、ステージング、運用環境などのさまざまな環境に使用できます。 グループ全体のロールアップ コストを表示すると、組織の課金ついて明確に把握できます。 詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview.md)します。
    - **[場所]**:  Data Lake Analytics アカウントの Azure データ センターを選択します。
7. **[スタート画面にピン留めする]** を選択します。 このチュートリアルに従う場合はこれが必要です。
8. **[作成]** をクリックします。 ポータルのスタート画面が表示されます。 新しいタイルはホーム ページに追加され、"Azure Data Lake Analytics のデプロイ" を示すラベルが付けられます。 Data Lake 分析アカウントを作成するのにはしばらくをかかります。 アカウントが作成されると、ポータルの新しいブレードにアカウントが開きます。

    ![Azure Data Lake Analytics ポータルのブレード](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

## Web サイト ログ分析の対話型チュートリアルの実行

**Web サイト ログ分析の対話型チュートリアルを開くには**

1. ポータルで、左側のメニューにある **[Microsoft Azure]** をクリックしてスタート画面を開きます。
2. Data Lake Analytics アカウントにリンクされているタイルをクリックします。
3. **[要点]** バーの **[対話型チュートリアルの検索]** をクリックします。

    ![Data Lake Analytics の対話型チュートリアル](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. "サンプルがセットアップされていません。...をクリックしてください。" という内容のオレンジ色の警告が表示された場合は、**[サンプル データのコピー]** をクリックして、サンプル データを既定の Data Lake Store アカウントにコピーします。 対話型チュートリアルを実行するにはデータが必要です。
5. **対話型チュートリアル**のブレードで、**[Web サイト ログの分析]** をクリックします。 ポータルの新しいポータル ブレードにチュートリアルが開きます。
5. **[1 概要]** クリックしてから指示に従います。

## 関連項目

- [Microsoft Azure Data Lake 分析の概要](data-lake-analytics-overview.md)
- [Azure ポータルを使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md)
- [Azure PowerShell を使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-powershell.md)
- [Visual Studio の Data Lake Tools を使用して U SQL スクリプトを開発します。](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure Data Lake 分析を使用して web サイト ログを分析します。](data-lake-analytics-analyze-weblogs.md)





