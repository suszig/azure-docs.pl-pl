<properties
   pageTitle="Web サービスのスケーリング | Microsoft Azure"
   description="同時実行を増やし、新しいエンドポイントを追加して Web サービスを拡大する方法について説明します。"
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="azure machine learning, web services, operationalization, scaling, endpoint, concurrency"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/27/2015"
   ms.author="neerajkh"/>

# Web サービスのスケーリング

## 同時実行を増やす

既定では、発行される Web サービスはそれぞれ、20 件の同時実行要求をサポートするように構成されます。 この同時実行モードから 200 の同時要求を増やすことができます [Azure Classic Portal](https://manage.windowsazure.com/) 次の図に示すようにします。

移動して [Azure Classic Portal](https://manage.windowsazure.com/), 、左側の Machine Learning のアイコンをクリックして、web サービスを公開するために使用される作業領域を選択して、目的の web サービス] をクリックして、同時実行性の向上が必要なエンドポイントを選択および順にクリックして **構成**します。 スライダーを使用して、同時実行制御を向上し、クリックして **保存** 下側のパネルにします。

同時実行性を向上させるのを参照してください。 [API エンドポイントのスケーリング](machine-learning-scaling-endpoints.md)します。

   ![機械学習、エンドポイントのスケーリングします。][1]

## 同じ Web サービスの新しいエンドポイントを追加する

200 超の同時実行要求をサポートするか、複数のエンドポイントで可用性を上げるか、Web サービスの利用者ごとにエンドポイントを提供するとき、共通のタスクとして Web サービスのスケーリングが行われます。 同じ Web サービスのエンドポイントを追加することで規模を拡大できます。 ユーザーがを通じて追加のエンドポイントを追加できる [Azure Classic Portal](https://manage.windowsazure.com/) 次の図に示すようにします。

移動して [Azure Classic Portal](https://manage.windowsazure.com/), 、左側の Machine Learning のアイコンをクリックして、web サービスを公開するために使用される作業領域を選択して、目的の web サービス] をクリックして、] をクリックして **エンドポイントの追加** 下部にあるパネル、およびエンドポイントの名前、説明、および必要な同時実行制御を提供します。

新しいエンドポイントを追加するを参照してください。 [のエンドポイントの作成](machine-learning-create-endpoint.md)します。

   ![機械学習、新しいエンドポイントを追加します。][2]

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
