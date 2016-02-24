<properties
   pageTitle="複数の地理的リージョンに関するヘルプ ドキュメント | Microsoft Azure"
   description="ワークスペースを作成し、米国中南部 (SCUS) Azure リージョン以外の Azure リージョンに Web サービスを発行する方法について学習します。"
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="paulettm"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="11/23/2015"
   ms.author="tedway; neerajkh"/>

# 複数の地理的リージョンに関するヘルプ ドキュメント

この記事では、ワークスペースを作成し、その他の Azure リージョンに Web サービスを発行する方法について説明します。

## ワークスペースの作成

1. Azure クラシック ポータルにサインインします。

2.  クリックして **+ 新規** > **DATA SERVICES** > **MACHINE LEARNING** > **簡易作成**します。   **場所** などの別の領域の選択 **東南アジア**します。
![イメージ 1 のマルチ Geo ヘルプ][1]
3. ワークスペースを選択し、クリックして **ML Studio にサインイン**します。
![マルチ Geo ヘルプ image 2][2]

4. これで別のリージョンにワークスペースが作成され、他のワークスペースと同じように使用できます。 ワークスペースは画面の右上で切り替えます。 ドロップダウンをクリックし、リージョンを選択して、ワークスペースを選択します。 すべてがローカル ワークスペースの領域です。たとえば、すべてのワークスペースから作成された web サービスにワークスペースがある同じリージョンになります。
![マルチ Geo ヘルプ image 3][3]

## ギャラリーから実験を開く

ギャラリーから実験を開く場合、その実験をコピーするリージョンを選択することもできます。

![イメージ 4 のマルチ Geo ヘルプ][4a]

## Web サービスの管理

地域に固有のアドレスを使用して、再トレーニングなど web サービスをプログラムで管理する: **https://asiasoutheast.management.azureml.net**

### 注意する点

1.  同じリージョンに属しているワークスペース間でのみ実験をコピーするこができます。 将来的には、複数のリージョンにわたるワークスペース間で実験をコピーできるようにする予定です。
2.  リージョン セレクターには、一度に 1 つのリージョンのワークスペースのみが表示されます。 将来的には、ユーザーがアクセスできるすべてのリージョンにあるワークスペースを同時に表示できるようにする予定です。  
3.  フリー ワークスペース、またはゲスト アクセス (匿名の) ワークスペースは米国中南部に作成およびホストされます。 将来的には、フリー ワークスペースとゲスト アクセス ワークスペースをお好きなリージョンに作成できるようにする予定です。  
4.  東南アジアのワークスペースからデプロイされた Web サービスは、東南アジアにもホストされます。 将来的には、1 つのリージョンで実験を作成し、生成された Web サービス エンドポイントを別のリージョンに柔軟にデプロイできるようにする予定です。  

## 詳細情報

質問する、 [Azure Machine Learning フォーラム](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning)します。

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png

