<properties
    pageTitle="手順 5: Machine Learning Web サービスをデプロイする | Microsoft Azure"
    description="予測ソリューションの開発チュートリアルの 5 番目の手順: Machine Learning Studio で予測実験を Web サービスとしてデプロイする。"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/04/2015"
    ms.author="garye"/>


# チュートリアル手順 5: Azure Machine Learning Web サービスをデプロイする

これは、5 番目の手順のチュートリアルでは、 [Azure ML を使用した予測ソリューションの開発](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Machine Learning ワークスペースの作成](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [既存のデータをアップロードする](machine-learning-walkthrough-2-upload-data.md)
3.  [新しい実験を作成する](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [モデルをトレーニングして評価する](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  **Web サービスをデプロイする**
6.  [Web サービスにアクセスする](machine-learning-walkthrough-6-access-web-service.md)

----------

この予測モデルを他のユーザーが利用できるようにするには、Azure 上に Web サービスとしてデプロイします。

これまでは、モデルのトレーニングを実験してきました。 ただし、デプロイしたサービスのトレーニングは終了します。ユーザーの入力に基づく予測を開始します。 それではいくつかの準備を行い、この実験をユーザーがアクセスできる機能する Web サービスとしてデプロイします。 このサービスでは、ユーザーから送信された信用貸付申請データのセットに対して信用リスクの予測が返されます。

これを実現するには、次の操作を行う必要があります。  

- 変換、 *トレーニング実験* 作成した、 *予測実験*
- Web サービスとして予測実験をデプロイする

最初に、この実験を少しスリム化する必要があります。 現在、実験には 2 つの異なるモデルがありますが、モデルを 1 つ選択してデプロイします。  

使用にあたってはブースト ツリー モデルの方が優れていると判断したとします。 最初にすることを削除、 [2 クラス サポート ベクター マシン][two-class-support-vector-machine] モジュールとトレーニングのために使用されたモジュール。 クリックすると、まず、実験のコピーを作成することも **名前を付けて保存** 実験キャンバスの下部にあります。

次のモジュールを削除する必要があります。  

1.  [2 クラス サポート ベクター マシン][two-class-support-vector-machine]
2.  [モデルのトレーニング][train-model] と [モデルのスコア付け][score-model] 接続されているモジュール
3.  [データの正規化][normalize-data] (それらの両方)
4.  [モデルの評価][evaluate-model]

これでこのモデルをデプロイする準備ができました。

## トレーニング実験を予測実験に変換する

予測実験に変換するには、次の 3 つの手順を実行する必要があります。

1. トレーニングしたモデルを保存し、トレーニング モジュールと置き換えます。
2. 実験をトリミングしてトレーニングのためのみに必要だったモジュールを削除します。
3. Web サービスの入力と出力ノードの場所を定義します。

すべての 3 つの手順を実行して、クリックするだけで幸いにも、 **Web サービスの展開** 実験キャンバスの下部にある (選択、 **予測 Web サービス** オプション)。

クリックすると、 **Web サービスの展開**, 、いくつかの処理が行われます。

- としてトレーニング モデルを保存、 **トレーニング済みのモデル** 実験キャンバスの左側にあるモジュール パレット内のモジュール (パレットの下で見つかります **トレーニング済みのモデル**)。
- トレーニングに使用したモジュールは削除されます。 具体的には次の処理が行われます。
  - [2クラスのブースト デシジョン ツリー][two-class-boosted-decision-tree]
  - [モデルのトレーニング][train-model]
  - [Split][split]
  - 2 番目 [R スクリプトの実行][execute-r-script] テスト データに使用したモジュール
- 保存したトレーニング済みのモデルは実験に追加されます。
- **Web サービスの入力** と **サービスからの出力を Web** モジュールが追加されます。

> [AZURE.NOTE] 実験を 2 つの部分に保存されている: 元のトレーニング実験と新しい予測実験します。 実験のキャンバスの上部にあるタブを使用していずれかにアクセスできます。

この実験では、追加の手順を実行する必要があります。
Machine Learning Studio では、1 つ削除 [R スクリプトの実行][execute-r-script] モジュールに削除する際に、 [分割][split] モジュールが、もう一方のまま [R スクリプトの実行][execute-r-script] モジュールです。
トレーニングとテスト (、重み付けで関数を提供、サンプル データ) のみそのモジュールが使用した、のでお今すぐ削除して接続できます [メタデータ エディター][metadata-editor] に [モデルのスコア付け][score-model]します。    

現在、実験は以下のようになっています。  

![Scoring the trained model][4]  


UCI のドイツ語のクレジット カード データ データセットはどうして予測実験に残されたか疑問に思われるでしょう。 このサービスでは、元のデータセットではなくユーザーのデータを使用します。それにもかかわらずなぜこれらを接続したままにしているのでしょうか?

サービスで元のクレジット カード データを必要としないのは本当です。 しかし、列がいくつあり、どの列が数値型であるかなどの情報を含むこのデータのスキーマが必要になります。 このスキーマ情報は、ユーザーのデータを解釈するうえで必要になります。 そこで、これらのコンポーネントを接続したままにして、サービスが実行されているときにスコア付けモジュールがデータセット スキーマを利用できるようにしています。 データは使用されません。スキーマだけが使用されます。  

最後にもう一度実験を実行 (をクリックして **実行**)。 モデルがまだ稼動していることを確認する場合の出力] をクリックして、 [モデルのスコア付け][score-model] モジュールと選択 **結果を表示する**です。 元のデータが、信用リスク値 ("スコア付けラベル") とスコア付け確率値 ("スコア付け確率") と共に表示されます。  

## Web サービスをデプロイする

実験から派生する web サービスを展開するにはクリックして **Web サービスの展開** キャンバスの下です。 Machine Learning Studio によって実験が Web サービスとしてデプロイされ、サービス ダッシュボードが表示されます。   

> [AZURE.TIP] 展開した後は、web サービスを更新できます。 たとえば、モデルを変更する場合は、トレーニング実験を編集、モデル パラメーターを調整し、クリックするだけ **Web サービスの展開**します。 もう一度実験をデプロイすると、Web サービスが置き換えられ、更新済みのモデルが使用されるようになります。  

クリックしてサービスを構成することができます、 **構成** ] タブをクリックします。 このタブでは、サービス名 (既定で実験名が付けられます) を変更したり、説明を追加したりできます。 また、入力列と出力列にわかりやすいラベルを付けることもできます。  

![Web サービスを構成する][5]  

## Web サービスをテストする
 **ダッシュ ボード** ] ページで、をクリックして、 **テスト** ] にある [ **既定のエンドポイント**します。 サービスの入力データを要求するダイアログが表示されます。 これらは、元のドイツの信用リスク データセットに含まれるのと同じ列です。  
![Web サービスをテストします。][6]  


一連のデータを入力し、クリックして **OK**します。  

Web サービスによって生成された結果がダッシュボードの下部に表示されます。 サービスの構成に応じて、スコア付けモジュールによって生成された結果が表示されます。   


----------

**次: [web サービスにアクセス](machine-learning-walkthrough-6-access-web-service.md)**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/


