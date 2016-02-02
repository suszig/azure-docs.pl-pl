<properties
    pageTitle="Azure クラシック ポータルを使用した HDInsight での Hadoop クラスターの管理 | Microsoft Azure"
    description="HDInsight サービスを管理する方法を学習します。HDInsight クラスターを作成し、対話型 JavaScript コンソールを開いて、Hadoop コマンド コンソールを開きます。"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/04/2015"
    ms.author="jgao"/>


# Azure クラシック ポータルを使用した HDInsight での Hadoop クラスターの管理

使用して、 [Azure Classic Portal](https://manage.windowsazure.com), 、Azure HDInsight で Hadoop クラスターのプロビジョニング、Hadoop ユーザーのパスワードを変更して、クラスターで Hadoop コマンド コンソールにアクセスできるように、リモート デスクトップ プロトコル (RDP) を有効にすることができます。

[AZURE.INCLUDE [hdinsight-azure-portal](../../includes/hdinsight-azure-portal.md)]

* [Azure ポータルを使用して HDInsight で Hadoop クラスターを管理します。](hdinsight-administer-use-management-portal.md)

## HDInsight を管理するためのその他のツール

Azure クラシック ポータル以外にも、HDInsight を管理するツールが用意されています。

- Azure PowerShell を使用して HDInsight を管理する方法の詳細については、次を参照してください。 [Azure PowerShell を使用して HDInsight を管理](hdinsight-administer-use-powershell.md)します。

- Azure CLI を使用して HDInsight を管理する方法の詳細については、次を参照してください。 [Azure CLI を使用して HDInsight を管理](hdinsight-administer-use-command-line.md)します。

## 前提条件

この記事を読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- **Azure ストレージ アカウント** - HDInsight クラスターは、既定のファイル システムとして Azure BLOB ストレージ コンテナーを使用します。 Azure Blob ストレージが HDInsight クラスターにシームレスなエクスペリエンスを提供する方法の詳細については、次を参照してください。 [HDInsight で Azure Blob ストレージの使用](../hdinsight-use-blob-storage.md)します。 Azure ストレージ アカウントの作成の詳細については、「 [をストレージ アカウントを作成する方法](../storage-create-storage-account.md)します。


## HDInsight クラスターのプロビジョニング

Azure クラシック ポータルから [簡易作成] または [カスタム作成] オプションを使用して、HDInsight クラスターをプロビジョニングできます。 手順については、次の各リンクを参照してください。

- [簡易作成を使用してクラスターをプロビジョニングします。](../hdinsight-get-started.md#provision)
- [カスタム作成を使用してクラスターをプロビジョニングします。](hdinsight-provision-clusters.md#portal)

[AZURE.INCLUDE [data center list](../../includes/hdinsight-pricing-data-centers-clusters.md)]


## HDInsight クラスターのカスタマイズ

HDInsight は、広範囲の Hadoop コンポーネントで動作します。 検証されサポートされているコンポーネントの一覧を参照してください。 [Azure HDInsight でサポートされている Hadoop のバージョン](hdinsight-component-versioning.md)します。 次のいずれかのオプションを使用して、HDInsight をカスタマイズできます。

- Script Action を使用してカスタム スクリプトを実行します。これによってクラスターをカスタマイズして、クラスター構成を変更することや、Giraph や Solr などのカスタム コンポーネントをインストールすることができます。 詳細については、次を参照してください。 [HDInsight クラスターのカスタマイズ Script Action を使って](hdinsight-hadoop-customize-cluster.md)します。
- クラスターのプロビジョニング中に、HDInsight .NET SDK または Azure PowerShell でクラスター カスタマイズ パラメーターを使用します。 そうすることで、クラスターの有効期間中はこれらの構成変更が保持され、Azure プラットフォームが保守のために定期的に実行するクラスター ノードの再イメージ化の影響も受けません。 クラスター カスタマイズ パラメーターの使用に関する詳細については、次を参照してください。 [HDInsight クラスターのプロビジョニング](hdinsight-provision-clusters.md)します。
- Mahout や Cascading などの一部のネイティブ Java コンポーネントは、JAR ファイルとしてクラスター上で実行できます。 これらの JAR ファイルは、Azure BLOB ストレージに分配し、Hadoop ジョブ送信メカニズムによって HDInsight クラスターに送信できます。 詳細については、次を参照してください。 [プログラムによる Hadoop の送信ジョブ](hdinsight-submit-hadoop-jobs-programmatically.md)します。

    >[AZURE.NOTE] HDInsight クラスターへの JAR ファイルの配置に関する問題がある場合は、HDInsight クラスターでの JAR ファイルを呼び出すことにお問い合わせください [Microsoft サポート](http://azure.microsoft.com/support/options/)します。

  > Cascading は HDInsight ではサポートされておらず、Microsoft サポートの対象でもありません。 サポートされているコンポーネントの一覧は、を参照してください [HDInsight で提供されるクラスター バージョンの新機能ですか?](hdinsight-component-versioning.md)。.


リモート デスクトップ接続を使用したクラスターへのカスタム ソフトウェアのインストールはサポートされていません。 クラスターの再作成が必要な場合にファイルが失われるため、ヘッド ノードのドライブにはファイルを保存しないでください。 Azure BLOB ストレージにファイルを保存することをお勧めします。 BLOB ストレージは永続的です。

## HDInsight クラスターのユーザー名とパスワードの変更

HDInsight クラスターは、2 つのユーザー アカウントを持つことができます。 HDInsight クラスターのユーザー アカウントは、プロビジョニング処理中に作成されます。 RDP を使用してクラスターにアクセスするために、RDP ユーザー アカウントを作成することもできます。 参照してください [リモート デスクトップを有効にする](#connect-to-hdinsight-clusters-by-using-rdp)します。

**HDInsight クラスターのユーザー名とパスワードを変更するには**

1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。
2. 左側のウィンドウで **[HDINSIGHT]** をクリックします。 デプロイした HDInsight クラスターが一覧表示されます。
3. ユーザー名とパスワードをリセットする HDInsight クラスターをクリックします。
4. ページの上部にある **[構成]** をクリックします。
5. **[HADOOP サービス]** の横の **[オフ]** をクリックします。
6. ページの下部にある **[保存]** をクリックし、無効化処理の完了を待ちます。
7. サービスが無効になった後で、**[HADOOP サービス]** の横にある **[オン]** をクリックします。
8. **[ユーザー名]** と **[新しいパスワード]** に、クラスターの新しいユーザー名とパスワードをそれぞれ入力します。
8. **[保存]** をクリックします。


## RDP を使用した HDInsight クラスターへの接続

クラスターの作成時に指定したクラスターの資格情報を使用するとクラスター上のサービスにアクセスできますが、リモート デスクトップを介してクラスター自体にアクセスすることはできません。 リモート デスクトップ アクセスは既定でオフに設定されているため、リモート デスクトップを使用してクラスターに直接アクセスするには、クラスターの作成後に追加の構成をする必要があります。

**リモート デスクトップを有効にするには**

1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。
2. 左側のウィンドウで **[HDINSIGHT]** をクリックします。 デプロイした HDInsight クラスターが一覧表示されます。
3. 接続する HDInsight クラスターをクリックします。
4. ページの上部にある **[構成]** をクリックします。
5. ページの下部にある **[リモートを有効にする]** をクリックします。
6. **[リモート デスクトップの構成]** ウィザードで、リモート デスクトップのユーザー名とパスワードを入力します。 クラスターの作成時に使用したユーザー名 ([簡易作成] オプションの場合、既定で **admin**) とは異なるユーザー名を指定する必要があります。 **[有効期限]** ボックスに有効期限の日付を入力します。 有効期限は、今日から 90 日以内の将来の日付である必要があります。 有効期限の時刻は、既定で、指定した日付の真夜中と想定されます。 チェック マーク アイコンをクリックします。

    ![HDI.CreateRDPUser][image-hdi-create-rpd-user]


> [AZURE.NOTE] HDInsight .NET SDK を使用して、クラスターに対するリモート デスクトップを有効にすることもできます。 HDInsight クライアント オブジェクトで、**EnableRdp** メソッドを次の形式で使用します: **client.EnableRdp(clustername, location, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**。 同様に、クラスターに対するリモート デスクトップを無効にするには、**client.DisableRdp(clustername, location)** を使用します。 これらの方法の詳細については、次を参照してください。 [HDInsight .NET SDK のリファレンス](http://go.microsoft.com/fwlink/?LinkId=529017)します。 この方法は、Windows で実行されている HDInsight クラスターにのみ適用できます。



> [AZURE.NOTE] クラスターの RDP が有効になった後、クラスターに接続する前にページを更新する必要があります。

**RDP を使用してクラスターに接続するには**

1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。
2. 左側のウィンドウで **[HDINSIGHT]** をクリックします。 デプロイした HDInsight クラスターが一覧表示されます。
3. 接続する HDInsight クラスターをクリックします。
4. ページの上部にある **[構成]** をクリックします。
5. **[接続]** クリックし、指示に従います。

## 自己署名証明書の作成

クラスターに対し、.NET SDK を使用して何らかの操作を実行するには、ワークステーションで自己署名証明書を作成したうえで、ご利用の Azure サブスクリプションにその証明書をアップロードする必要があります。 これは 1 回限りの作業です。 証明書が有効である限り、同じ証明書を他のコンピューターにインストールすることができます。

**自己署名証明書を作成するには**

1. 要求を認証するために使用する自己署名証明書を作成します。 インターネット インフォメーション サービス (IIS) を使用するか、 [makecert](http://go.microsoft.com/fwlink/?LinkId=534000) 証明書を作成します。

2. 証明書の場所を参照して、証明書を右クリックします。**[証明書のインストール]** をクリックし、コンピューターの個人用ストアに証明書をインストールします。 証明書のプロパティを編集し、フレンドリ名を割り当てます。

3. Azure クラシック ポータルに証明書をインポートします。 ポータルで、ページの左下にある **[設定]** をクリックし、**[管理証明書]** をクリックします。 ページの下部で **[アップロード]** をクリックして指示に従い、前の手順で作成した .cer ファイルをアップロードします。

    ![HDI.ClusterCreate.UploadCert][image-hdiclustercreate-uploadcert]


## HTTP サービスのアクセス許可の付与/取り消し

HDInsight クラスターには、以下の HTTP Web サービスがあります (これらすべてのサービスには、REST ベースのエンドポイントがあります)。

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton

既定では、これらのサービスへのアクセス許可が付与されます。 アクセス権の取り消しまたは付与は、Azure クラシック ポータルから実行できます。
>[AZURE.NOTE] アクセス許可を付与するか、取り消すことで、クラスターのユーザー名とパスワードがリセットされます。

**HTTP Web サービスのアクセス許可を付与する/取り消すには**

1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。
2. 左側のウィンドウで **[HDINSIGHT]** をクリックします。 デプロイした HDInsight クラスターが一覧表示されます。
3. 構成する HDInsight クラスターをクリックします。
4. ページの上部にある **[構成]** をクリックします。
5. **[HADOOP サービス]** の横の **[オン]** または **[オフ]** をクリックします。
6. **[ユーザー名]** と **[新しいパスワード]** に、クラスターの新しいユーザー名とパスワードをそれぞれ入力します。
7. **[保存]** をクリックします。

参照してください [Azure PowerShell を使用した HDInsight の管理](hdinsight-administer-use-powershell.md)します。

## Hadoop コマンド ラインを開く

リモート デスクトップを使用してクラスターに接続して Hadoop コマンド ラインを使用するには、まず、先のセクションで説明したように、クラスターに対するリモート デスクトップ アクセスを有効にする必要があります。

**Hadoop コマンド ラインを開くには**

1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。
2. 左側のウィンドウで **[HDINSIGHT]** をクリックします。 デプロイした Hadoop クラスターが一覧表示されます。
3. 接続する HDInsight クラスターをクリックします。
3. ページの上部にある **[構成]** をクリックします。
4. ページの下部にある **[接続]** をクリックします。
5. **[開く]** をクリックします。
6. ユーザー名とパスワードを入力して **[OK]** をクリックします。 クラスターの作成時に構成したユーザー名とパスワードを使用してください。
7. **[はい]** をクリックします。
8. デスクトップで、**[Hadoop コマンド ライン]** をダブルクリックします。

    ![HDI.HadoopCommandLine][image-hadoopcommandline]

    Hadoop コマンドの詳細については、次を参照してください。 [Hadoop コマンド リファレンス](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html)します。

先のスクリーンショットで、フォルダー名には Hadoop のバージョン番号が埋め込まれています。 バージョン番号は、クラスターにインストールされている Hadoop コンポーネントのバージョンに基づいて変更できます。 Hadoop 環境変数を使用して、これらのフォルダーを参照できます。 次に例を示します。

    cd %hadoop_home%
    cd %hive_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## クラスターのスケール

参照してください [スケール Hadoop クラスターの HDInsight](hdinsight-hadoop-cluster-scaling.md)します。

## 次のステップ

この記事では、Azure クラシック ポータルを使用して HDInsight クラスターを作成する方法、および Hadoop コマンド ライン ツールを開く方法について説明しました。 詳細については、次の記事を参照してください。

* [Azure PowerShell を使用した HDInsight を管理します。](hdinsight-administer-use-powershell.md)
* [Azure CLI を使用した HDInsight を管理します。](hdinsight-administer-use-command-line.md)
* [HDInsight クラスターをプロビジョニングします。](hdinsight-provision-clusters.md)
* [プログラムによる Hadoop ジョブを送信します。](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Azure HDInsight を概要します。](../hdinsight-get-started.md)
* [Azure HDInsight で Hadoop のバージョンとは](hdinsight-component-versioning.md)


[image-hdi-create-rpd-user]: ./media/hdinsight-administer-use-management-portal-v1/hdi.createrdpuser.png 
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal-v1/hdinsight-hadoop-command-line.png "Hadoop command line"
[image-hdiclustercreate-uploadcert]: ./media/hdinsight-administer-use-management-portal-v1/hdi.clustercreate.uploadcert.png 

