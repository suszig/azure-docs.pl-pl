<properties
    pageTitle="ローカル ファイルのデータを Machine Learning Studio にインポートする | Microsoft Azure"
    description="ローカル ファイルのトレーニング データを Azure Machine Learning Studio にインポートする方法。"
    keywords="import data,data format,data types,data sources,training data"
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
    ms.date="10/07/2015"
    ms.author="garye;bradsev" />


# さまざまなデータ ソースから Azure Machine Learning Studio にトレーニング データをインポートする

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]


独自のデータを Machine Learning Studio で使用するために、事前にローカル ハード ドライブからデータ ファイルをアップロードして、データセット モジュールをワークスペースで作成できます。 


## ローカル ファイルからデータをインポートする

次の手順に従って、ローカルのハード ドライブからデータをインポートできます。

1. クリックして **+ 新規** Machine Learning Studio ウィンドウの下部にあります。
2. 選択 **データセット** と **ローカル ファイルから**します。
3.  **新しいデータセットをアップロード** ダイアログ ボックスで、アップロードするファイルを参照
4. 名前を入力し、データ型を指定したら、必要に応じて説明を入力します。 データの特徴を記録しておくと、後でデータを使用する際に参照できるため、説明を入力しておくことをお勧めします。
5. チェック ボックスを **これは、既存のデータセットの新しいバージョン** 新しいデータで既存のデータセットを更新することができます。 このチェックボックスをクリックし、既存のデータセット名を入力するだけです。

アップロードする際に、ファイルがアップロードされていることを示すメッセージが表示されます。 アップロード時間は、データのサイズと、サービスへの接続速度に依存します。
ファイルのアップロードに時間がかかる場合、待機している間に Machine Learning Studio で他の作業も実行できます。 ただし、ブラウザーを閉じるとデータのアップロードに失敗します。

データがアップロードされると、データセット モジュールに格納され、ワークスペース内のすべての実験で使用できるようになります。
あらかじめ読み込まれているサンプル データセットの場合は、データセットを表示することができます、 **保存されたデータセット** 実験の編集中に、モジュール パレットの一覧です。 データ セットを別の分析と機械学習のために使用する場合、実験キャンバスのデータセットをドラッグ アンド ドロップできます。



