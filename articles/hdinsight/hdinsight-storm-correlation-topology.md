---
title: "Korelowanie zdarzeń w czasie z Storm oraz bazy danych HBase w usłudze HDInsight"
description: "Dowiedz się, jak skorelować zdarzenia pojawiające się w różnych momentach za pomocą Storm i bazy danych HBase w usłudze HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 17d11479-2d02-4790-8d29-05fb38351479
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 06630096383601e48e8f69f8553314cee42f5f3e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="correlate-events-that-arrive-at-different-times-using-storm-and-hbase"></a>Korelowanie zdarzeń pojawiające się w różnych momentach za pomocą Storm i bazy danych HBase

Przy użyciu magazynu danych z systemu Apache Storm, może skorelować danych wpisów, pojawiające się w różnych godzinach. Na przykład łączenie zdarzeń logowania i wylogowywania sesji użytkownika do obliczenia, jak długo trwającej sesji.

W tym dokumencie Dowiedz się jak utworzyć podstawowy topologii Storm C#, który śledzi zdarzenia logowania i wylogowywania sesji użytkownika, a czas trwania sesji. Topologia używa bazy danych HBase jako magazynu danych. HBase można też wykonać partii zapytania na danych historycznych, aby wygenerować dodatkowe informacje szczegółowe. Na przykład jak wiele sesji użytkownika zostały rozpoczął się lub zakończył się w określonym czasie.

## <a name="prerequisites"></a>Wymagania wstępne

* Visual Studio i narzędzi HDInsight tools for Visual Studio. Aby uzyskać więcej informacji, zobacz [rozpocząć korzystanie z narzędzia HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

* Apache Storm w usłudze HDInsight klastra (z systemem Windows).

  > [!WARNING]
  > Topologie SCP.NET są obsługiwane w klastrach opartych na systemie Linux Storm utworzone po 10/28/2016, HBase zestawu SDK pakietu .NET jest dostępny od 10/28/2016 nie działa poprawnie na HDInsight opartych na systemie Linux

* Bazy danych Apache HBase w klastrze usługi HDInsight (Linux lub z systemem Windows).

  > [!IMPORTANT]
  > Linux jest jedynym systemem operacyjnym używanym w połączeniu z usługą HDInsight w wersji 3.4 lub nowszą. Aby uzyskać więcej informacji, zobacz sekcję [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Wycofanie usługi HDInsight w systemie Windows).

* [Java](https://java.com) 1.7 lub nowszej na środowiska deweloperskiego. Java służy do pakietu topologii po przesłaniu do klastra usługi HDInsight.

  * **JAVA_HOME** zmiennej środowiskowej musi wskazywać katalog, który zawiera Java.
  * **%JAVA_HOME%/bin** katalog musi znajdować się w ścieżce

## <a name="architecture"></a>Architektura

![Diagram przepływu danych za pośrednictwem topologii](./media/hdinsight-storm-correlation-topology/correlationtopology.png)

Korelowanie zdarzeń wymaga identyfikator wspólnej dla źródła zdarzenia. For example identyfikator użytkownika, identyfikator sesji lub inny element danych) unikatowy i (b) uwzględnionych w wszystkich danych przesyłanych do systemu Storm. W tym przykładzie używane wartości identyfikatora GUID do reprezentowania identyfikator sesji.

W tym przykładzie składa się z dwóch klastrów usługi HDInsight:

* HBase: dane historyczne w magazynie danych trwałych
* STORM: używany do pozyskiwania przychodzących danych.

Dane losowo generowany przez topologii Storm i składa się z następujących elementów:

* Identyfikator sesji: identyfikator GUID, który unikatowo identyfikuje każdej sesji
* Zdarzenie: POCZĄTKOWY lub końcowy zdarzeniem. Na przykład, START zawsze występuje przed zakończeniem
* Czas: czas zdarzenia.

Te dane są przetwarzane i przechowywane w bazie danych HBase.

### <a name="storm-topology"></a>Topologii STORM

Po uruchomieniu sesji **START** zdarzeń jest odbierany przez topologii i logowania do bazy danych HBase. Gdy **zakończenia** odebraniu zdarzenia, pobiera topologię **START** zdarzeń i oblicza czasu między dwoma zdarzeniami. To **czas trwania** wartość następnie jest przechowywana w bazie danych HBase, wraz z **zakończenia** informacji o zdarzeniu.

> [!IMPORTANT]
> Gdy ta topologia przedstawia podstawowy wzorzec, rozwiązanie produkcji musi podjąć projektu w następujących scenariuszach:
>
> * Zdarzeń przybywających poza kolejnością
> * Zduplikowane zdarzenia
> * Porzuconych zdarzeń

Przykładowa topologia składa się z następujących składników:

* Session.CS: symuluje sesji użytkownika, tworząc Identyfikatora sesji losowe początkowy czas i czas trwania sesji.

* Spout.CS: tworzy 100 sesji, emituje zdarzenia URUCHAMIANIA czeka losowe limitu czasu dla każdej sesji i następnie emituje zdarzenia zakończenia. Następnie odtwarza zakończenia sesji, aby wygenerować nowe.

* HBaseLookupBolt.cs: używa Identyfikatora sesji do wyszukiwania informacji o sesji z bazy danych HBase. Podczas przetwarzania zdarzenia zakończenia odnajduje odpowiednie zdarzenie rozpoczęcia i czas trwania sesji.

* HBaseBolt.cs: Przechowuje informacje do bazy danych HBase.

* TypeHelper.cs: Ułatwia konwersja typu podczas odczytu / zapisu do bazy danych HBase.

### <a name="hbase-schema"></a>Schemat bazy danych HBase

W bazie danych HBase dane są przechowywane w tabeli zawierającej następujące schematu/ustawienia:

* Klucz wiersza: sesji identyfikator jest używany jako klucz wiersze w tej tabeli.

* Rodziny kolumn: Nazwa rodziny jest "cf". Są przechowywane w tej rodzinie kolumn:

  * Zdarzenie: POCZĄTKOWY lub końcowy.

  * czasu: czas w milisekundach, które wystąpiło zdarzenie.

  * czas trwania: odległość między POCZĄTKOWĄ i KOŃCOWĄ zdarzeń.

* WERSJE: ustawiono rodziny "cf" do zachowania 5 wersje każdego wiersza.

  > [!NOTE]
  > Wersje są dziennik z poprzedniej wartości przechowywanych dla klucza określonego wiersza. Domyślnie HBase tylko zwraca wartość dla najnowszej wersji wiersza. W takim przypadku tego samego wiersza jest używany dla każdej wersji wiersza jest identyfikowany przez wartość sygnatury czasowej wszystkie zdarzenia (START, koniec.). Wersje zapewniają widok historycznych danych dotyczących zdarzenia zarejestrowane dla określonego identyfikatora.

## <a name="download-the-project"></a>Pobieranie projektu

Przykładowy projekt można pobrać z [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).

Ten plik do pobrania zawiera następujących projektów C#:

* CorrelationTopology: Topologii Storm C# losowo emituje zdarzenia rozpoczęcia i zakończenia sesji użytkownika. Każdej sesji ważny od 1 do 5 minut.

* SessionInfo: C# aplikacja konsolowa, która tworzy tabeli HBase i zawiera przykładowe zapytania do zwracania informacji dotyczących sesji przechowywanych danych.

## <a name="create-the-table"></a>Tworzenie tabeli

1. Otwórz **SessionInfo** projektu programu Visual Studio.

2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **SessionInfo** projekt i wybierz **właściwości**.

    ![Zrzut ekranu przedstawiający menu z wybrane właściwości](./media/hdinsight-storm-correlation-topology/selectproperties.png)

3. Wybierz **ustawienia**, następnie ustaw następujące wartości:

   * HBaseClusterURL: adres URL klastra HBase. Na przykład https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: admin/HTTP konta użytkownika dla klastra

   * HBaseClusterPassword: hasło dla konta użytkownika administracyjnego/HTTP

   * HBaseTableName: Nazwa tabeli, aby użyć tego przykładu

   * HBaseTableColumnFamily: Kolumna nazwę rodziny

     ![Obraz okna dialogowego Ustawienia](./media/hdinsight-storm-correlation-topology/settings.png)

4. Uruchom rozwiązanie. Po wyświetleniu monitu wybierz klawisz "c", aby utworzyć tabelę na klaster HBase.

## <a name="build-and-deploy-the-storm-topology"></a>Tworzenie i wdrażanie topologii Storm

1. Otwórz **CorrelationTopology** rozwiązań w programie Visual Studio.

2. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **CorrelationTopology** projektu i wybierz polecenie Właściwości.

3. W oknie właściwości wybierz **ustawienia** , a następnie wprowadź wartości konfiguracji dla tego projektu. 5 pierwszych są takie same wartości używane przez **SessionInfo** projektu:

   * HBaseClusterURL: adres URL klastra HBase. Na przykład https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: admin/HTTP konta użytkownika dla klastra.

   * HBaseClusterPassword: hasło dla konta użytkownika administracyjnego/HTTP.

   * HBaseTableName: Nazwa tabeli do użycia z tym przykładem. Ta wartość jest taka sama nazwa tabeli używana w projekcie SessionInfo.

   * HBaseTableColumnFamily: Kolumna nazwę rodziny. Ta wartość jest tę samą nazwę rodziny kolumny, które jest używane w projekcie SessionInfo.

   > [!IMPORTANT]
   > Nie należy zmieniać HBaseTableColumnNames, wartości domyślne są nazwy używane przez **SessionInfo** do pobierania danych.

4. Zapisz właściwości, a następnie skompilować projekt.

5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **przesyłania do systemu Storm w usłudze HDInsight**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia dla subskrypcji platformy Azure.

   ![Obraz przesyłania do elementu menu storm](./media/hdinsight-storm-correlation-topology/submittostorm.png)

6. W **przesyłania topologii** okno dialogowe, wybierz klaster Storm chcesz wdrożyć tej topologii do.

   > [!NOTE]
   > Przy pierwszym przesłaniu topologii, może potrwać kilka sekund pobrać nazwy klastrów usługi HDInsight.

7. Gdy topologia zostało przekazane i przesłane do klastra, **widoku topologii Storm** otwiera i wyświetla uruchomionej topologii. Aby odświeżyć dane, wybierz **CorrelationTopology** i użyj przycisku Odśwież u góry po prawej części strony.

   ![Obraz topologii widoku](./media/hdinsight-storm-correlation-topology/topologyview.png)

   Kiedy rozpoczyna generowania danych, wartość w topologii **Emitted** zwiększa kolumny.

   > [!NOTE]
   > Jeśli **widoku topologii Storm** nie Otwórz automatycznie, aby go otworzyć, wykonaj następujące kroki:
   >
   > 1. W **Eksploratora rozwiązań**, rozwiń węzeł **Azure**, a następnie rozwiń węzeł **HDInsight**.
   > 2. Kliknij prawym przyciskiem myszy klaster Storm topologia działa na, a następnie wybierz **topologii Storm widoku**

## <a name="query-the-data"></a>Dane zapytania

Po wysyłanego danych wykonaj następujące kroki, aby wysyłać zapytania o dane.

1. Wróć do **SessionInfo** projektu. Jeśli nie jest uruchomiona, Uruchom nowe wystąpienie.

2. Po wyświetleniu monitu wybierz **s** do wyszukania rozpoczęcia zdarzenia. Zostanie wyświetlony monit o wprowadź czas rozpoczęcia i zakończenia, aby zdefiniować zakres czasu - zwracane są tylko zdarzenia między tymi dwiema wartościami godziny.

    Podczas wprowadzania godziny rozpoczęcia i zakończenia, użyj następującego formatu: GG: mm i "Używam" lub "pm". Na przykład 23:20:00.

    Aby przywrócić zarejestrowane zdarzenia, użyj czas rozpoczęcia z przed wdrożonej topologii Storm i czas zakończenia teraz. Danych zwrotnych zawiera wpisy podobne do następującego tekstu:

        Session e6992b3e-79be-4991-afcf-5cb47dd1c81c started at 6/5/2015 6:10:15 PM. Timestamp = 1433527820737

Wyszukiwanie zdarzeń zakończenia działa tak samo, jak rozpoczęcia zdarzenia. Jednak zakończenia zdarzenia są generowane losowo od 1 do 5 minut po zdarzeniu rozpoczęcia. Może być konieczne spróbuj kilka przedziałów czasu, aby znaleźć zdarzenia zakończenia. Zdarzenia zakończenia zawierają również czas trwania sesji - różnicę czasu zdarzenia rozpoczęcia i godzina zakończenia zdarzenia. Oto przykładowe dane dla zdarzenia zakończenia:

    Session fc9fa8e6-6892-4073-93b3-a587040d892e lasted 2 minutes, and ended at 6/5/2015 6:12:15 PM

> [!NOTE]
> Wprowadzone wartości czasu są według czasu lokalnego, czas zwróconych przez kwerendę jest w formacie UTC.

## <a name="stop-the-topology"></a>Zatrzymywanie topologii

Gdy wszystko będzie gotowe zatrzymać topologii, wróć do **CorrelationTopology** projektu programu Visual Studio. W **widoku topologii Storm**, wybierz topologii, a następnie użyj **Kill** u góry widoku topologii.

## <a name="delete-your-cluster"></a>Usuwanie klastra

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej przykładów Storm, zobacz [przykładowe topologie dla systemu Storm w usłudze HDInsight](hdinsight-storm-example-topology.md).
