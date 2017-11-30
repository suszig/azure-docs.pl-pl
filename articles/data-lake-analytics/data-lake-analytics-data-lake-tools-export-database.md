---
title: "Eksportowanie bazy danych U-SQL przy użyciu usługi Azure Data Lake Tools dla programu Visual Studio | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać narzędzi Azure Data Lake Tools dla programu Visual Studio do eksportu U-SQL bazy danych i zaimportuj go do konta lokalnego, w tym samym czasie."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: yanacai
ms.openlocfilehash: 9f11e07f7fbaf2b708d6fbc8a746238745132bda
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-export-u-sql-database"></a>Jak wyeksportować U-SQL w bazie danych

W tym dokumencie będzie Dowiedz się jak używać [Azure Data Lake Tools dla programu Visual Studio](http://aka.ms/adltoolsvs) eksportu U-SQL bazy danych jako pojedynczego skryptu U-SQL i pobranych zasobów. Zaimportowanie wyeksportowanej bazy danych do lokalnego konta jest również obsługiwany w tym samym procesie.

Klienci obsługi zwykle wiele środowisk dla rozwoju, badanie i produkcji. Tych środowisk znajdują się na konto usługi Azure Data Lake Analytics na platformie Azure i lokalnego konta na komputerze lokalnym deweloperów. Podczas projektowania i dostrajania zapytań U-SQL na środowisk projektowania i testowania, jest często, że deweloperzy konieczne ponowne utworzenie wszystkich elementów w produkcyjnej bazie danych. **Kreator eksportu bazy danych** pomaga przyspieszyć ten proces. Za pomocą kreatora, deweloperzy mogą sklonować istniejącego środowiska bazy danych i przykładowe dane do innych kont usługi Azure Data Lake Analytics.

## <a name="export-steps"></a>Kroki eksportowania

### <a name="step-1-right-click-the-database-in-server-explorer-and-click-export"></a>Krok 1: Kliknij prawym przyciskiem myszy w Eksploratorze serwera bazy danych i kliknij pozycję "Eksportuj..."

Wszystkie konta usługi Azure Data Lake Analytics, masz uprawnienia do znajdują się w Eksploratorze serwera. Rozwiń ten, który zawiera bazę danych do wyeksportowania, a następnie kliknij prawym przyciskiem myszy bazę danych, aby wybrać **Eksportuj...** . Jeśli nie możesz znaleźć menu kontekstowego, musisz [aktualizacji narzędzie do wersji najnowszą wersję apletu](http://aka.ms/adltoolsvs).

![Data Lake Analytics narzędzi eksportu bazy danych](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

### <a name="step-2-configure-the-objects-you-want-to-export"></a>Krok 2: Konfigurowanie obiektów, które mają zostać wyeksportowane

Czasami bazy danych jest duży, lecz wystarczy niewielką częścią, a następnie można skonfigurować podzbioru obiektów, który chcesz wyeksportować w Kreatorze eksportu. Należy pamiętać, że akcja eksportu jest wykonywana, uruchamiając zadanie U-SQL, a w związku z tym eksportowanie z konta Azure generuje pewien koszt związany.

![Kreator danych takie narzędzia eksportu bazy danych](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-more-configurations"></a>Krok 3: Sprawdź listę obiektów i konfiguracji więcej

W tym kroku double można sprawdzić wybranych obiektów w górnej części okna dialogowego. Jeśli występują błędy, można kliknij przycisk Wstecz, aby wrócić do poprzedniej strony i skonfiguruj obiekty, które mają zostać wyeksportowane ponownie.

Możesz również wykonać inne konfiguracje o docelowego eksportu, opisy te konfiguracje są wymienione w poniższej tabeli:

|Konfiguracja|Opis|
|-------------|-----------|
|Nazwa docelowego|Ta nazwa wskazuje, gdzie chcesz zapisać zasobów wyeksportowanej bazy danych, takich jak zestawy, dodatkowe pliki i przykładowych danych. Folder o takiej nazwie zostanie utworzony w folderze głównym Twojego danych lokalnych.|
|Katalog projektu|Ta ścieżka definiuje, gdzie chcesz zapisać wyeksportowany skryptu U-SQL, w tym wszystkich definicji obiektu bazy danych.|
|Tylko schemat|Wybranie tej opcji powoduje tylko definicje bazy danych i zasobów (takich jak zestawy i dodatkowe pliki) eksportowane.|
|Schemat i dane|Wybranie tej opcji powoduje definicje bazy danych, zasobów i danych do wyeksportowania. Top N wierszy tabel są eksportowane.|
|Automatycznie zaimportuj do lokalnej bazy danych|Sprawdzanie, czy ta konfiguracja oznacza wyeksportowanej bazy danych zostaną zaimportowane z lokalną bazą danych automatycznie po zakończeniu eksportowania.|

![Dane takie narzędzia eksportu bazy danych Kreatora konfiguracji](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>Krok 4: Sprawdzenie wyników eksportu

Po wszystkie te ustawienia i postępu eksportu wyeksportowanych wyników z okna dziennika można znaleźć w kreatorze. Za pomocą dziennika oznaczony przez czerwony prostokąt bNiski zrzucie ekranu można znaleźć lokalizacji wyeksportowanego U-SQL skryptu i bazy danych zasobów, w tym zestawów, dodatkowe pliki i przykładowych danych.

![Dane takie narzędzia eksportu bazy danych Kreatora ukończone](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="how-to-import-the-exported-database-to-local-account"></a>Jak zaimportować wyeksportowanej bazy danych do konta lokalnego

Sprawdza, czy najbardziej wygodny sposób, aby zrobić to zaimportowanie **Importuj, aby automatycznie lokalnej bazy danych** podczas eksportowanie postęp w kroku 3. Jeśli nie pamiętasz to zrobić, można znaleźć wyeksportowanego skryptu U-SQL za pomocą dziennika eksportowanie i uruchom skrypt U-SQL lokalnie, aby zaimportować bazę danych do konta lokalnego.

## <a name="how-to-import-the-exported-database-to-azure-data-lake-analytics-account"></a>Jak zaimportować wyeksportowanej bazy danych do konta usługi Azure Data Lake Analytics

Aby zaimportować bazę danych na inne konto usługi Azure Data Lake Analytics, należy wykonać dwie czynności:

1. Prześlij wyeksportowane zasoby, w tym zestawów, dodatkowe pliki i przykładowe dane do domyślnego konta usługi Azure Data Lake Store konta usługi Azure Data Lake Analytics, który chcesz zaimportować do. Znajdź folder zasobów wyeksportowanego w folderze głównym danych lokalnych i przekaż cały folder z katalogiem głównym domyślne konto magazynu.
2. Prześlij wyeksportowane skryptu U-SQL do konta usługi Azure Data Lake Analytics, które chcesz zaimportować bazę danych do po przekazaniu ukończone.

## <a name="known-limitation"></a>Znane ograniczenia

Obecnie w przypadku wybrania **schemat i dane** w Kreatorze narzędzie jest uruchamiane zadanie U-SQL, aby wyeksportować dane przechowywane w tabelach. Dlatego procesu eksportowania danych może zająć więcej czasu i ponoszenia kosztów. 

## <a name="next-steps"></a>Następne kroki

* [Zrozumienie U-SQL w bazie danych](https://msdn.microsoft.com/library/azure/mt621299.aspx) 
* [jak testowanie i debugowanie zadań U-SQL przy użyciu zestawu SDK usługi Azure Data Lake U-SQL i lokalne uruchamianie](data-lake-analytics-data-lake-tools-local-run.md)


