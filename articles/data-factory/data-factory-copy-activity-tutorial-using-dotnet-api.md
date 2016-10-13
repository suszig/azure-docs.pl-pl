<properties 
    pageTitle="Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu interfejsu API .NET | Microsoft Azure" 
    description="Ten samouczek zawiera instrukcje tworzenia potoku usługi Azure Data Factory za pomocą działania kopiowania przy użyciu interfejsu API .NET." 
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
    ms.topic="get-started-article" 
    ms.date="09/16/2016" 
    ms.author="spelluru"/>


# Samouczek: tworzenie potoku za pomocą działania kopiowania przy użyciu interfejsu API .NET
> [AZURE.SELECTOR]
- [Przegląd i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kreator kopiowania](data-factory-copy-data-wizard-tutorial.md)
- [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Interfejs API REST](data-factory-copy-activity-tutorial-using-rest-api.md)
- [Interfejs API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Ten samouczek pokazuje, jak utworzyć i monitorować fabrykę danych Azure przy użyciu interfejsu API .NET. Potok w fabryce danych używa działania kopiowania w celu kopiowania danych z usługi Azure Blob Storage do usługi Azure SQL Database.

Działanie kopiowania wykonuje operację przenoszenia danych w usłudze Azure Data Factory. Działanie jest obsługiwane przez globalnie dostępną usługę, która może kopiować dane między różnymi magazynami danych w sposób bezpieczny, niezawodny i skalowalny. Szczegółowe informacje dotyczące działania kopiowania znajdują się w artykule [Data Movement Activities](data-factory-data-movement-activities.md) (Działania przenoszenia danych).   

> [AZURE.NOTE] 
> Ten artykuł nie obejmuje całego interfejsu API .NET usługi Data Factory. Szczegółowe informacje na temat zestawu SDK .NET usługi Data Factory zamieszczono w temacie [Data Factory .NET API Reference](https://msdn.microsoft.com/library/mt415893.aspx) (Informacje o interfejsie API .NET usługi Data Factory). 

## Wymagania wstępne
- Przejrzyj [omówienie samouczka i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md), aby uzyskać omówienie samouczka i pełne kroki **wymagań wstępnych**. 
- Program Visual Studio w wersji 2012, 2013 lub 2015.
- Pobierz i zainstaluj zestaw [SDK .NET Azure](http://azure.microsoft.com/downloads/).
- Azure PowerShell. Postępuj zgodnie z instrukcjami w artykule [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Instalowanie i konfigurowanie programu Azure PowerShell), aby zainstalować program Azure PowerShell na komputerze. Program Azure PowerShell służy do tworzenia aplikacji Azure Active Directory.

### Tworzenie aplikacji w usłudze Azure Active Directory
Utwórz aplikację usługi Azure Active Directory, utwórz nazwę główną usługi aplikacji i przypisz ją do roli **Współautor Data Factory**.  

1. Uruchom program **PowerShell**. 
1. Uruchom poniższe polecenie i wprowadź nazwę użytkownika oraz hasło, których używasz do logowania się w witrynie Azure Portal.
    
        Login-AzureRmAccount   
2. Uruchom poniższe polecenie, aby wyświetlić wszystkie subskrypcje dla tego konta.

        Get-AzureRmSubscription 
3. Uruchom poniższe polecenie, aby wybrać subskrypcję, z którą chcesz pracować. Zastąp ciąg **&lt;NameOfAzureSubscription**&gt; nazwą subskrypcji platformy Azure. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Zanotuj dane **SubscriptionId** i **TenantId** z danych wyjściowych tego polecenia. 
4. Utwórz grupę zasobów platformy Azure o nazwie **ADFTutorialResourceGroup** przez uruchomienie następującego polecenia w programie PowerShell.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Jeśli istnieje już grupa zasobów, można określić, czy należy ją zaktualizować (Y), czy zachować (N). 

    Jeśli używasz innej grupy zasobów, podczas wykonywania instrukcji w tym samouczku trzeba będzie wstawić jej nazwę zamiast nazwy ADFTutorialResourceGroup.
5. Utwórz aplikację usługi Azure Active Directory. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Jeśli zostanie wyświetlony następujący błąd, określ inny adres URL i uruchom polecenie ponownie. 

        Another object with the same value for property identifierUris already exists.

6. Utwórz nazwę główną usługi AD. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Dodaj nazwę główną usługi do roli **Współautor Data Factory**. 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Uzyskaj identyfikator aplikacji.

        $azureAdApplication

    Zanotuj identyfikator aplikacji (**applicationID** z danych wyjściowych).

Po wykonaniu tych kroków powinny być dostępne cztery następujące wartości: 

- Identyfikator dzierżawy
- Identyfikator subskrypcji
- Identyfikator aplikacji 
- Hasło (określone w pierwszym poleceniu)   

## Przewodnik
1. Za pomocą programu Visual Studio 2012/2013/2015 utwórz aplikację konsolową .NET C#.
    1. Uruchom program **Visual Studio** 2012/2013/2015.
    2. Kliknij pozycję **Plik**, wskaż polecenie **Nowy** i kliknij pozycję **Projekt**.
    3. Rozwiń węzeł **Szablony** i wybierz opcję **Visual C#**. W tym przewodniku stosowany jest język C#, ale można użyć dowolnego języka platformy .NET.
    4. Wybierz opcję **Aplikacja konsolowa** z listy typów projektów po prawej stronie.
    5. Wprowadź wartość **DataFactoryAPITestApp** jako nazwę.
    6. Wybierz ścieżkę **C:\ADFGetStarted** jako lokalizację.
    7. Kliknij przycisk **OK**, aby utworzyć projekt.
2. Kliknij pozycję **Narzędzia**, wskaż pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **Konsola menedżera pakietów**.
3.  W oknie **Konsola menedżera pakietów** wykonaj po kolei następujące polecenia. 

        Install-Package Microsoft.Azure.Management.DataFactories
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213
6. Dodaj następującą sekcję **appSetttings** do pliku **App.config**. Te ustawienia są używane przez metodę pomocy **GetAuthorizationHeader**. 

    Zastąp wartości **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;** i **&lt;tenant ID&gt;** własnymi wartościami. 

        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <!-- Replace the following values with your own -->
            <add key="ApplicationId" value="<Application ID>" />
            <add key="Password" value="<Password>" />    
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="tenant ID" />
        </appSettings>
6. Dodaj następujące instrukcje **using** do pliku źródłowego (Program.cs) w projekcie.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Dodaj następujący kod, który tworzy wystąpienie klasy **DataPipelineManagementClient** w metodzie **Main**. Ten obiekt jest używany do tworzenia fabryki danych, połączonej usługi, zestawów danych wejściowych i wyjściowych oraz potoku. Tego obiektu można również używać do monitorowania wycinków zestawu danych w czasie wykonywania.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > Zastąp wartość **resourceGroupName** nazwą grupy zasobów platformy Azure. 
    > 
    > Zaktualizuj nazwę fabryki danych (**dataFactoryName**), aby była unikatowa. Nazwa fabryki danych musi być globalnie unikatowa. Artykuł [Data Factory — Naming Rules](data-factory-naming-rules.md) (Fabryka danych — zasady nazewnictwa) zawiera zasady nazewnictwa artefaktów usługi Fabryka danych. 

7. Dodaj następujący kod, który tworzy **fabrykę danych**, do metody **Main**.

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

8. Dodaj następujący kod, który tworzy **usługę połączoną usługi Azure Storage**, do metody **Main**. 

    > [AZURE.IMPORTANT] Zastąp wartości **storageaccountname** i **accountkey** nazwą konta usługi Azure Storage oraz jego kluczem. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Dodaj następujący kod, który tworzy **usługę połączoną Azure SQL**, do metody **Main**.
 
    > [AZURE.IMPORTANT] Zastąp wartości **servername**, **databasename**, **username** oraz **password** nazwą serwera SQL Azure, nazwą bazy danych, nazwą użytkownika i hasłem.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Dodaj następujący kod, który tworzy **wejściowe i wyjściowe zestawy danych**, do metody **Main**. 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
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


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Dodaj następujący kod, który **tworzy i aktywuje potok**, do metody **Main**. Ten potok zawiera działanie **CopyActivity**, które pobiera element **BlobSource** jako źródło i **BlobSink** jako ujście.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

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
                                    Name = "BlobToAzureSql",
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

12. Dodaj następujący kod do metody **Main**, aby pobrać stan wycinka danych zestawu danych wyjściowych. W tym przykładzie oczekiwany jest tylko wycinek.   
 
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

14. Dodaj następujący kod do metody **Main**, aby uzyskać szczegóły uruchomienia dla wycinka danych.

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

13. Dodaj następującą metodę pomocnika używaną przez metodę **Main** do klasy **Program**.  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
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


15. W Eksploratorze rozwiązań rozwiń projekt (**DataFactoryAPITestApp**), kliknij prawym przyciskiem myszy pozycję **Odwołania** i kliknij polecenie **Dodaj odwołanie**. Zaznacz pole wyboru zestawu „**System.Configuration**” i kliknij przycisk **OK**. 
16. Skompiluj aplikację konsolową. Kliknij przycisk **Kompiluj** w menu i kliknij opcję **Kompiluj rozwiązanie**. 
16. Potwierdź, że istnieje co najmniej jeden plik w kontenerze **adftutorial** w magazynie obiektów blob platformy Azure. W przeciwnym razie utwórz w Notatniku plik **Emp.txt** o następującej treści i przekaż go do kontenera adftutorial.

        John, Doe
        Jane, Doe
     
17. Uruchom próbkę, klikając pozycję **Debuguj** -> **Rozpocznij debugowanie** w menu. Po wyświetleniu komunikatu **Pobieranie szczegółów uruchomienia wycinka danych** zaczekaj kilka minut, a następnie naciśnij klawisz **ENTER**. 
18. Użyj witryny Azure Portal, aby upewnić się, że fabryka danych **APITutorialFactory** została utworzona z następującymi artefaktami: 
    - Połączona usługa: **LinkedService_AzureStorage** 
    - Zestaw danych: **DatasetBlobSource** i **DatasetBlobDestination**.
    - Potok: **PipelineBlobSample** 
18. Sprawdź, czy plik wyjściowy został utworzony w folderze „**apifactoryoutput**” w kontenerze **adftutorial**.

## Następne kroki

- Przeczytaj artykuł [Data Movement Activities](data-factory-data-movement-activities.md) (Działania przenoszenia danych), który zawiera szczegółowe informacje dotyczące działania kopiowania używanego w tym samouczku.
- Szczegółowe informacje na temat zestawu SDK .NET usługi Data Factory zamieszczono w temacie [Data Factory .NET API Reference](https://msdn.microsoft.com/library/mt415893.aspx) (Informacje o interfejsie API .NET usługi Data Factory). Ten artykuł nie obejmuje całego interfejsu API .NET usługi Data Factory. 

 



<!--HONumber=Sep16_HO5-->


