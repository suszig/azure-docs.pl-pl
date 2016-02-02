<properties 
    pageTitle="Cortana Analytics Process の Hadoop クラスターをカスタマイズする | Microsoft Azure" 
    description="一般的な Python モジュールは、カスタムの Azure HDInsight Hadoop クラスターで利用できます。"
    services="machine-learning" 
    documentationCenter="" 
    authors="hangzh-msft" 
    manager="paulettm" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/13/2015" 
    ms.author="hangzh;bradsev" />


# Cortana Analytics Process の Azure HDInsight Hadoop クラスターをカスタマイズする

このメニューは、Cortana Analytics Process (CAP) によって使用されるさまざまなデータ サイエンス環境の設定方法を説明するトピックにリンクしています。

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

この記事では、HDInsight サービスでクラスターをプロビジョニングするときに各ノードで 64 ビット Anaconda (Python 2.7) をインストールして、HDInsight Hadoop クラスターをカスタマイズする方法について説明します。 このカスタマイズでは、Cortana Analytics Process で使用するためのクラスターを準備します。 また、クラスターにカスタム ジョブを送信するためにヘッドノードにアクセスする方法も示します。

このカスタマイズでは、便宜上 Anaconda に含まれる多くの一般的な Python モジュールを、クラスター内のハイブ レコードを処理するように設計されたユーザー定義関数 (UDF) で使用できるようにします。 このシナリオで必要な手順については、次を参照してください。 [高度な分析プロセスでのクラスターの HDInsight の hadoop を使用して Hive クエリを送信](machine-learning-data-science-hive-queries.md)します。


## <a name="customize"></a>Azure HDInsight Hadoop クラスターをカスタマイズします。

カスタマイズされた HDInsight Hadoop クラスターを作成するには、ユーザーがログオンする必要が [* * クラシック ポータルの Azure * *](https://manage.windowsazure.com/), 、クリックして **新規** 下部左隅にあるとを選択し、データ サービス]、[HDINSIGHT] の [ **カスタム作成** を表示、 **クラスターの詳細** ウィンドウです。

![Create workspace][1]

構成ページ 1 で作成したクラスターの名前を入力し、他のフィールドの既定値をそのまま使用します。 矢印をクリックして、次の構成ページに移動します。

![Create workspace][2]

構成ページ 2 で **[データ ノード]** の番号を入力し、**[リージョン/仮想ネットワーク]** を選択して、**[ヘッド ノード]** と **[データ ノード]** のサイズを選択します。 矢印をクリックして、次の構成ページに進みます。
>[AZURE.NOTE] **[リージョン/仮想ネットワーク]** は、HDInsight Hadoop クラスターに使用するストレージ アカウントのリージョンと同じにする必要があります。 同じにしないと、4 番目の構成ページで、ユーザーが使用するストレージ アカウントが **[アカウント名]** ドロップダウン リストに表示されません。

![Create workspace][3]

構成ページ 3 では、HDInsight Hadoop クラスターのユーザー名とパスワードを指定します。 **しない** Hive/oozie メタストアの入力] は選択します。 矢印をクリックして、次の構成ページに進みます。

![ワークスペースの作成][4]

構成ページ 4 では、ストレージ アカウント名 (HDInsight Hadoop クラスターの既定のコンテナー) を指定します。 ユーザーの作成 (_c) 既定 container_ を選択する場合、 **既定のコンテナー** ドロップダウン リストと同じ名前のコンテナー、クラスターが作成されます。 矢印をクリックして、最後の構成ページに進みます。

![Create workspace][5]

最後の **[スクリプトのアクション]** 構成ページで **[スクリプト アクションの追加]** ボタンをクリックし、テキスト フィールドに次の値を入力します。

* **[名前]** - このスクリプト アクションの名前として任意の文字列を入力します。
* **[ノード タイプ]** - **[すべてのノード]** を選択します。
* **スクリプト URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*
    * *publicscripts* は、ストレージ アカウントのパブリック コンテナーです。
    * *getgoing* は、Azure でユーザーが作業しやすくなるように、PowerShell スクリプト ファイルを共有するために使用します。
* **[パラメーター]** - (空白のままにします)

最後に、チェック マークをクリックして、カスタマイズされた HDInsight Hadoop クラスターの作成を開始します。

![Create workspace][6]

## <a name="headnode"></a> Hadoop クラスターのヘッド ノードへのアクセスします。

ユーザーが RDP を介して Hadoop クラスターのヘッド ノードにアクセスするには、その前に Azure での Hadoop クラスターへのリモート アクセスを有効にする必要があります。

1. ログイン、 [* * クラシック ポータルの Azure * *](https://manage.windowsazure.com/), [ **HDInsight** 左側で、クラスターの一覧から、Hadoop クラスターを選択して、 **構成** タブをクリックし、をクリックして、 **リモートの有効化** ページの下部にあるアイコン。

    ![Create workspace][7]

2. **[リモート デスクトップの構成]** ウィンドウで、[ユーザー名]、[パスワード] の各フィールドに入力し、リモート アクセスの有効期限を選択します。 チェック マークをクリックして Hadoop クラスターのヘッド ノードへのリモート アクセスを有効にします。
    >[AZURE.NOTE] 
    >
    >1.リモート アクセスのユーザー名とパスワードは、Hadoop クラスターを作成したときに使用するユーザー名とパスワードではありません。 これらは別個の資格情報のセットです。
    >
    >2.リモート アクセスの有効期限は、現在の日付から 7 日以内にする必要があります。

    ![Create workspace][8]

3. リモート アクセスを有効にした後、ページ下部の **[接続]** をクリックして、ヘッド ノードにリモートで接続します。 以前に指定したリモート アクセス ユーザーの資格情報を入力して、Hadoop クラスターのヘッド ノードにログオンします。

     ![Create workspace][9]

高度な分析プロセスにおける次のステップがマップされて、 [学習ガイド: Azure でのデータ処理を高度な](machine-learning-data-science-advanced-data-processing.md) HDInsight にデータを移動して、Azure Machine Learning でデータから学習するための準備にサンプリングする手順が含まれています。

参照してください [高度な分析プロセスでのクラスターの HDInsight の hadoop を使用して Hive クエリを送信](machine-learning-data-science-process-hive-tables.md) ユーザーで、クラスターのヘッド ノードから Anaconda に含まれる Python モジュールにアクセスする方法については、クラスターに格納された Hive レコードの処理に使用される関数 (Udf) を定義します。


[1]: ./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png 
[2]: ./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img2.png 
[3]: ./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png 
[4]: ./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png 
[5]: ./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png 
[6]: ./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png 
[7]: ./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png 
[8]: ./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png 
[9]: ./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png 

