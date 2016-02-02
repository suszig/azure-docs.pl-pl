<properties 
    pageTitle="Machine Learning のアプリケーション例: Frequently Bought Together | Microsoft Azure" 
    description="オンラインのショッピング カート分析によって、ユーザーによって提供されるトランザクション履歴から、頻繁に一緒に購入された品目を推奨製品として表示する Machine Learning Web サービスです。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="CoromT" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/08/2015" 
    ms.author="luisca"/> 


# Machine Learning のアプリケーション例: Frequently Bought Together

## 重要: このサービスは推奨されていません

このサービスを提供 Frequently Bought Together 機能は、複数の広範な [Recommendations API] (http://gallery.azureml.net/MachineLearningAPI/3574432384684cac9cc766e57729ea4c) に統合されているようになりました。 このサービスではなく、Recommendations を使用することをお勧めします。

## 概要

[Frequently Bought Together web サービス](https://datamarket.azure.com/dataset/amla/mba) Machine Learning では、トランザクション履歴から頻繁にまとめて購入された品目を推奨製品としてにオンラインのショッピング カート分析を実行します。 Frequently Bought Together による推奨は、買い物客が特定の品目を購入する際に、それと最も関係の深いカタログ内の製品を特定するのに役立ちます。 これらの推奨製品を目立つように表示することは、オンライン ショッピング業者の売上の増加に効果的であることが証明されています。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## 作業開始

Frequently Bought Together web サービスにサブスクライブしている後を使用して、 [マーケット バスケット分析サービス web アプリケーションの例](https://marketbasket.cloudapp.net/) 購入される製品セットを簡単にデータ モデルをアップロードして、頻繁に検出します。 使用するアプリケーションまたは API では、まずから入手できる API キーを [Azure データ マーケットのアカウント ページ](https://datamarket.azure.com/account)します。

## Web サービスの使用

このサービスに含まれる API を使用して、Frequently Bought Together モデルを管理および作成し、トランザクションの履歴をアップロードし、特定の製品に対して最も頻繁に一緒に購入された製品のセットを取得することができます。 これらの Api を使用する方法を示す例を参照して、 [の Azure-machinelearning-datascience](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Apps/FrequentlyBoughtTogether) GitHub のリポジトリです。







