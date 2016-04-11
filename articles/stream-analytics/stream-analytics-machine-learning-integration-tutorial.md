<properties 
    pageTitle="チュートリアル: Azure Stream Analytics と Azure Machine Learning の統合 | Microsoft Azure" 
    description="Stream Analytics ジョブで UDF と Machine Learning を利用する方法"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="paulettm" 
    editor="cgronlun"
/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="12/14/2015" 
    ms.author="jeffstok"
/> 

# チュートリアル: Stream Analytics と Machine Learning の統合の概要 #

このチュートリアルは、単純な Stream Analytics ジョブを Machine Learning とすばやく統合できるように設計されています。 ここでは、Cortana Analytics ギャラリーのセンチメント分析 Machine Learning モデルを利用して、ストリーミング テキスト データを分析し、リアルタイムでセンチメント スコアを決定します。 ストリーミング Twitter データに関するリアルタイムのセンチメント分析、サポート スタッフによるユーザー チャット レコード分析、フォーラム/ブログ/ビデオに対するコメントなどのリアルタイムの予測スコアリング シナリオを理解するには、このチュートリアルがお勧めです。
  
このチュートリアルでは、テキストが入力されたサンプル CSV ファイル (下の図 1 を参照してください) が Azure BLOB ストアの入力として用意されています。 このジョブは、BLOB ストアのサンプル テキスト データに対して、ユーザー定義関数 (UDF) としてセンチメント分析モデルを適用します。 最終的な結果は、同じ Azure BLOB ストア内の別の CSV ファイルに出力されます。 この構成の内容を以下の図 2 に示します。 さらに現実的なシナリオとしては、この BLOB ストアの入力を Azure Event Hub 入力からのストリーミング Twitter データと置き換えることができます。 さらに、 [Power BI](https://powerbi.microsoft.com/) 集計センチメントのリアルタイムの視覚エフェクトを構築できるようにします。 この記事の今後の版には、このような拡張機能が追加される予定です。

図 1:  

![Stream Analytics Machine Learning チュートリアル: 図 1](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

図 2:    

![Stream Analytics Machine Learning チュートリアル: 図 2](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

## 前提条件

この記事の前提条件は次のとおりです。

1.  有効な Azure サブスクリプション
2.  ある程度のデータが入力された CSV ファイル。 図 2 に 1 つが提供される [GitHub で](https://github.com/jeffstokes72/azure-stream-analytics-repository/blob/master/sampleinputs.csv) のダウンロード、またはするが、独自に作成します。 このチュートリアルは、ダウンロードしたファイルを使用している前提で説明しています。

概要としては次の手順を実行します。

1.  CSV の入力ファイルを Blob Storage にアップロードします
2.  Cortana Analytics ギャラリーのセンチメント分析モデルを Machine Learning ワークスペースに追加します
3.  このモデルを Web サービスとして Azure Machine Learning ワークスペースにデプロイします
4.  この Web サービスを、テキスト入力のセンチメントを決定する関数として呼び出す Stream Analytics ジョブを作成します
5.  Stream Analytics ジョブを開始し、出力を確認します 


## CSV の入力ファイルを Blob Storage にアップロードします

この手順では、概要で説明したものなど、任意の CSV ファイルを使用できます。 ファイルをアップロードする [Azure ストレージ エクスプ ローラー](http://storageexplorer.com/) またはカスタム コードだけでなく Visual Studio を使用することがあります。 このチュートリアルでは、Visual Studio の例を紹介しています。

1.  Azure に展開し、右クリックし、 **ストレージ**します。 選択 **外部の記憶域をアタッチ** でき、 **アカウント名** と **アカウント キー**します。  

    ![Stream Analytics Machine Learning チュートリアル: サーバー エクスプローラー](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-server-explorer.png)  

2.  アタッチした記憶域を展開し、選択 **Blob コンテナーの作成** 論理名を指定します。 賛成されたら、コンテナーをダブルクリックして内容を確認します (この時点では空です)。  

    ![Stream Analytics Machine Learning チュートリアル: BLOB の作成](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-create-blob.png)  

3.  クリックして、CSV ファイルをアップロード、 **Blob のアップロード** アイコンを選択し **ローカル ディスクからファイルを**します。  

## Cortana Analytics ギャラリーからセンチメント分析モデルを追加する

1.  ダウンロード、 [センチメントの予測分析モデル](https://gallery.cortanaanalytics.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) Cortana Analytics ギャラリーにします。  
2.  クリックして **開く** studio:  

    ![Stream Analytics Machine Learning チュートリアル: Machine Learning Studio を開く](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3.  サインインしてワークスペースを開きます。 最適な場所を選択します。
4.  [今すぐ] をクリックして **実行** 、Studio の下部にあります。  
5.  実行が成功したら **Web サービスの展開**します。
6.  これで、センチメント分析モデルを使用できるようになりました。 を検証する] をクリックして、 **テスト** ボタンとテキストが大好き Microsoft」と、次のように、テストが同様の結果を返す必要がありますなどの入力を提供します。

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Stream Analytics Machine Learning チュートリアル: 分析データ](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

をクリックして、 **Excel 2010 以前のバージョン** API キーと、Stream Analytics ジョブを設定するため後で必要となる URL を取得するブックにリンクします。 (この手順は、別の Azure アカウントのワークスペースから Machine Learning モデルを利用する場合にのみ必要です。 このチュートリアルでは、このシナリオに対応する場合を想定しています)。  

![Stream Analytics Machine Learning チュートリアル: 分析実験](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-experiement.png)  

以下のように、ダウンロードした Excel の Web サービス URL とアクセス キーを書き留めます。  

![Stream Analytics Machine Learning チュートリアル: 概要](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## Machine Learning モデルを使用する Stream Analytics ジョブを作成します。

1.  移動し、 [Azure 管理ポータル](https://manage.windowsazure.com)します。  
2.  クリックして **新しい**, 、**Data Services**, 、**Stream Analytics** と **簡易作成**します。 提供、 **ジョブ名**, 、適切な **地域** ジョブの選択、 **地域の監視ストレージ アカウント**します。    
3.  移動し、ジョブが作成されたら、 **入力** ] タブでをクリックし、 **入力の追加**します。  

    ![Stream Analytics Machine Learning チュートリアル: Machine Learning 入力のデータ入力](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-input-screen.png)  

4.  最初のページで **入力の追加** ウィザード ウィンドウの選択 **データ ストリーム** し、[次へ] をクリックします。 2 ページ目に次のように選択します。 **Blob ストレージ** をクリックして、入力として **次**します。  
5.   **Blob ストレージの設定** ウィザードのページは、ストレージ アカウントの blob コンテナーを用意するデータがアップロードしたときに、先ほど定義した名前です。 クリックして **次**します。 選択 **CSV** として **イベントのシリアル化形式**します。 残りの既定値を受け入れ、 **のシリアル化設定**します。 Click **OK**.  
6.  移動し、 **出力** ] タブでをクリックし、 **出力を追加する**です。  

    ![Stream Analytics Machine Learning チュートリアル: 出力の追加](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-output-screen.png)  

7.  選択 **Blob ストレージ** し、コンテナーを除き、同じパラメーターを指定します。  **入力** "test"をという名前のコンテナーからの読み取りが構成されていた、 **CSV** ファイルをアップロードします。  **出力**, 、"testoutput"を配置します。 コンテナーは重複しない名前にする必要があります。また、そのコンテナーが存在していることを確認します。     
8.  をクリックして **次** 出力の構成に **のシリアル化設定**します。 選択、入力を持つ **CSV** ] をクリックし、 **[ok]** ] ボタンをクリックします。
9.  移動 **関数** ] タブでをクリックし、 **Machine Learning 関数を追加**します。  

    ![Stream Analytics Machine Learning チュートリアル: Machine Learning 関数の追加](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-ml-function.png)  

10.  **Machine Learning Web サービスの設定** ] ページで、Machine Learning ワークスペース、web サービス、および既定のエンドポイントを検索します。 このチュートリアルでは、URLを知っていてキーを持っている場合は、ワークスペースに合わせた Web サービス構成に慣れるように、手動で設定を適用します。 エンドポイントを指定 **URL** と **API キー**します。 クリックして **OK**します。    

    ![Stream Analytics Machine Learning チュートリアル: Machine Learning Web サービス](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-web-service.png)    

11. 移動 **クエリ** タブし、次に示すようにクエリを変更します。  

```
    WITH subquery AS (  
        SELECT text, sentiment(text) as result from input  
    )  
      
    Select text, result.[Score]  
    Into output  
    From subquery  
```

12. クリックして **保存** クエリを保存します。    

## Stream Analytics ジョブを開始し、出力を確認します

1.  クリックして **開始** 、ジョブの下部にあります。 
2.   **クエリ ダイアログの開始**, 、選択 **ユーザー設定の時刻** し CSV を Blob ストレージにアップロードされたときの前に時刻を選択します。 Click **OK**.  

    ![Stream Analytics Machine Learning チュートリアル: カスタム時間](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-custom-time.png)  

3.  CSV ファイルのアップロード時に使用したツールを使用して、Blob Storage に移動します。 このチュートリアルでは、Visual Studio を使用しました。
4.  ジョブが開始されて数分後に、出力コンテナーが作成され、CSV ファイルがアップロードされます。  
5.  ファイルをダブルクリックすると、既定の CSV エディターが開き、次のような内容が表示されます。  

    ![Stream Analytics Machine Learning チュートリアル: csv ビュー](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## まとめ

このチュートリアルでは、リアルタイムでストリーミング テキスト データを読み取り、そのデータに対してセンチメント分析を適用する Stream Analytics ジョブを作成しました。 このすべての処理は、センチメント分析モデルを構築する場合の複雑な作業を心配することなく実行できます。 これが Cortana Analytics スイートの利点の 1 つです。

Azure Machine Language 関数に関連するメトリックも確認できます。  をクリックして、 **モニター** ] タブをクリックします。 関数に関連する 3 つのメトリックがあります。  
  
- [関数要求] は、Machine Learning Web サービスに対する要求数を示します。  
- [関数イベント] は、要求のイベント数を示します。既定では、ML Web サービスに対する各要求に最大 1,000 イベントが含まれます。  
- [関数要求の失敗] は、Machine Learning Web サービスに対する要求が失敗した数を示します。  

    ![Stream Analytics Machine Learning チュートリアル: ML 監視ビュー](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-monitor-view.png)  

