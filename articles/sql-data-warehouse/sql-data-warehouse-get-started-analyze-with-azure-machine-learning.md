<properties
   pageTitle="Azure Machine Learning を使用したデータの分析 | Microsoft Azure"
   description="ソリューション開発のための、Azure SQL Data Warehouse での Azure Machine Learning の使用に関するヒント。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/21/2015"
   ms.author="sahajs"/>

# Azure Machine Learning を使用したデータの分析
このチュートリアルでは、Azure SQL Data Warehouse のデータを使用して Azure Machine Learning の機械学習予測モデルを作成する方法を使用します。 このチュートリアルでは、顧客が自転車を購入する可能性があるかどうかを予測することで、Adventure Works のターゲット マーケティング キャンペーン (自転車店) を作成します。


## 前提条件
このチュートリアルを進めるには、次のものが必要です。

- AdventureWorksDW サンプル データベースが含まれた SQL Data Warehouse

[SQL Data Warehouse を作成する][] サンプル データを含むデータベースをプロビジョニングする方法を示します。 場合は、既に SQL Data Warehouse データベースがあるが、サンプル データがない、 [サンプル データを手動で読み込む][]


## 手順 1. データを取得する 
AdventureWorksDW データベースの dbo.vTargetMail ビューからデータを読み取ります。

1. サインイン [Azure Machine Learning studio][] 実験をクリックします。
2. クリックして **+ 新規** 選択 **空白の実験**します。
3. 実験の名前として「対象を絞ったマーケティング」と入力します。
4. ドラッグ、 **リーダー** モジュールをキャンバスに [モジュール] ウィンドウからです。
5. [プロパティ] ウィンドウで、SQL Data Warehouse データベースの詳細を指定します。 
6. データベースを指定 **クエリ** 関心のあるデータを読み取れません。
   
   ```
   SELECT [CustomerKey]
      ,[GeographyKey]
      ,[CustomerAlternateKey]
      ,[MaritalStatus]
      ,[Gender]
      ,cast ([YearlyIncome] as int) as SalaryYear
      ,[TotalChildren]
      ,[NumberChildrenAtHome]
      ,[EnglishEducation]
      ,[EnglishOccupation]
      ,[HouseOwnerFlag]
      ,[NumberCarsOwned]
      ,[CommuteDistance]
      ,[Region]
      ,[Age]
      ,[BikeBuyer]
  FROM [dbo].[vTargetMail]
   ```

クリックして、実験を実行 **実行** 実験キャンバスの下にあります。
![実験を実行します][1]


実験では、正常に実行が終了すると、リーダー モジュールの下部にある出力ポートをクリックし、選択 **視覚化** インポートされたデータを表示します。
![インポートされたデータを表示します。][3]





## 手順 2. データをクリーニングする
モデルとの関連性が低い列をいくつか削除します。

1. ドラッグ、 **プロジェクト列** モジュールをキャンバスにします。
2. クリックして **列セレクターの起動** 列を指定するプロパティ ウィンドウで削除します。
![プロジェクト列][4]

3. CustomerAlternateKey と GeographyKey の 2 つの列を除外します。
![不要な列を削除する][5]




## 手順 3. モデルを作成する
データを 80 対 20 に分割し、80% を機械学習モデルのトレーニングに、20% をモデルのテストに使用します。 今回の二項分類の問題には "2 クラス" アルゴリズムを使用します。

1. ドラッグ、 **分割** モジュールをキャンバスにします。
2. [プロパティ] ウィンドウの [最初の出力データセットにおける列の割合] に「0.8」を入力します。
![データをトレーニング セットとテスト セットに分割する][6]
3. ドラッグ、 **2 クラス ブースト デシジョン ツリー** モジュールをキャンバスにします。
4. ドラッグ、 **モデルのトレーニング** モジュールをキャンバスにし、入力値を指定します。 クリックして **列セレクターの起動** プロパティ ペインにします。
      - 1 つ目の入力: ML アルゴリズム。
      - 2 つ目の入力: アルゴリズムをトレーニングするためのデータ。
![[モデルのトレーニング] モジュールを接続する][7]
5. 選択、 **BikeBuyer** 列を予測する列として。
![予測する列を選択します。][8]





## 手順 4. モデルにスコアを付ける
ここでは、テスト データに対するモデルのパフォーマンスをテストします。 選択したアルゴリズムを別のアルゴリズムと比較し、どちらのパフォーマンスが優れているかを評価します。

1. ドラッグ **モデルのスコア付け** モジュールをキャンバスにします。
    最初の入力: トレーニング済みのモデル
    2 番目の入力: テスト データ
![モデルをスコアします。][9]
2. ドラッグ、 **2 クラスのベイズ ポイント マシン** 実験キャンバスにします。 このアルゴリズムのパフォーマンスを 2 クラスのブースト デシジョン ツリーのパフォーマンスと比較します。
3. [モデルのトレーニング] モジュールと [モデルのスコア付け] モジュールをコピーしてキャンバスに貼り付けます。
4. ドラッグ、 **モデルの評価** にモジュールをキャンバスに 2 つのアルゴリズムを比較します。
5. **実行** 実験します。
![実験を実行します][10]
6. [モデルの評価] モジュールの下部にある出力ポートをクリックし、[視覚化] をクリックします。
![評価結果を視覚化する][11]



指定されているメトリックは、ROC 曲線、精度/再現率図、およびリフト曲線です。 これらのメトリックを見ると、最初に実行されたモデルの方が 2 つ目のモデルよりもパフォーマンスが優れていることがわかります。 1 つ目のモデルが予測した内容を確認するには、[モデルのスコア付け] の出力ポートをクリックし、[視覚化] をクリックします。
![スコア結果を視覚化する][12]

テスト データセットに追加された 2 つの列が表示されます。

- スコア付け確率: 顧客が自転車を購入する可能性
- スコア付けラベル: モデルによって行われた分類 - 自転車を購入する顧客 (1) か、購入しない顧客 (0) このラベル付けの確率のしきい値は 50% に設定されており、調整できます。

[BikeBuyer] 列 (実際) をスコア付けラベル (予測) と比較すると、モデルのパフォーマンスがどの程度優れていたかを評価できます。 次のステップとして、このモデルを使用して新規顧客の予測を行い、Web サービスとしてこのモデルを発行したり、SQL Data Warehouse に結果を書き戻したりできます。 

予測の機械学習モデルの構築に関する詳細についてを参照してください [Azure での Machine Learning の概要][]します。



<!--Image references-->
[1]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]:./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning studio]:https://studio.azureml.net/
[Introduction to Machine Learning on Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[load sample data manually]: sql-data-warehouse-get-started-manually-load-samples.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md



