<properties
    pageTitle="Zapytania na danych z magazynu obiektów blob zgodnego systemem plików HDFS | Microsoft Azure"
    description="Usługa HDInsight używa magazynu obiektów blob platformy Azure do przechowywania danych big data dla systemu plików HDFS. Dowiedz się, jak wykonywać zapytania na danych z magazynu obiektów blob i przechowywać wyniki analiz."
    keywords="blob storage,hdfs,structured data,unstructured data"
    services="hdinsight,storage"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/18/2016"
    ms.author="jgao"/>


# Użyj magazynu obiektów blob platformy Azure zgodnego z systemem plików HDFS w połączeniu z platformą Hadoop w usłudze HDInsight

Dowiedz się, jak używać niedrogiego magazynu obiektów blob platformy Azure z usługą HDInsight, tworzyć konto magazynu Azure i kontener magazynu obiektów blob, a następnie adresować zawarte w nim dane.

Magazyn obiektów blob systemu Azure to niezawodne rozwiązanie ogólnego przeznaczenia, które bezproblemowo integruje się z usługą HDInsight. Korzystając z interfejsu rozproszonego systemu plików Hadoop (HDFS), pełny zestaw składników usługi HDInsight może operować bezpośrednio na danych strukturalnych lub bez struktury w magazynie obiektów blob.

Przechowywanie danych w magazynie obiektów blob pozwala bezpiecznie usuwać klastry usługi HDInsight używane do obliczeń bez utraty danych użytkownika.

> [AZURE.NOTE]  Składnia *asv://* nie jest obsługiwana w klastrach usługi HDInsight w wersji 3.0. Oznacza to, że przesłane do klastra usługi HDInsight w wersji 3.0 zadania, które jawnie używają składni *asv://*, zakończą się niepowodzeniem. Należy zamiast tego używać składni *wasb://*. Ponadto przesłane do dowolnych klastrów usługi HDInsight w wersji 3.0 zadania utworzone na podstawie istniejącego magazynu metadanych zawierającego jawne odwołania do zasobów ze składnią asv:// zakończą się niepowodzeniem. Te magazyny metadanych muszą zostać ponownie utworzone przy użyciu składni wasb:// do adresowania zasobów.

> HDInsight aktualnie obsługuje tylko blokowe obiekty blob.

> Większość poleceń systemu plików HDFS (na przykład <b>ls</b>, <b>copyFromLocal</b> i <b>mkdir</b>) nadal działa zgodnie z oczekiwaniami. Tylko polecenia specyficzne dla natywnych implementacji systemu plików HDFS (określanych jako systemy plików DFS), takie jak <b>fschk</b> i <b>dfsadmin</b>, będą działać inaczej w magazynie obiektów blob platformy Azure.

Informacje dotyczące tworzenia klastra usługi HDInsight można znaleźć w tematach [Get Started with HDInsight][hdinsight-get-started] (Wprowadzenie do usługi HDInsight) lub [Create HDInsight clusters][hdinsight-creation] (Tworzenie klastrów usługi HDInsight).


## Architektura magazynu usługi HDInsight
Na poniższym diagramie przedstawiono abstrakcyjny widok architektury magazynu usługi HDInsight:

![Klastry Hadoop używają interfejsu API systemu plików HDFS w celu dostępu do danych strukturalnych i bez struktury oraz przechowywania ich w magazynie obiektów blob.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage Architecture")

Usługa HDInsight zapewnia dostęp do rozproszonego systemu plików, który jest lokalnie dołączony do węzłów obliczeniowych. Dostęp do tego systemu plików można uzyskać przy użyciu w pełni kwalifikowanego identyfikatora URI, na przykład:

    hdfs://<namenodehost>/<path>

Ponadto usługa HDInsight zapewnia możliwość dostępu do danych przechowywanych w magazynie obiektów blob systemu Azure. Składnia jest następująca:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>


Platforma Hadoop obsługuje pojęcie domyślnego systemu plików. Domyślny system plików wyznacza domyślny schemat i element authority. Może również służyć do rozpoznawania ścieżek względnych. Podczas procesu tworzenia usługi HDInsight konto Azure Storage i określony na podstawie tego konta kontener magazynu obiektów blob platformy Azure wyznaczają domyślny system plików.

W ramach procesu tworzenia oprócz tego konta magazynu można dodać dodatkowe konta magazynu z tej samej subskrypcji platformy Azure lub różnych subskrypcji platformy Azure. Aby uzyskać instrukcje dotyczące dodawania dodatkowych kont magazynu, zobacz [Create HDInsight clusters][hdinsight-creation] (Tworzenie klastrów HDInsight).

- **Kontenery w ramach kont magazynu, które są podłączone do klastra:** ponieważ nazwa konta i klucz są kojarzone z klastrem podczas tworzenia, masz pełny dostęp do obiektów blob w tych kontenerach.

- **Publiczne kontenery lub publiczne obiekty blob na kontach magazynu, które NIE są podłączone do klastra:** masz uprawnienia tylko do odczytu obiektów blob w kontenerach.

    > [AZURE.NOTE]
        > Kontenery publiczne pozwalają na pobranie listy wszystkich obiektów blob, które są dostępne w danym kontenerze, oraz pobranie metadanych kontenera. Publiczne obiekty blob umożliwiają dostęp do obiektów blob jedynie osobom znającym dokładny adres URL. Aby uzyskać więcej informacji, zobacz artykuł <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Restrict access to containers and blobs</a> (Ograniczanie dostępu do kontenerów i obiektów blob).

- **Prywatne kontenery na kontach magazynu, które NIE są podłączone do klastra:** nie masz dostępu do obiektów blob w kontenerach, chyba że zdefiniujesz konto magazynu podczas przesyłania zadań WebHCat. Wyjaśnienie jest zawarte w dalszej części tego artykułu.


Konta magazynu definiowane w procesie tworzenia oraz ich klucze są przechowywane w pliku %HADOOP_HOME%/conf/core-site.xml w węzłach klastra. Domyślne działanie usługi HDInsight polega na korzystaniu z kont magazynu zdefiniowanych w pliku core-site.xml. Nie zaleca się edytowania pliku core-site.xml, ponieważ główny węzeł klastra (master) może być odtwarzany z obrazu lub poddawany migracji w dowolnym momencie, a wówczas wszelkie zmiany w tych plikach zostaną utracone.

Wiele zadań WebHCat, w tym Hive, MapReduce, przesyłanie strumieniowe Hadoop, a także Pig, może przenosić ze sobą opis kont magazynu i metadane. (W przypadku technologii Pig obecnie działa to z kontami magazynu, ale nie dla metadanych). W sekcji [Dostęp do obiektów blob przy użyciu programu Azure PowerShell](#powershell) tego artykułu znajduje się przykład tej funkcjonalności. Aby uzyskać więcej informacji, zobacz [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx) (Używanie klastra usługi HDInsight z alternatywnymi kontami magazynu i magazynami metadanych).

Magazyn obiektów blob może być używany z danymi strukturalnymi i bez struktury. Kontenery magazynu obiektów blob przechowują dane jako pary klucz/wartość, bez hierarchii katalogów. Jednak wewnątrz nazwy klucza można użyć znaku ukośnika (/), co sprawi, że będzie wyglądała, jakby plik był przechowywany w ramach struktury katalogów. Na przykład klucz obiektu blob może mieć postać *input/log1.txt*. Katalog *input* w rzeczywistości nie istnieje, ale z powodu obecności znaku ukośnika w nazwie klucza, klucz ma wygląd ścieżki do pliku.

###<a id="benefits"></a>Korzyści z magazynu obiektów blob
Wynikowy koszt wydajności związany z brakiem łączenia klastrów obliczeniowych i zasobów magazynu jest zmniejszany przez sposób tworzenia klastrów obliczeniowych w pobliżu zasobów konta magazynu w obrębie regionu Azure, gdzie szybkie sieci zapewniają bardzo wydajny dostęp węzłów obliczeniowych do danych wewnątrz magazynu obiektów blob platformy Azure.

Przechowywanie danych w magazynie obiektów blob platformy Azure zamiast w systemie plików HDFS ma wiele zalet:

* **Udostępnianie i ponowne użycie danych:** dane w systemie plików HDFS znajdują się wewnątrz klastra obliczeniowego. Tylko te aplikacje, które mają dostęp do klastra obliczeniowego mogą używać danych za pomocą interfejsów API systemu plików HDFS. Dane w magazynie obiektów blob platformy Azure są dostępne za pośrednictwem interfejsów API systemu plików HDFS lub za pomocą [interfejsów API REST usługi Blob Storage][blob-storage-restAPI]. W związku z tym większy zestaw narzędzi i aplikacji (w tym inne klastry HDInsight) może służyć do tworzenia i wykorzystywania danych.
* **Archiwizacja danych:** przechowywanie danych w magazynie obiektów blob platformy Azure pozwala bezpiecznie usuwać klastry usługi HDInsight używane do obliczeń bez utraty danych użytkownika.
* **Koszt magazynowania danych:** długoterminowe przechowywanie danych w systemie plików DFS jest droższe niż przechowywanie danych w magazynie obiektów blob platformy Azure, ponieważ koszt klastra obliczeniowego jest wyższy niż koszt kontenera magazynu obiektów blob platformy Azure. Ponadto ponieważ nie trzeba ponownie ładować danych przy każdej generacji klastra obliczeniowego, oszczędzamy również koszty ładowania danych.
* **Elastyczne skalowanie w poziomie:** chociaż system plików HDFS zapewnia skalowanie w poziomie, skala jest wyznaczana przez liczbę węzłów tworzonych dla klastra. Zmiana skali może stać się procesem bardziej skomplikowanym w porównaniu z elastycznymi możliwościami skalowania, które można uzyskać automatycznie w magazynie obiektów blob platformy Azure.
* **Replikacja geograficzna:** kontenery magazynu obiektów blob platformy Azure można replikować geograficznie. Chociaż zapewnia to odzyskiwanie geograficzne i nadmiarowość danych, praca w trybie failover w przypadku lokalizacji zreplikowanych geograficznie poważnie wpływa na wydajność i może pociągać za sobą dodatkowe koszty. Dlatego zalecamy, aby wybierać replikację geograficzną w rozsądny sposób i tylko wtedy, gdy wartość danych uzasadnia ponoszenie dodatkowych kosztów.

Niektóre zadania i pakiety MapReduce mogą tworzyć wyniki pośrednie, których nie potrzeba przechowywać w magazynie obiektów blob platformy Azure. W takim przypadku można zdecydować się na przechowywanie danych w lokalnym systemie plików HDFS. W rzeczywistości HDInsight używa systemu plików DFS dla wielu wyników pośrednich w zadaniach Hive i innych procesach.



## Tworzenie kontenerów obiektów blob

Aby użyć obiektów blob, należy najpierw utworzyć [konto Azure Storage][azure-storage-create]. W ramach tego działania można określić region platformy Azure, w którym będą przechowywanie obiekty utworzone za pomocą tego konta. Klaster i konto magazynu muszą być hostowane w tym samym regionie. Baza danych SQL Server na potrzeby magazynu metadanych Hive i baza danych SQL Server na potrzeby magazynu metadanych Oozie również muszą znajdować się w tym samym regionie.

Wszędzie tam, gdzie go umieszczono, każdy utworzony obiekt blob należy do kontenera na koncie usługi Azure Storage. Ten kontener może być istniejącym obiektem blob utworzonym poza usługą HDInsight lub może być kontenerem, który jest tworzony dla klastra usługi HDInsight.


Domyślny kontener obiektów blob przechowuje informacje dotyczące klastra, takie jak dzienniki i historię zadań. Nie należy współużytkować domyślnego kontenera obiektów blob dla wielu klastrów usługi HDInsight. Mogłoby to spowodować uszkodzenie historii zadań i błędne działanie klastra. Zalecane jest stosowanie różnych kontenerów do każdego klastra i umieszczanie udostępnionych danych w połączonym koncie magazynu określonym we wdrożeniu wszystkich odpowiednich klastrów zamiast domyślnego konta magazynu. Aby uzyskać więcej informacji na temat konfigurowania kont magazynu połączone, zobacz artykuł [Create HDInsight clusters][hdinsight-creation] (Tworzenie klastrów usługi HDInsight). Jednak po usunięciu oryginalnego klastra usługi HDInsight można ponownie użyć domyślnego kontenera magazynu. W przypadku klastrów HBase faktycznie można zachować schemat tabeli HBase i dane przez utworzenie nowego klastra HBase przy użyciu domyślnego kontenera magazynu obiektów blob, używanego przez klaster HBase, który został usunięty.


### Korzystanie z portalu Azure

Podczas tworzenia klastra usługi HDInsight za pomocą portalu masz do wyboru opcje użycia istniejącego konta magazynu lub utworzenie nowego konta magazynu:

![źródło danych tworzenia hdinsight hadoop](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

###Korzystanie z interfejsu wiersza polecenia platformy Azure

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

Jeśli masz [zainstalowany i skonfigurowany interfejs wiersza polecenia platformy Azure](../xplat-cli-install.md), możesz zastosować następujące polecenie do konta magazynu i kontenera.

    azure storage account create <storageaccountname> --type LRS

> [AZURE.NOTE] Parametr `--type` wskazuje sposób replikacji konta magazynu. Aby uzyskać więcej informacji, zobacz artykuł [Azure Storage Replication](../storage/storage-redundancy.md) (Replikacja usługi Azure Storage). Nie używaj technologii ZRS, ponieważ nie obsługuje ona stronicowych obiektów blob, plików, tabel ani kolejek.

Pojawi się monit o region geograficzny, w którym zostanie umieszczone konto magazynu. Należy utworzyć konto magazynu w tym samym regionie, w którym planujesz utworzenie klastra usługi HDInsight.

Po utworzeniu konta magazynu użyj następującego polecenia w celu uzyskania kluczy do konta magazynu:

    azure storage account keys list <storageaccountname>

Aby utworzyć kontener, użyj następującego polecenia:

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

### Korzystanie z programu Azure PowerShell

Po [zainstalowaniu i skonfigurowaniu programu Azure PowerShell][powershell-install] można użyć następujących poleceń w wierszu polecenia programu Azure PowerShell, aby utworzyć konto magazynu i kontener:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"
    
    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"
    
    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID
    
    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location
    
    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 
    
    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

## Adresowanie plików w magazynie obiektów blob

Schemat identyfikatora URI do uzyskiwania dostępu do plików w magazynie obiektów blob z usługi HDInsight to:

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>


> [AZURE.NOTE] Składnia adresowania plików w emulatorze magazynu (działającym w emulatorze usługi HDInsight) to <i>wasb://&lt;NazwaKontenera&gt;@storageemulator</i>.



Schemat identyfikatora URI zapewnia nieszyfrowany dostęp (z prefiksem *wasb:*) oraz szyfrowany dostęp SSL (z prefiksem *wasbs*). Zalecamy używanie prefiksu *wasbs* wszędzie tam, gdzie to możliwe, nawet w przypadku uzyskiwania dostępu do danych, które znajdują się wewnątrz tego samego regionu w systemie Azure.

&lt;BlobStorageContainerName&gt; identyfikuje nazwę kontenera w Magazynie obiektów Blob platformy Azure.
&lt;StorageAccountName&gt; identyfikuje nazwę konta Azure Storage. Wymagana jest w pełni kwalifikowana nazwa domeny (FQDN).

Jeśli żadna z nazw &lt;BlobStorageContainerName&gt; ani &lt;StorageAccountName&gt; nie zostanie określona, używany jest domyślny system plików. W przypadku plików w domyślnym systemie plików można używać ścieżki względnej lub bezwzględnej. Na przykład do pliku *hadoop-mapreduce-examples.jar* dostarczanego z klastrami usługi HDInsight można odwoływać się w jeden z następujących sposobów:

    wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasb:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [AZURE.NOTE] Nazwa pliku to <i>hadoop-examples.jar</i> w klastrach usługi HDInsight w wersji 2.1 i 1.6.


&lt;path&gt; jest nazwą ścieżki do pliku lub katalogu w systemie plików HDFS. Ponieważ kontenery w magazynie obiektów blob platformy Azure przechowują po prostu pary klucz-wartość, nie istnieje prawdziwy hierarchiczny system plików. Znak ukośnika (/) wewnątrz klucza obiektu blob jest interpretowany jako separator katalogu. Na przykład nazwą obiektu blob dla pliku *hadoop-mapreduce-examples.jar* jest:

    example/jars/hadoop-mapreduce-examples.jar

> [AZURE.NOTE] Podczas pracy z obiektami blob poza usługą HDInsight, większość narzędzi nie rozpoznaje formatu WASB i zamiast tego oczekuje podstawowego formatu ścieżki, takiego jak `example/jars/hadoop-mapreduce-examples.jar`.

## Dostęp do obiektów blob za pomocą interfejsu wiersza polecenia platformy Azure

Użyj następującego polecenia, aby wyświetlić listę poleceń związanych z obiektami blob:

    azure storage blob

**Przykład użycia interfejsu wiersza polecenia platformy Azure, aby przekazać plik**

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Przykład użycia interfejsu wiersza polecenia platformy Azure, aby pobrać plik**

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

**Przykład użycia interfejsu wiersza polecenia platformy Azure, aby usunąć plik**

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Przykład użycia interfejsu wiersza polecenia platformy Azure, aby wyświetlić listę plików**

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## Dostęp do obiektów blob przy użyciu programu Azure PowerShell

> [AZURE.NOTE] Polecenia w tej sekcji stanowią podstawowy przykład użycia programu PowerShell w celu dostępu do danych przechowywanych w obiektach blob. Obszerniejszy przykład dostosowany do pracy z usługą HDInsight znajdziesz w artykule [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools) (Narzędzia usługi HDInsight).

Użyj następującego polecenia, aby wyświetlić listę poleceń cmdlet związanych z obiektami blob:

    Get-Command *blob*

![Lista poleceń cmdlet programu PowerShell związanych z obiektami blob.][img-hdi-powershell-blobcommands]

###Przekazywanie plików

Zobacz [Upload data to HDInsight][hdinsight-upload-data] (Przekazywanie danych do usługi HDInsight).

###Pobieranie plików

Następujący skrypt służy do pobierania blokowego obiektu blob do bieżącego folderu. Przed uruchomieniem skryptu zmień katalog na folder, w którym masz uprawnienia do zapisu.

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.
    
    # Use Add-AzureAccount if you haven't connected to your Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    
    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "List the downloaded file ..." -ForegroundColor Green
    cat "./$blob"

Po podaniu nazwy grupy zasobów i nazwy klastra możesz użyć następującego kodu:

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.
    
    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
    
    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force

###Usuwanie plików


    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

###Lista plików

    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

###Uruchamianie zapytań Hive przy użyciu niezdefiniowanego konta magazynu

Ten przykład przedstawia sposób wyświetlania zawartości folderu z konta magazynu, które nie zostało zdefiniowane podczas procesu tworzenia.
$clusterName = "<HDInsightClusterName>"

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

## Następne kroki

W tym artykule przedstawiono sposób używania magazynu obiektów blob platformy Azure zgodnego z systemem plików HDFS w ramach usługi HDInsight oraz wyjaśniono, że magazyn obiektów blob platformy Azure jest podstawowym składnikiem usługi HDInsight. Podane tu informacje umożliwiają tworzenie skalowalnych, długoterminowych rozwiązań do pozyskiwania danych archiwalnych z magazynu obiektów blob platformy Azure i używanie usługi HDInsight w celu efektywnego wykorzystywania informacji przechowywanych w postaci danych strukturalnych i danych bez struktury.

Aby uzyskać więcej informacji, zobacz:

* [Get Started with Azure HDInsight][hdinsight-get-started] (Wprowadzenie do usługi Azure HDInsight)
* [Upload data to HDInsight][hdinsight-upload-data] (Przekazywanie danych do usługi HDInsight)
* [Korzystanie z programu Hive z usługą HDInsight][hdinsight-use-hive]
* [Korzystanie z języka Pig z usługą HDInsight][hdinsight-use-pig]
* [Use Azure Storage Shared Access Signatures to restrict access to data with HDInsight][hdinsight-use-sas] (Używanie sygnatur dostępu współdzielonego do usługi Azure Storage, aby ograniczyć dostęp do danych za pomocą usługi HDInsight)

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: ../powershell-install-configure.md
[hdinsight-creation]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../storage/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  



<!--HONumber=Jun16_HO2-->


