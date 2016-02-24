<properties
    pageTitle="Stream Analytics でのリアルタイム Twitter センチメント分析 |Microsoft Azure"
    description="リアルタイム Twitter センチメント分析で Stream Analytics を使用する方法について説明します。 イベントの生成からライブ ダッシュボード上でのデータ操作までの手順。"
    keywords="real-time twitter trend analysis, sentiment analysis, social media analysis, trend analysis example"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="12/04/2015"
    ms.author="jeffstok"/>


# ソーシャル メディア分析: Azure Stream Analytics での Twitter センチメントのリアルタイム分析

Event Hubs に Twitter イベントをリアルタイム入力することで、ソーシャル メディア分析のためのセンチメント分析ソリューションを構築する方法について説明します。 データの分析しし、後で確認の結果を保存またはリアルタイムで洞察を提供するダッシュ ボードを使用する Stream Analytics クエリを記述して [Power BI](https://powerbi.com/)します。

ソーシャル メディア分析ツールは、組織がソーシャル メディアへの大量投稿に含まれるトレンディング トピック、重要なテーマや考え方を理解するのに役立ちます。 センチメント分析 (「意見マイニング」ともいう) ではソーシャル メディア分析ツールを使用して、製品やアイデアに対する考え方を特定します。 リアルタイム Twitter 傾向分析が優れた例となります。ハッシュタグ サブスクリプション モデルにより、特定のキーワードに耳を澄ませ、フィードに基づいてセンチメント分析を展開できます。 

## シナリオ - リアルタイムのセンチメント分析

ニュース メディア Web サイトは、その閲覧者に直接関係するサイト コンテンツを際立たせることで、競合他社より優位に立つことに着目しています。 そのため、Twitter データのリアルタイム センチメント分析を実行することで、閲覧者に関連するトピックのソーシャル メディア分析を利用します。 特に、Twitter のトレンディング トピックをリアルタイムで特定するには、主要なトピックのツイートの量とセンチメントに関するリアルタイム分析が必要です。 そのため、基本的に、このソーシャル メディア フィードに基づいたセンチメント分析エンジンが必要となります。

## 前提条件
1.  このチュートリアルでは、Twitter アカウントが必要です。  
2.  このチュートリアルでは、GitHub にある Twitter クライアント アプリケーションを使用します。  ダウンロード [ここ](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TwitterClient) し、ソリューションを設定するには、次の手順に従います。

## イベント ハブ入力とコンシューマー グループの作成

サンプル アプリケーションでは、イベントを生成し、それらを Event Hubs インスタンス (略して、イベント ハブ) にプッシュします。 Service Bus Event Hubs は、Stream Analytics に推奨されるイベント取り込み方法です。 Event Hubs のドキュメントを参照して [Service Bus のマニュアル](/documentation/services/service-bus/)

Event Hub を作成するには、次の手順に従います。

1.  Azure ポータルのクリックで **新規** > **APP SERVICES** > **SERVICE BUS** > **EVENT HUB** > **簡易作成** し、名前、地域、および新しい Event Hub を作成する新規または既存の名前空間を提供します。  
2.  ベスト プラクティスとして、各 Stream Analytics ジョブは、単一の Event Hubs コンシューマー グループから読み取ります。 以下でコンシューマー グループを作成するプロセスを段階的に説明します。詳細について、ここで学習できます。  コンシューマー グループを作成するには、新しく作成した Event Hub に移動し、をクリックして、 **コンシューマー グループ** ] タブの [ **作成** 、ページの下部にあるし、コンシューマー グループの名前を指定します。
3.  イベント ハブへのアクセスを許可するには、共有アクセス ポリシーを作成する必要があります。  クリックして、 **構成** イベント ハブのタブをクリックします。
4.   **共有アクセス ポリシー**, で新しいポリシーを作成する **管理** アクセス許可。


    ![Shared Access Policies where you can create a policy with Manage permissions.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-ananlytics-shared-access-policies.png)

5.  クリックして **保存** ページの下部にあります。
6.  移動、 **ダッシュ ボード** ] をクリック **接続情報** 下部ページおよびコピーと、接続情報を保存します。 (検索アイコンの下のコピー アイコンを使用します)。

## Twitter クライアント アプリケーションの構成および開始

使用して Twitter データを利用するクライアント アプリケーションを提供して [Twitter のストリーミング Api](https://dev.twitter.com/streaming/overview) をパラメーター化された一連のトピックに関するツイート イベントを収集します。 サード パーティのオープン ソース ツール [Sentiment140](http://help.sentiment140.com/) 各ツイートにセンチメント値を割り当てるために使用 (0: 否定的、2: 普通、4: 正)、ツイート イベントを Event Hub にプッシュします。  

アプリケーションを設定するには、次の手順に従います。

1.  [TwitterClient ソリューションをダウンロードします。](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TwitterClient)
2.  App.config を開き、oauth_consumer_key、oauth_consumer_secret、oauth_token、oauth_token_secret を、自身の値を設定した Twitter トークンに置き換えます。  

    [OAuth アクセス トークンを生成する手順](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)  

    トークンを生成するには、空のアプリケーションを作成する必要がある点に注意してください。  
3.  App.config 内の EventHubConnectionString 値と EventHubName 値をイベント ハブの接続文字列と名前に置き換えます。
4.  *省略可能:* を検索するキーワードを調整します。  既定で、このアプリケーションでは "Azure、Skype、XBox、Microsoft、シアトル" が検索されます。  必要な場合は、App.config の twitter_keywords の値を調整できます。
5.  ソリューションをビルドします。
6.  アプリケーションを起動します。  CreatedAt、Topic、SentimentScore の値が設定されたツイート イベントがイベント ハブに送信されていることがわかります。

    ![センチメント分析: イベント ハブに送信される SentimentScore 値。](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-sentiment-output-to-event-hub.png)

## Stream Analytics ジョブの作成

既に Twitter からのリアルタイムのツイート イベント ストリームが存在するため、リアルタイムでこれらのイベントを分析するための Stream Analytics ジョブを設定できます。

### Stream Analytics のジョブの準備

1.   [Azure ポータル](https://manage.windowsazure.com/), 、クリックして **新規** > **DATA SERVICES** > **STREAM ANALYTICS** > **簡易作成**します。
2.  次の値を指定し、クリックして **STREAM ANALYTICS ジョブの作成**:

    * **ジョブ名**: ジョブ名を入力します。
    * **地域**: ジョブを実行するリージョンを選択します。 パフォーマンスを向上させ、リージョン間のデータ転送が有料にならないように、同じリージョンにジョブとイベント ハブを配置することを検討します。
    * **ストレージ アカウント**: を使用してこのリージョン内で実行されているすべての Stream Analytics ジョブの監視データを格納するストレージ アカウントを選択します。 既存のストレージ アカウントを選択するか、新しいストレージ アカウントを作成することができます。

3.  クリックして **STREAM ANALYTICS** Stream Analytics ジョブを一覧表示するには、左側のペインにします。
    ![Stream Analytics サービス アイコン](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-service-icon.png)

4.  新しいジョブは、状態が表示されます **CREATED**します。 注意して、 **開始** 、ページの下部にあるボタンが無効になります。 ジョブを開始する前に、ジョブの入力、出力、クエリなどを構成する必要があります。


### ジョブの入力の指定
1.  Stream Analytics ジョブをクリックして **入力** クリックして、ページの上部にある **入力の追加**します。 表示されたダイアログ ボックスには、入力を設定する手順がいくつか表示されます。
2.  選択 **データ ストリーム**, 、右側のボタンをクリックします。
3.  選択 **EVENT HUB**, 、右側のボタンをクリックします。
4.  3 ページ目で、次の値を入力または選択します。

    * **入力の別名**: このジョブの入力、TwitterStream などのわかりやすい名前を入力します。 後でクエリでこの名前を使用します。
    **EVENT HUB**: 作成した Event Hub が Stream Analytics ジョブと同じサブスクリプション内にある場合は、イベント ハブがある名前空間を選択します。

        イベント ハブが別のサブスクリプションの場合は選択 **イベント ハブを使用する別のサブスクリプションを**, 、情報を手動で入力して **SERVICE BUS 名前空間**, 、**イベント ハブ名**, 、**イベント ハブ ポリシー名**, 、**イベント ハブ ポリシー キー**, 、および **イベント ハブ パーティション数**します。

    * **イベント ハブ名**: イベント ハブの名前を選択
    * **イベント ハブ ポリシー名**: このチュートリアルで前に作成したイベント ハブ ポリシーを選択します。
    * **イベント ハブ コンシューマー グループ**: このチュートリアルで前に作成したコンシューマー グループを入力します。
5.  右側のボタンをクリックします。
6.  次の値を指定します。

    * **イベント シリアライザー形式**: JSON
    * **エンコーディング**: UTF8

7.  チェック ボタンをクリックしてこのソースを追加し、Stream Analytics がイベント ハブに正常に接続できることを確認します。

### ジョブのクエリの指定

Stream Analytics は、変換を記述するための単純な宣言型のクエリのモデルをサポートします。 言語に関する詳細については、「、 [Azure Stream Analytics クエリ言語リファレンス](https://msdn.microsoft.com/library/azure/dn834998.aspx)します。  このチュートリアルでは、Twitter データに対するいくつかのクエリを作成してテストすることができます。

#### サンプル データの入力

実際のジョブ データに対するクエリを検証するには、サンプル データ機能を使用して、ストリームからイベントを抽出し、テスト用のイベントの .JSON ファイルを作成できます。

1.  Event Hub の入力に選択し、クリックして **サンプル データ** ページの下部にあります。
2.  表示されるダイアログ ボックスで、次のように指定します。、 **開始時刻** からのデータの収集を開始すると、 **期間** を使用するその他のデータ量にします。
3.  をクリックして、 **詳細** ボタンし、 **ここをクリックして** をダウンロードして保存へのリンク、します。生成される JSON ファイルです。

#### パススルー クエリ
最初に、イベント内のすべてのフィールドを示す単純なパススルー クエリを実行します。

1.  クリックして **クエリ** Stream Analytics ジョブ ページの上部にあります。
2.  コード エディターで、最初のクエリ テンプレートを次で置き換えます。

        SELECT * FROM TwitterStream

    入力ソースの名前が前に指定した入力の名前と必ず一致するようにします。

3.  クリックして **テスト** クエリ エディターの下
4.  サンプル .JSON ファイルを参照します。
5.  チェック ボタンをクリックして、クエリ定義の下に表示される結果を確認します。

    ![クエリ定義の下に表示される結果](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sentiment-by-topic.png)

#### トピック別のツイート数: 集計を含むタンブリング ウィンドウ

利用してトピック間でメンション数を比較する、 [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) を 5 秒ごとのトピックでメンション数を取得します。

1.  コード エディターでクエリを次のように変更します。

        SELECT System.Timestamp as Time, Topic, COUNT(*)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

    このクエリを使用している、 **TIMESTAMP BY** 一時的な計算に使用されるペイロードのタイムスタンプ フィールドを指定するキーワードです。  このフィールドが指定されていない場合は、イベントがイベント ハブに到着した時間を使用してウィンドウ化操作が実行されます。  詳細情報「到着時間とアプリケーション時間」の下で、 [Stream Analytics クエリ リファレンス](https://msdn.microsoft.com/library/azure/dn834998.aspx)します。

    このクエリで各ウィンドウの最後のタイムスタンプをアクセスも **System.Timestamp**します。

2.  クリックして **再実行** クエリの結果を表示するクエリ エディターの下です。

#### トレンディング トピックの特定: スライディング ウィンドウ

トレンディング トピックを特定するには、一定期間にメンションのしきい値を超えるトピックを検索します。  このチュートリアルの目的で、皆様の評価いたしますを使用して、最後の 5 秒間に 20 回を超えてがメンションされたトピックについて、 [SlidingWindow](https://msdn.microsoft.com/library/azure/dn835051.aspx)します。

1.  コード エディターでクエリを次のように変更します。

        SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY SLIDINGWINDOW(s, 5), topic
        HAVING COUNT(*) > 20

2.  クリックして **再実行** クエリの結果を表示するクエリ エディターの下です。

    ![スライディング ウィンドウのクエリ出力](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-query-output.png)

#### メンションとセンチメントの数: 集計を含むタンブリング ウィンドウ

テストする最後のクエリでは、タンブリング ウィンドウを使用して、5 秒ごとに各トピックのメンション数と、センチメント スコアの平均値、最小値、最大値、標準偏差を取得します。

1.  コード エディターでクエリを次のように変更します。

        SELECT System.Timestamp as Time, Topic, COUNT(*), AVG(SentimentScore), MIN(SentimentScore),
        Max(SentimentScore), STDEV(SentimentScore)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

2.  クリックして **再実行** クエリの結果を表示するクエリ エディターの下です。
3.  これは、ダッシュボード用に使用されるクエリです。  クリックして **保存** ページの下部にあります。


## 出力シンクの作成

イベント ストリーム、イベントを取り込むためのイベント ハブ入力、ストリームに対して変換を実行するためのクエリを定義したところで、最後に、ジョブの出力シンクを定義します。  ジョブ クエリから Azure BLOB に集計済みツイート イベントを書き込みます。  また、結果は、具体的なアプリケーションのニーズに応じて、SQL Database、テーブル ストア、またはイベント ハブにプッシュできます。

BLOB ストレージ用のコンテナーがまだない場合は、次の手順に従って作成します。

1.  既存のストレージ アカウントを使用して、クリックして新しいストレージ アカウントを作成または **新規** > **DATA SERVICES** > **ストレージ** > **簡易作成** > 画面の指示に従います。
2.  ストレージ アカウントを選択し、クリックして **コンテナー** クリックして、ページの上部にある **追加**します。
3.  指定、 **名前** コンテナーとセットの **アクセス** パブリック Blob にします。

## ジョブの出力の指定

1.  Stream Analytics ジョブでクリックして **出力** クリックして、ページの上部にある **出力の追加**します。 表示されたダイアログ ボックスには、出力を設定する手順がいくつか表示されます。
2.  選択 **BLOB ストレージ**, 、右側のボタンをクリックします。
3.  3 ページ目で、次の値を入力または選択します。

    * **出力の別名**: このジョブの出力のフレンドリ名を入力してください。
    * **サブスクリプション**: 作成した Blob ストレージが Stream Analytics ジョブと同じサブスクリプションの場合は選択 **現在のサブスクリプションからストレージ アカウントを使用する**です。  ストレージが別のサブスクリプションの場合は選択 **別のサブスクリプションからストレージ アカウントを使用する** 情報を手動で入力して **ストレージ アカウント**, 、**ストレージ アカウント キー**, 、**コンテナー**します。
    * **ストレージ アカウント**: ストレージ アカウントの名前を選択
    * **コンテナー**: コンテナーの名前を選択
    * **ファイル名のプレフィックス**: blob 出力の書き込みに使用するファイルのプレフィックスの種類

4.  右側のボタンをクリックします。
5.  次の値を指定します。
    * **イベント シリアライザー形式**: JSON
    * **エンコーディング**: UTF8
6.  チェック ボタンをクリックしてこのソースを追加し、Stream Analytics からストレージ アカウントに正常に接続できることを確認します。

## ジョブの開始

ジョブの入力、クエリ、および出力がすべて指定されたため、いつでも Stream Analytics ジョブを開始できます。

1.  ジョブから **ダッシュ ボード**, 、] をクリックして **開始** ページの下部にあります。
2.  表示されるダイアログ ボックスで、次のように選択します。 **ジョブ開始時刻**, 、ダイアログ ボックスの下部にあるチェック マーク ボタンをクリックします。 ジョブの状態が変わります **開始** は、すぐに移動して **を実行している**します。


## センチメント分析の出力表示

ジョブが実行され、リアルタイム Twitter ストリームが処理されている場合は、センチメント分析の出力方法を選択します。 などのツールを使用して [Azure ストレージ エクスプ ローラー](https://azurestorageexplorer.codeplex.com/) または [Azure エクスプ ローラー](http://www.cerebrata.com/products/azure-explorer/introduction) をリアルタイムで、ジョブの出力を表示します。 ここでは、カスタマイズされたダッシュ ボードを示したを使用して下の図のように、出力が含まれるようにするアプリケーションを拡張できます [Power BI](https://powerbi.com/)します。

![ソーシャル メディア分析: Power BI ダッシュ ボードでの Stream Analytics センチメント分析 (意見マイニング) 出力。](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-output-power-bi.png)

## サポートを受ける
詳細については、参照してください、 [Azure Stream Analytics フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)します。 


## 次のステップ

- [Azure Stream Analytics の概要](stream-analytics-introduction.md)
- [Azure Stream Analytics の使用](stream-analytics-get-started.md)
- [Azure Stream Analytics ジョブのスケーリング](stream-analytics-scale-jobs.md)
- [Stream Analytics Query Language Reference (Stream Analytics クエリ言語リファレンス)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics management REST API reference (Azure ストリーム分析の管理 REST API リファレンス)](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 

