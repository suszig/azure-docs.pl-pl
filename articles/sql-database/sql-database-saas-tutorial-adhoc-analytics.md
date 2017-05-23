---
title: "Uruchamianie analitycznych zapytań ad-hoc w wielu bazach danych Azure SQL | Microsoft Docs"
description: "Uruchamianie analitycznych zapytań ad-hoc w wielu bazach danych w aplikacji z wieloma dzierżawami"
keywords: "samouczek usługi sql database"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: dd41e7f1f131f6c18e03d2434982c3d681342b8b
ms.contentlocale: pl-pl
ms.lasthandoff: 05/12/2017


---
# <a name="run-ad-hoc-analytics-queries-across-all-wtp-tenants"></a>Uruchamianie analitycznych zapytań ad hoc dla wszystkich dzierżaw aplikacji WTP

W tym samouczku utworzysz analityczną bazę danych ad hoc i uruchomisz kilka zapytań dla wszystkich dzierżaw. Te zapytania mogą wyodrębnić szczegółowe informacje ukryte w codziennych danych operacyjnych aplikacji WTP.

Aby uruchomić analityczne zapytania ad hoc (za pośrednictwem wielu dzierżaw), aplikacja WTP korzysta z [elastycznego zapytania](sql-database-elastic-query-overview.md) wraz z analityczną bazą danych.


Niniejszy samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Wdrażania analitycznej bazy danych ad hoc
> * Uruchamiania rozproszonych zapytań dla wszystkich baz danych dzierżawy



Do wykonania kroków tego samouczka niezbędne jest spełnienie następujących wymagań wstępnych:

* Jest wdrożona aplikacja WTP. Aby wdrożyć w mniej niż pięć minut, zobacz [Deploy and explore the WTP SaaS application](sql-database-saas-tutorial.md) (Wdrażanie i badanie aplikacji SaaS WTP)
* Jest zainstalowany program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)


## <a name="ad-hoc-analytics-pattern"></a>Wzorzec analizy ad-hoc

Jedną z najważniejszych możliwości aplikacji SaaS jest używanie dużych ilości danych dzierżawy centralnie przechowywanych w chmurze. Tych danych można używać do uzyskania szczegółowych informacji o operacjach i użyciu Twoich aplikacji, Twoich dzierżawach, ich użytkownikach oraz ich preferencjach i zachowaniach. Szczegółowe informacje, które możesz znaleźć, mogą ukierunkować rozwój funkcji, usprawnienia w zakresie użyteczności oraz inne inwestycje w aplikacje i usługi.

Uzyskiwanie dostępu do tych danych w jednej wielodostępnej bazie danych jest łatwe, ale nie jest tak proste, gdy są one znacznie rozproszone, potencjalnie nawet na tysiące baz danych. Jednym z podejść jest użycie elastycznego zapytania, które umożliwia wysyłanie zapytań ad hoc do rozproszonego zestawu baz danych za pomocą wspólnego schematu. Elastyczne zapytanie używa pojedynczej *głównej* bazy danych, w której są określone tabele zewnętrzne odzwierciedlające tabele w rozproszonych bazach danych (dzierżawy). Zapytania skierowane do tej głównej bazy danych są kompilowane w celu utworzenia planu zapytania rozproszonego, gdzie części zapytania są w razie potrzeby wypychane do baz danych dzierżawy. Elastyczne zapytanie używa podzielonej na fragmenty mapy w bazie danych wykazu, aby udostępnić lokalizację baz danych dzierżawy. Konfiguracja i zapytanie są proste przy użyciu normalnego języka T-SQL i obsługują zapytania ad hoc z takich narzędzi, jak usługa Power BI i program Excel.

## <a name="get-the-wingtip-application-scripts"></a>Pobieranie skryptów aplikacji Wingtip

Skrypty i kod źródłowy aplikacji Wingtip są dostępne w repozytorium GitHub [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Pliki skryptów znajdują się w [folderze Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Pobierz folder **Learning Modules** na komputer lokalny, zachowując jego strukturę folderów.

## <a name="deploy-the-database-used-for-ad-hoc-analytics-queries"></a>Wdrażanie bazy danych używanej do analitycznych zapytań ad hoc

To ćwiczenie wdraża analityczną bazę danych ad hoc, która zawiera schemat używany podczas wysyłania zapytań ad hoc obejmujących wszystkie bazy danych dzierżawy.

1. Otwórz plik ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalytics.ps1* w środowisku *PowerShell ISE* i ustaw następujące wartości:
   * **$DemoScenario** = 2, **Deploy Ad-hoc analytics database**.

1. Naciśnij klawisz **F5**, aby uruchomić skrypt, utwórz nową bazę danych SQL dla analizy ad hoc i dodaj ją do wykazu aplikacji WTP. Tabele ZakupyBiletów, Bilety i Lokalizacje są dodawane jako tabele zewnętrzne, do których możemy wysyłać zapytania.
   Sytuacja jest prawidłowa, jeśli wystąpią tutaj ostrzeżenia dotyczące *niedostępności serwera RPC*.


Masz teraz bazę danych *adhocanalytics*, która może służyć do uruchamiania rozproszonych zapytań i zbierania szczegółowych informacji ze wszystkich dzierżaw!

## <a name="run-ad-hoc-analytics-queries"></a>Uruchamianie analitycznych zapytań ad-hoc

To ćwiczenie powoduje uruchomienie analitycznych zapytań ad hoc w celu ujawnienia szczegółowych informacji o dzierżawie z aplikacji WTP.

1. Otwórz plik ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalyticsQueries.sql* w narzędziu SSMS.
1. Upewnij się, że istnieje połączenie z bazą danych **adhocanalytics**.
1. Wybierz oddzielne zapytanie, które chcesz uruchomić, a następnie naciśnij klawisz **F5**:

    ![query](media/sql-database-saas-tutorial-adhoc-analytics/query.png)




## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Wdrażania analitycznej bazy danych ad hoc
> * Uruchamiania rozproszonych zapytań dla wszystkich baz danych dzierżawy

[Samouczek usługi Log Analytics (OMS)](sql-database-saas-tutorial-log-analytics.md)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dodatkowe samouczki, które są oparte na początkowym wdrożeniu aplikacji platformy Wingtip (WTP)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Elastyczne zapytanie](sql-database-elastic-query-overview.md)

