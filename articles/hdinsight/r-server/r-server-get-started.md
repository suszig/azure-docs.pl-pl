---
title: "Wprowadzenie do oprogramowania R Server w usłudze HDInsight — platforma Azure | Microsoft Docs"
description: "Dowiedz się, jak utworzyć aparat Apache Spark w klastrze usługi HDInsight zawierającym oprogramowanie R Server, a następnie jak przesłać skrypt języka R do klastra."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/14/2017
ms.author: bradsev
ms.openlocfilehash: e688068efb41cdccbeb23de3c8ad7a09021e5b3f
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="get-started-with-r-server-on-hdinsight"></a>Wprowadzenie do oprogramowania R Server w usłudze HDInsight

Usługa Azure HDInsight obejmuje opcję oprogramowania R Server, którą można zintegrować z klastrem usługi HDInsight. Opcja ta pozwala skryptom języka R używać aparatu Spark i funkcji MapReduce do wykonywania obliczeń rozproszonych. W tym artykule omówiono sposób tworzenia wystąpienia oprogramowania R Server w klastrze usługi HDInsight. Następnie omówiono uruchamianie skryptu R, który demonstruje sposób użycia aparatu Spark na potrzeby wykonywania rozproszonych obliczeń przez kod R.


## <a name="prerequisites"></a>Wymagania wstępne

* **Subskrypcja platformy Azure**: przed rozpoczęciem tego samouczka musisz mieć subskrypcję platformy Azure. Aby uzyskać więcej informacji, zobacz [Get Microsoft Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (Uzyskaj bezpłatną wersję próbną platformy Azure).
* **Klient protokołu Secure Shell (SSH)**: klient SSH jest używany do zdalnego łączenia z klastrem usługi HDInsight i uruchamiania poleceń bezpośrednio w klastrze. Aby uzyskać więcej informacji, zobacz [Używanie protokołu SSH w usłudze HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
* **Klucze SSH (opcjonalnie)**: konto SSH użyte do nawiązania połączenia z klastrem można zabezpieczyć przy użyciu hasła lub klucza publicznego. Użycie hasła jest łatwiejsze i umożliwia rozpoczęcie pracy bez konieczności tworzenia pary kluczy publiczny-prywatny. Jednak użycie klucza jest bezpieczniejsze.

  > [!NOTE]
  > W krokach przedstawionych w tym artykule przyjęto założenie, że jest używane hasło.


## <a name="automate-cluster-creation"></a>Automatyzowanie tworzenia klastra

Aby zautomatyzować tworzenie wystąpień oprogramowania HDInsight R Server, możesz użyć szablonów usługi Azure Resource Manager, zestawu SDK oraz programu PowerShell.

* Aby utworzyć wystąpienie oprogramowania R Server za pomocą szablonu usługi Azure Resource Manager, zobacz [Deploy an R server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/) (Wdrażanie klastra usługi HDInsight oprogramowania R Server).
* Aby utworzyć wystąpienie oprogramowania R Server za pomocą zestawu .NET SDK, zobacz [Create Linux-based clusters in HDInsight using the .NET SDK](../hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) (Tworzenie klastrów opartych na systemie Linux w usłudze HDInsight przy użyciu zestawu .NET SDK).
* Aby wdrożyć oprogramowanie R Server za pomocą programu PowerShell, zobacz [Create Linux-based clusters in HDInsight using Azure PowerShell](../hdinsight-hadoop-create-linux-clusters-azure-powershell.md) (Tworzenie klastrów opartych na systemie Linux w usłudze HDInsight przy użyciu programu Azure PowerShell).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-a-cluster-by-using-the-azure-portal"></a>Tworzenie klastra przy użyciu witryny Azure Portal

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Wybierz pozycję **Nowy** > **Zbieranie danych i analiza** > **HDInsight**.

    ![Obraz tworzenia nowego klastra](./media/r-server-get-started/newcluster.png)

3. W środowisku **Szybkie tworzenie** podaj nazwę klastra w polu **Nazwa klastra**. Jeśli masz wiele subskrypcji platformy Azure, za pomocą pola **Subskrypcja** wybierz subskrypcję do użycia.

    ![Wybór nazwy klastra i subskrypcji](./media/r-server-get-started/clustername.png)

4. Wybierz pozycję **Typ klastra**, aby otworzyć okienko **Konfiguracja klastra**. W okienku **Konfiguracja klastra** wybierz następujące opcje:

    * **Typ klastra**: wybierz pozycję **R Server**.
    * **Wersja**: wybierz wersję oprogramowania R Server do zainstalowania w klastrze. Aktualnie dostępna jest wersja **R Server 9.1 (HDI 3.6)**. Informacje o wersji dotyczące dostępnych wersji oprogramowania R Server można znaleźć w witrynie [MSDN](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).
    * **Program R Studio Community Edition for R Server**: to środowisko IDE oparte na przeglądarce, które jest instalowane domyślnie w węźle brzegowym. Jeśli nie chcesz go instalować, wyczyść pole wyboru. Jeśli wybierzesz opcję instalacji, adres URL umożliwiający logowanie do programu RStudio Server będzie dostępny w oknie aplikacji portalu dla utworzonego klastra.
    * Pozostaw wartości domyślne innych opcji i za pomocą przycisku **Wybierz** zapisz typ klastra.

        ![Zrzut ekranu okienka Typ klastra](./media/r-server-get-started/clustertypeconfig.png)

5. W okienku **Podstawy** w polach **Nazwa użytkownika logowania klastra** i **Hasło logowania klastra** wprowadź odpowiednio nazwę użytkownika i hasło dla klastra.

6. W obszarze **Nazwa użytkownika protokołu SSH (Secure Shell)** określ nazwę użytkownika SSH. Protokół SSH jest używany do zdalnego łączenia z klastrem przy użyciu klienta protokołu SSH. Użytkownika SSH można określić w tym obszarze lub po utworzeniu klastra (na karcie **Konfiguracja** klastra).
   
   > [!NOTE] 
   > Oprogramowanie R Server jest skonfigurowane pod kątem użycia nazwy użytkownika SSH „remoteuser”. Jeśli używasz innej nazwy użytkownika, musisz wykonać dodatkowy krok po utworzeniu klastra.

7. Pozostaw zaznaczone pole **Użyj tego samego hasła podczas logowania do klastra**, aby użyć typu uwierzytelniania **HASŁO**, chyba że wolisz użyć klucza publicznego. Jeśli planujesz uzyskiwanie dostępu do oprogramowania R Server w klastrze za pomocą zdalnego klienta, na przykład programu R Tools for Visual Studio, RStudio lub innego komputerowego środowiska IDE, będzie potrzebna para kluczy publiczny-prywatny. W przypadku instalowania programu RStudio Server Community Edition, musisz wybrać hasło SSH.     

   Aby utworzyć parę kluczy publiczny-prywatny, wyczyść opcję **Użyj tego samego hasła podczas logowania do klastra**. Następnie wybierz pozycję **KLUCZ PUBLICZNY** i kontynuuj w podany poniżej sposób. W poniższych instrukcjach przyjęto, że jest zainstalowane środowisko Cygwin z programem ssh-keygen lub równoważnym.

   a. Wygeneruj parę kluczy publiczny-prywatny w wierszu polecenia na komputerze przenośnym:

        ssh-keygen -t rsa -b 2048

   b. Podaj nazwę pliku klucza po wyświetleniu monitu, a następnie określ hasło, aby podwyższyć poziom zabezpieczeń. Ekran powinien wyglądać podobnie do następującego:

      ![Wiersz polecenia SSH w systemie Windows](./media/r-server-get-started/sshcmdline.png)

      Polecenie to pozwala utworzyć plik klucza prywatnego i plik klucza publicznego o nazwie <nazwa-pliku-klucza-prywatnego>.pub. W tym przykładzie są to pliki furiosa i furiosa.pub:

      ![Przykład wyniku polecenia dir](./media/r-server-get-started/dir.png)

   d. Określ plik klucza publicznego (&#42;.pub) podczas przypisywania poświadczeń klastra usługi HDI. Potwierdź grupę zasobów i region, po czym wybierz pozycję **Dalej**.

      ![Okienko poświadczeń](./media/r-server-get-started/publickeyfile.png)  

   d. Zmień uprawnienia do pliku klucza prywatnego na komputerze przenośnym:

        chmod 600 <private-key-filename>

   e. Użyj pliku klucza prywatnego do zdalnego logowania za pomocą protokołu SSH:

        ssh –i <private-key-filename> remoteuser@<hostname public ip>

      Ewentualnie użyj pliku klucza prywatnego jako części definicji kontekstu obliczeniowego aparatu Spark usługi Hadoop dla oprogramowania R Server na kliencie. Aby uzyskać więcej informacji, zobacz [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started) (Tworzenie kontekstu obliczeniowego dla aparatu Spark).

8. Funkcja szybkiego tworzenia przeniesie Cię do okienka **Magazyn**. Możesz w nim wybrać ustawienia konta magazynu do użycia dla lokalizacji głównej systemu plików HDFS używanego przez klaster. Wybierz nowe lub istniejące konto usługi Azure Storage lub istniejące konto usługi Azure Data Lake Store.

    - Jeśli wybierzesz konto usługi Azure Storage, możesz określić istniejące konto magazynu, wybierając pozycję **Wybierz konto magazynu**, a następnie wybierając odpowiednie konto. Aby utworzyć nowe konto, użyj linku **Utwórz nowe** w sekcji **Wybierz konto magazynu**.

      > [!NOTE]
      > Jeśli wybierzesz pozycję **Nowe**, musisz podać nazwę nowego konta magazynu. Jeśli nazwa zostanie zaakceptowana, pojawi się zielony znacznik.

      Domyślną wartością pola **Kontener domyślny** jest nazwa klastra. Nie zmieniaj tej wartości.

      Jeśli zostało wybrane nowe konto magazynu, za pomocą pozycji **Lokalizacja** w monicie wybierz region.  

         ![Okienko Źródło danych](./media/r-server-get-started/datastore.png)  

      > [!IMPORTANT]
      > Wybranie lokalizacji domyślnego źródła danych spowoduje także ustawienie lokalizacji klastra usługi HDInsight. Klaster i domyślne źródło danych muszą znajdować się w tym samym regionie.

    - Jeśli chcesz użyć istniejącego konta usługi Data Lake Store, wybierz je. Następnie dodaj tożsamość usługi *ADD* klastra do klastra, aby umożliwić dostęp do magazynu. Aby uzyskać więcej informacji na temat tego procesu, zobacz [Create HDInsight clusters with Data Lake Store by using the Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) (Tworzenie klastra HDInsight z usługą Data Lake Store za pomocą witryny Azure Portal).

    Użyj przycisku **Wybierz**, aby zapisać konfigurację źródła danych.


9. Zostanie wyświetlone okienko **Podsumowanie**, w którym można zweryfikować wszystkie ustawienia. W tym miejscu możesz zmienić rozmiar klastra w celu zmodyfikowania liczby serwerów w klastrze. Możesz też określić akcje skryptu do uruchomienia. Pozostaw domyślną liczbę węzłów procesu roboczego — **4**, chyba że wiesz, że potrzebujesz większego klastra. W okienku zostanie też wyświetlony szacowany koszt klastra.

    ![Podsumowanie klastra](./media/r-server-get-started/clustersummary.png)

   > [!NOTE]
   > W razie potrzeby możesz później zmienić rozmiar klastra w witrynie Portal (**Klaster** > **Ustawienia** > **Skaluj klaster**), aby zwiększyć lub zmniejszyć liczbę węzłów procesu roboczego. Zmiana rozmiaru może być przydatna do zmniejszenia klastra, gdy nie jest on używany, lub zwiększenia mocy obliczeniowej w celu spełnienia wymagań bardziej zaawansowanych zadań.
   >
   >

   Podczas zmiany rozmiaru klastra, węzłów danych i węzła brzegowego należy uwzględnić pewne czynniki:  

   * Wydajność rozproszonych analiz wykonywanych za pomocą aparatu Spark w oprogramowaniu R Server jest proporcjonalna do liczby węzłów procesu roboczego, jeśli danych jest dużo.  

   * Wydajność analiz oprogramowania R Server jest liniowa w stosunku do rozmiaru analizowanych danych. Na przykład:  

     * Dla małych i średnich ilości danych wydajność jest najwyższa w przypadku analizowania danych w lokalnym kontekście obliczeniowym w węźle brzegowym. Aby uzyskać więcej informacji na temat scenariuszy, w których lokalne konteksty obliczeniowe i konteksty obliczeniowe aparatu Spark działają najlepiej, zobacz [Compute context options for R Server on HDInsight](r-server-compute-contexts.md) (Opcje kontekstu obliczeniowego dla oprogramowania R Server w usłudze HDInsight).<br>
     * Jeśli zalogujesz się do węzła brzegowego i uruchomisz skrypt języka R, wszystkie funkcje poza funkcjami rx programu ScaleR zostaną uruchomione *lokalnie* w węźle brzegowym. Z tego względu musisz odpowiednio dostosować wielkość pamięci i liczbę rdzeni węzła brzegowego. To samo dotyczy sytuacji, w której oprogramowanie R Server w usłudze HDI jest używane z komputera przenośnego jako zdalny kontekst obliczeniowy.

   Użyj przycisku **Wybierz**, aby zapisać konfigurację cen węzła.

   ![Okienko z warstwami cenowymi węzłów](./media/r-server-get-started/pricingtier.png)

   Dostępny jest również link **Pobierz szablon i parametry**. Wybranie tego linku spowoduje wyświetlenie skryptów, których można użyć do zautomatyzowania tworzenia klastra z wybraną konfiguracją. Te skrypty są także dostępne z pozycji Azure Portal dla klastra po jego utworzeniu.

   > [!NOTE]
   > Tworzenie klastra zajmuje trochę czasu, zwykle około 20 minut. Użyj kafelka na tablicy startowej lub pozycji **Powiadomienia** w lewej części strony, aby sprawdzić postęp procesu tworzenia.
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-to-rstudio-server"></a>Łączenie z programem RStudio Server

Jeśli wybrano opcję instalacji programu RStudio Server Community Edition, można się do niego zalogować na dwa sposoby:

- Przejdź do następującego adresu URL (gdzie *NAZWA-KLASTRA* to nazwa utworzonego klastra):

    https://*NAZWA-KLASTRA*.azurehdinsight.net/rstudio/

- Otwórz pozycję klastra w witrynie Azure Portal, wybierz szybki link **Pulpity nawigacyjne oprogramowania R Server**, a następnie wybierz **pulpit nawigacyjny programu R Studio**:

  ![Dostęp do pulpitu nawigacyjnego programu RStudio](./media/r-server-get-started/rstudiodashboard1.png)

  ![Dostęp do pulpitu nawigacyjnego programu RStudio](./media/r-server-get-started/rstudiodashboard2.png)

> [!IMPORTANT]
> Niezależnie od wybranej metody, pierwsze logowanie wymaga dwukrotnego uwierzytelnienia. Podczas pierwszego uwierzytelniania podaj identyfikator użytkownika administratora klastra i hasło. Przy drugim monicie podaj identyfikator użytkownika SSH i hasło. Podczas kolejnych logowań będą wymagane tylko identyfikator użytkownika SSH i hasło.

<a name="connect-to-edge-node"></a>
## <a name="connect-to-the-r-server-edge-node"></a>Łączenie z węzłem krawędzi oprogramowania R Server

Następujące polecenie umożliwia połączenie się z węzłem brzegowym oprogramowania R Server klastra usługi HDInsight za pomocą protokołu SSH:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> Adres `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` można znaleźć w witrynie Azure Portal. Wybierz klaster, a następnie wybierz pozycję **Wszystkie ustawienia** > **Aplikacje** > **RServer**. Spowoduje to wyświetlenie informacji o punkcie końcowym SSH dla węzła brzegowego.
>
> ![Obraz punktu końcowego SSH dla węzła brzegowego](./media/r-server-get-started/sshendpoint.png)
>
>

Jeśli do zabezpieczenia konta użytkownika SSH użyto hasła, zostanie wyświetlony monit o jego wprowadzenie. Jeśli używasz klucza publicznego, może być konieczne użycie parametru `-i` w celu określenia zgodnego klucza prywatnego. Na przykład:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Aby uzyskać więcej informacji, zobacz [Łączenie się z usługą HDInsight (Hadoop) przy użyciu protokołu SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

Po nawiązaniu połączenia zostanie wyświetlony monit podobny do następującego:

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a>Włączanie obsługi równoczesnych użytkowników

Można umożliwić jednoczesną pracę wielu użytkowników, dodając użytkowników do węzła krawędzi, na którym jest uruchomiony program RStudio Community.

Podczas tworzenia klastra usługi HDInsight musisz podać dwóch użytkowników: użytkownika HTTP i użytkownika SSH.

![Wprowadzanie poświadczeń użytkownika klastra i użytkownika SSH](./media/r-server-get-started/concurrent-users-1.png)

- **Nazwa użytkownika logowania klastra**: użytkownik HTTP uwierzytelniany za pośrednictwem bramy HDInsight, która umożliwia ochronę utworzonych klastrów usługi HDInsight. Za pomocą użytkownika HTTP można uzyskiwać dostęp do interfejsu użytkownika Ambari lub YARN oraz innych składników interfejsu użytkownika.
- **Nazwa użytkownika protokołu SSH (Secure Shell)**: użytkownik SSH zapewniający dostęp do klastra za pośrednictwem protokołu Secure Shell. Jest to użytkownik systemu Linux, który ma dostęp do wszystkich węzłów głównych, węzłów procesu roboczego oraz węzłów krawędzi. Pozwala to na korzystanie z dowolnego węzła klastra zdalnego za pomocą protokołu SSH.

W mechanizmie logowania wersji programu RStudio Server Community używanej w oprogramowaniu Microsoft R Server w klastrze typu HDInsight są akceptowane tylko nazwa użytkownika i hasło systemu Linux. Przekazywanie tokenów nie jest obsługiwane. Jeśli chcesz użyć programu RStudio do uzyskania dostępu do nowo utworzonego klastra, musisz zalogować się dwukrotnie.

1. Zaloguj się przy użyciu poświadczeń użytkownika HTTP za pośrednictwem bramy usługi HDInsight: 

    ![Wprowadzanie poświadczeń użytkownika HTTP](./media/r-server-get-started/concurrent-users-2a.png)

2. Zaloguj się do programu RStudio przy użyciu poświadczeń użytkownika SSH:
  
    ![Wprowadzanie poświadczeń użytkownika SSH](./media/r-server-get-started/concurrent-users-2b.png)

Aktualnie podczas aprowizowania klastra usługi HDInsight można utworzyć tylko jedno konto użytkownika SSH. Dlatego aby umożliwić wielu użytkownikom dostęp do oprogramowania Microsoft R Server w klastrach usługi HDInsight, należy utworzyć dodatkowych użytkowników w systemie Linux.

Ponieważ wersja Community programu RStudio Server działa w węźle brzegowym klastra, wymagane jest wykonanie trzech czynności:

1. Zaloguj się do węzła brzegowego przy użyciu poświadczeń utworzonego użytkownika SSH.
2. Dodaj użytkowników systemu Linux w węźle brzegowym.
3. Za pomocą utworzonego użytkownika możesz korzystać z programu RStudio Community.

### <a name="use-the-created-ssh-user-to-log-in-to-the-edge-node"></a>Zaloguj się do węzła krawędzi przy użyciu poświadczeń utworzonego użytkownika SSH

Pobierz dowolne narzędzie SSH (takie jak PuTTY) i zaloguj się za pomocą istniejącego konta użytkownika SSH. Aby uzyskać dostęp do węzła krawędzi, postępuj zgodnie z instrukcjami podanymi w temacie [Łączenie się z usługą HDInsight (Hadoop) przy użyciu protokołu SSH](../hdinsight-hadoop-linux-use-ssh-unix.md). Adres węzła brzegowego oprogramowania R Server w klastrze usługi HDInsight to: **nazwa_klastra-ed-ssh.azurehdinsight.net**


### <a name="add-more-linux-users-to-the-edge-node"></a>Dodawanie użytkowników systemu Linux w węźle brzegowym

Aby dodać użytkownika do węzła brzegowego, uruchom te polecenia:

    sudo useradd yournewusername -m
    sudo passwd yourusername

Powinny zostać zwrócone następujące elementy: 

![Dane wyjściowe poleceń sudo](./media/r-server-get-started/concurrent-users-3.png)

Gdy pojawi się monit o podanie bieżącego hasła protokołu Kerberos, po prostu go zignoruj, wybierając klawisz Enter. Podanie opcji `-m` w poleceniu `useradd` powoduje, że system utworzy folder macierzysty użytkownika. Ten folder jest wymagany przez program RStudio Community.


### <a name="use-the-rstudio-community-version-with-the-created-user"></a>Korzystanie z programu RStudio Community za pomocą utworzonego użytkownika

Zaloguj się do programu RStudio za pomocą utworzonego użytkownika:

![Strona logowania programu RStudio](./media/r-server-get-started/concurrent-users-4.png)

Program RStudio wyświetla informację, że do logowania się w klastrze jest używane nowe konto użytkownika (w tym przypadku **sshuser6**): 

![Lokalizacja nowego użytkownika na pasku poleceń programu RStudio](./media/r-server-get-started/concurrent-users-5.png)

Jednocześnie w innym oknie przeglądarki możesz także zalogować się przy użyciu oryginalnych poświadczeń (domyślnie: **sshuser**).

Zadania można przesyłać za pomocą funkcji programu ScaleR. Oto przykładowe polecenia służące do uruchamiania zadania:

    # Set the HDFS (Azure Blob storage) location of example data
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storing data temporarily
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data to the tmp folder
    remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

    # Set the directory in bigDataDirRoot to load the data
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create the directory
    rxHadoopMakeDir(inputDir)

    # Copy the data from source to input
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define the HDFS (Blob storage) file system
    hdfsFS <- RxHdfsFileSystem()

    # Create an info list for the airline data
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all the column names
    varNames <- names(airlineColInfo)

    # Define the text data source in HDFS
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define the text data source in the local system
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify the formula to use
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define the Spark compute context
    mySparkCluster <- RxSpark()

    # Set the compute context
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary
    summary(modelSpark)


Zwróć uwagę, że przesłane zadania mają inne nazwy użytkowników w interfejsie użytkownika YARN:

![Lista użytkowników w interfejsie użytkownika YARN](./media/r-server-get-started/concurrent-users-6.png)

Pamiętaj, że nowo dodani użytkownicy nie mają uprawnień użytkownika root w systemie Linux. Mają jednak takie same prawa dostępu do wszystkich plików w zdalnym systemie plików HDFS i magazynie obiektów blob.


<a name="use-r-console"></a>
## <a name="use-the-r-console"></a>Użycie konsoli R

1. W sesji SSH wpisz następujące polecenie, aby uruchomić konsolę R:  

        R

2. Powinny zostać wyświetlone dane wyjściowe podobne do następujących:
    
        R version 3.2.2 (2015-08-14) -- "Fire Safety"
        Copyright (C) 2015 The R Foundation for Statistical Computing
        Platform: x86_64-pc-linux-gnu (64-bit)

        R is free software and comes with ABSOLUTELY NO WARRANTY.
        You are welcome to redistribute it under certain conditions.
        Type 'license()' or 'licence()' for distribution details.

        Natural language support but running in an English locale

        R is a collaborative project with many contributors.
        Type 'contributors()' for more information and
        'citation()' on how to cite R or R packages in publications.

        Type 'demo()' for some demos, 'help()' for on-line help, or
        'help.start()' for an HTML browser interface to help.
        Type 'q()' to quit R.

        Microsoft R Server version 8.0: an enhanced distribution of R
        Microsoft packages Copyright (C) 2016 Microsoft Corporation

        Type 'readme()' for release notes.
        >

3. W monicie `>` możesz podać kod R. Oprogramowanie R Server zawiera pakiety, które umożliwiają łatwą współpracę z usługą Hadoop i uruchamianie obliczeń rozproszonych. Na przykład następujące polecenie umożliwia wyświetlenie katalogu głównego domyślnego systemu plików klastra usługi HDInsight:

        rxHadoopListFiles("/")

4. Dostępne jest także adresowanie w stylu magazynu obiektów blob:

        rxHadoopListFiles("wasb:///")


## <a name="use-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>Używanie oprogramowania R Server w usłudze HDI ze zdalnego wystąpienia oprogramowania Microsoft R Server lub programu Microsoft R Client

Możliwe jest skonfigurowanie dostępu do kontekstu obliczeniowego aparatu Spark usługi Hadoop w usłudze HDI ze zdalnego wystąpienia programu Microsoft R Server lub programu Microsoft R Client uruchomionego na komputerze stacjonarnym lub przenośnym. Aby uzyskać więcej informacji, zobacz sekcję Using Microsoft R Server as a Hadoop Client (Używanie oprogramowania Microsoft R Server jako klienta usługi Hadoop) tematu [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md) (Tworzenie kontekstu obliczeniowego dla aparatu Spark). W tym celu określ następujące opcje podczas definiowania kontekstu obliczeniowego programu RxSpark na komputerze przenośnym: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches i sshProfileScript. Oto przykład:


    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )


## <a name="use-a-compute-context"></a>Używanie kontekstu obliczeniowego

Kontekst obliczeniowy pozwala określić, czy obliczenia są wykonywane lokalnie w węźle brzegowym, czy są rozproszone w węzłach klastra usługi HDInsight.

1. W programie RStudio Server lub konsoli R (w ramach sesji SSH) załaduj przykładowe dane do domyślnego magazynu usługi HDInsight za pomocą następującego kodu:

        # Set the HDFS (Blob storage) location of example data
        bigDataDirRoot <- "/example/data"

        # Create a local folder for storing data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data to the tmp folder
        remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
        download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
        download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
        download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
        download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
        download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
        download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
        download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
        download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
        download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
        download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
        download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
        download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

        # Set the directory in bigDataDirRoot to load the data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make the directory
        rxHadoopMakeDir(inputDir)

        # Copy the data from source to input
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. Utwórz trochę informacji o danych i zdefiniuj dwa źródła danych, aby umożliwić pracę z danymi:

        # Define the HDFS (Blob storage) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create an info list for the airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # Get all the column names
        varNames <- names(airlineColInfo)

        # Define the text data source in HDFS
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define the text data source in the local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # Formula to use
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. Uruchom regresję logistyczną na danych za pomocą lokalnego kontekstu obliczeniowego:

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    Powinny zostać wyświetlone dane wyjściowe kończące się wierszami podobnymi do następujących:

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                         Estimate Std. Error z value Pr(>|z|)
         (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
         ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
         ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
         ......
         DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
         DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
         DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
         DEST=BPT         Dropped    Dropped Dropped  Dropped

         ---

         Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

         Condition number of final variance-covariance matrix: 11904202
         Number of iterations: 7

4. Przeprowadź tę samą regresję logistyczną za pomocą kontekstu aparatu Spark. Dzięki kontekstowi aparatu Spark przetwarzanie jest dystrybuowane do wszystkich węzłów procesu roboczego w klastrze usługi HDInsight.

        # Define the Spark compute context
        mySparkCluster <- RxSpark()

        # Set the compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > Możesz także użyć funkcji MapReduce do rozproszenia obliczeń na węzłach klastra. Aby uzyskać więcej informacji na temat kontekstu obliczeniowego, zobacz [Compute context options for R Server on HDInsight](r-server-compute-contexts.md) (Opcje kontekstu obliczeniowego dla oprogramowania R Server w usłudze HDInsight).


## <a name="distribute-r-code-to-multiple-nodes"></a>Dystrybucja kodu R do wielu węzłów

Przy użyciu oprogramowania R Server możesz w łatwy sposób uruchomić istniejący kod R w wielu węzłach klastra za pomocą programu `rxExec`. Funkcja ta jest przydatna podczas czyszczenia parametrów lub przeprowadzania symulacji. Poniższy kod przedstawia przykładowe użycie programu `rxExec`:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Jeśli nadal używasz kontekstu Spark lub MapReduce, uruchomienie tego polecenia spowoduje zwrócenie wartości `nodename` dla węzłów procesu roboczego, w których uruchomiono kod `(Sys.info()["nodename"])`. Na przykład w przypadku klastra składającego się z czterech węzłów dane wyjściowe mogą być podobne do następujących:

    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"


## <a name="access-data-in-hive-and-parquet"></a>Dostęp do danych w usługach Hive i Parquet

Funkcja dostępna w oprogramowaniu R Server 9.1 umożliwia bezpośredni dostęp do danych w usługach Hive i Parquet w celu użycia ich w funkcjach programu ScaleR w kontekście obliczeniowym aparatu Spark. Te możliwości są dostępne za pomocą nowych funkcji źródła danych programu ScaleR o nazwie RxHiveData i RxParquetData. Te funkcje używają kodu Spark SQL do ładowania danych bezpośrednio do elementów DataFrame aparatu Spark na potrzeby analizy przez program ScaleR.  

Poniżej przedstawiono przykładowy kod korzystający z nowych funkcji:

    #Create a Spark compute context
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, clean up, and close the Spark session
    lsObj <- rxSparkListData() #Two data objects are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() #It should show an empty list
    rxSparkDisconnect(myHadoopCluster)


Więcej informacji na temat używania tych nowych funkcji zawiera pomoc online oprogramowania R Server dostępna przy użyciu poleceń `?RxHivedata` i `?RxParquetData`.  


## <a name="install-additional-r-packages-on-the-edge-node"></a>Instalowanie dodatkowych pakietów R w węźle krawędzi

Jeśli chcesz zainstalować dodatkowe pakiety R w węźle brzegowym, możesz użyć polecenia `install.packages()` bezpośrednio z konsoli R, gdy masz połączenie SSH z węzłem brzegowym. Jednak jeśli musisz zainstalować pakiety R na węzłach procesu roboczego klastra, musisz użyć akcji skryptu.

Akcje skryptu to skrypty powłoki Bash używane do wprowadzania zmian w konfiguracji klastra usługi HDInsight lub instalowania dodatkowego oprogramowania, np. pakietów R. Aby zainstalować dodatkowe pakiety przy użyciu akcji skryptu, wykonaj następujące kroki.

> [!IMPORTANT]
> Dodatkowe pakiety R można zainstalować przy użyciu akcji skryptu dopiero po utworzeniu klastra. Nie wykonuj tej procedury podczas tworzenia klastra, ponieważ skrypt wymaga w pełni zainstalowanego i skonfigurowanego oprogramowania R Server.
>
>

1. W witrynie [Azure Portal](https://portal.azure.com) wybierz oprogramowanie R Server lub klaster usługi HDInsight.

2. W okienku **Ustawienia** wybierz pozycję **Akcje skryptu** > **Prześlij nową**.

   ![Obraz okienka akcji skryptu](./media/r-server-get-started/scriptaction.png)

3. W okienku **Prześlij akcję skryptu** podaj następujące informacje:

   * **Nazwa**: przyjazna nazwa identyfikująca skrypt.

   * **Identyfikator URI skryptu powłoki systemowej**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Główny**: to pole powinno być wyczyszczone.

   * **Proces roboczy**: to pole powinno być wyczyszczone.

   * **Dozorca**: to pole powinno być wyczyszczone.

   * **Węzły brzegowe**: to pole powinno być zaznaczone.

   * **Parametry**: pakiety R do zainstalowania, na przykład `bitops stringr arules`.

   * **Utrwal ten skrypt**: to pole powinno być zaznaczone.  

   > [!NOTE]
   > Domyślnie wszystkie pakiety R są instalowane z migawki repozytorium Microsoft R Application Network odpowiedniej do zainstalowanej wersji oprogramowania R Server. Jeśli chcesz zainstalować nowsze wersje pakietów, musisz uwzględnić pewne ryzyko niezgodności. Jednak możesz to zrobić za pomocą parametru `useCRAN` użytego jako pierwszy element listy pakietów, na przykład `useCRAN bitops, stringr, arules`.  
   > 
   > Niektóre pakiety R wymagają dodatkowych bibliotek systemu Linux. Dla Twojej wygody preinstalowaliśmy zależności dla 100 najpopularniejszych pakietów R. Jeśli instalowane pakiety R wymagają jeszcze innych bibliotek, musisz pobrać skrypt podstawowy użyty tutaj i dodać kroki instalowania bibliotek systemowych. Następnie musisz przekazać zmodyfikowany skrypt do publicznego kontenera obiektów blob w usłudze Azure Storage i użyć zmodyfikowanego skryptu do zainstalowania pakietów.
   >
   > Aby uzyskać informacje na temat tworzenia akcji skryptu, zobacz [Script Action development](../hdinsight-hadoop-script-actions-linux.md) (Tworzenie akcji skryptu).  
   >
   >

   ![Dodawanie akcji skryptu](./media/r-server-get-started/submitscriptaction.png)

4. Wybierz polecenie **Utwórz**, aby uruchomić skrypt. Po zakończeniu działania skryptu pakiety R będą dostępne we wszystkich węzłach procesu roboczego.


## <a name="configure-microsoft-r-server-operationalization"></a>Konfigurowanie operacjonalizacji oprogramowania Microsoft R Server

Po zakończeniu modelowania danych możesz zopernacjonalizować model, aby wykonywać prognozowanie. Aby skonfigurować funkcję operacjonalizacji oprogramowania Microsoft R Server, wykonaj poniższe kroki:

1. W przypadku węzła brzegowego użyj `ssh` polecenia — na przykład: 

       ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

2. Zmień katalog dla odpowiedniej wersji i użyj polecenia `sudo dotnet` dla pliku dll. 

   W przypadku oprogramowania Microsoft R Server 9.1:

       cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0
       sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

   W przypadku oprogramowania Microsoft R Server 9.0:

       cd /usr/lib64/microsoft-deployr/9.0.1
       sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. Aby skonfigurować operacjonalizację oprogramowania Microsoft R Server pod kątem jednej maszyny, wykonaj następujące kroki:

   a. Wybierz pozycję `Configure R Server for Operationalization`.

   b. Wybierz pozycję `A. One-box (web + compute nodes)`.

   d. Wprowadź hasło dla użytkownika `admin`.

   ![Operacjonalizacja pod kątem jednej maszyny](./media/r-server-get-started/admin-util-one-box-.png)

4. Opcjonalnie możesz wykonać test diagnostyczny w sposób pokazany poniżej:

   a. Wybierz pozycję `6. Run diagnostic tests`.

   b. Wybierz pozycję `A. Test configuration`.

   d. Jako nazwę użytkownika wpisz ciąg `admin`. Wpisz też hasło określone w poprzednim kroku konfiguracji.

   d. Potwierdź `Overall Health = pass`.

   e. Zamknij narzędzie administracyjne.

   f. Zamknij połączenie SSH.

   ![Diagnostyka operacjonalizacji](./media/r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
>Jeśli wystąpią duże opóźnienia podczas próby korzystania z usługi internetowej utworzonej za pomocą funkcji mrsdeploy w ramach kontekstu obliczeniowego platformy Spark, może być konieczne dodanie niektórych brakujących folderów. Aplikacja Spark należy do użytkownika o nazwie „*rserve2*” zawsze wtedy, gdy jest wywoływana z usługi internetowej przy użyciu funkcji mrsdeploy. Aby obejść ten problem:

    #Create these required folders for user rserve2 in local and HDFS
    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    #Create a new Spark compute context 
    rxSparkConnect(reset = TRUE)


Na tym etapie konfiguracja operacjonalizacji jest ukończona. Teraz za pomocą pakietu mrsdeploy w programie R Client można nawiązywać połączenia z funkcją operacjonalizacji w węźle brzegowym. Można rozpocząć korzystanie z jej funkcji, takich jak [zdalne wykonywanie](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) i [usługi internetowe](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette). W zależności od tego, czy klaster został skonfigurowany w sieci wirtualnej, może być konieczne skonfigurowanie tunelowania przekierowania portów za pomocą logowania SSH.

### <a name="r-server-cluster-on-a-virtual-network"></a>Klaster oprogramowania R Server w sieci wirtualnej

Upewnij się, że ruch przez port 12800 węzła brzegowego jest dozwolony. Pozwala to użyć węzła brzegowego do nawiązania połączenia z funkcją operacjonalizacji.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Jeśli metoda `remoteLogin()` nie może połączyć się z węzłem brzegowym, lecz nawiązanie połączenia SSH z węzłem brzegowym jest możliwe, sprawdź, czy reguła zezwalająca na ruch przez port 12800 jest skonfigurowana poprawnie. Jeśli problem nie ustąpi, możesz go obejść, konfigurując tunelowanie przekierowania portów przez połączenie SSH. Odpowiednie instrukcje znajdują się w następującej sekcji.

### <a name="r-server-cluster-not-set-up-on-a-virtual-network"></a>Klaster oprogramowania R Server nie w sieci wirtualnej

Jeśli klaster nie jest skonfigurowany w sieci wirtualnej lub występują problemy z korzystaniem z sieci wirtualnej, możesz użyć tunelowania przekierowania portów za pomocą protokołu SSH:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Można to też skonfigurować w programie PuTTY:

![Połączenie SSH w programie PuTTY](./media/r-server-get-started/putty.png)

Gdy sesja SSH jest aktywna, ruch z portu 12800 maszyny jest przekazywany do portu 12800 węzła brzegowego za pomocą sesji SSH. Upewnij się, że w metodzie `remoteLogin()` użyto adresu `127.0.0.1:12800`. Ta metoda umożliwi logowanie do funkcji operacjonalizacji węzła brzegowego przez przekierowanie portów.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Skalowanie węzłów obliczeniowych operacjonalizacji oprogramowania Microsoft R Server w węzłach procesu roboczego usługi HDInsight

### <a name="decommission-the-worker-nodes"></a>Likwidowanie węzłów procesu roboczego

Oprogramowanie Microsoft R Server nie jest aktualnie zarządzane za pomocą usługi Yarn. Jeśli węzły procesu roboczego nie zostaną zlikwidowane, menedżer zasobów usługi Yarn nie będzie działać w oczekiwany sposób, ponieważ nie będzie znał zasobów zajętych przez serwer. Aby tego uniknąć, zalecamy zlikwidowanie węzłów procesu roboczego przed przystąpieniem do skalowania węzłów obliczeniowych na zewnątrz.

Aby zlikwidować węzły procesu roboczego:

1. Zaloguj się do konsoli Ambari klastra usługi HDI i wybierz kartę **Hosts** (Hosty).
2. Wybierz węzły procesu roboczego do zlikwidowania, a następnie wybierz pozycję **Actions** > **Selected Hosts** > **Hosts** > **Turn On Maintenance Mode** (Akcje > Wybrane hosty > Hosty > Włącz tryb konserwacji). Na przykład na poniższej ilustracji węzły wn3 i wn4 są przeznaczone do likwidacji.  

   ![Zrzut ekranu przedstawiający polecenia włączania trybu konserwacji](./media/r-server-get-started/get-started-operationalization.png)  

3. Wybierz pozycję **Actions** > **Selected Hosts** > **DataNodes** > **Decommission** (Akcje > Wybrane hosty > Węzły danych > Likwiduj).
4. Wybierz pozycję **Actions** > **Selected Hosts** > **NodeManagers** > **Decommission** (Akcje > Wybrane hosty > Menedżerowie węzła > Likwiduj).
5. Wybierz pozycję **Actions** > **Selected Hosts** > **DataNodes** > **Stop** (Akcje > Wybrane hosty > Węzły danych > Zatrzymaj).
6. Wybierz pozycję **Actions** > **Selected Hosts** > **NodeManagers** > **Stop** (Akcje > Wybrane hosty > Menedżerowie węzła > Zatrzymaj).
7. Wybierz pozycję **Actions** > **Selected Hosts** > **Hosts** > **Stop All Components** (Akcje > Wybrane hosty > Hosty > Zatrzymaj wszystkie składniki).
8. Usuń zaznaczenie węzłów procesu roboczego i wybierz węzły główne.
9. Wybierz pozycję **Actions** > **Selected Hosts** > **Hosts** > **Restart All Components** (Akcje > Wybrane hosty > Hosty > Uruchom ponownie wszystkie składniki).

### <a name="configure-compute-nodes-on-each-decommissioned-worker-node"></a>Konfigurowanie węzłów obliczeniowych we wszystkich zlikwidowanych węzłach procesu roboczego

1. Za pomocą protokołu SSH połącz się z każdym zlikwidowanym węzłem procesu roboczego.
2. Uruchom narzędzie administracyjne za pomocą polecenia `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.
3. Wprowadź `1`, aby wybrać opcję `Configure R Server for Operationalization`.
4. Wprowadź `c`, aby wybrać opcję `C. Compute node`. Ten krok umożliwi skonfigurowanie węzła obliczeniowego w węźle procesu roboczego.
5. Zamknij narzędzie administracyjne.

### <a name="add-compute-nodes-details-on-the-web-node"></a>Dodawanie szczegółów węzłów obliczeniowych w węźle internetowym

Po skonfigurowaniu wszystkich zlikwidowanych węzłów procesu roboczego pod kątem uruchamiania węzła obliczeniowego wróć do węzła brzegowego i dodaj adresy IP zlikwidowanych węzłów procesu roboczego do konfiguracji węzła internetowego oprogramowania Microsoft R Server:

1. Połącz się z węzłem brzegowym za pomocą protokołu SSH.
2. Uruchom polecenie `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.
3. W sekcji `URIs` dodaj adresy IP i porty węzłów procesu roboczego.

    ![Wiersz polecenia dla węzła brzegowego](./media/r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a>Rozwiązywanie problemów

W razie problemów podczas tworzenia klastrów usługi HDInsight zapoznaj się z [wymaganiami dotyczącymi kontroli dostępu](../hdinsight-administer-use-portal-linux.md#create-clusters).


## <a name="next-steps"></a>Następne kroki

Wiesz już, jak utworzyć klaster usługi HDInsight zawierający oprogramowanie R Server. Znasz też podstawy używania konsoli R w sesji SSH. W poniższych tematach opisano inne sposoby korzystania z serwera R Server w usłudze HDInsight oraz zarządzania nim:

* [Compute context options for R Server on HDInsight](r-server-compute-contexts.md) (Opcje kontekstu obliczeniowego dla oprogramowania R Server w usłudze HDInsight)
* [Azure Storage options for R Server on HDInsight](r-server-storage.md) (Opcje usługi Azure Storage dla oprogramowania R Server w usłudze HDInsight)
