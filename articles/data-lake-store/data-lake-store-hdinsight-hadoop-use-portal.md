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
- [Using Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Using PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)



Azure ポータルを使用して、Azure Data Lake Store を使用するように HDInsight クラスター (Hadoop、HBase、または Storm) を構成する方法について説明します。 このリリースに関する重要な考慮事項をいくつか以下に示します。

* **Hadoop クラスターと Storm クラスター (Windows および Linux) の場合**、Data Lake Store は、追加のストレージ アカウントとしてのみ使用できます。 このようなクラスターの既定のストレージ アカウントは、Azure ストレージ BLOB (WASB) のままです。

* **HBase クラスター (Windows および Linux) の場合**、Data Lake Store を既定のストレージまたは追加ストレージとして使用できます。


この記事では、追加のストレージとして Data Lake Store を使用して Hadoop クラスターをプロビジョニングします。 Azure ポータルを使用して Data Lake Store を使用するように HDInsight を構成するには、次の手順が必要です。

* Azure Active Directory サービス プリンシパルへの認証を使用して HDInsight クラスターを作成する
* 同じサービス プリンシパルを使用して Data Lake Store アクセスを構成する
* クラスター上でテスト ジョブを実行する

## 前提条件

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。
- Data Lake Store パブリック プレビューに対して、**Azure サブスクリプションを有効にする**。 参照してください [指示](data-lake-store-get-started-portal.md#signup)します。


## Azure Active Directory サービス プリンシパルへの認証を使用して HDInsight クラスターを作成する

このセクションでは、Data Lake Store を追加のストレージとして使用する HDInsight Hadoop クラスターを作成します。 このリリースでは、Hadoop クラスターの場合、Data Lake Store はクラスターの追加のストレージとしてのみ使用できます。 既定のストレージは、Azure Storage BLOB (WASB) のままです。 そのため、クラスターに必要なストレージ アカウントとストレージ コンテナーを最初に作成します。

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。

2. 手順に従う [作成の Hadoop クラスターの HDInsight](../hdinsight/hdinsight-provision-clusters.md#create-using-the-preview-portal) HDInsight クラスターのプロビジョニングを開始します。

3. **[オプションの構成]** ブレードで、**[データ ソース]** をクリックします。 **[データ ソース]** ブレードでストレージ アカウントおよびストレージ コンテナーの詳細を指定し、**[場所]** を **[East US 2]** として指定して、**[クラスター AAD ID]** をクリックします。

    ![HDInsight クラスターにサービス プリンシパルを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "Add service principal to HDInsight cluster")

4. **[クラスター AAD ID]** ブレードで、既存のサービス プリンシパルを選択するか、または新しいサービス プリンシパルを作成することができます。

    * **新しいサービス プリンシパルを作成する:**  **[クラスター AAD ID]** ブレードで、**[新規作成]** をクリックします。 **[サービス プリンシパル]** をクリックし、**[サービス プリンシパルの作成]** ブレードで、新しいサービス プリンシパルを作成するための値を指定します。 その一環として、証明書と Azure Active Directory アプリケーションも作成されます。 **[作成]** をクリックします。

        ![HDInsight クラスターにサービス プリンシパルを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.4.png "Add service principal to HDInsight cluster")

        **[クラスター AAD ID]** ブレードで **[選択]** をクリックして、サービス プリンシパルの作成を続行します。

        ![HDInsight クラスターにサービス プリンシパルを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "Add service principal to HDInsight cluster")

    * **既存のサービス プリンシパルを選択する:**  **[クラスター AAD ID]** ブレードで、**[既存のものを使用]** をクリックします。 **[サービス プリンシパル]** をクリックし、**[サービス プリンシパルの選択]** ブレードで既存のサービス プリンシパルを探します。 サービス プリンシパルの名前をクリックし、**[選択]** をクリックします。

        ![HDInsight クラスターにサービス プリンシパルを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "Add service principal to HDInsight cluster")

        **[クラスター AAD ID]** ブレードで、前に作成した証明書 (.pfx) をアップロードし、証明書の作成に使用したパスワードを指定します。 **[選択]** をクリックします。 これで、HDInsight クラスターでの Azure Active Directory の構成が完了しました。

        ![HDInsight クラスターにサービス プリンシパルを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "Add service principal to HDInsight cluster")

6. をクリックして **選択** 上、 **データ ソース** ブレードし、クラスターのプロビジョニング」の説明に従ってを続行 [作成の Hadoop クラスターの HDInsight](../hdinsight/hdinsight-provision-clusters.md#create-using-the-preview-portal)します。

7. クラスターがプロビジョニングされたら、サービス プリンシパルが HDInsight クラスターに関連付けられていることを確認できます。 そのためには、クラスター ブレードで **[設定]** をクリックし、**[クラスター AAD ID]** をクリックして、関連付けられているサービス プリンシパルを確認します。

    ![HDInsight クラスターにサービス プリンシパルを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "Add service principal to HDInsight cluster")

## <a name="acl"></a>湖のデータ ストアのファイル システムにアクセスするサービス プリンシパルを構成します。

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。

2. Data Lake Store アカウントがない場合は、アカウントを作成します。 」の手順に従って [Azure ポータルを使用して Azure データ湖ストアを使ってみる](data-lake-store-get-started-portal.md)します。

    Data Lake Store アカウントが既にある場合は、左側のウィンドウで **[参照]**、**[Data Lake Store]** の順にクリックし、アクセスを許可するアカウントの名前をクリックします。

    その Data Lake Store アカウントで、次のタスクを実行します。

    * [データストア湖にフォルダーを作成](data-lake-store-get-started-portal.md#createfolder)します。
    * [ファイルをアップロードするデータ湖ストア](data-lake-store-get-started-portal.md#uploaddata)します。 アップロードするには、いくつかのサンプル データを探している場合は取得できます、 **救急データ** フォルダーから、 [Azure Data Lake Git リポジトリ](https://github.com/MicrosoftBigData/AzureDataLake/tree/master/SQLIPSamples/SampleData/AmbulanceData)します。

    アップロードされたファイルは、後で HDInsight クラスターで Data Lake Store アカウントをテストする際に使用します。

3. Data Lake Store ブレードで、**[データ エクスプローラー]** をクリックします。

    ![データ エクスプローラー](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.start.data.explorer.png "Data Explorer")

4. **[データ エクスプローラー]** ブレードで、アカウントのルートをクリックし、アカウント ブレードの **[アクセス]** アイコンをクリックします。

    ![Data Lake ファイル システムに ACL を設定する](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.acl.1.png "Set ACLs on Data Lake file system")

5. **[アクセス]** ブレードには、既にルートに割り当てられている標準アクセスとカスタム アクセスが一覧表示されます。 **[追加]** アイコンをクリックして、カスタムレベルの ACL を追加し、前に作成したサービス プリンシパルを含めます。

    ![標準アクセスとカスタム アクセスを一覧表示する](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.acl.2.png "List standard and custom access")

6. **[追加]** アイコンをクリックして、**[カスタム アクセスの追加]** ブレードを開きます。 このブレードで **[ユーザーまたはグループの選択]** をクリックし、**[ユーザーまたはグループの選択]** ブレードで、前に Azure Active Directory で作成したサービス プリンシパルを探します。 前に作成したサービス プリンシパルの名前は **HDIADL** です。 サービス プリンシパルの名前をクリックし、**[選択]** をクリックします。

    ![グループを追加する](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.acl.3.png "Add a group")

7. **[アクセス許可の選択]** をクリックし、そのサービス プリンシパルに割り当てるアクセス許可を選択して、**[OK]** をクリックします。

    ![グループにアクセス許可を割り当てる](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.acl.4.png "Assign permissions to group")

8. **[カスタム アクセスの追加]** ブレードで **[OK]** をクリックします。 新しく追加されたグループは、関連付けられたアクセス許可と一緒に **[アクセス]** ブレードに一覧表示されます。

    ![グループにアクセス許可を割り当てる](./media/data-lake-store-hdinsight-hadoop-use-portal/adl.acl.5.png "Assign permissions to group")

7. 必要に応じて、サービス プリンシパルを追加した後で、アクセス許可を変更することもできます。 各種類のアクセス許可 (Read、Write、Execute) のチェック ボックスをオフにするかオンにするかは、そのアクセス許可を割り当てるか、または削除するかに応じて決定します。 **[保存]** をクリックして変更を保存するか、**[破棄]** をクリックして変更を元に戻します。



## Azure Data Lake Store を使用する HDInsight クラスターでテスト ジョブを実行する

HDInsight クラスターを構成したら、クラスターでテスト ジョブを実行して、HDInsight クラスターが Azure Data Lake Store のデータにアクセス可能であるかどうかをテストできます。 そのためには、Data Lake Store を対象とするいくつかの Hive クエリを実行します。

1. プロビジョニングしたクラスターのクラスター ブレードを開き、**[ダッシュボード]** をクリックします。

    ![クラスター ダッシュボードの起動](./media/data-lake-store-hdinsight-hadoop-use-portal/hdiadlcluster1.png "Launch cluster dashboard")

    入力を求められたら、クラスターの管理者資格情報を入力します。

2. これにより、Microsoft Azure HDInsight クエリ コンソールが開きます。 **[Hive エディター]** をクリックします。

    ![Hive エディターを開く](./media/data-lake-store-hdinsight-hadoop-use-portal/hdiadlcluster2.png "Open Hive editor")

3. Hive エディターで次のクエリを入力し、**[送信]** をクリックします。

        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder'

    この Hive クエリでデータ湖ストアに格納されたデータからテーブルを作成して `adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder`します。 この場所には、前にアップロードしたサンプル データ ファイルが含まれます。

    下部にある **[ジョブ セッション]** テーブルに、ジョブの状態が **[初期化中]** から **[実行中]**、**[完了]** へと変化していくのが示されます。 また、**[詳細の表示]** をクリックして、完了したジョブの詳細を確認することもできます。

    ![テーブルを作成する](./media/data-lake-store-hdinsight-hadoop-use-portal/hdiadlcluster3.png "Create table")

4. 次のクエリを実行して、テーブルが作成されたことを確認します。

        SHOW TABLES;

    このクエリに対応する [詳細の表示] をクリックすると、出力が次のように表示されます。

        hivesampletable
        vehicles

    **車両** 前に作成したテーブルです。 **hivesampletable** は、テーブルのサンプルを既定ですべての HDInsight クラスターで使用できます。

5. **vehicles** からデータを取得するクエリを実行することもできます。

        SELECT * FROM vehicles LIMIT 5;


## HDFS コマンドを使用して Data Lake Store にアクセスする

Data Lake Store を使用するように HDInsight クラスターを構成したら、HDFS シェル コマンドを使用してストアにアクセスできます。

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。

2. **[参照]**、**[HDInsight クラスター]** の順にクリックし、作成した HDInsight クラスターをクリックします。

3. クラスター ブレードで **[リモート デスクトップ]** をクリックし、**[リモート デスクトップ]** ブレードで **[接続]** をクリックします。

    ![HDInsight へのリモート接続](./media/data-lake-store-hdinsight-hadoop-use-portal/ADL.HDI.PS.Remote.Desktop.png "Create an Azure Resource Group")

    メッセージが表示されたら、リモート デスクトップ ユーザーに対して指定した資格情報を入力します。

4. リモート セッションで、Windows PowerShell を起動し、HDFS ファイル システムのコマンドを使用して、Azure Data Lake Store のファイルを一覧表示します。

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    これにより、以前に Data Lake Store にアップロードしたファイルが一覧表示されます。

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

    使用することも、 `hdfs dfs-配置` データストア湖にいくつかのファイルをアップロードし、使用するコマンド `hdfs dfs ls` ファイルが正常にアップロードされたかどうかを確認します。

## Data Lake Store を既定のストレージとして使用する HBase クラスターのプロビジョニングに関する考慮事項

HBase クラスターでは、Data Lake Store アカウントを既定のストレージとして使用できます。 その場合、クラスターを正常にプロビジョニングするには、クラスターに関連付けられているサービス プリンシパルが Data Lake Store アカウントにアクセスできる**必要があります**。 このことは次の 2 つの方法で確認できます。

* **既存のサービス プリンシパルを使用する場合**は、クラスターのプロビジョニングを開始する前に、サービス プリンシパルが Data Lake Store ファイル システムのルートレベルで ACL に追加されていることを確認する必要があります。
* クラスターのプロビジョニングの一環として**新しいサービス プリンシパルを作成する場合**は、クラスターのプロビジョニングを開始したらすぐに、新しく作成したサービス プリンシパルを Data Lake Store ファイル システムのルートレベルに追加する必要があります。 追加できなかった場合、クラスターはプロビジョニングされますが、HBase サービスは開始できません。 この問題を回避するには、サービス プリンシパルを Data Lake Store アカウントの ACL に追加し、Ambari Web UI を使用して HBase サービスを再起動します。

湖のデータ ストアのファイル システムにサービス プリンシパルを追加する方法については、次を参照してください。 [湖のデータ ストアのファイル システムへのアクセスを構成するサービス プリンシパル](#acl)します。



## 関連項目

* [PowerShell: 湖のデータ ストアを使用する HDInsight クラスターを作成します。](data-lake-store-hdinsight-hadoop-use-powershell.md)


[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx 
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx 

