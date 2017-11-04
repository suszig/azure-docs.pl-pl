---
title: Migrowanie serwera SQL bazy danych do bazy danych Azure SQL | Dokumentacja firmy Microsoft
description: "Dowiedz się przeprowadzić migrację bazy danych SQL Server z bazą danych SQL Azure."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,migrate
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 09/01/2017
ms.author: carlrab
ms.openlocfilehash: 526222944974c08f92aec2a8418e9b42401bc4d3
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a>Migrowanie bazy danych SQL Server do bazy danych SQL Azure

Przenoszenie programu SQL Server bazy danych do bazy danych SQL Azure jest tak proste, jak tworząc pustej bazy danych SQL Azure, a następnie za pomocą [Asystenta migracji danych](https://www.microsoft.com/download/details.aspx?id=53595) (DMA), aby zaimportować bazę danych na platformie Azure. Z tego samouczka dowiesz się, aby:

> [!div class="checklist"]
> * Utwórz pustą bazę danych Azure SQL w portalu Azure (przy użyciu nowego lub istniejącego serwera Azure SQL Database)
> * Utworzenie zapory poziomu serwera w portalu Azure (jeśli wcześniej utworzone)
> * Użyj [Asystenta migracji danych](https://www.microsoft.com/download/details.aspx?id=53595) (DMA), aby zaimportować bazę danych programu SQL Server do pustej bazy danych Azure SQL 
> * Użyj [programu SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS), aby zmienić właściwości bazy danych.

Jeśli nie masz subskrypcji platformy Azure, [utworzyć bezpłatne konto](https://azure.microsoft.com/free/) przed rozpoczęciem.

## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

- Zainstalowana najnowsza wersja [programu SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).  
- Zainstalowana najnowsza wersja [Asystenta migracji danych](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).
- Zidentyfikowano i mają dostęp do bazy danych do migracji. W tym samouczku używana [programu SQL Server 2008 R2 bazy danych AdventureWorks OLTP](https://msftdbprodsamples.codeplex.com/releases/view/59211) w wystąpieniu programu SQL Server 2008 R2 lub nowszej, ale możesz użyć dowolnej bazy danych wybrane. Aby rozwiązać problemy ze zgodnością, należy użyć [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-a-blank-sql-database"></a>Utwórz pustą bazę danych SQL

Baza danych Azure SQL jest tworzona ze zdefiniowanym zestawem [zasobów obliczeniowych i przechowywania](sql-database-service-tiers.md). Baza danych jest tworzona w [grupie zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) oraz na [serwerze logicznym bazy danych Azure SQL Database](sql-database-features.md). 

Wykonaj następujące kroki, aby utworzyć pustą bazę danych SQL. 

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

2. Na stronie **Nowy** wybierz pozycję **Bazy danych**, a następnie na stronie **Nowy** w obszarze **SQL Database** wybierz pozycję **Utwórz**.

   ![Tworzenie bazy danych puste](./media/sql-database-design-first-database/create-empty-database.png)

3. Wypełnij formularz Baza danych SQL w sposób pokazany na wcześniejszej ilustracji, używając następujących informacji:   

   | Ustawienie       | Sugerowana wartość | Opis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nazwa bazy danych** | mySampleDatabase | Prawidłowe nazwy baz danych opisano w artykule [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Identyfikatory baz danych). | 
   | **Subskrypcja** | Twoja subskrypcja  | Aby uzyskać szczegółowe informacje o subskrypcjach, zobacz [Subskrypcje](https://account.windowsazure.com/Subscriptions). |
   | **Grupa zasobów** | myResourceGroup | Prawidłowe nazwy grup zasobów opisano w artykule [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Reguły i ograniczenia nazewnictwa). |
   | **Wybierz źródło** | Pusta baza danych | Określa, czy można utworzyć pustej bazy danych. |

4. Kliknij pozycję **Serwer**, aby utworzyć i skonfigurować nowy serwer dla nowej bazy danych. Wypełnianie **nowy formularz serwera** z następującymi informacjami: 

   | Ustawienie       | Sugerowana wartość | Opis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nazwa serwera** | Dowolna nazwa unikatowa w skali globalnej | Prawidłowe nazwy serwera opisano w artykule [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Reguły i ograniczenia nazewnictwa). | 
   | **Identyfikator logowania administratora serwera** | Dowolna prawidłowa nazwa | Prawidłowe nazwy identyfikatorów logowania opisano w artykule [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Identyfikatory baz danych).|
   | **Hasło** | Dowolne prawidłowe hasło | Hasło musi mieć co najmniej ośmiu znaków i musi zawierać znaki z trzech z następujących kategorii: wielkich liter, małych liter, cyfr i znaków innych niż alfanumeryczne. |
   | **Lokalizacja** | Dowolna prawidłowa lokalizacja | Aby uzyskać informacje na temat regionów, zobacz temat [Regiony systemu Azure](https://azure.microsoft.com/regions/). |

   ![tworzenie serwera bazy danych](./media/sql-database-design-first-database/create-database-server.png)

5. Kliknij pozycję **Wybierz**.

6. Kliknij pozycję **Warstwa cenowa**, aby określić warstwę usługi, liczbę jednostek DTU i ilość miejsca do magazynowania. Zapoznaj się z opcjami liczby jednostek Dtu i Magazyn, które są dostępne dla każdej warstwy usług. 

7. W tym samouczku, wybierz **standardowe** warstwę usługi, a następnie wybierz za pomocą suwaka **100 Dtu (S3)** i **400** GB miejsca do magazynowania.

   ![tworzenie bazy danych s1](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

8. Zaakceptuj warunki wersji zapoznawczej, aby użyć opcji **dodatkowego magazynu**. 

   > [!IMPORTANT]
   > \* Magazyn o rozmiarze większym niż ilość miejsca do magazynowania są dostępne w wersji zapoznawczej dodatkowych kosztów za dodatkową opłatą. Szczegóły można znaleźć w [cenniku usługi SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). 
   >
   >\* W warstwie Premium magazyn o rozmiarze ponad 1 TB jest obecnie dostępny w następujących regionach: Wschodnie stany USA 2, Zachodnie stany USA, Administracja USA — Wirginia, Europa Zachodnia, Niemcy Środkowe, Azja Południowo-Wschodnia, Japonia Wschodnia, Australia Wschodnia, Kanada Środkowa i Kanada Wschodnia. Więcej informacji można znaleźć na stronie [bieżących ograniczeń poziomów P11–P15](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   > 

9. Po wybraniu warstwy serwera, liczby jednostek DTU i ilości miejsca do magazynowania kliknij przycisk **Zastosuj**.  

10. Wybierz **sortowania** dla pustą bazę danych (w tym samouczku, użyj wartości domyślnej). Aby uzyskać więcej informacji na temat sortowań zobacz [sortowania](https://docs.microsoft.com/sql/t-sql/statements/collations)

11. Teraz po uzupełnieniu formularza SQL Database kliknij przycisk **Utwórz**, aby aprowizować bazę danych. Aprowizacja zajmuje kilka minut. 

12. Na pasku narzędzi kliknij pozycję **Powiadomienia**, aby monitorować proces wdrażania.
    
     ![powiadomienie](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Tworzenie reguły zapory na poziomie serwera

Usługa SQL Database tworzy zaporę na poziomie serwera, która uniemożliwia zewnętrznym aplikacjom i narzędziom łączenie się z serwerem i wszelkimi bazami danych na tym serwerze, chyba że zostanie utworzona reguła zapory otwierająca zaporę dla konkretnych adresów IP. Wykonaj następujące kroki, aby utworzyć [regułę zapory na poziomie serwera usługi SQL Database](sql-database-firewall-configure.md) dla podanego adresu IP klienta i włączyć zewnętrzną łączność przez zaporę usługi SQL Database wyłącznie dla konkretnego adresu IP. 

> [!NOTE]
> Usługa SQL Database nawiązuje komunikację na porcie 1433. Jeśli próbujesz nawiązać połączenie z sieci firmowej, ruch wychodzący na porcie 1433 może być zablokowany przez firmową zaporę. Jeśli nastąpi taka sytuacja, nie będzie można nawiązać połączenia z serwerem usługi Azure SQL Database, chyba że dział IT otworzy port 1433.
>

1. Po ukończeniu wdrażania kliknij pozycję **Bazy danych SQL** w menu po lewej stronie i kliknij bazę danych **mySampleDatabase** na stronie **Bazy danych SQL**. Zostanie otwarta strona przeglądu bazy danych zawierająca w pełni kwalifikowaną nazwę serwera (na przykład **mynewserver-20170824.database.windows.net**) i opcje dalszej konfiguracji. 

2. Skopiuj tę w pełni kwalifikowaną nazwę serwera w celu nawiązania połączenia z serwerem i jego bazami danych w kolejnych przewodnikach Szybki start. 

   ![nazwa serwera](./media/sql-database-get-started-portal/server-name.png) 

3. Kliknij przycisk **ustawić Zapora serwera** na pasku narzędzi. Zostanie otwarta strona **Ustawienia zapory** dla serwera SQL Database. 

   ![reguła zapory serwera](./media/sql-database-get-started-portal/server-firewall-rule.png) 

4. Kliknij pozycję **Dodaj adres IP klienta** na pasku narzędzi, aby dodać bieżący adres IP do nowej reguły zapory. Reguła zapory może otworzyć port 1433 dla pojedynczego adresu IP lub zakresu adresów IP.

5. Kliknij pozycję **Zapisz**. Dla bieżącego adresu IP zostanie utworzona reguła zapory na poziomie serwera otwierająca port 1433 na serwerze logicznym.

6. Kliknij przycisk **OK**, a następnie zamknij stronę **Ustawienia zapory**.

Teraz można podłączyć do serwera bazy danych SQL i jego bazy danych przy użyciu programu SQL Server Management Studio, asystenta migracji danych lub inne narzędzie do dowolnego z tego adresu IP przy użyciu konta administratora serwera utworzony w poprzedniej procedurze.

> [!IMPORTANT]
> Domyślnie dostęp za pośrednictwem zapory usługi SQL Database jest włączony dla wszystkich usług platformy Azure. Kliknij przycisk **WYŁ.** na tej stronie, aby wyłączyć tę opcję dla wszystkich usług platformy Azure.

## <a name="sql-server-connection-information"></a>Informacje o połączeniu z serwerem SQL

Pobierz w pełni kwalifikowaną nazwę serwera dla serwera Azure SQL Database w witrynie Azure Portal. Nazwa FQDN serwera umożliwia łączenie się z serwerem Azure SQL za pomocą narzędzi klienta, w tym pomocy migracji danych i SQL Server Management Studio.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. W okienku **Essentials** na stronie bazy danych w witrynie Azure Portal zlokalizuj i skopiuj **nazwę serwera**.

   ![informacje o połączeniu](./media/sql-database-get-started-portal/server-name.png)

## <a name="migrate-your-database"></a>Migrowanie bazy danych

Wykonaj następujące kroki, aby użyć  **[Asystenta migracji danych](https://www.microsoft.com/download/details.aspx?id=53595)**  oceny gotowości bazy danych do migracji do usługi Azure SQL Database i do przeprowadzenia migracji.

1. Otwórz **Asystenta migracji danych**. DMA można uruchomić na dowolnym komputerze z połączenia z wystąpieniem programu SQL Server zawierającego bazę danych, które mają zostać poddane migracji i łączność z Internetem. Nie należy go zainstalować na komputerze obsługującym wystąpienie programu SQL Server, które ma być migrowane. Reguły zapory, który został utworzony w poprzedniej procedurze, musi być dla komputera, na którym są uruchomione Asystenta migracji danych.

     ![Otwórz Asystenta migracji danych](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. W menu po lewej stronie kliknij **+ nowy** utworzyć **oceny** projektu. Wypełnij wymagane wartości, a następnie kliknij przycisk **Utwórz**:

   | Ustawienie      | Sugerowana wartość | Opis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Typ projektu | Migracja | Oceny bazy danych do migracji lub wybrać do oceny i migracja w ramach tego samego przepływu pracy |
   |Nazwa projektu|Samouczek migracji| Nazwę opisową |
   |Typ serwera źródłowego| Oprogramowanie SQL Server | Jest to jedyne źródło, które są obecnie obsługiwane |
   |Typ serwera docelowego| Usługa Azure SQL Database| Można wybierać: baza danych SQL Azure, programu SQL Server, SQL Server na maszynach wirtualnych Azure |
   |Zakres migracji| Schemat i dane| Można wybierać: tylko dane schemat i dane, tylko schematu |
   
   ![Nowy projekt Asystenta migracji danych](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3.  Na **wybierz źródło** , wypełnij żądanej wartości, a następnie kliknij przycisk **Connect**:

    | Ustawienie      | Sugerowana wartość | Opis | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Nazwa serwera | Nazwa serwera lub adres IP | Nazwa serwera lub adres IP |
    | Typ uwierzytelniania | Typ uwierzytelniania preferowaną| Wybór: Active Directory systemu Windows uwierzytelniania, uwierzytelnianie programu SQL Server, zintegrowane uwierzytelnianie, uwierzytelnianie hasłem usługi Active Directory |
    | Nazwa użytkownika | Nazwy logowania | Logowanie musi mieć **serwera kontroli** uprawnień |
    | Hasło| hasło | hasło |
    | Właściwości połączenia| Wybierz **połączenie szyfrowane** i **zaufania certyfikatów serwera** jako odpowiednie dla danego środowiska. | Wybierz polecenie Właściwości, które są odpowiednie dla danego połączenia z serwerem |

    ![nowe źródło wybierz migracji danych](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-source.png)

5. Wybierz pojedynczą bazę danych z serwera źródłowego, aby przeprowadzić migrację do bazy danych SQL Azure, a następnie kliknij przycisk **dalej**. W tym samouczku jest tylko jednej bazy danych.

6. Na **wybierz docelowy** , wypełnij żądanej wartości, a następnie kliknij przycisk **Connect**:

    | Ustawienie      | Sugerowana wartość | Opis | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Nazwa serwera | Pełna nazwa serwera bazy danych Azure | Pełna nazwa serwera bazy danych Azure z poprzedniej procedury |
    | Typ uwierzytelniania | Uwierzytelnianie programu SQL Server | Uwierzytelnianie programu SQL Server jest jedyną opcją są zapisywane w tym samouczku, ale zintegrowane uwierzytelnianie usługi Active Directory i uwierzytelniania hasła w usłudze Active Directory są również obsługiwane przez bazę danych SQL Azure |
    | Nazwa użytkownika | Nazwy logowania | Logowanie musi mieć **bazy danych kontroli** uprawnień do źródłowej bazy danych |
    | Hasło| hasło | hasło |
    | Właściwości połączenia| Wybierz **połączenie szyfrowane** i **zaufania certyfikatów serwera** jako odpowiednie dla danego środowiska. | Wybierz polecenie Właściwości, które są odpowiednie dla danego połączenia z serwerem |

    ![nowy cel wybierz migracji danych](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-target.png)

7. Wybierz bazę danych z serwera docelowego, który został utworzony w poprzedniej procedurze, a następnie kliknij przycisk **dalej** aby rozpocząć proces oceny źródła schematu bazy danych. W tym samouczku jest tylko jednej bazy danych. Należy zauważyć, że poziom zgodności dla tej bazy danych ustawiono 140, która jest domyślny poziom zgodności dla wszystkich nowych baz danych w bazie danych SQL Azure.

   > [!IMPORTANT] 
   > Po przeprowadzeniu migracji bazy danych do bazy danych SQL Azure, są dostępne do obsługi bazy danych na poziomie zgodności określonego dla celów zgodności z poprzednimi wersjami. Aby uzyskać więcej informacji na wpływ i opcje dla działania bazy danych na poziomie zgodności określonego, zobacz [zmienić poziom zgodności bazy danych](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Zobacz też [ALTER DATABASE CONFIGURATION zakres](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) informacji o dodatkowe ustawienia bazy danych na poziomie związane z poziomy zgodności.
   >

8. Na **wybierz obiekty** zakończeniu strony, po źródłowej bazy danych procesu oceny schematu, przejrzyj obiekty wybrana do migracji i przejrzyj obiektów zawierających problemy. Na przykład Przejrzyj **dbo.uspSearchCandidateResumes** obiekt do **SERVERPROPERTY('LCID')** zmiany zachowania i **HumanResourcesJobCandidate** obiekt do Zmiany wyszukiwania pełnotekstowego. 

   > [!IMPORTANT] 
   > W zależności od projektu bazy danych i projektowania aplikacji, podczas migracji źródłowej bazy danych, konieczne może być albo zmodyfikować lub zarówno bazy danych lub aplikacji po migracji (i w niektórych przypadkach przed migracją). Informacje o różnice języka Transact-SQL, które mogą dotyczyć migracji znajdują się w temacie [różnice języka Transact-SQL rozpoznawania podczas migracji do usługi SQL Database](sql-database-transact-sql-information.md).

     ![dane migracji do oceny i obiekt wyboru nowego](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results.png)

9. Kliknij przycisk **skryptu SQL Generowanie** do skryptów obiektów schematu w źródłowej bazy danych. 
10. Przejrzyj wygenerowany skrypt, a następnie kliknij przycisk **obok wystawiać** zgodnie z potrzebami, aby przejrzeć problemy zidentyfikowane oceny i zalecenia. Na przykład dla wyszukiwania pełnotekstowego, podczas uaktualniania zaleca testowania aplikacji korzystania z funkcji pełnego tekstu. Można zapisać lub skopiuj skrypt, w razie potrzeby.

     ![nowego skryptu migracji wygenerowanych danych](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-generated-script.png)

11. Kliknij przycisk **schema wdrażania** i obejrzyj schematu procesu migracji.

     ![nowe migracji schematu migracji danych](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-schema-migration.png)

12. Po zakończeniu migracji schematu, przejrzyj wyniki błędy, a następnie zakładając, że nie ma żadnych, kliknij **migracji danych**.
13. Na **wybierz tabele** strony, przejrzyj tabele wybrana do migracji, a następnie kliknij przycisk **rozpocząć migrację danych**.

     ![nowe dane migracji danych migracji](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration.png)

14. Obejrzyj procesu migracji.

     ![nowy proces migracji danych migracji danych](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration-process.png)

## <a name="connect-to-the-database-with-ssms"></a>Połączenie z bazą danych z narzędzia SSMS

Użyj [programu SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) nawiązać połączenia z serwerem bazy danych SQL Azure.

1. Otwórz program SQL Server Management Studio.

2. W oknie dialogowym **Połącz z serwerem** wprowadź następujące informacje:

   | Ustawienie       | Sugerowana wartość | Opis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Typ serwera | Aparat bazy danych | Ta wartość jest wymagana |
   | Nazwa serwera | W pełni kwalifikowana nazwa serwera | Nazwa powinna być podobny do następującego: **mynewserver20170824.database.windows.net**. |
   | Authentication | Uwierzytelnianie programu SQL Server | Uwierzytelnianie SQL to jedyny typ uwierzytelniania skonfigurowany w tym samouczku. |
   | Login | Konto administratora serwera | To konto określono podczas tworzenia serwera. |
   | Hasło | Hasło konta administratora serwera | To hasło określono podczas tworzenia serwera. |

   ![łączenie z serwerem](./media/sql-database-connect-query-ssms/connect.png)

3. Kliknij przycisk **Opcje** w oknie dialogowym **Połącz z serwerem**. W sekcji **Nawiązywanie połączenia z bazą danych** wprowadź ciąg **mySampleDatabase**, aby nawiązać połączenie z tą bazą danych.

   ![nawiązywanie połączenia z bazą danych na serwerze](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Kliknij przycisk **Połącz**. W programie SSMS zostanie otwarte okno Eksplorator obiektów. 

5. W Eksploratorze obiektów rozwiń pozycję **Bazy danych**, a następnie rozwiń pozycję **mySampleDatabase**, aby wyświetlić obiekty w przykładowej bazie danych.

   ![Obiekty bazy danych](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="change-database-properties"></a>Zmień właściwości bazy danych

Można zmienić warstwy usług, poziom wydajności i poziom zgodności przy użyciu programu SQL Server Management Studio. Podczas fazy importu zalecamy importowania do wyższych bazy danych warstwy wydajności, aby uzyskać najlepszą wydajność, ale skalowanie w dół po zakończeniu importowania zaoszczędzić, dopóki nie będą gotowe do użycia aktywnie importowanych bazy danych. Zmiana poziomu zgodności może spowodować lepszą wydajność i dostęp do najnowszych funkcji usługi baza danych SQL Azure. Podczas migracji starszej bazy danych, jej poziom zgodności bazy danych utrzymywana jest najniższa obsługiwany poziom jest zgodny z bazy danych zostały zaimportowane. Aby uzyskać więcej informacji, zobacz [zwiększona wydajność zapytań ze zgodnością 130 poziom w bazie danych SQL Azure](sql-database-compatibility-level-query-performance-130.md).

1. W Eksploratorze obiektów kliknij prawym przyciskiem myszy **mySampleDatabase** , a następnie kliknij przycisk **nowe zapytanie**. Zostanie otwarte okno zapytania połączenie z bazą danych.

2. Uruchom następujące polecenie, aby ustawić warstwy usług **standardowe** i poziomu wydajności do **S1**.

    ```sql
    ALTER DATABASE mySampleDatabase 
    MODIFY 
        (
        EDITION = 'Standard'
        , MAXSIZE = 250 GB
        , SERVICE_OBJECTIVE = 'S1'
    );
    ```

## <a name="next-steps"></a>Następne kroki 
W tym samouczku przedstawiono do:

> * Utwórz pustą bazę danych Azure SQL w portalu Azure 
> * Utworzenie zapory poziomu serwera w portalu Azure 
> * Użyj [Asystenta migracji danych](https://www.microsoft.com/download/details.aspx?id=53595) (DMA), aby zaimportować bazę danych programu SQL Server do pustej bazy danych Azure SQL 
> * Użyj [programu SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS), aby zmienić właściwości bazy danych.

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak zabezpieczenia bazy danych.

> [!div class="nextstepaction"]
> [Zabezpieczenia bazy danych Azure SQL](sql-database-security-tutorial.md).


