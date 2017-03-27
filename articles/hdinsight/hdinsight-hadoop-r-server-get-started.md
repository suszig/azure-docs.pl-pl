---
title: "Wprowadzenie do oprogramowania R Server w usłudze HDInsight | Microsoft Docs"
description: "Dowiedz się, jak utworzyć aparat Apache Spark w klastrze usługi HDInsight zawierającym oprogramowanie R Server, a następnie prześlij skrypt R do klastra."
services: HDInsight
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/28/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 2c9877f84873c825f96b62b492f49d1733e6c64e
ms.openlocfilehash: 383a325bfd2620f6c4fd25ce2f3a66522131efef
ms.lasthandoff: 03/15/2017


---
# <a name="get-started-using-r-server-on-hdinsight"></a>Wprowadzenie do korzystania z oprogramowania R Server w usłudze HDInsight
Usługa HDInsight obejmuje opcję oprogramowania R Server, którą można zintegrować z klastrem usługi HDInsight. Dzięki temu skrypty R mogą używać aparatu Spark i funkcji MapReduce do wykonywania obliczeń rozproszonych. Ten dokument umożliwia poznanie procedury tworzenia oprogramowania R Server w klastrze usługi HDInsight, a następnie uruchamiania skryptu R, który demonstruje sposób użycia aparatu Spark na potrzeby wykonywania rozproszonych obliczeń przez kod R.

## <a name="prerequisites"></a>Wymagania wstępne
* **Subskrypcja platformy Azure**: przed rozpoczęciem tego samouczka musisz mieć subskrypcję platformy Azure. Przejdź na stronę [Get a Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (Uzyskaj bezpłatną wersję próbną platformy Azure), aby uzyskać więcej informacji.
* **Klient protokołu Secure Shell (SSH)**: klient SSH jest używany do zdalnego łączenia z klastrem usługi HDInsight i uruchamiania poleceń bezpośrednio w klastrze. Systemy Linux, Unix i OS X udostępniają klienta SSH w postaci polecenia `ssh`. W przypadku systemów Windows zalecamy użycie programu [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

  * **Klucze SSH (opcjonalnie)**: konto SSH użyte do nawiązania połączenia z klastrem można zabezpieczyć przy użyciu hasła lub klucza publicznego. Użycie hasła jest łatwiejsze i umożliwia rozpoczęcie pracy bez konieczności tworzenia pary kluczy publiczny-prywatny. Jednak użycie klucza jest bezpieczniejsze.

      W krokach przedstawionych w tym dokumencie przyjęto założenie, że jest używane hasło. Instrukcje dotyczące sposobu tworzenia i używania kluczy SSH z usługą HDInsight można znaleźć w następujących dokumentach:

    * [Use SSH with HDInsight from Linux, Unix, or OS X clients](hdinsight-hadoop-linux-use-ssh-unix.md) (Używanie połączenia SSH z usługą HDInsight z klientów z systemem Linux, Unix lub OS X)
    * [Use SSH with HDInsight from Windows clients](hdinsight-hadoop-linux-use-ssh-windows.md) (Używanie połączenia SSH z usługą HDInsight z klientów z systemem Windows)

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-the-cluster"></a>Tworzenie klastra
> [!NOTE]
> Kroki opisane w tym dokumencie przeprowadzą Cię przez proces tworzenia oprogramowania R Server w klastrze usługi HDInsight przy użyciu podstawowej konfiguracji. Inne ustawienia konfiguracji klastra (takie jak dodawanie kolejnych kont magazynu, używanie usługi Azure Virtual Network lub tworzenie magazynu metadanych programu Hive) są opisane w artykule [Create Linux-based HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md) (Tworzenie klastrów usługi HDInsight opartych na systemie Linux). Aby utworzyć serwer R Server za pomocą szablonu usługi Azure Resource Management, zobacz [Deploy an R-server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/) (Wdrażanie klastra usługi HDInsight serwera R Server).
>
> 

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Wybierz pozycję **NOWY**, **Dane i analizy**, a następnie **Usługa HDInsight**.

    ![Obraz tworzenia nowego klastra](./media/hdinsight-getting-started-with-r/newcluster.png)

3. W środowisku **Szybkie tworzenie** podaj nazwę klastra w polu **Nazwa klastra**. Jeśli masz wiele subskrypcji platformy Azure, użyj pozycji **Subskrypcja**, aby wybrać subskrypcję do użycia.

    ![Wybór nazwy klastra i subskrypcji](./media/hdinsight-getting-started-with-r/clustername.png)

4. Wybierz pozycję **Typ klastra**, aby otworzyć blok **Konfiguracja klastra**. W bloku **Konfiguracja klastra** wybierz następujące opcje:

   * **Typ klastra**: R Server
   * **Wersja**: wybierz wersję oprogramowania R Server do zainstalowania w klastrze. Wybierz najnowszą wersję, aby korzystać z najnowszych możliwości. Inne wersje są dostępne w razie potrzeby zapewnienia zgodności. Informacje o wersji dla każdej z dostępnych wersji są dostępne [tutaj](https://msdn.microsoft.com/en-us/microsoft-r/notes/r-server-notes).
   * **Program R Studio Community Edition for R Server**: to środowisko IDE oparte na przeglądarce, które jest instalowane domyślnie w węźle krawędzi.  Jeśli nie chcesz go instalować, usuń zaznaczenie pola wyboru. Jeśli wybierzesz jego instalację, adres URL umożliwiający logowanie do programu RStudio Server będzie dostępny w bloku aplikacji portalu dla klastra po jego utworzeniu.

   Pozostaw wartości domyślne innych opcji i użyj przycisku **Wybierz**, aby zapisać typ klastra.
   
   ![Zrzut ekranu bloku typu klastra](./media/hdinsight-getting-started-with-r/clustertypeconfig.png)
         
5. Podaj wartości w pozycjach **Nazwa użytkownika logowania klastra** i **Hasło logowania klastra**.

   Określ wartość w pozycji **Nazwa użytkownika SSH**.  Protokół SSH jest używany do zdalnego łączenia z klastrem przy użyciu klienta protokołu **Secure Shell (SSH)**. Użytkownika SSH można określić w tym oknie dialogowym lub po utworzeniu klastra (na karcie Konfiguracja klastra). Oprogramowanie R Server jest skonfigurowane pod kątem użycia **nazwy użytkownika SSH** „remoteuser”.  **Jeśli używasz innej nazwy użytkownika, musisz wykonać dodatkowy krok po utworzeniu klastra.**
   
   Pozostaw zaznaczone pole **Użyj tego samego hasła podczas logowania do klastra**, aby użyć typu uwierzytelniania **HASŁO**, chyba że wolisz użyć klucza publicznego.  Będzie potrzebna para kluczy publiczny-prywatny, jeśli planujesz uzyskiwanie dostępu do oprogramowania R Server w klastrze za pomocą zdalnego klienta, na przykład programu RTVS, RStudio lub innego komputerowego środowiska IDE. Należy wybrać hasło SSH w przypadku instalowania programu RStudio Server Community Edition.     
   
   Aby utworzyć parę kluczy publiczny-prywatny, usuń zaznaczenie pola **Użyj tego samego hasła podczas logowania do klastra**, a następnie wybierz typ uwierzytelniania **KLUCZ PUBLICZNY** i kontynuuj w podany poniżej sposób.  W poniższych instrukcjach przyjęto, że jest zainstalowane środowisko Cygwin z programem ssh-keygen lub równoważnym.
   
   * Wygeneruj parę kluczy publiczny-prywatny w wierszu polecenia na komputerze przenośnym:
   
   `ssh-keygen -t rsa -b 2048`

   * Podaj nazwę pliku klucza po wyświetleniu monitu, a następnie określ hasło, aby podwyższyć poziom zabezpieczeń. Ekran powinien wyglądać podobnie do następującego:

   ![Wiersz polecenia SSH w systemie Windows](./media/hdinsight-getting-started-with-r/sshcmdline.png)
   
   * W wyniku zostanie utworzony plik klucza prywatnego i plik klucza publicznego o nazwie <nazwa-pliku-klucza-prywatnego>.pub, na przykład furiosa i furiosa.pub.
   
   ![Katalog polecenia SSH](./media/hdinsight-getting-started-with-r/dir.png)

   * Następnie określ plik klucza publicznego (*pub) podczas przypisywania poświadczeń klastra usługi HDI oraz potwierdź grupę zasobów i region, po czym wybierz pozycję**Dalej**
   
   ![Blok poświadczeń](./media/hdinsight-getting-started-with-r/publickeyfile.png)  
   
   * Zmienianie uprawnień pliku klucza prywatnego na komputerze przenośnym
   
   `chmod 600 <private-key-filename>`
   
   * Użycie pliku klucza prywatnego do zdalnego logowania za pomocą protokołu SSH
   
   `ssh –i <private-key-filename> remoteuser@<hostname public ip>`
   
   lub jako części definicji kontekstu obliczeniowego aparatu Spark usługi Hadoop dla oprogramowania R Server na kliencie — zobacz temat Using Microsoft R Server as a Hadoop Client (Używanie oprogramowania Microsoft R Server jako klienta usługi Hadoop) w sekcji [Creating a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) (Tworzenie kontekstu obliczeniowego dla aparatu Spark) dokumentu online [Get started with ScaleR on Apache Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started) (Wprowadzenie do programu ScaleR w aparacie Apache Spark).
   
6. Funkcja szybkiego tworzenia przeniesie Cię do bloku **Magazyn**, aby umożliwić wybranie ustawień konta magazynu do użycia dla lokalizacji głównej systemu plików HDFS używanego przez klaster. Wybierz nowe lub istniejące konto usługi Azure Storage lub istniejące konto usługi Data Lake Storage.

   1. Jeśli wybierzesz konto usługi Azure Storage, możesz określić istniejące konto magazynu, wybierając pozycję **Wybierz konto magazynu**, a następnie wybierając konto. Możesz też utworzyć nowe konto przy użyciu linku **Utwórz nowe** w sekcji **Wybierz konto magazynu**.

      > [!NOTE]
      > Jeśli wybierzesz pozycję **Nowe**, musisz podać nazwę nowego konta magazynu. Jeśli nazwa zostanie zaakceptowana, pojawi się zielony znacznik.

      Domyślną wartością pola **Kontener domyślny** jest nazwa klastra. Pozostaw tę wartość.

      Jeśli wybrano opcję nowego konta magazynu, zostanie wyświetlony monit o określenie wartości **Lokalizacja**, która definiuje region, w którym ma zostać utworzone konto magazynu.  
   
         ![Blok źródła danych](./media/hdinsight-getting-started-with-r/datastore.png)  
   
      > [!IMPORTANT]
      > Wybranie lokalizacji dla domyślnego źródła danych spowoduje także ustawienie lokalizacji klastra usługi HDInsight. Klaster i domyślne źródło danych muszą znajdować się w tym samym regionie.

   2. Jeśli wybierzesz użycie istniejącej usługi Data Lake Store, wybierz następnie konto magazynu usługi ADLS do użycia i dodaj tożsamość usługi ADD klastra do klastra, aby umożliwić dostęp do magazynu. Aby uzyskać więcej informacji na temat tego procesu, zobacz [Tworzenie klastra HDInsight z usługą Data Lake Store za pomocą witryny Azure Portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).

   Użyj przycisku **Wybierz**, aby zapisać konfigurację źródła danych.


7. Zostanie wyświetlony blok **Podsumowanie**, aby umożliwić zweryfikowanie wszystkich ustawień. W tym miejscu możesz zmienić wartość pozycji **Rozmiar klastra** w celu zmodyfikowania liczby serwerów w klastrze, a także określić wartość pozycji **Akcje skryptu** definiującej skrypty do uruchomienia. Pozostaw domyślną liczbę węzłów procesu roboczego — `4`, chyba że wiesz, że potrzebujesz większego klastra. Szacowany koszt klastra zostanie pokazany w bloku.
   
   ![podsumowanie klastra](./media/hdinsight-getting-started-with-r/clustersummary.png)

   > [!NOTE]
   > W razie potrzeby możesz później zmienić ponownie rozmiar klastra w portalu (Klaster -> Ustawienia -> Skaluj klaster), aby zwiększyć lub zmniejszyć liczbę węzłów procesu roboczego.  Działanie takie może być przydatne do zmniejszenia klastra, gdy nie jest używany, lub dodania pojemności w celu spełnienia wymagań większych zadań.
   >
   > 

    Podczas zmiany rozmiaru klastra, węzłów danych i węzła krawędzi należy uwzględnić pewne czynniki:  

   * Wydajność rozproszonych analiz wykonywanych za pomocą aparatu Spark w oprogramowaniu R Server jest proporcjonalna do liczby węzłów procesu roboczego, jeśli danych jest dużo.  

   * Wydajność analiz oprogramowania R Server jest liniowa w stosunku do rozmiaru analizowanych danych. Na przykład:  

     * W przypadku małej do średniej ilości danych wydajność jest najwyższa podczas analizowania w lokalnym kontekście obliczeniowym na węźle krawędzi.  Więcej informacji na temat scenariuszy, w których konteksty obliczeniowe aparatu Spark i lokalne działają najlepiej, zobacz Compute context options for R Server on HDInsight (Opcje kontekstu obliczeniowego dla oprogramowania R Server w usłudze HDInsight).<br>
     * Jeśli zalogujesz się do węzła krawędzi i uruchomisz skrypt R, wszystkie funkcje poza funkcjami rx programu ScaleR zostaną uruchomione <strong>lokalnie</strong> na węźle krawędzi, dlatego wielkość pamięci i liczba rdzeni węzła krawędzi powinny być odpowiednie. To samo dotyczy sytuacji, w której oprogramowanie R Server w usłudze HDI jest używane z komputera przenośnego jako zdalny kontekst obliczeniowy.

     ![Blok warstw cenowych węzła](./media/hdinsight-getting-started-with-r/pricingtier.png)

     Użyj przycisku **Wybierz**, aby zapisać konfigurację cen węzła.

   Zwróć uwagę na link **Pobierz szablon i parametry**. Kliknięcie tego linku spowoduje wyświetlenie skryptów, których można użyć do automatyzacji tworzenia klastra z wybraną konfiguracją. Te skrypty są także dostępne z pozycji Azure Portal dla klastra po jego utworzeniu.

   > [!NOTE]
   > Tworzenie klastra zajmuje trochę czasu, zwykle około 20 minut. Użyj kafelka na tablicy startowej lub pozycji **Powiadomienia** w lewej części strony, aby sprawdzić postęp procesu tworzenia.
   >
   > 

## <a name="connect-to-rstudio-server"></a>Łączenie z programem RStudio Server

Jeśli wybrano dołączenie programu RStudio Server Community Edition do instalacji, można się do niego zalogować na dwa sposoby.

1. Przechodząc do następującego adresu URL (gdzie **NAZWA-KLASTRA** to nazwa utworzonego klastra): 

    https://**NAZWA-KLASTRA**.azurehdinsight.net/rstudio/

2. Otwierając pozycję klastra w witrynie Azure Portal, wybierając szybki link Pulpity nawigacyjne oprogramowania R Server, a następnie wybierając pulpit nawigacyjny programu R Studio:

     ![Dostęp do pulpitu nawigacyjnego programu R Studio](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)
     
     ![Dostęp do pulpitu nawigacyjnego programu R Studio](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > Niezależnie od metody, pierwsze logowanie wymaga dwukrotnego uwierzytelnienia.  Podczas pierwszego uwierzytelniania podaj identyfikator użytkownika i hasło administratora klastra. Przy drugim monicie podaj identyfikator użytkownika i hasło połączenia SSH. Podczas kolejnych logowań będzie wymagany tylko identyfikator użytkownika i hasło połączenia SSH. 

## <a name="connect-to-the-r-server-edge-node"></a>Łączenie z węzłem krawędzi oprogramowania R Server
Połącz się z węzłem krawędzi oprogramowania R Server klastra usługi HDInsight za pomocą protokołu SSH:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> Adres `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` jest także dostępny w witrynie Azure Portal po wybraniu klastra, a następnie pozycji **Wszystkie ustawienia**, **Aplikacje** i **Program RServer**. Spowoduje to wyświetlenie informacji o punkcie końcowym SSH dla węzła krawędzi.
>
> ![Obraz punktu końcowego SSH dla węzła krawędzi](./media/hdinsight-getting-started-with-r/sshendpoint.png)
>
> 

Jeśli do zabezpieczenia konta użytkownika SSH użyto hasła, zostanie wyświetlony monit o jego wprowadzenie. Jeśli używasz klucza publicznego, może być konieczne użycie parametru `-i` w celu określenia zgodnego klucza prywatnego. Na przykład `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`.

Aby uzyskać więcej informacji o korzystaniu z protokołu SSH w usłudze HDInsight w systemie Linux, zobacz jeden z następujących artykułów:

* [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

Po nawiązaniu połączenia zostanie wyświetlony monit podobny do następującego.

`username@ed00-myrser:~$`

## <a name="use-the-r-console"></a>Użycie konsoli R

1. W sesji SSH wpisz następujące polecenie, aby uruchomić konsolę R.  
   
   ```
   R

   You will see output similar to the following.
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
   ```
   
2. W monicie `>` możesz podać kod R. Oprogramowanie R Server zawiera pakiety, które umożliwiają łatwą współpracę z usługą Hadoop i uruchamianie rozproszonych obliczeń. Na przykład następujące polecenie umożliwia wyświetlenie katalogu głównego domyślnego systemu plików klastra usługi HDInsight.

`rxHadoopListFiles("/")`

Dostępne jest także adresowanie w stylu WASB.

`rxHadoopListFiles("wasbs:///")`

## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>Używanie oprogramowania R Server w usłudze HDI ze zdalnego wystąpienia oprogramowania Microsoft R Server lub programu Microsoft R Client
Według informacji w powyższej sekcji dotyczących użycia pary kluczy publiczny-prywatny w celu uzyskania dostępu do klastra jest możliwe skonfigurowanie dostępu do kontekstu obliczeniowego aparatu Spark usługi Hadoop w usłudze HDI ze zdalnego wystąpienia oprogramowania Microsoft R Server lub programu Microsoft R Client uruchomionego na komputerze stacjonarnym lub przenośnym — zobacz temat Using Microsoft R Server as a Hadoop Client (Używanie oprogramowania Microsoft R Server jako klienta usługi Hadoop) w sekcji [Creating a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) (Tworzenie kontekstu obliczeniowego dla aparatu Spark) przewodnika online [RevoScaleR Hadoop Spark Getting Started](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started) (Wprowadzenie do programu RevoScaleR w ramach aparatu Spark w usłudze Hadoop).  W tym celu należy określić następujące opcje podczas definiowania kontekstu obliczeniowego programu RxSpark na komputerze przenośnym: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches i sshProfileScript. Na przykład:

```
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
```


## <a name="use-a-compute-context"></a>Używanie kontekstu obliczeniowego
Kontekst obliczeniowy pozwala na kontrolowanie, czy obliczenia będą wykonywane lokalnie, na węźle krawędzi lub czy będą rozproszone na węzłach klastra usługi HDInsight.

1. W programie RStudio Server lub w konsoli R (w ramach sesji SSH) użyj następującej procedury, aby załadować przykładowe dane do domyślnego magazynu usługi HDInsight.
   
   ```
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
   ```
   
2. Następnie utwórzmy trochę informacji o danych i zdefiniujmy dwa źródła danych, aby umożliwić pracę z danymi.
   
   ```
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
   ```
   
3. Przeprowadźmy regresję logistyczną na danych za pomocą lokalnego kontekstu obliczeniowego.
   
   ```
   # Set a local compute context
   rxSetComputeContext("local")
   # Run a logistic regression
   system.time(
       modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
   )
   # Display a summary 
   summary(modelLocal)
   ```
   
   Powinny zostać wyświetlone dane wyjściowe kończące się wierszami podobnymi do następujących.
   
   ```
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
   ```
   
4. Następnie przeprowadźmy tę samą regresję logistyczną za pomocą kontekstu aparatu Spark. Kontekst aparatu Spark będzie dystrybuować przetwarzanie do wszystkich węzłów procesu roboczego w klastrze usługi HDInsight.
   
   ```
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
   ```

   
   > [!NOTE]
   > Możesz także użyć funkcji MapReduce do rozproszenia obliczeń na węzłach klastra. Aby uzyskać więcej informacji na temat kontekstu obliczeniowego, zobacz [Compute context options for R Server on HDInsight](hdinsight-hadoop-r-server-compute-contexts.md) (Opcje kontekstu obliczeniowego dla oprogramowania R Server w usłudze HDInsight).
   >
   >
   
## <a name="distribute-r-code-to-multiple-nodes"></a>Dystrybucja kodu R do wielu węzłów
Za pomocą oprogramowania R Server możesz w łatwy sposób uruchomić istniejący kod R na wielu węzłach klastra za pomocą programu `rxExec`. Jest to przydatne podczas czyszczenia parametrów lub symulacji. Poniżej znajduje się przykład sposobu użycia programu `rxExec`.

`rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )`

Jeśli nadal używasz kontekstu Spark lub MapReduce, zostanie zwrócona wartość nodename węzłów procesu roboczego, na których uruchomiono kod `(Sys.info()["nodename"])`. Na przykład w klastrze składającym się z czterech węzłów dane wyjściowe mogą być podobne do następujących.


```
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
```

## <a name="accessing-data-in-hive-and-parquet"></a>Dostęp do danych w usługach Hive i Parquet
Nowa funkcja dostępna w oprogramowaniu R Server 9.0 i nowszym umożliwia bezpośredni dostęp do danych za pomocą technologii Hive i Parquet w celu użycia ich w funkcjach programu ScaleR w kontekście obliczeniowym aparatu Spark. Te możliwości są dostępne za pomocą nowych funkcji źródła danych programu ScaleR o nazwie RxHiveData i RxParquetData, które używają kodu Spark SQL do ładowania danych bezpośrednio do elementów DataFrame aparatu Spark na potrzeby analizy przez program ScaleR.  

Poniżej przedstawiono przykładowy kod korzystający z nowych funkcji: 



```
#..create a Spark compute context

myHadoopCluster <- rxSparkConnect(reset = TRUE)
```


```
#..retrieve some sample data from Hive and run a model 

hiveData <- RxHiveData("select * from hivesampletable", 
                 colInfo = list(devicemake = list(type = "factor")))
rxGetInfo(hiveData, getVarInfo = TRUE)

rxLinMod(querydwelltime ~ devicemake, data=hiveData)
```

```
#..retrieve some sample data from Parquet and run a model 

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
```


``` 
#..check on Spark data objects, cleanup, and close the Spark session 

lsObj <- rxSparkListData() # two data objs are cached
lsObj
rxSparkRemoveData(lsObj)
rxSparkListData() # it should show empty list
rxSparkDisconnect(myHadoopCluster)
```

Dodatkowe informacje na temat tych nowych funkcji zawiera pomoc online oprogramowania R Server dostępna przy użyciu poleceń ?RxHivedata i ?RxParquetData.  


## <a name="install-r-packages"></a>Instalowanie pakietów R
Jeśli chcesz zainstalować dodatkowe pakiety R na węźle krawędzi, możesz użyć polecenia `install.packages()` bezpośrednio z konsoli R, gdy masz połączenie SSH z węzłem krawędzi. Jednak jeśli potrzebujesz zainstalować pakiety R na węzłach procesu roboczego klastra, musisz użyć akcji skryptu.

Akcje skryptu są to skrypty powłoki Bash używane do wprowadzania zmian w konfiguracji klastra usługi HDInsight lub instalowania dodatkowego oprogramowania. W tym przypadku do instalowania dodatkowych pakietów R. Aby zainstalować dodatkowe pakiety przy użyciu akcji skryptu, wykonaj następujące kroki.

> [!IMPORTANT]
> Dodatkowe pakiety R można zainstalować przy użyciu akcji skryptu dopiero po utworzeniu klastra. Nie można ich używać podczas tworzenia klastra, ponieważ skrypt zależy od całkowicie zainstalowanego i skonfigurowanego oprogramowania R Server.
>
> 

1. W witrynie [Azure Portal](https://portal.azure.com) wybierz oprogramowanie R Server lub klaster usługi HDInsight.
   
2. W bloku **Ustawienia** wybierz pozycję **Akcje skryptu**, a następnie pozycję **Prześlij nową**, aby przesłać nową akcję skryptu.
   
   ![Obraz bloku akcji skryptu](./media/hdinsight-getting-started-with-r/scriptaction.png)
   
3. W bloku **Prześlij akcję skryptu** podaj następujące informacje.
   
   * **Nazwa**: przyjazna nazwa identyfikująca skrypt
   
   * **Identyfikator URI skryptu powłoki systemowej**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`
   
   * **Główny**: to pole powinno być **niezaznaczone**
   
   * **Proces roboczy**: to pole powinno być **niezaznaczone**
   
   * **Węzły krawędzi**: to pole powinno być **niezaznaczone**.
   
   * **Dozorca**: to pole powinno być **niezaznaczone**
   
   * **Parametry**: pakiety R do zainstalowania. Na przykład: `bitops stringr arules`
   
   * **Utrwal ten skrypt...**: to pole powinno być **zaznaczone**  
   
   > [!NOTE]
   > 1. Domyślnie wszystkie pakiety R są instalowane z migawki repozytorium Microsoft MRAN odpowiedniej do zainstalowanej wersji oprogramowania R Server.  Jeśli chcesz zainstalować nowsze wersje pakietów, musisz uwzględnić pewne ryzyko niezgodności, jednak jest to możliwe za pomocą elementu `useCRAN` użytego jako pierwszy element listy pakietów, na przykład `useCRAN bitops, stringr, arules`.  
   > 2. Niektóre pakiety R będą wymagać dodatkowych bibliotek systemu Linux. Dla Twojej wygody zainstalowaliśmy wstępnie wymagania dla 100 najpopularniejszych pakietów R. Jednak jeśli instalowane pakiety R wymagają jeszcze innych bibliotek, musisz pobrać skrypt podstawowy użyty tutaj i dodać kroki instalowania bibliotek systemowych. Następnie musisz przekazać zmodyfikowany skrypt do publicznego kontenera obiektów blob w usłudze Azure Storage i użyć zmodyfikowanego skryptu do zainstalowania pakietów.
   >    Aby uzyskać informacje na temat tworzenia akcji skryptu, zobacz [Script Action development](hdinsight-hadoop-script-actions-linux.md) (Tworzenie akcji skryptu).  
   >
   >
   
   ![Dodawanie akcji skryptu](./media/hdinsight-getting-started-with-r/submitscriptaction.png)
   
4. Wybierz polecenie **Utwórz**, aby uruchomić skrypt. Po zakończeniu działania skryptu pakiety R będą dostępne na wszystkich węzłach procesu roboczego.
   
## <a name="using-microsoft-r-server-operationalization"></a>Używanie funkcji opernacjonalizacji oprogramowania Microsoft R Server
Po zakończeniu modelowania danych możesz zopernacjonalizować model, aby wykonywać prognozowanie. Aby skonfigurować funkcję opernacjonalizacji oprogramowania Microsoft R Server, wykonaj poniższe kroki.

Nawiąż połączenie SSH z węzłem krawędzi. Na przykład ```ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net```.

Po nawiązaniu połączenia SSH przejdź do następującego katalogu i użyj polecenia sudo dla biblioteki dll platformy .Net w sposób pokazany poniżej.

```
   cd /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil
   sudo dotnet Microsoft.DeployR.Utils.AdminUtil.dll
```

Aby skonfigurować opernacjonalizację oprogramowania Microsoft R Server pod kątem konfiguracji z jedną maszyną, wykonaj następujące kroki:

* Wybierz pozycję „1. Configure R Server for Operationalization” (Konfiguruj oprogramowanie R Server pod kątem opernacjonalizacji)
* Wybierz pozycję „A. One-box (web + compute nodes)” (Jedna maszyna — sieć Web i węzły obliczeniowe)
* Podaj hasło dla **administratora**

![opernacjonalizacja przy użyciu jednej maszyny](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

Opcjonalnie możesz wykonać sprawdzenia diagnostyczne, uruchamiając test diagnostyczny w sposób pokazany poniżej.

* Wybierz pozycję „6. Run diagnostic tests” (Uruchom testy diagnostyczne)
* Wybierz pozycję „A. Test configuration” (Testuj konfigurację)
* Wpisz ciąg Username = “admin” i hasło określone w kroku konfiguracji powyżej
* Potwierdź wynik: Overall Health = pass (Ogólna kondycja — dobra)
* Zamknij narzędzie administracyjne
* Zamknij połączenie SSH

![Diagnostyka opernacjonalizacji](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)

Na tym etapie konfiguracja opernacjonalizacji jest ukończona. Teraz możesz użyć pakietu „mrsdeploy” w programie RClient do nawiązania połączenia z funkcją opernacjonalizacji na węźle krawędzi i rozpocząć korzystanie z jej możliwości, takich jak [zdalne wykonywanie](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) i [usługi sieci Web](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette). W zależności od tego, czy klaster jest skonfigurowany w sieci wirtualnej, może być konieczne skonfigurowanie tunelowania przekierowania portów za pomocą logowania SSH w opisany poniżej sposób:

### <a name="rserver-cluster-on-virtual-network"></a>Klaster oprogramowania RServer w sieci wirtualnej

Sprawdź, czy ruch na porcie 12800 węzła krawędzi jest dozwolony. W ten sposób możesz użyć węzła krawędzi do nawiązania połączenia z funkcją opernacjonalizacji.

```
library(mrsdeploy)

remoteLogin(
    deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
    username = "admin",
    password = "xxxxxxx"
)
```

Jeśli metoda remoteLogin() nie może połączyć się z węzłem krawędzi, lecz nawiązanie połączenia SSH z węzłem krawędzi jest możliwe, sprawdź, czy reguła zezwalająca na ruch na porcie 12800 jest skonfigurowana poprawnie. Jeśli problem będzie występować nadal, możesz go obejść, konfigurując tunelowanie przekierowania portów przez połączenie SSH.

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a>Klaster oprogramowania RServer w sieci niewirtualnej

Jeśli klaster nie jest skonfigurowany w sieci wirtualnej lub występują problemy z korzystaniem z sieci wirtualnej, możesz użyć tunelowania przekierowania portów protokołu SSH w następujący sposób:

```
ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net
```

W programie Putty także możesz je skonfigurować.

![połączenie ssh w programie putty](./media/hdinsight-hadoop-r-server-get-started/putty.png)

Gdy sesja SSH jest aktywna, ruch z portu 12800 Twojej maszyny będzie przekazywany do portu 12800 węzła krawędzi za pomocą sesji SSH. Upewnij się, że w metodzie remoteLogin() użyto adresu `127.0.0.1:12800`. Umożliwi to logowanie do funkcji opernacjonalizacji węzła krawędzi przez przekierowanie portów.

```
library(mrsdeploy)

remoteLogin(
    deployr_endpoint = "http://127.0.0.1:12800",
    username = "admin",
    password = "xxxxxxx"
)
```

## <a name="how-to-scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Jak skalować węzły obliczeniowe opernacjonalizacji oprogramowania Microsoft R Server na węzłach procesu roboczego usługi HDInsight?
 
 
### <a name="decommission-the-worker-nodes"></a>Likwidowanie węzłów procesu roboczego
Oprogramowanie Microsoft R Server nie jest aktualnie zarządzane za pomocą usługi Yarn. Jeśli węzły procesu roboczego nie zostaną zlikwidowane, menedżer zasobów usługi Yarn nie będzie działać w oczekiwany sposób, ponieważ nie będzie znał zasobów zajętych przez serwer. Aby tego uniknąć, zalecamy zlikwidowanie węzłów procesu roboczego, na które mają być skalowane węzły obliczeniowe.
 
Kroki likwidowania węzłów procesu roboczego:
 
* Zaloguj się do konsoli Ambari klastra usługi HDI i kliknij kartę „hosts” (Hosty)
* Wybierz węzły procesu roboczego do zlikwidowania. Kliknij pozycje „Actions” > „Selected Hosts” > „Hosts” > „Turn ON Maintenance Mode” (Akcje > Wybrane hosty > Hosty > Włącz tryb konserwacji). Na przykład na obrazie poniżej pokazano węzły wn3 i wn4 wybrane do likwidacji.  
   
   ![likwidowanie węzłów procesu roboczego](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  
   
* Kliknij pozycje „Actions” > „Selected Hosts” > „DataNodes” > „Decommission” (Akcje > Wybrane hosty > Węzły danych > Likwiduj)
* Kliknij pozycje „Actions” > „Selected Hosts” > „NodeManagers” > „Decommission” (Akcje > Wybrane hosty > Menedżerowie węzła > Likwiduj)
* Kliknij pozycje „Actions” > „Selected Hosts” > „DataNodes” > „Stop” (Akcje > Wybrane hosty > Węzły danych > Zatrzymaj)
* Kliknij pozycje „Actions” > „Selected Hosts” > „NodeManagers” > „Stop” (Akcje > Wybrane hosty > Menedżerowie węzła > Zatrzymaj)
* Kliknij pozycje „Actions” > „Selected Hosts” > „Hosts” > „Stop All Components” (Akcje > Wybrane hosty > Hosty > Zatrzymaj wszystkie składniki)
* Usuń zaznaczenie węzłów procesu roboczego i wybierz węzły główne
* Kliknij pozycje „Actions” > „Selected Hosts” > „Hosts” > „Restart All Components” (Akcje > Wybrane hosty > Hosty > Uruchom ponownie wszystkie składniki)
 
 
###    <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Konfigurowanie węzłów obliczeniowych na wszystkich zlikwidowanych węzłach procesu roboczego
 
* Połącz się za pomocą protokołu SSH z każdym likwidowanym węzłem procesu roboczego
* Uruchom narzędzie administracyjne za pomocą polecenia `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`
* Wpisz „1”, aby wybrać opcję „1. Configure R Server for Operationalization” (Konfiguruj oprogramowanie R Server pod kątem opernacjonalizacji)
* Wpisz „c”, aby wybrać opcję „C. Compute node” (Węzeł obliczeniowy). To spowoduje skonfigurowanie węzła obliczeniowego na węźle procesu roboczego.
* Zamknij narzędzie administracyjne
 
### <a name="add-compute-nodes-details-on-web-node"></a>Dodawanie szczegółów węzłów obliczeniowych na węźle sieci Web
Po skonfigurowaniu wszystkich zlikwidowanych węzłów procesu roboczego pod kątem uruchamiania węzła obliczeniowego wróć do węzła krawędzi i dodaj adresy IP zlikwidowanych węzłów procesu roboczego do konfiguracji węzła sieci Web oprogramowania Microsoft R Server:
 
* Połącz się z węzłem krawędzi za pomocą protokołu SSH
* Uruchom polecenie `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`
* W sekcji „URIs” (Identyfikatory URI) dodaj adres IP i port węzła procesu roboczego.

![wiersz polecenia likwidowania węzłów procesu roboczego](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)

## <a name="next-steps"></a>Następne kroki
Teraz, gdy już wiesz, jak utworzyć nowy klaster usługi HDInsight z oprogramowaniem R Server i znasz podstawy korzystania z konsoli R w sesji SSH, użyj informacji w następujących sekcjach, aby poznać inne sposoby pracy z oprogramowaniem R Server w usłudze HDInsight.

* [Add RStudio Server to HDInsight (if not installed during cluster creation)](hdinsight-hadoop-r-server-install-r-studio.md) (Dodawanie programu RStudio Server do usługi HDInsight — jeśli nie zainstalowano podczas tworzenia klastra)
* [Compute context options for R Server on HDInsight](hdinsight-hadoop-r-server-compute-contexts.md) (Opcje kontekstu obliczeniowego dla oprogramowania R Server w usłudze HDInsight)
* [Azure Storage options for R Server on HDInsight](hdinsight-hadoop-r-server-storage.md) (Opcje usługi Azure Storage dla oprogramowania R Server w usłudze HDInsight)


