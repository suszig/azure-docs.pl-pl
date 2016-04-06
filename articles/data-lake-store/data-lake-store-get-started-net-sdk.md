<properties 
   pageTitle="Data Lake Store .NET SDK を使用してアプリケーションを開発する | Azure" 
   description="Data Lake Store .NET SDK を使用してアプリケーションを開発する" 
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

# .NET SDK で Data Lake Store の使用を開始する

> [AZURE.SELECTOR]
- [ポータルの使用](data-lake-store-get-started-portal.md)
- [PowerShell の使用](data-lake-store-get-started-powershell.md)
- [.NET SDK の使用](data-lake-store-get-started-net-sdk.md)
- [Azure CLI の使用](data-lake-store-get-started-cli.md)
- [Node.js の使用](data-lake-store-manage-use-nodejs.md)

Azure Data Lake Store .NET SDK を使用して、Azure Data Lake アカウントを作成し、フォルダーの作成、データ ファイルのアップロードとダウンロード、アカウントの削除などの基本操作を行う方法について説明します。Data Lake に関する詳細については、次を参照してください。 [Azure Data Lake ストア](data-lake-store-overview.md)します。

## 前提条件

* Visual Studio 2013 または 2015 以下の手順では、Visual Studio 2015 を使用します。
* **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。
* **Azure サブスクリプションを有効にする** 湖のデータ ストアのパブリック プレビューします。 参照してください [指示](data-lake-store-get-started-portal.md#signup)します。

## .NET アプリケーションの作成

1. Visual Studio を開き、コンソール アプリケーションを作成します。

2.  **ファイル** ] メニューのをクリックして **新規**, 、クリックして **プロジェクト**します。

3.  **新しいプロジェクト**, を入力するか、次の値を選択します。

  	| プロパティ | 値                       |
  	|----------|-----------------------------|
  	| カテゴリ | テンプレート/Visual C#/Windows |
  	| テンプレート | コンソール アプリケーション         |
  	| 名前     | CreateADLApplication        |

4. クリックして **OK** プロジェクトを作成します。

5. Nuget パッケージをプロジェクトに追加します。 

    1. ソリューション エクスプ ローラーでプロジェクト名を右クリックし、クリックして **NuGet パッケージの管理**します。
    2.  **Nuget パッケージ マネージャー** ] タブで、必ず **パッケージ ソース** に設定されている **nuget.org** と **プレリリースを含める** ] チェック ボックスをオンします。
    3. 以下のパッケージを検索してインストールします。
    
        * Microsoft.Azure.Common.Authentication
        * Microsoft.Azure.Management.DataLake.Store
        * Microsoft.Azure.Management.DataLake.StoreFileSystem
        * Microsoft.Azure.Management.DataLake.StoreUploader

        ![Nuget ソースの追加](./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Create a new Azure Data Lake account")

    4. 閉じる、 **Nuget パッケージ マネージャー**します。

7. 開いている **Program.cs** し、既存のコード ブロックを次のコードに置き換えます。 また、コード スニペットでパラメーターの値を指定します。 

    このコードでは、Data Lake Store の作成、ストアでのフォルダーの作成、ファイルのアップロード、ファイルのダウンロード、最後にアカウントの削除のプロセスを行います。 アップロードするには、いくつかのサンプル データを探している場合は取得できます、 **救急データ** フォルダーから、 [Azure Data Lake Git リポジトリ](https://github.com/MicrosoftBigData/AzureDataLake/tree/master/SQLIPSamples/SampleData/AmbulanceData)します。
    
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Security;
        using System.IO;
        
        using Microsoft.Azure;
        using Microsoft.Azure.Common.Authentication;
        using Microsoft.Azure.Common.Authentication.Models;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.Azure.Management.DataLake.StoreFileSystem;
        using Microsoft.Azure.Management.DataLake.StoreFileSystem.Models;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.Azure.Common.Authentication.Factories;
        
        
        namespace CreateADLApplication
        {
            class CreateADLApplication
            {
                private static DataLakeStoreManagementClient _dataLakeStoreClient;
                private static DataLakeStoreFileSystemManagementClient _dataLakeStoreFileSystemClient;
                private const string ResourceGroupName = "<resource_grp_name>"; //THIS SHOULD ALREADY EXIST
        
                static void Main(string[] args)
                {
                    var subscriptionId = new Guid("<subscription_ID>");
                    var _credentials = GetAccessToken();
                    string dataLakeAccountName = "<data_lake_store_name>";
                    string location = "East US 2";
        
                    _credentials = GetCloudCredentials(_credentials, subscriptionId);
                    _dataLakeStoreClient = new DataLakeStoreManagementClient(_credentials);
                    _dataLakeStoreFileSystemClient = new DataLakeStoreFileSystemManagementClient(_credentials);
        
                    var parameters = new DataLakeStoreAccountCreateOrUpdateParameters();
                    parameters.DataLakeStoreAccount = new DataLakeStoreAccount
                    {
                        Name = dataLakeAccountName,
                        Location = location
                    };
        
                    // Create a Data Lake Store account
                    Console.WriteLine("Creating an Azure Data Lake Store account ...");
                    _dataLakeStoreClient.DataLakeStoreAccount.Create(ResourceGroupName, parameters);
        
                    Console.WriteLine("Account created. Press ENTER to continue...");
                    Console.ReadLine();
        
                    // Create a directory called MYTEMPDIR in the store
                    Console.WriteLine("Creating a directory under the Azure Data Lake Store account");
                    CreateDir(_dataLakeStoreFileSystemClient, "/mytempdir", dataLakeAccountName, "777");
                    Console.WriteLine("Directory created. Press ENTER to continue...");
                    Console.ReadLine();
        
                    // Upload a file under MYTEMPDIR
                    Console.WriteLine("Uploading a file to the Azure Data Lake Store account");
                    bool force = false; //Set this to true if you want to overwrite existing data
                    UploadFile(_dataLakeStoreFileSystemClient, dataLakeAccountName, "C:\\users\\nitinme\\desktop\\vehicle1_09142014.csv", "/mytempdir/vehicle1_09142014.csv", force);
                    Console.WriteLine("File uploaded. Press ENTER to continue...");
                    Console.ReadLine();
        
                    // List the files in the Data Lake Store
                    Console.WriteLine("Listing all files in the Azure Data Lake Store account");
                    var fileList = ListItems(_dataLakeStoreFileSystemClient, dataLakeAccountName, "/mytempdir");
                    var fileMenuItems = fileList.Select(a => String.Format("{0,15} {1}", a.Type, a.PathSuffix)).ToList();
                    foreach (var filename in fileMenuItems)
                    {
                        Console.WriteLine(filename);
        
                    }
                    Console.WriteLine("Files listed. Press ENTER to continue...");
                    Console.ReadLine();
        
                    // Download the files from the Data Lake Store
                    Console.WriteLine("Downloading files from an Azure Data Lake Store account");
                    DownloadFile(_dataLakeStoreFileSystemClient, dataLakeAccountName, "/mytempdir/vehicle1_09142014.csv", "C:\\users\\nitinme\\desktop\\vehicle1_09142014_copy.csv", force);
                    Console.WriteLine("Files downloaded. Press ENTER to continue...");
                    Console.ReadLine();
        
                    // Delete the Data Lake Store account
                    Console.WriteLine("Azure Data Lake Store account will be deleted. Press ENTER to continue...");
                    _dataLakeStoreClient.DataLakeStoreAccount.Delete(ResourceGroupName, dataLakeAccountName);
                    Console.ReadLine();
                    Console.WriteLine("Account deleted. Press ENTER to exit...");
                    Console.ReadLine();
                }
        
                public static SubscriptionCloudCredentials GetAccessToken(string username = null, SecureString password = null)
                {
                    var authFactory = new AuthenticationFactory();
        
                    var account = new AzureAccount { Type = AzureAccount.AccountType.User };
        
                    if (username != null && password != null)
                        account.Id = username;
        
                    var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
                    return new TokenCloudCredentials(authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken);
                }
        
                public static SubscriptionCloudCredentials GetCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
                {
                    return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
                }
        
                public static bool CreateDir(DataLakeStoreFileSystemManagementClient dataLakeStoreFileSystemClient, string path, string dlAccountName, string permission)
                {
                    dataLakeStoreFileSystemClient.FileSystem.Mkdirs(path, dlAccountName, permission);
                    return true;
                }
        
                public static bool UploadFile(DataLakeStoreFileSystemManagementClient dataLakeStoreFileSystemClient, string dlAccountName, string srcPath, string destPath, bool force = false)
                {
                    var parameters = new UploadParameters(srcPath, destPath, dlAccountName, isOverwrite: true);
                    var frontend = new DataLakeStoreFrontEndAdapter(dlAccountName, dataLakeStoreFileSystemClient);
                    var uploader = new DataLakeStoreUploader(parameters, frontend);
                    uploader.Execute();
                    return true;
                }
        
                public static void DownloadFile(DataLakeStoreFileSystemManagementClient dataLakeFileSystemClient,
                string dataLakeAccountName, string srcPath, string destPath, bool force)
                {
                    var beginOpenResponse = dataLakeFileSystemClient.FileSystem.BeginOpen(srcPath, dataLakeAccountName,
                        new FileOpenParameters());
                    var openResponse = dataLakeFileSystemClient.FileSystem.Open(beginOpenResponse.Location);
                    if (force || !File.Exists(destPath))
                        File.WriteAllBytes(destPath, openResponse.FileContents);
                }
        
                public static List<FileStatusProperties> ListItems(DataLakeStoreFileSystemManagementClient dataLakeFileSystemClient, string dataLakeAccountName, string path)
                {
                    var response = dataLakeFileSystemClient.FileSystem.ListFileStatus(path, dataLakeAccountName, new DataLakeStoreFileSystemListParameters());
                    return response.FileStatuses.FileStatus.ToList();
                }
            }
        }


8. アプリケーションをビルドし、実行します。 指示に従って、アプリケーションを実行して完了します。

## Data Lake Store アカウントの他の作成方法

- [Azure プレビュー ポータルで Azure Data Lake Store の使用を開始する](data-lake-store-get-started-portal.md)
- [PowerShell で Data Lake Store の使用を開始する](data-lake-store-get-started-powershell.md)
- [Azure CLI で Azure Data Lake Store の使用を開始する](data-lake-store-get-started-cli.md)


## 次のステップ

- [Data Lake Store のデータをセキュリティで保護する](data-lake-store-secure-data.md)
- [Data Lake Store で Azure Data Lake Analytics を使用する](data-lake-analytics-get-started-portal.md)
- [Data Lake Store で Azure HDInsight を使用する](data-lake-store-hdinsight-hadoop-use-portal.md)



