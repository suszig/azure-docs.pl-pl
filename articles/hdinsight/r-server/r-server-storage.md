---
title: "Azure rozwiązań magazynów na potrzeby R Server w usłudze HDInsight - Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat magazynu różne opcje dostępne dla użytkowników z serwerem R w usłudze HDInsight"
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 1cf30096-d3ca-45ea-b526-aa3954402f66
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 863277294fc0462e9221edffab1dd4e2001d7493
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a>Azure rozwiązań magazynów na potrzeby R Server w usłudze HDInsight

Microsoft R Server w usłudze HDInsight zawiera różnych rozwiązań magazynu do utrwalenia danych, kodu lub obiektów zawierających wyniki analizy. Należą do nich następujące opcje:

- [Obiektów Blob platformy Azure](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage](https://azure.microsoft.com/services/data-lake-store/)
- [Magazyn plików Azure](https://azure.microsoft.com/services/storage/files/)

Istnieje również możliwość uzyskiwania dostępu do wielu kont magazynu Azure lub kontenery z klastra HDI. Magazyn plików Azure to wygodny danych opcji magazynu do użycia na węzeł krawędzi, który umożliwia zainstalowanie udziału plików magazynu Azure, na przykład system plików systemu Linux. Jednak udziały plików platformy Azure można zainstalować i używane przez system z obsługiwane systemy operacyjne, takie jak Windows lub Linux. 

Podczas tworzenia klastra usługi Hadoop w usłudze HDInsight, należy określić albo **magazynu Azure** konta lub **Data Lake store**. Z tego konta kontener magazynu określonych przechowuje systemu plików dla klastra, który można utworzyć (na przykład rozproszonego systemu plików Hadoop). Aby uzyskać więcej informacji i wskazówek zobacz:

- [Użyj magazynu platformy Azure z usługą HDInsight](../hdinsight-hadoop-use-blob-storage.md)
- [Użyj Data Lake Store z usługą Azure HDInsight clusters](../hdinsight-hadoop-use-data-lake-store.md). 

Aby uzyskać więcej informacji na temat rozwiązań magazynu Azure, zobacz [wprowadzenie do usługi Microsoft Azure Storage](../../storage/common/storage-introduction.md). 

Aby uzyskać wskazówki dotyczące wybierania najbardziej odpowiedniej opcji magazynu do użycia na potrzeby danego scenariusza, zobacz [decydowania, kiedy używać obiektów blob Azure, Azure pliki i dyski danych Azure](../../storage/common/storage-decide-blobs-files-disks.md) 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a>Używanie kont magazynu obiektów Blob platformy Azure z serwerem R

Jeśli określono więcej niż jedno konto magazynu podczas tworzenia klastra serwerów R, poniższe instrukcje wyjaśniają sposób użycia dodatkowej konta dla dostępu do danych i operacji na serwerze R. Załóżmy następujące konta magazynu i kontener: **storage1** i domyślny kontener o nazwie **container1**, i **storage2**.

> [!WARNING]
> Ze względów wydajnościowych klastra usługi HDInsight jest tworzony w tym samym centrum danych, co konto magazynu podstawowego, który określisz. Używanie konta magazynu w innej lokalizacji niż klastra usługi HDInsight nie jest obsługiwane.

1. Przy użyciu klienta SSH, połącz się z węzłem krawędzi klastra jako remoteuser.  

  + W portalu Azure > stronę usługi klastra HDI > Przegląd kliknij **Secure Shell (SSH)**.
  + Nazwa hosta, wybierz węzeł krawędzi (zawiera *ed-ssh.azurehdinsight.net* w nazwie).
  + Skopiuj nazwę hosta.
  + Otwórz klienta SSH PutTY lub SmartTY, a następnie wprowadź nazwę hosta.
  + Wprowadź nazwę użytkownika, a następnie hasło klastra remoteuser.
  
2. Skopiuj plik mycsv.csv do katalogu/Share. 

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

3. Przełącz się do R Studio lub innej konsoli R i napisać kod R, aby ustawić nazwę węzła **domyślne** i lokalizację pliku, którego chcesz uzyskać dostęp.  

        myNameNode <- "default"
        myPort <- 0

        #Location of the data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(nameNode=myNameNode, consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define the Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify the input file to analyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mycsv.csv")

Wszystkie odwołania katalogów i plików wskaż konta magazynu wasb://container1@storage1.blob.core.windows.net. Jest to **domyślne konto magazynu** który jest skojarzony z klastrem usługi HDInsight.

Załóżmy, że chcesz przetworzyć pliku o nazwie mySpecial.csv, który znajduje się w /private katalog **container2** w **storage2**.

W kodzie języka R punkt nazwa węzła odwołania do **storage2** konta magazynu.


    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of the data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mySpecial.csv")

Wszystkie odwołania do plików i katalogów wskazywać konto magazynu wasb://container2@storage2.blob.core.windows.net. Jest to **nazwa węzła** , który został określony.

Należy skonfigurować User/RevoShare/<SSH username> katalogu na **storage2** w następujący sposób:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a>Używanie usługi Azure Data Lake store z serwerem R

W celu używania usługi Data Lake magazynów z Twoim kontem usługi HDInsight, należy zapewnić dostęp do sieci klastra do każdego Azure Data Lake store, którego chcesz używać. Aby uzyskać instrukcje dotyczące sposobu tworzenia klastra usługi HDInsight przy użyciu konta usługi Azure Data Lake Store jako domyślnego magazynu lub dodatkowego magazynu za pomocą portalu Azure, zobacz [tworzenia klastra usługi HDInsight z Data Lake Store za pomocą portalu Azure](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Następnie należy użyć magazynu w skrypcie R znacznie jak konto magazynu Azure dodatkowej zgodnie z opisem w poprzedniej procedurze.

### <a name="add-cluster-access-to-your-azure-data-lake-stores"></a>Dodaj klaster dostęp do usługi Azure Data Lake sklepach
Data Lake store jest dostęp przy użyciu nazwy głównej usługi Azure Active Directory (Azure AD), który został skojarzony z klastrem usługi HDInsight.

Aby dodać nazwy głównej usługi Azure AD:

1. Podczas tworzenia klastra usługi HDInsight, wybierz **tożsamość usługi AAD klastra** z **źródła danych** kartę.

2. W **tożsamość usługi AAD klastra** okna dialogowego, w obszarze **wybierz jednostkę usługi AD**, wybierz pozycję **Utwórz nowy**.

Po nadaj nazwę główną usługi i Utwórz hasło dla niego kliknij **zarządzać dostępem ADLS** do skojarzenia z sklepach usługi Data Lake nazwy głównej usługi.

Istnieje również możliwość można dodać jeden lub więcej sklepów usługi Data Lake po utworzeniu klastra dostępu do klastra. Otwórz pozycję portalu Azure Data Lake store i przejdź do **Eksploratora danych > dostępu > Dodaj**. 

### <a name="how-to-access-the-data-lake-store-from-r-server"></a>Jak uzyskać dostęp z serwera R Data Lake store

Po został przez Ciebie udzielony dostęp do magazynu usługi Data Lake, używając magazynu w R Server w usłudze HDInsight sposób, w jaki konto magazynu Azure dodatkowej. Jedyną różnicą jest to, że prefiks **wasb: / /** zmienia się na **adl: / /** w następujący sposób:


    # Point to the ADL store (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of the data (assumes a /share directory on the ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify the input file in HDFS to analyze
    inputFile <-file.path(bigDataDirRoot,"AirlineDemoSmall.csv")

    # Create factors for days of the week
    colInfo <- list(DayOfWeek = list(type = "factor",
               levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
                          "Friday", "Saturday", "Sunday")))

    # Define the data source
    airDS <- RxTextData(file = inputFile, missingValueString = "M",
                    colInfo  = colInfo, fileSystem = hdfsFS)

    # Run a linear regression
    model <- rxLinMod(ArrDelay~CRSDepTime+DayOfWeek, data = airDS)


Następujące polecenia są używane do konfigurowania konta usługi Data Lake magazynu z katalogiem RevoShare i dodać przykładowy plik CSV z poprzedniego przykładu:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a>Użyj usługi Magazyn plików Azure z serwerem R

Istnieje również wygodne danych opcji magazynu do użycia na węzeł krawędzi, o nazwie — Azure plików ((https://azure.microsoft.com/services/storage/files/). Umożliwia instalowanie udziału plików magazynu Azure w systemie Linux. Ta opcja może być przydatna do przechowywania plików danych, skrypty języka R i obiektów wynikowych, które mogą być wymagane później, szczególnie w przypadku warto system plików natywnych węzła krawędzi zamiast systemu plików HDFS. 

Główną zaletą usługi pliki Azure jest udziały plików może być zainstalowane i używane przez system z obsługiwane systemy operacyjne, takie jak Windows lub Linux. Na przykład można używany przez inny klaster usługi HDInsight, który użytkownik lub inna w zespole ma, maszyny Wirtualnej platformy Azure lub nawet przez system lokalny. Aby uzyskać więcej informacji, zobacz:

- [Jak używać usługi Azure File Storage z systemem Linux](../../storage/files/storage-how-to-use-files-linux.md)
- [Jak używać magazynu plików Azure w systemie Windows](../../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Następne kroki

Po zapoznaniu się z opcji magazynu Azure, użyj następujących łączy odkrywanie sposobów uzyskiwania zadania nauki danych odbywa się za pomocą R Server w usłudze HDInsight.

* [Omówienie R Server w usłudze HDInsight](r-server-overview.md)
* [Rozpoczynanie pracy z serwerem R na platformie Hadoop](r-server-get-started.md)
* [Compute context options for R Server on HDInsight](r-server-compute-contexts.md) (Opcje kontekstu obliczeniowego dla oprogramowania R Server w usłudze HDInsight)

