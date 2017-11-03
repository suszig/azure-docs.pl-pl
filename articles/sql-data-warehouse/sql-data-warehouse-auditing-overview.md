---
title: "Inspekcji w usłudze Azure SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do inspekcji w usłudze Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 0e6af148-b218-4b43-bb5f-907917d20330
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 08/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: f851c82ebeaa647f663d499a4d327c3479e36121
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Inspekcja w magazynie danych Azure SQL
> [!div class="op_single_selector"]
> * [Inspekcja](sql-data-warehouse-auditing-overview.md)
> * [Wykrywanie zagrożeń](sql-data-warehouse-security-threat-detection.md)
> 
> 

Inspekcja SQL Data Warehouse pozwala do rekordu dziennika zdarzeń w bazie danych inspekcji na koncie magazynu Azure. Inspekcja pomaga zachować zgodność z przepisami, analizować aktywność bazy danych oraz uzyskać wgląd w odchylenia i anomalie, które mogą oznaczać problemy biznesowe lub podejrzane naruszenia zabezpieczeń. Inspekcja SQL Data Warehouse integruje się również z usługi Microsoft Power BI do przechodzenia raportowania i analiz.

Narzędzia inspekcji włączyć i ułatwienia przestrzeganie standardów zgodności, ale nie gwarantuje się zgodności. Aby uzyskać więcej informacji na temat usługi Azure programy tego zgodność ze standardami pomocy technicznej, zobacz <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Centrum zaufania Azure</a>.

* [Podstawowe informacje dotyczące bazy danych inspekcji]
* [Inspekcja bazy danych]
* [Analizowanie dzienników inspekcji i raportów]

## <a id="subheading-1"></a>Podstawy inspekcja bazy danych magazynu danych SQL Azure
Inspekcja bazy danych SQL Data Warehouse umożliwia:

* **Zachowaj** dziennik inspekcji wybranych zdarzeń. Można zdefiniować kategorie działań przeprowadzać inspekcję bazy danych.
* **Raport** na działanie bazy danych. Wstępnie skonfigurowane raporty i pulpit nawigacyjny umożliwia szybkie rozpoczęcie pracy z aktywności i raportowanie zdarzeń.
* **Analizowanie** raportów. Można znaleźć podejrzane zdarzenia, nietypowe działania i trendów.

Można skonfigurować inspekcji w ramach następujących kategorii zdarzeń:

**Zwykły SQL** i **sparametryzowana SQL** dla którego dzienników inspekcji zbierane są sklasyfikowane jako  

* **Dostęp do danych**
* **Zmiany schematu (DDL)**
* **Zmiany danych (DML)**
* **Konta, role i uprawnienia (DCL)**
* **Procedura składowana**, **logowania** i **zarządzania transakcji**.

Dla każdej kategorii zdarzenia inspekcji z **Powodzenie** i **błąd** operacje nie zostały skonfigurowane osobno.

Aby uzyskać więcej informacji dotyczących działań i zdarzeń inspekcji, zobacz <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">odwołanie do formatu dziennika inspekcji (Pobieranie pliku doc)</a>.

Dzienniki inspekcji są przechowywane na koncie magazynu Azure. Można zdefiniować okres przechowywania dziennika inspekcji.

Zasady inspekcji mogą być definiowane dla określonej bazy danych lub jako domyślne zasady serwera. Domyślne zasady inspekcji serwera ma zastosowanie do wszystkich baz danych na serwerze, które nie mają określonej bazy danych inspekcji zasad zdefiniowane.

Przed przystąpieniem do ustawiania inspekcji inspekcji wyboru, jeśli używasz ["Klientów niższych poziomów."](sql-data-warehouse-auditing-downlevel-clients.md)

## <a id="subheading-2"></a>Inspekcja bazy danych
1. Uruchom <a href="https://portal.azure.com" target="_blank">portalu Azure</a>.
2. Przejdź do **ustawienia** bloku inspekcji ma usługi SQL Data Warehouse. W **ustawienia** bloku, wybierz opcję **Inspekcja i wykrywanie zagrożeń**.
   
    ![][1]
3. Należy również włączyć inspekcję, klikając **ON** przycisku.
   
    ![][3]
4. W bloku inspekcji konfiguracji, wybierz **szczegóły MAGAZYNU** aby otworzyć blok magazyn dzienników inspekcji. Wybierz konto magazynu Azure, w którym zostanie zapisany dzienniki i okres przechowywania. 
>[!TIP]
>Użyj tego samego konta magazynu dla wszystkich baz danych inspekcji na maksymalne wykorzystanie szablonów wstępnie skonfigurowane raporty.
   
    ![][4]
5. Kliknij przycisk **OK** przycisk, aby zapisać konfigurację szczegóły magazynu.
6. W obszarze **rejestrowanie przez zdarzenie**, kliknij przycisk **Powodzenie** i **błąd** do rejestrowania wszystkich zdarzeń lub wybierz kategorie poszczególnych zdarzeń.
7. W przypadku konfigurowania inspekcji bazy danych, może być konieczne zmienić parametry połączenia klientowi upewnij się, że poprawnie przechwycone dane inspekcji. Sprawdź [zmodyfikować FDQN serwera w parametrach połączenia](sql-data-warehouse-auditing-downlevel-clients.md) tematu dla połączeń klientów niższych poziomów.
8. Kliknij przycisk **OK**.

## <a id="subheading-3"></a>Analizowanie dzienników inspekcji i raportów
Dzienniki inspekcji są agregowane w zbiorze magazynu tabel z **SQLDBAuditLogs** prefiks na koncie magazynu Azure wybrana w Instalatorze. Można wyświetlić przy użyciu narzędzia, takie jak pliki dziennika <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Eksploratora usługi Storage Azure</a>.

Szablon raportu dotyczącego wstępnie skonfigurowane pulpit nawigacyjny jest dostępny jako <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">arkusz kalkulacyjny programu Excel do pobrania</a> ułatwiają szybkie analizowanie danych dziennika. Aby użyć szablonu w dziennikach inspekcji, należy Excel 2013 lub nowszy i dodatku Power Query, który można pobrać <a href="http://www.microsoft.com/download/details.aspx?id=39379">tutaj</a>.

Szablon ma fikcyjnej przykładowe dane w nim, a dodatku Power Query można skonfigurować do zaimportowania dziennik inspekcji bezpośrednio z kontem magazynu platformy Azure.

## <a id="subheading-4"></a>Ponowne generowanie klucza magazynu
W środowisku produkcyjnym najprawdopodobniej będzie okresowo Odśwież kluczy magazynu. Podczas odświeżania kluczy, należy zapisać zasady. Proces przebiega w następujący sposób:

1. W bloku konfiguracji inspekcji (opisany wyżej w ustawieniach inspekcji sekcji) Przełącz **klucz dostępu do magazynu** z *głównej* do *dodatkowej* i **ZAPISAĆ**.

   ![][4]
2. Przejdź do bloku konfiguracji magazynu i **ponownie wygenerować** *podstawowy klucz dostępu*.
3. Wróć do bloku konfiguracji inspekcji, Przełącz **klucz dostępu do magazynu** z *dodatkowej* do *głównej* i naciśnij klawisz **ZAPISAĆ**.
4. Wróć do magazynu interfejsu użytkownika i **ponownie wygenerować** *pomocniczy klucz dostępu* (jako przygotowania do następnej klucze odświeżania w tle.

## <a id="subheading-5"></a>Automatyzacja (programu PowerShell/REST API)
Można również skonfigurować inspekcji w usłudze Azure SQL Data Warehouse przy użyciu następujących narzędzi automatyzacji:

* **Polecenia cmdlet programu PowerShell**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Usuń AzureRMSqlDatabaseAuditing][103]
   * [Usuń AzureRMSqlServerAuditing][104]
   * [Zestaw AzureRMSqlDatabaseAuditingPolicy][105]
   * [Zestaw AzureRMSqlServerAuditingPolicy][106]
   * [Użyj AzureRMSqlServerAuditingPolicy][107]

<!--Anchors-->
[Podstawowe informacje dotyczące bazy danych inspekcji]: #subheading-1
[Inspekcja bazy danych]: #subheading-2
[Analizowanie dzienników inspekcji i raportów]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy