---
title: Zabezpieczenia bazy danych Azure SQL | Dokumentacja firmy Microsoft
description: "Więcej informacji na temat techniki i funkcje do zabezpieczenia bazy danych Azure SQL."
services: sql-database
documentationcenter: 
author: DRediske
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 06/28/2017
ms.author: daredis
ms.openlocfilehash: 90c03f1538197e1cd1c90165417a4ec74c9c5961
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="secure-your-azure-sql-database"></a>Zabezpieczenia bazy danych Azure SQL

Usługa SQL Database zabezpiecza dane przez ograniczenie dostępu do bazy danych przy użyciu reguł zapory, mechanizmów uwierzytelniania wymagających od użytkowników potwierdzenia tożsamości oraz funkcji autoryzacji do danych za pośrednictwem członkostw i uprawnień opartych na rolach, a także zabezpieczeń na poziomie wiersza i dynamicznego maskowania danych.

Można zwiększyć ochrony bazy danych przed złośliwymi użytkownikami lub nieautoryzowanego dostępu, wystarczy kilka prostych kroków. W tym samouczku dowiesz się: 

> [!div class="checklist"]
> * Konfigurowanie reguł zapory poziomu serwera dla serwera w portalu Azure
> * Konfigurowanie reguł zapory na poziomie bazy danych dla bazy danych przy użyciu narzędzia SSMS
> * Połączenia z bazą danych przy użyciu ciągu bezpiecznego połączenia
> * Zarządzanie dostępem użytkowników
> * Ochrona danych za pomocą szyfrowania
> * Włączanie inspekcji bazy danych SQL
> * Włączyć wykrywanie zagrożeń bazy danych SQL

Jeśli nie masz subskrypcji platformy Azure, [utworzyć bezpłatne konto](https://azure.microsoft.com/free/) przed rozpoczęciem.

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka, upewnij się, że należy dysponować następującymi elementami:

- Zainstalowana najnowsza wersja [programu SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). 
- Zainstalowany program Microsoft Excel
- Utworzone usługi Azure SQL server i bazy danych — zobacz [tworzenie bazy danych Azure SQL w portalu Azure](sql-database-get-started-portal.md), [tworzenia pojedynczej bazy danych Azure SQL przy użyciu interfejsu wiersza polecenia Azure](sql-database-get-started-cli.md), i [tworzenia pojedynczej bazy danych Azure SQL przy użyciu programu PowerShell](sql-database-get-started-powershell.md). 

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Tworzenie reguły zapory na poziomie serwera w witrynie Azure Portal

Baz danych są chronione przez zaporę Windows na platformie Azure. Domyślnie wszystkie połączenia z serwerem i bazy danych w obrębie serwera są odrzucane, z wyjątkiem połączeń z innymi usługami Azure. Aby uzyskać więcej informacji, zobacz [Azure SQL Database server-level and database-level firewall rules (Reguły zapory na poziomie serwera i na poziomie bazy danych usługi Azure SQL Database)](sql-database-firewall-configure.md).

Najbezpieczniejszą konfigurację jest do ustawienia "Zezwalaj na dostęp do usług platformy Azure" off. Jeśli potrzebujesz połączenia z bazą danych z maszyny Wirtualnej platformy Azure lub usługę w chmurze, należy utworzyć [zastrzeżonego adresu IP](../virtual-network/virtual-networks-reserved-public-ip.md) i tylko zastrzeżonego adresu IP adres dostęp przez zaporę. 

Wykonaj następujące kroki, aby utworzyć [regułę zapory poziomu serwera bazy danych SQL](sql-database-firewall-configure.md) serwera umożliwia nawiązywanie połączeń z określonego adresu IP. 

> [!NOTE]
> Jeśli utworzono przykładowej bazy danych na platformie Azure przy użyciu jednej z poprzednich samouczki i Przewodniki Szybki Start i wykonywania tego samouczka na komputerze przy użyciu tego samego adresu IP, który znajdował się, gdy udał przez te samouczki, można pominąć ten krok jako zostaną utworzone reguły zapory poziomu serwera.
>

1. Kliknij przycisk **baz danych SQL** z menu po lewej stronie i kliknij bazę danych, aby skonfigurować zaporę reguły dla **baz danych SQL** strony. Zostanie otwarta strona Przegląd bazy danych, wyświetlając nazwę FQDN serwera (takich jak **mynewserver 20170313.database.windows.net**) i udostępnia opcje dla dalszej konfiguracji.

      ![reguła zapory serwera](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. Kliknij pozycję **Ustaw zaporę serwera** na pasku narzędzi, tak jak pokazano to na wcześniejszej ilustracji. Zostanie otwarta strona **Ustawienia zapory** dla serwera SQL Database. 

3. Kliknij przycisk **Dodaj adres IP klienta** na pasku narzędzi, aby dodać publiczny adres IP komputera połączonego z portalu lub ręcznie wprowadzić reguły zapory, a następnie kliknij przycisk **zapisać**.

      ![ustawianie reguły zapory serwera](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. Kliknij przycisk **OK**, a następnie kliknij przycisk **X**, aby zamknąć stronę **Ustawienia zapory**.

Teraz można podłączyć do dowolnej bazy danych na serwerze z określonego adresu IP lub zakresu adresów IP.

> [!NOTE]
> Usługa SQL Database nawiązuje komunikację na porcie 1433. Jeśli próbujesz nawiązać połączenie z sieci firmowej, ruch wychodzący na porcie 1433 może być zablokowany przez firmową zaporę. Jeśli zachodzi taka sytuacja, nie będzie można nawiązać połączenia z serwerem Azure SQL Database, chyba że dział IT otworzy port 1433.
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a>Utworzyć regułę zapory poziomu bazy danych przy użyciu narzędzia SSMS

Włącz reguły zapory poziomu bazy danych, można utworzyć ustawień innej zapory dla różnych baz danych w tym samym serwerze logicznym i tworzenie reguł zapory, które można przenosić — co oznacza, że należy wykonać bazy danych podczas [pracy awaryjnej](sql-database-geo-replication-overview.md) zamiast są przechowywane na serwerze SQL. Reguły zapory poziomu bazy danych można tylko skonfigurowane za pomocą instrukcji języka Transact-SQL i tylko po skonfigurowaniu pierwszą regułę zapory poziomu serwera. Aby uzyskać więcej informacji, zobacz [Azure SQL Database server-level and database-level firewall rules (Reguły zapory na poziomie serwera i na poziomie bazy danych usługi Azure SQL Database)](sql-database-firewall-configure.md).

Obejmuje następujące kroki, aby utworzyć regułę zapory dotyczące bazy danych.

1. Połączenia z bazą danych, na przykład za pomocą [programu SQL Server Management Studio](./sql-database-connect-query-ssms.md).

2. W Eksploratorze obiektów kliknij prawym przyciskiem myszy bazę danych, aby dodać regułę zapory, a następnie kliknij przycisk **nowe zapytanie**. Zostanie otwarte puste okno zapytania, które jest połączone z Twoją bazą danych.

3. W oknie zapytania zmienić adres IP do publicznego adresu IP, a następnie wykonaj następujące zapytanie:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. Na pasku narzędzi kliknij **Execute** do utworzenia reguły zapory.

## <a name="view-how-to-connect-an-application-to-your-database-using-a-secure-connection-string"></a>Wyświetl jak łączenie aplikacji z bazy danych przy użyciu ciągu bezpiecznego połączenia

W celu zapewnienia bezpiecznego zaszyfrowanego połączenia między aplikacji klienckiej i bazy danych SQL, ciąg połączenia musi być skonfigurowana do:

- Żądanie połączenia szyfrowanego i
- Aby nie ufać certyfikatowi serwera. 

Ustanawia połączenie przy użyciu zabezpieczeń TLS (Transport Layer) i zmniejsza ryzyko ataków man-in--middle. Można uzyskać poprawnie skonfigurowane parametry połączenia bazy danych SQL, aby obsługiwany klient sterowniki z portalu Azure pokazany dla ADO.net w tym zrzucie ekranu.

1. Wybierz **baz danych SQL** z menu po lewej stronie i kliknij bazę danych na **baz danych SQL** strony.

2. Na **omówienie** stron dla bazy danych, kliknij przycisk **Pokaż parametry połączenia bazy danych**.

3. Sprawdź pełne parametry połączenia sterownika **ADO.NET**.

    ![Parametry połączenia sterownika ADO.NET](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a>Tworzenie bazy danych użytkowników

Przed utworzeniem żadnych użytkowników, należy najpierw wybrać jedną z dwóch typów uwierzytelniania obsługiwanych przez bazę danych SQL Azure: 

**Uwierzytelnianie SQL**, który używa nazwy użytkownika i hasła do logowania i użytkowników, którzy są prawidłowe tylko w kontekście określonej bazy danych w ramach serwera logicznego. 

**Uwierzytelniania usługi Azure Active Directory**, który używa tożsamości zarządzanych przez usługę Azure Active Directory. 

Jeśli chcesz użyć [usługi Azure Active Directory](./sql-database-aad-authentication.md) na potrzeby uwierzytelniania w bazie danych SQL, wypełnione usługi Azure Active Directory musi istnieć, zanim przejdziesz dalej.

Wykonaj następujące kroki, aby utworzyć użytkownika przy użyciu uwierzytelniania programu SQL:

1. Połączenia z bazą danych, na przykład za pomocą [programu SQL Server Management Studio](./sql-database-connect-query-ssms.md) przy użyciu poświadczeń administratora serwera.

2. W Eksploratorze obiektów kliknij prawym przyciskiem myszy bazę danych, aby dodać nowego użytkownika na, a następnie kliknij przycisk **nowe zapytanie**. Zostanie otwarte okno puste zapytania podłączoną do wybranej bazy danych.

3. W oknie zapytania wprowadź następujące zapytanie:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. Na pasku narzędzi kliknij **Execute** do utworzenia użytkownika.

5. Domyślnie użytkownik może połączyć się z bazą danych, ale nie ma uprawnień do odczytu lub zapisu danych. Aby przyznać uprawnienia do nowo utworzonego użytkownika, należy wykonać następujące dwa polecenia w nowym oknie zapytania

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

Jest najlepszym rozwiązaniem jest tworzenie tych kont bez uprawnień administratora na poziomie bazy danych w celu połączenia z bazą danych, chyba że trzeba wykonać zadania administratora, takie jak tworzenie nowych użytkowników. Zapoznaj się z tematem [samouczek usługi Azure Active Directory](./sql-database-aad-authentication-configure.md) na temat uwierzytelniania za pomocą usługi Azure Active Directory.


## <a name="protect-your-data-with-encryption"></a>Ochrona danych za pomocą szyfrowania

Azure SQL Database przezroczystego szyfrowania danych (funkcji TDE) automatycznie szyfruje dane przechowywane, bez konieczności wprowadzania jakichkolwiek zmian w aplikacji dostęp do zaszyfrowanych bazy danych. W przypadku nowo utworzonego baz danych funkcji TDE jest domyślnie włączone. Aby włączyć funkcji TDE bazy danych lub sprawdź, czy w funkcji TDE, wykonaj następujące kroki:

1. Wybierz **baz danych SQL** z menu po lewej stronie i kliknij bazę danych na **baz danych SQL** strony. 

2. Polecenie **przezroczystego szyfrowania danych** aby otworzyć stronę konfiguracji dla funkcji TDE.

    ![Przezroczyste szyfrowanie danych](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. W razie potrzeby ustaw **szyfrowanie danych** on i kliknij przycisk **zapisać**.

Proces szyfrowania jest uruchamiany w tle. Można monitorować postęp, łącząc się z bazą danych SQL przy użyciu [programu SQL Server Management Studio](./sql-database-connect-query-ssms.md) badając kolumnie encryption_state `sys.dm_database_encryption_keys` widoku.

## <a name="enable-sql-database-auditing-if-necessary"></a>Włączanie inspekcji bazy danych SQL, w razie potrzeby

Usługa Azure SQL Database Auditing śledzi zdarzenia bazy danych i zapisuje je inspekcji logowania na koncie magazynu Azure. Inspekcja pomaga zachować zgodność z przepisami, analizować aktywność bazy danych oraz uzyskać wgląd w odchylenia i anomalie, które mogą wskazywać potencjalne naruszenie zabezpieczeń. Wykonaj następujące kroki, aby utworzyć domyślne zasady dla Twojej bazy danych SQL inspekcji:

1. Wybierz **baz danych SQL** z menu po lewej stronie i kliknij bazę danych na **baz danych SQL** strony. 

2. W bloku ustawienia, wybierz **Inspekcja i wykrywanie zagrożeń**. Powiadomienie, że inspekcję na poziomie serwera jest diabled i że istnieje **wyświetlić ustawienia serwera** łącze, które pozwala wyświetlać lub modyfikować ustawienia z tego kontekstu inspekcji serwera.

    ![Inspekcja bloku](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Jeśli wolisz włączyć typu inspekcji (lub lokalizacji?) innej niż określono na poziomie serwera, Włącz **ON** inspekcji i wybierz polecenie **obiektu Blob** typu inspekcji. Po włączeniu inspekcji obiektu Blob serwera inspekcji bazy danych skonfigurowane będą istniały równolegle z obiektu Blob inspekcji serwera.

    ![Włączanie inspekcji](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. Wybierz **szczegóły magazynu** aby otworzyć blok magazyn dzienników inspekcji. Wybierz konto magazynu Azure, w którym zostaną zapisane dzienniki, a następnie kliknij przycisk okres przechowywania, po upływie którego będzie można usunąć stare dzienniki, **OK** u dołu. 

   > [!TIP]
   > Użyj tego samego konta magazynu dla wszystkich baz danych inspekcji na maksymalne wykorzystanie inspekcji szablonów raportów.
   > 

5. Kliknij pozycję **Zapisz**.

> [!IMPORTANT]
> Jeśli chcesz dostosować zdarzeń inspekcji, można to zrobić za pomocą programu PowerShell lub interfejsu API REST — zobacz [inspekcji bazy danych SQL](sql-database-auditing.md) więcej szczegółów.
>

## <a name="enable-sql-database-threat-detection"></a>Włączyć wykrywanie zagrożeń bazy danych SQL

Wykrywanie zagrożeń udostępnia nową warstwę zabezpieczeń, co umożliwia klientom wykrywanie i odpowiadanie na potencjalne zagrożenia w miarę ich występowania, zapewniając alerty zabezpieczeń w nietypowych działań. Użytkownicy mogą eksplorować podejrzane zdarzenia przy użyciu inspekcji bazy danych SQL w celu określenia, czy są one wynikiem próba uzyskania dostępu, naruszenia lub wykorzystać w bazie danych. Wykrywanie zagrożeń upraszcza potencjalne zagrożenia do bazy danych bez konieczności ekspertów zabezpieczeń lub zarządzać zabezpieczeniami zaawansowanymi, monitorowanie systemów.
Na przykład wykrywanie zagrożeń wykrywa niektóre działania nietypowych bazy danych, które wskazują potencjalne prób iniekcji kodu SQL. Iniekcja kodu SQL jest jednym z typowych problemów zabezpieczeń aplikacji sieci Web w Internecie, używane do ataków opartych na danych aplikacji. Osoby atakujące wykorzystać luki w zabezpieczeniach aplikacji do dodania złośliwego instrukcje SQL do pola wejścia aplikacji, naruszenia i modyfikacji danych w bazie danych.

1. Przejdź do bloku konfiguracji bazy danych SQL, które chcesz monitorować. W bloku ustawienia, wybierz **Inspekcja i wykrywanie zagrożeń**.

    ![Okienko nawigacji](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. W **Inspekcja i wykrywanie zagrożeń** Włącz bloku konfiguracji **ON** inspekcji, które będą wyświetlane ustawienia wykrywania zagrożeń.

3. Włącz **ON** wykrywanie zagrożeń.

4. Skonfiguruj listę wiadomości e-mail, które będą wysyłane alerty zabezpieczeń po wykryciu nietypowe działania bazy danych.

5. Kliknij przycisk **zapisać** w **Inspekcja i wykrywanie zagrożeń** bloku, aby zapisać nowe lub zaktualizowane inspekcji i zasady wykrywania zagrożeń.

    ![Okienko nawigacji](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    W przypadku wykrycia są nietypowe działania bazy danych, otrzymasz wiadomość e-mail z powiadomieniem po wykryciu nietypowe działania bazy danych. Wiadomość e-mail będzie zawierał informacje o zdarzeniu podejrzane zabezpieczeń w tym rodzaju nietypowych działań, nazwa bazy danych, nazwę serwera i czas trwania zdarzenia. Ponadto zawierają informacje dotyczące możliwe przyczyny i zalecane akcje do sprawdzania i ograniczyć potencjalne zagrożenie dla bazy danych. Następne kroki opisano za pośrednictwem co zrobić, należy pojawi się wiadomość e-mail:

    ![Wykrywanie zagrożeń w wiadomości e-mail](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. W wiadomości e-mail, kliknij polecenie **dziennik inspekcji SQL Azure** łącza, co spowoduje uruchomienie portalu Azure i wyświetlić odpowiednie rekordy inspekcji w czasie zbliżonym do podejrzane zdarzenia.

    ![Rekordy inspekcji](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Polecenie rekordów inspekcji, aby wyświetlić więcej szczegółów na działania podejrzane bazy danych, takie jak instrukcji SQL, Niepowodzenie IP Przyczyna i klienta.

    ![Szczegóły rekordu](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. W bloku rekordów inspekcji kliknij **Otwórz w programie Excel** otworzyć wstępnie skonfigurowany szablon programu excel Aby zaimportować i uruchomić dokładniejszej analizy w czasie zbliżonym do podejrzane zdarzenia dziennika inspekcji.

    > [!NOTE]
    > Excel 2010 lub nowszej, zasilanie kwerendy i **szybkie łączenie** ustawienie jest wymagane.

    ![Otwarte rekordy w programie Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. Aby skonfigurować **szybkie łączenie** ustawienie — w **dodatku POWER QUERY** karty wstążki, wybierz opcję **opcje** do wyświetlenia okna dialogowego Opcje. Wybierz sekcję prywatności i wybierz opcję drugi — "Ignorowanie poziomów prywatności i potencjalne poprawianie wydajności":

    ![Łączenie programu Excel fast](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. Aby załadować dzienników inspekcji SQL, upewnij się, że parametry w ustawieniach kartę są poprawnie ustawione, a następnie wybierz wstążki "Dane" i kliknij przycisk "Odśwież wszystko".

    ![Parametry programu Excel](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. Wyniki są wyświetlane w **dzienników inspekcji SQL** arkusza, co pozwala na przeprowadzanie analizy głębiej nietypowych działań, które zostały wykryte i ograniczyć wpływ zdarzeń zabezpieczeń w aplikacji.


## <a name="next-steps"></a>Następne kroki
W tym samouczku przedstawiono poprawienia ochrony przed złośliwymi użytkownikami lub nieautoryzowanego dostępu, wystarczy kilka prostych kroków bazy danych.  W tym samouczku omówiono: 

> [!div class="checklist"]
> * Konfigurowanie reguł zapory dla serwera i lub bazy danych
> * Połączenia z bazą danych przy użyciu ciągu bezpiecznego połączenia
> * Zarządzanie dostępem użytkowników
> * Ochrona danych za pomocą szyfrowania
> * Włączanie inspekcji bazy danych SQL
> * Włączyć wykrywanie zagrożeń bazy danych SQL

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak wdrożyć rozproszona geograficznie bazy danych.

> [!div class="nextstepaction"]
>[Implementowanie rozproszonej geograficznie bazy danych](sql-database-implement-geo-distributed-database.md)

