<properties 
   pageTitle="Data Lake Store の使用 | Azure" 
   description="ポータルを使用して、Data Lake Store アカウントを作成し、Data Lake Store で基本的な操作を実行します" 
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


# Azure ポータルで Azure Data Lake Store の使用を開始する

> [AZURE.SELECTOR]
- [Using Portal](data-lake-store-get-started-portal.md)
- [Using PowerShell](data-lake-store-get-started-powershell.md)
- [Using .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Using Azure CLI](data-lake-store-get-started-cli.md)
- [Using Node.js](data-lake-store-manage-use-nodejs.md)


Azure ポータルを使用して、Azure Data Lake Store アカウントを作成し、フォルダーの作成、データ ファイルのアップロードとダウンロード、アカウントの削除などの基本操作を行う方法について説明します。湖のデータ ストアの詳細については、次を参照してください。 [Azure Data Lake ストアの概要](data-lake-store-overview.md)します。

## 前提条件

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。

## <a name="signup"></a>Data Lake ストアのパブリック プレビューの Azure サブスクリプションを有効にします。

まず、Azure サブスクリプションを Data Lake Store パブリック プレビューに対して有効にする必要があります。 以下の手順に従ってください。

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。
2. **[新規]**、**[データ + ストレージ]**、**[Azure Data Lake Store]** の順にクリックします。
3. **[新しい Data Lake Store]** ブレードで、**[プレビュー版にサインアップ]** をクリックします。 情報を確認し、**[OK]** をクリックします。 パブリック プレビューに対して Azure サブスクリプションが有効になると、電子メールを受信します。

    ![パブリック プレビューにサインアップ](./media/data-lake-store-get-started-portal/preview-signup.png "Create a new Azure Data Lake account")

## Azure Data Lake Store アカウントを作成する

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。

2. **[新規]**、**[データ + ストレージ]**、**[Azure Data Lake Store]** の順にクリックします。 **[Azure Data Lake Store]** ブレードに表示された情報を確認し、ブレードの左下隅にある **[作成]** をクリックします。

3. **[新しい Data Lake Store]** ブレードで、次の画面キャプチャに示すように値を指定します。

    ![新しい Azure Data Lake Store アカウントの作成](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Create a new Azure Data Lake account")

    - **[サブスクリプション]**:  新しい Data Lake Store アカウントを作成するサブスクリプションを選択します。
    - **リソース グループ**。 既存のリソース グループを選択するか、**[リソース グループの作成]** をクリックしてリソース グループを作成します。 リソース グループは、1 つのアプリケーションの関連リソースを保持するコンテナーです。 詳細については、次を参照してください。 [Azure リソース グループ](resource-group-overview.md#resource-groups)します。
    - **[場所]**: Data Lake Store アカウントを作成する場所を選択します。

4. Data Lake Store アカウントにスタート画面からアクセスできるようにする場合は、**[スタート画面にピン留めする]** を選択します。

5. **[作成]** をクリックします。 アカウントをスタート画面にピン留めした場合は、スタート画面に戻ると、Data Lake Store アカウントのプロビジョニングの進行状況を確認できます。 Data Lake Store アカウントのプロビジョニングが完了すると、アカウントのブレードが表示されます。

6. **[要点]** ボックスの一覧を展開し、Data Lake Store アカウントに関する情報 (属しているリソース グループや場所など) を確認します。**[クイック スタート]** アイコンをクリックすると、Data Lake Store に関連するその他のリソースへのリンクが表示されます。

    ![自分の Azure Data Lake Store アカウント](./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Your Azure Data Lake account")

## <a name="createfolder"></a>Azure Data Lake ストア アカウントにフォルダーを作成します。

Data Lake Store アカウントにフォルダーを作成し、データの管理と保存を行うことができます。

1. 先ほど作成した Data Lake Store アカウントを開きます。 左側のウィンドウで、**[参照]**、**[Data Lake Store]** の順にクリックし、[Data Lake Store] ブレードで、フォルダーを作成するアカウント名をクリックします。 スタート画面にアカウントをピン留めした場合は、そのアカウントのタイルをクリックします。

2. Data Lake Store アカウントのブレードで、**[データ エクスプローラー]** をクリックします。

    ![Data Lake Store アカウントにフォルダーを作成](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Create folders in Data Lake Store account")

3. Data Lake Store アカウントのブレードで、**[新しいフォルダー]** をクリックし、新しいフォルダーの名前を入力して、**[OK]** をクリックします。

    ![Data Lake Store アカウントにフォルダーを作成](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Create folders in Data Lake Store account")

    新しく作成されたフォルダーが **[データ エクスプローラー]** ブレードに表示されます。 レベルに上限なく入れ子になったフォルダーを作成できます。

    ![Data Lake アカウントにフォルダーを作成](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Create folders in Data Lake account")


## <a name="uploaddata"></a>Azure Data Lake ストア アカウントにデータをアップロードします。

データは、Azure Data Lake Store アカウントのルート レベルに直接をアップロードすることも、アカウント内に作成したフォルダーにアップロードすることもできます。 次の画面キャプチャでは、**[データ エクスプローラー]** ブレードからサブフォルダーにファイルをアップロードする手順を示しています。 この画面キャプチャの場合、ファイルは、階層リンクで示されているサブフォルダー (赤色の四角形で示されています) にアップロードされます。

アップロードするには、いくつかのサンプル データを探している場合は取得できます、 **救急データ** フォルダーから、 [Azure Data Lake Git リポジトリ](https://github.com/MicrosoftBigData/AzureDataLake/tree/master/SQLIPSamples/SampleData/AmbulanceData)します。

![データのアップロード](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Upload data")


## <a name="properties"></a>プロパティと、格納されたデータで使用できる操作

新しく追加したファイルをクリックし、**[プロパティ]** ブレードを開きます。 このブレードでは、そのファイルに関連付けられたプロパティと、そのファイルに対して実行できる操作を利用できます。 また、次の画面キャプチャで赤い四角形で強調表示されている、Azure Data Lake Store アカウント内のファイルへの完全なパスをコピーすることもできます。

![データのプロパティ](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Properties on the data")

* **[プレビュー]** をクリックすると、ブラウザーから直接、ファイルのプレビューが表示されます。 プレビューの形式を指定することもできます。 **[プレビュー]** をクリックし、**[ファイルのプレビュー]** ブレードの **[形式]** をクリックします。**[ファイルのプレビュー形式]** ブレードで、表示する行数、使用するエンコード、使用する区切り記号などのオプションを指定します。

  ![ファイル プレビューのフォーマット](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "File preview format")

* ファイルをコンピューターにダウンロードするには、**[ダウンロード]** をクリックします。

* ファイルの名前を変更するには、**[ファイル名の変更]** をクリックします。

* ファイルを削除するには、**[ファイルの削除]** をクリックします。


## データのセキュリティ保護

Azure Data Lake Store アカウントに保存したデータは、Azure Active Directory とアクセス制御 (ACL) を使用してセキュリティで保護することができます。 手順を実行する方法については、次を参照してください。 [Azure Data Lake ストア内のデータをセキュリティで保護する](data-lake-store-secure-data.md)します。


## Azure Data Lake Store アカウントの削除

Azure Data Lake Store アカウントを削除するには、Data Lake Store ブレードで **[削除]** をクリックします。 この操作を確認するために、削除するアカウントの名前を入力するよう求められます。 アカウントの名前を入力し、**[削除]** をクリックします。

![Data Lake アカウントの削除](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Delete Data Lake account")

## Data Lake Store アカウントの他の作成方法

- [PowerShell を使用してデータ湖ストアを使ってみる](data-lake-store-get-started-powershell.md)
- [.NET SDK を使用してデータ湖ストアを使ってみる](data-lake-store-get-started-net-sdk.md)
- [Azure CLI を使用してデータ湖ストアを使ってみる](data-lake-store-get-started-cli.md)


## 次のステップ

- [Data Lake ストア内のデータをセキュリティで保護します。](data-lake-store-secure-data.md)
- [データ湖ストアでの Azure Data Lake 分析の使用します。](data-lake-analytics-get-started-portal.md)
- [Data Lake ストアと Azure HDInsight を使用してください。](data-lake-store-hdinsight-hadoop-use-portal.md)





