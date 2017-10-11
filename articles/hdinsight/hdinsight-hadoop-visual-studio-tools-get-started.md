---
title: "Łączenie z usługą Azure HDInsight przy użyciu narzędzi Data Lake Tools for Visual Studio | Microsoft Docs"
description: "Dowiedz się, jak zainstalować narzędzia Data Lake Tools for Visual Studio oraz korzystać z nich, aby połączyć się z klastrami Hadoop w usłudze Azure HDInsight i uruchomić zapytania programu Hive."
keywords: hadoop tools,hive query,visual studio,visual studio hadoop
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ce9c572a-1e98-46bf-9581-13a9767f1fa5
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: jgao
ms.openlocfilehash: f7eff70ece5e8dc7e0c5983871d40bd1f6eafff2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="connect-to-azure-hdinsight-and-run-hive-queries-using-data-lake-tools-for-visual-studio"></a>Łączenie z usługą Azure HDInsight i uruchamianie zapytań programu Hive przy użyciu narzędzi Data Lake Tools for Visual Studio

Dowiedz się, jak korzystać z narzędzi Data Lake Tools for Visual Studio, aby połączyć się z klastrami Hadoop w usłudze [Azure HDInsight](hdinsight-hadoop-introduction.md) i przesłać zapytania programu Hive. Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight, zobacz artykuły [Introduction to HDInsight](hdinsight-hadoop-introduction.md) (Wprowadzenie do usługi HDInsight) i [Get started with HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) (Rozpoczynanie pracy z usługą HDInsight). Aby uzyskać więcej informacji na temat łączenia się z klastrem Storm, zobacz artykuł [Develop C# topologies for Apache Storm on HDInsight using Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) (Tworzenie topologii C# dla usługi Apache Storm w usłudze HDInsight przy użyciu programu Visual Studio).

Narzędzia Data Lake Tools for Visual Studio mogą być używane do uzyskiwania dostępu do usług Data Lake Analytics i HDInsight.  Aby uzyskać informacje na temat narzędzi Data Lake Tools, zobacz [Samouczek: tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

**Wymagania wstępne**

Do wykonania kroków tego samouczka i korzystania z narzędzi Data Lake Tools w programie Visual Studio potrzebne są:

* Klaster usługi Azure HDInsight: aby utworzyć klaster, zobacz [Rozpoczynanie pracy z usługą HDInsight opartą na systemie Linux](hdinsight-hadoop-linux-tutorial-get-started.md)
* Stacja robocza z następującym oprogramowaniem:
  
  * Windows 10, Windows 8.1, Windows 8 lub Windows 7.
  * Visual Studio 2013/2015/2017.
    
    > [!NOTE]
    > Obecnie narzędzia Data Lake Tools for Visual Studio występują tylko w angielskiej wersji językowej.
    > 
    > 

## <a name="install-data-lake-tools-for-visual-studio"></a>Instalacja narzędzi Data Lake Tools for Visual Studio

Narzędzia Data Lake Tools są domyślnie instalowane w programie Visual Studio 2017. W starszych wersjach można je zainstalować za pomocą [Instalatora platformy sieci Web](https://www.microsoft.com/web/downloads/). Należy wybrać wersję zgodną z używaną wersją programu Visual Studio. Jeśli nie masz zainstalowanego programu Visual Studio, możesz zainstalować najnowszą wersję programu Visual Studio Community i zestawu Azure SDK za pomocą [Instalatora platformy sieci Web](https://www.microsoft.com/web/downloads/):

![Instalator platformy sieci Web dla narzędzi Data Lake Tools for Visual Studio.](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png "Korzystanie z instalatora platformy sieci Web w celu zainstalowania narzędzi Data Lake Tools for Visual Studio")

## <a name="connect-to-azure-subscriptions"></a>Nawiązywanie połączenia z subskrypcjami platformy Azure
Narzędzia Data Lake Tools for Visual Studio umożliwiają nawiązywanie połączenia z klastrami HDInsight, wykonywanie podstawowych operacji zarządzania i uruchamianie zapytań programu Hive.

> [!NOTE]
> Informacje dotyczące nawiązywania połączenia z ogólnym klastrem Hadoop znajdują się w temacie [Write and submit Hive queries using Visual Studio](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx) (Zapisywanie i wysyłanie zapytań programu Hive za pomocą programu Visual Studio).
> 
> 

**Aby nawiązać połączenie z subskrypcją platformy Azure**

1. Otwórz program Visual Studio.
2. W menu **Widok** kliknij przycisk **Eksplorator serwera**, aby otworzyć okno Eksploratora serwera.
3. Rozwiń węzeł **Azure**, a następnie rozwiń węzeł **HDInsight**.
   
   > [!NOTE]
   > Należy pamiętać, że okno **Lista zadań HDInsight** powinno być otwarte. Jeśli go nie widać, należy kliknąć przycisk **Inne okna** w menu **Widok**, a następnie kliknąć przycisk **Okno listy zadań HDInsight**.  
   > 
   > 
4. Wprowadź swoje poświadczenia subskrypcji platformy Azure, a następnie kliknij przycisk **Zaloguj**. Jest to wymagane tylko wtedy, gdy nigdy wcześniej nie nawiązano połączenia z subskrypcją platformy Azure w programie Visual Studio na danej stacji roboczej.
5. W oknie Eksploratora serwera zostanie wyświetlona lista istniejących klastrów usługi HDInsight. W przypadku braku klastrów możesz go utworzyć za pomocą witryny Azure Portal, programu Azure PowerShell lub zestawu SDK usługi HDInsight. Więcej informacji można znaleźć w artykule [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md) (Tworzenie klastrów usługi HDInsight).
   
   ![Lista klastrów Eksploratora serwera narzędzi Data Lake Tools for Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Eksplorator serwera narzędzi Data Lake Tools for Visual Studio")
6. Rozwiń węzeł klastra usługi HDInsight. Zobaczysz **Bazy danych programu Hive**, domyślne konto magazynu, połączone konta magazynu i **Dziennik usługi Hadoop**. Jednostki można rozwinąć.

Po nawiązaniu połączenia z subskrypcją platformy Azure można wykonać następujące czynności:

**Nawiązywanie połączenia z witryną Azure Portal w programie Visual Studio**

* Z poziomu Eksploratora serwera rozwiń węzeł **Azure** > **HDInsight**, kliknij prawym przyciskiem myszy klaster usługi HDInsight, a następnie kliknij przycisk **Zarządzaj klastrem w witrynie Azure Portal**.

**Zadawanie pytań i wyrażanie opinii w programie Visual Studio**

* W menu **Narzędzia** kliknij przycisk **HDInsight**, a następnie kliknij przycisk **Forum MSDN**, aby zadać pytania, lub kliknij przycisk **Dodaj opinię**.

## <a name="navigate-the-linked-resources"></a>Nawigacja wśród połączonych zasobów
Z poziomu Eksploratora serwera można zobaczyć domyślne konto magazynu i wszystkie połączone konta magazynu. Po rozwinięciu domyślnego konta magazynu można wyświetlić kontenery konta magazynu. Domyślne konto magazynu i domyślny kontener są oznaczone. Można również kliknąć prawym przyciskiem myszy dowolny kontener, aby wyświetlić jego zawartość.

![Połączone zasoby listy Eksploratora serwera narzędzi Data Lake Tools for Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "połączone zasoby listy")

Po otwarciu kontenera można używać następujących przycisków do przekazywania, usuwania i pobierania obiektów blob:

![Operacje obiektów blob Eksploratora serwera narzędzi Data Lake Tools for Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "przekazywanie, usuwanie i pobieranie obiektów blob")

## <a name="run-a-hive-query"></a>Uruchomienie zapytania programu Hive
[Apache Hive](http://hive.apache.org) to infrastruktura magazynu danych oparta na usłudze Hadoop umożliwiająca dostarczanie podsumowań, zapytań i analizy danych. Narzędzia Data Lake Tools for Visual Studio obsługują uruchamianie zapytań programu Hive w programie Visual Studio. Aby uzyskać więcej informacji na temat programu Hive, zobacz artykuł [Use Hive with HDInsight](hdinsight-use-hive.md) (Korzystanie z programu Hive z usługą HDInsight).

Testowanie skryptu Hive w odniesieniu do klastra usługi HDInsight jest czasochłonne. Może potrwać kilka minut lub dłużej. Narzędzia Data Lake Tools for Visual Studio mogą sprawdzić poprawność skryptu Hive lokalnie bez konieczności nawiązywania połączenia z działającym klastrem.

Przy użyciu narzędzi Data Lake Tools for Visual Studio użytkownicy mogą również zobaczyć zawartość zadania Hive, zbierając i udostępniając dzienniki YARN wybranych zadań Hive.

### <a name="view-the-hivesampletable"></a>Wyświetlanie tabeli **hivesampletable**
Wszystkie klastry usługi HDInsight zawierają przykładową tabelę programu Hive o nazwie *hivesampletable*. Za pomocą tej tabeli można sprawdzić, w jaki sposób należy wyświetlać tabele programu Hive, przeglądać schematy tabeli i wyświetlać wiersze tabeli programu Hive.

**Aby wyświetlić tabele programu Hive oraz schemat tabeli programu Hive**

1. Z poziomu **Eksploratora serwera** rozwiń węzeł **Azure** > **HDInsight** > wybrany klaster > **Bazy danych programu Hive** > **Domyślne** > **hivesampletable**, aby wyświetlić schemat tabeli.
2. Kliknij prawym przyciskiem myszy tabelę **hivesampletable**, a następnie kliknij przycisk **Wyświetl pierwszych 100 wierszy**, aby wyświetlić wiersze. Jest to równoważne uruchomieniu następującego zapytania programu Hive za pomocą sterownika ODBC programu Hive:
   
     SELECT * FROM hivesampletable LIMIT 100
   
   Liczbę wierszy można dostosować.
   
   ![Narzędzia Data Lake Tools: zapytanie schematu HDInsight Hive Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Wyniki zapytania programu Hive")

### <a name="create-hive-tables"></a>Tworzenie tabel programu Hive
Do utworzenia tabeli programu Hive można użyć graficznego interfejsu użytkownika lub zapytań programu Hive. Aby uzyskać informacje o używaniu zapytań programu Hive, zobacz temat [Uruchamianie zapytań Hive](#run.queries).

**Tworzenie tabeli programu Hive**

1. Z poziomu **Eksploratora serwera** rozwiń węzeł **Azure** > **Klastry usługi HDInsight** klaster usługi HDInsight > **Bazy danych programu Hive**, następnie kliknij prawym przyciskiem myszy pozycję **domyślne** i kliknij przycisk **Utwórz tabelę**.
2. Skonfiguruj tabelę.
3. Kliknij przycisk **Utwórz tabelę**, aby przesłać zadanie i utworzyć nową tabelę programu Hive.
   
    ![Narzędzia Data Lake Tools: narzędzia HDInsight Visual Studio Tools tworzą tabelę programu Hive](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Tworzenie tabeli programu Hive")

### <a name="run.queries"></a>Weryfikowanie i uruchamianie zapytań Hive
Istnieją dwa sposoby tworzenia i uruchamiania zapytań Hive:

* Tworzenie zapytań ad hoc
* Tworzenie aplikacji Hive

**Tworzenie, weryfikowanie i uruchamianie zapytań ad hoc**

1. Z poziomu **Eksploratora serwera** rozwiń węzeł **Azure**, a następnie rozwiń pozycję **Klastry usługi HDInsight**.
2. Kliknij prawym przyciskiem myszy klaster, w którym chcesz uruchomić zapytanie, a następnie kliknij przycisk **Napisz zapytanie Hive**.
3. Wprowadź zapytania Hive. Należy pamiętać, że edytor Hive obsługuje funkcję IntelliSense. Narzędzia Data Lake Tools for Visual Studio obsługują ładowanie zdalnych metadanych podczas edycji skryptu Hive. Na przykład po wpisaniu „SELECT * FROM” funkcja IntelliSense wyświetla wszystkie sugerowane nazwy tabeli. Po określeniu nazwy tabeli nazwy kolumn są wyświetlane przez funkcję IntelliSense. Narzędzia obsługują prawie wszystkie instrukcje DML programu Hive, podzapytania i wbudowane sterowniki UDF.
   
    ![Narzędzia Data Lake Tools: funkcja IntelliSense narzędzi HDInsight Visual Studio Tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "U-SQL IntelliSense")
   
    ![Narzędzia Data Lake Tools: funkcja IntelliSense narzędzi HDInsight Visual Studio Tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "U-SQL IntelliSense")
   
   > [!NOTE]
   > Zostaną zasugerowane tylko metadane klastrów zaznaczone na pasku narzędzi usługi HDInsight.
   > 
   > 
4. (Opcjonalnie): kliknij przycisk **Weryfikuj skrypt**, aby sprawdzić błędy składniowe skryptu.
   
    ![Data Lake Tools: lokalna weryfikacja narzędzi Data Lake Tools for Visual Studio](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png "Weryfikacja skryptu")
5. Kliknij przycisk **Prześlij** lub **Prześlij (zaawansowane)**. Za pomocą zaawansowanych opcji przesyłania należy skonfigurować wartości, takie jak **Nazwa zadania**, **Argumenty**, **Dodatkowe konfiguracje**, i **Katalog stanu** skryptu:
   
    ![Zapytanie programu Hive w usłudze HDInsight Hadoop](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Przesłanie zapytań")
   
    Po przesłaniu zadania zostanie wyświetlone okno **Podsumowanie zadania Hive**.
   
    ![Podsumowanie zapytania programu Hive w usłudze HDInsight Hadoop](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png "Podsumowanie zadania Hive")
6. Użyj przycisku **Odśwież**, aby aktualizować stan do momentu wartości **Ukończono** stanu zadania.
7. Kliknij linki na dole, aby wyświetlić następujące wartości: **Zapytanie dotyczące zadań**, **Dane wyjściowe zadań**, **Dziennik zadań** lub **Dziennik Yarn**.

**Tworzenie i uruchamianie rozwiązania Hive**

1. W menu **PLIK** kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**.
2. Wybierz pozycję **HDInsight** w lewym okienku, wybierz pozycję **Aplikacja Hive** w środkowym okienku, wprowadź właściwości, a następnie kliknij przycisk **OK**.
   
    ![Narzędzia Data Lake Tools: nowy projekt programu Hive w usłudze HDInsight Visual Studio Tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Tworzenie aplikacji Hive z programu Visual Studio")
3. W **Eksploratorze rozwiązań** kliknij dwukrotnie plik **Script.hql**, aby go otworzyć.
4. Aby zweryfikować skrypt Hive, możesz kliknąć przycisk **Weryfikuj skrypt** lub kliknąć prawym przyciskiem myszy skrypt w edytorze Hive, a następnie kliknąć przycisk **Weryfikuj skrypt** w menu kontekstowym.

### <a name="view-hive-jobs"></a>Wyświetlanie zadań Hive
Istnieje możliwość wyświetlenia zapytań dotyczących zadań, danych wyjściowych zadań, dzienników zadań oraz dzienników Yarn dla zadań Hive. Aby uzyskać więcej informacji, zobacz poprzedni zrzut ekranu.

Za pomocą najnowszej wersji narzędzi można sprawdzić zawartość zadań Hive, zbierając i udostępniając dzienniki YARN. Dziennik YARN może być pomocny w badaniu problemów z wydajnością.  Więcej informacji na temat sposobu zbierania dzienników YARN przez usługę HDInsight można znaleźć w artykule [Access HDInsight Application Logs Programmatically](hdinsight-hadoop-access-yarn-app-logs.md) (Programowe uzyskiwanie dostępu do dzienników aplikacji usługi HDInsight).

**Wyświetlanie zadań Hive**

1. Z poziomu **Eksploratora serwera** rozwiń węzeł **Azure**, a następnie rozwiń pozycję **HDInsight**.
2. Kliknij prawym przyciskiem myszy klaster usługi HDInsight, a następnie kliknij przycisk **Wyświetl zadania**. Zostanie wyświetlona lista zadań Hive uruchomionych w klastrze.
3. Kliknij zadanie na liście zadań, aby je zaznaczyć, a następnie skorzystaj z okna **Podsumowanie zadania Hive**, aby otworzyć **Zapytanie dotyczące zadań**, **Dane wyjściowe zadań**, **Dziennik zadań** lub **Dziennik YARN**.
   
    ![Narzędzia Data Lake Tools: zadania Hive widoku narzędzi HDInsight Visual Studio Tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Wyświetlanie zadań Hive")

### <a name="faster-path-hive-execution-via-hiveserver2"></a>Szybsza ścieżka wykonywania zadań Hive za pośrednictwem serwera HiveServer2
> [!NOTE]
> Ta funkcja działa tylko w klastrze HDInsight w wersji 3.2 i nowszej.
> 
> 

Narzędzia Data Lake Tools używane do przesyłania zadań Hive za pośrednictwem usługi [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (znanej także jako Templeton). Zwrócenie szczegółów zadania i informacji o błędzie zajęło dużo czasu.
Aby rozwiązać ten problem z wydajnością, narzędzia Data Lake Tools wykonują zadania Hive bezpośrednio w klastrze za pomocą serwera HiveServer2, co pozwala pominąć użycie protokołu RDP/SSH.
Oprócz korzystania z lepszej wydajności użytkownicy mogą także wyświetlać na wykresach Tez zadania Hive oraz szczegóły zadania.

W przypadku klastra HDInsight w wersji 3.2 lub nowszej widoczny jest przycisk **Wykonaj na serwerze HiveServer2**:

![Wykonywanie przez narzędzia Data Lake Visual Studio Tools na serwerze hiveserver2](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png "Wykonywanie zapytań Hive przy użyciu serwera HiveServer2")

Można również zobaczyć dzienniki przesyłane strumieniowo z powrotem w czasie rzeczywistym, a także wykresy zadań, jeśli zapytanie Hive jest wykonywane w aplikacji Tez.

![Szybka ścieżka wykonywania zadań Hive w usłudze Data Lake Visual Studio Tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "Wyświetlanie wykresów zadań")

**Różnica między wykonywaniem zapytań za pomocą serwera HiveServer2 i wysyłaniem zapytań przy użyciu usługi WebHCat**

Mimo że wykonywanie zapytań za pośrednictwem serwera HiveServer2 ma wiele zalet dotyczących wydajności, ma również kilka ograniczeń. Niektóre ograniczenia nie są odpowiednie w środowisku produkcyjnym. W poniższej tabeli przedstawiono różnice:

|  | Wykonywanie za pośrednictwem serwera HiveServer2 | Przesyłanie za pośrednictwem usługi WebHCat |
| --- | --- | --- |
| Wykonywanie zapytań |Eliminuje obciążenie w usłudze WebHCat (które powoduje uruchomienie zadania MapReduce o nazwie TempletonControllerJob). |Jeśli zapytanie jest wykonywane przy użyciu usługi WebHCat, spowoduje ona uruchomienie zadania MapReduce, które wprowadza dodatkowe opóźnienia. |
| Strumieniowe przesyłanie dzienników z powrotem |Niemal w czasie rzeczywistym. |Dzienniki wykonywania zadania są dostępne tylko po zakończeniu zadania. |
| Wyświetlanie historii zadań |Jeśli zapytanie jest wykonywane przy użyciu serwera HiveServer2, jego historia zadania, dziennik zadania ani dane wyjściowe zadania nie są zachowywane. Aplikację można wyświetlić w interfejsie użytkownika YARN z ograniczonymi informacjami. |Jeśli zapytanie jest wykonywane przy użyciu usługi WebHCat, historia zadania, dziennik zadania i dane wyjściowe zadania są zachowywane i można je wyświetlać za pomocą programu Visual Studio/HDInsight SDK PowerShell. |
| Zamykanie okna |Wykonywanie zadania za pośrednictwem serwera HiveServer2 odbywa się w sposób „synchroniczny”, dlatego należy pozostawić okna otwarte. Zamknięcie okien powoduje anulowanie wykonywania zapytania. |Wysyłanie zadania za pośrednictwem usługi WebHCat odbywa się w sposób „asynchroniczny”, można więc wysłać zapytanie przy użyciu usługi WebHCat i zamknąć program Visual Studio. Można w dowolnym momencie wrócić i wyświetlić wyniki. |

### <a name="tez-hive-job-performance-graph"></a>Wykres wydajności zadania Hive w aplikacji Tez
Narzędzia Data Lake Tools obsługują wyświetlanie wykresów wydajności dla zadań Hive uruchomionych przez aparat wykonywania platformy Tez. Aby uzyskać informacje na temat włączania aplikacji Tez, zobacz artykuł [Use Hive in HDInsight](hdinsight-use-hive.md) (Używanie programu Hive w usłudze HDInsight). Po przesłaniu zadania Hive w programie Visual Studio program ten wyświetla wykres po zakończeniu zadania.  Może być konieczne kliknięcie przycisku **Odśwież** w celu pobrania najnowszego stanu zadania.

> [!NOTE]
> Ta funkcja jest dostępna tylko dla klastra usługi HDInsight w wersji nowszej niż 3.2.4.593 i można jej używać tylko w przypadku zakończonych zadań (jeśli zadanie zostało przesłane za pośrednictwem usługi WebHCat; ten wykres zostanie wyświetlony podczas wykonywania zapytania za pośrednictwem serwera HiveServer2). Działa to zarówno w przypadku klastrów w systemie Windows, jak i Linux.
> 
> 

![Wykres wydajności aplikacji Tez w usłudze Hadoop Hive](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png "Stan zadania")

Aby umożliwić użytkownikom lepsze rozumienie zapytania Hive, w tej wersji narzędzi dodano widok operatorów Hive. Wystarczy dwukrotnie kliknąć wierzchołki wykresu zadania, aby zobaczyć wszystkie operatory wewnątrz wierzchołka. Można również ustawić kursor na konkretnym operatorze, aby wyświetlić więcej szczegółów dotyczących tego operatora.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Widok wykonywania zadania dla zadań Hive w aplikacji Tez
Widok wykonywania zadania dla zadań Hive w aplikacji Tez może służyć do uzyskiwania strukturalnych i wizualizowanych informacji na temat zadań Hive oraz pozwala uzyskać więcej szczegółów zadania. W przypadku problemów z wydajnością można użyć tego widoku, aby uzyskać więcej szczegółów. Można na przykład uzyskać informacje na temat działania każdego zadania i szczegółowe informacje dotyczące każdego zadania (odczytu i zapisu danych, harmonogramu/rozpoczęcia/zakończenia, itp.) i dzięki temu dostroić konfigurację zadania lub architekturę systemu na podstawie wizualizowanych informacji.

![Widok wykonywania zadań narzędzi Data Lake Visual Studio Tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "Widok wykonywania zadań")

## <a name="run-pig-scripts"></a>Uruchamianie skryptów usługi Pig
Narzędzia Data Lake Tools for Visual Studio obsługują tworzenie i przesyłanie skryptów usługi Pig do klastrów usługi HDInsight. Użytkownicy mogą utworzyć projekt usługi Pig na podstawie szablonu i przesłać skrypt do klastrów usługi HDInsight.

## <a name="feedbacks--known-issues"></a>Opinie i znane problemy
* Obecnie wyniki serwera HiveServer2 są wyświetlane w formie czystego tekstu, co nie jest idealnym rozwiązaniem. Pracujemy nad rozwiązaniem tego problemu.
* Obecnie wyniki rozpoczynające się od wartości NULL nie są wyświetlane. Wyeliminowaliśmy ten problem. Jeśli mimo tego napotkasz jakiekolwiek przeszkody w pracy wynikające z tego problemu, wyślij do nas wiadomość e-mail lub skontaktuj się z zespołem pomocy technicznej.
* Skrypt HQL utworzony w programie Visual Studio jest zakodowany w zależności od ustawienia lokalnego regionu użytkownika. Skrypt może nie zostać prawidłowo wykonany, jeśli użytkownik prześle skrypt do klastra w postaci binarnej.

## <a name="next-steps"></a>Następne kroki
Ten artykuł zawiera informacje na temat sposobów łączenia klastrów usługi HDInsight w programie Visual Studio za pomocą pakietu narzędzi Data Lake (HDInsight) Tools oraz sposobu uruchamiania zapytań Hive. Aby uzyskać więcej informacji, zobacz:

* [Use Hadoop Hive in HDInsight](hdinsight-use-hive.md) (Używanie usługi Hadoop Hive w usłudze HDInsight)
* [Rozpoczęcie korzystania z usługi Hadoop w usłudze HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Przesyłanie zadań Hadoop w usłudze HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Analizowanie danych z serwisu Twitter na platformie Hadoop w usłudze HDInsight](hdinsight-analyze-twitter-data.md)

