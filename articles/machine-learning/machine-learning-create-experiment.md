<properties
    pageTitle="Machine Learning Studio での簡単な実験の作成 | Microsoft Azure"
    description="機械学習に初めて取り組む方のためのチュートリアルです。Azure Machine Learning Studio で線形回帰モデルをトレーニングし、テストする単純な実験を作成する方法について説明しています。"
    keywords="experiment,linear regression,machine learning algorithms,machine learning tutorial,predictive modeling techniques"
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
    ms.topic="hero-article"
    ms.date="11/03/2015"
    ms.author="garye"/>


# Machine Learning のチュートリアル: Azure Machine Learning Studio で初めての実験を作成する

このチュートリアルでは、製造仕様と技術仕様などのさまざまな変数に基づいて自動車の価格を予測する線形回帰モデルを作成します。 これを行うには、Microsoft Azure Machine Learning Studio を使用して、簡単な予測分析の実験を開発し、繰り返します。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Machine Learning Studio の実験では、コンポーネントをキャンバスにドラッグし、それらを接続して*モデルの作成*、*モデルのトレーニング*、*モデルのスコア付けとテスト*を行います。 実験では、データを取り込んで、そのデータに対してモデルをトレーニングし、新しいデータにモデルを適用する Machine Learning Studio モジュール形式の予測モデリング手法を用います。 また、データを前処理して特徴を選択し、データをトレーニング セットとテスト セットに分割することもできます。さらに、モデルの品質を評価または相互検証したりするモジュールを追加することもできます。

Machine Learning Studio を入力してください: [https://studio.azureml.net](https://studio.azureml.net), 、] をクリックし、 **開始** ] ボタンをクリックします。 [ゲスト アクセス] を選択するか、Microsoft アカウントを使用してサインインできます。

Machine Learning Studio の概要については、を参照してください [Machine Learning Studio は何ですか?](machine-learning-what-is-ml-studio.md)。.
>[AZURE.TIP] ダウンロードして、Machine Learning Studio の機能の概要を示すダイアグラムを印刷する、次を参照してください。 [Azure Machine Learning Studio の機能の概要図](machine-learning-studio-overview-diagram.md)します。


## 実験を作成する 5 つの手順

このチュートリアルでは、Machine Learning Studio で 5 つの基本的なステップに従い、モデルを作成、トレーニング、スコア付けするための実験を構築します。

- モデルの作成
    - [手順 1: データの取得]
    - [手順 2: データを前処理する]
    - [手順 3: 特徴を定義する]
- モデルをトレーニングする
    - [手順 4: を選択し、学習アルゴリズムの適用]
- モデルのスコア付けとテストを実行する
    - [手順 5: 新しい自動車価格の予測]

## 手順 1: データを取得する

Machine Learning Studio には、多数のサンプル データセットが含まれており、多様なソースからデータをインポートできます。 この例では、含まれているサンプル データセット **自動車価格データ (未加工)** を使用します。
このデータセットには、製造仕様、モデル仕様、技術仕様などの情報を含む、多くの個別の自動車のエントリが含まれています。

1. Machine Learning Studio ウィンドウの下部にある **[+新規]** をクリックして新しい実験を開始し、**[実験]** を選択してから「**空の実験**」を選択します。 キャンバスの上部にある既定の実験名を選択して、**自動車価格の予測**など、わかりやすい名前に変更します。

2. 実験キャンバスの左側には、データセットとモジュールのパレットがあります。 このパレットの上部にある検索ボックスに「**自動車**」と入力し、**自動車価格データ (未加工)** というラベルが付いたデータセットを検索します。

 ![パレット検索
][screen1a]

3. 実験キャンバスにデータセットをドラッグします。

    ![Dataset][screen1]

このデータの中身を確認するには、自動車データセットの下部にある出力ポートをクリックし、**[視覚化]** を選択します。 データセットの変数が列として表示され、自動車の各インスタンスが行として表示されます。 最も右側の列 (列 26 の "価格") が、予想の対象となる変数です。

![データセットの視覚化
][screen1b]

右上隅の "**x**" をクリックして、視覚化ウィンドウを閉じます。

## 手順 2: データを前処理する

通常、データセットには、分析前にある程度の前処理が必要です。 さまざまな行の中に、値が不足している列があります。 モデルがデータを正しく分析するには、これらの不足値をクリーニングする必要があります。 この例では、値が見つからない行をすべて削除します。 また、見つからない値の大部分は、**正規化された損失**列にあります。したがって、モデルからこの列も一緒に除外します。
> [AZURE.TIP] 見つからない値を入力データから整理することが、ほとんどのモジュールを使用するための前提条件となっています。

まず、**正規化された損失**列を削除してから、見つからないデータがある列をすべて削除します。

1. 型 **列を投影** 検索にあるモジュール パレットの上部にある検索ボックスに、 [プロジェクト列 ][project-columns] し、モジュールにドラッグして、実験キャンバスの出力ポートに接続、 **自動車価格データ (未加工)** データセット。 このモジュールを使用すると、モデルに含める、またはモデルから除外するデータの列を選択できます。

2. 選択、 [プロジェクト列 ][project-columns] モジュールをクリック **列セレクターの起動** で、 **プロパティ** ウィンドウです。

 - フィルターのドロップダウン リスト **[が次の値で始まる]** で、**[すべての列]** が選択されていることを確認します。 これによって [プロジェクト列 ][project-columns] (私たちはから除外するもの) を除くすべての列を通過します。
 - 次の行で、**[除外]** と **[列名]** を選択し、テキスト ボックスの内側をクリックします。 列の一覧が表示されます。 **[正規化された損失]**を選ぶと、テキスト ボックスに追加されます。
 - チェック マーク (OK) ボタンをクリックして、列セレクターを閉じます。

 ![Select columns][screen3]

 **プロジェクト列**のプロパティ ウィンドウに、**正規化された損失**以外のすべての列がデータセットからフィルターされることが示されます。

 ![プロジェクト列のプロパティ
][screen4]
 > [AZURE.TIP] モジュールをダブルクリックして、テキストを入力すると、モジュールにコメントを追加できます。 これで、実験でモジュールがどのような処理をするのかがひとめでわかります。 この場合をダブルクリックして、 [プロジェクト列 ][project-columns] モジュールというコメントを入力"「を除外する正規化された損失です。

3. ドラッグ、 [欠損データのクリーニング ][clean-missing-data] モジュールを実験キャンバスし、に接続、 [プロジェクト列 ][project-columns] モジュールです。 **プロパティ** ウィンドウで、**[クリーニング モード]** の下にある **[行全体を削除]** を選択し、値が見つからない列を削除してデータを整理します。 モジュールをダブルクリックして、「値が見つからない行を削除する」というコメントを入力します。

    ![見つからないデータのクリーンアップ プロパティ][screen4a]

4. 実験キャンバスの下にある **[実行]** をクリックして、実験を実行します。

実験が終了すると、すべてのモジュールに緑色のチェック マークが付けられ、正常に完了したことが示されます。 右上隅にも **[実行が完了しました]** というステータスが表示されます。

![最初の実験を実行
][screen5]

ここまで、実験で実行したことは、データのクリーンアップだけです。 整理したデータセットを表示するには、左側の出力ポートをクリックして、 [欠損データのクリーニング ][clean-missing-data] モジュール (「整理したデータセット」) を選択 **視覚化**します。 **正規化された損失**列が、もう含まれていないことと、見つからない値がなくなったことが確認できます。

データが整理できたので、予測モデルで使用する特徴を指定する準備が整いました。

## 手順 3: 特徴を定義する

機械学習において*特徴*とは、関心のある項目を個別に測定できるプロパティです。 この例のデータセットでは、各行が 1 つの車種を表し、各列がその車種の特徴に対応します。 予測モデルの作成に役立つ特徴を見つけるには、解決する問題に関する実験と知識が必要です。 一部の特徴は、他よりも対象を予測することに優れています。 また、特徴によっては、たとえば市内での燃費と高速道路での燃費のように、他の特徴との強い相関がある場合があります。したがって、このような特徴はモデルに新しい情報をそれほど多く追加しないので、削除できます。

ここでは、データセット内の特徴のサブセットを使用するモデルを構築します。 この手順に戻って別の特徴を選択し、もう一度実験を実行して、より優れた結果が得られるかどうかを確認できます。 最初に推測でを選択、次の特徴 (列) と、 [プロジェクト列 ][project-columns] モジュールです。 モデルをトレーニングするために、予測する *price* の値を含める必要があることに注意してください。

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price

1. 別のドラッグ [プロジェクト列 ][project-columns] 実験するモジュールを選択し、キャンバスの左側の出力ポートに接続、 [欠損データのクリーニング ][clean-missing-data] モジュールです。 モジュールをダブルクリックして、「予測対象の特徴を選択する」と入力します。

2. **プロパティ** ウィンドウで、**[列セレクターの起動]** をクリックします。

3. 列セレクターで、**[が次の値で始まる]** で **[列なし]** を選択し、フィルター行で **[含む]** と **[列名]** を選択します。 列名の一覧を入力します。 これにより、モジュールでは、指定する列のみがフィルターされます。
    > [AZURE.TIP] データに対する列の定義がを介して元のデータセットから渡されました実験を実行したので、 [欠損データのクリーニング ][clean-missing-data] モジュールです。 接続すると [プロジェクト列 ][project-columns] に [欠損データのクリーニング ][clean-missing-data], 、 [プロジェクト列 ][project-columns] モジュールは、データ内の列の定義を認識します。 **[列名]** ボックスをクリックすると、列の一覧が表示され、一覧に追加する列を複数選択できます。

4. チェック マーク (OK) ボタンをクリックします。

![Select columns][screen6]

これで、次の手順の学習アルゴリズムで使用するデータセットが作成されます。 後でこの手順に戻り、別の特徴を選択して再度実行することができます。

## 手順 4: 学習アルゴリズムを選択して、適用する

データが準備できると、分析モデルの構築に必要なのは、トレーニングとテストになります。 このデータを使用してモデルのトレーニングとテストを行い、予測できる価格がどれがけ近いかを確認します。

*分類*と*回帰*は、2 種類の管理された機械学習の手法です。 分類は、色 (赤、青、または緑) のような定義された一連の値から予測するために使用します。 回帰は、人の年齢のような連続した一連の値から予測するために使用します。

ここでは、自動車の価格を予測するため、任意の値にすることができます。よって、回帰モデルを使用します。 この例では、簡単な*線形回帰*モデルをトレーニングし、次の手順でこのモデルをテストします。

1. このデータをトレーニング セットとテスト セットに分割すると、トレーニング用とテスト用の両方に使用できます。 選択し、ドラッグ、 [分割データ ][split] モジュールを実験キャンバスし、最後の出力に接続 [プロジェクト列 ][project-columns] モジュールです。 **[最初の出力データセットにおける列の割合]** を 0.75 に設定します。 このようにして、データの 75% をモデルのトレーニングに使用し、25% をテスト用に保持しておきます。
    > [AZURE.TIP] **ランダム シード** パラメーターを変更して、トレーニング用とテスト用に異なるランダムなサンプルを作成できます。 このパラメーターでは、擬似乱数ジェネレーターによる生成を制御できます。

2. 実験を実行します。 これにより、 [プロジェクト列 ][project-columns] と [分割データ ][split] モジュールに列の定義を次に追加するモジュールに渡します。

3. 学習アルゴリズムを選ぶには、キャンバスの左側にあるモジュールのパレットの **[Machine Learning]** カテゴリを展開してから、**[モデルを初期化]** を展開します。 これにより、機械学習アルゴリズムの初期化に使用できるモジュールのカテゴリが複数表示されます。

    この実験では、選択、 [線形回帰 ][linear-regression] 対象のモジュール、 **回帰** カテゴリ (することもモジュールを見つけ、パレットの検索ボックスに「線形回帰」を入力して)、実験キャンバスにドラッグします。

4. 検索し、ドラッグ、 [[モデルのトレーニング] モデルのトレーニング][train-model] モジュールを実験キャンバスにします。 左側の入力ポートの出力に接続、 [線形回帰 ][linear-regression] モジュールです。 右側の入力ポートのトレーニング データの出力 (左側のポート) を接続、 [分割データ ][split] モジュールです。

5. 選択、 [[モデルのトレーニング] モデルのトレーニング][train-model] モジュール、] をクリックして **列セレクターの起動** で、 **プロパティ** ] ウィンドウで、クリックして、 **価格** 列です。 これが、作成しているモデルで予測する値です。

 ![[価格] 列を選択
][screen7]

6. 実験を実行します。

結果として、トレーニングされた回帰モデルが作成され、新しいサンプルにスコアを付けて予測するのに使用できます。

![機械学習アルゴリズムの適用][screen8]

## 手順 5: 新しい自動車の価格を予測する

これまでにデータの 75% を使用してモデルをトレーニングしました。ここからは残りの 25% のデータにスコアを付け、モデルの機能の効果を確認します。

1. 検索し、ドラッグ、 [モデルのスコア付け ][score-model] モジュールを実験キャンバスし、左側の入力ポートの出力に接続、 [[モデルのトレーニング] モデルのトレーニング][train-model] モジュールです。 テスト データの出力 (右側のポート) の右側の入力ポートを接続、 [分割データ ][split] モジュールです。

 ![スコア モデル モジュール
][screen8a]

2. 実験を実行し、出力を表示する、 [モデルのスコア付け ][score-model] モジュールでは、出力ポートをクリックし、[ **視覚化**します。 出力に、予測された価格の値と、テスト データから既知の値が表示されます。

3. 最後に、結果の品質をテストするには、選択し、ドラッグ、 [モデルの評価 ][evaluate-model] モジュールを実験キャンバスで、左側の入力ポートの出力に接続、 [モデルのスコア付け ][score-model] モジュールです。 (2 つの入力ポートがあるため、 [モデルの評価 ][evaluate-model] モジュールは、2 つのモデルを比較するために使用できます)。

4. 実験を実行します。

出力を表示する、 [モデルの評価 ][evaluate-model] モジュールでは、出力ポートをクリックし、[ **視覚化**します。 作成したモデルに対して、以下の統計値が表示されます。

- **平均絶対誤差** (MAE): 絶対誤差の平均です (*誤差*とは、予測された値と実際の値との差です)。
- **二乗平均平方根誤差** (RMSE): テスト データセットに対して実行した予測の二乗誤差平均の平方根です。
- **相対絶対誤差**: 実際の値とすべての実際の値の平均との絶対差を基準にした絶対誤差の平均です。
- **相対二乗誤差**: 実際の値とすべての実際の値の平均との二乗差を基準にした二乗誤差の平均です。
- **決定係数**: **R-2 乗値**ともいいます。どの程度モデルが高い精度でデータと適合するかを示す統計指標です。

エラーの各統計情報を小さくすることをお勧めします。 小さい値は、予測が実際の値をより厳密に照合することを示します。 **決定係数**では、値が 1 (1.0) に近づくほど、予測の精度が高くなります。

![Evaluation results][screen9]

最終的な実験の表示は次のようになります。

![機械学習チュートリアル: 予測モデリング手法を使った線形回帰実験を作成します。][screen10]

## 次のステップ

初めての機械学習チュートリアルはこれで終わりです。実験がセットアップされたら、反復によってモデルの改善を試みることができます。 たとえば、予測で使用する特徴を変更できます。 プロパティを変更するか、 [線形回帰 ][linear-regression] アルゴリズムか、別のアルゴリズムを一緒に試します。 学習の実験にアルゴリズムを同時に複数のマシンを追加し、を使用して 2 つを比較することも、 [モデルの評価 ][evaluate-model] モジュールです。
> [AZURE.TIP] 実験キャンバスの下にある **[名前を付けて保存]** ボタンを使用して、繰り返す実験をコピーできます。 キャンバスの下にある **[実行履歴を表示]** をクリックすると、繰り返す実験をすべて表示できます。 参照してください [Azure Machine Learning Studio ][runhistory] 詳細です。

作成したモデルが期待どおりであれば、Web サービスとしてデプロイし、新しいデータを使用した自動車価格の予測に使用できます。 参照してください [Azure Machine Learning web サービスの展開 ][publish] 詳細です。

予測モデリング手法を作成、トレーニング、スコア付け、およびモデルを配置するためのより広範かつ詳細なチュートリアルは、次を参照してください。 [Azure Machine Learning ][walkthrough]します。




[step 1: get data]: #step-1-get-data 
[step 2: preprocess data]: #step-2-preprocess-data 
[step 3: define features]: #step-3-define-features 
[step 4: choose and apply a learning algorithm]: #step-4-choose-and-apply-a-learning-algorithm 
[step 5: predict new automobile prices]: #step-5-predict-new-automobile-prices 
[runhistory]: machine-learning-manage-experiment-iterations.md 
[publish]: machine-learning-publish-a-machine-learning-web-service.md 
[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md 
[screen1]: ./media/machine-learning-create-experiment/screen1.png 
[screen1a]: ./media/machine-learning-create-experiment/screen1a.png 
[screen1b]: ./media/machine-learning-create-experiment/screen1b.png 
[screen2]: ./media/machine-learning-create-experiment/screen2.png 
[screen3]: ./media/machine-learning-create-experiment/screen3.png 
[screen4]: ./media/machine-learning-create-experiment/screen4.png 
[screen4a]: ./media/machine-learning-create-experiment/screen4a.png 
[screen5]: ./media/machine-learning-create-experiment/screen5.png 
[screen6]: ./media/machine-learning-create-experiment/screen6.png 
[screen7]: ./media/machine-learning-create-experiment/screen7.png 
[screen8]: ./media/machine-learning-create-experiment/screen8.png 
[screen8a]: ./media/machine-learning-create-experiment/screen8a.png 
[screen9]: ./media/machine-learning-create-experiment/screen9.png 
[screen10]: ./media/machine-learning-create-experiment/complete-linear-regression-experiment.png 
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/ 
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/ 
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/ 
[project-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/ 
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/ 
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/ 
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/ 

