<properties
    pageTitle="Machine Learning Web サービスをデプロイする | Microsoft Azure"
    description="トレーニング実験を予測実験に変換する方法、デプロイメントの準備をする方法、Azure Machine Learning Web サービスとしてデプロイする方法について説明します。"
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
    ms.date="11/03/2015"
    ms.author="garye"/>


# Azure Machine Learning Web サービスをデプロイする

Azure Machine Learning では、予測分析ソリューションをビルド、テスト、およびデプロイできます。

大まかに次の 3 つの手順で行われます。

- **[トレーニング実験の作成]** -Azure Machine Learning Studio は、共同開発環境の調整し、供給するトレーニング データを使用して予測分析モデルをテストするために使用します。
- **[予測の実験に変換]** -準備して、スコア付け実験を効率化に既存のデータ モデルを学習し、新しいデータのスコア付けに使用する準備ができたら、します。
- **[Web サービスとして deploy]** -クリックするだけでは、Azure の web サービスとしてスコア付け実験を展開することができます。 ユーザーはご利用のモデルにデータを送信し、モデルの予測を受信できます。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Web サービスをデプロイすると、次のことが可能になります。

- **[アクセス]** web サービス API から
- **[管理]** Azure クラシック ポータルで、
- **[Update]** 、モデルが変更されました。

## トレーニング実験を作成する

予測分析モデルを学習するには、Azure Machine Learning Studio を使用してトレーニング実験を作成します。ここではトレーニング データを読み込み、必要に応じてデータを用意し、機械学習アルゴリズムに適用し、結果を評価するさまざまなモジュールを含めることができます。 実験を繰り返し、別の機械学習アルゴリズムを試して結果を比較したり評価したりできます。

トレーニング実験を作成したり管理したりするプロセスはあらゆる場所でより包括的に使用できます。詳細と例については、次の記事をご覧ください。

- [Azure Machine Learning Studio で簡単な実験を作成します。](machine-learning-create-experiment.md)
- [Azure Machine Learning を使用した予測ソリューションを開発します。](machine-learning-walkthrough-develop-predictive-solution.md)
- [Azure Machine Learning Studio にトレーニング データをインポートします。](machine-learning-import-data.md)
- [Azure Machine Learning Studio での実験イテレーションを管理します。](machine-learning-manage-experiment-iterations.md)

## トレーニング実験を予測実験に変換する

モデルをトレーニングすると、新しいデータをスコア付けで使用できるようになります。 そのためには、トレーニング実験を予測実験に変換します。
予測実験に変換することで、トレーニングされたモデルをスコア付け Web サービスとしてデプロイできるようになります。 Web サービスのユーザーがモデルに入力データを送信し、モデルは予測結果を返します。 したがって、予測実験を変換した場合は、モデルが他のユーザーに使用されることを想定しておいてください。

トレーニング実験を予測実験に変換するには、実験キャンバスの下部にある **[実行]** をクリックし、**[Web サービスの設定]** をクリックして、**[予測 Web サービス]** を選択します。

![スコア付け実験に変換する](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

この変換を行う方法の詳細については、を参照してください [予測実験を Machine Learning トレーニング実験に変換](machine-learning-convert-training-experiment-to-scoring-experiment.md)。


## Web サービスとして予測実験をデプロイする

予測実験の準備を十分に実行したので、Azure Web サービスとしてデプロイできます。 Web サービスを使用してユーザーはデータをモデルに送信でき、モデルは予測を返します。

予測実験をデプロイするには、実験キャンバスの下部にある **[実行]** をクリックし、**[Web サービスのデプロイ]** をクリックします。 Web サービスがセットアップされ、Web サービスのダッシュ ボードに配置されます。

![Web サービスをデプロイする](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

Web サービスをテストするには、Web サービス ダッシュボードで **[テスト]** リンクをクリックします。 ダイアログ ボックスが表示され、サービスへのデータの入力が促されます。 これらはスコア付け実験で想定される列になります。 データのセットを入力し、**[OK]** をクリックします。 Web サービスによって生成された結果がダッシュボードの下部に表示されます。

![Web サービスをテストする](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

**[構成]** タブで、サービスの表示名を変更したり、説明を付けたりできます。 この名前と説明は、Web サービスを管理する Azure クラシック ポータルに表示されます。
**INPUT SCHEMA**、**OUTPUT SCHEMA**、**WEB SERVICE PARAMETER** の各列に文字列を入力し、入力データ、出力データ、Web サービス パラメーターの説明を追加できます。 これらの説明は、Web サービスのサンプル コードのドキュメントで使用されます。
また、ログ記録を有効にすれば、Web サービスのアクセスで発生するすべてのエラーを診断できます。

詳細については、次を参照してください。 [Machine Learning web サービスのログ記録を有効にする](machine-learning-web-services-logging.md)します。

![Web サービスを構成する](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)


## Web サービスにアクセスする

Machine Learning Studio から Web サービスをデプロイすると、サービスにデータを送信し、プログラムからの応答が得られます。

ダッシュ ボードでは、Web サービスへのアクセスが必要なすべての情報が提供されます。 たとえば、サービスへの承認済みアクセスを許可する API キーや、コードの記述に役立つ API のヘルプ ページが提供されます。

Machine Learning web サービスにアクセスする方法の詳細については、次を参照してください。 [展開済み Azure Machine Learning web サービスを使用する方法について](machine-learning-consume-web-services.md)します。


## Azure クラシック ポータルで Web サービスを管理する

Azure クラシック ポータルでは、**[Machine Learning]** サービスをクリックして Machine Learning ワークスペースを開き、**[WEB SERVICES]** タブから Web サービスを開くことで Web サービスを管理できます。 このページから Web サービスの監視、更新、削除ができます。 Web サービスのデプロイ時に作成される既定のエンドポイントに加え、Web サービスの 2 つ目のエンドポイントを追加することもできます。

詳細については、次を参照してください。 [Azure Machine Learning ワークスペースの管理](machine-learning-manage-workspace.md)します。



## Web サービスを更新する

別のトレーニング データでのモデルの更新など、Web サービスに変更を加えたり、元の Web サービスを上書きしてもう一度デプロイしたりできます。

Web サービスを更新するには、web サービスを展開し、クリックすると、編集可能なコピーを作成するために使用する元の予測実験を開き **名前を付けて保存**します。 必要な変更を加えて **[WEB サービスのデプロイ]** をクリックします。 この実験は以前にデプロイしているため、Machine Learning Studio で既存のサービスを上書きするかどうかの確認を求められます。 **[はい]** をクリックすると、既存の Web サービスが停止され、新しい予測実験が代わりにデプロイされます。
> [AZURE.NOTE] たとえば元の Web サービスの構成に変更を加えた場合や新しい表示名または説明を入力した場合、それらの値をもう一度入力する必要が生じます。

Web サービスを更新するオプションの 1 つに、モデルをプログラムによって再トレーニングする方法があります。 詳細については、次を参照してください。 [再トレーニングの機械学習モデルをプログラムによって](machine-learning-retrain-models-programmatically.md)します。




[create a training experiment]: #create-a-training-experiment 
[convert it to a predictive experiment]: #convert-the-training-experiment-to-a-predictive-experiment 
[deploy it as a web service]: #deploy-the-predictive-experiment-as-a-web-service 
[access]: #access-the-web-service 
[manage]: #manage-the-web-service-in-the-azure-management-portal 
[update]: #update-the-web-service 

