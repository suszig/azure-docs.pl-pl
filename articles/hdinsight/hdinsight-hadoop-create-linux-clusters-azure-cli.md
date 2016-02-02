<properties
    pageTitle="クロスプラットフォーム Azure CLI を使用した HDInsight での Linux ベースの Hadoop、HBase、または Storm クラスターの作成 | Microsoft Azure"
    description="クロスプラットフォーム Azure CLI、Azure リソース マネージャー テンプレート、および Azure REST API を使用して Linux ベースの HDInsight クラスターを作成する方法について説明します。クラスターの種類 (Hadoop、HBase、または Storm) を指定するか、スクリプトを使用してカスタム コンポーネントをインストールすることができます。"
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


# Azure CLI を使用した HDInsight の Linux ベースのクラスターの作成

[AZURE.INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure CLI は、Azure サービスを管理できる、プラットフォームに依存しないコマンドライン ユーティリティです。 Azure リソース管理テンプレートと共に使用して、HDInsight クラスター、関連するストレージ アカウント、その他のサービスを作成できます。

Azure リソース管理テンプレートは、__リソース グループ__とその中のすべてのリソース (HDInsight など) について記述する JSON ドキュメントです。 このテンプレート基準の手法では、1 つのテンプレートで HDInsight に必要なすべてのリソースを定義でき、グループに変更を適用する__デプロイメント__を介して、グループに対する変更をまとめて管理できます。

このドキュメントの手順では、Azure CLI とテンプレートを使用して新しい HDInsight クラスターを作成するプロセスを示します。
> [AZURE.IMPORTANT] この文書の手順では、HDInsight クラスターにワーカー ノードの既定数 (4) を使用します。 クラスター作成または作成後の拡大で 32 以上の worker ノードを予定している場合、コア数が 8 個以上で RAM が 14GB 以上のサイズのヘッド ノードを選択する必要があります。
>
> ノードのサイズとそれに伴うコストに関する詳細については、次を参照してください。 [HDInsight 料金](https://azure.microsoft.com/pricing/details/hdinsight/)します。

## 前提条件

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

- __Azure CLI__。 CLI のインストール方法の詳細については、次を参照してください。 [Azure CLI をインストール](../xplat-cli-install.md)します。

## Azure サブスクリプションへのログイン

記載されている手順に従います [Azure サブスクリプションへの接続から、Azure コマンド ライン インターフェイス (Azure CLI)](../xplat-cli-connect.md) を使用して、サブスクリプションに接続し、 __ログイン__ メソッドです。

## クラスターの作成

Azure CLI をインストールして構成したら、コマンド プロンプト、シェル、またはターミナル セッションから次の手順を実行します。

1. 次のコマンドを使用して、Azure サブスクリプションに対して認証します。

        azure login

    名前とパスワードを入力するように求められます。複数の Azure サブスクリプションがあればは、使って `azure アカウントの設定 < subscriptionname >` Azure CLI コマンドを使用するサブスクリプションを設定します。

3. 次のコマンドで Azure リソース マネージャー モードに切り替えます。

        azure config mode arm

4. HDInsight クラスターのテンプレートを作成します。 基本的なテンプレートの例を次に示します。

    * [SSH 公開キーを使用して、Linux ベースのクラスター](https://github.com/Azure/azure-quickstart-templates/tree/master/hdinsight-linux-ssh-publickey)
    * [SSH アカウントのパスワードを使用して Linux ベースのクラスター](https://github.com/Azure/azure-quickstart-templates/tree/master/hdinsight-linux-ssh-password)

    これらのテンプレートのいずれでも、HDInsight が使用する既定の Azure ストレージ アカウントが作成されます。

    必要なファイルは、__azuredeploy.json__ と __azuredeploy.parameters.json__ です。 続行する前に、これらのファイルをローカルにコピーします。

5. 開いている、 __azuredeploy.parameters.json__ エディターで、ファイルおよび内の項目の値を指定、 `パラメーター` セクション。

    * __location__: リソースが作成されるデータ センター。 表示することができます、 `場所` セクション、 __azuredeploy.json__ 許可されている場所の一覧のファイルです。
    * __clusterName__: HDInsight クラスターの名前。 この名前は一意にする必要があります。一意にしなければデプロイメントは失敗します。
    * __clusterStorageAccountName__: HDInsight に対して作成される Azure Storage のアカウント名。 この名前は一意にする必要があります。一意にしなければデプロイメントは失敗します。
    * __clusterLoginPassword__: クラスターの管理者ユーザーのパスワード。 これは Web サイトやクラスターの REST サービスのアクセスに使用されるので、安全なパスワードにする必要があります。
    * __sshUserName__: このクラスターに作成する最初の SSH ユーザーの名前。 SSH はこのアカウントでクラスターにリモート アクセスするために使用されます。
    * __sshPublicKey__: SSH 公開キーを必要とするテンプレートを使用している場合、この行に公開キーを追加する必要があります。 パブリック キーを生成し、それを使用する方法については、次の記事を参照してください。

        * [Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-unix.md)
        * [Windows から Linux ベースの Hadoop で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-windows.md)

    * __sshPassword__: SSH パスワードを必要とするテンプレートを使用している場合、この行にパスワードを追加する必要があります。

    完了後、ファイルを保存し、終了します。

5. 次を利用して空のリソース グループを作成します。 __RESOURCEGROUPNAME__ をこのグループに使用する名前に置き換えます。 __LOCATION__ を HDInsight クラスターを作成するデータ センターに置き換えます。

        azure group create RESOURCEGROUPNAME LOCATION

    > [AZURE.NOTE] 場所の名前にスペースが含まれる場合は、名前を引用符で囲みます。 たとえば、"South Central US" のようになります。

6. 次のコマンドを利用し、このリソース グループの最初のデプロイメントを作成します。 __PATHTOTEMPLATE__ を __azuredeploy.json__ テンプレート ファイルのパスに置き換えます。 __PATHTOPARAMETERSFILE__ を __azuredeploy.parameters.json__ ファイルのパスに置き換えます。 __RESOURCEGROUPNAME__ を前の手順で作成したグループの名前に置き換えます。

        azure group deployment create -f PATHTOTEMPLATE -e PATHTOPARAMETERSFILE -g RESOURCEGROUPNAME -n InitialDeployment

    展開が受け入れられるのようなメッセージが表示されます `グループの展開コマンドを作成する [ok]`します。

7. 展開を完了するまでに約 15 分に多少の時間がかかる場合があります。 次のコマンドを使用して、展開に関する情報を表示できます。 __RESOURCEGROUPNAME__ を前の手順で使用したリソース グループの名前に置き換えます。

        azure group log show -l RESOURCEGROUPNAME

    デプロイメントが完了すると、__Status__ フィールドに __Succeeded__.という値が表示されます。 デプロイメント中にエラーが発生した場合、次のコマンドでエラーに関する詳細を取得できます。

        azure group log show -l -v RESOURCEGROUPNAME


## 次のステップ

HDInsight クラスターが正常に作成されました。次に、クラスターの使用方法について、以下のトピックをご覧ください。

### Hadoop クラスター

* [HDInsight での Hive を使用します。](hdinsight-use-hive.md)
* [HDInsight での Pig を使用します。](hdinsight-use-pig.md)
* [HDInsight での MapReduce を使用します。](hdinsight-use-mapreduce.md)

### HBase クラスター

* [HDInsight での HBase を使用します。](hdinsight-hbase-tutorial-get-started-linux.md)
* [HDInsight での HBase の Java アプリケーションを開発します。](hdinsight-hbase-build-java-maven-linux.md)

### Storm クラスター

* [HDInsight で Storm の Java トポロジを開発します。](hdinsight-storm-develop-java-topology.md)
* [HDInsight での Storm で Python コンポーネントを使用します。](hdinsight-storm-develop-python-topology.md)
* [展開および HDInsight での Storm トポロジの監視](hdinsight-storm-deploy-monitor-topology-linux.md)




