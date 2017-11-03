---
title: "Użyj widoku wykonania wierzchołka w narzędzi Data Lake Tools dla programu Visual Studio | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak użyć widoku wykonania wierzchołka do zadania usługi Data Lake Analytics egzaminu."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/13/2016
ms.author: jgao
ms.openlocfilehash: b788e7bc8ded86ebd49cc0be73e5b4e1bcbeaba3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Użyj widoku wykonania wierzchołka w narzędzi Data Lake Tools dla programu Visual Studio
Dowiedz się, jak użyć widoku wykonania wierzchołka do zadania usługi Data Lake Analytics egzaminu.

## <a name="prerequisites"></a>Wymagania wstępne

Należy podstawową wiedzę na temat tworzenia skryptu U-SQL przy użyciu narzędzi Data Lake Tools dla programu Visual Studio.  Zobacz [samouczek: tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Otwieranie widoku wykonania wierzchołka
Otwórz zadania skryptu U-SQL w narzędzi Data Lake Tools dla programu Visual Studio. Kliknij przycisk **widoku wykonania wierzchołka** w lewym dolnym rogu. Może pojawić się prośba można najpierw załadować profile i może potrwać pewien czas w zależności od połączenia sieciowego.

![Widoku wykonania wierzchołka narzędzi Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Zrozumienie widoku wykonania wierzchołka
Widoku wykonania wierzchołka ma trzy części:

![Widoku wykonania wierzchołka narzędzi Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

**Selektora wierzchołków** na umożliwia po lewej stronie wybierz wierzchołków przez funkcje (takie jak pierwszych 10 danych do odczytu, lub wybierz etapem). Jednym z najbardziej często używane filtry jest zobacz **wierzchołków ścieżkę krytyczną**. **Ścieżkę krytyczną** najdłuższym łańcuch wierzchołków zadania skryptu U-SQL. Opis ścieżkę krytyczną jest przydatne w przypadku optymalizacji zadaniach sprawdzając, które wierzchołek ma najdłuższy czas.
  
![Widoku wykonania wierzchołka narzędzi Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

Pokazuje okienko górnej **bieżący stan wszystkich wierzchołków**.
  
![Widoku wykonania wierzchołka narzędzi Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

Dolne okienko center zawiera informacje dotyczące każdego wierzchołka:
* Nazwa procesu: Nazwa wystąpienia wierzchołka. Składa się z różnych części w Nazwa etapu | VertexName | VertexRunInstance. Na przykład wierzchołka [62] .v1 SV7_Split oznacza drugi uruchomione wystąpienie (.v1, indeksem rozpoczynającym się od 0) liczby wierzchołków 62 w SV7_Split etapu.
* Odczyt danych łączna/Written: Dane były odczytana/zapisana przez ten wierzchołka.
* Stan Stan/wyjścia: Stan końcowy po zakończeniu wierzchołka.
* Typ wystąpił błąd i kodu zakończenia: Błąd podczas wierzchołka nie powiodło się.
* Tworzenie Przyczyna: Dlaczego wierzchołka został utworzony.
* Zasobów opóźnienia/procesu opóźnienia/NC kolejki opóźnienia: czas poświęcony wierzchołka oczekiwania na zasoby, do przetwarzania danych, a pozostanie w kolejce.
* Identyfikator GUID procesu/Twórcy: Identyfikator GUID bieżącej wierzchołka uruchomione lub twórcy.
* Wersja: N-ty percentyl wystąpienia uruchomionych wierzchołka (system zaplanować nowych wystąpień wierzchołek dla wiele przyczyn, na przykład przejścia w tryb failover obliczeniowe nadmiarowość itp.)
* Utworzona wersja czasu.
* Utwórz początkowy czas i proces w kolejce czas i proces początkowy czas i proces Complete przetworzyć czasu: podczas uruchamiania procesu wierzchołków tworzenia; podczas uruchamiania procesu wierzchołków do kolejki; podczas uruchamiania procesu niektórych wierzchołka; Po wykonaniu pewnych wierzchołka.

## <a name="next-steps"></a>Następne kroki
* Aby rejestrować informacje diagnostyczne, zobacz [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md) (Dostęp do dzienników diagnostycznych usługi Azure Data Lake Analytics)
* Aby uzyskać informacje na temat bardziej złożonego zapytania, zobacz temat [Analizowanie dzienników witryn sieci Web przy użyciu usługi Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Aby wyświetlić szczegóły zadania, zobacz [użyj przeglądarki zadania i widok zadań dla zadania usługi Azure Data lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md)
