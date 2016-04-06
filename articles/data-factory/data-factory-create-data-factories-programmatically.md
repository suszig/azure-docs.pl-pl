<properties 
    pageTitle="Data Factory SDK を使用して Azure Data Factory を作成、監視、管理する" 
    description="Data Factory SDK を使用して Azure Data Factory をプログラムによって作成、監視、管理する方法について説明します。" 
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
    ms.date="10/06/2015" 
    ms.author="spelluru"/>

# Data Factory .NET SDK を使用して Azure Data Factory を作成、監視、管理する
## 概要
Data Factory .NET SDK を使用して Azure Data Factory をプログラムによって作成、監視、管理できます。 この記事には、Data Factory の作成と監視を行うサンプルの .NET コンソール アプリケーションを作成できるチュートリアルが含まれています。 参照してください [Data Factory クラス ライブラリ リファレンス][adf-class-library-reference] の詳細については Data Factory .NET SDK です。 



## 前提条件

- Visual Studio 2012 または 2013
- ダウンロードしてインストール [Azure .NET SDK][azure-developer-center]
- Azure Data Factory の NuGet パッケージをダウンロードしてインストールします。 手順はこのチュートリアルにあります。

## チュートリアル
1. Visual Studio 2012 または 2013 を使用して、C# .NET コンソール アプリケーションを作成します。
    <ol type="a">
        <li>起動 <b>Visual Studio 2012</b> または <b>Visual Studio 2013</b>します。</li>
        <li>クリックして <b>ファイル</b>, 、] をポイント <b>新規</b>, 、] をクリック <b>プロジェクト</b>します。</li> 
        <li>展開 <b>テンプレート</b>, を選択して <b>Visual c#</b>します。 このチュートリアルでは C# を使用しますが、どの .NET 言語でも使用できます。</li> 
        <li>選択 <b>コンソール アプリケーション</b> 右側にプロジェクトの種類の一覧からです。</li>
        <li>入力 <b>DataFactoryAPITestApp</b> の <b>名前</b>します。</li> 
        <li>選択 <b>C:\ADFGetStarted</b> の <b>場所</b>します。</li>
        <li>クリックして <b>OK</b> プロジェクトを作成します。</li>
    </ol>
2. <b>[ツール]</b> をクリックし、<b>[NuGet パッケージ マネージャー]</b> をポイントして、<b>[パッケージ マネージャー コンソール]</b> をクリックします。
3.  <b>[パッケージ マネージャー コンソール]</b> で、次のコマンドを 1 つずつ入力します。</b> 

        Install-Package Microsoft.Azure.Management.DataFactories
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
6. 次の追加 **appSetttings** セクションに、 **App.config** ファイルです。 ヘルパー メソッドが使用している: **GetAuthorizationHeader**します。 

    値を置き換える **SubscriptionId** と **ActiveDirectoryTenantId** を Azure サブスクリプションとテナント Id です。 実行してこれらの値を取得できる **Get-azureaccount** から Azure PowerShell (最初に Add-azureaccount を使用してログインする必要があります)。
 
        <appSettings>
            <!--CSM Prod related values-->
            <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
            <add key="AdfClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
            <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
            <!--Make sure to write your own tenenat id and subscription ID here-->
            <add key="SubscriptionId" value="your subscription ID" />
            <add key="ActiveDirectoryTenantId" value="your tenant ID" />
        </appSettings>
6. 次の追加 **を使用して** ステートメントをプロジェクトのソース ファイル (Program.cs) にします。

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. インスタンスを作成する次のコードを追加  **DataPipelineManagementClient** クラスを **Main** メソッドです。 このオブジェクトを使用して、Data Factory、リンクされたサービス、入力と出力データセット、およびパイプラインを作成します。 また、実行時にデータセットのスライスを監視する際にもこのオブジェクトを使用します。    

        // create data factory management client
        string resourceGroupName = "resourcegroupname";
        string dataFactoryName = "APITutorialFactorySP";

        TokenCloudCredentials aadTokenCredentials =
            new TokenCloudCredentials(
                ConfigurationManager.AppSettings["SubscriptionId"],
                GetAuthorizationHeader());

        Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

        DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.NOTE] 置き換える、 **resourcegroupname** Azure リソース グループの名前に置き換えます。 使用してリソース グループを作成することができます、 [New-azureresourcegroup](https://msdn.microsoft.com/library/Dn654594.aspx) コマンドレットです。

7. 作成する次のコードを追加、 **データ ファクトリ** に、 **Main** メソッドです。

        // create a data factory
        Console.WriteLine("Creating a data factory");
        client.DataFactories.CreateOrUpdate(resourceGroupName,
            new DataFactoryCreateOrUpdateParameters()
            {
                DataFactory = new DataFactory()
                {
                    Name = dataFactoryName,
                    Location = "westus",
                    Properties = new DataFactoryProperties() { }
                }
            }
        );

8. 作成する次のコードを追加、 **のリンクされたサービス** に、 **Main** メソッドです。 

    > [AZURE.NOTE] 使用 **アカウント名** と **アカウント キー** のための Azure ストレージ アカウント、 **ConnectionString**します。 

        // create a linked service
        Console.WriteLine("Creating a linked service");
        client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
            new LinkedServiceCreateOrUpdateParameters()
            {
                LinkedService = new LinkedService()
                {
                    Name = "LinkedService-AzureStorage",
                    Properties = new LinkedServiceProperties
                    (
                        new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>")
                    )
                }
            }
        );
9. 次のコードを作成する追加 **入力呼び出し力データセット** に、 **Main** メソッドです。 

    注意してください、 **FolderPath** に設定されている入力 blob の **adftutorial/** 、 **adftutorial** blob ストレージ内のコンテナーの名前を指定します。 Azure blob ストレージにこのコンテナーが存在しない場合は、この名前のコンテナーを作成します。 **adftutorial** し、テキスト ファイルをコンテナーにアップロードします。
    
    設定されている出力 blob の FolderPath を注意してください: **adftutorial/apifactoryoutput/{slice} {スライス}/** 、 **スライス** の値に基づいて動的に計算が **SliceStart** (各スライスの日付/時刻を開始します)。  

 
        // create input and output datasets
        Console.WriteLine("Creating input and output datasets");
        string Dataset_Source = "DatasetBlobSource";
        string Dataset_Destination = "DatasetBlobDestination";

        client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        LinkedServiceName = "LinkedService-AzureStorage",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });

        client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Destination,
                    Properties = new DatasetProperties()
                    {

                        LinkedServiceName = "LinkedService-AzureStorage",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/apifactoryoutput/{Slice}",
                            PartitionedBy = new Collection<Partition>()
                            {
                                new Partition()
                                {
                                    Name = "Slice",
                                    Value = new DateTimePartitionValue()
                                    {
                                        Date = "SliceStart",
                                        Format = "yyyyMMdd-HH"
                                    }
                                }
                            }
                        },

                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },
                    }
                }
            });

11. 次のコードを追加 **作成してパイプラインをアクティブに** に、 **Main** メソッドです。 このパイプラインには、 **CopyActivity** を受け取る **BlobSource** ソースとして、 **BlobSink** 、シンクとして。

            // create a pipeline
        Console.WriteLine("Creating a pipeline");
        DateTime PipelineActivePeriodStartTime = new DateTime(2014, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
        DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
        string PipelineName = "PipelineBlobSample";

        client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
            new PipelineCreateOrUpdateParameters()
            {
                Pipeline = new Pipeline()
                {
                    Name = PipelineName,
                    Properties = new PipelineProperties()
                    {
                        Description = "Demo Pipeline for data transfer between blobs",

                        // Initial value for pipeline's active period. With this, you won't need to set slice status
                        Start = PipelineActivePeriodStartTime,
                        End = PipelineActivePeriodEndTime,

                        Activities = new List<Activity>()
                        {                                
                            new Activity()
                            {   
                                Name = "BlobToBlob",
                                Inputs = new List<ActivityInput>()
                                {
                                    new ActivityInput() {
                                        Name = Dataset_Source
                                    }
                                },
                                Outputs = new List<ActivityOutput>()
                                {
                                    new ActivityOutput()
                                    {
                                        Name = Dataset_Destination
                                    }
                                },
                                TypeProperties = new CopyActivity()
                                {
                                    Source = new BlobSource(),
                                    Sink = new BlobSink()
                                    {
                                        WriteBatchSize = 10000,
                                        WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                    }
                                }
                            }

                        },
                    }
                }
            });

    

12. 使用される次のヘルパー メソッドを追加、 **Main** メソッドを **プログラム** クラスです。 このメソッドは入力が可能にする] ダイアログ ボックスを表示 **ユーザー名** と **パスワード** に使用する Azure 旧ポータルにログインします。 
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    result = context.AcquireToken(
                        resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                        clientId: ConfigurationManager.AppSettings["AdfClientId"],
                        redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                        promptBehavior: PromptBehavior.Always);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });

            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();

            if (result != null)
            {
                return result.AccessToken;
            }

            throw new InvalidOperationException("Failed to acquire token");
        }  
 
13. 次のコードを追加、 **Main** 出力データセットのデータ スライスの状態を取得します。 この例で予想されるスライスのみが存在します。   
 
        // Pulling status within a timeout threshold
        DateTime start = DateTime.Now;
        bool done = false;

        while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
        {
            Console.WriteLine("Pulling the slice status");
            // wait before the next status check
            Thread.Sleep(1000 * 12);

            var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                new DataSliceListParameters()
                {
                    DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                    DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                });

            foreach (DataSlice slice in datalistResponse.DataSlices)
            {
                if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                {
                    Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                    done = true;
                    break;
                }
                else
                {
                    Console.WriteLine("Slice status is: {0}", slice.State);
                }
            }
        }

14. **(省略可能)** 次のコードを実行するデータ スライスの詳細の取得を追加、 **Main** メソッドです。

        Console.WriteLine("Getting run details of a data slice");

        // give it a few minutes for the output slice to be ready
        Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
        Console.ReadKey();

        var datasliceRunListResponse = client.DataSliceRuns.List(
                resourceGroupName,
                dataFactoryName,
                Dataset_Destination,
                new DataSliceRunListParameters()
                {
                    DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                }
            );
        
        foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
        {
            Console.WriteLine("Status: \t\t{0}", run.Status);
            Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
            Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
            Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
            Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
            Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
            Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
        }

        Console.WriteLine("\nPress any key to exit.");
        Console.ReadKey();

15. ソリューション エクスプ ローラーでプロジェクトを展開 (**DataFactoryAPITestApp**)、右クリックし **参照**, 、] をクリック **参照の追加**します。 チェック ボックスをオン **System.Configuration** アセンブリとクリック **OK**します。 
16. コンソール アプリケーションをビルドします。 クリックして **ビルド** ] をクリックし、メニューに **ソリューションのビルド**します。 
16. Azure BLOB ストレージ内の adftutorial コンテナーに少なくとも 1 つのファイルが存在することを確認します。 存在しない場合は、以下の内容を記述した Emp.txt ファイルをメモ帳で作成し、これを adftutorial コンテナーにアップロードします。

        John, Doe
        Jane, Doe
     
17. クリックして、サンプルを実行 **デバッグ** ]-> [ **[デバッグ開始]** メニュー。 表示されている場合、 **実行されるデータ スライスの詳細**, 、いくつかの分、およびキーを押して待つ **ENTER**します。 
18. Data factory 内のことを確認する、Azure ポータルを使用して: **APITutorialFactory** は、次のアーティファクトで作成されます。 
    - リンクされたサービス: **LinkedService_AzureStorage** 
    - データセット: **DatasetBlobSource** と **DatasetBlobDestination**します。
    - パイプライン: **PipelineBlobSample** 
18. 出力ファイルが作成されることを確認、 **{slice}** 内のフォルダー、 **adftutorial** コンテナーです。



> [AZURE.NOTE] 上記のサンプル コードでは、Azure の資格情報を入力するためのダイアログ ボックスを開きます。 サインイン プログラムを使用してダイアログ ボックスを使用せず、参照する必要がある場合 [Azure リソース マネージャーでサービス プリンシパルの認証](resource-group-authenticate-service-principal.md#authenticate-service-principal-with-certificate---powershell)します。 


[data-factory-introduction]: data-factory-introduction.md
[adf-getstarted]: data-factory-get-started.md
[adf-tutorial]: data-factory-tutorial.md
[use-custom-activities]: data-factory-use-custom-activities.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
 
[adf-class-library-reference]: http://go.microsoft.com/fwlink/?LinkID=521877
[azure-developer-center]: http://azure.microsoft.com/downloads/
 


