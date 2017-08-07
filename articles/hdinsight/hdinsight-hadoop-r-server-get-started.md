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
ms.date: 07/13/2017
ms.author: bradsev
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: f0ee0d23f28df2824ea41f7c9f7490e1ec62d041
ms.contentlocale: pl-pl
ms.lasthandoff: 07/27/2017

---
# <a name="get-started-using-r-server-on-hdinsight"></a>Wprowadzenie do korzystania z oprogramowania R Server w usłudze HDInsight

Usługa HDInsight obejmuje opcję oprogramowania R Server, którą można zintegrować z klastrem usługi HDInsight. Opcja ta pozwala skryptom języka R używać aparatu Spark i funkcji MapReduce do wykonywania obliczeń rozproszonych. Ten dokument umożliwia poznanie procedury tworzenia oprogramowania R Server w klastrze usługi HDInsight, a następnie uruchamiania skryptu R, który demonstruje sposób użycia aparatu Spark na potrzeby wykonywania rozproszonych obliczeń przez kod R.


## <a name="prerequisites"></a>Wymagania wstępne

* **Subskrypcja platformy Azure**: przed rozpoczęciem tego samouczka musisz mieć subskrypcję platformy Azure. Aby uzyskać więcej informacji, przejdź do artykułu [Get Microsoft Azure free trial (Uzyskaj bezpłatną wersję próbną platformy Azure)](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Klient protokołu Secure Shell (SSH)**: klient SSH jest używany do zdalnego łączenia z klastrem usługi HDInsight i uruchamiania poleceń bezpośrednio w klastrze. Aby uzyskać więcej informacji, zobacz [Używanie protokołu SSH w usłudze HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
* **Klucze SSH (opcjonalnie)**: konto SSH użyte do nawiązania połączenia z klastrem można zabezpieczyć przy użyciu hasła lub klucza publicznego. Użycie hasła jest łatwiejsze i umożliwia rozpoczęcie pracy bez konieczności tworzenia pary kluczy publiczny-prywatny. Jednak użycie klucza jest bezpieczniejsze.

> [!NOTE]
> W krokach przedstawionych w tym dokumencie przyjęto założenie, że jest używane hasło.


## <a name="automated-cluster-creation"></a>Zautomatyzowane tworzenie klastra

Aby zautomatyzować tworzenie serwerów HDInsight R Server, możesz użyć szablonów usługi Azure Resource Manager, zestawu SDK oraz programu PowerShell.

* Aby utworzyć serwer R Server za pomocą szablonu usługi Azure Resource Management, zobacz [Deploy an R server HDInsight cluster (Wdrażanie klastra usługi HDInsight serwera R Server)](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).
* Aby utworzyć serwer R Server za pomocą zestawu .NET SDK, zobacz [Create Linux-based clusters in HDInsight using the .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) (Tworzenie klastrów opartych na systemie Linux w usłudze HDInsight przy użyciu zestawu .NET SDK).
* Aby wdrożyć serwer R Server za pomocą programu PowerShell, zobacz artykuł [Creating an R Server on HDInsight with PowerShell (Tworzenie serwera R Server w usłudze HDInsight przy użyciu programu PowerShell)](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-the-cluster-using-the-azure-portal"></a>Tworzenie klastra przy użyciu witryny Azure Portal

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Wybierz pozycję **Nowy** -> **Rozwiązania inteligentne + analiza** -> **HDInsight**.

    ![Obraz tworzenia nowego klastra](./media/hdinsight-hadoop-r-server-get-started/newcluster.png)

3. W środowisku **Szybkie tworzenie** podaj nazwę klastra w polu **Nazwa klastra**. Jeśli masz wiele subskrypcji platformy Azure, użyj pozycji **Subskrypcja**, aby wybrać subskrypcję do użycia.

    ![Wybór nazwy klastra i subskrypcji](./media/hdinsight-hadoop-r-server-get-started/clustername.png)

4. Wybierz pozycję **Typ klastra**, aby otworzyć blok **Konfiguracja klastra**. W bloku **Konfiguracja klastra** wybierz następujące opcje:

    * **Typ klastra**: R Server
    * **Wersja**: wybierz wersję oprogramowania R Server do zainstalowania w klastrze. Aktualnie dostępna jest wersja ***R Server 9.1 (HDI 3.6)***. Informacje o wersji dotyczące dostępnych wersji oprogramowania R Server można znaleźć [tutaj](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).
    * **Program R Studio Community Edition for R Server**: to środowisko IDE oparte na przeglądarce, które jest instalowane domyślnie w węźle krawędzi. Jeśli nie chcesz go instalować, usuń zaznaczenie pola wyboru. Jeśli wybierzesz opcję instalacji, adres URL umożliwiający logowanie do programu RStudio Server będzie dostępny w bloku aplikacji portalu dla utworzonego klastra.
    * Pozostaw wartości domyślne innych opcji i użyj przycisku **Wybierz**, aby zapisać typ klastra.

        ![Zrzut ekranu bloku typu klastra](./media/hdinsight-hadoop-r-server-get-started/clustertypeconfig.png)

5. Podaj wartości w pozycjach **Nazwa użytkownika logowania klastra** i **Hasło logowania klastra**.

    Określ wartość w pozycji **Nazwa użytkownika SSH**. Protokół SSH jest używany do zdalnego łączenia z klastrem przy użyciu klienta protokołu **Secure Shell (SSH)**. Użytkownika SSH można określić w tym oknie dialogowym lub po utworzeniu klastra (na karcie Konfiguracja klastra). Oprogramowanie R Server jest skonfigurowane pod kątem użycia **nazwy użytkownika SSH** „remoteuser”.  **Jeśli używasz innej nazwy użytkownika, musisz wykonać dodatkowy krok po utworzeniu klastra.**

    Pozostaw zaznaczone pole **Użyj tego samego hasła podczas logowania do klastra**, aby użyć typu uwierzytelniania **HASŁO**, chyba że wolisz użyć klucza publicznego.  Jeśli planujesz uzyskiwanie dostępu do oprogramowania R Server w klastrze za pomocą zdalnego klienta, na przykład programu RTVS, RStudio lub innego komputerowego środowiska IDE, będzie potrzebna para kluczy publiczny-prywatny. W przypadku instalowania programu RStudio Server Community Edition, musisz wybrać hasło SSH.     

    Aby utworzyć parę kluczy publiczny-prywatny, usuń zaznaczenie pola **Użyj tego samego hasła podczas logowania do klastra**, a następnie wybierz typ uwierzytelniania **KLUCZ PUBLICZNY** i kontynuuj w podany poniżej sposób. W poniższych instrukcjach przyjęto, że jest zainstalowane środowisko Cygwin z programem ssh-keygen lub równoważnym.

    * Wygeneruj parę kluczy publiczny-prywatny w wierszu polecenia na komputerze przenośnym:

        ssh-keygen -t rsa -b 2048

    * Podaj nazwę pliku klucza po wyświetleniu monitu, a następnie określ hasło, aby podwyższyć poziom zabezpieczeń. Ekran powinien wyglądać podobnie do następującego:

        ![Wiersz polecenia SSH w systemie Windows](./media/hdinsight-hadoop-r-server-get-started/sshcmdline.png)

    * Polecenie to pozwala utworzyć plik klucza prywatnego i plik klucza publicznego o nazwie <nazwa-pliku-klucza-prywatnego>.pub, na przykład furiosa i furiosa.pub.

        ![Katalog polecenia SSH](./media/hdinsight-hadoop-r-server-get-started/dir.png)

    * Następnie określ plik klucza publicznego (&#42;.pub) podczas przypisywania poświadczeń klastra usługi HDI oraz potwierdź grupę zasobów i region, po czym wybierz pozycję **Dalej**.

        ![Blok poświadczeń](./media/hdinsight-hadoop-r-server-get-started/publickeyfile.png)  

   * Zmień uprawnienia do pliku klucza prywatnego na komputerze przenośnym:

        chmod 600 <nazwa-pliku-klucza-prywatnego>

   * Użyj pliku klucza prywatnego do zdalnego logowania za pomocą protokołu SSH:

        ssh –i <nazwa-pliku-klucza-prywatnego> remoteuser@<hostname public ip>

      lub jako części definicji kontekstu obliczeniowego aparatu Spark usługi Hadoop dla oprogramowania R Server na kliencie. Zobacz podsekcję **Using Microsoft R Server as a Hadoop Client (Używanie oprogramowania Microsoft R Server jako klienta usługi Hadoop)** tematu [Create a Compute Context for Spark (Tworzenie kontekstu obliczeniowego dla aparatu Spark)](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).

6. Funkcja szybkiego tworzenia przeniesie Cię do bloku **Magazyn**, aby umożliwić wybranie ustawień konta magazynu do użycia dla lokalizacji głównej systemu plików HDFS używanego przez klaster. Wybierz nowe lub istniejące konto usługi Azure Storage lub istniejące konto usługi Data Lake Storage.

    - Jeśli wybierzesz konto usługi Microsoft Azure Storage, możesz wskazać istniejące konto magazynu, wybierając pozycję **Wybierz konto magazynu**, a następnie wybierając odpowiednie konto. Aby utworzyć nowe konto, użyj linku **Utwórz nowe** w sekcji **Wybierz konto magazynu**.

      > [!NOTE]
      > Jeśli wybierzesz pozycję **Nowe**, musisz podać nazwę nowego konta magazynu. Jeśli nazwa zostanie zaakceptowana, pojawi się zielony znacznik.

      Domyślną wartością pola **Kontener domyślny** jest nazwa klastra. Nie zmieniaj tej wartości.

      Jeśli wybrano opcję nowego konta magazynu, zostanie wyświetlony monit o określenie wartości **Lokalizacja** umożliwiającej wskazanie regionu, w którym ma zostać utworzone konto magazynu.  

         ![Blok źródła danych](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > Wybranie lokalizacji domyślnego źródła danych spowoduje także ustawienie lokalizacji klastra usługi HDInsight. Klaster i domyślne źródło danych muszą znajdować się w tym samym regionie.

    - Jeśli chcesz użyć istniejącej usługi Data Lake Store, wybierz konto magazynu usługi ADLS, które ma być używane, i dodaj tożsamość usługi *ADD* klastra do klastra, aby umożliwić dostęp do magazynu. Aby uzyskać więcej informacji na temat tego procesu, zobacz [Tworzenie klastra HDInsight z usługą Data Lake Store za pomocą witryny Azure Portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).

    Użyj przycisku **Wybierz**, aby zapisać konfigurację źródła danych.


7. Zostanie wyświetlony blok **Podsumowanie**, w którym można zweryfikować wszystkie ustawienia. W tym miejscu możesz zmienić wartość pozycji **Rozmiar klastra** w celu zmodyfikowania liczby serwerów w klastrze, a także określić wartość pozycji **Akcje skryptu** definiującej skrypty do uruchomienia. Pozostaw domyślną liczbę węzłów procesu roboczego — `4`, chyba że wiesz, że potrzebujesz większego klastra. Szacowany koszt klastra zostanie pokazany w bloku.

    ![podsumowanie klastra](./media/hdinsight-hadoop-r-server-get-started/clustersummary.png)

   > [!NOTE]
   > W razie potrzeby możesz później zmienić rozmiar klastra w witrynie Portal (**Klaster** -> **Ustawienia** -> **Skaluj klaster**), aby zwiększyć lub zmniejszyć liczbę węzłów procesu roboczego.  Zmiana rozmiaru może być przydatna do zmniejszenia klastra, gdy nie jest on używany, lub zwiększenia mocy obliczeniowej w celu spełnienia wymagań bardziej zaawansowanych zadań.
   >
   >

   Podczas zmiany rozmiaru klastra, węzłów danych i węzła krawędzi należy uwzględnić pewne czynniki:  

   * Wydajność rozproszonych analiz wykonywanych za pomocą aparatu Spark w oprogramowaniu R Server jest proporcjonalna do liczby węzłów procesu roboczego, jeśli danych jest dużo.  

   * Wydajność analiz oprogramowania R Server jest liniowa w stosunku do rozmiaru analizowanych danych. Na przykład:  

     * Dla małych i średnich ilości danych wydajność jest najwyższa w przypadku analizowania w lokalnym kontekście obliczeniowym w węźle krawędzi.  Aby uzyskać więcej informacji na temat scenariuszy, w których lokalne konteksty obliczeniowe i konteksty obliczeniowe aparatu Spark działają najlepiej, zobacz Compute context options for R Server on HDInsight (Opcje kontekstu obliczeniowego dla oprogramowania R Server w usłudze HDInsight).<br>
     * Jeśli zalogujesz się do węzła krawędzi i uruchomisz skrypt języka R, wszystkie funkcje poza funkcjami rx programu ScaleR zostaną uruchomione <strong>lokalnie</strong> w węźle krawędzi. Z tego względu musisz odpowiednio dostosować wielkość pamięci i liczbę rdzeni węzła krawędzi. To samo dotyczy sytuacji, w której oprogramowanie R Server w usłudze HDI jest używane z komputera przenośnego jako zdalny kontekst obliczeniowy.

     ![Blok warstw cenowych węzła](./media/hdinsight-hadoop-r-server-get-started/pricingtier.png)

     Użyj przycisku **Wybierz**, aby zapisać konfigurację cen węzła.

   Dostępny jest również link **Pobierz szablon i parametry**. Kliknięcie tego linku spowoduje wyświetlenie skryptów, których można użyć do zautomatyzowania tworzenia klastra z wybraną konfiguracją. Te skrypty są także dostępne z pozycji Azure Portal dla klastra po jego utworzeniu.

   > [!NOTE]
   > Tworzenie klastra zajmuje trochę czasu, zwykle około 20 minut. Użyj kafelka na tablicy startowej lub pozycji **Powiadomienia** w lewej części strony, aby sprawdzić postęp procesu tworzenia.
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-to-rstudio-server"></a>Łączenie z programem RStudio Server

Jeśli wybrano opcję instalacji programu RStudio Server Community Edition, można się do niego zalogować na dwa sposoby.

1. Przejdź do następującego adresu URL (gdzie **NAZWA-KLASTRA** to nazwa utworzonego klastra):

    https://**NAZWA-KLASTRA**.azurehdinsight.net/rstudio/

2. Otwórz pozycję klastra w witrynie Azure Portal, wybierz szybki link **Pulpity nawigacyjne oprogramowania R Server**, a następnie wybierz **pulpit nawigacyjny programu R Studio**:

     ![Dostęp do pulpitu nawigacyjnego programu R Studio](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Dostęp do pulpitu nawigacyjnego programu R Studio](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > Niezależnie od wybranej metody, pierwsze logowanie wymaga dwukrotnego uwierzytelnienia.  Podczas pierwszego uwierzytelniania podaj *identyfikator użytkownika administratora klastra* i *hasło*. Przy drugim monicie podaj *identyfikator użytkownika SSH* i *hasło*. Podczas kolejnych logowań będą wymagane tylko *hasło SSH* oraz *identyfikator użytkownika*.

<a name="connect-to-edge-node"></a>
## <a name="connect-to-the-r-server-edge-node"></a>Łączenie z węzłem krawędzi oprogramowania R Server

Następujące polecenie umożliwia połączenie się z węzłem krawędzi oprogramowania R Server klastra usługi HDInsight za pomocą protokołu SSH:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> Adres `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` jest także dostępny w witrynie Azure Portal po wybraniu klastra, a następnie pozycji **Wszystkie ustawienia** -> **Aplikacje** -> **RServer**. Spowoduje to wyświetlenie informacji o punkcie końcowym SSH dla węzła krawędzi.
>
> ![Obraz punktu końcowego SSH dla węzła krawędzi](./media/hdinsight-hadoop-r-server-get-started/sshendpoint.png)
>
>

Jeśli do zabezpieczenia konta użytkownika SSH użyto hasła, zostanie wyświetlony monit o jego wprowadzenie. Jeśli używasz klucza publicznego, może być konieczne użycie parametru `-i` w celu określenia zgodnego klucza prywatnego. Na przykład:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Aby uzyskać więcej informacji, zobacz [Łączenie się z usługą HDInsight (Hadoop) przy użyciu protokołu SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

Po nawiązaniu połączenia zostanie wyświetlony monit podobny do następującego:

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a>Włączanie obsługi równoczesnych użytkowników

Można umożliwić jednoczesną pracę wielu użytkowników, dodając użytkowników do węzła krawędzi, na którym jest uruchomiony program RStudio Community.

Podczas tworzenia klastra usługi HDInsight musisz podać dwóch użytkowników: użytkownika HTTP i użytkownika SSH:

![Równoczesny użytkownik 1](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-1.png)

- **Nazwa użytkownika logowania klastra**: użytkownik HTTP uwierzytelniany za pośrednictwem bramy HDInsight, która umożliwia ochronę utworzonych klastrów usługi HDInsight. Przy pomocy użytkownika HTTP można uzyskiwać dostęp do interfejsu użytkownika Ambari lub YARN oraz innych składników interfejsu użytkownika.
- **Nazwa użytkownika protokołu SSH (Secure Shell)**: użytkownik SSH zapewniający dostęp do klastra za pośrednictwem protokołu Secure Shell. Jest to użytkownik systemu Linux, który ma dostęp do wszystkich węzłów głównych, węzłów procesu roboczego oraz węzłów krawędzi. Pozwala to na korzystanie z dowolnego węzła klastra zdalnego za pomocą protokołu Secure Shell.

W mechanizmie logowania wersji programu R Studio Server Community używanej na serwerze Microsoft R Server w klastrze typu HDInsight są akceptowane tylko nazwa użytkownika i hasło systemu Linux. Przekazywanie tokenów nie jest obsługiwane. Jeśli chcesz użyć programu R Studio do uzyskania dostępu do nowo utworzonego klastra, musisz zalogować się dwukrotnie.

- Najpierw zaloguj się przy użyciu poświadczeń użytkownika HTTP za pośrednictwem bramy usługi HDInsight: 

    ![Równoczesny użytkownik 2a](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2a.png)

- Następnie zaloguj się do programu RStudio przy użyciu poświadczeń użytkownika SSH:
  
    ![Równoczesny użytkownik 2b](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2b.png)

Aktualnie podczas aprowizowania klastra usługi HDInsight można utworzyć tylko jedno konto użytkownika SSH. Dlatego aby umożliwić wielu użytkownikom dostęp do serwera Microsoft R Server w klastrach usługi HDInsight, należy utworzyć dodatkowych użytkowników w systemie Linux.

Ponieważ program RStudio Server Community działa w węźle krawędzi klastra, wymagane jest wykonanie kilku czynności:

1. Zaloguj się do węzła krawędzi przy użyciu poświadczeń utworzonego użytkownika SSH
2. Dodaj użytkowników systemu Linux w węźle krawędzi
3. Przy pomocy utworzonego użytkownika możesz korzystać z programu RStudio Community

### <a name="step-1-use-the-created-ssh-user-to-log-in-to-the-edge-node"></a>Krok 1. Logowanie do węzła krawędzi przy użyciu poświadczeń utworzonego użytkownika SSH

Pobierz dowolne narzędzie SSH (takie jak Putty) i zaloguj się za pomocą istniejącego konta użytkownika SSH. Aby uzyskać dostęp do węzła krawędzi, postępuj zgodnie z instrukcjami podanymi w temacie [Łączenie się z usługą HDInsight (Hadoop) przy użyciu protokołu SSH](hdinsight-hadoop-linux-use-ssh-unix.md). Adres węzła krawędzi serwera R Server w klastrze usługi HDInsight to: *nazwa_klastra-ed-ssh.azurehdinsight.net*


### <a name="step-2-add-more-linux-users-in-edge-node"></a>Krok 2. Dodawanie użytkowników systemu Linux w węźle krawędzi

Aby dodać użytkownika do węzła krawędzi, uruchom te polecenia:

    sudo useradd yournewusername -m
    sudo passwd yourusername

Powinny zostać zwrócone następujące elementy: 

![Równoczesny użytkownik 3](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-3.png)

Gdy pojawi się monit o podanie bieżącego hasła protokołu Kerberos, po prostu go zignoruj, naciskając klawisz **Enter**. Podanie opcji `-m` w poleceniu `useradd` powoduje, że system utworzy folder macierzysty użytkownika, wymagany przez program RStudio Community.


### <a name="step-3-use-rstudio-community-version-with-the-user-created"></a>Krok 3. Korzystanie z programu RStudio Community przy pomocy utworzonego użytkownika

Zaloguj się do programu RStudio przy użyciu utworzonego konta użytkownika:

![Równoczesny użytkownik 4](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-4.png)

Zwróć uwagę na to, że program RStudio wyświetla informację, że do logowania się w klastrze jest używane nowe konto użytkownika (w tym przypadku *sshuser6*): 

![Równoczesny użytkownik 5](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-5.png)

Jednocześnie w innym oknie przeglądarki możesz także zalogować się przy użyciu oryginalnych poświadczeń (domyślnie: *sshuser*).

Zadania można przesyłać za pomocą funkcji programu ScaleR. Oto przykładowe polecenia służące do uruchamiania zadania:

    # Set the HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data to the tmp folder.
    remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
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

    # Set directory in bigDataDirRoot to load the data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create the directory.
    rxHadoopMakeDir(inputDir)

    # Copy the data from source to input.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define the HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for the airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all the column names.
    varNames <- names(airlineColInfo)

    # Define the text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define the text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify the formula to use.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define the Spark compute context.
    mySparkCluster <- RxSpark()

    # Set the compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)


Zwróć uwagę, że przesłane zadania mają inne nazwy użytkowników w interfejsie użytkownika YARN:

![Równoczesny użytkownik 6](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-6.png)

Pamiętaj, że nowo dodani użytkownicy nie mają uprawnień użytkownika root w systemie Linux, ale mają takie same prawa dostępu do wszystkich plików w magazynie zdalnym HDFS i WASB.


<a name="use-r-console"></a>
## <a name="use-the-r-console"></a>Użycie konsoli R

1. W sesji SSH wpisz następujące polecenie, aby uruchomić konsolę R:  

        R

2. Powinny zostać wyświetlone dane wyjściowe podobne do następujących:
    
    R version 3.2.2 (2015-08-14) — "Fire Safety"  Copyright (C) 2015 The R Foundation for Statistical Computing  Platform: x86_64-pc-linux-gnu (64-bit)

    Bezpłatne oprogramowanie R jest dostarczane BEZ ŻADNEJ GWARANCJI.
    Zachęcamy do jego rozpowszechniania pod pewnymi warunkami.
    Wpisz „license()” lub „licence()”, aby uzyskać szczegółowe informacje o dystrybucji.

    Oprogramowanie obsługuje język naturalny, ale z angielskimi ustawieniami regionalnymi.

    R jest projektem zbiorowym, zrzeszającym wielu uczestników.
    Aby uzyskać więcej informacji, wpisz „contributors()”. Aby wyświetlić wskazówki dotyczące wskazywania elementów lub pakietów platformy R, wpisz „citation()”.

    Za pomocą polecenia „demo()” można wyświetlić prezentacje. Polecenie „help()” umożliwia uzyskanie pomocy online. Aby wyświetlić pomoc w przeglądarce z interfejsem HTML, wpisz „help.start()”.
    Aby zamknąć środowisko R, wpisz „q()”.

    Oprogramowanie Microsoft R Server w wersji 8.0: rozszerzona dystrybucja pakietów R firmy Microsoft. Copyright (C) 2016 Microsoft Corporation

    Aby uzyskać informacje o wersji, wpisz „readme()”.
    >

3. W monicie `>` możesz podać kod R. Oprogramowanie R Server zawiera pakiety, które umożliwiają łatwą współpracę z usługą Hadoop i uruchamianie rozproszonych obliczeń. Na przykład następujące polecenie umożliwia wyświetlenie katalogu głównego domyślnego systemu plików klastra usługi HDInsight:

    rxHadoopListFiles("/")

4. Dostępne jest także adresowanie w stylu WASB.

    rxHadoopListFiles("wasb:///")


## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>Używanie oprogramowania R Server w usłudze HDI ze zdalnego wystąpienia oprogramowania Microsoft R Server lub programu Microsoft R Client

Możliwe jest skonfigurowanie dostępu do kontekstu obliczeniowego aparatu Spark usługi Hadoop w usłudze HDI ze zdalnego wystąpienia programu Microsoft R Server lub programu Microsoft R Client uruchomionego na komputerze stacjonarnym lub przenośnym. Zobacz podsekcję **Using Microsoft R Server as a Hadoop Client (Używanie oprogramowania Microsoft R Server jako klienta usługi Hadoop)** tematu [Create a Compute Context for Spark (Tworzenie kontekstu obliczeniowego dla aparatu Spark)](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md). W tym celu należy określić następujące opcje podczas definiowania kontekstu obliczeniowego programu RxSpark na komputerze przenośnym: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches i sshProfileScript. Na przykład:


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

Kontekst obliczeniowy pozwala określić, czy obliczenia są wykonywane lokalnie w węźle krawędzi czy są rozproszone w węzłach klastra usługi HDInsight.

1. W programie RStudio Server lub konsoli R (w ramach sesji SSH) załaduj przykładowe dane do domyślnego magazynu usługi HDInsight za pomocą następującego kodu:

        # Set the HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data to the tmp folder
        remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
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

        # Set directory in bigDataDirRoot to load the data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make the directory
        rxHadoopMakeDir(inputDir)

        # Copy the data from source to input
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. Następnie utwórzmy trochę informacji o danych i zdefiniujmy dwa źródła danych, aby umożliwić pracę z danymi.

        # Define the HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create info list for the airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # get all the column names
        varNames <- names(airlineColInfo)

        # Define the text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define the text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula to use
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. Przeprowadźmy regresję logistyczną na danych za pomocą lokalnego kontekstu obliczeniowego.

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

4. Następnie przeprowadźmy tę samą regresję logistyczną za pomocą kontekstu aparatu Spark. Dzięki kontekstowi aparatu Spark przetwarzanie jest dystrybuowane do wszystkich węzłów procesu roboczego w klastrze usługi HDInsight.

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
   > Możesz także użyć funkcji MapReduce do rozproszenia obliczeń na węzłach klastra. Aby uzyskać więcej informacji na temat kontekstu obliczeniowego, zobacz [Compute context options for R Server on HDInsight](hdinsight-hadoop-r-server-compute-contexts.md) (Opcje kontekstu obliczeniowego dla oprogramowania R Server w usłudze HDInsight).


## <a name="distribute-r-code-to-multiple-nodes"></a>Dystrybucja kodu R do wielu węzłów

Przy użyciu oprogramowania R Server możesz w łatwy sposób uruchomić istniejący kod R w wielu węzłach klastra za pomocą programu `rxExec`. Funkcja ta jest przydatna podczas czyszczenia parametrów lub przeprowadzania symulacji. Poniższy kod przedstawia przykładowe użycie programu `rxExec`:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Jeśli nadal używasz kontekstu Spark lub MapReduce, uruchomienie tego polecenia spowoduje zwrócenie wartości nodename dla węzłów procesu roboczego, w których uruchomiono kod `(Sys.info()["nodename"])`. Na przykład w przypadku klastra składającego się z czterech węzłów dane wyjściowe mogą być podobne do następujących:

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


## <a name="accessing-data-in-hive-and-parquet"></a>Dostęp do danych w usługach Hive i Parquet

Funkcja dostępna w oprogramowaniu R Server 9.1 umożliwia bezpośredni dostęp do danych w usługach Hive i Parquet w celu użycia ich w funkcjach programu ScaleR w kontekście obliczeniowym aparatu Spark. Te możliwości są dostępne za pomocą nowych funkcji źródła danych programu ScaleR o nazwie RxHiveData i RxParquetData, które używają kodu Spark SQL do ładowania danych bezpośrednio do elementów DataFrame aparatu Spark na potrzeby analizy przez program ScaleR.  

Poniżej przedstawiono przykładowy kod korzystający z nowych funkcji:

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
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

    #Check on Spark data objects, cleanup, and close the Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


Dodatkowe informacje na temat używania tych nowych funkcji zawiera pomoc online oprogramowania R Server dostępna przy użyciu poleceń `?RxHivedata` i `?RxParquetData`.  


## <a name="install-additional-r-packages-on-the-edge-node"></a>Instalowanie dodatkowych pakietów R w węźle krawędzi

Jeśli chcesz zainstalować dodatkowe pakiety R na węźle krawędzi, możesz użyć polecenia `install.packages()` bezpośrednio z konsoli R, gdy masz połączenie SSH z węzłem krawędzi. Jednak jeśli potrzebujesz zainstalować pakiety R na węzłach procesu roboczego klastra, musisz użyć akcji skryptu.

Akcje skryptu to skrypty powłoki Bash używane do wprowadzania zmian w konfiguracji klastra usługi HDInsight lub instalowania dodatkowego oprogramowania, np. pakietów R. Aby zainstalować dodatkowe pakiety przy użyciu akcji skryptu, wykonaj następujące kroki:

> [!IMPORTANT]
> Dodatkowe pakiety R można zainstalować przy użyciu akcji skryptu dopiero po utworzeniu klastra. Nie wykonuj tej procedury podczas tworzenia klastra, ponieważ skrypt wymaga w pełni zainstalowanego i skonfigurowanego oprogramowania R Server.
>
>

1. W witrynie [Azure Portal](https://portal.azure.com) wybierz oprogramowanie R Server lub klaster usługi HDInsight.

2. W bloku **Ustawienia** wybierz pozycję **Akcje skryptu**, a następnie pozycję **Prześlij nową**, aby przesłać nową akcję skryptu.

   ![Obraz bloku akcji skryptu](./media/hdinsight-hadoop-r-server-get-started/scriptaction.png)

3. W bloku **Prześlij akcję skryptu** podaj następujące informacje:

   * **Nazwa**: przyjazna nazwa identyfikująca skrypt

   * **Identyfikator URI skryptu powłoki systemowej**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Główny**: to pole powinno być **niezaznaczone**

   * **Proces roboczy**: to pole powinno być **zaznaczone**

   * **Węzły krawędzi**: to pole powinno być **niezaznaczone**

   * **Dozorca**: to pole powinno być **niezaznaczone**

   * **Parametry**: pakiety R do zainstalowania. Na przykład: `bitops stringr arules`

   * **Utrwal ten skrypt...**: to pole powinno być **zaznaczone**  

   > [!NOTE]
   > 1. Domyślnie wszystkie pakiety R są instalowane z migawki repozytorium Microsoft MRAN odpowiedniej do zainstalowanej wersji oprogramowania R Server. Jeśli chcesz zainstalować nowsze wersje pakietów, musisz uwzględnić pewne ryzyko niezgodności. Jednak możesz to zrobić za pomocą parametru `useCRAN` użytego jako pierwszy element listy pakietów, na przykład `useCRAN bitops, stringr, arules`.  
   > 2. Niektóre pakiety R wymagają dodatkowych bibliotek systemu Linux. Dla Twojej wygody zainstalowaliśmy wstępnie wymagania dla 100 najpopularniejszych pakietów R. Jednak jeśli instalowane pakiety R wymagają jeszcze innych bibliotek, musisz pobrać skrypt podstawowy użyty tutaj i dodać kroki instalowania bibliotek systemowych. Następnie musisz przekazać zmodyfikowany skrypt do publicznego kontenera obiektów blob w usłudze Azure Storage i użyć zmodyfikowanego skryptu do zainstalowania pakietów.
   >    Aby uzyskać informacje na temat tworzenia akcji skryptu, zobacz [Script Action development](hdinsight-hadoop-script-actions-linux.md) (Tworzenie akcji skryptu).  
   >
   >

   ![Dodawanie akcji skryptu](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. Wybierz polecenie **Utwórz**, aby uruchomić skrypt. Po zakończeniu działania skryptu pakiety R będą dostępne we wszystkich węzłach procesu roboczego.


## <a name="using-microsoft-r-server-operationalization"></a>Używanie funkcji opernacjonalizacji oprogramowania Microsoft R Server

Po zakończeniu modelowania danych możesz zopernacjonalizować model, aby wykonywać prognozowanie. Aby skonfigurować funkcję operacjonalizacji oprogramowania Microsoft R Server, wykonaj poniższe kroki:

Nawiąż połączenie SSH z węzłem krawędzi. Na przykład: 

    ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Po nawiązaniu połączenia SSH zmień katalog dla odpowiedniej wersji i użyj polecenia sudo dla biblioteki dotnet dll: 

- W przypadku oprogramowania Microsoft R Server 9.1:

    cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

- W przypadku oprogramowania Microsoft R Server 9.0:

    cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

Aby skonfigurować operacjonalizację oprogramowania Microsoft R Server pod kątem jednej maszyny, wykonaj następujące kroki:

1. Wybierz pozycję „Configure R Server for Operationalization” (Konfiguruj oprogramowanie R Server pod kątem operacjonalizacji)
2. Wybierz pozycję „A. One-box (web + compute nodes)” (Jedna maszyna — sieć Web i węzły obliczeniowe)
3. Podaj hasło dla **administratora**

![opernacjonalizacja przy użyciu jednej maszyny](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

Opcjonalnie możesz wykonać kontrolę diagnostyczną, uruchamiając test diagnostyczny w sposób pokazany poniżej:

1. Wybierz pozycję „6. Run diagnostic tests” (Uruchom testy diagnostyczne)
2. Wybierz pozycję „A. Test configuration” (Testuj konfigurację)
3. Wpisz ciąg Username = “admin” i hasło określone w poprzednim kroku konfiguracji
4. Potwierdź wynik: Overall Health = pass (Ogólna kondycja — dobra)
5. Zamknij narzędzie administracyjne
6. Zamknij połączenie SSH

![Diagnostyka opernacjonalizacji](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)

Na tym etapie konfiguracja opernacjonalizacji jest ukończona. Teraz możesz użyć pakietu „mrsdeploy” w programie RClient, aby nawiązać połączenie z operacjonalizacją w węźle krawędzi i rozpocząć korzystanie z jej funkcji, takich jak [zdalne wykonywanie](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) i [usługi internetowe](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette). W zależności od tego, czy klaster został skonfigurowany w sieci wirtualnej, może być konieczne skonfigurowanie tunelowania przekierowania portów za pomocą logowania SSH. W poniższych sekcjach wyjaśniono, jak skonfigurować taki tunel.

### <a name="rserver-cluster-on-virtual-network"></a>Klaster oprogramowania RServer w sieci wirtualnej

Sprawdź, czy ruch przez port 12800 węzła krawędzi jest dozwolony. Pozwala to użyć węzła krawędzi do nawiązania połączenia z funkcją operacjonalizacji.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Jeśli metoda `remoteLogin()` nie może połączyć się z węzłem krawędzi, lecz nawiązanie połączenia SSH z węzłem krawędzi jest możliwe, sprawdź, czy reguła zezwalająca na ruch przez port 12800 jest skonfigurowana poprawnie. Jeśli problem nie ustąpi, możesz go obejść, konfigurując tunelowanie przekierowania portów przez połączenie SSH. Odpowiednie instrukcje znajdują się w następującej sekcji.

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a>Klaster oprogramowania RServer w sieci niewirtualnej

Jeśli klaster nie jest skonfigurowany w sieci wirtualnej lub występują problemy z korzystaniem z sieci wirtualnej, możesz użyć tunelowania przekierowania portów za pomocą protokołu SSH:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

W programie Putty także możesz je skonfigurować.

![połączenie ssh w programie putty](./media/hdinsight-hadoop-r-server-get-started/putty.png)

Gdy sesja SSH jest aktywna, ruch z portu 12800 maszyny jest przekazywany do portu 12800 węzła krawędzi za pomocą sesji SSH. Upewnij się, że w metodzie `remoteLogin()` użyto adresu `127.0.0.1:12800`. Umożliwi to logowanie do funkcji operacjonalizacji węzła krawędzi przez przekierowanie portów.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="how-to-scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Jak skalować węzły obliczeniowe operacjonalizacji oprogramowania Microsoft R Server na węzłach procesu roboczego usługi HDInsight

### <a name="decommission-the-worker-nodes"></a>Likwidowanie węzłów procesu roboczego

Oprogramowanie Microsoft R Server nie jest aktualnie zarządzane za pomocą usługi Yarn. Jeśli węzły procesu roboczego nie zostaną zlikwidowane, menedżer zasobów usługi Yarn nie będzie działać w oczekiwany sposób, ponieważ nie będzie znał zasobów zajętych przez serwer. Aby tego uniknąć, zalecamy zlikwidowanie węzłów procesu roboczego przed przystąpieniem do skalowania węzłów obliczeniowych na zewnątrz.

Kroki likwidowania węzłów procesu roboczego:

* Zaloguj się do konsoli Ambari klastra usługi HDI i kliknij kartę „hosts” (Hosty)
* Wybierz węzły procesu roboczego do zlikwidowania. Kliknij pozycje „Actions” > „Selected Hosts” > „Hosts” > „Turn ON Maintenance Mode” (Akcje > Wybrane hosty > Hosty > Włącz tryb konserwacji). Na przykład na poniższej ilustracji węzły wn3 i wn4 są przeznaczone do likwidacji.  

   ![likwidowanie węzłów procesu roboczego](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* Wybierz pozycję **Actions** > **Selected Hosts** > **DataNodes** (Akcje > Wybrane hosty > Węzły danych) i kliknij pozycję **Decommission** (Likwiduj)
* Wybierz pozycję **Actions** > **Selected Hosts** > **NodeManagers** (Akcje > Wybrane hosty > Menedżerowie węzła) i kliknij pozycję **Decommission** (Likwiduj)
* Wybierz pozycję **Actions** > **Selected Hosts** > **DataNodes** (Akcje > Wybrane hosty > Węzły danych) i kliknij pozycję **Stop** (Zatrzymaj)
* Wybierz pozycję **Actions** > **Selected Hosts** > **NodeManagers** (Akcje > Wybrane hosty > Menedżerowie węzła) i kliknij pozycję **Stop** (Zatrzymaj)
* Wybierz pozycję **Actions** > **Selected Hosts** > **Hosts** (Akcje > Wybrane hosty > Hosty) i kliknij pozycję **Stop All Components** (Zatrzymaj wszystkie składniki)
* Usuń zaznaczenie węzłów procesu roboczego i wybierz węzły główne
* Wybierz pozycję **Actions** > **Selected Hosts** > **Hosts** > **Restart All Components** (Akcje > Wybrane hosty > Hosty > Uruchom ponownie wszystkie składniki)

### <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Konfigurowanie węzłów obliczeniowych na wszystkich zlikwidowanych węzłach procesu roboczego

1. Za pomocą protokołu SSH połącz się z każdym zlikwidowanym węzłem procesu roboczego.
2. Uruchom narzędzie administracyjne za pomocą polecenia `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.
3. Wpisz „1”, aby wybrać opcję „Configure R Server for Operationalization” (Konfiguruj oprogramowanie R Server pod kątem operacjonalizacji).
4. Wpisz „c”, aby wybrać opcję „C. Compute node” (Węzeł obliczeniowy). Umożliwi to skonfigurowanie węzła obliczeniowego w węźle procesu roboczego.
5. Zamknij narzędzie administracyjne.

### <a name="add-compute-nodes-details-on-web-node"></a>Dodawanie szczegółów węzłów obliczeniowych na węźle sieci Web

Po skonfigurowaniu wszystkich zlikwidowanych węzłów procesu roboczego pod kątem uruchamiania węzła obliczeniowego wróć do węzła krawędzi i dodaj adresy IP zlikwidowanych węzłów procesu roboczego do konfiguracji węzła internetowego oprogramowania Microsoft R Server:

* Połącz się z węzłem krawędzi za pomocą protokołu SSH.
* Uruchom polecenie `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.
* W sekcji „URIs” (Identyfikatory URI) dodaj adres IP i port węzła procesu roboczego.

    ![wiersz polecenia likwidowania węzłów procesu roboczego](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a>Rozwiązywanie problemów

W razie problemów podczas tworzenia klastrów usługi HDInsight zapoznaj się z [wymaganiami dotyczącymi kontroli dostępu](hdinsight-administer-use-portal-linux.md#create-clusters).


## <a name="next-steps"></a>Następne kroki

Wiesz już, jak utworzyć nowy klaster usługi HDInsight zawierający serwer R Server, oraz znasz podstawy używania konsoli R w sesji SSH. W poniższych tematach opisano inne sposoby korzystania z serwera R Server w usłudze HDInsight oraz zarządzania nim:

* [Add RStudio Server to HDInsight (if not installed during cluster creation)](hdinsight-hadoop-r-server-install-r-studio.md) (Dodawanie programu RStudio Server do usługi HDInsight — jeśli nie zainstalowano podczas tworzenia klastra)
* [Compute context options for R Server on HDInsight](hdinsight-hadoop-r-server-compute-contexts.md) (Opcje kontekstu obliczeniowego dla oprogramowania R Server w usłudze HDInsight)
* [Azure Storage options for R Server on HDInsight](hdinsight-hadoop-r-server-storage.md) (Opcje usługi Azure Storage dla oprogramowania R Server w usłudze HDInsight)

