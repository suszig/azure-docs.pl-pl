---
title: Wprowadzenie do synchronizacji danych SQL Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft
description: "Ten samouczek ułatwia szybkie wprowadzenie do synchronizacji danych SQL Azure (wersja zapoznawcza)"
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: a295a768-7ff2-4a86-a253-0090281c8efa
ms.service: sql-database
ms.custom: load & move data
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: douglasl
ms.reviewer: douglasl
ms.openlocfilehash: ddcf6868a0fca88a52774e20623d25de31c063bb
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/08/2017
---
# <a name="get-started-with-azure-sql-data-sync-preview"></a>Wprowadzenie do synchronizacji danych SQL Azure (wersja zapoznawcza)
Z tego samouczka dowiesz się sposobu konfigurowania synchronizacji danych SQL Azure, tworząc grupy synchronizacji hybrydowych, zawierającej wystąpienia zarówno usługi Azure SQL Database i programu SQL Server. Nowa grupa synchronizacji jest w pełni skonfigurowane i synchronizuje się zgodnie z harmonogramem, które można ustawić.

W tym samouczku założono, że co najmniej pewne doświadczenie z bazy danych SQL i programu SQL Server. 

Omówienie synchronizacji danych SQL, zobacz [synchronizacji danych między wieloma bazami danych chmury i lokalnych z synchronizacji danych SQL Azure (wersja zapoznawcza)](sql-database-sync-data.md).

Aby uzyskać pełną przykładów programu PowerShell, które przedstawiają sposób konfigurowania synchronizacji danych SQL, zobacz następujące artykuły:
-   [Synchronizacja między wiele baz danych Azure SQL przy użyciu programu PowerShell](scripts/sql-database-sync-data-between-sql-databases.md)
-   [Synchronizacja między bazą danych SQL Azure i lokalnej bazy danych programu SQL Server przy użyciu programu PowerShell](scripts/sql-database-sync-data-between-azure-onprem.md)

## <a name="step-1---create-sync-group"></a>Krok 1 — Tworzenie grupy synchronizacji

### <a name="locate-the-data-sync-settings"></a>Zlokalizuj ustawienia synchronizacji danych

1.  W przeglądarce przejdź do portalu Azure.

2.  W portalu Znajdź bazy danych SQL z pulpitu nawigacyjnego lub ikonę baz danych na pasku narzędzi.

    ![Lista baz danych Azure SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  Na **baz danych SQL** wybierz istniejącej bazy danych SQL, który ma być używany jako baza danych Centrum synchronizacji danych. Zostanie otwarta strona bazy danych SQL.

4.  Na stronie bazy danych SQL dla wybranej bazy danych wybierz **synchronizacji do innych baz danych**. Zostanie otwarta strona synchronizacji danych.

    ![Synchronizowanie innych opcji bazy danych](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Utwórz nową grupę synchronizacji

1.  Na stronie synchronizacja danych wybierz **nowej grupy synchronizacji**. **Nowej grupy synchronizacji** z kroku 1, zostanie otwarta strona **Utwórz grupę synchronizacji**, zaznaczony. **Tworzenie grupy synchronizacji danych** zostanie otwarta strona również.

2.  Na **Tworzenie grupy synchronizacji danych** wykonaj następujące czynności:

    1.  W **nazwy grupy synchronizacji** wprowadź nazwę nowej grupy synchronizacji.

    2.  W **bazy danych usługi synchronizacji metadanych** sekcji wybierz, czy można utworzyć nową bazę danych (zalecane) lub użyj istniejącej bazy danych.

        > [!NOTE]
        > Firma Microsoft zaleca, aby utworzyć nową, pustą bazę danych do użycia jako bazy danych usługi synchronizacji metadanych. Synchronizacja danych tworzy tabele w tej bazie danych i uruchamia częste obciążenia. Ta baza danych jest automatycznie udostępniony jako bazy danych usługi synchronizacji metadanych dla wszystkich grup synchronizacji w wybranym regionie. Nie można zmienić bazy danych usługi synchronizacji metadanych lub jego nazwa bez porzuceniem jej.

        Jeśli została wybrana opcja **nową bazę danych**, wybierz pozycję **Utwórz nową bazę danych.** **Bazy danych SQL** zostanie otwarta strona. Na **bazy danych SQL** strony, nazwy i skonfiguruj nową bazę danych. Następnie wybierz **OK**.

        Jeśli została wybrana opcja **Użyj istniejącej bazy danych**, wybierz bazę danych z listy.

    3.  W **synchronizacji automatycznej** najpierw wybierz **na** lub **poza**.

        Jeśli została wybrana opcja **na**w **częstotliwości synchronizacji** , wprowadź liczbę z zakresu a następnie wybierz opcję sekundy, minuty, godziny lub dni.

        ![Określ częstotliwość synchronizacji](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  W **rozwiązywania konfliktów** wybierz "Centrum wins" lub "Elementu członkowskiego wins".

        ![Określ, jak są rozwiązywane konflikty](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  Wybierz **OK** i poczekaj, aż nową grupę synchronizacji, aby utworzyć i wdrożyć.

## <a name="step-2---add-sync-members"></a>Krok 2 — Dodawanie członków synchronizacji

Po utworzeniu i wdrożeniu, krok 2 nowej grupy synchronizacji **dodawać członków synchronizacji**, zostanie wyróżniona w **nowej grupy synchronizacji** strony.

W **bazy danych Centrum** wprowadź istniejących poświadczeń dla serwera bazy danych SQL, na którym znajduje się baza danych Centrum. Nie wprowadzaj *nowe* poświadczeń w tej sekcji.

![Koncentrator bazy danych został dodany do grupy synchronizacji](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

### <a name="add-an-azure-sql-database"></a>Dodaj bazę danych Azure SQL

W **bazy danych elementów członkowskich** sekcji i opcjonalnie Dodaj do grupy synchronizacji bazy danych SQL Azure, wybierając **dodać bazy danych Azure**. **Konfigurowanie bazy danych Azure** zostanie otwarta strona.

Na **Konfigurowanie bazy danych Azure** wykonaj następujące czynności:

1.  W **nazwa elementu członkowskiego synchronizacji** Podaj nazwę nowego elementu synchronizacji. Ta nazwa różni się od nazwy bazy danych.

2.  W **subskrypcji** wybierz skojarzona subskrypcja platformy Azure na potrzeby rozliczeń.

3.  W **Azure SQL Server** wybierz istniejącego serwera baz danych SQL.

4.  W **bazy danych SQL Azure** wybierz istniejącą bazę danych SQL.

5.  W **kierunki synchronizacji** pola, wybierz opcję synchronizacji dwukierunkowe, do koncentratora lub z Centrum.

    ![Dodawanie nowego elementu członkowskiego synchronizacji bazy danych SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  W **Username** i **hasło** wprowadź istniejących poświadczeń dla serwera bazy danych SQL, na którym znajduje się bazy danych elementów członkowskich. Nie wprowadzaj *nowe* poświadczeń w tej sekcji.

7.  Wybierz **OK** i poczekaj, aż nowy element członkowski synchronizacji utworzyć i wdrożyć.

    ![Dodano nowy element członkowski synchronizacji bazy danych SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

### <a name="add-an-on-premises-sql-server-database"></a>Dodaj lokalną bazą danych programu SQL Server

W **bazy danych elementów członkowskich** sekcji i opcjonalnie Dodaj lokalny serwer SQL do grupy synchronizacji, wybierając **Dodaj bazę danych z lokalnego**. **Konfigurowanie lokalnego** zostanie otwarta strona.

Na **Konfigurowanie lokalnego** wykonaj następujące czynności:

1.  Wybierz **wybierz bramę Agent synchronizacji**. **Agent synchronizacji wybierz** zostanie otwarta strona.

    ![Wybierz bramę agent synchronizacji](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  Na **wybierz bramę Agent synchronizacji** wybierz, czy użyć istniejącego agenta lub utworzyć nowy agent.

    Jeśli została wybrana opcja **agentów istniejące**, wybierz z listy istniejącego agenta.

    Jeśli została wybrana opcja **Utwórz nowy agent**, wykonaj następujące czynności:

    1.  Pobierz oprogramowanie klienckie synchronizacji agenta z linku podanego i zainstaluj go na komputerze, na którym znajduje się serwer SQL.
 
        > [!IMPORTANT]
        > Należy otworzyć w zaporze, aby umożliwić agenta klienta komunikacji z serwerem wychodzący port TCP 1433.


    2.  Wprowadź nazwę dla agenta.

    3.  Wybierz **tworzenie i generowanie klucza**.

    4.  Klucz agenta należy skopiować do Schowka.
        
        ![Tworzenie nowego agenta synchronizacji](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  Wybierz **OK** zamknąć **Agent synchronizacji wybierz** strony.

    6.  Na komputerze serwera SQL Znajdź i uruchom aplikację klienta synchronizacji agenta.

        ![Aplikacja kliencka agenta synchronizacji danych](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  W aplikacji agenta synchronizacji, wybierz **przesłać klucz agenta**. **Synchronizacji metadanych bazy danych konfiguracji** zostanie otwarte okno dialogowe.

    8.  W **synchronizacji metadanych bazy danych konfiguracji** okno dialogowe, Wklej klucz agenta skopiowany z portalu Azure. Udostępniają istniejących poświadczeń dla serwera bazy danych SQL Azure, na którym znajduje się baza danych metadanych. (Jeśli została utworzona nowa baza danych metadanych, ta baza danych jest na tym samym serwerze co baza danych Centrum). Wybierz **OK** i poczekaj, aż do zakończenia konfiguracji.

        ![Wprowadź poświadczenia agenta klucza i serwera](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   Jeśli w tym momencie zostanie wyświetlony błąd zapory, należy utworzyć regułę zapory na platformie Azure, aby zezwolić na ruch przychodzący z komputera programu SQL Server. Regułę można utworzyć ręcznie w portalu, ale użytkownik może ułatwić ją utworzyć w programu SQL Server Management Studio (SSMS). W programie SSMS próby nawiązania połączenia z bazą danych Centrum na platformie Azure. Wpisz jej nazwę jako \<hub_database_name\>. database.windows.net. Aby skonfigurować regułę zapory platformy Azure, wykonaj czynności opisane w oknie dialogowym. Następnie wróć do aplikacji agenta klienta synchronizacji.

    9.  W aplikacji klienta synchronizacji agenta, kliknij przycisk **zarejestrować** zarejestrować bazy danych programu SQL Server z agentem. **Konfiguracji serwera SQL** zostanie otwarte okno dialogowe.

        ![Dodaj i skonfiguruj bazę danych programu SQL Server](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. W **konfiguracji serwera SQL** oknie dialogowym Wybierz, czy nawiązać połączenie przy użyciu uwierzytelniania programu SQL Server lub uwierzytelniania systemu Windows. Jeśli wybrano opcję uwierzytelniania programu SQL Server, wprowadź istniejących poświadczeń. Podaj nazwę serwera SQL i nazwę bazy danych, które mają być synchronizowane. Wybierz **połączenie testowe** Aby przetestować ustawienia. Następnie wybierz **zapisać**. Zarejestrowane bazy danych zostanie wyświetlony na liście.

        ![Baza danych SQL Server zostanie zarejestrowany](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. Można teraz zamknąć aplikacji agenta klienta synchronizacji.

    12. W portalu na **Konfigurowanie lokalnego** wybierz pozycję **wybierz bazę danych.** **Wybierz bazę danych** zostanie otwarta strona.

    13. Na **wybierz bazę danych** strony w **nazwa elementu członkowskiego synchronizacji** Podaj nazwę nowego elementu synchronizacji. Ta nazwa różni się od nazwy bazy danych. Wybierz bazę danych z listy. W **kierunki synchronizacji** pola, wybierz opcję synchronizacji dwukierunkowe, do koncentratora lub z Centrum.

        ![Wybierz bazę danych na lokalnym](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. Wybierz **OK** zamknąć **wybierz bazę danych** strony. Następnie wybierz **OK** zamknąć **Konfigurowanie lokalnego** strony i poczekaj, aż nowy element członkowski synchronizacji utworzyć i wdrożyć. Na koniec kliknij **OK** zamknąć **Wybierz członków synchronizacji** strony.

        ![W lokalnej bazie danych dodane do grupy synchronizacji](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  Aby połączyć się synchronizacja danych SQL i agent lokalny, Dodaj nazwę użytkownika do roli `DataSync_Executor`. Synchronizacja danych tworzy tę rolę w wystąpieniu programu SQL Server.

## <a name="step-3---configure-sync-group"></a>Krok 3 — Konfigurowanie synchronizacji grupy

Po nowych członków grupy synchronizacji są tworzone i wdrażane, krok 3 **Konfigurowanie synchronizacji grupy**, zostanie wyróżniona w **nowej grupy synchronizacji** strony.

1.  Na **tabel** , wybierz bazę danych z listy członków grupy synchronizacji, a następnie wybierz **schematu odświeżania**.

2.  Z listy dostępnych tabel wybierz tabele, które mają być synchronizowane.

    ![Wybierz tabele do synchronizacji](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  Domyślnie wybrane są wszystkie kolumny w tabeli. Jeśli nie chcesz zsynchronizować wszystkie kolumny, wyłącz pole wyboru dla kolumn, które nie mają być synchronizowane. Pamiętaj pozostawić wybrana kolumna klucza podstawowego.

    ![Wybierz pola do synchronizacji](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  Na koniec wybierz **zapisać**.

## <a name="next-steps"></a>Następne kroki
Gratulacje. Utworzono grupę synchronizacji, która zawiera zarówno wystąpienie bazy danych SQL, jak i bazy danych programu SQL Server.

Aby uzyskać więcej informacji na temat synchronizacji danych SQL zobacz:

-   [Synchronizowanie danych w wielu w chmurze i lokalnych baz danych z synchronizacji danych SQL Azure](sql-database-sync-data.md)
-   [Najlepsze rozwiązania dotyczące synchronizacji danych SQL Azure](sql-database-best-practices-data-sync.md)
-   [Rozwiązywanie problemów z synchronizacją danych SQL Azure](sql-database-troubleshoot-data-sync.md)

-   Wykonaj przykłady z programu PowerShell, które przedstawiają sposób konfigurowania synchronizacji danych SQL:
    -   [Synchronizacja między wiele baz danych Azure SQL przy użyciu programu PowerShell](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Synchronizacja między bazą danych SQL Azure i lokalnej bazy danych programu SQL Server przy użyciu programu PowerShell](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Pobrać dokumentację interfejsu API REST synchronizacji danych SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Aby uzyskać więcej informacji na temat bazy danych SQL zobacz:

-   [Omówienie bazy danych SQL](sql-database-technical-overview.md)
-   [Zarządzanie cyklem życia bazy danych](https://msdn.microsoft.com/library/jj907294.aspx)
