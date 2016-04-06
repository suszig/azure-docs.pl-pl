<properties
    pageTitle="ポータルを使用して Linux ベースの Hadoop、HBase、Storm クラスターを HDInsight に作成する | Microsoft Azure"
    description="Web ブラウザーと Azure ポータルを使用して、Linux ベースの Hadoop、HBase、Storm クラスターを HDInsight に作成する方法を説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="11/16/2015"
    ms.author="nitinme"/>


#Azure ポータルを使用した HDInsight の Linux ベースのクラスターの作成

[AZURE.INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure ポータルは、Microsoft Azure クラウドでホストされるサービスやリソースの Web ベースの管理ツールです。 このドキュメントでは、ポータルと Web ブラウザーを使用して、Linux ベースの HDInsight クラスターを新たに作成する際に役立つ情報を紹介しています。

##前提条件

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

- __最新の web ブラウザー__します。 Azure ポータルには、HTML5 と Javascript が使用されています。Web ブラウザーのバージョンが低いと正しく機能しない場合があります。

##クラスターの作成

1. サインイン、 [Azure ポータル](https://portal.azure.com)します。

2. クリックして **新規**, 、] をクリックして **データ分析**, 、] をクリックし、 **HDInsight**します。

    ![Azure ポータルでの新しいクラスターの作成](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.1.png "Creating a new cluster in the Azure Portal")

3. 入力、 **クラスター名**, [ **Hadoop** の **クラスターの種類**, との間、 **クラスターのオペレーティング システム** ドロップダウン リストで、[ **Ubuntu**します。 クラスターを使用できる場合は、クラスター名の横に緑色のチェック マークが表示されます。

    > [AZURE.NOTE] HBase や Storm クラスターをプロビジョニング、approprite の値を選択、 **クラスターの種類** ボックスの一覧です。

    ![クラスターの名前と種類の入力](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.2.png "Enter cluster name and type")

4. 複数のサブスクリプションの場合はクリックして、 **サブスクリプション** エントリをクラスターに使用される Azure サブスクリプションを選択します。

5. クリックして **リソース グループ** を既存のリソース グループの一覧を表示しでクラスターを作成する 1 つを選択します。 またはをクリックして **新規作成** し、新しいリソース グループの名前を入力します。 新しいグループ名を使用できる場合は、緑のチェック マークが表示されます。

    > [AZURE.NOTE] このエントリは、いずれかが使用可能な場合、既存のリソース グループのいずれかに設定されます。

6. クリックして **資格情報** 管理者ユーザーのパスワードを入力するとします。 入力する必要があります、 **SSH ユーザー名** いずれかと、 **パスワード** または **公開キー**, 、SSH ユーザーの認証に使用されます。 公開キーを使用することをお勧めします。 クリックして **選択** 下部の資格情報の構成を保存します。

    ![クラスターの資格情報の指定](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.3.png "Provide cluster credentials")

    HDInsight での SSH の使用方法の詳細については、次の記事をご覧ください。

    * [Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)


7. クリックして **データソース** クラスターの場合、既存のデータ ソースを選択するか、新しいものを作成します。

    ![[データ ソース] ブレード](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.4.png "Provide data source configuration")

    現在、HDInsight クラスターのデータ ソースとして Azure Storage アカウントを選択できます。 エントリを理解するために、次を使用して、 **データソース** ブレードです。

    - **選択方法**: これを設定 **すべてのサブスクリプションから** すべてのサブスクリプションからストレージ アカウントの参照を有効にします。 これを設定 **アクセス キー** を入力する場合、 **ストレージ名** と **アクセス キー** の既存のストレージ アカウント。

    - **ストレージ アカウントを選択]、[新規作成**: クリックして **ストレージ アカウントを選択** を参照して、クラスターに関連付ける既存のストレージ アカウントを選択します。 または、[ **新規作成** 新しいストレージ アカウントを作成します。 表示されたフィールドに、ストレージ アカウントの名前を入力します。 名前を使用できる場合は、緑色のチェック マークが表示されます。

    - **既定のコンテナーを選択して**: これを使用して、クラスターで使用する既定のコンテナーの名前を入力します。 任意の名前を入力できますが、特定のクラスターで使用されていることを簡単に認識できるように、クラスターと同じ名前を使用することをお勧めします。

    - **場所**: リージョン、ストレージ アカウントには、やで作成されます。

        > [AZURE.IMPORTANT] 既定のデータ ソースの場所を選択すると、HDInsight クラスターの場所も設定されます。 クラスターと既定のデータ ソースは、同じリージョンに存在する必要があります。

    クリックして **選択** をデータ ソースの構成を保存します。

8. クリックして **ノード料金レベル** このクラスター用に作成するノードに関する情報を表示します。 クラスターで必要な worker ノードの数を設定します。 クラスターの推定コストがブレード内に表示されます。

    ![[ノード価格レベル] ブレード](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.5.png "Specify number of cluster nodes")
    
    > [AZURE.IMPORTANT] クラスターの作成で、または作成後、クラスターを拡大/縮小して、32 を超えるワーカー ノードに計画する場合は、少なくとも 8 つのコアと 14 GB の ram のヘッド ノード サイズを選択する必要があります。
    >
    > ノードのサイズとそれに伴うコストに関する詳細については、次を参照してください。 [HDInsight 料金](https://azure.microsoft.com/pricing/details/hdinsight/)します。

    をクリックして **選択** 価格構成ノードを保存します。

9. をクリックして **オプションの構成** をクラスターのバージョンを選択できるだけでなく、その他のオプションの設定を追加するなど、 **仮想ネットワーク**, で、設定を **外部メタストア** Hive と Oozie のデータを保持するために、Script Action を使用して、カスタム コンポーネントをインストールするのには、クラスターをカスタマイズするか、クラスターで追加のストレージ アカウントを使用しています。

    * クリックして、 **HDInsight Version** ドロップダウンをクリックし、クラスターを使用するバージョンを選択します。 詳細については、次を参照してください。 [HDInsight クラスター バージョン](hdinsight-component-versioning.md)です。

    * **仮想ネットワーク**: クラスターを仮想ネットワークに配置する場合は、Azure の仮想ネットワークとサブネットを選択します。  

        ![[仮想ネットワーク] ブレード](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.6.png "Specify virtual network details")

        >[AZURE.NOTE] Windows ベースの HDInsight クラスターは、従来の仮想ネットワークにのみ配置できます。

    * クリックして **外部メタストア** を使用して、クラスターに関連付けられている Hive と Oozie メタデータを保存する SQL データベースを指定します。
    
        > [AZURE.NOTE] メタストアの構成では、HBase クラスターの種類を使用できません。

        ![[カスタム メタストア] ブレード](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.7.png "Specify external metastores")

         **Hive 用の既存の SQL DB を使用して** メタデータ、] をクリックして **はい**, SQL データベースを選択して、データベースのユーザー名とパスワードを提供します。 する場合は、次の手順を繰り返します **Oozie メタデータに対して既存の SQL DB を使用**します。 クリックして **選択** にするまで、 **オプションの構成** ブレードです。

        >[AZURE.NOTE] メタストアに使用される Azure SQL データベースには、Azure HDInsight などの Azure サービスへの接続を許可する必要があります。 Azure SQL データベース ダッシュボードの右側に表示されているサーバー名をクリックします。 これは、SQL Database インスタンスが実行されているサーバーです。 サーバー ビューに表示されたらクリックして **構成**, 、に対して **Azure Services**, 、] をクリックして **[はい]**, 、] をクリックし、 **保存**します。

    * **アクションのスクリプトを作成** として、クラスターが作成されるカスタム スクリプトを使用してクラスターをカスタマイズする場合。 スクリプト アクションの詳細については、次を参照してください。 [をカスタマイズする HDInsight クラスターの Script Action を使って](hdinsight-hadoop-customize-cluster-linux.md)します。 [Script Action] ブレードで、次の画面キャプチャに示すように、詳細を指定します。

        ![[スクリプト アクション] ブレード](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.8.png "Specify script action")

    * クリックして **Azure ストレージ キー** をクラスターに関連付ける追加のストレージ アカウントを指定します。  **Azure ストレージ キー** ブレードで、をクリックして **ストレージ キーを追加する**, 、既存のストレージ アカウントを選択するか、新しいアカウントを作成します。

        ![[追加ストレージ] ブレード](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.9.png "Specify additional storage accounts")

        クリックして **選択** にするまで、 **新しい HDInsight クラスター** ブレードです。

10.  **新しい HDInsight クラスター** ブレードで、いることを確認 **スタート画面にピン** クリックして選択すると、 **作成**します。 これでクラスターが作成され、Azure ポータルのスタート画面にクラスター用のタイルが追加されます。 アイコンはクラスターがプロビジョニング中であることを示し、プロビジョニングが完了すると、[HDInsight] アイコンを表示するように変化します。

  	| プロビジョニング中 | プロビジョニング完了 |
  	| ------------------ | --------------------- |
  	| ![スタート画面のプロビジョニング中インジケーター](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioning.png) | ![プロビジョニングされたクラスターのタイル](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioned.png) |

    > [AZURE.NOTE] クラスターを作成、通常は約 15 分間での時間がかかります。 タイルを使用して、スタート画面で、または **通知** プロビジョニング プロセスをチェックするページの左にあるエントリです。

11. プロビジョニングが完了したら、スタート画面でクラスター用のタイルをクリックして、クラスター ブレードを起動します。 クラスター ブレードには、名前、属しているリソース グループ、場所、オペレーティング システム、クラスター ダッシュボードの URL などのクラスターに関する基本情報が表示されます。

    ![クラスター ブレード](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.Cluster.Blade.png "Cluster properties")

    このブレードの上部にあるアイコンを理解して、次を使用して、 **Essentials** セクション。

    * **設定** と **すべての設定**: 表示、 **設定** ブレードで、クラスターのクラスターの詳細な構成情報にアクセスすることができます。

    * **ダッシュ ボード**, 、**クラスターのダッシュ ボード**, 、および **URL**: これらは、クラスターでジョブを実行する Web ポータルは、クラスターのダッシュ ボードにアクセスするすべての方法です。

    * **Secure Shell**: SSH を使用してクラスターにアクセスするために必要な情報です。

    * **削除**: HDInsight クラスターを削除します。

    * **クイック スタート** (![クラウドとサンダー ボルトのアイコンのクイック スタートを =](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)): のに役立つ情報を表示 HDInsight の概要です。

    * **ユーザー** (![ユーザー アイコン](./media/hdinsight-hadoop-create-linux-cluster-portal/users.png)): アクセス許可を設定することができます _ポータル管理_ 、Azure サブスクリプションに他のユーザーは、このクラスターのです。

        > [AZURE.IMPORTANT] これは、 _のみ_ アクセスと Azure ポータルでは、このクラスターへのアクセス許可に影響しへの接続または HDInsight クラスターにジョブを送信できるユーザーに影響を与えません。

    * **タグ** (![タグ アイコン](./media/hdinsight-hadoop-create-linux-cluster-portal/tags.png)): タグでは、クラウド サービスのカスタム分類を定義するキー/値ペアを設定することができます。 たとえば、という名前のキーを作成することがあります __プロジェクト__, 、し、特定のプロジェクトに関連付けられているすべてのサービスの一般的な値を使用します。

##次のステップ

HDInsight クラスターが正常に作成されました。次に、クラスターの使用方法について、以下のトピックをご覧ください。

###Hadoop クラスター

* [HDInsight での Hive の使用](hdinsight-use-hive.md)
* [HDInsight での Pig の使用](hdinsight-use-pig.md)
* [HDInsight での MapReduce の使用](hdinsight-use-mapreduce.md)

###HBase クラスター

* [HDInsight での HBase の使用](hdinsight-hbase-tutorial-get-stared-linux.md)
* [HDInsight での HBase の Java アプリケーションの開発](hdinsight-hbase-build-java-maven-linux)

###Storm クラスター

* [HDInsight での Storm の Java トポロジの開発](hdinsight-storm-develop-java-topology.md)
* [HDInsight の Storm での Python コンポーネントの使用](hdinsight-storm-develop-python.md)
* [HDInsight の Storm を使用したトポロジのデプロイと監視](hdinsight-storm-deploy-monitor-topology-linux.md)

