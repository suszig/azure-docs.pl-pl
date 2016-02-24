<properties
    pageTitle="Machine Learning Web サービスを発行する | Microsoft Azure"
    description="C# または Python を使用して、Azure Machine Learning Web サービスに承認キーを利用して接続します。"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2015" 
    ms.author="derrickv" />


# Azure Machine Learning Web サービスに接続する
開発者が体験する Azure Machine Learning は、入力データから予測をリアルタイムまたはバッチ モードで作成する Web サービス API です。 Azure Machine Learning Studio を使用して予測を作成し、Azure Machine Learning Web サービスをデプロイします。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Studio を使用して Azure Machine Learning Web サービスを作成してデプロイする方法の詳細については、次をご覧ください。

- [Machine Learning Web サービスをデプロイする](machine-learning-publish-a-machine-learning-web-service.md)
- [ML Studio を使ってみる](http://azure.microsoft.com/documentation/videos/getting-started-with-ml-studio/)
- [Azure Machine Learning のプレビュー](https://studio.azureml.net/)
- [Machine Learning ドキュメント センター](http://azure.microsoft.com/documentation/services/machine-learning/)

## Azure Machine Learning Web サービス ##

Azure Machine Learning (ML) Web サービスを使用して、外部のアプリケーションが ML のワークフローのスコア付けモデルとリアルタイムで通信します。 ML Web サービスの呼び出しは、予測結果を外部のアプリケーションに返します。 ML Web サービスの呼び出しを実行するために、予測のデプロイ時に作成される API キーを渡します。 ML Web サービスは、Web プログラミング プロジェクトでよく選択されるアーキテクチャの REST に基づいています。

Azure Machine Learning には、2 種類のサービスがあります。

- 要求応答サービス (RRS) – 待ち時間が短く拡張性の高い、ML Studio から作成およびデプロイされるステートレスなモデルへのインターフェイスを提供するサービス。
- バッチ実行サービス (BES) – データ レコードのバッチをスコア付けする非同期のサービス。

Azure Machine Learning web サービスに関する詳細については、次を参照してください。 [Machine Learning web サービスの配置](machine-learning-publish-a-machine-learning-web-service.md)します。

## Azure Machine Learning の承認キーを取得する ##
Web サービスの API キーを ML Web サービスから取得します。 Microsoft Azure Machine Learning Studio または Azure クラシック ポータルから取得できます。
### Microsoft Azure Machine Learning Studio ###
1. Microsoft Azure Machine Learning studio で、[ **WEB サービス** 左側です。
2. Web サービスをクリックします。 "API キー] が [、 **ダッシュ ボード** ] タブをクリックします。

### Azure クラシック ポータル ###

1. クリックして **MACHINE LEARNING** 左側です。
2. ワークスペースをクリックします。
3. クリックして **WEB サービス**します。
4. Web サービスをクリックします。
5. エンドポイントをクリックします。 [API キー] が右下にあります。

## <a id="connect"></a>Azure Machine Learning Web サービスに接続する

HTTP 要求と応答をサポートする任意のプログラミング言語を使用して、Azure Machine Learning Web サービスに接続することができます。 Azure ML Web サービス ヘルプ ページから、C#、Python、および R の例を表示できます。

### Azure ML Web サービス API のヘルプ ページを表示するには ###
Azure ML API ヘルプ ページは、Web サービスをデプロイするときに作成されます。 参照してください [Azure Machine Learning チュートリアル - Web サービスをデプロイする](machine-learning-walkthrough-5-publish-web-service.md)です。


**Azure ML API ヘルプ ページを表示するには**
Microsoft Azure Machine Learning Studio

1. 選択 **WEB サービス**します。
2. Web サービスを選択します。
3. 選択 **API ヘルプ ページ** - **要求/応答** または **バッチ実行**します。


**Azure ML API ヘルプ ページ**
Azure ML API ヘルプ ページには、予測 web サービスの詳細が含まれています。



### C# のサンプル ###

Azure ML web サービスに接続するには、使用、 **HttpClient** ScoreData を渡します。 ScoreData には、ScoreData を表す数値機能の n 次元ベクトルである FeatureVector が含まれています。 API キーを使用して、Azure ML サービスを認証できます。

ML web サービスに接続するため、 **Microsoft.AspNet.WebApi.Client** Nuget パッケージをインストールする必要があります。

**Microsoft.AspNet.WebApi.Client Nuget in Visual Studio をインストールする**

1. 「UCI からデータセットをダウンロード: 成人向け 2 クラス データセット」Web サービスを発行します。
2. クリックして **ツール** > **Nuget パッケージ マネージャー** > **パッケージ マネージャー コンソール**します。
2. 選択 **Install-package Microsoft.AspNet.WebApi.Client**します。

**サンプル コードを実行するには**

1. Azure ML サンプル コレクションに含まれる「サンプル 1: UCI からデータセットをダウンロード: 成人向け 2 クラス データセット」実験を発行します。
2. Web サービスからのキーを持つ apiKey を割り当てます。 Azure の ML 承認キーを取得する方法を参照してください。
3. 要求の URI を含む serviceUri を割り当てます。


### Python サンプル ###

Azure ML web サービスに接続するには、使用、 **urllib2** ScoreData を渡すライブラリです。 ScoreData には、ScoreData を表す数値機能の n 次元ベクトルである FeatureVector が含まれています。 API キーを使用して、Azure ML サービスを認証できます。


**サンプル コードを実行するには**

1. Azure ML サンプル コレクションに含まれる「サンプル 1: UCI からデータセットをダウンロード: 成人向け 2 クラス データセット」実験を発行します。
2. Web サービスからのキーを持つ apiKey を割り当てます。 Azure の ML 承認キーを取得する方法を参照してください。
3. 要求の URI を含む serviceUri を割り当てます。 要求の URI を取得する方法を参照してください。

