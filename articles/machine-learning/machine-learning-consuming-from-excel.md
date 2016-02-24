<properties
    pageTitle="Excel からの Machine Learning Web サービスの使用 | Microsoft Azure"
    description="Excel からの Azure Machine Learning Web サービスの使用"
    services="machine-learning"
    documentationCenter=""
    authors="LuisCabrer"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/23/2015"
    ms.author="tedway"/>


# Excel からの Azure Machine Learning Web サービスの使用

 Azure Machine Learning Studio では、Excel から直接 Web サービスを簡単に呼び出すことができ、コードを作成する必要がありません。

 実行する場合は Excel 2013 (またはそれ以降) または Excel Online と OneDrive または SharePoint で使用するためのファイルを保存する場合、 [Excel アドイン](machine-learning-excel-add-in-for-web-services.md) をお勧めします。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## 手順

1. Web サービスを発行します。 [このページ](machine-learning-walkthrough-5-publish-web-service.md) を実現する方法について説明します。 現在、Excel ブック機能は、1 つの出力を持つ要求/応答サービス (つまり 1 つのスコア付けラベル) のみサポートしています。

2. Web サービスを作成したら、クリックして、 **WEB サービス** 、studio の左側のセクションし、Excel から使用する web サービスを選択します。

3.  **ダッシュ ボード** タブでは、web サービスが 1 行のデータ、 **要求/応答** サービスです。 このサービスは、1 つの出力数がある表示されるはずです、 **Excel ブックのダウンロード** その行にリンクします。

    ![][1]

4. をクリックして **Excel ブックのダウンロード**, 、Excel で開きます。

5. セキュリティの警告が表示されます。をクリックして、 **編集の有効化** ] ボタンをクリックします。

    ![][2]

6. セキュリティの警告が表示されます。 をクリックして、 **コンテンツの有効化** 、スプレッドシートのマクロを実行するボタンをクリックします。

    ![][3]

7. マクロが有効になると、テーブルが生成されます。 青は内の列が、RRS web サービスへの入力として必要なまたは **パラメーター**します。 RRS サービスの出力に注意してください **PREDICTED VALUES** 緑色で表示します。 特定の行のすべての列がいっぱいになると、ブックは自動的に、スコア付け API を呼び出し、スコア付けされた結果を表示します。

    ![][4]

7. 複数の行のスコア付けを実行するには、2 番目の行にデータを入力すると、予測値が生成されます。 複数の行を一度に貼り付けることもできます。

8. これで、Excel 機能 (グラフ、Power Map、条件付き書式など) と予測値を使用できます。    


## ブックを共有する

マクロが機能するには、ACCESS KEY がスプレッドシートの一部である必要があります。 つまり、信頼するエンティティ/個人とだけ、ブックを共有する必要があります。

## 自動更新

RRS 呼び出しは、次の 2 つの状況で発生します。

1. すべてのページでの最初に、行がコンテンツの **パラメーター**

2. いずれかの操作をいつ、 **パラメーター** いるすべての行の変更、 **パラメーター** 入力します。

[1]: ./media/machine-learning-consuming-from-excel/excellink.png
[2]: ./media/machine-learning-consuming-from-excel/enableeditting.png
[3]: ./media/machine-learning-consuming-from-excel/enablecontent.png
[4]: ./media/machine-learning-consuming-from-excel/sampletable.png

