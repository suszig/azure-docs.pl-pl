---
title: "Usługa migracji bazy danych Azure używany do migracji migracji programu SQL Server do usługi Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Dowiedz się przeprowadzić migrację z lokalnie programu SQL Server do bazy danych SQL Azure przy użyciu usługi migracji bazy danych Azure."
services: dms
author: HJToland3
ms.author: jtoland
manager: jhubbard
ms.reviewer: 
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 11/09/2017
ms.openlocfilehash: 70127b09e64ea4f19de437297498bdf78d415b99
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="migrate-sql-server-to-azure-sql-database"></a>Migrowanie serwera SQL z bazą danych Azure SQL
Usługa Azure bazy danych migracji umożliwia migrację bazy danych z lokalnego wystąpienia programu SQL Server do bazy danych SQL Azure. W tym samouczku, wykonywana jest migracja **Adventureworks2012** przywrócone do lokalnego wystąpienia programu SQL Server 2016 (lub nowszym) z bazą danych SQL Azure za pomocą usługi Azure migracji bazy danych w bazie danych.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Oceń lokalnej bazy danych przy użyciu Asystenta migracji danych.
> * Migrowanie schematu próbki za pomocą Asystenta migracji danych.
> * Utwórz wystąpienie usługi migracji bazy danych Azure.
> * Tworzenie projektu migracji usługi migracji bazy danych Azure.
> * Uruchom migrację.
> * Monitoruj migrację.

## <a name="prerequisites"></a>Wymagania wstępne
Do ukończenia tego samouczka niezbędne są następujące elementy:

- [SQL Server 2016 lub nowszego](https://www.microsoft.com/sql-server/sql-server-downloads) (dowolna wersja)
- Protokół TCP/IP jest domyślnie wyłączona, z instalacją programu SQL Server Express. Włącz go, wykonując [instrukcje w tym artykule](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Aby skonfigurować Twoje [zapory systemu Windows dla dostępu aparatu bazy danych](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Wystąpienie bazy danych SQL Azure. Można utworzyć wystąpienia bazy danych SQL Azure, wykonując szczegółowo w artykule [tworzenie bazy danych Azure SQL w portalu Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- [Dane migracji Asystenta](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 lub nowszym.
- Usługa Azure bazy danych migracji wymaga utworzony przy użyciu modelu wdrażania usługi Azure Resource Manager, który zapewnia połączenie lokacja lokacja z serwerami lokalnymi źródła przy użyciu sieci Wirtualnej [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) lub [ Sieć VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Poświadczenia użyte do nawiązania połączenia źródła wystąpienia programu SQL Server musi mieć [serwera kontroli](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) uprawnienia.
- Poświadczenia użyte do nawiązania połączenia docelowego wystąpienia bazy danych SQL Azure wymagane uprawnienie bazy danych kontroli z docelowymi bazami danych bazy danych SQL Azure.
- Zapora systemu Windows musi być otwarty, aby umożliwić uzyskiwanie dostępu do źródła programu SQL Server z usługi migracji bazy danych Azure.

## <a name="assess-your-on-premises-database"></a>Oceny lokalnej bazy danych
Przed przeprowadzeniem migracji danych z lokalnego wystąpienia programu SQL Server z bazą danych SQL Azure, należy dokonać oceny problemy blokujące, które mogą uniemożliwić migracji bazy danych SQL Server. Po pobraniu i zainstalowaniu v3.3 Asystenta migracji danych, wykonaj czynności opisane w artykule [wykonywania oceny migracji programu SQL Server](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem) przeprowadzenie lokalnej bazy danych do oceny. Podsumowanie kroki wymagane są następujące:
1.  W Asystencie migracji danych wybierz ikonę Nowy (+), a następnie wybierz **oceny** typu projektu.
2.  Określić nazwę projektu w **typ serwera źródła** pole tekstowe, wybierz **programu SQL Server**, a następnie w **typ serwer docelowy** pole tekstowe, wybierz **Azure SQL Baza danych**.
3.  Wybierz **Utwórz** Aby utworzyć projekt.
    Oceniając źródłowej bazy danych programu SQL Server, migracja do bazy danych SQL Azure, możesz wybrać jedną lub obie następujące typy raportów oceny:
    - Sprawdzenie zgodności bazy danych
    - Sprawdzanie parzystości funkcji

    Domyślnie są zaznaczone obu typów raportów.
4.  W Asystencie migracji danych na **opcje** ekranu wybierz **dalej**.
5.  Na **Wybierz źródła** ekranu w **Połącz z serwerem** okno dialogowe, podaj szczegóły połączenia z serwerem SQL, a następnie wybierz **Connect**.
6.  Wybierz **AdventureWorks2012**, wybierz pozycję **Dodaj**, a następnie wybierz **Start oceny**.

    Po zakończeniu oceny wyniki są wyświetlane jak pokazano na poniższym rysunku:

    ![Oceny migracji danych](media\tutorial-sql-server-to-azure-sql\dma-assessments.png)

    Bazy danych SQL Azure oceny zidentyfikować problemy z blokowaniem migracji i problemów parzystość funkcji.

7.  Przejrzyj wyniki oceny do rozwiązywania problemów parzystość funkcji i problemy z blokowaniem migracji wybranie określonych opcji.
    - Kategoria parzystość funkcji programu SQL Server zapewnia rozbudowany zestaw zaleceń, alternatywnych metod dostępne w Azure i ograniczające zagrożenie kroki ułatwiają planowanie nakład pracy w projektach migracji.
    - Kategoria problemy ze zgodnością zapewnia częściowo lub nieobsługiwane funkcje odzwierciedlające problemy ze zgodnością, które mogłyby zablokować migracja lokalnych baz danych programu SQL Server do baz danych SQL Azure. Podawane są również zalecenia ułatwiające rozwiązanie tych problemów.


## <a name="migrate-the-sample-schema"></a>Migrowanie schematu próbki
Po stwierdzeniu, że wybrana baza danych odpowiednimi kandydatami do migracji do bazy danych SQL Azure, użyć Asystenta migracji danych do migracji schematu do bazy danych SQL Azure w potrafisz oceny.

> [!NOTE]
> Przed utworzeniem projektu migracji Asystenta migracji danych w, należy się upewnić, że już zapewnieniu bazy danych Azure SQL określone wymagania wstępne. Do celów tego samouczka, nazwa bazy danych SQL Azure zakłada się, że **AdventureWorks2012**, ale można określić nazwę inaczej w razie potrzeby.

Aby przeprowadzić migrację **AdventureWorks2012** schematu z bazą danych SQL Azure, wykonaj następujące czynności:

1.  Uruchamianie Asystenta migracji danych.
2.  Wybierz ikonę Nowy (+), a następnie w obszarze **typ projektu**, wybierz pozycję **migracji**.
3.  Określić nazwę projektu w **typ serwera źródła** pole tekstowe, wybierz **programu SQL Server**, a następnie w **typ serwer docelowy** pole tekstowe, wybierz **Azure SQL Baza danych**.
4.  W obszarze **zakresu migracji**, wybierz pozycję **tylko schemat**.

    Po wykonaniu poprzednich kroków, interfejs Asystenta migracji danych powinna być taka jak pokazano na poniższym rysunku:
    
    ![Utwórz projekt Asystenta migracji danych](media\tutorial-sql-server-to-azure-sql\dma-create-project.png)

5.  Wybierz **Utwórz** Aby utworzyć projekt.
6.  W Asystencie migracji danych Określ szczegóły połączenia źródła dla programu SQL Server, wybierz **Connect**, a następnie wybierz **AdventureWorks2012** bazy danych.

    ![Szczegóły połączenia źródła Asystenta migracji danych](media\tutorial-sql-server-to-azure-sql\dma-source-connect.png)
7.  Wybierz **dalej**w obszarze **Połącz z serwerem docelowym**, określ szczegóły połączenia docelowej bazy danych Azure SQL, wybierz **Connect**, a następnie wybierz **AdventureWorks2012** bazy danych można było wstępnie tworzyć w bazie danych Azure SQL.

    ![Szczegóły połączenia z docelowym Asystenta migracji danych](media\tutorial-sql-server-to-azure-sql\dma-target-connect.png)
8.  Wybierz **dalej** aby przejść do **wybierz obiekty** ekran, na którym można określić obiektów schematu w **AdventureWorks2012** bazy danych, które muszą zostać wdrożone na platformie Azure Baza danych SQL.

    Domyślnie są zaznaczone wszystkie obiekty.

    ![Generowanie skryptów SQL](media\tutorial-sql-server-to-azure-sql\dma-assessment-source.png)
9.  Wybierz **skryptu SQL Generowanie** można utworzyć skrypty SQL, a następnie przejrzyj skrypty występują błędy.

    ![Skrypt schematu](media\tutorial-sql-server-to-azure-sql\dma-schema-script.png)
10. Wybierz **schema wdrażania** można wdrożyć schematu bazy danych SQL Azure, a następnie po wdrożeniu schematu Sprawdź serwera docelowego dla wszelkich nieprawidłowości.

    ![Wdrażanie schematu](media\tutorial-sql-server-to-azure-sql\dma-schema-deploy.png)

## <a name="create-an-instance"></a>Tworzenie wystąpienia
1.  Zaloguj się do portalu Azure, wybierz opcję **+ Utwórz zasób**, wyszukaj usługi migracji bazy danych Azure, a następnie wybierz **usługi migracji bazy danych Azure** z listy rozwijanej.

    ![Azure Marketplace](media\tutorial-sql-server-to-azure-sql\portal-marketplace.png)
2.  Na **usługi migracji bazy danych Azure (wersja zapoznawcza)** ekranu wybierz **Utwórz**.
 
    ![Utwórz wystąpienie usługi migracji bazy danych Azure](media\tutorial-sql-server-to-azure-sql\dms-create.png)
  
3.  Na **bazy danych migracji usługi** ekranu, określ nazwę usługi, subskrypcja sieci wirtualnej i warstwę cenową.

    Aby uzyskać więcej informacji na kosztów i warstwy cenowej zobacz stronę cenową.

     ![Skonfiguruj ustawienia wystąpienia usługi migracji bazy danych Azure](media\tutorial-sql-server-to-azure-sql\dms-settings.png)

4.  Wybierz **Utwórz** można utworzyć usługi.

## <a name="create-a-migration-project"></a>Tworzenie projektu migracji
Po utworzeniu usługi znalezienie go w portalu Azure, a następnie utwórz projekt migracji.
1. W portalu Azure wybierz **wszystkie usługi**, wyszukaj usługi migracji bazy danych Azure, a następnie wybierz **usługi migracji bazy danych Azure**.
 
      ![Znajdź wszystkie wystąpienia usługi migracji bazy danych Azure](media\tutorial-sql-server-to-azure-sql\dms-search.png)
2. Na **usługi migracji bazy danych Azure** ekranu, wyszukaj nazwę Azure DMS wystąpienia utworzony, a następnie wybierz wystąpienie.
 
     ![Zlokalizuj Twoje wystąpienie usługi migracji bazy danych Azure](media\tutorial-sql-server-to-azure-sql\dms-instance-search.png)
 
3. Wybierz **+ nowy projekt migracji**.
4. Na **nowy projekt migracji** ekranu, należy określić nazwę projektu, w **typ serwera źródła** pole tekstowe, wybierz opcję **programu SQL Server**, a następnie w **docelowego Typ serwera** pole tekstowe, wybierz opcję **bazy danych SQL Azure**.

    ![Tworzenie projektu usługi bazy danych migracji](media\tutorial-sql-server-to-azure-sql\dms-create-project.png)

5.  Wybierz **Utwórz** Aby utworzyć projekt.


## <a name="specify-source-details"></a>Określ szczegóły źródła
1. Na **źródła szczegóły** ekranu, określ szczegóły połączenia dla źródła SQL Server.

    ![Wybierz źródło](media\tutorial-sql-server-to-azure-sql\dms-select-source.png)

2. Wybierz **zapisać**, a następnie wybierz **AdventureWorks2012** bazy danych do migracji.

    ![Wybierz źródło bazy danych](media\tutorial-sql-server-to-azure-sql\dms-select-source-db.png)

## <a name="specify-target-details"></a>Określ szczegóły docelowego
1. Wybierz **zapisać**, a następnie na **docelowe szczegóły** ekranu, określ szczegóły połączenia dla obiektu docelowego, który jest wstępnie przygotowany bazy danych SQL Azure z którym **AdventureWorks2012**  schematu została wdrożona przy użyciu Asystenta migracji danych.

    ![Wybierz element docelowy](media\tutorial-sql-server-to-azure-sql\dms-select-target.png)

2. Wybierz **zapisać** zapisać projektu.
3. Na **podsumowanie migracji** ekranu, przejrzyj i sprawdź szczegóły skojarzone z projektem migracji.

    ![Podsumowanie DMS](media\tutorial-sql-server-to-azure-sql\dms-summary.png)

4. Wybierz pozycję **Zapisz**.

## <a name="run-the-migration"></a>Uruchom migrację
1.  Wybierz ostatnio zapisany projekt, wybierz pozycję **+ nowe działanie**, a następnie wybierz **Uruchom migrację danych**.

    ![Nowe działanie](media\tutorial-sql-server-to-azure-sql\dms-new-activity.png)

2.  Po wyświetleniu monitu wprowadź poświadczenia dla źródła i serwerów docelowych, a następnie wybierz **zapisać**.
3.  Na **mapy w celu docelowymi bazami danych** ekranu, mapy źródłowej i docelowej bazy danych do migracji.

    Jeśli docelowa baza danych zawiera nazwę bazy danych jako źródłowej bazy danych, Azure DMS domyślnie wybiera docelowej bazy danych.

    ![Mapowanie do docelowej bazy danych](media\tutorial-sql-server-to-azure-sql\dms-map-targets-activity.png)

4. Wybierz **zapisać**na **wybierz tabele** ekranu, rozwiń pozycję tabeli i zapoznaj się z listą odpowiednich pól.

    ![Wybór tabel](media\tutorial-sql-server-to-azure-sql\dms-configure-setting-activity.png)

5.  Wybierz **zapisać**na **podsumowanie migracji** ekranu w **Nazwa działania** tekst Określ nazwę działania migracji.

    Na tym ekranie można również rozwinąć **wybierz opcję sprawdzania poprawności** ekranu, w którym można określić, czy sprawdzanie poprawności migrowanych bazy danych dla:
    - Schemat
    - Spójność danych
    - Poprawność zapytania i wydajności

    ![Wybierz opcję sprawdzania poprawności](media\tutorial-sql-server-to-azure-sql\dms-configuration.png)

6.  Wybierz **zapisać**, zapoznaj się z podsumowaniem, aby upewnić się, że szczegóły źródłowa i docelowa są zgodne co wcześniej określona.

    ![Podsumowanie migracji](media\tutorial-sql-server-to-azure-sql\dms-run-migration.png)

7.  Wybierz **uruchamiania migracji** do uruchomienia działania migracji, a następnie wybierz **Odśwież** informacje dotyczące bieżącego stanu.

    ![Stan działania](media\tutorial-sql-server-to-azure-sql\dms-activity-status.png)

## <a name="monitor-the-migration"></a>Monitoruj migrację
1. Wybierz działanie migracji, aby sprawdzić stan działania.
2. Sprawdź docelowa baza danych Azure SQL po zakończeniu migracji.

    ![Ukończony](media\tutorial-sql-server-to-azure-sql\dms-completed-activity.png)
