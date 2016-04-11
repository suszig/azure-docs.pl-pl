<properties
    pageTitle="Machine Learning Web サービスの Excel アドイン | Microsoft Azure"
    description="コードを作成することなく、Excel から直接 Azure Machine Learning Web サービスを使用する方法。"
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="paulettm"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="11/23/2015"
    ms.author="tedway;garye" />

# Azure Machine Learning Web サービスの Excel アドイン

Excel を使用すると、コードを作成することなく、直接 Web サービスを簡単に呼び出すことができます。

## ブックで既存の Web サービスを使用する手順

1. 開いている、 [サンプル Excel ファイル](http://aka.ms/amlexcel-sample-2), 、Excel アドインとタイタニック号に乗客に関するデータが含まれています。
2. 対象の Web サービスをクリックして選択します。この例では、「Titanic Survivor Predictor (Excel Add-in Sample) [Score]」を選択します。

    ![Web サービスの選択][01]

3. この操作により、 **Predict** セクションです。  このブックには、サンプル データは、既に含まれていますが、空のブックも Excel のセルを選択して、をクリックすることが **サンプル データを使用して**します。
4. ヘッダーを含めてデータを選択し、入力データ範囲のアイコンをクリックします。  [先頭行をデータの見出しとして使用する] ボックスを必ずオンにします。
5.  **出力**, 、セルの番号を入力、出力をする例。この場合、「H1」などと入力します。
6. クリックして **予測**します。

    ![[予測] セクション][02]

## 新しい Web サービスを追加する手順

1. Web サービスを発行 ([このページ](machine-learning-walkthrough-5-publish-web-service.md) を実現する方法について説明します) か、既存の web サービスを使用します。
2. Excel に移動、 **Web サービス** セクション (の場合は、 **Predict** セクションで、[戻る] 矢印を web サービスの一覧に移動] をクリックして)。

    ![Web サービスの選択への移動][03]

3. クリックして **Web サービスの追加**します。
4. Machine Learning Studio で、クリックして、 **WEB サービス** 左側のウィンドウでセクションし、web サービスを選択します。

    ![Studio での Web サービスの選択][04]

5. Web サービスの API キーをコピーします。

    ![Studio での API キー][05]

6. Excel のアドインのテキスト ボックスの [API キーを貼り付けて **API キー**します。
7.  **ダッシュ ボード** web サービスのタブをクリックして、 **要求/応答** リンクします。
8. 検索、 **OData エンドポイント アドレス** セクションです。  URL をコピーしているというラベルのテキスト ボックスに貼り付けます **URL**します。
9. クリックして **追加**します。

    ![URL と API キー][06]

10. Web サービスを使用するには、前述の「既存の Web サービスを使用する手順」に従います。

## ブックを共有する

ブックを保存すると、追加した Web サービス用の API キーも保存されます。  このため、信頼できる個人とのみブックを共有することをお勧めします。

ご質問は上または下、 [フォーラム](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409)します。

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png

