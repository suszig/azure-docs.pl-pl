<properties
    pageTitle="Dowiedz się, jak używać narzędzi Visual Studio Hadoop dla usługi HDInsight | Microsoft Azure"
    description="Dowiedz się, jak zainstalować narzędzia Visual Studio Hadoop dla usługi HDInsight i jak ich używać do nawiązania połączenia z klastrem usługi Hadoop oraz uruchomienia zapytania programu Hive."
    keywords="hadoop tools,hive query,visual studio"
    services="HDInsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="05/25/2016"
    ms.author="jgao"/>

# Rozpoczynanie pracy za pomocą narzędzi Visual Studio Hadoop dla usługi HDInsight w celu uruchamiania zapytań programu Hive

Dowiedz się, jak narzędzia HDInsight Tools for Visual Studio umożliwiają łączenie się z klastrami HDInsight i wysyłanie zapytań programu Hive. Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight, zobacz artykuły [Introduction to HDInsight][hdinsight.introduction] (Wprowadzenie do usługi HDInsight) i [Get started with HDInsight][hdinsight.get.started] (Rozpoczynanie pracy z usługą HDInsight). Aby uzyskać więcej informacji na temat łączenia się z klastrem Storm zobacz [topologii opracowywania C# dla Storm Apache na HDInsight przy użyciu programu Visual Studio][hdinsight.storm.visual.studio.tools] (Tworzenie topologii C# dla Apache Storm w usłudze HDInsight przy użyciu programu Visual Studio).

**Wymagania wstępne**

Do wykonania kroków tego samouczka i korzystania z narzędzi Hadoop w programie Visual Studio potrzebne są:

- Klaster HDInsight Azure: kroki zawarte w tym dokumencie można wykonać, korzystając z klastra opartego na systemie Linux lub Windows. Informacje o tworzeniu klastra można znaleźć w następujących tematach:

    - [Rozpoczynanie pracy z usługą HDInsight opartą na systemie Linux](hdinsight-hadoop-linux-tutorial-get-started.md)
    - [Rozpoczynanie pracy z usługą HDInsight opartą na systemie Windows](hdinsight-hadoop-tutorial-get-started-windows.md)

- Stacja robocza z następującym oprogramowaniem:

    - Windows 8.1, Windows 8 lub Windows 7
    - Visual Studio (jedna z następujących wersji):
        - Visual Studio 2013 Community/Professional/Premium/Ultimate, [Aktualizacja 4](https://www.microsoft.com/download/details.aspx?id=44921)
        - Visual Studio 2015 (Community/Enterprise)

    >[AZURE.NOTE] Obecnie narzędzia HDInsight Tools for Visual Studio występują tylko w angielskiej wersji językowej.


## Instalowanie narzędzi HDInsight Tools for Visual Studio

Narzędzia HDInsight Tools for Visual Studio i sterownik Microsoft Hive ODBC są dołączane do zestawu Microsoft Azure SDK dla programu .NET w wersji 2.5.1 lub nowszej. Można je zainstalować za pomocą [Instalatora platformy sieci Web](http://go.microsoft.com/fwlink/?LinkId=255386). Należy wybrać wersję zgodną z używaną wersją programu Visual Studio. Jeśli nie masz zainstalowanego programu Visual Studio, możesz zainstalować najnowszą wersję programu Visual Studio Community i zestawu Azure SDK za pomocą [Instalatora platformy sieci Web](http://go.microsoft.com/fwlink/?LinkId=255386) lub korzystając z następujących linków:

- [Visual Studio Community 2015 z zestawem Microsoft Azure SDK](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VS2015CommunityAzurePack.appids)
- [Visual Studio Community 2013 z zestawem Microsoft Azure SDK](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VS2013CommunityAzurePack.appids)
- [Zestaw Microsoft Azure SDK dla programu .NET (VS 2015)](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VWDOrVs2015AzurePack.appids)
- [Zestaw Microsoft Azure SDK dla programu .NET (VS 2013)](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VWDOrVs2013AzurePack.appids)

![Narzędzia Hadoop: Instalator platformy sieci Web narzędzi HDinsight Tools for Visual Studio][1]

## Nawiązywanie połączenia z subskrypcjami platformy Azure
Narzędzia HDInsight Tools for Visual Studio umożliwiają nawiązywanie połączenia z klastrami HDInsight, wykonywanie podstawowych operacji zarządzania i uruchamianie zapytań programu Hive.

>[AZURE.NOTE] Informacje dotyczące nawiązywania połączenia z ogólnym klastrem Hadoop znajdują się w temacie [Write and submit Hive queries using Visual Studio](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx) (Zapisywanie i wysyłanie zapytań programu Hive za pomocą programu Visual Studio).


**Aby nawiązać połączenie z subskrypcją platformy Azure**

1.  Otwórz program Visual Studio.
2.  W menu **Widok** kliknij przycisk **Eksplorator serwera**, aby otworzyć okno Eksploratora serwera.
3.  Rozwiń węzeł **Azure**, a następnie rozwiń węzeł **HDInsight**.

    >[AZURE.NOTE]Należy pamiętać, że okno **Lista zadań HDInsight** powinno być otwarte. Jeśli go nie widać, należy kliknąć przycisk **Inne okna** w menu **Widok**, a następnie kliknąć przycisk **Okno listy zadań HDInsight**.  
4.  Wprowadź swoje poświadczenia subskrypcji platformy Azure, a następnie kliknij przycisk **Zaloguj**. Jest to wymagane tylko wtedy, gdy nigdy wcześniej nie nawiązano połączenia z subskrypcją platformy Azure w programie Visual Studio na danej stacji roboczej.
5.  W oknie Eksploratora serwera zostanie wyświetlona lista istniejących klastrów usługi HDInsight. W przypadku braku klastrów można je udostępnić za pomocą portalu Azure, programu Azure PowerShell lub zestawu SDK HDInsight. Aby uzyskać więcej informacji, zobacz artykuł [Provision HDInsight clusters][hdinsight-provision] (Obsługa administracyjna klastrów HDInsight).

    ![Narzędzia Hadoop: lista klastrów Eksploratora serwerów narzędzi HDInsight Tools for Visual Studio][5]
6.  Rozwiń węzeł klastra usługi HDInsight. Zobaczysz **Bazy danych programu Hive**, domyślne konto magazynu, połączone konta magazynu i **Dziennik usługi Hadoop**. Jednostki można rozwinąć.

Po nawiązaniu połączenia z subskrypcją platformy Azure można wykonać następujące czynności:

**Nawiązywanie połączenia z portalem Azure w programie Visual Studio**

- Z poziomu Eksploratora serwera rozwiń węzeł **Azure** > **HDInsight**, kliknij prawym przyciskiem myszy klaster usługi HDInsight, a następnie kliknij przycisk **Zarządzaj klastrem w portalu Azure**.

**Aby zadawać pytania i wyrazić swoją opinię w programie Visual Studio**

- W menu **Narzędzia** kliknij przycisk **HDInsight**, a następnie kliknij przycisk **Forum MSDN**, aby zadać pytania, lub kliknij przycisk **Dodaj opinię**.

## Nawigacja wśród połączonych zasobów

Z poziomu Eksploratora serwera można zobaczyć domyślne konto magazynu i wszystkie połączone konta magazynu. Po rozwinięciu domyślnego konta magazynu można wyświetlić kontenery konta magazynu. Domyślne konto magazynu i domyślny kontener są oznaczone. Można również kliknąć prawym przyciskiem myszy dowolny kontener, aby wyświetlić jego zawartość.

![Lista klastrów Eksploratora serwerów narzędzi HDInsight Tools for Visual Studio][2]

## Uruchomienie zapytania programu Hive
[Apache Hive][apache.hive] to infrastruktura magazynu danych oparta na usłudze Hadoop umożliwiająca dostarczanie podsumowań, zapytań i analiz danych. Narzędzia HDInsight Tools for Visual Studio obsługują uruchamianie zapytań programu Hive w programie Visual Studio. Aby uzyskać więcej informacji na temat programu Hive, zobacz temat [Use Hive with HDInsight][hdinsight.hive] (Korzystanie z programu Hive z usługą HDInsight).

Testowanie skryptu Hive w odniesieniu do klastra usługi HDInsight jest czasochłonne. Może potrwać kilka minut lub dłużej. Narzędzia HDInsight Tools for Visual Studio mogą sprawdzić poprawność skryptu Hive lokalnie bez konieczności nawiązywania połączenia z działającym klastrem.

Przy użyciu narzędzi HDInsight Tools for Visual Studio użytkownicy mogą również zobaczyć zawartość zadania Hive, zbierając i udostępniając dzienniki YARN wybranych zadań Hive.

### Wyświetlanie tabeli **hivesampletable**
Wszystkie klastry usługi HDInsight zawierają przykładową tabelę programu Hive o nazwie *hivesampletable*. Za pomocą tej tabeli można sprawdzić, w jaki sposób należy wyświetlać tabele programu Hive, przeglądać schematy tabeli i wyświetlać wiersze tabeli programu Hive.



**Aby wyświetlić tabele programu Hive oraz schemat tabeli programu Hive**

1.  Z poziomu **Eksploratora serwera** rozwiń węzeł **Azure** > **HDInsight** > wybrany klaster > **Bazy danych programu Hive** > **Domyślne** > **hivesampletable**, aby wyświetlić schemat tabeli.
4.  Kliknij prawym przyciskiem myszy tabelę **hivesampletable**, a następnie kliknij przycisk **Wyświetl pierwszych 100 wierszy**, aby wyświetlić wiersze. Jest to równoważne uruchomieniu następującego zapytania programu Hive za pomocą sterownika ODBC programu Hive:

        SELECT * FROM hivesampletable LIMIT 100

    Liczbę wierszy można dostosować.

    ![Narzędzia Hadoop: zapytanie schematu HDinsight Hive Visual Studio][6]

### Tworzenie tabel programu Hive

Do utworzenia tabeli programu Hive można użyć graficznego interfejsu użytkownika lub zapytań programu Hive. Aby uzyskać informacje o używaniu zapytań programu Hive, zobacz temat [Uruchamianie zapytań Hive](#run.queries).

**Tworzenie tabeli programu Hive**

1. Z poziomu **Eksploratora serwera** rozwiń węzeł **Azure** > **Klastry usługi HDInsight** klaster usługi HDInsight > **Bazy danych programu Hive**, następnie kliknij prawym przyciskiem myszy pozycję **domyślne** i kliknij przycisk **Utwórz tabelę**.
2. Skonfiguruj tabelę.
3. Kliknij przycisk **Utwórz tabelę**, aby przesłać zadanie i utworzyć nową tabelę programu Hive.

    ![Narzędzia usługi Hadoop: tworzenie tabeli hive za pomocą narzędzi hdinsight visual studio tools][7]

### <a name="run.queries"></a>Weryfikowanie i uruchamianie zapytań Hive
Istnieją dwa sposoby tworzenia i uruchamiania zapytań Hive:

- Tworzenie zapytań ad hoc
- Tworzenie aplikacji Hive

**Tworzenie, weryfikowanie i uruchamianie zapytań ad hoc**

1. Z poziomu **Eksploratora serwera** rozwiń węzeł **Azure**, a następnie rozwiń pozycję **Klastry usługi HDInsight**.
2. Kliknij prawym przyciskiem myszy klaster, w którym chcesz uruchomić zapytanie, a następnie kliknij przycisk **Napisz zapytanie Hive**.
3. Wprowadź zapytania Hive. Należy pamiętać, że edytor Hive obsługuje funkcję IntelliSense. Narzędzia HDInsight Tools for Visual Studio obsługują ładowanie zdalnych metadanych podczas edycji skryptu Hive. Na przykład po wpisaniu „SELECT * FROM” funkcja IntelliSense wyświetla wszystkie sugerowane nazwy tabeli. Po określeniu nazwy tabeli nazwy kolumn są wyświetlane przez funkcję IntelliSense. Narzędzie obsługuje prawie wszystkie instrukcje DML programu Hive, podzapytania i wbudowane sterowniki UDF.

    ![Narzędzia usługi Hadoop: funkcja IntelliSense narzędzi HDInsight Visual Studio Tools][13]

    ![Narzędzia usługi Hadoop: funkcja IntelliSense narzędzi HDInsight Visual Studio Tools][14]

    > [AZURE.NOTE] Zostaną zasugerowane tylko metadane klastrów zaznaczone na pasku narzędzi usługi HDInsight.
4. (Opcjonalnie): kliknij przycisk **Weryfikuj skrypt**, aby sprawdzić błędy składniowe skryptu.

    ![Narzędzia Hadoop: lokalna weryfikacja narzędzi HDinsight Tools for Visual Studio][10]

4. Kliknij przycisk **Prześlij** lub **Prześlij (zaawansowane)**. Za pomocą zaawansowanych opcji przesyłania należy skonfigurować wartości, takie jak **Nazwa zadania**, **Argumenty**, **Dodatkowe konfiguracje**, i **Katalog stanu** skryptu:

    ![zapytanie hive hdinsight hadoop][9]

    Po przesłaniu zadania zostanie wyświetlone okno **Podsumowanie zadania Hive**.

    ![Podsumowanie zapytania zapytanie Hive HDInsight Hadoop][8]
5. Użyj przycisku **Odśwież**, aby aktualizować stan do momentu wartości **Ukończono** stanu zadania.
6. Kliknij linki na dole, aby wyświetlić następujące wartości: **Zapytanie dotyczące zadań**, **Dane wyjściowe zadań**, **Dziennik zadań** lub **Dziennik Yarn**.



**Tworzenie i uruchamianie rozwiązania Hive**

1. W menu **PLIK** kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**.
2. Wybierz pozycję **HDInsight** w lewym okienku, wybierz pozycję **Aplikacja Hive** w środkowym okienku, wprowadź właściwości, a następnie kliknij przycisk **OK**.

    ![Narzędzia Hadoop: nowy projekt hive narzędzi hdinsight visual studio tools][11]
3. W **Eksploratorze rozwiązań** kliknij dwukrotnie plik **Script.hql**, aby go otworzyć.
4. Aby zweryfikować skrypt Hive, możesz kliknąć przycisk **Weryfikuj skrypt** lub kliknąć prawym przyciskiem myszy skrypt w edytorze Hive, a następnie kliknąć przycisk **Weryfikuj skrypt** w menu kontekstowym.


### Wyświetlanie zadań Hive
Istnieje możliwość wyświetlenia zapytań dotyczących zadań, danych wyjściowych zadań, dzienników zadań oraz dzienników Yarn dla zadań Hive. Aby uzyskać więcej informacji, zobacz poprzedni zrzut ekranu.

Za pomocą najnowszej wersji narzędzia można sprawdzić zawartość zadań Hive, zbierając i udostępniając dzienniki YARN. Dziennik YARN może być pomocny w badaniu problemów z wydajnością.  Więcej informacji na temat sposobu zbierania dzienników YARN przez usługę HDInsight można znaleźć w temacie [Access HDInsight Application Logs Programmatically][hdinsight.access.application.logs] (Programowe uzyskiwanie dostępu do dzienników aplikacji HDInsight).

**Aby wyświetlić zadania Hive**

1. Z poziomu **Eksploratora serwera** rozwiń węzeł **Azure**, a następnie rozwiń pozycję **HDInsight**.
2. Kliknij prawym przyciskiem myszy klaster usługi HDInsight, a następnie kliknij przycisk **Wyświetl zadania**. Zostanie wyświetlona lista zadań Hive uruchomionych w klastrze.
3. Kliknij zadanie na liście zadań, aby je zaznaczyć, a następnie skorzystaj z okna **Podsumowanie zadania Hive**, aby otworzyć **Zapytanie dotyczące zadań**, **Dane wyjściowe zadań**, **Dziennik zadań** lub **Dziennik YARN**.

    ![Narzędzia Hadoop: zadania Hive widoku narzędzi HDInsight Visual Studio Tools][12]

### Szybsza ścieżka wykonywania zadań Hive za pośrednictwem serwera HiveServer2

>[AZURE.NOTE] Ta funkcja działa tylko w klastrze HDInsight w wersji 3.2 i nowszej.

Narzędzia HDInsight Tools używane do przesyłania zadań Hive za pośrednictwem usługi [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (znanej także jako Templeton). Zwrócenie szczegółów zadania i informacji o błędzie zajęło dużo czasu.
Aby rozwiązać ten problem z wydajnością, narzędzia HDInsight Tools wykonują zadania Hive bezpośrednio w klastrze za pomocą serwera HiveServer2, co pozwala pominąć użycie protokołu RDP/SSH.
Oprócz korzystania z lepszej wydajności użytkownicy mogą także wyświetlać na wykresach Tez zadania Hive oraz szczegóły zadania.

W przypadku klastra HDInsight w wersji 3.2 lub nowszej widoczny jest przycisk **Wykonaj na serwerze HiveServer2**:

![wykonywanie zadań hdinsight visual studio tools na serwerze hiveserver2](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png)

Można również zobaczyć dzienniki przesyłane strumieniowo z powrotem w czasie rzeczywistym, a także wykresy zadań, jeśli zapytanie Hive jest wykonywane w aplikacji Tez.

![szybka ścieżka wykonywania zadań hive hdinsight visual studio tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png)

**Różnica między wykonywaniem zapytań za pomocą serwera HiveServer2 i wysyłaniem zapytań przy użyciu usługi WebHCat**

Mimo że wykonywanie zapytań za pośrednictwem serwera HiveServer2 ma wiele zalet dotyczących wydajności, ma również kilka ograniczeń. Niektóre ograniczenia nie są odpowiednie w środowisku produkcyjnym. W poniższej tabeli przedstawiono różnice:

| |Wykonywanie za pośrednictwem serwera HiveServer2 |Przesyłanie za pośrednictwem usługi WebHCat|
|---|---|---|
|Wykonywanie zapytań|Eliminuje obciążenie w usłudze WebHCat (które powoduje uruchomienie zadania MapReduce o nazwie TempletonControllerJob).|Jeśli zapytanie jest wykonywane przy użyciu usługi WebHCat, spowoduje ona uruchomienie zadania MapReduce, które wprowadza dodatkowe opóźnienia.|
|Strumieniowe przesyłanie dzienników z powrotem|Niemal w czasie rzeczywistym.|Dzienniki wykonywania zadania są dostępne tylko po zakończeniu zadania.|
|Wyświetlanie historii zadań|Jeśli zapytanie jest wykonywane przy użyciu serwera HiveServer2, historia zadania, dziennik zadania ani dane wyjściowe zadania nie są zachowywane. Aplikację można wyświetlić w interfejsie użytkownika YARN z ograniczonymi informacjami.|Jeśli zapytanie jest wykonywane przy użyciu usługi WebHCat, historia zadania, dziennik zadania i dane wyjściowe zadania są zachowywane i można je wyświetlać za pomocą programu Visual Studio/HDInsight SDK PowerShell. |
|Zamykanie okna|  Wykonywanie zadania za pośrednictwem serwera HiveServer2 odbywa się w sposób „synchroniczny”, dlatego należy pozostawić okna otwarte. Zamknięcie okien powoduje anulowanie wykonywania zapytania.|Wysyłanie zadania za pośrednictwem usługi WebHCat odbywa się w sposób „asynchroniczny”, można więc wysłać zapytanie przy użyciu usługi WebHCat i zamknąć program Visual Studio. Można w dowolnym momencie wrócić i wyświetlić wyniki.|


### Wykres wydajności zadania Hive w aplikacji Tez

Narzędzia HDInsight Visual Studio Tools obsługują wyświetlanie wykresów wydajności dla zadań Hive uruchomionych przez aparat wykonywania platformy Tez. Aby uzyskać informacje na temat włączania aplikacji Tez, zobacz artykuł [use Hive in HDInsight][hdinsight.hive] (Używanie programu Hive w usłudze HDInsight). Po przesłaniu zadania Hive w programie Visual Studio program ten wyświetla wykres po zakończeniu zadania.  Może być konieczne kliknięcie przycisku **Odśwież** w celu pobrania najnowszego stanu zadania.

> [AZURE.NOTE] Ta funkcja jest dostępna tylko dla klastra usługi HDInsight w wersji nowszej niż 3.2.4.593 i można jej używać tylko w przypadku zakończonych zadań (jeśli zadanie zostało przesłane za pośrednictwem usługi WebHCat; ten wykres zostanie wyświetlony podczas wykonywania zapytania za pośrednictwem serwera HiveServer2). Działa to zarówno w przypadku klastrów w systemie Windows, jak i Linux.

![wykres wydajności platformy hadoop hive tez](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png)

Aby umożliwić użytkownikom lepsze rozumienie zapytania Hive, w tej wersji narzędzia dodano widok operatorów Hive. Wystarczy dwukrotnie kliknąć wierzchołki wykresu zadania, aby zobaczyć wszystkie operatory wewnątrz wierzchołka. Można również ustawić kursor na konkretnym operatorze, aby wyświetlić więcej szczegółów dotyczących tego operatora.

### Widok wykonywania zadania dla zadań Hive w aplikacji Tez

Widok wykonywania zadania dla zadań Hive w aplikacji Tez może służyć do uzyskiwania strukturalnych i wizualizowanych informacji na temat zadań Hive oraz pozwala uzyskać więcej szczegółów zadania. W przypadku problemów z wydajnością można użyć tego widoku, aby uzyskać więcej szczegółów. Można na przykład uzyskać informacje na temat działania każdego zadania i szczegółowe informacje dotyczące każdego zadania (odczytu i zapisu danych, harmonogramu/rozpoczęcia/zakończenia, itp.) i dzięki temu dostroić konfigurację zadania lub architekturę systemu na podstawie wizualizowanych informacji.

![widok wykonywania zadań narzędzi hdinsight visual studio tools](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png)

## Uruchamianie skryptów usługi Pig

Narzędzia HDInsight Tools for Visual Studio obsługują tworzenie i przesyłanie skryptów usługi Pig do klastrów usługi HDInsight. Użytkownicy mogą utworzyć projekt usługi Pig na podstawie szablonu i przesłać skrypt do klastrów usługi HDInsight.

## Opinie i znane problemy

- Obecnie wyniki serwera HiveServer2 są wyświetlane w formie czystego tekstu, co nie jest idealnym rozwiązaniem. Pracujemy nad rozwiązaniem tego problemu.

- Obecnie wyniki rozpoczynające się od wartości NULL nie są wyświetlane. Wyeliminowaliśmy ten problem. Jeśli mimo tego napotkasz jakiekolwiek przeszkody w pracy wynikające z tego problemu, wyślij do nas wiadomość e-mail lub skontaktuj się z zespołem pomocy technicznej.

- Skrypt HQL utworzony w programie Visual Studio jest zakodowany w zależności od ustawienia lokalnego regionu użytkownika. Skrypt może nie zostać prawidłowo wykonany, jeśli użytkownik prześle skrypt do klastra w postaci binarnej.

Jeśli masz jakiekolwiek sugestie lub wnioski albo napotykasz problemy podczas korzystania z tego narzędzia, wyślij do nas wiadomość e-mail na adres hdivstool@microsoft com.

## Następne kroki
Ten artykuł zawiera informacje na temat sposobów łączenia klastrów usługi HDInsight w programie Visual Studio za pomocą pakietu narzędzi Hadoop oraz sposobu uruchamiania zapytań Hive. Aby uzyskać więcej informacji, zobacz:

- [Use Hadoop Hive in HDInsight][hdinsight.hive] (Używanie usług Hadoop i Hive w usłudze HDInsight)
- [Get started using Hadoop in HDInsight][hdinsight.get.started] (Rozpoczęcie korzystania z usługi Hadoop w usłudze HDInsight)
- [Submit Hadoop jobs in HDInsight][hdinsight.submit.jobs] (Przesyłanie zadań Hadoop w usłudze HDInsight)
- [Analyze Twitter data with Hadoop in HDInsight][hdinsight.analyze.twitter.data] (Analizowanie danych w serwisie Twitter na platformie Hadoop w usłudze HDInsight)


<!--Anchors-->
[Instalacja]: #installation
[Nawiązywanie połączenia z subskrypcją platformy Azure]: #connect-to-your-azure-subscription
[Nawigacja wśród połączonych zasobów]: #navigate-the-linked-resources
[Uruchamianie zapytań Hive]: #run-hive-queries
[Następne kroki]: #next-steps

<!--Image references-->
[1]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png
[2]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png
[5]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png
[6]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png
[7]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png
[8]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png
[9]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png
[10]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png
[11]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png
[12]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png
[13]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png
[14]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png


<!--Link references-->
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight.introduction]: hdinsight-hadoop-introduction.md
[hdinsight.get.started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight.hive]: hdinsight-use-hive.md
[hdinsight.submit.jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight.analyze.twitter.data]: hdinsight-analyze-twitter-data.md
[hdinsight.storm.visual.studio.tools]: hdinsight-storm-develop-csharp-visual-studio-topology.md
[hdinsight.access.application.logs]: hdinsight-hadoop-access-yarn-app-logs.md

[apache.hive]: http://hive.apache.org



<!--HONumber=Jun16_HO2-->


