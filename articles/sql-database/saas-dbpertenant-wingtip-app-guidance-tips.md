---
title: "Wskazówki dotyczące na przykład wielodostępnych aplikacji bazy danych SQL — Wingtip SaaS | Dokumentacja firmy Microsoft"
description: "Zawiera wskazówki i kroki dotyczące instalowania i uruchamiania przykładowej aplikacji wielodostępnych, która używa usługi Azure SQL Database w przykładzie Wingtip SaaS."
keywords: "samouczek usługi sql database"
services: sql-database
author: ajlam
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: andrela
ms.openlocfilehash: f7ba0569a0ede80a6579ccf3f30efb1997d4ad90
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="guidance-and-tips-for-azure-sql-database-multi-tenant-saas-app-example"></a>Wskazówki i porady dotyczące przykład aplikacji SaaS wielodostępne bazy danych SQL Azure


## <a name="download-and-unblock-the-wingtip-tickets-saas-database-per-tenant-scripts"></a>Pobierz i odblokować Wingtip biletów SaaS bazę danych na skryptów dzierżawy

Zawartość pliku wykonywalnego (skrypty, biblioteki dll) mogą być blokowane przez system Windows, gdy pliki zip są pobierane z zewnętrznego źródła i wyodrębnione. Podczas wyodrębniania pliku zip skrypty ***wykonaj poniższe kroki, aby odblokować plik zip przed wyodrębniania***. Dzięki temu można uruchamiać skrypty.

1. Przejdź do [Wingtip biletów SaaS bazy danych dla repozytorium GitHub dzierżawy](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant).
2. Kliknij przycisk **klonowania lub pobierania**.
3. Kliknij przycisk **Pobierz ZIP** i Zapisz plik.
4. Kliknij prawym przyciskiem myszy **WingtipTicketsSaaS-DbPerTenant-master.zip** pliku, a następnie wybierz **właściwości**.
5. Na **ogólne** wybierz opcję **Odblokuj**.
6. Kliknij przycisk **OK**.
7. Wyodrębnij pliki.

Skrypty znajdują się w *... \\Modułów uczenia* folderu.


## <a name="working-with-the-wingtip-tickets-saas-database-per-tenant-powershell-scripts"></a>Baza danych SaaS biletów Wingtip na dzierżawy skryptów programu PowerShell

Maksymalne wykorzystanie próbki należy przejść do podanego skryptów. Użyj punktów przerwania i krok za pomocą skryptów, sprawdzając szczegóły dotyczące implementowania różnych wzorców SaaS. Aby łatwo krok po kroku podany skryptów i modułów do zrozumienia najlepsze, zaleca się używanie [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-the-configuration-file-for-your-deployment"></a>Aktualizuj plik konfiguracji wdrożenia

Edytuj **UserConfig.psm1** pliku z zasobów grup i użytkowników zostanie ustawiona wartość podczas wdrażania:

1. Otwórz *PowerShell ISE* i obciążenia... \\Modułów szkoleniowych\\*UserConfig.psm1* 
2. Aktualizacja *ResourceGroupName* i *nazwa* z określonymi wartościami dla danego wdrożenia (wierszach 10 i 11 tylko).
3. Zapisać zmiany!

Ustawienie tych wartości w tym miejscu po prostu zapewnia trzeba zaktualizować te wartości specyficzne dla wdrażania wszystkie skrypty.

### <a name="execute-scripts-by-pressing-f5"></a>Wykonywanie skryptów poprzez naciśnięcie klawisza F5

Użyj skryptów kilka *$PSScriptRoot* do przeglądania folderów, i *$PSScriptRoot* jest oceniana tylko wtedy, gdy skryptów są wykonywane przez naciśnięcie przycisku **F5**.  Wyróżnianie i uruchamianie zaznaczenia (**F8**) może powodować błędy, więc naciśnij **F5** podczas uruchamiania skryptów.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Przechodzenie przez skrypty w celu zapoznania się z implementacją

Najlepszy sposób, aby zrozumieć skryptów jest krokowe je, aby zobaczyć, co zrobić. Zapoznaj się z dołączonej **pokaz -** skrypty, które stanowią czytelna ogólny przepływ pracy. **Pokaz -** skrypty Pokaż kroki wymagane do wykonywania poszczególnych zadań, a więc Ustaw punkty przerwania i przejść głębiej do poszczególnych wywołań, aby wyświetlić szczegóły implementacji dla różnych wzorców SaaS.

Wskazówki dotyczące eksplorowania i krokowe wykonywanie skryptów programu PowerShell:

- Otwórz **pokaz -** skryptów w programie PowerShell ISE.
- Wykonanie lub kontynuować **F5** (przy użyciu **F8** nie jest zalecana, ponieważ *$PSScriptRoot* nie jest oceniany, podczas uruchamiania opcji skryptu).
- Umieść punkty przerwania, klikając lub zaznaczając wiersz i naciskając klawisz **F9**.
- Pomiń wywołanie funkcji lub skryptu, używając klawisza **F10**.
- Wejdź do wywoływanej funkcji lub skryptu, używając klawisza **F11**.
- Wyjdź z bieżącego wywołania funkcji lub skryptu, używając kombinacji klawiszy **Shift + F11**.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Badanie schematu bazy danych i wykonywanie zapytań SQL za pomocą aplikacji SSMS

Użyj [programu SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) połączenia i przeglądanie serwerów aplikacji i baz danych.

Wdrożenie początkowo ma dwa serwery bazy danych SQL, aby nawiązać połączenie — *tenants1-dpt -&lt;użytkownika&gt;*  serwera i *katalogu-dpt -&lt;użytkownika&gt;* serwera. W celu zapewnienia połączenia pomyślne demonstracyjnej, oba serwery mają [reguły zapory](sql-database-firewall-configure.md) stosowanie wszystkich adresów IP za pośrednictwem.


1. Otwórz *SSMS* i połącz się z *tenants1-dpt -&lt;użytkownika&gt;. database.windows.net* serwera.
2. Kliknij kolejno opcje **Połącz** > **Aparat bazy danych...** :

   ![serwer wykazu](media/saas-dbpertenant-wingtip-app-guidance-tips/connect.png)

3. W wersji demonstracyjnej są używane następujące poświadczenia: Nazwa logowania = *developer*, hasło =*P@ssword1*

   ![połączenie](media/saas-dbpertenant-wingtip-app-guidance-tips/tenants1-connect.png)

4. Powtórz kroki 2 i 3 i połącz się z *katalogu-dpt -&lt;użytkownika&gt;. database.windows.net* serwera.


Po pomyślnym nawiązaniu połączenia powinny być widoczne oba serwery. Listy baz danych może być różne w zależności od dzierżawcy, które zostały udostępnione.

![eksplorator obiektów](media/saas-dbpertenant-wingtip-app-guidance-tips/object-explorer.png)



## <a name="next-steps"></a>Następne kroki

[Wdrażania bazy danych SaaS biletów Wingtip każdej dzierżawy aplikacji](saas-dbpertenant-get-started-deploy.md)

