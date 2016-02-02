<properties 
   pageTitle="Microsoft Azure Data Lake Analytics の概要 | Azure" 
   description="Data Lake Analytics は Azure のビッグ データ コンピューティング サービスであり、クラウドのデータから得られた洞察を活用し、データを使用してビジネスを推進できます。その場所やサイズは関係ありません。Data Lake Analytics は、可能な限り最もシンプルで、スケーラブルかつ経済的な方法でこれを可能にします。 " 
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


# Microsoft Azure Data Lake Analytics の概要

## Azure Data Lake Analytics とは

Azure Data Lake Analytics は、ビッグ データ分析を容易にするために構築された、新しいサービスです。 このサービスでは、分散インフラストラクチャの操作ではなく、ジョブの記述、実行および管理に集中できます。 ハードウェアのデプロイ、構成、チューニングを行う代わりに、クエリを作成してデータを変換し、価値ある洞察を抽出します。 この分析サービスでは、必要な性能をダイヤルで設定するだけで、どんな規模のジョブでも即座に処理できます。 ジョブの実行中にのみ課金されるコスト効率の良いサービスです。 この分析サービスは Azure Active Directory をサポートしているので、既存のオンプレミスの ID システムと統合してアクセス権限とロールをシンプルに管理できます。 また、SQL のメリットとユーザー コードの表現力を融合した U-SQL 言語が組み込まれています。 U-SQL のスケーラブルな分散ランタイムで、Azure の SQL Server、Azure SQL Database、Azure SQL Data Warehouse にまたがるストア内のデータを効率良く分析できます。


## 主な機能

- **動的スケーリング**

    Data Lake Analytics は、クラウドのスケールとパフォーマンスのために一から構築されました。 リソースを動的にプロビジョニングするので、テラバイト規模のデータや、エクサバイト規模のデータも分析できます。 ジョブが完了するとリソースが自動的に縮小され、使用した処理能力の分だけのお支払いで済みます。 保存するデータのサイズや使用するコンピューティングの量を増やしたり減らしたりする際に、コードを書き直す必要はありません。 そのため、大規模なデータセットの処理や保存の仕方に悩むことなく、ビジネス ロジックに集中できます。

- **使い慣れたツールで、開発を迅速に、デバッグと最適化をスマートに**

    Data Lake Analytics は Visual Studio と密に連携しているため、使い慣れたツールでコードの実行、デバッグ、調整を行うことができます。 U-SQL ジョブが視覚化されるので、コードがどのように大規模に実行されるのかを見ることができます。そのため、パフォーマンスのボトルネックを簡単に特定し、コストを最適化できます。

- **U-SQL: シンプルで使いやすく、強力で拡張が可能**

    Data Lake Analytics には U-SQL が含まれています。これは、SQL のシンプルで使いやすい宣言的な特性を C# の表現力で拡張するクエリ言語です。 U-SQL 言語は、Microsoft 内のビッグ データ システムを支えているのと同じ分散ランタイムを基礎にしています。 SQL や .NET の何百万人もの開発者が、既に身に付けているスキルで、あらゆるデータを処理して分析できるようになります。

- **既存の IT 投資とシームレスに統合**

    Data Lake Analytics では、ID、管理、セキュリティ、データ ウェアハウジングのために既存の IT 投資を使用できます。 データ ガバナンスがシンプルになり、現在のデータ アプリケーションも容易に拡張できます。 Data Lake Analytics はユーザー管理とアクセス許可のための Active Directory と統合されており、監視と監査も組み込まれています。

- **リーズナブルな料金と高いコスト効率**

    Data Lake Analytics は、ビッグ データ ワークロードを実行するためのコスト効率の良いソリューションです。 データが処理されたときに、ジョブ ベースで料金が発生します。 ハードウェア、ライセンス、サービス固有のサポート契約は必要ありません。 ジョブの開始や完了に応じて、システムのスケールアップとスケールダウンが自動的に行われます。つまり、余分に課金されることはありません。

- **すべての Azure データに対応**

    Data Lake Analytics は、Azure BLOB ストレージや Azure SQL Database などの数多くの Azure データ ソースに対応できます。Data Lake Analytics は Azure Data Lake Store と連動するように特別に最適化されており、ビッグ データ ワークロードに対して最高レベルのパフォーマンス、スループット、並列化を提供します。

## 関連項目

- 作業開始
    - [Azure ポータルを使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md)
    - [Azure PowerShell を使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-powershell.md)
    - [Azure .NET SDK を使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-net-sdk.md)
    - [Visual Studio の Data Lake Tools を使用して U SQL スクリプトを開発します。](data-lake-analytics-data-lake-tools-get-started.md)
    - [Azure Data Lake 分析 U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md)

- U-SQL と開発
    - [Azure Data Lake 分析 U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md)
    - [Azure Data Lake 分析ジョブに対して U SQL ウィンドウ関数を使用します。](data-lake-analytics-use-window-functions.md)
    - [Data Lake 分析ジョブに対して U SQL ユーザー定義の演算子を開発します。](data-lake-analtyics-u-sql-develop-user-defined-operators.md)

- 管理
    - [Azure ポータルを使用して Azure Data Lake 分析を管理します。](data-lake-analytics-manage-use-portal.md)
    - [Azure PowerShell を使用して Azure Data Lake 分析を管理します。](data-lake-analytics-manage-use-powershell.md)
    - [監視し、Azure ポータルを使用して Azure Data Lake 分析ジョブのトラブルシューティング](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- エンド ツー エンド チュートリアル
    - [Azure Data Lake 分析の対話型チュートリアルを使用します。](data-lake-analytics-use-interactive-tutorials.md)
    - [Azure Data Lake 分析を使用して web サイト ログを分析します。](data-lake-analytics-analyze-weblogs.md)

- 意見の投稿
    - [ドキュメント バックログに対するコメントします。](data-lake-analytics-documentation-backlog.md)
    - [要求を送信する機能](http://aka.ms/adlafeedback)
    - [フォーラムのヘルプを表示します。](http://aka.ms/adlaforums)







