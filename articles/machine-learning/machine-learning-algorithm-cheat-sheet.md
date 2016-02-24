<properties
    pageTitle="機械学習アルゴリズム チート シート | Microsoft Azure"
    description="印刷可能な機械学習アルゴリズム チート シートは、Azure Machine Learning Studio で予測モデルに適したアルゴリズムを選択するのに役立ちます。"
    keywords="algorithm cheat sheet,cheat sheet,machine learning algorithm"
    services="machine-learning"
    documentationCenter=""
    authors="brohrer"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2015"
    ms.author="brohrer;garye" />


# Microsoft Azure Machine Learning Studio の機械学習アルゴリズム チート シート

 **Microsoft Azure Machine Learning Algorithm Cheat Sheet** コンピューター、Microsoft Azure Machine Learning のアルゴリズム ライブラリから、予測分析ソリューションの学習アルゴリズムの右側の選択に役立ちます。

[Azure Machine Learning Studio](https://studio.azureml.net/) 多数の予測分析ソリューションの機械学習アルゴリズムが付属します。 学習のカテゴリの一般的なコンピューターにこれらのアルゴリズムが分類 ***回帰***, 、***分類***, 、***クラスタ リング***, 、および ***異常検出***, 、し、それぞれが別の種類の機械学習の問題に対処するように設計します。

> [AZURE.NOTE] 記事を参照して [を Microsoft Azure Machine Learning のアルゴリズムを選択する方法](machine-learning-algorithm-choice.md) このチート シートを使用する詳細なガイドです。

## 機械学習アルゴリズム チート シートをダウンロードします。

Machine Learning アルゴリズム チート シートをダウンロードし、ソリューションに適した機械学習アルゴリズムの選択方法を確認するのに役立てます。 チート シートをタブロイド サイズ (11 x 17 インチ) で印刷し、手元に置いておくことができます。

**チート シートをダウンロード: [Microsoft Azure Machine Learning Algorithm Cheat Sheet](http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf)**

![Machine Learning Algorithm Cheat Sheet : Machine Learning アルゴリズムの選択方法][cheat-sheet]

[cheat-sheet]: ./media/machine-learning-algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet-small_v_0_6-01.png


## アルゴリズムに関するその他のヘルプ

* さまざまな種類の機械学習アルゴリズム、その使用方法、および適切なアルゴリズムを選択するためにこのチート シートを使用する方法の詳細については、次を参照してください。 [Microsoft Azure Machine Learning のアルゴリズムを選択する方法](machine-learning-algorithm-choice.md)します。
* すべての機械学習アルゴリズムの Machine Learning Studio で利用可能なカテゴリ別の一覧、Machine Learning Studio アルゴリズムとモジュールのヘルプの [モデルの初期化] 初期化のモデルを参照してください。
* アルゴリズムと Machine Learning Studio でのモジュールの完全な一覧については [Machine Learning Studio モジュールの A ~ Z 一覧] を参照してください [a z リスト ・] Machine Learning Studio アルゴリズムとモジュールのヘルプでできます。
* ダウンロードして、Machine Learning Studio の機能の概要を示すダイアグラムを印刷する、次を参照してください。 [Azure Machine Learning Studio の機能の概要図](machine-learning-studio-overview-diagram.md)します。


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- This needs to be updated based on the new Choosing and Algorithm article

## 機械学習アルゴリズム チート シートの注意事項と用語の定義

* このアルゴリズム チート シートに示した提案は経験則です。 変化する場合や著しく異なる場合があります。 これは、開始点を提案するものです。 データ上の複数のアルゴリズム間で競合が発生しても心配しないでください。 それぞれのアルゴリズムの原則を理解し、データが生成されたシステムを理解することに代わるものはありません。

* すべての機械学習アルゴリズムが、独自のスタイルまたは *帰納的バイアス*します。 特定の問題に対しては、複数のアルゴリズムが適切な場合や、1 つのアルゴリズムが他のアルゴリズムよりも適している場合があります。 しかし、事前にどれが最適かを知ることができるとは限りません。 このような場合は、複数のアルゴリズムがチート シートに一緒に記載されています。 1 つのアルゴリズムを試してみて、結果に満足できない場合は、他のアルゴリズムを試してみるのが適切な方策でしょう。 次の例に示します、 [Cortana Analytics ギャラリー](http://gallery.azureml.net/) 実験を同じデータに対して複数のアルゴリズムをしようし、した結果を比較したときの: [多クラス分類モデルの比較: 文字認識](http://gallery.azureml.net/Details/a635502fc98b402a890efe21cec65b92)します。

* 機械学習の 3 つの主なカテゴリがあります: **教師なし学習**, 、**教師なし学習**, 、および **強化学習**します。

  *  **教師なし学習**, 、各データ ポイントのラベル付けまたはカテゴリや関心のある値に関連付けられています。  カテゴリのラベルには、たとえば '猫' または '犬' のいずれかの画像を割り当てています。  値のラベルの例は、中古車に関連付けられている販売価格です。 教師あり学習の目的は、このような多くのラベルの付いた例を学習し、将来のデータ ポイントを予測して、たとえば、新しい写真の動物を正しく識別したり、他の中古車に正しい販売価格を割り当てることができるようになることです。 これは、人気のある便利な機械学習の種類です。 すべての Azure Machine learning のモジュールは教師あり学習アルゴリズム [K 平均法クラスタ リング] [k-手段-クラスタ リング] を除く。

  *  **教師なし学習**, 、データ ポイントを使用しないため、それらに関連付けられているラベルを持ちます。 代わりに、教師なし学習アルゴリズムの目的は、いくつかの方法でデータを整理したり、その構造を記述することです。 これは、K 平均法と同様にクラスターにグループ化したり、複雑なデータを簡単に表示できるようにさまざまな表示方法を検索することを意味します。

  *  **強化学習**, 、各データ ポイントに応答アクションを選択するアルゴリズムを取得します。 これはロボット工学の一般的な手法です。ある時点での一連のセンサーの読み取りがデータ ポイントになり、アルゴリズムがロボットの次の動作を選択します。 モノのインターネット アプリケーションにも自然に適合します。 学習アルゴリズムでは、短時間で報酬信号を受信し、その決定がいかに優れていたかを示します。 アルゴリズムはこれに基に、最大の報酬を実現するために、戦略を変更します。 現時点で、Azure ML には強化学習アルゴリズム モジュールはありません。

* **ベイズ法** 統計的に独立したデータ ポイントの想定を行います。 これは、1 つのデータ ポイントでモデル化されていない変動性は、他と相関関係がないことを意味するため、予測できません。 たとえば、記録されているデータが、次の地下鉄が到着するまでの分単位の時間である場合は、1 日の間に行われた 2 つの測定値は統計的に独立しています。 ただし、1 分間に行われた 2 つの測定値は統計的に独立していません。一方の値は他の値の高い予測です。

* **ブースト デシジョン ツリー回帰** 特徴の重複や機能間の相互作用を活用します。 つまり、特定のデータ ポイントの 1 つの特徴の値は、ある程度別の値の予測であるということです。 たとえば、毎日の最高 / 最低気温データで、その日の最低気温がわかれば、最高気温を合理的に推測できます。 2 つの特徴に含まれる情報は、少し冗長です。

* データを複数の 2 つのカテゴリに分類することができます、本質的に多クラス分類モデルを使用するか、またはを実行するのに 2 つのクラス分類モデルのセットを結合、 **アンサンブル**します。 アンサンブルの手法では、クラスごとに別々 の 2 クラス分類器 - 2 つのカテゴリにデータを区切る 1 つずつ:「このクラス」と「このクラスではなく」。 これらの分類モデルは、データ ポイントの適切な割り当てに投票します。 これは、[一対全多クラス] の背後にある運用原理 [1 つの vs-全-多クラス] です。

* ロジスティック回帰やベイズ ポイント マシンなどいくつかのメソッドと見なさ **線形クラス境界**, 、つまり、あるクラス間の境界は直線行 (または一般的に超平面)。 多くの場合、これは、分離を試みるまでわからないデータの特性ですが、通常は事前に視覚化することで学習できるものです。 クラス境界が非常に不規則に見える場合は、デシジョン ツリー、デシジョン ジャングル、サポート ベクター マシン、ニューラル ネットワークを使用します。

* ニューラル ネットワークを作成することでカテゴリ変数と一緒に使用することができます、 **ダミー変数** 場所カテゴリ適用されない場合は 0 の場合 1 に、各カテゴリと設定します。

-->

<!-- This is how you can add a link to the image in HTML. Don't know how to do this in markdown.
<a href="http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet.pdf">
<img src="C:\Users\garye\azure-content-pr\articles\media\machine-learning-algorithm-cheat-sheet\cheat-sheet-small.png">
</a>
-->

<!-- Module References -->
[a-z-list]: https://msdn.microsoft.com/library/azure/dn906033.aspx
[initialize-model]: https://msdn.microsoft.com/library/azure/0c67013c-bfbc-428b-87f3-f552d8dd41f6/
[k-means-clustering]: https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/
[one-vs-all-multiclass]: https://msdn.microsoft.com/library/azure/7191efae-b4b1-4d03-a6f8-7205f87be664/

