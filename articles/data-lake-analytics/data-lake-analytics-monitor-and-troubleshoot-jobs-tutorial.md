---
title: "Rozwiązywanie problemów z zadania usługi Azure Data Lake Analytics przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak rozwiązywać problemy z zadań usługi Data Lake Analytics przy użyciu portalu Azure. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: f6168997c449be5354bd223c516d4f929a1bf894
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshoot-azure-data-lake-analytics-jobs-using-azure-portal"></a>Rozwiązywanie problemów z zadań usługi Azure Data Lake Analytics przy użyciu portalu Azure
Dowiedz się, jak rozwiązywanie problemów z zadań usługi Data Lake Analytics za pomocą portalu Azure.

W tym samouczku zostanie Konfigurowanie Brak problem pliku źródłowego i użyj portalu Azure, aby rozwiązać problem.

## <a name="submit-a-data-lake-analytics-job"></a>Przesyłanie zadania usługi Data Lake Analytics

Prześlij zadanie U-SQL:

```
@searchlog =
   EXTRACT UserId          int,
           Start           DateTime,
           Region          string,
           Query           string,
           Duration        int?,
           Urls            string,
           ClickedUrls     string
   FROM "/Samples/Data/SearchLog.tsv1"
   USING Extractors.Tsv();

OUTPUT @searchlog   
   TO "/output/SearchLog-from-adls.csv"
   USING Outputters.Csv();
```
    
Plik źródłowy zdefiniowane w skrypcie jest **/Samples/Data/SearchLog.tsv1**, gdzie powinien być **/Samples/Data/SearchLog.tsv**.


## <a name="troubleshoot-the-job"></a>Rozwiązywanie problemów z zadania

**Aby wyświetlić wszystkie zadania**

1. W portalu Azure kliknij **Microsoft Azure** w lewym górnym rogu.
2. Kliknij kafelek z nazwą konta usługi Data Lake Analytics.  Zadanie podsumowania jest wyświetlany na **zadania zarządzania** kafelka.

    ![Zarządzanie zadaniami usługi Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    Zadania zarządzania zapewnia podstawowe informacje o stanie zadania. Należy zauważyć, że zadania nie powiodło się.
3. Kliknij przycisk **zadania zarządzania** Kafelek, aby wyświetlić zadania. Zadania są podzielone na **systemem**, **w kolejce**, i **zakończone**. Zostanie wyświetlona zadania nie powiodło się **zakończone** sekcji. Jest ona pierwsza z nich na liście. Jeśli masz wiele zadań, można kliknąć **filtru** ułatwiające można znaleźć zadania.

    ![Usługa Azure Data Lake Analytics filtrowania zadań](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. Kliknij zadanie zakończone niepowodzeniem z listy, aby otworzyć szczegóły zadania:

    ![Azure Data Lake Analytics zadania nie powiodło się](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Powiadomienie **ponownie prześlij** przycisku. Po rozwiązaniu problemu należy ponownie przesłać zadania.
5. Kliknij część wyróżnione na poprzednim zrzucie ekranu, aby otworzyć szczegóły błędu.  Zostanie wyświetlona wyglądać mniej więcej tak:

    ![Azure Data Lake Analytics szczegóły zadania nie powiodło się](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Określa, że nie znaleziono folderu źródłowego.
6. Kliknij przycisk **zduplikowane skryptu**.
7. Aktualizacja **FROM** ścieżkę do:

    "/ Samples/Data/SearchLog.tsv"
8. Kliknij przycisk **Prześlij zadanie**.

## <a name="see-also"></a>Zobacz także
* [Omówienie programu Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Wprowadzenie do usługi Azure Data Lake Analytics przy użyciu programu Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Rozpoczynanie pracy z usługą Azure Data Lake Analytics i języka U-SQL przy użyciu programu Visual Studio](data-lake-analytics-u-sql-get-started.md)
* [Zarządzanie usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-manage-use-portal.md)
