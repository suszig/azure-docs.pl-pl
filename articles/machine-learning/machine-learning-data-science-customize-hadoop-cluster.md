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


## <a name="customize"></a>Azure HDInsight Hadoop クラスターのカスタマイズ

カスタマイズされた HDInsight Hadoop クラスターを作成するには、ユーザーがログオンする必要が [**Azure クラシック ポータル**](https://manage.windowsazure.com/), 、クリックして **新規** 下部左隅にあるとを選択し、データ サービス]、[HDINSIGHT] の [ **カスタム作成** を表示、 **クラスターの詳細** ウィンドウです。 

![Create workspace][1]

構成ページ 1 で作成したクラスターの名前を入力し、他のフィールドの既定値をそのまま使用します。 矢印をクリックして、次の構成ページに移動します。 

![Create workspace][2]

[構成] ページ 2 での数を入力 **データ ノード**, を選択、 **リージョン/仮想ネットワーク**, のサイズを選択し、 **ヘッド ノードの** と **データ ノード**します。 矢印をクリックして、次の構成ページに進みます。

>[AZURE.NOTE]  **リージョン/仮想ネットワーク** HDInsight Hadoop クラスターに使用するストレージ アカウントのリージョンと同じにします。 それ以外の場合、4 番目の構成] ページで、ユーザーが使用するストレージ アカウントには表示されませんのドロップダウン リスト **アカウント名**します。

![Create workspace][3]

構成ページ 3 では、HDInsight Hadoop クラスターのユーザー名とパスワードを指定します。 **そうしない** 選択、 _Hive/oozie メタストアの入力_します。 矢印をクリックして、次の構成ページに進みます。 

![Create workspace][4]

構成ページ 4 では、ストレージ アカウント名 (HDInsight Hadoop クラスターの既定のコンテナー) を指定します。 ユーザーの選択と _既定のコンテナーの作成_ で、 **既定のコンテナー** 、クラスターを作成するには、ドロップ ダウン リストと同じ名前のコンテナーです。 矢印をクリックして、最後の構成ページに進みます。

![Create workspace][5]

最終 **スクリプトのアクション** 構成] ページでをクリックして **スクリプト アクションを追加** ボタンをクリックし、次の値がテキスト フィールドの値を入力します。
 
* **名前** -このスクリプト アクションの名前として任意の文字列です。 
* **ノード型** 選択 - **のすべてのノード**します。 
* **スクリプト URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
    * *publicscripts* ストレージ アカウント内のパブリック コンテナー 
    * *getgoing* を Azure でユーザーが作業を容易にする PowerShell スクリプトのファイル共有に使用します。 
* **パラメーター** -(空白のままに)

最後に、チェック マークをクリックして、カスタマイズされた HDInsight Hadoop クラスターの作成を開始します。 

![Create workspace][6]

## <a name="headnode"></a> Hadoop クラスターのヘッド ノードへのアクセス

ユーザーが RDP を介して Hadoop クラスターのヘッド ノードにアクセスするには、その前に Azure での Hadoop クラスターへのリモート アクセスを有効にする必要があります。 

1. ログイン、 [**Azure クラシック ポータル**](https://manage.windowsazure.com/), [ **HDInsight** 左側で、クラスターの一覧から、Hadoop クラスターを選択して、 **構成** タブをクリックし、をクリックして、 **リモートの有効化** ページの下部にあるアイコン。
    
    ![Create workspace][7]

2.  **リモート デスクトップの構成** ウィンドウでは、ユーザー名とパスワードのフィールドを入力し、リモート アクセスの有効期限日を選択します。 チェック マークをクリックして Hadoop クラスターのヘッド ノードへのリモート アクセスを有効にします。
    
    >[AZURE.NOTE] 
    >
    >1. リモート アクセスのユーザー名とパスワードは、Hadoop クラスターを作成したときに使用するユーザー名とパスワードではありません。 これらは別個の資格情報のセットです。
    >
    >2. リモート アクセスの有効期限は、現在の日付から 7 日以内にする必要があります。

    ![Create workspace][8]

3. リモート アクセスを有効にすると、クリックして **接続** ヘッド ノードにリモート ページの下部にあります。 以前に指定したリモート アクセス ユーザーの資格情報を入力して、Hadoop クラスターのヘッド ノードにログオンします。

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

 
