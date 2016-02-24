<properties
    pageTitle="手順 6: Machine Learning Web サービスにアクセスする | Microsoft Azure"
    description="予測ソリューションの開発に関するチュートリアルの中の、アクティブな Azure Machine Learning Web サービスにアクセスするステップの 6 番目の手順です。"
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
    ms.date="09/08/2015"
    ms.author="garye"/>


# チュートリアル手順 6: Azure Machine Learning Web サービスにアクセスする

これは、このチュートリアルの最後に、 [Azure ML を使用した予測ソリューションの開発](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Machine Learning ワークスペースの作成](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [既存のデータをアップロードする](machine-learning-walkthrough-2-upload-data.md)
3.  [新しい実験を作成する](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [モデルをトレーニングして評価する](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Web サービスをデプロイする](machine-learning-walkthrough-5-publish-web-service.md)
6.  **Web サービスにアクセスする**

----------

Web サービスとして役に立つためには、ユーザーが Web サービスにデータを送信し、結果を受信できるようにする必要があります。 Web サービスを Azure Web サービスとして提供することで、以下のいずれかの方法でデータを受信して返すことができるようになります。  

-   **要求/応答** - ユーザーは、HTTP プロトコルを使用して、サービスを 1 つまたは複数の行の信用データを送信し、結果のセットで、サービスが応答します。
-   **バッチ実行** -ユーザーが、サービスを 1 つまたは複数の行の信用データを含む Azure blob の URL を送信します。 サービスは結果を別の BLOB に保存し、この BLOB のURL を返します。  

 **ダッシュ ボード** ] タブは、このサービスにアクセスするコードを記述する開発者に役立つ情報への 2 つのリンクして、web サービスのです。 クリックして、 **API ヘルプ ページ** リンクを **要求/応答** 行、およびサービスの要求/応答プロトコルを使用するサンプル コードを含むページが表示されます。 同様に、上のリンク、 **バッチ実行** 行は、サービスにバッチ要求を行うためのコード例を提供します。  

API ヘルプ ページには、R、C#、Python の各プログラミング言語に対応したサンプルがあります。

アクセスして、web サービスを使用する方法の詳細については、次を参照してください。 [Machine Learning の実験から発行された Azure Machine Learning web サービスを使用する方法について](machine-learning-consume-web-services.md)します。

