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
- [ポータルを使用してください。](data-lake-store-get-started-portal.md)
- [PowerShell の使用](data-lake-store-get-started-powershell.md)
- [.NET SDK の使用](data-lake-store-get-started-net-sdk.md)
- [Azure CLI の使用](data-lake-store-get-started-cli.md)
- [Node.js の使用](data-lake-store-manage-use-nodejs.md)

Azure ポータルを使用して、Azure Data Lake Store アカウントを作成し、フォルダーの作成、データ ファイルのアップロードとダウンロード、アカウントの削除などの基本操作を行う方法について説明します。湖のデータ ストアの詳細については、次を参照してください。 [Azure Data Lake ストアの概要](data-lake-store-overview.md)します。

## 前提条件

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。

## <a name="signup"></a>Data Lake ストアのパブリック プレビューの Azure サブスクリプションを有効にします。

まず、Azure サブスクリプションを Data Lake Store パブリック プレビューに対して有効にする必要があります。 以下の手順に従ってください。

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。
2. クリックして **新規**, 、] をクリックして **データ + ストレージ**, 、] をクリックし、 **Azure Data Lake ストア**します。
3.  **湖 [新しいデータ ストア** ブレードで、をクリックして **プレビューにサインアップ**します。 クリックして、情報を読み取る **OK**します。 パブリック プレビューに対して Azure サブスクリプションが有効になると、電子メールを受信します。

    ![パブリック プレビューにサインアップ](./media/data-lake-store-get-started-portal/preview-signup.png "Create a new Azure Data Lake account")

## Azure Data Lake Store アカウントを作成する

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。

2. クリックして **新規**, 、] をクリックして **データ + ストレージ**, 、] をクリックし、 **Azure Data Lake ストア**します。 情報を読んで、 **Azure Data Lake ストア** ブレードで、クリックして **作成** ブレードの左下隅にします。

3.  **湖 [新しいデータ ストア** ブレードで、次の画面キャプチャで示すように、値を指定します。

    ![新しい Azure Data Lake Store アカウントの作成](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Create a new Azure Data Lake account")

    - **サブスクリプション**します。 新しい Data Lake Store アカウントを作成するサブスクリプションを選択します。
    - **リソース グループ**します。 既存のリソース グループを選択するか [ **リソース グループを作成** を作成します。 リソース グループは、1 つのアプリケーションの関連リソースを保持するコンテナーです。 詳細については、次を参照してください。 [Azure リソース グループ](resource-group-overview.md#resource-groups)します。
    - **場所**: 湖のデータ ストアのアカウントを作成する場所を選択します。

4. 選択 **スタート画面にピン** データ湖ストア アカウントのスタート画面からアクセスできるようにする場合。

5. クリックして **作成**します。 アカウントをスタート画面にピン留めした場合は、スタート画面に戻ると、Data Lake Store アカウントのプロビジョニングの進行状況を確認できます。 Data Lake Store アカウントのプロビジョニングが完了すると、アカウントのブレードが表示されます。

6. 展開、 **Essentials** リソースがグループ化など、データ湖ストア アカウントの情報を表示するには、ドロップダウン、場所などの一部であります。クリックして、 **クイック スタート** 湖のデータ ストアに関連するその他のリソースへのリンクを表示するアイコン。

    ![自分の Azure Data Lake Store アカウント](./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Your Azure Data Lake account")

## <a name="createfolder"></a>Azure Data Lake ストア アカウントにフォルダーを作成します。

Data Lake Store アカウントにフォルダーを作成し、データの管理と保存を行うことができます。

1. 先ほど作成した Data Lake Store アカウントを開きます。 左側のウィンドウから次のようにクリックします。 **参照**, 、] をクリック **データストア湖**, 、湖のデータ ストア] ブレードの [フォルダーを作成するアカウント名] をクリックします。 スタート画面にアカウントをピン留めした場合は、そのアカウントのタイルをクリックします。

2. Data Lake ストア アカウントのブレードでクリックして **データ エクスプ ローラー**します。

    ![Data Lake Store アカウントにフォルダーを作成](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Create folders in Data Lake Store account")

3. Data Lake ストア アカウントのブレードでをクリックして **新しいフォルダー**, 、新しいフォルダーの名前を入力し、クリックして **OK**します。
    
    ![Data Lake Store アカウントにフォルダーを作成](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Create folders in Data Lake Store account")
    
    新しく作成したフォルダーを表示する、 **データ エクスプ ローラー** ブレードです。 レベルに上限なく入れ子になったフォルダーを作成できます。

    ![Data Lake アカウントにフォルダーを作成](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Create folders in Data Lake account")


## <a name="uploaddata"></a>Azure Data Lake ストア アカウントにデータをアップロードします。

データは、Azure Data Lake Store アカウントのルート レベルに直接をアップロードすることも、アカウント内に作成したフォルダーにアップロードすることもできます。 次の画面キャプチャでからのサブ フォルダーにファイルをアップロードする手順に従います、 **データ エクスプ ローラー** ブレードです。 この画面キャプチャの場合、ファイルは、階層リンクで示されているサブフォルダー (赤色の四角形で示されています) にアップロードされます。

アップロードするには、いくつかのサンプル データを探している場合は取得できます、 **救急データ** フォルダーから、 [Azure Data Lake Git リポジトリ](https://github.com/MicrosoftBigData/AzureDataLake/tree/master/SQLIPSamples/SampleData/AmbulanceData)します。

![データのアップロード](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Upload data")


## <a name="properties"></a>プロパティと、格納されたデータで使用できる操作

新しく追加されたファイルを開く] をクリックして、 **プロパティ** ブレードです。 このブレードでは、そのファイルに関連付けられたプロパティと、そのファイルに対して実行できる操作を利用できます。 また、次の画面キャプチャで赤い四角形で強調表示されている、Azure Data Lake Store アカウント内のファイルへの完全なパスをコピーすることもできます。

![データのプロパティ](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Properties on the data")

* クリックして **プレビュー** 、ブラウザーから直接、ファイルのプレビューを表示します。 プレビューの形式を指定することもできます。 をクリックして **プレビュー**, 、] をクリックして **形式** で、 **ファイル プレビュー** ブレードで、し、[、 **ファイル プレビュー形式** ブレードはなどの区切り記号を使用するエンコーディングを表示するには、行の数などのオプションを指定します。

  ![ファイル プレビューのフォーマット](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "File preview format")

* をクリックして **ダウンロード** をコンピューターにファイルをダウンロードします。

* クリックして **名前変更ファイル** ファイルの名前を変更します。

* クリックして **ファイルの削除** ファイルを削除します。


## データのセキュリティ保護

Azure Data Lake Store アカウントに保存したデータは、Azure Active Directory とアクセス制御 (ACL) を使用してセキュリティで保護することができます。 手順を実行する方法については、次を参照してください。 [Azure Data Lake ストア内のデータをセキュリティで保護する](data-lake-store-secure-data.md)です。


## Azure Data Lake Store アカウントの削除

湖のデータ ストア] ブレードから、Azure Data Lake ストア アカウントを削除しても **削除**します。 この操作を確認するために、削除するアカウントの名前を入力するよう求められます。 アカウントの名前を入力し、クリックして **削除**します。

![Data Lake アカウントの削除](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Delete Data Lake account")

## Data Lake Store アカウントの他の作成方法

- [Azure PowerShell で Data Lake Store の使用を開始する](data-lake-store-get-started-powershell.md)
- [.NET SDK で Azure Data Lake Store の使用を開始する](data-lake-store-get-started-net-sdk.md)
- [Azure CLI で Azure Data Lake Store の使用を開始する](data-lake-store-get-started-cli.md)


## 次のステップ

- [Data Lake Store のデータをセキュリティで保護する](data-lake-store-secure-data.md)
- [Data Lake Store で Azure Data Lake Analytics を使用する](data-lake-analytics-get-started-portal.md)
- [Data Lake Store で Azure HDInsight を使用する](data-lake-store-hdinsight-hadoop-use-portal.md)

