---
title: "Przetwarzania dużych zestawów danych przy użyciu fabryki danych i partii | Dokumentacja firmy Microsoft"
description: "Zawiera opis sposobu przetwarzania dużych ilości danych w potoku fabryki danych Azure przy użyciu możliwości przetwarzania równoległego partii zadań Azure."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 2ceef65eaf195b605fada2f8dfe511fe33a5daa0
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a>Przetwarzanie dużych ilości danych za pomocą usług Data Factory i Batch
> [!NOTE]
> Ten artykuł dotyczy wersji 1 usługi Data Factory, która jest ogólnie dostępna (GA). Jeśli używasz wersji 2 usługi fabryka danych, która jest w wersji zapoznawczej, zobacz [działań niestandardowych z fabryki danych w wersji 2](../transform-data-using-dotnet-custom-activity.md).

W tym artykule opisano architekturę rozwiązania próbki, które przenosi i przetwarzania dużych zestawów danych w sposób automatycznego i zaplanowane. Umożliwia także wskazówki end-to-end, aby zaimplementować to rozwiązanie przy użyciu fabryki danych Azure i partii zadań Azure.

W tym artykule jest dłuższa niż artykułem typowe, ponieważ zawiera ona wskazówki przykładowe całego rozwiązania. Jeśli jesteś nowym użytkownikiem usługi partia zadań i fabryki danych, informacje na temat tych usług i jak one współdziałają ze sobą. Jeśli znasz coś o usługach i są projektowanie/zaprojektowanie rozwiązania, użytkownik może skupić się tylko na [sekcji architektura](#architecture-of-sample-solution) artykułu i jeśli tworzysz prototypu lub rozwiązania może również chcesz wypróbować instrukcje krok po kroku w [wskazówki](#implementation-of-sample-solution). Zapraszamy komentarze dotyczące tej zawartości i sposobie używania jej.

Po pierwsze Zobaczmy, jak może pomóc fabryki danych i instancji usługi przy użyciu przetwarzania dużych zestawów danych w chmurze.     

## <a name="why-azure-batch"></a>Dlaczego usługa partia zadań Azure?
Usługa Azure Batch umożliwia wydajne uruchamianie aplikacji równoległych o wielkiej skali oraz aplikacji do obliczeń o wysokiej wydajności (HPC, High-Performance Computing). Jest to usługa platformy, która umożliwia planowanie pracy wymagającej intensywnych obliczeń do wykonania na zarządzanym zestawie maszyn wirtualnych oraz automatyczne skalowanie zasobów obliczeniowych w celu spełnienia wymagań związanych z zadaniami.

W usłudze Batch definiuje się zasoby obliczeniowe Azure do wykonywania aplikacji równolegle i na dużą skalę. Można uruchamiać zadania zaplanowane i na żądanie oraz nie ma potrzeby ręcznego tworzenia i konfigurowania klastra HPC, poszczególnych maszyn wirtualnych, sieci wirtualnych lub złożonej infrastruktury planowania zadań i podzadań ani zarządzania nimi.

Zobacz następujące artykuły, jeśli nie masz doświadczenia z partii zadań Azure jako ułatwia zrozumienie architektury/implementacji rozwiązania opisane w tym artykule.   

* [Podstawowe informacje o partii zadań Azure](../../batch/batch-technical-overview.md)
* [Przegląd funkcji partii](../../batch/batch-api-basics.md)

(opcjonalnie) Aby dowiedzieć się więcej na temat partii zadań Azure, zobacz [ścieżka szkoleniowa dotycząca partii zadań Azure](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Dlaczego fabryki danych Azure?
Fabryka danych jest usługą integracji danych w chmurze, która służy do aranżacji i automatyzacji przenoszenia i przekształcania danych. Przy użyciu usługi fabryka danych, można utworzyć potoki zarządzanych danych, które przenoszenia danych z lokalnego i w chmurze magazyny danych w magazynie danych scentralizowane (na przykład: magazyn obiektów Blob Azure) i procesu/Przekształcanie danych za pomocą usług, takich jak Azure HDInsight i uczenia maszynowego Azure. Można również zaplanować potoki danych do uruchamiania w zaplanowanym czasie (co godzinę, codziennie, co tydzień, itp.) i monitor i zarządzać nimi w skrócie do identyfikowania problemów i podejmij akcję.

Zobacz następujące artykuły, jeśli nie masz doświadczenia z fabryką danych Azure jako ułatwia zrozumienie architektury/implementacji rozwiązania opisane w tym artykule.  

* [Wprowadzenie do fabryki danych Azure](data-factory-introduction.md)
* [Tworzenie swój pierwszy potok danych](data-factory-build-your-first-pipeline.md)   

(opcjonalnie) Aby dowiedzieć się więcej na temat fabryki danych Azure, zobacz [ścieżka szkoleniowa dotycząca fabryki danych Azure](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Fabryki danych i partii razem
Fabryka danych zawiera wbudowane działania, takie jak działanie kopiowania do skopiowania/przeniesienia danych z magazynu danych źródłowych w magazynie danych docelowym i Hive działania do przetwarzania danych na platformie Azure przy użyciu klastrów platformy Hadoop (HDInsight). Zobacz [działań przekształcania danych](data-factory-data-transformation-activities.md) listę obsługiwanych transformacji działania.

Można też utworzyć niestandardowe działania .NET do przeniesienia i przetwarzania danych na własną logikę te działania są uruchomione w klastrze usługi HDInsight Azure lub puli partii zadań Azure maszyn wirtualnych. Korzystając z partii zadań Azure, można skonfigurować do automatycznego skalowania puli (Dodawanie lub usuwanie maszyn wirtualnych, w zależności od obciążenia) na podstawie formuły, musisz podać.     

## <a name="architecture-of-sample-solution"></a>Architektura przykładowe rozwiązanie
Mimo że architektury opisanej w tym artykule jest prostym rozwiązaniem, ma zastosowanie w złożonych scenariuszy, takich jak modelowania branży usług finansowych, przetwarzania obrazów i renderowania i genomiczne analizy ryzyka.

Diagram przedstawia 1) jak fabryki danych organizuje przenoszenia danych i przetwarzania i 2) jak partii zadań Azure przetwarza dane w sposób równoległy. Pobrać i wydrukować diagram dla ułatwienia (11 x 17 cali. lub rozmiaru A3): [aranżacji HPC i danych przy użyciu partii zadań Azure i fabryki danych](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Diagram przetwarzania danych na dużą skalę](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

Poniższa lista zawiera podstawowe kroki procesu. Rozwiązanie zawiera kod i wyjaśnienia dotyczące rozwiązania end-to-end.

1. **Skonfiguruj partii zadań Azure z pulą węzłów obliczeniowych (VM)**. Można określić liczbę węzłów i rozmiar każdego węzła.
2. **Tworzy wystąpienie fabryki danych Azure** skonfigurowanego jednostek, które reprezentują magazynu obiektów blob platformy Azure, usługa obliczeniowych partii zadań Azure danych wejścia/wyjścia i przepływu pracy/potoku z działaniami, które Przenieś i przekształcania danych.
3. **Tworzenie niestandardowego działania .NET w potoku fabryki danych**. Działanie jest swój kod użytkownika, który jest uruchamiany w puli partii zadań Azure.
4. **Przechowywania dużych ilości danych wejściowych jako obiekty BLOB w magazynie Azure**. Danych jest podzielona na wycinków logiczne (zazwyczaj za czas).
5. **Fabryka danych kopiuje dane są przetwarzane równolegle** do lokalizacji dodatkowej.
6. **Fabryka danych uruchamia to niestandardowe działanie przy użyciu puli przydzielone przez partię**. Fabryka danych można uruchomić jednocześnie działań. Każde działanie przetwarza wycinka danych. Wyniki są przechowywane w magazynie Azure.
7. **Fabryka danych przenosi wyniki końcowe do lokalizacji trzeci**, do dystrybucji za pośrednictwem aplikacji lub dla dalszego przetwarzania przez inne narzędzia.

## <a name="implementation-of-sample-solution"></a>Implementacja przykładowe rozwiązanie
Przykładowe rozwiązanie jest celowo proste i jak do fabryki danych i partii razem przetwarzania zestawów danych. Rozwiązanie po prostu zlicza wystąpienia terminu wyszukiwania ("Microsoft") w organizacji w szeregów czasowych plików wejściowych. Generuje on liczbę do plików wyjściowych.

**Czas**: Jeśli znasz już podstawy usługi Azure, fabryki danych i partii i zakończył wymagania wstępne wymienione poniżej, firma Microsoft oszacować to rozwiązanie ma 1 – 2 godz. Aby zakończyć.

### <a name="prerequisites"></a>Wymagania wstępne
#### <a name="azure-subscription"></a>Subskrypcja platformy Azure
Jeśli nie masz subskrypcji platformy Azure, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Zobacz [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Konto magazynu Azure
Używasz konta magazynu Azure do przechowywania danych w tym samouczku. Jeśli nie masz konta magazynu platformy Azure, zobacz [Utwórz konto magazynu](../../storage/common/storage-create-storage-account.md#create-a-storage-account). Przykładowe rozwiązanie korzysta z magazynu obiektów blob.

#### <a name="azure-batch-account"></a>Konto usługi partia zadań Azure
Utwórz konto partii zadań Azure za pomocą [portalu Azure](http://portal.azure.com/). Zobacz [tworzenie i zarządzanie nimi konto partii zadań Azure](../../batch/batch-account-create-portal.md). Zanotuj klucz nazwy i konta konto partii zadań Azure. Można również użyć [AzureRmBatchAccount nowy](https://msdn.microsoft.com/library/mt603749.aspx) polecenia cmdlet, aby utworzyć konto partii zadań Azure. Zobacz [wprowadzenie do poleceń cmdlet programu PowerShell usługi partia zadań Azure](../../batch/batch-powershell-cmdlets-get-started.md) szczegółowe instrukcje na temat używania tego polecenia cmdlet.

Przykładowe rozwiązanie używa partii zadań Azure (pośrednio przez potok fabryki danych Azure) do przetwarzania danych w sposób równoległy w puli węzłów obliczeniowych (zarządzanej kolekcji maszyn wirtualnych).

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a>Azure puli partii maszyn wirtualnych (VM)
Utwórz **puli partii zadań Azure** z co najmniej 2 węzły obliczeniowe.

1. W [portalu Azure](https://portal.azure.com), kliknij przycisk **Przeglądaj** w menu po lewej stronie, a następnie kliknij przycisk **konta usługi partia zadań**.
2. Wybierz konto partii zadań Azure, aby otworzyć **konta usługi partia zadań** bloku.
3. Kliknij przycisk **pule** kafelka.
4. W **pule** bloku, kliknij przycisk Dodaj, aby dodać pulę na pasku narzędzi.
   1. Wpisz identyfikator puli (**identyfikator puli**). Uwaga **identyfikator puli**; potrzebne podczas tworzenia rozwiązania fabryki danych.
   2. Określ **systemu Windows Server 2012 R2** ustawienia rodziny systemów operacyjnych.
   3. Wybierz **warstwę cenową węzła**.
   4. Wprowadź **2** wartości dla **docelowego w wersji dedykowanej** ustawienie.
   5. Wprowadź **2** wartości dla **maksymalna liczba zadań na węzeł** ustawienie.
   6. Kliknij przycisk **OK**, aby utworzyć pulę.

#### <a name="azure-storage-explorer"></a>Eksplorator usługi Azure Storage
[Azure magazynu Explorer 6 (Narzędzia)](https://azurestorageexplorer.codeplex.com/) lub [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (z oprogramowania ClumsyLeaf). Te narzędzia do sprawdzania i zmiany danych w projektach usługi Azure Storage, w tym dzienniki aplikacji hostowanych w chmurze.

1. Utworzyć kontener o nazwie **mojkontener** prywatny dostęp (Brak dostępu anonimowego)
2. Jeśli używasz **CloudXplorer**, tworzyć foldery i podfoldery o następującej strukturze:

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder`i `outputfolder` są folderów najwyższego poziomu w `mycontainer`. `inputfolder` Ma podfoldery z sygnaturami daty i godziny (RRRR-MM-DD-HH).

   Jeśli używasz **Eksploratora usługi Storage Azure**, w następnym kroku należy przekazać pliki z nazwami: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` i tak dalej. Ten krok automatycznie tworzy foldery.
3. Utwórz plik tekstowy **plik.txt** na komputerze z zawartością, która ma kluczowego **Microsoft**. Na przykład: "test niestandardowe aktywności testów Microsoft działania niestandardowego Microsoft".
4. Przekaż plik do następujących folderów wejściowego w magazynie obiektów blob platformy Azure.

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   Jeśli używasz **Eksploratora usługi Storage Azure**, Przekaż plik **plik.txt** do **mojkontener**. Kliknij przycisk **kopiowania** na pasku narzędzi, aby utworzyć kopię obiektu blob. W **kopiowania obiektu Blob** okno dialogowe, zmień **Nazwa docelowego obiektu blob** do `inputfolder/2015-11-16-00/file.txt`. Powtórz ten krok, aby utworzyć `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` i tak dalej. Ta akcja powoduje automatyczne utworzenie foldery.
5. Tworzenie kontenera o nazwie: `customactivitycontainer`. Możesz przekazać plik zip działań niestandardowych do tego kontenera.

#### <a name="visual-studio"></a>Visual Studio
Zainstaluj program Microsoft Visual Studio 2012 lub nowszym można utworzyć niestandardowego działania dotyczącego partii usługi do użycia w rozwiązaniu fabryki danych.

### <a name="high-level-steps-to-create-the-solution"></a>Ogólne kroki umożliwiające utworzenie rozwiązania
1. Tworzenie niestandardowego działania, który zawiera logikę przetwarzania danych.
2. Tworzenie fabryki danych Azure, który korzysta z działań niestandardowych:

### <a name="create-the-custom-activity"></a>Tworzenie niestandardowego działania
Niestandardowe działanie fabryki danych jest bardzo ważne dla tego rozwiązania próbki. Przykładowe rozwiązanie używa partii zadań Azure do uruchomienia działania niestandardowego. Zobacz [skorzystać z działań niestandardowych w potoku fabryki danych Azure](data-factory-use-custom-activities.md) podstawowe informacje z opracowywanie niestandardowych działań i używać ich w potokach fabryki danych Azure.

Aby utworzyć .NET działań niestandardowych, którego można używać w potoku fabryki danych Azure, musisz utworzyć **Biblioteka klas programu .NET** projektu z klasy, która implementuje, który **IDotNetActivity** interfejsu. Ten interfejs jest tylko jedna metoda: **Execute**. Podpis metody jest następujący:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

Metoda ma kilka kluczowych składników, które należy zrozumieć.

* Metoda przyjmuje cztery parametry:

  1. **linkedServices**. Wyliczalny lista połączonych usług połączonych źródeł danych wejścia/wyjścia (na przykład: magazyn obiektów Blob Azure) z fabryką danych. W tym przykładzie istnieje tylko jeden połączonej usługi typu usługi Azure Storage używane dla danych wejściowych i wyjściowych.
  2. **zestawy danych**. Jest to lista wyliczalny zestawów danych. Ten parametr umożliwia pobieranie lokalizacji i schematy wynika z zestawów danych wejściowych i wyjściowych.
  3. **działanie**. Tego parametru reprezentuje jednostkę bieżącego obliczeniowej — w tym przypadku usługi partia zadań Azure.
  4. **Rejestrator**. Rejestrator umożliwia zapisanie komentarze debugowania tej powierzchni logowania "Użytkownika" dla potoku.
* Metoda zwraca słownik, który może służyć do łańcuch niestandardowych działań w przyszłości. Ta funkcja nie jest jeszcze zaimplementowana, więc Zwróć pusty słownik z metody.

#### <a name="procedure-create-the-custom-activity"></a>Procedura: Tworzenie niestandardowego działania
1. Utwórz projekt Biblioteka klas programu .NET w programie Visual Studio.

   1. Uruchom **programu Visual Studio 2012**/**2013/2015**.
   2. Kliknij pozycję **Plik**, wskaż polecenie **Nowy** i kliknij pozycję **Projekt**.
   3. Rozwiń węzeł **szablony**i wybierz **Visual C\#**. W tym przewodniku, należy użyć serwera C\#, ale żadnego języka .NET umożliwia tworzenie działań niestandardowych.
   4. Wybierz **biblioteki klas** z listy typów projektów po prawej stronie.
   5. Wprowadź **MyDotNetActivity** dla **nazwa**.
   6. Wybierz **C:\\ADF** dla **lokalizacji**. Utwórz folder **ADF** Jeśli nie istnieje.
   7. Kliknij przycisk **OK**, aby utworzyć projekt.
2. Kliknij pozycję **Narzędzia**, wskaż pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **Konsola menedżera pakietów**.
3. W **Konsola Menedżera pakietów**, uruchom następujące polecenie, aby zaimportować **Microsoft.Azure.Management.DataFactories**.

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Importuj **usługi Azure Storage** pakiet NuGet do projektu. Ten pakiet jest potrzebna, ponieważ należy używać magazynu obiektów Blob interfejsu API w tym przykładzie.

    ```powershell
    Install-Package Azure.Storage
    ```
5. Dodaj następujące **przy użyciu** dyrektywy do pliku źródłowego w projekcie.

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;
    
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Zmień nazwę **przestrzeni nazw** do **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Zmień nazwę klasy, która ma **MyDotNetActivity** i pochodzi ona z **IDotNetActivity** interfejsu, jak pokazano poniżej.

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Implementowanie (Dodaj) **Execute** metody **IDotNetActivity** interfejsu do **MyDotNetActivity** klasy i skopiuj następujący kod do metody. Zobacz [wykonywanie metody](#execute-method) sekcji wyjaśnienie przez logikę używaną w ramach tej metody.

    ```csharp
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
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
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
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob to the folder: {0}", folderPath);
    
       // create a storage object for the output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write the name of the file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
       logger.Write("output blob URI: {0}", outputBlobUri.ToString());
       // create a blob and upload the output text.
       CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
       logger.Write("Writing {0} to the output blob", output);
       outputBlob.UploadText(output);
    
       // The dictionary can be used to chain custom activities together in the future.
       // This feature is not implemented yet, so just return an empty dictionary.
       return new Dictionary<string, string>();
    }
    ```
9. Dodaj następujące metody pomocnika do klasy. Te metody są wywoływane przez **Execute** metody. Przede wszystkim **Calculate** metody izoluje kodu, który iteruje po każdy obiekt blob.

    ```csharp
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
    /// and prepares the output text that is written to the output blob.
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
    ```
    **GetFolderPath** metoda zwraca ścieżkę do folderu, który wskazuje element dataset i **GetFileName** metoda zwraca nazwę obiekt blob/pliku wskazujące zestaw danych.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    **Calculate** metody oblicza liczbę wystąpień słowa kluczowego **Microsoft** w pliki wejściowe (obiekty BLOB w folderze). Wyszukiwany termin ("Microsoft") jest ustalony w kodzie.

1. Skompiluj projekt. Kliknij przycisk **kompilacji** w menu, kliknij **Kompiluj rozwiązanie**.
2. Uruchom **Eksploratora Windows**i przejdź do **bin\\debugowania** lub **bin\\wersji** folder, w zależności od typu kompilacji.
3. Utwórz plik zip **MyDotNetActivity.zip** zawierający wszystkie pliki binarne w  **\\bin\\debugowania** folderu. Możesz dołączyć MyDotNetActivity. **pdb** pliku, tak aby uzyskać dodatkowe szczegóły, takie jak numer wiersza w kodzie źródłowym, która spowodowała problem, gdy wystąpi błąd.

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. Przekaż **MyDotNetActivity.zip** jako obiekt blob do kontenera obiektów blob: `customactivitycontainer` na platformie Azure magazynu obiektów blob który **StorageLinkedService** połączonej usługi w **ADFTutorialDataFactory** używa. Tworzenie kontenera obiektów blob `customactivitycontainer` Jeśli jeszcze nie istnieje.

#### <a name="execute-method"></a>Execute — Metoda
Ta sekcja zawiera bardziej szczegółowe informacje i notatki dotyczące kodu metody Execute.

1. Elementy iteracji w kolekcji wejściowych znajdują się w [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) przestrzeni nazw. Iteracji w kolekcji obiektów blob wymaga użycia **BlobContinuationToken** klasy. W zasadzie, należy użyć-pętli z tokenem jako mechanizm wyjścia z pętli while. Aby uzyskać więcej informacji, zobacz [jak używać magazynu obiektów Blob z .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md). Podstawowe pętli jest następujący:

    ```csharp
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

    ```
   Zajrzyj do dokumentacji [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) metody, aby uzyskać szczegółowe informacje.
2. Kod do pracy za pomocą zestawu obiektów blob logicznie przechodzi w ramach nie-pętli while. W **Execute** metody, nie-podczas pętli przekazuje listę obiektów blob do metodę o nazwie **Calculate**. Metoda zwraca ciąg zmiennej o nazwie **dane wyjściowe** będący wynikiem o iterowane przez wszystkie obiekty BLOB w segmencie.

   Zwraca liczbę wystąpień wyszukiwanego (**Microsoft**) w obiekcie blob przekazany do **Calculate** metody.

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. Raz **Calculate** metody przeprowadził pracy, jego musi być przystosowana do nowego obiektu blob. Dlatego dla każdego zestawu obiektów blob przetwarzane nowego obiektu blob można pisać z wynikami. Można zapisać do nowego obiektu blob, najpierw należy znaleźć wyjściowego zestawu danych.

    ```csharp
    // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. Kod również wywołuje metodę pomocnika: **GetFolderPath** pobierania ścieżki folderu (nazwa kontenera magazynu).

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   **GetFolderPath** rzutuje na AzureBlobDataSet, który ma właściwość o nazwie FolderPath obiektu DataSet.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. Kod wywołuje **GetFileName** metodę, aby pobrać nazwę pliku (obiektów blob). Kod jest podobny do kodu powyżej można uzyskać ścieżki folderu.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. Nazwa pliku są zapisywane przez utworzenie obiekt URI. Identyfikator URI Konstruktor korzysta **BlobEndpoint** właściwość, aby zwrócić nazwę kontenera. Nazwa i ścieżka pliku folderu są dodawane do skonstruowania identyfikator URI obiektu blob danych wyjściowych.  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. Nazwa pliku została zapisana, i teraz możesz zapisywać ciąg dane wyjściowe z **Calculate** metody do nowego obiektu blob:

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a>Tworzenie fabryki danych
W [tworzenia działań niestandardowych](#create-the-custom-activity) sekcji utworzone niestandardowe działania i przekazać plik zip z plikami binarnymi i pliku PDB do kontenera obiektów blob platformy Azure. W tej sekcji utworzysz Azure **fabryki danych** z **potoku** używającą **działania niestandardowego**.

Wejściowy zestaw danych dla działań niestandardowych reprezentuje obiektów blob (pliki) w folderze wejściowych (`mycontainer\\inputfolder`) w magazynie obiektów blob. Zestaw danych wyjściowych dla działania reprezentuje obiekty BLOB danych wyjściowych w folderze wyjściowym (`mycontainer\\outputfolder`) w magazynie obiektów blob.

Upuść jeden lub więcej plików w folderach wejściowych:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Na przykład usunąć jeden plik (plik.txt) o następującej zawartości do poszczególnych folderów.

```
test custom activity Microsoft test custom activity Microsoft
```

Każdego folderu wejściowych odpowiada wycinka w fabryce danych Azure, nawet wtedy, gdy znajduje się w nim pliki 2 lub większą. Każdy wycinek jest przetwarzany przez potok, działania niestandardowego iterację wszystkich obiektów blob w folderze wejściowych dla tego wycinka.

Zobaczysz pięć plików wyjściowych o tej samej zawartości. Na przykład plik wyjściowy przetwarzanie pliku w folderze 2015-11-16-00 ma następującą zawartość:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

Jeśli zgubisz wiele plików (plik.txt, Plik2.txt file3.txt) o tej samej zawartości do folderu wejściowych zobaczysz następującą zawartość w pliku wyjściowym. Każdego folderu (2015-11-16-00 itp.) odpowiada wycinka w tym przykładzie, mimo że znajduje się w nim wiele plików wejściowych.

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

Plik wyjściowy ma trzy wiersze teraz, po jednej dla każdego pliku wejściowego (blob) w folderze skojarzone z slice (2015-11-16-00).

Zadanie jest tworzone dla każdej uruchamiania działania. W tym przykładzie istnieje tylko jedno działanie w potoku. Podczas wycinek jest przetwarzany przez potok, niestandardowe działanie jest uruchomione w partii zadań Azure do przetwarzania wycinka. Ponieważ pięć wycinków (każdy wycinek może mieć wielu obiektów blob lub pliku), jest pięć zadań utworzona w partii zadań Azure. Po uruchomieniu zadania w partii, jest działanie niestandardowego, który jest uruchomiony.

Poniższe wskazówki zawiera dodatkowe szczegóły.

#### <a name="step-1-create-the-data-factory"></a>Krok 1: Tworzenie fabryki danych
1. Po zalogowaniu się do [portalu Azure](https://portal.azure.com/), wykonaj następujące czynności:

   1. Kliknij przycisk **NOWY** w menu po lewej stronie.
   2. Kliknij przycisk **dane i analiza** w **nowy** bloku.
   3. Kliknij przycisk **Fabryka danych** w bloku **Analiza danych**.
2. W **nowa fabryka danych** bloku, wprowadź **CustomActivityFactory** dla nazwy. Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli zostanie wyświetlony błąd: **nazwa fabryki danych "CustomActivityFactory" nie jest dostępna**, Zmień nazwę fabryki danych (na przykład **yournameCustomActivityFactory**), a następnie spróbuj ponownie utworzyć.
3. Kliknij przycisk **Nazwa grupy zasobów**i wybierz istniejącą grupę zasobów lub Utwórz grupę zasobów.
4. Sprawdź, czy używasz poprawne subskrypcji i regionu, w którym ma fabryki danych do utworzenia.
5. Kliknij przycisk **Utwórz** w bloku **Nowa fabryka danych**.
6. Zobacz tworzony w fabryce danych **pulpitu nawigacyjnego** portalu Azure.
7. Po pomyślnym utworzeniu fabryki danych zostanie wyświetlona strona fabryki danych z zawartością fabryki danych.

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Krok 2: Tworzenie usługi połączonej
Połączone usługi łączą magazyny danych lub usługi obliczeniowe z fabryką danych Azure. W tym kroku zostanie połączony z **usługi Azure Storage** konta i **partii zadań Azure** konta z fabryką danych.

#### <a name="create-azure-storage-linked-service"></a>Tworzenie połączonej usługi Azure Storage
1. Kliknij przycisk **tworzenie i wdrażanie** Kafelek na **FABRYKI danych** bloku **CustomActivityFactory**. Zostanie wyświetlony Edytor fabryki danych.
2. Kliknij przycisk **nowy magazyn danych** polecenie Pasek i wybierz polecenie **magazynu Azure.** Powinien zostać wyświetlony skrypt JSON do tworzenia połączonej usługi Azure Storage w edytorze.

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. Zastąp wartość **account name** nazwą konta usługi Azure Storage oraz wartość **account key** kluczem dostępu konta usługi Azure Storage. Informacje na temat pobierania klucza dostępu do magazynu znajdują się w artykule [Wyświetlanie, kopiowanie i ponowne generowanie kluczy dostępu do magazynu](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

4. Kliknij przycisk **Wdróż** na pasku poleceń, aby wdrożyć połączoną usługę.

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a>Tworzenie usługi partia zadań Azure połączone
W tym kroku możesz utworzyć połączonej usługi dla Twojego **partii zadań Azure** konta używanego do uruchamiania działań niestandardowych fabryki danych.

1. Kliknij przycisk **nowych obliczeń** polecenie Pasek i wybierz polecenie **partii zadań Azure.** Powinna zostać wyświetlona skryptu JSON do tworzenia usługi partia zadań Azure połączone w edytorze.
2. W skrypcie JSON:

   1. Zastąp **nazwa konta** o nazwie konto partii zadań Azure.
   2. Zastąp **klucz dostępu** kluczem dostępu do konta partii zadań Azure.
   3. Wprowadź identyfikator puli dla **poolName** właściwości**.** Dla tej właściwości można określić albo nazwę puli lub puli identyfikatora.
   4. Wprowadź identyfikator URI usługi partia zadań dla **batchUri** właściwości JSON.

      > [!IMPORTANT]
      > **Adres URL** z **bloku konta usługi partia zadań Azure** znajduje się w następującym formacie: \<accountname\>.\< region\>. batch.azure.com. Aby uzyskać **batchUri** właściwości w formacie JSON, musisz **Usuń "accountname."** z adresu URL. Przykład: `"batchUri": "https://eastus.batch.azure.com"`.
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      Aby uzyskać **poolName** właściwości, można również określić identyfikator puli zamiast nazwy puli.

      > [!NOTE]
      > Usługi fabryka danych nie obsługuje opcji na żądanie dla partii zadań Azure, w przeciwieństwie do usługi HDInsight. Pulę partii zadań Azure można używać tylko w fabryce danych Azure.
      >
      >
   5. Określ **StorageLinkedService** dla **linkedServiceName** właściwości. Tej połączonej usługi jest utworzony w poprzednim kroku. Ten magazyn jest używany jako obszaru przemieszczania dla plików i dzienników.
3. Kliknij przycisk **Wdróż** na pasku poleceń, aby wdrożyć połączoną usługę.

#### <a name="step-3-create-datasets"></a>Krok 3: Tworzenie zestawów danych
W tym kroku możesz utworzyć zestawy danych do reprezentowania danych wejściowych i wyjściowych.

#### <a name="create-input-dataset"></a>Tworzenie wejściowego zestawu danych
1. W **edytor** dla fabryki danych, kliknij przycisk **nowy zestaw danych** przycisk na pasku narzędzi i kliknij przycisk **magazynu obiektów Blob Azure** z menu rozwijanego.
2. Zastąp dane JSON w okienku po prawej stronie następujący fragment kodu JSON:

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
               },
               "partitionedBy": [
                   {
                       "name": "Year",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy"
                       }
                   },
                   {
                       "name": "Month",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "MM"
                       }
                   },
                   {
                       "name": "Day",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "dd"
                       }
                   },
                   {
                       "name": "Hour",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           },
           "external": true,
           "policy": {}
       }
    }
    ```

    Utworzyć potok w dalszej części tego przewodnika, czas rozpoczęcia: 2015-11-16T00:00:00Z i na końcu czasu: 2015-11-16T05:00:00Z. Planuje wyprodukować danych **co godzinę**, więc istnieje 5 wycinków wejścia/wyjścia (między **00**: 00:00 -\> **05**: 00:00).

    **Częstotliwość** i **interwał** dla zestawu danych wejściowych jest ustawiony na **godzina** i **1**, co oznacza, że wejściowy wycinek jest dostępny co godzinę.

    Poniżej przedstawiono godziny rozpoczęcia dla każdego wycinka, które jest reprezentowana przez **SliceStart** zmiennej systemowej w powyższym fragment kodu JSON.

    | **Wycinek** | **Godzina rozpoczęcia**          |
    |-----------|-------------------------|
    | 1         | 2015-11-16T**00**: 00:00 |
    | 2         | 2015-11-16T**01**: 00:00 |
    | 3         | 2015-11-16T**02**: 00:00 |
    | 4         | 2015-11-16T**03**: 00:00 |
    | 5         | 2015-11-16T**04**: 00:00 |

    **FolderPath** jest obliczana przy użyciu rok, miesiąc, dzień i godzinę część wycinek czas rozpoczęcia (**SliceStart**). W związku z tym Oto jak folder wejściowy jest mapowany na wycinek.

    | **Wycinek** | **Godzina rozpoczęcia**          | **Folder wejściowy**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015-11-16T**00**: 00:00 | 2015-11-16-**00** |
    | 2         | 2015-11-16T**01**: 00:00 | 2015-11-16-**01** |
    | 3         | 2015-11-16T**02**: 00:00 | 2015-11-16-**02** |
    | 4         | 2015-11-16T**03**: 00:00 | 2015-11-16-**03** |
    | 5         | 2015-11-16T**04**: 00:00 | 2015-11-16-**04** |

1. Kliknij przycisk **Wdróż** na pasku narzędzi, aby utworzyć i wdrożyć **InputDataset** tabeli.

#### <a name="create-output-dataset"></a>Tworzenie wyjściowego zestawu danych
W tym kroku utworzysz innego elementu dataset typu AzureBlob do reprezentowania danych wyjściowych.

1. W **edytor** dla fabryki danych, kliknij przycisk **nowy zestaw danych** przycisk na pasku narzędzi i kliknij przycisk **magazynu obiektów Blob Azure** z menu rozwijanego.
2. Zastąp dane JSON w okienku po prawej stronie następujący fragment kodu JSON:

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
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
    ```

    Obiekt blob/plik wyjściowy jest generowany dla każdego wejściowego wycinka. Oto, jak dla każdego wycinka nosi nazwę pliku wyjściowego. Wszystkie pliki wyjściowe są generowane w jednym folderze wyjściowym: `mycontainer\\outputfolder`.

    | **Wycinek** | **Godzina rozpoczęcia**          | **Plik wyjściowy**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015-11-16T**00**: 00:00 | 2015-11-16 -**00. txt** |
    | 2         | 2015-11-16T**01**: 00:00 | 2015-11-16 -**01. txt** |
    | 3         | 2015-11-16T**02**: 00:00 | 2015-11-16 -**02. txt** |
    | 4         | 2015-11-16T**03**: 00:00 | 2015-11-16 -**03. txt** |
    | 5         | 2015-11-16T**04**: 00:00 | 2015-11-16 -**04. txt** |

    Należy pamiętać, że wszystkie pliki w folderze wejściowych (na przykład: 2015-11-16-00) są częścią wycinek czas rozpoczęcia: 2015-11-16-00. Podczas przetwarzania tego wycinka działania niestandardowego skanowania za pomocą każdego pliku i tworzy wiersz w pliku danych wyjściowych z liczbą wystąpień terminu wyszukiwania ("Microsoft"). Jeśli istnieją trzy pliki w folderze 2015-11-16-00, istnieją trzy wiersze w pliku wyjściowym: 2015-11-16-00.txt.

1. Kliknij przycisk **Wdróż** na pasku narzędzi, aby utworzyć i wdrożyć **OutputDataset**.

#### <a name="step-4-create-and-run-the-pipeline-with-custom-activity"></a>Krok 4: Tworzenie i uruchamianie potoku z działań niestandardowych
W tym kroku możesz utworzyć potok z jednego działania, niestandardowego działania utworzony wcześniej.

> [!IMPORTANT]
> Jeśli nie zostały jeszcze przekazane **plik.txt** jako danych wejściowych folderów w kontenerze obiektów blob, zrobić przed utworzeniem potoku. **IsPaused** właściwość ma wartość false w potoku JSON, więc potoku uruchamia natychmiast jako **start** przypada w przeszłości.
>
>

1. W edytorze fabryki danych, kliknij polecenie **nowy potok** na pasku poleceń. Jeśli polecenie nie jest widoczne, kliknij **... (Wielokropek)**  go wyświetlać.
2. Zastąp dane JSON w okienku po prawej stronie poniższy skrypt JSON:

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
                   "inputs": [
                       {
                           "name": "InputDataset"
                       }
                   ],
                   "outputs": [
                       {
                           "name": "OutputDataset"
                       }
                   ],
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   Pamiętaj o następujących kwestiach:

   * Istnieje tylko jedno działanie w potoku, którym jest typu: **DotNetActivity**.
   * **AssemblyName** ma ustawioną nazwę biblioteki DLL: **MyDotNetActivity.dll**.
   * **Punkt wejścia** ustawiono **MyDotNetActivityNS.MyDotNetActivity**. Zasadniczo jest \<przestrzeni nazw\>.\< ClassName\> w kodzie.
   * **PackageLinkedService** ustawiono **StorageLinkedService** wskazującego do magazynu obiektów blob, który zawiera plik zip działania niestandardowego. Jeśli używasz różnych kont usługi Azure Storage dla wejścia/wyjścia plików i działania niestandardowego pliku zip, należy utworzyć inny połączoną usługą magazynu Azure. W tym artykule przyjęto założenie, że używasz tego samego konta magazynu Azure.
   * **PackageFile** ustawiono **customactivitycontainer/MyDotNetActivity.zip**. Jest w formacie: \<containerforthezip\>/\<nameofthezip.zip\>.
   * Trwa to niestandardowe działanie **InputDataset** jako dane wejściowe i **OutputDataset** jako dane wyjściowe.
   * **LinkedServiceName** wskazuje właściwości niestandardowe działania **AzureBatchLinkedService**, który informuje fabryki danych Azure, która to niestandardowe działanie musi być uruchamiane w partii zadań Azure.
   * **Współbieżności** ustawienie jest ważne. Jeśli używasz wartość domyślna to 1, nawet jeśli masz 2 lub obliczeniowe więcej węzłów w puli partii zadań Azure, są przetwarzane wycinków jeden po drugim. W związku z tym nie korzystasz z możliwości przetwarzania równoległego partii zadań Azure. Jeśli ustawisz **współbieżności** na wartość większą powiedz 2, oznacza to dwa wycinków (odpowiada dwa zadania w partii zadań Azure) mogą być przetwarzane w tym samym czasie, w którym to przypadku obie maszyny wirtualne w partii zadań Azure puli są wykorzystywane. W związku z tym odpowiednio ustawione właściwości współbieżności.
   * Tylko jedno zadanie (wycinek) jest wykonywana na maszynie Wirtualnej w dowolnym momencie domyślnie. Przyczyną jest fakt, że domyślnie **maksymalna zadania dla maszyny Wirtualnej** ma ustawioną wartość 1 dla puli partii zadań Azure. W ramach wymagań wstępnych utworzono pulę z tej właściwości wartości 2, co dwa wycinków fabryki danych może być uruchomiony na maszynie Wirtualnej, w tym samym czasie.

    -   **isPaused** właściwość jest domyślnie ustawiona na wartość false. Potok uruchamia natychmiast w tym przykładzie, ponieważ wycinków rozpoczęcia w przeszłości. Tę właściwość można ustawić wartość true, aby wstrzymać potoku i go ustawić ponownie na wartość false, aby uruchomić ponownie.

    -   **Start** czasu i **zakończenia** czasy są od siebie pięć godzin i wycinki są tworzone co godzinę, więc pięć wycinków są produkowane przez potok.

1. Kliknij przycisk **Wdróż** na pasku poleceń, aby wdrożyć potok.

#### <a name="step-5-test-the-pipeline"></a>Krok 5: Testowanie potoku
W tym kroku należy przetestować potoku upuszczanie plików w folderach wejściowego. Zacznijmy od testowania potoku za pomocą jednego pliku na jeden folder wejściowy.

1. W bloku fabryki danych w portalu Azure, kliknij **Diagram**.

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. W widoku diagramu, kliknij dwukrotnie plik wejściowy zestaw danych: **InputDataset**.

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. Powinny pojawić się **InputDataset** blok z wszystkich pięciu wycinków gotowe. Powiadomienie **czas rozpoczęcia WYCINEK** i **czas zakończenia WYCINEK** dla każdego wycinka.

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. W **widoku diagramu**, kliknij przycisk **OutputDataset**.
5. Powinny być widoczne czy wycinków pięć dane wyjściowe są w stanie gotowe, jeśli zostały już utworzone.

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. Użyj portalu Azure, aby wyświetlić **zadania** skojarzone z **wycinków** i sprawdzić, jakie maszyny Wirtualnej, każdy wycinek uruchomionego na. Zobacz [integracji fabryki danych i partii](#data-factory-and-batch-integration) sekcji, aby uzyskać szczegółowe informacje.
7. Powinny pojawić się pliki wyjściowe w `outputfolder` z `mycontainer` w Azure magazynu obiektów blob.

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   Powinny pojawić się pięć plików wyjściowych, po jednej dla każdego wejściowego wycinka. Każdy plik wyjściowy powinien mieć zawartość podobne do następujących danych wyjściowych:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   Na poniższym diagramie przedstawiono sposób mapowania wycinków fabryki danych do zadań w partii zadań Azure. W tym przykładzie wycinek ma tylko jeden przebieg.

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. Teraz spróbujmy z wieloma plikami w folderze. Tworzenie plików: **Plik2.txt**, **file3.txt**, **file4.txt**, i **file5.txt** o tej samej zawartości, tak jak plik.txt w folderze: **2015-11-06-01**.
9. W folderze wyjściowym **usunąć** pliku wyjściowego: **2015-11-16-01.txt**.
10. Teraz, w **OutputDataset** bloku, kliknij prawym przyciskiem myszy wycinek z **czas rozpoczęcia WYCINEK** ustawioną **2015-11-16 01:00:00 AM**i kliknij przycisk **Uruchom** do Uruchom ponownie/ponownie-process wycinka. Teraz wycinek ma pięć plików zamiast jeden plik.

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. Po uruchomieniu wycinka i jego stan jest **gotowe**, sprawdź zawartość pliku wyjściowego dla tego wycinka (**2015-11-16-01.txt**) w `outputfolder` z `mycontainer` w magazynie obiektów blob. Powinien istnieć jeden wiersz dla każdego pliku wycinka.

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Jeśli nie usunął dane wyjściowe pliku 2015-11-16-01.txt przed podjęciem próby z pięć plików wejściowych, zobaczysz jednego wiersza z poprzedniego uruchomienia wycinka i pięciu wierszach z bieżącego uruchomienia wycinka. Domyślnie zawartość jest dołączany do pliku wyjściowego, jeśli już istnieje.
>
>

#### <a name="data-factory-and-batch-integration"></a>Integracja z fabryki danych i usługi partia zadań
Usługi fabryka danych tworzy zadanie w partii zadań Azure o nazwie: `adf-poolname:job-xxx`.

![Fabryka danych Azure - zadań wsadowych](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Zadania w zadaniu jest tworzony przy każdym uruchomieniu działania wycinek. W przypadku 10 wycinków gotowy do przetwarzania 10 zadań są tworzone w zadaniu. Może mieć więcej niż jeden wycinek typu uruchamiania równolegle, jeśli masz wiele węzłów obliczeniowych w puli. Jeśli maksymalny zadań na węzeł obliczeń jest ustawiona na > 1, może istnieć więcej niż jeden wycinek uruchomionych na tej samej mocy obliczeniowej.

W tym przykładzie istnieje pięć wycinków, więc pięć zadań w partii zadań Azure. Z **współbieżności** ustawioną **5** w potoku JSON w fabryce danych Azure i **maksymalna zadania dla maszyny Wirtualnej** ustawioną **2** w puli partii zadań Azure z **2** maszyn wirtualnych, uruchamia zadania szybkiego (Sprawdź godziny rozpoczęcia i zakończenia zadania).

Użyj portalu, aby wyświetlić zadania wsadowego i jego zadań, które są skojarzone z **wycinków** i sprawdzić, jakie maszyny Wirtualnej, każdy wycinek uruchomionego na.

![Fabryka danych Azure - zadań zadania wsadowego](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>Debugowanie potoku
Debugowanie obejmuje kilka podstawowych technik:

1. Jeśli wycinek wejściowy nie jest ustawiony na **gotowe**, potwierdź, że struktura folderów wejściowe są poprawne i plik.txt istnieje w folderze wejściowego.

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. W **Execute** metody działania niestandardowego, użyj **IActivityLogger** obiektu do logowania się informacje ułatwiające rozwiązywanie problemów. Zarejestrowane komunikaty wyświetlane w użytkownika\_plików dziennika 0.

   W **OutputDataset** bloku, kliknij wycinek, aby wyświetlić **WYCINKA danych** bloku dla tego wycinka. Zostanie wyświetlony **uruchomień działania** dla tego wycinka. Powinny pojawić się jeden uruchamiania dla wycinka działania. Jeśli klikniesz przycisk **Uruchom** na pasku poleceń, można uruchomić inne działanie Uruchom dla tej samej wycinka.

   Po kliknięciu przycisku uruchamiania działania, zobacz **szczegóły uruchomienia działania** bloku zawierającego listę plików dziennika. Zobacz zarejestrowane komunikaty w **użytkownika\_dziennika 0** pliku. Gdy wystąpi błąd, zostanie wyświetlony trzech uruchomień działania, ponieważ liczby ponownych prób jest ustawiona na 3 w potoku/aktywność JSON. Po kliknięciu przycisku Uruchom działania, zobaczysz plików dziennika, które można przejrzeć, aby rozwiązać problem.

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   Na liście plików dziennika, kliknij **0.log użytkownika**. W prawym okienku są wyniki za pomocą **IActivityLogger.Write** metody.

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   Sprawdź system-0.log za wszelkie komunikaty o błędach systemu i wyjątki.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. Obejmują **PDB** plików w pliku zip, aby szczegóły błędu mają informacje takie jak **stosu wywołań** po wystąpieniu błędu.
4. Wszystkie pliki w pliku zip dla działań niestandardowych musi wynosić **najwyższego poziomu** z bez podfolderów.

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. Upewnij się, że **assemblyName** (MyDotNetActivity.dll), **punktu wejścia** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip) i **packageLinkedService** (powinny wskazywać do magazynu obiektów blob platformy Azure, który zawiera plik zip) są ustawione prawidłowe wartości.
6. Jeśli naprawiono błąd i chcesz przetworzyć wycinek ponownie, kliknij prawym przyciskiem wycinek w bloku **OutputDataset** i kliknij polecenie **Uruchom**.

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > Zostanie wyświetlony **kontenera** w magazynie obiektów Blob Azure o nazwie: `adfjobs`. Ten kontener nie jest automatycznie usuwana, ale można je bezpiecznie usunąć po zakończeniu testowania rozwiązania. Podobnie, w zakresie fabryki danych tworzy partii zadań Azure **zadania** o nazwie: `adf-\<pool ID/name\>:job-0000000001`. Po przetestowaniu rozwiązania Jeśli chcesz, możesz usunąć tego zadania.
   >
   >
7. Działania niestandardowe nie używa **app.config** plików z pakietu. W związku z tym jeśli kod odczytuje wszelkie parametry połączenia z pliku konfiguracji, działa w czasie wykonywania. Najlepszym rozwiązaniem w przypadku przy użyciu partii zadań Azure do przechowywania wszelkich kluczy tajnych w **Azure KeyVault**, użyj nazwy głównej usługi oparte na certyfikatach, aby chronić keyvault i dystrybuowania certyfikatów do puli partii zadań Azure. Niestandardowe działanie .NET będzie miało w takiej sytuacji dostęp do danych poufnych z magazynu KeyVault w czasie uruchomienia. To rozwiązanie jest rodzajowy i można skalować do dowolnego typu klucza tajnego, nie tylko w parametrach połączenia.

    Istnieje obejście łatwiejsze (ale nie najlepiej): można utworzyć **Azure SQL połączona usługa** z ustawień parametrów połączenia, utworzyć zestawu danych, który używa połączonej usługi, a powiązane zestawu danych jako fikcyjny wejściowy zestaw danych do działania niestandardowego .NET. Następnie dostęp do połączonej usługi parametry połączenia w kodzie działań niestandardowych i powinny działać prawidłowo w czasie wykonywania.  

#### <a name="extend-the-sample"></a>Rozszerzanie próbki
Można rozszerzyć tego przykładu, aby dowiedzieć się więcej o funkcjach usługi fabryka danych Azure i partii zadań Azure. Na przykład aby przetwarzać wycinki inny zakres czasu, wykonaj następujące czynności:

1. Dodaj następujące podfoldery `inputfolder`: 2015-11-16-05 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 i miejscu plików wejściowych zawierających w tych folderach. Zmień godzinę zakończenia dla potoku z `2015-11-16T05:00:00Z` do `2015-11-16T10:00:00Z`. W **widoku diagramu**, kliknij dwukrotnie **InputDataset**i Potwierdź, że wejściowy wycinków są gotowe. Kliknij dwukrotnie **OuptutDataset** aby zobaczyć stan wycinki danych wyjściowych. Jeśli są w stanie gotowe, sprawdź folder wyjściowy dla plików wyjściowych.
2. Zwiększanie lub zmniejszanie **współbieżności** ustawienie, aby zrozumieć, jak wpływają na wydajność rozwiązania, szczególnie przetwarzania, który występuje w partii zadań Azure. (Zobacz krok 4: tworzenie i uruchamianie potoku, aby uzyskać więcej informacji **współbieżności** ustawienie.)
3. Tworzenie puli za pomocą/małe **maksymalna zadania dla maszyny Wirtualnej**. Aby używać nowego zestawu utworzonego, zaktualizuj partii zadań Azure połączonej usługi w rozwiązaniu fabryki danych. (Zobacz krok 4: tworzenie i uruchamianie potoku, aby uzyskać więcej informacji **maksymalna zadania dla maszyny Wirtualnej** ustawienie.)
4. Tworzenie puli partii zadań Azure z **skalowania automatycznego** funkcji. Automatyczne skalowanie węzłów obliczeniowych w puli partii zadań Azure jest dynamiczna dostosowania przetwarzania zasilania używanych przez aplikację. 

    Przykładowa formuła tutaj realizuje następujące zachowanie: podczas tworzenia puli, rozpoczyna się od 1 maszyny Wirtualnej. Metryka $PendingTasks definiuje liczbę zadań uruchomiona + aktywny (w kolejce) stanu.  Formuła znajduje średnią liczbę oczekujących zadań w ciągu ostatnich 180 sekund i odpowiednio ustawia TargetDedicated. Gwarantuje, że TargetDedicated nigdy nie wykracza poza 25 maszyn wirtualnych. Tak, jak nowe zadania są przesyłane, automatycznie zwiększa rozmiar puli i jako zakończenie zadania, maszyn wirtualnych stają się wolnego jeden po drugim i skalowanie automatyczne zmniejsza tych maszyn wirtualnych. startingNumberOfVMs i maxNumberofVMs można dostosować do własnych potrzeb.
 
    Formuła skalowania automatycznego:

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   Zobacz [automatycznie skali obliczeniowe węzłów w puli partii zadań Azure](../../batch/batch-automatic-scaling.md) szczegółowe informacje.

   Jeśli w puli jest przy użyciu domyślnego [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), usługa partia zadań może zająć 15 do 30 minut, aby przygotować maszyny Wirtualnej przed uruchomieniem działania niestandardowego.  Jeśli pula używa innego autoScaleEvaluationInterval, usługa partia zadań może zająć autoScaleEvaluationInterval + 10 minut.
5. W rozwiązaniu próbki **Execute** wywołuje metodę **Calculate** metody, która przetwarza wycinek danych wejściowych do produkcji wycinek danych wyjściowych. Napisania własnej metody do przetwarzania danych wejściowych i Zastąp wywołanie metody Calculate w metody Execute Wywołanie do metody.

### <a name="next-steps-consume-the-data"></a>Następne kroki: danych
Po przetwarzania danych, można pobrać go online narzędzia, takie jak **Microsoft Power BI**. Oto łącza, aby ułatwić zrozumienie usługi Power BI i jak z niego korzystać na platformie Azure:

* [Eksploruj zestawu danych w usłudze Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Wprowadzenie do korzystania z programu Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Odświeżanie danych w usłudze Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure i usługi Power BI — omówienie](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Dokumentacja
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Wprowadzenie do usługi fabryka danych Azure](data-factory-introduction.md)
  * [Rozpoczynanie pracy z fabryką danych Azure](data-factory-build-your-first-pipeline.md)
  * [Korzystanie z działań niestandardowych w potoku usługi Azure Data Factory](data-factory-use-custom-activities.md)
* [Partia zadań Azure](https://azure.microsoft.com/documentation/services/batch/)

  * [Podstawowe informacje o partii zadań Azure](../../batch/batch-technical-overview.md)
  * [Przegląd funkcji partii zadań Azure](../../batch/batch-api-basics.md)
  * [Tworzenie i zarządzanie nimi konto partii zadań Azure w portalu Azure](../../batch/batch-account-create-portal.md)
  * [Rozpoczynanie pracy z platformą .NET biblioteki usługi partia zadań Azure](../../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
