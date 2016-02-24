<properties
    pageTitle="手順 3: Machine Learning の新しい実験を作成する | Microsoft Azure"
    description="予測ソリューションの開発に関するチュートリアルの中の、Azure Machine Learning Studio で新しいトレーニング実験を作成するステップの 3 番目の手順です。"
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


# チュートリアル手順 3: Azure Machine Learning の新しい実験を作成する

これは、このチュートリアルでは、3 番目の手順 [Azure ML を使用した予測ソリューションの開発](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Machine Learning ワークスペースの作成](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [既存のデータをアップロードする](machine-learning-walkthrough-2-upload-data.md)
3.  **新しい実験を作成する**
4.  [モデルをトレーニングして評価する](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Web サービスをデプロイする](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Web サービスにアクセスする](machine-learning-walkthrough-6-access-web-service.md)

----------

アップロードしたデータセットを使用する実験を ML Studio で作成する必要があります。  

1.  ML Studio で、[ **+ 新規** ウィンドウの下部にあります。
2.  選択 **実験**, 、"空の実験] を選択します。 キャンバスの上部にある既定の実験名を選択して、わかりやすい名前に変更します。

    > [AZURE.TIP] 入力することをお勧め **概要** と **説明** 実験のため、 **プロパティ** ウィンドウです。 これらのプロパティを指定しておくと、実験を文書に残すことができるので、誰でも後から見て目標と手法を理解できます。

3.  実験キャンバスの左側に、モジュール パレットで展開 **保存されたデータセット**します。
4.  下に作成したデータセットを見つける **My Datasets** キャンバスにドラッグします。 名前を入力してデータセットを検索することもできる、 **検索** パレットの上のボックスです。  

##データを準備する
データの最初の 100 行とデータセット全体の統計情報を表示するには、データセットの出力ポートをクリックして選択 **結果を表示する**です。 各列のデータ型は、ML Studio によってこの時点で特定されています。 また、データ ファイルには列見出しが含まれていないため、汎用の見出しも追加されています。  

列見出しは絶対に必要なものではありませんが、モデルのデータを操作する際に便利です。 また、最終的にモデルを Web サービスに発行する際に、見出しが設定されていることによってサービスのユーザーが列を見つけやすくなります。  

[メタデータ エディター] [メタデータ エディター] モジュールを使用して列見出しを追加します。
[メタデータ エディター] [メタデータ エディター] モジュールを使用して、データセットに関連付けられているメタデータを変更します。 この場合は、列見出しに分かりやすい名前を複数提供できます。 これを行うには、[メタデータ エディター] [メタデータ エディター] 列に追加する名前の一覧を指定し、すべての列で動作するを直接おします。

1.  モジュールのパレットに「メタデータ」を入力、 **検索** ボックス。 [メタデータ エディター] [メタデータ エディター] は、モジュールの一覧に表示されます。
2.  をクリックし、[メタデータ エディター] [メタデータ エディター] モジュールをキャンバスにドラッグして、データセットの下にドロップします。
3.  データセットに、[メタデータ エディター] [メタデータ エディター] の接続: データセットの出力ポートをクリックして、入力ポートの [メタデータ エディター] [メタデータのエディター] をドラッグして、マウス ボタンを離します。 これで、マウスをキャンバス上でどこに移動しても、データセットとモジュールが接続されたままになります。
4.  選択したら、[メタデータ エディター] [メタデータ エディター] のままで、 **プロパティ** 、キャンバスの右側のウィンドウをクリックして **列セレクターの起動**します。
5.   **列を選択して** ダイアログ ボックスで、セット、 **値で始まる** フィールドを「すべての列」です。
6.  下の行 **値で始まる** を含めるか、[メタデータ エディターの] [メタデータ エディター] を変更する特定の列を除外することができます。 すべての列を変更するので、行の右にあるマイナス記号 ("-") をクリックしてこの行を削除します。 ダイアログ ボックスは、次のようになります。
    ![選択したすべての列を含む列セレクター][4]
7.  クリックして、 **OK** チェック マークします。
8.  戻り、 **プロパティ** ] ウィンドウで、[検索対象の **新しい列名** パラメーター。 このフィールドには、コンマ区切りで列の順番どおりに、データセットの 21 列分の名前を入力します。 列名は、UCI Web サイトのデータセットに関するドキュメントから入手できます。または、以下に用意された名前をコピーして貼り付けることができます。  

<!-- try the same thing without upper-case
        Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
-->

    status of checking account, duration in months, credit history, purpose, credit amount, savings account/bond, present employment since, installment rate in percentage of disposable income, personal status and sex, other debtors, present residence since, property, age in years, other installment plans, housing, number of existing credits, job, number of people providing maintenance for, telephone, foreign worker, credit risk  

[プロパティ] ウィンドウは以下のようになります。

![Properties for Metadata Editor][1]

> [AZURE.TIP] 列見出しを検証する場合は、実験を実行 (をクリックして **実行** 実験キャンバスの下)、[メタデータ エディター] [メタデータ エディター] モジュールの出力ポートをクリックし、選択 **結果を表示する**です。 すべてのモジュールで、この方法によって実験データの進捗状況を確認することができます。

実験は以下のようになっているはずです。  

![Adding Metadata Editor][2]

##トレーニング用およびテスト用のデータセットを作成する
実験の次のステップでは、モデルのトレーニングとテストに使用するデータセットをそれぞれ生成します。 これには、ここには、[分割] [分割] モジュールを使用します。  

1.  [分割] [分割] モジュールを見つけて、キャンバスにドラッグし、最後の [メタデータ エディター] [メタデータ エディター] モジュールに接続します。
2.  分割比が 0.5 が既定では、および **ランダム分割** パラメーターを設定します。 これは、データのランダムな半分がから、[分割] [分割] モジュールの 1 つのポートと残りの半分がもう一方の出力になることを意味します。 これらを調整するだけでなく **ランダム シード** パラメーター、トレーニング データとスコア付けデータ間の比率を変更します。 この例では設定はそのままにしておきます。
    > [AZURE.TIP] 分割比では、本質的には、左側の出力ポートから出力は、データの量を決定します。 たとえば、比率を 0.7 に設定すると、データの 70% が左側のポートから、30% が右側のポートから出力されます。  

[分割] [分割] モジュールの出力が、私たちですが、それでは左側の出力をトレーニング データとして使用して、右側の出力をスコア付けデータとして使用できます。  

UCI での説明のとおり、高い信用リスクを誤って低リスクと分類すると、低い信用リスクを誤って高リスクと分類した場合の 5 倍のコストが発生します。 これに対応するため、このコストの特徴を反映した新しいデータセットを生成します。 この新しいデータセットでは、高リスクの例を 5 回重複させ、低リスクの例は重複させません。   

この重複は、R コードを使用して以下の手順で実行します。  

1.  [R スクリプトの実行] [- r のスクリプトの実行] モジュールを実験キャンバスにドラッグし、[R スクリプトの実行] [- r のスクリプトの実行] モジュールの最初の入力ポート ("Dataset1") に、[分割] [分割] モジュールの左側の出力ポートを接続します。
2.   **プロパティ** ] ウィンドウで、既定の文字列を削除、 **R スクリプトの** パラメーターと、このスクリプトを入力します。

        dataset1 <- maml.mapInputPort(1)
        data.set<-dataset1[dataset1[,21]==1,]
        pos<-dataset1[dataset1[,21]==2,]
        for (i in 1:5) data.set<-rbind(data.set,pos)
        maml.mapOutputPort("data.set")


この操作を行う同じレプリケーション各出力に対する [分割] [分割] モジュールのためが、トレーニング データとスコア付けデータ同じのコスト調整が必要です。

1.  [R スクリプトの実行] [- r のスクリプトの実行] モジュールを右クリックして **コピー**します。
2.  実験キャンバスを右クリックして **貼り付け**します。
3.  [分割] [分割] モジュールの右側の出力ポートには、この [R スクリプトの実行] [- r のスクリプトの実行] モジュールの最初の入力ポートを接続します。  

> [AZURE.TIP] R スクリプトの実行モジュールのコピーには、元のモジュールと同じスクリプトが含まれています。 モジュールをコピーしてキャンバスに貼り付けた場合、モジュールのコピーには元のモジュールのすべてのプロパティが保持されています。  
>
現在、実験は以下のようになっているはずです。

![Adding Split module and R scripts][3]

実験で R スクリプトの使用の詳細については、次を参照してください。 [実験の拡張](machine-learning-extend-your-experiment-with-r.md)します。

**次: [トレーニングし、モデルの評価](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

