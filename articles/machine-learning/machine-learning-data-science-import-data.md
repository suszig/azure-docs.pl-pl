<properties
    pageTitle="Machine Learning Studio にデータをインポートする | Microsoft Azure"
    description="さまざまなデータ ソースから Azure Machine Learning Studio にデータをインポートする方法 サポートされているデータ型とデータ形式に関する説明"
    keywords="import data,data format,data types,data sources,training data"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2015"
    ms.author="garye;bradsev" />


# さまざまなデータ ソースから Azure Machine Learning Studio にトレーニング データをインポートする

## はじめに

Machine Learning Studio で独自のデータを使用して予測分析ソリューションを開発し、トレーニングする場合、次の操作を実行できます。 

- データをアップロード、 **ローカル ファイル** 前もって、ハード ディスク ドライブから、ワークスペースで、データセット モジュールを作成します。  
- いくつかのいずれかのデータにアクセス **オンライン データ ソース** を使用して、実験の実行中に、 [リーダー][リーダー] モジュールです。 
- Azure Machine learning の実験として保存から別のデータを使用して、 **データセット**します。 

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

これらの各オプションについては、上にあるメニューの各トピックを参照してください。 各トピックでは、多様なデータ ソースのデータをインポートして Machine Learning Studio で使用する方法が説明されています。 

> [AZURE.NOTE] この目的で使用できる Machine Learning Studio で利用可能な数多くのサンプル データセットができます。 詳細については、これらは、次を参照してください。 [Azure Machine Learning Studio におけるサンプル データセットの使用](machine-learning-use-sample-datasets.md))。

この概要のトピックでは、Machine Learning Studio で使用できるようにデータを準備する方法と、サポートされるデータ形式とデータ型についても説明します。 

> [AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## Azure Machine Learning Studio で使用できるようにデータを準備する
Machine Learning Studio は、区切られたテキスト データやデータベースからの構造化されたデータなど、四角形のデータや表形式のデータで作業するように設計されています。場合によっては、四角形以外のデータが使用されることもあります。

比較的クリーンなデータが最適です。 具体的には、実験にデータをアップロードする前に、引用符で囲まれていない文字列などを処理しておくことをお勧めします。

ただし、Machine Learning Studio には、実験内でデータを操作できるモジュールが用意されています。 使用する機械学習アルゴリズムに応じて、値の欠落やスパース データなどのデータ構造上の問題を処理する際に、その対処方法を決定する必要があります。そのような場合に、これらのモジュールが役に立ちます。 ファイルの場所、 **データ変換** これらの関数を実行するモジュールについては、モジュール パレットのセクションです。

実験中は、出力ポートを右クリックして、モジュールで生成されたデータをいつでも表示、ダウンロードできます。 モジュールによっては、使用可能なダウンロードのオプションが異なる場合があります。また、Machine Learning Studio 上でデータを Web ブラウザーに表示できる場合もあります。

## サポートされるデータ形式とデータ型

データのインポートに使用するメカニズムや、データの送信元に応じて、さまざまなデータ型を実験にインポートできます。

- プレーン テキスト (.txt)
- コンマ区切り値 (CSV) - ヘッダー付き (.csv) またはヘッダーなし (.nh.csv)
- タブ区切り値 (TSV) - ヘッダー付き (.tsv) またはヘッダーなし (.nh.tsv)
- Hive テーブル
- SQL データベース テーブル
- OData 値
- SVMLight データ (.svmlight) (を参照してください、 [SVMLight の定義](http://svmlight.joachims.org/) フォーマット情報を)
- 属性関係ファイル形式 (ARFF) データ (.arff) (を参照してください、 [ARFF の定義](http://weka.wikispaces.com/ARFF) フォーマット情報を)
- Zip ファイル (.zip)
- R オブジェクトまたはワークスペース ファイル (.RData)

メタデータを含む ARFF などの形式でデータをインポートする場合、Machine Learning Studio はこのメタデータを使用して各列の見出しとデータ型を定義します。

このメタデータが含まれていない TSV や CSV 形式などのデータをインポートする場合、Machine Learning Studio は、データをサンプリングすることによって、各列のデータ型を推論します。 また、データに列見出しがない場合、Machine Learning Studio は既定の名前を提供します。

明示的に指定するかを使用して列の見出しやデータ型を変更する、 [メタデータ エディター][メタデータ エディター]します。

次 **データ型** Machine Learning Studio で認識されます。

- String
- Integer
- Double
- Boolean
- DateTime
- TimeSpan

Machine Learning Studio と呼ばれる内部データ型を使用して ***データ テーブル*** モジュール間でデータを渡す。 使用してデータ テーブル形式をデータを明示的に変換できる、 [データセットへの変換][変換-データセット] モジュールです。

データ テーブル以外の形式を受け取るどのモジュールでも、次のモジュールに渡す前に、サイレント モードでデータをデータ テーブルに変換します。

必要に応じて、その他の変換モジュールを使用して CSV、TSV、ARFF、SVMLight 形式にデータ テーブル形式をもう一度変換できます。
ファイルの場所、 **Data Format Conversions** これらの関数を実行するモジュールについては、モジュール パレットのセクションです。



<!-- Module References -->
[変換-データセット]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[メタデータ エディター]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[リーダー]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

<!--HONumber=Apr16_HO2-->
