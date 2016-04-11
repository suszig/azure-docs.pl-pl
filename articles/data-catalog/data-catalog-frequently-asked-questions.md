<properties
   pageTitle="Azure Data Catalog のよく寄せられる質問"
   description="データ ソースの検出、注釈、管理など、Azure Data Catalog プレビューについてのよく寄せられる質問です。"
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="11/20/2015"
   ms.author="maroche"/>

# Azure Data Catalog のよく寄せられる質問

この記事では、Microsoft に関連する、よく寄せられる質問の回答 **Azure Data Catalog** サービスです。

## Q: what ' s **Azure Data Catalog**でしょうか。

A: Microsoft **Azure Data Catalog** 登録のシステムとエンタープライズ データ ソースの検出のシステムとして機能する Microsoft Azure クラウドでホストされている完全に管理されたサービスです。 **Azure Data Catalog** データ サイエンティスト、開発者を登録するには、アナリストによる評価をすべてのユーザーを有効にする機能の検出、理解、およびデータ ソースの使用を提供します。

## Q: Azure Data Catalog はどのようなお客様の課題を解決しますか。

**Azure Data Catalog** を検出し、エンタープライズ データ ソースを理解できるので、データ ソースの検出と「ダーク データ」の問題を解決します。

## Q: Azure Data Catalog の対象ユーザーはだれですか。

**Azure Data Catalog** などの技術者と技術者以外のユーザーに対して機能を提供します。

- データ開発者、BI、および分析のプロフェッショナル: 他のユーザーが使用するデータおよび分析用コンテンツの生成を担当するユーザー
-   データ スチュワード: データの意味、意図された使用方法、目的などのデータに関する知識を持つユーザー
- データ コンシューマー: 選択したツールを使用して、ジョブの実行に必要なデータを簡単に検出し、理解し、接続できる必要があるユーザー
- 中央 IT: ビジネス ユーザー向けに、数百のデータ ソースを探索可能にする必要があり、データの使用方法と使用者を監視し続ける必要があるユーザー

## Q: Azure Data Catalog の提供先のリージョンはどこですか。

プレビュー期間中、 **Azure Data Catalog** サービスは次のデータ センターでのみ使用できます。

- 米国西部
- 米国東部
- 西ヨーロッパ
- オーストラリア東部

## Q: Azure Data Catalog のデータ資産の数の制限はどのくらいですか。

Free エディションの **Azure Data Catalog** 5,000 の登録されているデータ資産に制限されます。

Standard Edition **Azure Data Catalog** 最大 100,000 の登録されているデータ資産をサポートしています。

## Q: サポートされているデータ ソースと資産の種類は何ですか。

プレビュー期間中、 **Azure Data Catalog** も、SQL Server Reporting Services (ネイティブ モードのみ)、Oracle データベースとして SQL Server リレーショナル (などの Azure SQL DB) と SQL Server Analysis Services (多次元および表形式) データベースがサポートされています。

プレビュー期間中、 **Azure Data Catalog** 登録と次の資産の種類の検出をサポートします。

- SQL Server テーブル
- SQL Server ビュー
- SQL Server Analysis Services ディメンション
- SQL Server Analysis Services メジャー
- SQL Server Analysis Services KPI
- SQL Server Analysis Services テーブル
- SQL Server Reporting Services レポート  
- Oracle Database テーブル
- Oracle Database ビュー
- Azure Storage BLOB
- Azure Storage ディレクトリ
- HDFS ファイル
- HDFS ディレクトリ
- Teradata テーブル
- Teradata ビュー

、上記の資産だけでなく **Azure Data Catalog** 、次のデータ資産コンテナーの検出をサポートしています。

- SQL Server Database
- Oracle Database
- HDFS クラスター
- Teradata データベース


## Q: 別のデータ ソースのサポートを要求するにはどうすればよいですか。

は、機能要求やその他のフィードバックを送信することができます、 [Azure Data Catalog フォーラム](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)します。

## Q: Azure Data Catalog はどのように使用を開始するのですか。

開始する最適な場所は次の手順に従って、 [Data Catalog の概要](../data-catalog-get-started/)します。 この記事は、パブリック プレビューでの機能のエンド ツー エンドのツアーです。

## Q: データの登録方法を教えてください。

データを登録する **Azure Data Catalog**, 、起動、 **Azure Data Catalog** 登録ツールの [発行] 領域から、 **Azure Data Catalog** ポータルです。  **Azure Data Catalog** を使用すると、同じ資格情報を使用してアプリケーションを発行するには、ログイン アクセス、 **Azure Data Catalog** ポータルで、データ ソースとを登録する特定の資産を選択します。

## Q: 登録されているデータ資産に対し、どのようなプロパティが抽出されますか。

データ ソースには、一般に、データ ソースから特定のプロパティが異なります、 **Azure Data Catalog** サービスの発行は、次の情報を抽出します。

- 資産名
- 資産の種類
- 資産の説明
- 属性/列名
- 属性/列データ型
- 属性/列の説明

> [AZURE.IMPORTANT] データ資産を登録する **Azure Data Catalog** をクラウドにデータをコピーまたは移動しません。 データ ソースから資産を登録すると、資産のメタデータが Azure にコピーされますが、データは、既存のデータ ソースの場所に残ります。 このルールの唯一の例外は、ユーザーが資産の登録時に、プレビュー レコードまたはデータ プロファイルをアップロードするように選択した場合です。 プレビューを含む、最大で 20 個のレコードは、各資産からコピーされますされにスナップショットとして格納 **Azure Data Catalog**します。 データ プロファイルを含む場合、集計情報 (テーブルのサイズ、列あたりの null 値の割合、および列の最小、最大、および平均値など) が計算され、カタログ内に格納されたメタデータに含められます。

<br/>

> [AZURE.NOTE] ファーストクラスのある SQL Server Analysis Services などのデータ ソースに対する **説明** プロパティには、 **Azure Data Catalog** アプリケーションの発行はそのプロパティ値を抽出します。 SQL Server のリレーショナル データベースがないに対してファーストクラス **説明** 、プロパティ、 **Azure Data Catalog** ms_description 拡張プロパティのオブジェクトと列から値を抽出はアプリケーションを発行します。 詳細については、TechNet を参照してください。 [データベース オブジェクトに対する拡張プロパティの使用](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx)します。

## Q: 新しく登録された資産が Azure Data Catalog に表示されるまでにどのくらいの時間がかかりますか。

資産を登録した後 **Azure Data Catalog** に表示されるまで 5 ~ 10 秒の期間がある可能性があります、 **Azure Data Catalog** ポータルです。

## Q: 登録したデータ資産のメタデータに注釈を付け、強化するには、どうするのですか。

登録した資産のメタデータを提供する最も簡単な方法で資産を選択、 **Azure Data Catalog** ポータルのプロパティ] ウィンドウまたはスキーマ ペインで選択したオブジェクトのメタデータの値を入力するとします。

エキスパートやタグなどのいくつかのメタデータは、登録プロセス中に指定することもできます。 指定された値、 **Azure Data Catalog** サービスの発行、その時点で登録されているすべての資産に適用されます。 追加の注釈のポータルに最近登録されたオブジェクトを表示する選択、 **ポータルの表示** の最後の画面のボタン、 **Azure Data Catalog** アプリケーションを発行します。

## Q: 登録済みのデータ オブジェクトを削除するにはどうすればよいですか。

オブジェクトを削除する **Azure Data Catalog** ポータルで、オブジェクトを選択し、クリックして、 **削除** ] ボタンをクリックします。 これからオブジェクトのメタデータが削除されます **Azure Data Catalog** が、実際の基になるデータ ソースには影響ありません。

## Q: エキスパートとは何ですか。

エキスパートは、データ オブジェクトに関して、情報に基づいた、ある観点を持つ個人です。 オブジェクトには、複数のエキスパートを指定できます。 エキスパートは、オブジェクトの "所有者" である必要はありません。エキスパートは単純に、データをどのように使用でき、使用すべきかを知っている者です。

## Q: プレビューの SLA はどうなっていますか。

中に **Azure Data Catalog** プレビューしたり、明示的なサービス レベル アグリーメントがありません。

## Q: 問題が発生した場合に、Azure Data Catalog チームに情報を伝えるにはどうすればよいですか。

使用してください、 **Azure Data Catalog** 問題を報告し、情報の共有、質問をフォーラム。 Http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409 にフォーラムがあります。

##Q: Azure Data Catalog は興味があるこの他のデータ ソースを処理しますか。
データ ソースの追加に取り組んでいます積極的に **Azure Data Catalog**します。 サポートされている、してください [それを提案 (または [提案されている場合を表明) するか、データ ソースがある場合、 [Azure Data Catalog フォーラム](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)します。

## Q: Azure Data Catalog は Power BI for Office 365 のデータ カタログとどのように関連しますか。

考えることができます **Azure Data Catalog** データ カタログの進化します。 **Azure Data Catalog** データ ソースの発行と検出のような機能を提供しますより広範なシナリオに焦点を合わせ、Office 365 に依存しません。 直後に、 **Azure Data Catalog** が一般的に使用できる 2 つのカタログは 1 つのサービスにマージするされます。

## Q: ユーザーは Azure Data Catalog に資産を登録するために、どのようなアクセス許可が必要ですか。

実行するユーザー、 **Azure Data Catalog** 登録ツールには、ソースからのメタデータを読み取ることができるデータ ソースに対する権限が必要があります。 ユーザーがプレビューも含めるように選択した場合、登録されるオブジェクトからのデータの読み取りができるアクセス許可も必要です。

## Q: Azure Data Catalog は、オンプレミスのデプロイメントにも使用できますか。

**Azure Data Catalog** クラウド サービスのハイブリッド データ ソース検出ソリューションを提供するデータ ソースがクラウドと内部設置型の両方を操作です。 現在のバージョンの計画はありません、 **Azure Data Catalog** オンプレミスで実行するサービスです。

##Q: 登録するデータ ソースから抽出するメタデータを追加したり、強化したりできますか。

機能の拡張に積極的に取り組んで **Azure Data Catalog**します。 参照してください抽出したデータ ソースからの登録中に追加のメタデータがある場合くださいそれを提案 (または既に提案されている場合の投票) で、 [Azure Data Catalog フォーラム](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)します。 将来、サード パーティが、拡張 API によって、新しいデータ ソースの種類を追加できるようにします。

## Q: 特定のユーザーだけが検出できるように、登録されているデータ資産の可視性を制限するにはどうすればいいですか。

A: のデータ資産を選択する、 **Azure Data Catalog**, 、所有権の取得] ボタンをクリックします。 データ資産の所有者 **Azure Data Catalog** を所有している資産を検出するか、特定のユーザーに可視性を制限するのには、すべてのカタログ ユーザーを許可するか、可視性の設定を変更できます。

## Q: データ ソースの変更がカタログに反映されるように、データ資産の登録を更新するにはどうすればいいですか。

A: カタログに既に登録されているデータ資産のメタデータを更新するには、その資産を含むデータ ソースを再登録するだけです。 テーブルまたはビューの列の追加や削除などのデータ ソースのすべての変更は、カタログで更新されますが、ユーザーによって提供されたすべての注釈は維持されます。

## Q: Azure Data Catalog を操作しているときに、質問をしたり、ヘルプを参照したりするにはどうすればいいですか。

問題が発生するか、または協力が必要な場合、 **Azure Data Catalog** プレビューしたりに投稿してください、 [Azure Data Catalog フォーラム](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)します。

## Q: ここに質問の答えがありません。どうすればいいですか。

 [Azure Data Catalog フォーラム](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)します。 そこで寄せられている質問に、ここでの方法が見つかります。

