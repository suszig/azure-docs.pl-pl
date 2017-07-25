---
title: "Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools for Visual Studio | Microsoft Docs"
description: "Poznaj sposoby instalowania narzędzi Data Lake Tools for Visual Studio oraz tworzenia i testowania skryptów U-SQL."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/28/2017
ms.author: saveenr, yanacai
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 7bbbb08ff635477a88403a3ae6bd3486d31838ef
ms.contentlocale: pl-pl
ms.lasthandoff: 07/19/2017

---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools for Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Poznaj sposoby tworzenia kont usługi Azure Data Lake Analytics przy użyciu programu Visual Studio, definiowania zadań w języku [U-SQL](data-lake-analytics-u-sql-get-started.md) oraz przesyłania zadań do usługi Data Lake Analytics. Więcej informacji na temat usługi Data Lake Analytics można znaleźć w artykule [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).


## <a name="prerequisites"></a>Wymagania wstępne

* **Visual Studio**: obsługiwane są wszystkie wersje z wyjątkiem Express.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **Zestaw Microsoft Azure SDK dla platformy .NET** w wersji 2.7.1 lub nowszej.  Można go zainstalować przy użyciu [Instalatora platformy internetowej](http://www.microsoft.com/web/downloads/platform.aspx).
* Konto usługi **Data Lake Analytics**. Aby utworzyć konto, zobacz artykuł [Wprowadzenie do pracy z usługą Azure Data Lake Analytics za pomocą witryny Azure Portal](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>Instalowanie narzędzi Azure Data Lake Tools for Visual Studio 

Narzędzia Azure Data Lake Tools for Visual Studio możesz pobrać i zainstalować [z Centrum pobierania](http://aka.ms/adltoolsvs). Po instalacji pamiętaj, że:
* Węzeł **Eksplorator serwera** > **Azure** zawiera węzeł **Data Lake Analytics**. 
* Menu **Narzędzia** zawiera element **Data Lake**.

## <a name="connect-to-an-azure-data-lake-analytics-account"></a>Łączenie z kontem usługi Azure Data Lake Analytics

1. Otwórz program Visual Studio.
2. Otwórz Eksplorator serwera, wybierając pozycje **Widok** > **Eksplorator serwera**.
3. Kliknij prawym przyciskiem myszy pozycję **Azure**. Następnie wybierz pozycję **Połącz z subskrypcją platformy Microsoft Azure** i postępuj zgodnie z instrukcjami.
4. W Eksploratorze serwera wybierz pozycje **Azure** > **Data Lake Analytics**. Zobaczysz listę swoich kont usługi Data Lake Analytics.


## <a name="write-your-first-u-sql-script"></a>Pisanie pierwszego skryptu U-SQL

Poniższy tekst to prosty skrypt U-SQL. Definiuje on mały zestaw danych i zapisuje go w domyślnym magazynie Data Lake Store jako plik o nazwie `/data.csv`.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

### <a name="submit-a-data-lake-analytics-job"></a>Przesyłanie zadania usługi Data Lake Analytics

1. Wybierz kolejno pozycje **Plik** > **Nowy** > **Projekt**.

2. Wybierz typ **Projekt U-SQL**, a następnie kliknij pozycję **OK**. Program Visual Studio utworzy rozwiązanie z użyciem pliku **Script.usql**.

3. Wklej poprzedni skrypt do okna **Script.usql**.

4. W lewym górnym rogu okna **Script.usql** określ konto usługi Data Lake Analytics.

    ![Przesyłanie projektu U-SQL programu Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. W lewym górnym rogu okna **Script.usql** wybierz pozycję **Prześlij**.
6. Sprawdź **konto usługi Analytics**, a następnie wybierz pozycję **Prześlij**. Po zakończeniu przesyłania jego wyniki są dostępne w oknie wyników narzędzi Data Lake Tools for Visual Studio.

    ![Przesyłanie projektu U-SQL programu Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. Aby wyświetlić najbardziej aktualny stan zadania i odświeżyć ekran, kliknij pozycję **Odśwież**. W przypadku pomyślnego wykonania zadania widoczne będą karty **Graf zadania**, **Operacje dotyczące metadanych**, **Historia stanu** i **Diagnostyka**:

    ![Wykres wydajności zadania skryptu U-SQL programu Visual Studio w usłudze Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * Karta **Podsumowanie zadania** zawiera podsumowanie zadania.   
   * Karta **Szczegóły zadania** zawiera bardziej szczegółowe informacje o zadaniu, w tym o skrypcie, zasobach i wierzchołkach.
   * Karta **Graf zadania** wizualizuje postęp zadania.
   * Karta **Operacje dotyczące metadanych** pokazuje wszystkie akcje, które zostały wykonane na wykazie języka U-SQL.
   * Karta **Dane** przedstawia wszystkie dane wejściowe i wyjściowe.
   * Karta **Diagnostyka** udostępnia zaawansowaną analizę wykonywania zadania i optymalizacji wydajności.

### <a name="to-check-job-state"></a>Aby sprawdzić stan zadania

1. W Eksploratorze serwera wybierz pozycje **Azure** > **Data Lake Analytics**. 
2. Rozwiń nazwę konta usługi Data Lake Analytics.
3. Kliknij dwukrotnie pozycję **Zadania**.
4. Wybierz wcześniej przesłane zadanie.

### <a name="to-see-the-output-of-a-job"></a>Aby wyświetlić dane wyjściowe zadania

1. W Eksploratorze serwera przejdź do przesłanego zadania.
2. Kliknij kartę **Dane**.
3. Na karcie **Dane wyjściowe zadania** wybierz plik `"/data.csv"`.

## <a name="next-steps"></a>Następne kroki

* [Uruchamianie skryptów U-SQL na swojej stacji roboczej w celu testowania i debugowania](data-lake-analytics-data-lake-tools-local-run.md)
* [Debugowanie kodu C# w zadaniach U-SQL](data-lake-analytics-debug-u-sql-jobs.md)
* [Korzystanie z narzędzi Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)

