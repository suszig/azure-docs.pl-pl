<properties
    pageTitle="Web アプリ テンプレートによる Azure Machine Learning Web サービスの使用 | Microsoft Azure"
    description="Azure Marketplace の Web アプリ テンプレートを利用して、Azure Machine Learning で予測 Web サービスを使用します。"
    keywords="web service,operationalization,REST API,machine learning"
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
    ms.date="10/26/2015"
    ms.author="garye;raymondl"/>

# Web アプリケーション テンプレートによる Azure Machine Learning Web サービスの使用

予測モデルを開発し、Machine Learning Studio を使用して、R または Python などのツールを使用したり、Azure の web サービスとして展開した後
REST API を使用して運用可能なモデルにアクセスすることができます。

REST API を使用して Web サービスにアクセスする方法は、いろいろあります。 たとえば、Web サービスをデプロイしたときに生成されたサンプル コード (Machine Learning Studio の Web サービス ダッシュボードにある API ヘルプ ページで利用可能) を使用して、C#、R、または Python でアプリケーションを記述できます。 また、作成されたサンプル Microsoft Excel ブック (これも Studio の Web サービス ダッシュボードで利用可能) を使用することもできます。

Web サービスにアクセスする最も迅速かつ最も簡単な方法で使用できる Web アプリ テンプレート使用が、 [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/)します。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Azure Machine Learning Web アプリ テンプレート

Azure Marketplace で入手できる Web アプリ テンプレートを使用すると、Web サービスの入力データと予想される結果を認識するカスタム Web アプリを構築できます。 必要な操作は、Web アプリに Web サービスおよびデータへのアクセスを許可することだけで、後の処理はテンプレートによって行われます。

使用できるテンプレートは、次の 2 つです。

- [Azure ML Request-Response Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
- [Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

各テンプレートは、Web サービスの API URI とキーを使用してサンプル ASP.NET アプリケーションを作成し、Web サイトとして Azure にデプロイします。 Request-Response Service (RRS) テンプレートは、1 つの結果を取得するために Web サービスに 1 行のデータを送信できる Web アプリケーションを作成します。 Batch Execution Service (BES) テンプレートは、複数の結果を取得するために多くの行のデータを送信できる Web アプリケーションを作成します。

これらのテンプレートは、コードを記述せずに使用できます。 API URI とキーだけを指定すれば、テンプレートによってアプリケーションが構築されます。

## Request-Response Service (RRS) テンプレートの使用方法

Web サービスをデプロイした後は、次の図のように、以下の手順に従って RRS Web アプリケーション テンプレートを使用します。

![RRS Web テンプレートを使用する手順][image1]

1. Machine Learning Studio で開く、 **Web サービス** タブにアクセスする web サービスを開きます。 キーの下に一覧表示をコピー **API キー** し、保存します。

    ![API キー][image3]

2. 開いている、 **要求/応答** API ヘルプ ページです。 ヘルプ ページの上部にある [ **要求**, 、コピー、 **要求の URI** 値し、保存します。 この値は、次のようなものです。

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![要求 URI][image4]

3. 移動して [Azure ML 要求応答サービスの Web アプリ](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) ] をクリック **Web アプリの作成**します。 テンプレートから、新しい Web アプリが作成された Microsoft Azure クラシック ポータルにアクセスできます。

    - Web アプリに一意の名前を付けます。 Web アプリの URL は、この名前の後に `.azurewebsites.net.` を付けたものです。たとえば、`http://carprediction.azurewebsites.net.` のようになります。

    - 実行している Web サービスの Azure サブスクリプションとサービスを選択します。

    - クリックして **作成**します。

    ![Web アプリの作成][image5]

4. Azure の web アプリのデプロイが完了したら、クリックして、 **URL** Azure で web アプリ設定] ページまたは web ブラウザーで URL を入力します。 たとえば、`http://carprediction.azurewebsites.net.` のように指定します。

5. Web アプリケーションの初回実行時に要求を送信するため、 **API Post URL** と **API キー**します。
前に保存しておいた値を入力します。
    - **要求の URI** API からのヘルプ ページ **API Post URL**
    - **API キー** の web サービスのダッシュ ボードから、 **API キー**します。

    クリックして **送信**します。

    ![ポスト URI と API キーの入力][image6]

6. Web アプリが表示されます、 **Web アプリの構成** 現在 web サービスの設定を含むページ。 ここで、Web アプリで使用される設定を変更できます。

    > [AZURE.NOTE] このセクションの設定を変更する変更だけにこの web アプリです。 Web サービスの既定の設定は変更されません。 変更する場合など、 **説明** ここで Machine Learning Studio で web サービス ダッシュ ボードに表示される説明は変更されません。

    完了したら、クリックして **変更を保存**, 、] をクリックし、 **ホーム ページに移動**します。

7. ホーム ページで、web サービスに送信する値を入力することができます、クリックして **送信**, 、結果が返されます。

取得する場合、 **構成** ページに移動、 `setting.aspx` web アプリのページです。 たとえば、`http://carprediction.azurewebsites.net/setting.aspx.` ページに移動します。もう一度 API キーを入力するように求められます。ページにアクセスして設定を更新するには、キーの入力が必要です。

他の Web アプリと同様に、Azure クラシック ポータルでこの Web アプリの停止、再開、または削除を行うことができます。 実行中は、ホーム Web アドレスに移動して、新しい値を入力することができます。

## Batch Execution Service (BES) テンプレートの使用方法

BES Web アプリ テンプレートは、RRS テンプレートと同じように使用できます。ただし、作成される Web アプリで複数行のデータを送信し、複数の結果を受け取ることができる点が異なります。

バッチ実行 Web サービスからの結果は、Azure ストレージ コンテナーに格納されます。入力値は、Azure Storage から取得することも、ローカル ファイルから取得することもできます。
そのため、Web アプリから返される結果を保持するための Azure ストレージ コンテナーが必要であり、入力データも準備する必要があります。

![BES Web テンプレートを使用する手順][image2]

1. RRS テンプレートと同じ手順で、BES Web アプリを作成します。ただし、以下の点が異なります。
    - 取得、 **要求の URI** から、 **バッチ実行** web サービスの API ヘルプ ページです。
    - 移動して [Azure ML バッチ実行サービス Web アプリ テンプレート](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) Azure Marketplace で BES テンプレートを開く] をクリックして **Web アプリの作成**します。

2. 結果の格納先を指定するには、Web アプリのホーム ページで格納先コンテナーの情報を入力します。 Web アプリが入力値を取得する場所として、ローカル ファイルまたは Azure ストレージ コンテナーも指定します。
クリックして **送信**します。

    ![ストレージ情報][image7]

ページが表示され、ジョブの状態が示されます。
ジョブが完了すると、Azure BLOB ストレージ内の結果の場所が表示されます。 結果をローカル ファイルにダウンロードするためのオプションも表示されます。

## BLOB の詳細

詳細については、以下のページをご覧ください。

- Machine Learning Studio での machine learning の実験を作成するを参照してください [Azure Machine Learning Studio で、最初の実験の作成](machine-learning-create-experiment.md)

- 展開する方法を web サービスとして machine learning の実験」を参照してください [Azure Machine Learning web サービスを配置](machine-learning-publish-a-machine-learning-web-service.md)

- web サービスにアクセスするには、その他の方法を参照してください [Azure Machine Learning web サービスを使用する方法](machine-learning-consume-web-services.md)


[image1]: media\machine-learning-consume-web-service-with-web-app-template\rrs-web-template-flow.png
[image2]: media\machine-learning-consume-web-service-with-web-app-template\bes-web-template-flow.png
[image3]: media\machine-learning-consume-web-service-with-web-app-template\api-key.png
[image4]: media\machine-learning-consume-web-service-with-web-app-template\post-uri.png
[image5]: media\machine-learning-consume-web-service-with-web-app-template\create-web-app.png
[image6]: media\machine-learning-consume-web-service-with-web-app-template\web-service-info.png
[image7]: media\machine-learning-consume-web-service-with-web-app-template\storage.png

