<properties
    pageTitle="クロスプラットフォーム Azure CLI を使用した HDInsight での Linux ベースの Hadoop、HBase、または Storm クラスターの作成 | Microsoft Azure"
    description="クロスプラットフォーム Azure CLI、Azure リソース マネージャー テンプレート、および Azure REST API を使用して Linux ベースの HDInsight クラスターを作成する方法について説明します。 クラスターの種類 (Hadoop、HBase、または Storm) を指定するか、スクリプトを使用してカスタム コンポーネントをインストールすることができます。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="11/17/2015"
    ms.author="larryfr"/>

#Azure CLI を使用した HDInsight の Linux ベースのクラスターの作成

[AZURE.INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure CLI は、Azure サービスを管理できる、プラットフォームに依存しないコマンドライン ユーティリティです。 Azure リソース管理テンプレートと共に使用して、HDInsight クラスター、関連するストレージ アカウント、その他のサービスを作成できます。

Azure のリソース管理テンプレートは、JSON ドキュメントを記述する、 __リソース グループ__ と内のすべてのリソース (HDInsight)。このテンプレート ベースのアプローチでは、HDInsight の 1 つのテンプレートで必要なすべてのリソースを定義して、全体を通じてグループへの変更を管理できます。 __展開__ グループに変更を適用します。

このドキュメントの手順では、Azure CLI とテンプレートを使用して新しい HDInsight クラスターを作成するプロセスを示します。

> [AZURE.IMPORTANT] このドキュメントの手順では、HDInsight クラスターのワーカー ノード (4) の既定の数を使用します。 クラスター作成または作成後の拡大で 32 以上の worker ノードを予定している場合、コア数が 8 個以上で RAM が 14GB 以上のサイズのヘッド ノードを選択する必要があります。
>
> ノードのサイズとそれに伴うコストに関する詳細については、次を参照してください。 [HDInsight 料金](https://azure.microsoft.com/pricing/details/hdinsight/)します。

##前提条件

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

- __Azure CLI__します。 CLI のインストール方法の詳細については、次を参照してください。 [Azure CLI をインストール](../xplat-cli-install.md)します。

##Azure サブスクリプションへのログイン

記載されている手順に従います [Azure サブスクリプションへの接続から、Azure コマンド ライン インターフェイス (Azure CLI)](../xplat-cli-connect.md) を使用して、サブスクリプションに接続し、 __ログイン__ メソッドです。

##クラスターの作成

Azure CLI をインストールして構成したら、コマンド プロンプト、シェル、またはターミナル セッションから次の手順を実行します。

1. 次のコマンドを使用して、Azure サブスクリプションに対して認証します。

        azure login

    名前とパスワードを入力するように求められます。 複数の Azure サブスクリプションがある場合は、`azure account set <subscriptionname>` を使用して、Azure CLI コマンドが使用するサブスクリプションを設定できます。

3. 次のコマンドで Azure リソース マネージャー モードに切り替えます。

        azure config mode arm

4. HDInsight クラスターのテンプレートを作成します。 基本的なテンプレートの例を次に示します。

    * [Linux ベースのクラスター、SSH パブリック キーを使用](https://github.com/Azure/azure-quickstart-templates/tree/master/hdinsight-linux-ssh-publickey)
    * [Linux ベースのクラスター、SSH アカウントのパスワードを使用](https://github.com/Azure/azure-quickstart-templates/tree/master/hdinsight-linux-ssh-password)

    これらのテンプレートのいずれでも、HDInsight が使用する既定の Azure ストレージ アカウントが作成されます。

    ファイルの必要がありますが、 __azuredeploy.json__ と __azuredeploy.parameters.json__します。 続行する前に、これらのファイルをローカルにコピーします。

5. 開いている、 __azuredeploy.parameters.json__ エディターで、ファイルおよび内の項目の値を指定、 `parameters` セクション。

    * __場所__: リソースが作成されるデータ センターです。 表示することができます、 `location` セクション、 __azuredeploy.json__ 許可されている場所の一覧のファイルです。
    * __clusterName__: HDInsight クラスターの名前。 この名前は一意にする必要があります。一意にしなければデプロイメントは失敗します。
    * __clusterStorageAccountName__: HDInsight クラスター用に作成する Azure ストレージ アカウントの名前。 この名前は一意にする必要があります。一意にしなければデプロイメントは失敗します。
    * __clusterLoginPassword__: クラスターの管理者ユーザーのパスワード。 これは Web サイトやクラスターの REST サービスのアクセスに使用されるので、安全なパスワードにする必要があります。
    * __sshUserName__: このクラスターを作成する最初の SSH ユーザーの名前。 SSH はこのアカウントでクラスターにリモート アクセスするために使用されます。
    * __sshPublicKey__: SSH 公開キーを必要とするテンプレートを使用している場合は、次の行に、公開キーを追加する必要があります。 パブリック キーを生成し、それを使用する方法については、次の記事を参照してください。

        * [Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](hdinsight-hadoop-linux-use-ssh-unix.md)
        * [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)

    * __sshPassword__: SSH パスワードが必要なテンプレートを使用している場合は、次の行にパスワードを追加する必要があります。

    完了後、ファイルを保存し、終了します。

5. 次を利用して空のリソース グループを作成します。 置換 __RESOURCEGROUPNAME__ をこのグループに使用する名前にします。 置換 __場所__ で、HDInsight クラスターを作成するデータ センターとします。

        azure group create RESOURCEGROUPNAME LOCATION
    
    > [AZURE.NOTE] 場所の名前にスペースが含まれている場合は、引用符で配置します。 たとえば、"South Central US" のようになります。

6. 次のコマンドを利用し、このリソース グループの最初のデプロイメントを作成します。 置換 __PATHTOTEMPLATE__ へのパスで、 __azuredeploy.json__ テンプレート ファイル。 置換 __PATHTOPARAMETERSFILE__ へのパスで、 __azuredeploy.parameters.json__ ファイルです。 置換 __RESOURCEGROUPNAME__ 前の手順で作成したグループの名前に置き換えます。

        azure group deployment create -f PATHTOTEMPLATE -e PATHTOPARAMETERSFILE -g RESOURCEGROUPNAME -n InitialDeployment

    デプロイメントが承認されると、"`group deployment create command ok`" のようなメッセージが表示されます。

7. 展開を完了するまでに約 15 分に多少の時間がかかる場合があります。 次のコマンドを使用して、展開に関する情報を表示できます。 置換 __RESOURCEGROUPNAME__ 前の手順で使用されるリソース グループの名前に置き換えます。

        azure group log show -l RESOURCEGROUPNAME
    
    展開が完了すると、 __ステータス__ フィールド値が含まれます __Succeeded__します。  デプロイメント中にエラーが発生した場合、次のコマンドでエラーに関する詳細を取得できます。

        azure group log show -l -v RESOURCEGROUPNAME

##次のステップ

HDInsight クラスターが正常に作成されました。次に、クラスターの使用方法について、以下のトピックをご覧ください。

###Hadoop クラスター

* [HDInsight での Hive の使用](hdinsight-use-hive.md)
* [HDInsight での Pig の使用](hdinsight-use-pig.md)
* [HDInsight での MapReduce の使用](hdinsight-use-mapreduce.md)

###HBase クラスター

* [HDInsight での HBase の使用](hdinsight-hbase-tutorial-get-started-linux.md)
* [HDInsight での HBase の Java アプリケーションの開発](hdinsight-hbase-build-java-maven-linux.md)

###Storm クラスター

* [HDInsight での Storm の Java トポロジの開発](hdinsight-storm-develop-java-topology.md)
* [HDInsight の Storm での Python コンポーネントの使用](hdinsight-storm-develop-python-topology.md)
* [HDInsight の Storm を使用したトポロジのデプロイと監視](hdinsight-storm-deploy-monitor-topology-linux.md)
