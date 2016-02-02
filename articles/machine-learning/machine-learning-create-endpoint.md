<properties 
    pageTitle="Machine Learning で Web サービス エンドポイントを作成する | Microsoft Azure" 
    description="Azure Machine Learning で Web サービス エンドポイントを作成する" 
    services="machine-learning" 
    documentationCenter="" 
    authors="hiteshmadan" 
    manager="padou" 
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd" 
    ms.date="09/17/2015"
    ms.author="himad"/>



# エンドポイントを作成する

Azure Machine Learning では、発行された Web サービスに対して複数のエンドポイントを作成できます。 各エンドポイントは、その Web サービスの他のエンドポイントとは別に、アドレス指定、スロットル、管理が個別に実行されます。 各エンドポイントに対して、一意の URL と承認キーがあります。

これにより、Azure Machine Learning のユーザーが、お客様に先物販売をすることができる Web サービスを作成できます。 各エンドポイントは、独自のトレーニング済みのモデルを使用して個別にカスタマイズできますが、この Web サービスを作成した実験へのリンクは維持されます。 さらに、実験の更新は選択的にエンドポイントに適用でき、カスタマイズが上書きされることはありません。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## エンドポイントの作成手順

- 開いている [http://manage.windowsazure.com](http://manage.windowsazure.com), をクリックして **Machine Learning** 左の列にします。 Web サービスに関心のあるワークスペースをクリックします。
![ワークスペースに移動する](./media/machine-learning-create-endpoint/figure-1.png)

- クリックして、 **Web サービス** ] タブをクリックします。
![Web サービスに移動する](./media/machine-learning-create-endpoint/figure-2.png)

- 利用可能なエンドポイントの一覧を表示するに関心がある web サービスをクリックします。
![エンドポイントに移動する](./media/machine-learning-create-endpoint/figure-3.png)

- 下部の **[エンドポイントの追加]** ボタンをクリックします。 名前と説明を入力して、この Web サービスの他のエンドポイントに同じ名前がないことを確認します。 特別な要件がない限り、調整レベルは既定値のままにします。
調整の詳細については、次を参照してください。 [API エンドポイントのスケーリング](machine-learning-scaling-endpoints.md)します。
![エンドポイントを作成する](./media/machine-learning-create-endpoint/figure-4.png)


エンドポイントが作成されたら、同期 API、バッチ API、および Excel のワークシートによってそれを使用できます。 この UI によるエンドポイントの追加に加え、エンドポイントは Endpoint Management API を使用してプログラムで追加することもできます。 詳細については、Machine Learning web サービスを使用して、次を参照してください。 [発行済み Azure Machine Learning web サービスを使用する方法について](machine-learning-consume-web-services.md)します。






