<properties
   pageTitle="Azure Data Lake Analytics U-SQL カタログの紹介 |Azure"
   description="Azure Data Lake Analytics U-SQL カタログの紹介"
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
   ms.date="10/26/2015"
   ms.author="jgao"/>


# U-SQL カタログの使用

U-SQL カタログを使用して、U-SQL スクリプトで共有できるように、データとコードを構成します。 カタログでは、Azure Data Lake のデータを使用して可能な限り最高のパフォーマンスを実現できます。

各 Azure Data Lake Analytics アカウントには、必ず 1 つの U-SQL カタログが関連付けられています。 U-SQL カタログを削除することはできません。 現在、U-SQL カタログを Data Lake Store アカウント間で共有することはできません。

各 U-SQL カタログには、**Master** というデータベースが含まれています。 Master データベースを削除することはできません。 各 U-SQL カタログには、追加でさらにデータベースを含めることができます。

U-SQL データベースには以下のものが含まれています。

- アセンブリ - U-SQL スクリプト間で .NET コードを共有します。
- テーブル値関数 – U-SQL スクリプト間で U-SQL コードを共有します。
- テーブル - U-SQL スクリプト間でデータを共有します。
- スキーマ - U-SQL スクリプト間でテーブル スキーマを共有します。

## カタログの管理

各 Azure Data Lake Analytics アカウントには、既定の Azure Data Lake Store アカウントが関連付けられています。 この Data Lake Store アカウントを既定の Data Lake Store アカウントと呼びます。 U-SQL カタログは、/catalog フォルダーの下の既定の Data Lake Store アカウントに格納されます。 /catalog フォルダー内のファイルは削除しないでください。

### Azure ポータルを使用

を参照してください [管理 Data Lake 分析ポータルを使用して](data-lake-analytics-use-portal.md#view-u-sql-catalog)


### Data Lake Tools for Visual Studio の使用

Data Lake Tools for Visual Studio を使用して、カタログを管理できます。 ツールの詳細については、次を参照してください。 [Data Lake Tools for Visual Studio を使用して](data-lake-analytics-data-lake-tools-get-started.md)します。

**カタログを管理するには**

1. Visual Studio を開き、Azure に接続します。 手順については、次を参照してください。 [Azure への接続](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure)します。
1. **Ctrl + Alt + S** キーを押して、**サーバー エクスプローラー**を開きます。
2. **サーバー エクスプ ローラー**で、**[Azure]**、**[Data Lake Analytics]**、[Data Lake Analytics アカウント]、**[データベース]**、**[master]** の順に展開します。


    - 新しいデータベースを追加するには、**[データベース]** を右クリックしてから **[データベースの作成]** をクリックします。
    - 新しいアセンブリを追加するには、**[アセンブリ]** を右クリックしてから **[アセンブリの登録]** をクリックします。
    - 新しいスキーマを追加するには、**[スキーマ]** を右クリックしてから [スキーマの作成] をクリックします。
    - 新しいテーブルを追加するには、**[テーブル]** を右クリックしてから [テーブルの作成] をクリックします。
    - 新しいテーブル値関数を追加するを参照してください。 [Davelop U SQL ユーザー定義の Data Lake 分析ジョブに対して演算子](data-lake-analytics-u-sql-develop-user-defined-operators.md)します。


![U-SQL Visual Studio カタログの参照](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## 関連項目

- 作業開始
    - [Azure ポータルを使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md)
    - [Azure PowerShell を使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-powershell.md)
    - [Azure .NET SDK を使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-net-sdk.md)
    - [Visual Studio の Data Lake Tools を使用して U SQL スクリプトを開発します。](data-lake-analytics-data-lake-tools-get-started.md)
    - [Azure Data Lake 分析 U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md)

- U-SQL と開発
    - [Azure Data Lake 分析 U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md)
    - [Azure Data Lake Aanlytics ジョブに対して U SQL ウィンドウ関数を使用します。](data-lake-analytics-use-window-functions.md)
    - [Data Lake 分析ジョブに対して U SQL ユーザー定義の演算子を開発します。](data-lake-analtyics-u-sql-user-defined-operators.md)

- 管理
    - [Azure ポータルを使用して Azure Data Lake 分析を管理します。](data-lake-analytics-use-portal.md)
    - [Azure PowerShell を使用して Azure Data Lake 分析を管理します。](data-lake-analytics-use-powershell.md)
    - [監視し、Azure ポータルを使用して Azure Data Lake 分析ジョブのトラブルシューティング](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- エンド ツー エンド チュートリアル
    - [Azure Data Lake 分析の対話型チュートリアルを使用します。](data-lake-analytics-use-interactive-tutorials.md)
    - [Azure Data Lake 分析を使用して web サイト ログを分析します。](data-lake-analytics-analyze-weblogs.md)





