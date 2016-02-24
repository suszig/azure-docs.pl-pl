<properties 
   pageTitle="ポータルを使用して Azure Data Lake Store で HDInsight Hadoop クラスターをプロビジョニングする | Azure" 
   description="Azure ポータルを使用して、Azure Data Lake Store で HDInsight Hadoop クラスターを構成し、使用します。" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="11/13/2015"
   ms.author="nitinme"/>

# Azure ポータルを使用して、Data Lake Store を使用する HDInsight クラスターをプロビジョニングする

> [AZURE.SELECTOR]
- [ポータルを使用してください。](data-lake-store-hdinsight-hadoop-use-portal.md)
- [PowerShell の使用](data-lake-store-hdinsight-hadoop-use-powershell.md)


Azure ポータルを使用して、Azure Data Lake Store を使用するように HDInsight クラスター (Hadoop、HBase、または Storm) を構成する方法について説明します。 このリリースに関する重要な考慮事項をいくつか以下に示します。

* **Hadoop クラスターと Storm クラスター (Windows および Linux) の**, 、Data Lake ストアは、追加のストレージ アカウントとしてのみ使用できます。 このようなクラスターの既定のストレージ アカウントは、Azure Storage BLOB (WASB) のままです。

* **HBase クラスターの場合 (Windows および Linux)**, 、データストア湖は既定のストレージまたはその他の記憶域として使用できます。


この記事では、追加のストレージとして Data Lake Store を使用して Hadoop クラスターをプロビジョニングします。 Azure ポータルを使用して Data Lake Store を使用するように HDInsight を構成するには、次の手順が必要です。

* Azure Active Directory サービス プリンシパルへの認証を使用して HDInsight クラスターを作成する
* 同じサービス プリンシパルを使用して Data Lake Store アクセスを構成する
* クラスター上でテスト ジョブを実行する

## 前提条件

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。
- **Azure サブスクリプションを有効にする** Data Lake ストアのパブリック プレビューのです。 参照してください [指示](data-lake-store-get-started-portal.md#signup)します。


## Azure Active Directory サービス プリンシパルへの認証を使用して HDInsight クラスターを作成する

このセクションでは、Data Lake Store を追加のストレージとして使用する HDInsight Hadoop クラスターを作成します。 このリリースでは、Hadoop クラスターの場合、Data Lake Store はクラスターの追加のストレージとしてのみ使用できます。 既定のストレージは、Azure Storage BLOB (WASB) のままです。 そのため、クラスターに必要なストレージ アカウントとストレージ コンテナーを最初に作成します。

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。

2. 手順に従う [作成の Hadoop クラスターの HDInsight](../hdinsight/hdinsight-provision-clusters.md#create-using-the-preview-portal) HDInsight クラスターのプロビジョニングを開始します。
 
3.  **オプションの構成** ブレードで、をクリックして **データソース**します。  **データソース** ブレードで、ストレージ アカウントとストレージ コンテナーの詳細を指定して、 **場所** として **米国東部 2**, 、] をクリックし、 **クラスター AAD Id**します。

    ![HDInsight クラスターにサービス プリンシパルを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "Add service principal to HDInsight cluster")

4.  **クラスター AAD Id** ブレードで、既存のサービス プリンシパルを選択するか、新しく作成を選択できます。 
    
    * **サービス プリンシパルの新規作成**します。  **クラスター AAD Id** ブレードで、をクリックして **[新規作成**します。 をクリックして **サービス プリンシパル**, 、し、次に、 **サービス プリンシパルの作成** ブレードで、新しいサービス プリンシパルを作成する値を指定します。 その一環として、証明書と Azure Active Directory アプリケーションも作成されます。 クリックして **作成**します。

        ![HDInsight クラスターにサービス プリンシパルを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.4.png "Add service principal to HDInsight cluster")

         **クラスター AAD Id** ブレードで、をクリックして **選択** 作成されるサービス プリンシパルを続行します。

        ![HDInsight クラスターにサービス プリンシパルを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "Add service principal to HDInsight cluster")


    * **Choose an existing Service Principal**. In the **Cluster AAD Identity** blade, click **Use existing**. Click **Service Principal**, and then in the **Select a Service Principal** blade, search for an existing service principal. Click a service principal name and then click **Select**.

        ![Add service principal to HDInsight cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "Add service principal to HDInsight cluster")

        On the **Cluster AAD Identity** blade, upload the certificate (.pfx) you created earlier and provide the password you used to create the certificate. Click **Select**. This completes the Azure Active Directory configuration for HDInsight cluster.

        ![Add service principal to HDInsight cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "Add service principal to HDInsight cluster")

6. をクリックして **選択** 上、 **データ ソース** ブレードし、クラスターのプロビジョニング」の説明に従ってを続行 [作成の Hadoop クラスターの HDInsight](../hdinsight/hdinsight-provision-clusters.md#create-using-the-preview-portal)します。

7. クラスターがプロビジョニングされたら、サービス プリンシパルが HDInsight クラスターに関連付けられていることを確認できます。 [クラスター] ブレードからクリックして **設定**, 、クリックして **クラスター AAD Id**, と関連付けられているサービス プリンシパルを表示する必要があります。

    ![HDInsight クラスターにサービス プリンシパルを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "Add service principal to HDInsight cluster")

## <a name="acl"></a>湖のデータ ストアのファイル システムにアクセスするサービス プリンシパルを構成します。

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。

2. Data Lake Store アカウントがない場合は、アカウントを作成します。 」の手順に従って [Azure ポータルを使用して Azure データ湖ストアを使ってみる](data-lake-store-get-started-portal.md)します。

    左側のウィンドウからのデータ湖ストア アカウントが既にある場合はクリックして **参照**, 、] をクリックして **湖のデータ ストア**, 、アクセス権を付与するアカウント名をクリックします。

    その Data Lake Store アカウントで、次のタスクを実行します。 

    * [データストア湖にフォルダーを作成](data-lake-store-get-started-portal.md#createfolder)します。
    * [ファイルをアップロードするデータ湖ストア](data-lake-store-get-started-portal.md#uploaddata)します。 アップロードするには、いくつかのサンプル データを探している場合は取得できます、 **救急データ** フォルダーから、 [Azure Data Lake Git リポジトリ](https://github.com/MicrosoftBigData/AzureDataLake/tree/master/SQLIPSamples/SampleData/AmbulanceData)します。

    アップロードされたファイルは、後で HDInsight クラスターで Data Lake Store アカウントをテストする際に使用します。

3. 湖のデータ ストア] ブレードで [ **データ エクスプ ローラー**します。

    ![データ エクスプローラー](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.start.data.explorer.png "Data Explorer")

4.  **データ エクスプ ローラー** ブレードで、自分のアカウントのルートをクリック、[アカウント] ブレードで、 **アクセス** アイコン。

    ![Data Lake ファイル システムに ACL を設定する](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.acl.1.png "Set ACLs on Data Lake file system")

5.  **アクセス** ブレードが標準的なアクセスと、ルートに既に割り当てられているカスタムのアクセスを一覧表示します。 クリックして、 **追加** アイコン レベルのカスタマイズの Acl を追加し、先ほど作成したサービス プリンシパルを含めます。

    ![標準アクセスとカスタム アクセスを一覧表示する](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.acl.2.png "List standard and custom access")

6. クリックして、 **追加** を開くアイコン、 **カスタム アクセスの追加** ブレードです。 このブレードで、[ **[ユーザーまたはグループ**, 、し **[ユーザーまたはグループ** ブレードで、Azure Active Directory で作成したサービス プリンシパルを検索します。 前に作成したサービス プリンシパルの名前は **HDIADL**します。 サービス プリンシパル名をクリックし、クリックして **選択**します。

    ![グループを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.acl.3.png "Add a group")

7. をクリックして **アクセス許可を選択して**, 、サービス プリンシパルに割り当てる] をクリックするアクセス許可を選択 **OK**します。

    ![グループにアクセス許可を割り当てる](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.acl.4.png "Assign permissions to group")

8.  **カスタム アクセスの追加** ブレードで、をクリックして **[ok]**します。 関連付けられているアクセス許可を持つ、新しく追加されたグループが表示されるよう、 **アクセス** ブレードです。

    ![グループにアクセス許可を割り当てる](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.acl.5.png "Assign permissions to group")

7. 必要に応じて、サービス プリンシパルを追加した後で、アクセス許可を変更することもできます。 各種類のアクセス許可 (Read、Write、Execute) のチェック ボックスをオフにするかオンにするかは、そのアクセス許可を割り当てるか、または削除するかに応じて決定します。 をクリックして **保存** 、変更を保存または **破棄** 変更を元に戻す。



## Azure Data Lake Store を使用する HDInsight クラスターでテスト ジョブを実行する

HDInsight クラスターを構成したら、クラスターでテスト ジョブを実行して、HDInsight クラスターが Azure Data Lake Store のデータにアクセス可能であるかどうかをテストできます。 そのためには、Data Lake Store を対象とするいくつかの Hive クエリを実行します。

1. プロビジョニングしたクラスターのクラスターのブレードを開き、をクリックし、 **ダッシュ ボード**します。

    ![クラスター ダッシュボードの起動](./media/data-lake-store-hdinsight-hadoop-use-portal/hdiadlcluster1.png "Launch cluster dashboard")

    入力を求められたら、クラスターの管理者資格情報を入力します。

2. これにより、Microsoft Azure HDInsight クエリ コンソールが開きます。 クリックして **Hive エディター**します。

    ![Hive エディターを開く](./media/data-lake-store-hdinsight-hadoop-use-portal/hdiadlcluster2.png "Open Hive editor")

3. Hive エディターで、次のクエリを入力し、クリックして **送信**します。

        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder'

    この Hive クエリで、`adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder` の Data Lake Store に格納されているデータからテーブルを作成します。 この場所には、前にアップロードしたサンプル データ ファイルが含まれます。 

     **ジョブ セッション** 下にあるテーブルから変更するジョブの状態を表示する **Initializing**, を **を実行している**, を **完了**します。 クリックすることも **の詳細の表示** 完了したジョブに関する詳細を表示します。

    ![テーブルを作成する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdiadlcluster3.png "Create table")

4. 次のクエリを実行して、テーブルが作成されたことを確認します。

        SHOW TABLES;

    このクエリに対応する [詳細の表示] をクリックすると、出力が次のように表示されます。

        hivesampletable
        vehicles

    **車両** 前に作成したテーブルです。 **hivesampletable** は、テーブルのサンプルを既定ですべての HDInsight クラスターで使用できます。

5. データを取得するクエリを実行することも、 **車両**します。

        SELECT * FROM vehicles LIMIT 5;

## HDFS コマンドを使用して Data Lake Store にアクセスする

Data Lake Store を使用するように HDInsight クラスターを構成したら、HDFS シェル コマンドを使用してストアにアクセスできます。

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。

2. をクリックして **参照**, 、] をクリックして **HDInsight クラスター**, 、作成した HDInsight クラスターをクリックします。

3. クラスター] ブレードで [ **リモート デスクトップ**, 、し、次に、 **リモート デスクトップ** ブレードで、をクリックして **接続**します。

    ![HDI クラスターにリモートから接続する](./media/data-lake-store-hdinsight-hadoop-use-portal/ADL.HDI.PS.Remote.Desktop.png "Create an Azure Resource Group")

    メッセージが表示されたら、リモート デスクトップ ユーザーに対して指定した資格情報を入力します。 

4. リモート セッションで、Windows PowerShell を起動し、HDFS ファイル システムのコマンドを使用して、Azure Data Lake Store のファイルを一覧表示します。

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    これにより、以前に Data Lake Store にアップロードしたファイルが一覧表示されます。

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

    `hdfs dfs -put` コマンドを使用して Data Lake Store にいくつかのファイルをアップロードし、`hdfs dfs -ls` を使用してファイルが正常にアップロードされたかどうかを確認することもできます。

## Data Lake Store を既定のストレージとして使用する HBase クラスターのプロビジョニングに関する考慮事項

HBase クラスターでは、Data Lake Store アカウントを既定のストレージとして使用できます。 サービス プリンシパルが、クラスターに関連付けられている場合はそのため、クラスターを正常にプロビジョニングするには **必要があります** 湖のデータ ストアのアカウントにアクセスします。 このことは次の 2 つの方法で確認できます。

* **既存のサービス プリンシパルを使用する場合**, 、クラスターのプロビジョニングを開始する前に、サービス プリンシパルを湖のデータ ストアのファイル システムのルート レベルに ACL に追加されていることを確認する必要があります。
* **サービス プリンシパルの新規作成することを選択する場合は、** クラスターのプロビジョニングの一環として、クラスターでは、プロビジョニングが起動するとすぐにする必要があります、新しく作成したサービス プリンシパルを追加湖のデータ ストアのファイル システムのルート レベルです。 追加できなかった場合、クラスターはプロビジョニングされますが、HBase サービスは開始できません。 この問題を回避するには、サービス プリンシパルを Data Lake Store アカウントの ACL に追加し、Ambari Web UI を使用して HBase サービスを再起動します。

湖のデータ ストアのファイル システムにサービス プリンシパルを追加する方法については、次を参照してください。 [湖のデータ ストアのファイル システムへのアクセスを構成するサービス プリンシパル](#acl)します。



## 関連項目

* [Azure PowerShell を使用して、Data Lake Store を使用する HDInsight クラスターをプロビジョニングする](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

