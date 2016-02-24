<properties
    pageTitle="Azure Data Factory パイプラインでカスタム アクティビティを使用する"
    description="カスタム アクティビティを作成して Azure Data Factory パイプラインで使用する方法について説明します。"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="spelluru"/>

# Azure Data Factory パイプラインでカスタム アクティビティを使用する
Azure Data Factory などの組み込みアクティビティをサポート **コピー アクティビティ** と **HDInsight アクティビティ** に移動したり、データを処理するパイプラインで使用します。 独自の変換/処理ロジックで .NET カスタム アクティビティを作成し、そのアクティビティをパイプラインで使用することもできます。 いずれかを使用して実行するアクティビティを構成することができます、 **Azure HDInsight** クラスターまたは **Azure Batch** サービスです。   

この記事では、カスタム アクティビティを作成し、Azure Data Factory のパイプラインで使用する方法について説明します。 カスタム アクティビティを作成して使用するための詳細なチュートリアルも提供します。 チュートリアルでは、HDInsight リンク サービスを使用します。 Azure Batch を使用するには、サービスを代わりにリンクの種類のリンクされたサービスを作成する必要が **AzureBatch** パイプライン JSON のアクティビティ セクションで使用して (**linkedServiceName**)。 参照してください、 [Azure Batch リンク サービス](#AzureBatch) とカスタム アクティビティを Azure Batch の使用方法の詳細セクションです。


## <a name="walkthrough" /> チュートリアル
このチュートリアルでは、カスタム アクティビティを作成し、そのアクティビティを Azure Data Factory パイプラインで使用する手順について説明します。 このチュートリアルは、チュートリアルから、 [Azure Data Factory を使ってみる][adfgetstarted]します。 カスタム アクティビティの動作について確認したい場合は、まず「Azure Data Factory を使ってみる」のチュートリアルを終えてから、このチュートリアルを始めてください。

### 前提条件


- 」のチュートリアル [Azure Data Factory を使ってみる][adfgetstarted]します。 本チュートリアルを実行する前に、この記事のチュートリアルを完了しておく必要があります。
- Visual Studio 2012 または 2013
- ダウンロードしてインストール [Azure .NET SDK][azure-developer-center]
- 最新版をダウンロード [Azure Data Factory の NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories/) し、インストールします。 手順はこのチュートリアルにあります。
- Azure Storage の NuGet パッケージをダウンロードしてインストールします。 チュートリアルで方法を説明しているため、この手順は省略できます。

### 手順の概要 
1.  **カスタム アクティビティを作成** Data Factory ソリューションで使用します。 このカスタム アクティビティには、データ処理ロジックが含まれています。 
    1.  Visual Studio (または任意のコード エディター) で .NET Class Library プロジェクトを作成し、入力データを処理するコードを追加してプロジェクトをコンパイルします。  
    2.  出力フォルダーにあるすべてのバイナリ ファイルと PDB (省略可能) ファイルを zip 形式で圧縮します。  
    3.  zip ファイルを Azure Blob Storage にアップロードします。 詳細な手順については、「カスタム アクティビティの作成」セクションを参照してください。 
2. **カスタム アクティビティを使用する Azure data factory を作成する**:
    1. Azure Data Factory を作成します。
    2. リンクされたサービスを作成します。
        1. StorageLinkedService: BLOB にアクセスするためのストレージの資格情報を指定します。
        2. HDInsightLinkedService: コンピューティングとして Azure HDInsight を指定します。
    3. データセットを作成します。
        1. InputDataset: 入力 BLOB のストレージ コンテナーとフォルダーを指定します。
        1. OuputDataset: 出力 BLOB のストレージ コンテナーとフォルダーを指定します。
    2. カスタム アクティビティを使用するパイプラインを作成します。
    3. パラメーターを実行してテストします。
    4. パイプラインをデバッグします。

## カスタム アクティビティの作成
Azure Data Factory パイプラインで使用できる .NET カスタム アクティビティを作成するには、作成する必要があります、 **.NET クラス ライブラリ** を実装するクラスを含むプロジェクト **IDotNetActivity** インターフェイスです。 このインターフェイスには、Execute という 1 つのメソッドのみが含まれます。 このメソッドのシグネチャを次に示します。

    public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices, 
            IEnumerable<Dataset> datasets, 
            Activity activity, 
            IActivityLogger logger)
        
このメソッドには、理解しておく必要がある重要な要素がいくつかあります。 

- このメソッドには次の 4 つのパラメーターがあります。
    - **linkedServices**します。 入力/出力データ ソースを Data Factory にリンクする、リンクされたサービスの列挙可能な一覧です。 このサンプルでは、入力と出力の両方に使用される Azure Storage 型のリンクされたサービスは 1 つのみです。 
    - **データセット**します。 データセットの列挙可能な一覧です。 このパラメーターを使用すると、入力と出力のデータセットに定義されている場所とスキーマを取得できます。
    - **アクティビティ**します。 このパラメーターは、現在のコンピューティング エンティティ (この場合は Azure HDInsight) を表します。
    - **logger**します。 logger を使用すると、パイプラインの “User” ログとして表示されるデバッグ コメントが出力されます。 

- メソッドから、カスタム アクティビティの連結に使用できるディクショナリが返されます。 このサンプル ソリューションでは、この機能を使用しません。 

### 手順: 
1.  .NET クラス ライブラリ プロジェクトを作成します。
    <ol type="a">
        <li>ここで、 <b>Visual Studio 2012</b> または <b>Visual Studio 2013</b>.</li>
        <li>[ <b>ファイル</b>をクリックし、 <b>新規</b>] を展開して、[ <b>Project</b>.</li>
        <li>[ <b>テンプレート</b>を展開し、 <b>Visual C#</b>. このチュートリアルでは C# を使用しますが、カスタム アクティビティの開発には、どの .NET 言語でも使用できます。</li>
        <li>[ <b>クラス ライブラリ</b> を選択します。</li>
        <li>次に、 <b>MyDotNetActivity</b> を指定して <b>名前</b>.</li>
        <li>[ <b>C:\ADFGetStarted</b> を指定して <b>Location (場所)</b>.</li>
        <li>[ <b>OK</b> をクリックしてプロジェクトを作成します。</li>
    </ol>
2.  [ <b>ツール</b>をクリックし、 <b>[NuGet パッケージ マネージャー]</b>] を展開して、[ <b>パッケージ マネージャー コンソール</b>.
3.  [ <b>パッケージ マネージャー コンソール</b>をインポートするのには、次のコマンドを実行 <b>Microsoft.Azure.Management.DataFactories</b>.

        Install-Package Microsoft.Azure.Management.DataFactories

4. Azure Storage NuGet パッケージをプロジェクトにインポートします。

        Install-Package Azure.Storage

5. 次の追加 **を使用して** ステートメントをプロジェクト内のソース ファイルです。

        using System.IO;
        using System.Globalization;
        using System.Diagnostics;
        using System.Linq;

        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Runtime;

        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

6. 名前を変更、 **名前空間** に **MyDotNetActivityNS**します。

        namespace MyDotNetActivityNS

7. クラスの名前を変更 **MyDotNetActivity** から派生し、 **IDotNetActivity** インターフェイスの次のようにします。

        public class MyDotNetActivity : IDotNetActivity

8. 実装 (追加)、 **Execute** のメソッド、 **IDotNetActivity** へのインターフェイス、 **MyDotNetActivity** クラスし、メソッドに次のサンプル コードをコピーします。

    次のサンプル コードでは、入力 BLOB 内の行数をカウントし、BLOB へのパス、BLOB 内の行数、アクティビティが実行されたコンピューター、現在の日時を出力 BLOB に生成します。

        /// <summary>
        /// Execute method is the only method of IDotNetActivity interface you must implement. 
        /// In this sample, the method invokes the Calculate method to perform the core logic.  
        /// </summary>

        public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
        {

            // declare types for input and output data stores
            AzureStorageLinkedService inputLinkedService;

            // declare dataset types
            CustomDataset inputLocation;
            AzureBlobDataset outputLocation;

            Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
            inputLocation = inputDataset.Properties.TypeProperties as CustomDataset;

            foreach (LinkedService ls in linkedServices)
                logger.Write("linkedService.Name {0}", ls.Name);

            // using First method instead of Single since we are using the same 
            // Azure Storage linked service for input and output. 
            inputLinkedService = linkedServices.First(
                linkedService =>
                linkedService.Name ==
                inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
                as AzureStorageLinkedService;

            string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
            string folderPath = GetFolderPath(inputDataset);
            string output = string.Empty; // for use later.

            // create storage client for input. Pass the connection string.
            CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
            CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();

            // initialize the continuation token before using it in the do-while loop.
            BlobContinuationToken continuationToken = null;
            do
            {   // get the list of input blobs from the input storage client object.
                BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                         true,
                                         BlobListingDetails.Metadata,
                                         null,
                                         continuationToken,
                                         null,
                                         null);
                
                // Calculate method returns the number of occurrences of 
                // the search term (“Microsoft”) in each blob associated
                // with the data slice. 
                // 
                // definition of the method is shown in the next step.
 
                output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

            } while (continuationToken != null);

            // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
            Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
            // convert to blob location object.
            outputLocation = outputDataset.Properties.TypeProperties as AzureBlobDataset;

            folderPath = GetFolderPath(outputDataset);

            logger.Write("Writing blob to the folder: {0}", folderPath);

            // create a storage object for the output blob.
            CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
            // write the name of the file. 
            Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

            logger.Write("output blob URI: {0}", outputBlobUri.ToString());
            // create a new blob and upload the output text.
            CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
            logger.Write("Writing {0} to the output blob", output);
            outputBlob.UploadText(output);

            // return a new Dictionary object (unused in this code).
            return new Dictionary<string, string>();
        }

9. 以下のヘルパー メソッドを追加します。  **Execute** メソッドは、これらのヘルパー メソッドを呼び出します。  **GetConnectionString** メソッドは、Azure のストレージ接続文字列を取得し、 **GetFolderPath** メソッドは blob の場所を取得します。 最も重要なこと、 **Calculate** メソッドは、各 blob を反復処理するコードを分離します。

        /// <summary>
        /// Gets the folderPath value from the input/output dataset.   
        /// </summary>

        private static string GetFolderPath(Dataset dataArtifact)
        {
            if (dataArtifact == null || dataArtifact.Properties == null)
            {
                return null;
            }

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

            return blobDataset.FolderPath;
        }

        /// <summary>
        /// Gets the fileName value from the input/output dataset.   
        /// </summary>

        private static string GetFileName(Dataset dataArtifact)
        {
            if (dataArtifact == null || dataArtifact.Properties == null)
            {
                return null;
            }

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            if (blobDataset == null)
            {
                return null;
            }

            return blobDataset.FileName;
        }

        /// <summary>
        /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file, 
        /// and prepares the output text that will be written to the output blob. 
        /// </summary>

        public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
        {
            string output = string.Empty;
            logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
            foreach (IListBlobItem listBlobItem in Bresult.Results)
            {
                CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
                if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
                {
                    string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                    logger.Write("input blob text: {0}", blobText);
                    string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                    var matchQuery = from word in source
                                     where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                     select word;
                    int wordCount = matchQuery.Count();
                    output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
                }
            }
            return output;
        }

    GetFolderPath メソッドは、データセットが指すフォルダーのパスを返し、GetFileName メソッドは、データセットが指す BLOB/ファイルの名前を返します。 
    
            "name": "InputDataset",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "StorageLinkedService",
                "typeProperties": {
                    "fileName": "file.txt",
                    "folderPath": "mycontainer/inputfolder/",
    
    Calculate メソッドは、入力ファイル (フォルダー内の BLOB) 内のキーワード Microsoft のインスタンス数を計算します。 検索語句 ("Microsoft") は、コードにハード コーディングされています。

10. プロジェクトをコンパイルします。 クリックして **ビルド** ] をクリックし、メニューから **ソリューションのビルド**します。
11. 起動 **Windows エクスプ ローラー**, に移動して **bin \debug** または **bin \release** ビルドの種類に応じてをフォルダーです。
12. Zip ファイルを作成 **MyDotNetActivity.zip** 内のすべてのバイナリを含む、 <project folder>\bin\Debug フォルダーです。 追加する、 **MyDotNetActivity.pdb** ファイルにエラーが発生した場合、問題の原因となったソース コード内の行番号などの追加情報を取得します。

    ![バイナリ出力ファイル](./media/data-factory-use-custom-activities/Binaries.png)
13. アップロード **MyDotNetActivity.zip** blob コンテナーに blob として: **customactvitycontainer** 、Azure blob ストレージを **StorageLinkedService** のリンクされたサービスで、 **ADFTutorialDataFactory** を使用します。  Blob コンテナーを作成する **customactivitycontainer** が既に存在しない場合。

> [AZURE.NOTE] Data Factory プロジェクトを含む Visual Studio でソリューションにこの .NET アクティビティ プロジェクトを追加する場合は、zip ファイルを作成し、手動で Azure blob ストレージにアップロードすることの最後の 2 つの手順を実行する必要はありません。 Data Factory エンティティを Visual Studio を使用して発行すると、これらの手順は発行プロセスによって自動的に実行されます。 参照してください [Visual Studio を使用して最初のパイプラインの作成](data-factory-build-your-first-pipeline-using-vs.md) と [を Azure Blob から Azure SQL にデータをコピー](data-factory-get-started-using-vs.md) を作成して、Visual Studio を使用して Data Factory のエンティティの公開についての記事です。  

### Execute メソッド

このセクションでは詳細およびコードに関する注意事項、 **Execute** メソッドです。
 
1. 入力のコレクションを反復処理するメンバー内で検出されて、 [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) 名前空間。 Blob コレクションを反復処理する場合は、使用する必要があります、 **BlobContinuationToken** クラスです。 基本的に、既存のループのメカニズムとして、トークンに do-while ループを使用する必要があります。 詳細については、次を参照してください。 [.NET から Blob ストレージを使用する方法](../storage/storage-dotnet-how-to-use-blobs.md)します。 基本的なループを次に示します。

        // Initialize the continuation token.
        BlobContinuationToken continuationToken = null;
        do
        {   
            // Get the list of input blobs from the input storage client object.
            BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                      true,
                                      BlobListingDetails.Metadata,
                                      null,
                                      continuationToken,
                                      null,
                                      null);
            // Return a string derived from parsing each blob.
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
        } while (continuationToken != null);

    ドキュメントを参照して、 [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) 詳細についてはメソッドです。

2.  論理的には、BLOB セットを操作するコードを do-while ループ内に配置します。  **Execute** メソッドで、操作を実行-ループでは、blob の一覧をという名前のメソッドに渡しますながら **Calculate**します。 という名前の文字列変数を返します **出力** もセグメント内のすべての blob を反復処理の結果です。 

    検索語句の出現回数を返します (**Microsoft**) に渡される blob の **Calculate** メソッドです。 

            output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);

3.  1 回、 **Calculate** メソッドは、作業を実行したことに書き込まれると、新しい blob です。 そのため、処理対象の BLOB セットごとに、結果を新しい BLOB に出力できます。 新しい BLOB に出力するには、まず出力データセットを検索します。 

            // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
            Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

            // Convert to blob location object.
            outputLocation = outputDataset.Properties.TypeProperties as AzureBlobDataset;

4.  ヘルパー メソッドも呼び出します。 **GetFolderPath** フォルダーのパス (ストレージ コンテナー名) を取得します。
 
            folderPath = GetFolderPath(outputDataset);

     **GetFolderPath** FolderPath という名前のプロパティを持つ、AzureBlobDataSet に DataSet オブジェクトにキャストします。
            
            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
            
            return blobDataset.FolderPath;

5.  コードの呼び出し、 **GetFileName** ファイル名 (blob 名) を取得します。 このコードは、フォルダー パスを取得する上記のコードと似ています。 

            AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

            return blobDataset.FileName;

6.  新しい URI オブジェクトを作成してファイル名が出力されます。 URI のコンス トラクターを使用して、 **BlobEndpoint** コンテナー名を返すプロパティ。 フォルダー パスとファイル名が追加され、出力 BLOB URI が構築されます。  

            // Write the name of the file. 
            Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

7.  ファイル名が出力されるので、Calculate メソッドから出力文字列を新しい BLOB に出力できるようになります。

            // Create a new blob and upload the output text.
            CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
            logger.Write("Writing {0} to the output blob", output);
            outputBlob.UploadText(output);

## Data Factory の作成

 **カスタム アクティビティを作成** ] セクションで、カスタム アクティビティを作成して Azure blob コンテナーにバイナリを持つ zip ファイルと PDB ファイルをアップロードします。 このセクションでは、Azure を作成します **データ ファクトリ** で、 **パイプライン** を使用して、 **カスタム アクティビティ**します。
 
カスタム アクティビティの入力データセットは、Blob Storage の入力フォルダー (mycontainer\inputfolder) 内にある BLOB (ファイル) を表します。 アクティビティの出力データセットは、Blob Storage の出力フォルダー (mycontainer\outputfolder) 内にある出力 BLOB を表します。 

次の内容の file.txt というファイルを作成し、mycontainer\inputfolder にアップロードします (mycontainer は Azure Blob コンテナーの名前、inputfolder はそのコンテナー内のフォルダー名です)。

    test custom activity Microsoft test custom activity Microsoft

入力フォルダーに複数のファイルが含まれている場合でも、入力フォルダーは Azure Data Factory のスライスに相当します。 パイプラインで各スライスが処理されるときに、カスタム アクティビティでは、そのスライスの入力フォルダー内にあるすべての BLOB が反復処理されます。 

mycontainer\output フォルダーには、1 行または複数行 (入力フォルダーの BLOB 数と同じ数) が含まれる 1 つの出力ファイルが作成されます。
 
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.


このセクションで実行する手順を次に示します。

1. 作成、 **データ ファクトリ**します。
2. 作成 **リンクされたサービス** マップ専用のジョブおよび入力/出力 blob を保持している Azure ストレージとして、カスタム アクティビティを実行する HDInsight クラスター用です。 
2. 入力と出力の作成 **データセット** カスタム アクティビティの入力呼び出し力を表します。 
3. 作成し、実行、 **パイプライン** カスタム アクティビティを使用します。

### 手順 1: Data Factory を作成する

1.  Azure ポータルにログインした後、次の操作を行います。
    1.  クリックして **新規** 左側のメニュー。
    2.  クリックして **データ + 分析** で、 **新規** ブレードです。
    3.  クリックして **Data Factory** 上、 **データ分析** ブレードです。
2.   **新しいデータ ファクトリ** ブレードで、入力 **CustomActivityFactory** 名前にします。 Azure Data Factor の名前はグローバルに一意にする必要があります。 エラーが発生する場合: **データ ファクトリ名"CustomActivityFactory"は使用できません**, 、データ ファクトリの名前を変更 (たとえば、 **yournameCustomActivityFactory**) もう一度作成し、直してください。
3.  クリックして **リソース グループ名**, 、既存のリソース グループを選択するか、新しいリソース グループを作成します。 
4.  使用している正しいことを確認 **サブスクリプション** と **地域** データ ファクトリを作成する場合します。 
5.  をクリックして **作成** 上、 **新しいデータ ファクトリ** ブレードです。
6.  作成されているデータ ファクトリが表示されます、 **ダッシュ ボード** Azure ポータルのです。
7.  Data Factory が正常に作成されると、Data Factory の内容を表示する [Data Factory] ブレードが表示されます。

### 手順 2. リンク サービスを作成する

リンクされたサービスは、データ ストアまたはコンピューティング サービスを Azure Data Factory にリンクします。 この手順では、Azure Storage アカウントと Azure HDInsight クラスターを Data Factory にリンクします。

#### Azure Storage のリンクされたサービスを作成する

1.  クリックして、 **作成者および展開** タイルを **DATA FACTORY** のブレード **CustomActivityFactory**します。 Data Factory エディタが起動します。
2.  クリックして **新しいデータ ストア** コマンドの横棒グラフし、選択 **Azure ストレージ**します。 Azure Storage のリンクされたサービスを作成するための JSON スクリプトがエディターに表示されます。
3.  置換 **アカウント名** を Azure ストレージ アカウントの名前と **アカウント キー** Azure ストレージ アカウントのアクセス キーを持つ。 ストレージ アクセス キーを取得する方法については、次を参照してください。 [表示、コピーおよび再生成するストレージ アクセス キー](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)します。
4.  をクリックして **展開** 、コマンド バー、リンクされたサービスをデプロイします。


#### Azure HDInsight のリンクされたサービスを作成する 
Azure Data Factory サービスはオンデマンド クラスターの作成をサポートしており、このクラスターを使用して入力データの処理と出力データの生成を行います。 また、独自のクラスターを使って同じ処理を行うことも可能です。 オンデマンド HDInsight クラスターを使用する場合は、スライスごとにクラスターが作成されます。 一方、独自の HDInsight クラスターを使用する場合、そのクラスターはすぐにスライスを処理できる状態にあります。 そのため、オンデマンド クラスターを使用すると、独自のクラスターを使用するよりデータの出力が遅いと感じる場合があります。

> [AZURE.NOTE] 実行時に、.NET アクティビティのインスタンスは、HDInsight クラスターの 1 つのワーカー ノードでのみ実行されます。これは、複数のノード上で実行するスケールできません。 .NET アクティビティの複数のインスタンスは、HDInsight クラスターの別々のノードで並列に実行できます。

拡張した場合、 [Azure Data Factory を使ってみる][adfgetstarted] からチュートリアルをチュートリアル [を使用して Pig と Hive Azure Data Factory を][hivewalkthrough], 、このリンクされたサービスの作成を省略し、既に ADFTutorialDataFactory 内にあるリンク サービスを使用することができます。


##### オンデマンド HDInsight クラスターを使用するには

1.  **Azure ポータル**, 、クリックして **作成とデプロイ** Data Factory のホーム ページです。
2. Data Factory エディターをクリックして **新しいコンピューティング** し、コマンド バーから **、オンデマンド HDInsight クラスター** ] メニューからです。
2. JSON スクリプトで、以下の手順を実行します。
    1.  **ClusterSize** プロパティには、HDInsight クラスターのサイズを指定します。
    3.  **TimeToLive** プロパティには、どの顧客アイドル状態でいられる時間が削除されるかを指定します。
    4.  **バージョン** プロパティには、使用する HDInsight のバージョンを指定します。 このプロパティを除外した場合は、最新バージョンが使用されます。  
    5.  **LinkedServiceName**, 、指定 **StorageLinkedService** 入門チュートリアルで作成したすることです。

            {
              "name": "HDInsightOnDemandLinkedService",
              "properties": {
                "type": "HDInsightOnDemand",
                "typeProperties": {
                  "clusterSize": "1",
                  "timeToLive": "00:05:00",
                  "version": "3.2",
                  "linkedServiceName": "StorageLinkedService"
                }
              }
            }

2. をクリックして **展開** 、コマンド バー、リンクされたサービスをデプロイします。

##### 独自の HDInsight クラスターを使用するには

1.  **Azure ポータル**, 、クリックして **作成とデプロイ** Data Factory のホーム ページです。
2.  **Data Factory エディター**, 、クリックして **新しいコンピューティング** し、コマンド バーから **HDInsight クラスター** ] メニューからです。
2. JSON スクリプトで、以下の手順を実行します。
    1.  **ClusterUri** プロパティには、HDInsight の URL を入力します。 たとえば、https://<clustername>.azurehdinsight.net/ などです。     
    2.  **UserName** プロパティには、HDInsight クラスターへのアクセス権を持つユーザー名を入力します。
    3.  **パスワード** プロパティには、ユーザーのパスワードを入力します。
    4.  **LinkedServiceName** プロパティには、入力 **StorageLinkedService**します。 これは、入門チュートリアルで作成したリンク サービスです。

2. をクリックして **展開** 、コマンド バー、リンクされたサービスをデプロイします。

### 手順 3: データセットを作成する
この手順では、入力データと出力データを示すデータセットを作成します。

#### 入力データセットの作成
1.   **エディター** Data Factory をクリックして **新しいデータセット** をクリックしてツール バー ボタン **Azure Blob ストレージ** 、ドロップ ダウン メニューからです。
2.  右側のウィンドウの JSON を、次の JSON スニペットに置き換えます。

            {
                "name": "InputDataset",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "StorageLinkedService",
                    "typeProperties": {
                        "folderPath": "adftutorial/customactivityinput/",
                        "format": {
                            "type": "TextFormat"
                        }
                    },
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "external": true,
                    "policy": {}
                }
            }

    このチュートリアルで後半では、開始時刻 2015-11-16T00:00:00Z、終了時刻 2015-11-16T05:00:00Z のパイプラインを作成します。 入力/出力スライスが 5 があるために、1 時間ごとに、データの生成がスケジュールされます (間 **00**: 00]-> [ **05**: 00)。 

     **頻度** と **間隔** に設定されている入力データセットの **時間** と **1**, 、つまり入力スライスが毎時間利用可能なができます。 このサンプルでは、intputfolder の同じファイル (file.txt) です。 

    次に各スライスの開始時刻を示します。これは上記の JSON スニペットの SliceStart システム変数で表されます。 

    
3.  クリックして **展開** 、ツールバーの作成および展開する、 **InputDataset**します。 表示されていることを確認、 **テーブルは正常に作成された** エディターのタイトル バーにメッセージです。


#### 出力テーブルの作成

1.  **Data Factory エディター**, 、クリックして **新しいデータセット**, 、順にクリック **Azure Blob ストレージ** コマンド バーからです。
2. 右側のウィンドウの JSON スクリプトを、次の JSON スクリプトに置き換えます。

        {
            "name": "OutputDataset",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "StorageLinkedService",
                "typeProperties": {
                    "fileName": "{slice}.txt",
                    "folderPath": "adftutorial/customactivityoutput",
                    "partitionedBy": [
                        {
                            "name": "slice",
                            "value": {
                                "type": "DateTime",
                                "date": "SliceStart",
                                "format": "yyyy-MM-dd-HH"
                            }
                        }
                    ]
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }

    出力場所は **adftutorial/customactivityoutput/YYYYMMDDHH/** YYYYMMDDHH は年、月、日、およびスライスが生成された時間。 参照してください [開発者向けリファレンス][adf-developer-reference] 詳細です。

    各入力スライスの出力 BLOB/ファイルが生成されます。 次に、各スライスの出力ファイルの命名方法を示します。 すべての出力ファイルが 1 つの出力フォルダーに生成します。 **adftutorial\customactivityoutput**します。

    | スライス | 開始時刻 | 出力ファイル |
    | :---- | :--------- | :---------- | 
    | 1 | 2015-11-16T00:00:00 | 2015-11-16-00.txt |
    | 2 | 2015-11-16T01:00:00 | 2015-11-16-01.txt |
    | 3 | 2015-11-16T02:00:00 | 2015-11-16-02.txt |
    | 4 | 2015-11-16T03:00:00 | 2015-11-16-03.txt |
    | 5 | 2015-11-16T04:00:00 | 2015-11-16-04.txt |

    入力フォルダー内のすべてのファイルは、前述の開始時刻であるスライスの一部です。 このスライスを処理すると、カスタム アクティビティは各ファイルをスキャンし、出力ファイルに、検索語句 (“Microsoft”) の出現回数が記述された 1 行を生成します。 inputfolder に 3 つのファイルがある場合、出力ファイルには 1 時間ごとのスライス (2015-11-16-00.txt、2015-11-16:01:00:00.txt など) の 3 行が生成されます。 


2. クリックして **展開** を展開するコマンド バーで、 **OutputDataset**します。


### カスタム アクティビティを使用するパイプラインの作成と実行

1. Data Factory エディターをクリックして **新しいパイプライン** コマンド バーにします。 このコマンドが表示されない場合は、**[...](省略記号)** をクリックすると表示されます。
2. 右側のウィンドウの JSON を、次の JSON スクリプトに置き換えます。 したかどうかに、独自のクラスターを使用するを作成する手順、 **HDInsightLinkedService** のリンクされたサービス、置き換える **HDInsightOnDemandLinkedService** と **HDInsightLinkedService** 、次の JSON でします。

        {
          "name": "ADFTutorialPipelineCustom",
          "properties": {
            "description": "Use custom activity",
            "activities": [
              {
                "Name": "MyDotNetActivity",
                "Type": "DotNetActivity",
                "Inputs": [
                  {
                    "Name": "InputDataset"
                  }
                ],
                "Outputs": [
                  {
                    "Name": "OutputDataset"
                  }
                ],
                "LinkedServiceName": "HDInsightOnDemandLinkedService",
                "typeProperties": {
                  "AssemblyName": "MyDotNetActivity.dll",
                  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                  "PackageLinkedService": "StorageLinkedService",
                  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
                  "extendedProperties": {
                    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
                  }
                },
                "Policy": {
                  "Concurrency": 1,
                  "ExecutionPriorityOrder": "OldestFirst",
                  "Retry": 3,
                  "Timeout": "00:30:00",
                  "Delay": "00:00:00"
                }
              }
            ],
            "start": "2015-11-16T00:00:00Z",
            "end": "2015-11-16T05:00:00Z",
            "isPaused": false
          }
        }

    置換 **StartDateTime** 値と現在の日付の前に、3 日以内と **EndDateTime** を現在の日付の値。 StartDateTime と enddatetime は、いずれの両方がである必要があります [ISO 形式](http://en.wikipedia.org/wiki/ISO_8601)します。 (例: 2014-10-14T16:32:41Z)。 出力テーブルは毎日生成されるようスケジュール設定されているので、3 つのスライスが生成されることになります。

    以下の点に注意してください。

    - Activities セクションにある 1 つのアクティビティと型である: **DotNetActivity**します。
    - 同じ入力テーブルを使用して **EmpTableFromBlob** 入門チュートリアルで使用します。
    - 新しい出力テーブルを使用して **OutputTableForCustom** は、次の手順で作成します。
    - **AssemblyName** DLL の名前に設定されている: **MyActivities.dll**します。
    - **EntryPoint** に設定されている **MyDotNetActivityNS.MyDotNetActivity**します。
    - **PackageLinkedService** に設定されている **StorageLinkedService** をカスタム アクティビティの zip ファイルを含む blob ストレージをポイントします。 入力/出力ファイルとカスタム アクティビティ zip ファイルに別の Azure Storage アカウントを使用している場合、Azure Storage のリンクされたサービスを別に作成する必要があります。 この記事では、同じ Azure Storage アカウントを使用している前提で説明します。
    - **PackageFile** に設定されている **customactivitycontainer/MyDotNetActivity.zip**します。 形式: <containerforthezip>/< nameofthezip.zip > です。
    - カスタム アクティビティは、 **InputDataset** 入力としてと **OutputDataset** として出力します。
    - カスタム アクティビティの linkedServiceName プロパティが指す、 **HDInsightLinkedService**, 、カスタム アクティビティは、Azure HDInsight クラスター上で実行する必要がある Azure Data Factory に通知します。
    - **isPaused** にプロパティが設定されている **false** 既定です。 この例では、スライスが過去に開始されているので、パイプラインは即時に実行されます。 このプロパティを true に設定すると、パイプラインを一時停止できます。また false に設定し直すと再開されます。 
    -  **開始** 時間と **エンド** にかかる時間が **5** とスライスの時間間隔は 1 時間ごと作成、スライスが 5 がパイプラインによって生成されるようにします。 


4. をクリックして **展開** 、コマンド バー、パイプラインをデプロイします。

### パイプラインの監視
 
8. Azure ポータルで [Data Factory] ブレードで [ **ダイアグラム**します。
    
    ![Diagram tile](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
 
9. [ダイアグラム] ビューで、OutputDataset をクリックします。
 
    ![ダイアグラム ビュー](./media/data-factory-use-custom-activities/diagram.png)

10. スライスが生成されると、準備完了状態の出力スライスが 5 個表示されます。

    ![出力スライス](./media/data-factory-use-custom-activities/OutputSlices.png)
    
12. Blob ストレージに出力ファイルが生成されたことを確認、 **adftutorial** コンテナーです。

    ![カスタム アクティビティの出力][image-data-factory-ouput-from-custom-activity]

9. 出力ファイルを開くと、以下のような出力結果が表示されるはずです。

    ファイル inputfolder/2015-11-16-00/file.txt には、検索語句 "Microsoft" が 2 回出現しました。

10. 使用して、 [Azure ポータル][azure-preview-portal] または Azure PowerShell コマンドレットをデータ ファクトリ、パイプライン、およびデータ セットを監視します。 メッセージが表示、 **ActivityLogger** 、ポータルまたはコマンドレットを使用してからダウンロードできるログ (具体的にはユーザーの 0.log) 内でカスタム アクティビティ用のコードにします。

    ![カスタム アクティビティのダウンロード ログ][image-data-factory-download-logs-from-custom-activity]


参照してください [モニターおよび管理のパイプライン](data-factory-monitor-manage-pipelines.md) データセットとパイプラインを監視するための詳細な手順についてです。      

### パイプラインのデバッグ
デバッグには、いくつかの基本的な技術があります。

1.  入力スライスに設定されていない場合 **準備**, 、入力フォルダー構造が正しい file.txt は、入力フォルダーに存在していることを確認します。
2.   **Execute** を使用して、カスタム アクティビティのメソッド、 **IActivityLogger** の問題をトラブルシューティングするのに役立つ情報を記録するオブジェクト。 ログに記録されたメッセージは user_0.log ファイルに表示されます。 

     **OutputDataset** ブレードをクリックして、スライス、 **データ スライス** そのスライスのブレードです。 表示されます **アクティビティの実行** そのスライスのです。 このスライスには 1 回のアクティビティの実行が表示されます。 コマンド バーの [実行] をクリックする場合、同じスライスの別のアクティビティの実行を開始できます。 

    アクティビティの実行] をクリックすると表示されます、 **アクティビティの実行の詳細** ログ ファイルの一覧のブレードです。 user_0.log ファイルにログに記録されたメッセージが表示されます。 エラーが発生すると、パイプライン/アクティビティ JSON の再試行回数が 3 に設定されるので、3 つのアクティビティの実行が表示されます。 アクティビティの実行をクリックすると、ログ ファイルが表示されます。このファイルを確認すると、エラーを解決することができます。 

    ログ ファイルの一覧でクリックして、 **ユーザー 0.log**します。 右側のパネルを使用した結果、 **IActivityLogger.Write** メソッドです。

    確認することも必要があります **システム 0.log** なシステム エラー メッセージの例外のためです。

3.  含める、 **PDB** ができるように、エラーの詳細情報など、zip ファイルにファイル **コール スタック** エラーが発生します。
4.  カスタム アクティビティの zip ファイル内のすべてのファイルでなければなりません、 **最上位レベル** サブフォルダーは不要にします。
5.  いることを確認、 **assemblyName** (MyDotNetActivity.dll) **entryPoint**(MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer/MyDotNetActivity.zip) と **packageLinkedService** (zip ファイルを含む Azure blob ストレージをポイントする必要があります) が適切な値に設定します。 
6.  エラーと、スライスを再処理するを解決した場合は、スライスを右クリックし、 **OutputDataset** ブレードをクリック **実行**します。 


## カスタム アクティビティの更新
カスタム アクティビティのコードを更新する場合は、カスタム アクティビティを構築し、新しいバイナリを含む zip ファイルを BLOB ストレージにアップロードします。

## <a name="AzureBatch"></a> Azure Batch のリンクされたサービスを使用します。
> [AZURE.NOTE] 参照してください [Azure Batch の基本][batch-technical-overview] サービスの Azure Batch の概要についてを参照してください [.NET 向け Azure Batch ライブラリの概要][batch-get-started] 簡単に Azure Batch サービスを開始します。

コンピューティング リソースとして Azure Batch を使用するカスタム .NET アクティビティを実行できます。 独自の Azure Batch のプールを作成し、他の構成と併せて VM の数を指定する必要があります。 Azure Batch のプールは、顧客に以下の機能を提供します。

1. 作成するプールのコアは 1 個から数千個まで対応
2. 式に基づいて VM 数を自動スケール
3. あらゆるサイズの VM をサポート
4. VM あたりのタスク数を構成可能
5. 無制限の数のタスクをキューに配置


ここでは、前のセクションのチュートリアルで説明した、Azure Batch リンク サービスを使用するための手順の概要を示します。

1. 使用して Azure Batch アカウントを作成、 [Azure ポータル](http://manage.windowsazure.com)します。 参照してください [を作成して、Azure Batch アカウントを管理][batch-create-account] 手順については資料です。 Azure Batch のアカウント名とアカウント キーをメモしておきます。

    使用することも [New-azurebatchaccount][new-azure-batch-account] コマンドレットを Azure Batch アカウントを作成します。 参照してください [Azure Batch アカウントの管理に Azure PowerShell を使用して][azure-batch-blog] についてこのコマンドレットを使用します。
2. Azure Batch プールの作成 ソース コードをダウンロードすることができます、 [Azure Batch Explorer ツール][batch-explorer], 、コンパイル、およびそれを使用して (または) を使用して [.NET 向け Azure Batch ライブラリ][batch-net-library] Azure Batch プールを作成します。 参照してください [Azure Batch Explorer サンプル チュートリアル][batch-explorer-walkthrough] Azure Batch Explorer の使用手順の詳細について。

    使用することも [新規 AzureRmBatchPool](https://msdn.microsoft.com/library/mt628690.aspx) コマンドレットを Azure Batch プールを作成します。

    また、複数のコンピューティング ノードを含む Azure Batch プールを作成して、それらのスライスが並列して処理されるようにすることもできます。 Batch Explorer を使用している場合:  

    - プールの ID を入力 (**プール ID**)。 注、 **プールの ID**; Data Factory ソリューションを作成するときに必要になります。 
    - 指定 **Windows Server 2012 R2** オペレーティング システム ファミリの設定にします。
    - 指定 **2** 値、 **コンピューティング ノードあたりの最大タスク** 設定します。
    - 指定 **2** 値、 **数のターゲット専用** 設定します。 

    Data Factory サービスは、名前の Azure Batch のジョブを作成します adf-。<pool name>: ジョブ xxx。 タスクは、スライスのアクティビティ実行のたびに作成されます。 10 個のスライスを処理する準備ができた場合、このジョブに 10 個のタスクが作成されています。 プール内に複数のコンピューティング ノードがある場合、複数のスライスを並列して実行することができます。 コンピューティング ノードごとの最大タスクが 1 より大きな値に設定されている場合、同じコンピューティング ノードで複数のスライスを実行することもできます。 
    
    ![Batch Explorer のタスク](./media/data-factory-use-custom-activities/BatchExplorerTasks.png)

    ![Data Factory と Batch](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

2. 次の JSON テンプレートを使用して、Azure Batch リンク サービスを作成します。 Data Factory エディターには、最初に同様のテンプレートが表示されます。 JSON スニペットで、Azure Batch のアカウント名、アカウント キー、プール名を指定します。

        {
          "name": "AzureBatchLinkedService",
          "properties": {
            "type": "AzureBatch",
            "typeProperties": {
              "accountName": "<Azure Batch account name>",
              "batchUri": "https://<region>.batch.azure.com",
              "accessKey": "<Azure Batch account key>",
              "poolName": "<Azure Batch pool name>",
              "linkedServiceName": "<Specify associated storage linked service reference here>"
            }
          }
        }

    > [AZURE.IMPORTANT]  **URL** から、 **Azure Batch アカウント] ブレード** は次の形式: accountname.region.batch.azure.com です。  **BatchUri** JSON のプロパティ、する必要は **「accountname.」を削除** 使用して URL、 **accountname** の **accountName** JSON のプロパティです。

     **PoolName** プロパティには、プールの名前ではなく、プールの ID を指定することもできます。

    参照してください [Azure Batch リンク サービスに関する MSDN トピック](https://msdn.microsoft.com/library/mt163609.aspx) 説明については、これらのプロパティです。

2.  Data Factory エディターでは、置換、チュートリアルで作成したパイプラインの JSON 定義を開き **HDInsightLinkedService** と **AzureBatchLinkedService**します。
3.  Azure Batch サービスを使用してシナリオをテストできるように、パイプラインの開始時刻と終了時刻を変更できます。
4.  次の図に示すように、Azure Batch Explorer でスライスの処理に関連する Azure Batch のタスクを確認できます。

    ![Azure Batch のタスク][image-data-factory-azure-batch-tasks]

> [AZURE.NOTE] HDInsight の場合と同様、Data Factory サービスは、Azure Batch のオンデマンド オプションをサポートできません。 Azure データ ファクトリでは、独自の Azure Batch プールのみ使用できます。    

## 関連項目

[Azure Data Factory の更新: Azure Batch を使用した ADF カスタム .NET アクティビティを実行する](http://azure.microsoft.com/blog/2015/05/01/azure-data-factory-updates-execute-adf-custom-net-activities-using-azure-batch/)です。

[batch-net-library]: ../batch/batch-dotnet-get-started.md
[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
[batch-create-account]: ../batch/batch-account-create-portal.md
[batch-technical-overview]: ../batch/batch-technical-overview.md
[batch-get-started]: ../batch/batch-dotnet-get-started.md
[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md
[adf-tutorial]: data-factory-tutorial.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: http://go.microsoft.com/fwlink/?LinkId=517478
[azure-developer-center]: http://azure.microsoft.com/develop/net/
[adf-developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-get-started.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-ouput-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png

[image-data-factory-azure-batch-tasks]: ./media/data-factory-use-custom-activities/AzureBatchTasks.png

