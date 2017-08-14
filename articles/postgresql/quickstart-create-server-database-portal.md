---
title: "Azure Portal: tworzenie serwera usługi Azure Database for PostgreSQL | Dokumentacja firmy Microsoft"
description: "Przewodnik Szybki start dotyczący tworzenia serwera usługi Azure Database for PostgreSQL i zarządzania nim przy użyciu interfejsu użytkownika witryny Azure Portal."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/04/2017
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: e89058a500aeb542ae4c7dc6bb4a9b9ae54db7f2
ms.contentlocale: pl-pl
ms.lasthandoff: 08/09/2017

---

# <a name="create-an-azure-database-for-postgresql-in-the-azure-portal"></a>Tworzenie serwera usługi Azure Database for PostgreSQL w witrynie Azure Portal

Azure Database for PostgreSQL to usługa zarządzana, która umożliwia uruchamianie i skalowanie w chmurze baz danych PostgreSQL o wysokiej dostępności, a także zarządzanie nimi. W tym przewodniku Szybki start przedstawiono, jak utworzyć serwer usługi Azure Database for PostgreSQL za pomocą witryny Azure Portal.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-postgresql"></a>Tworzenie serwera usługi Azure Database for PostgreSQL

Serwer usługi Azure Database for PostgreSQL jest tworzony ze zdefiniowanym zestawem [zasobów obliczeniowych i przestrzeni dyskowej](./concepts-compute-unit-and-storage.md). Serwer jest tworzony w ramach [grupy zasobów Azure](../azure-resource-manager/resource-group-overview.md).

Wykonaj następujące kroki, aby utworzyć serwer usługi Azure Database for PostgreSQL:
1.  Kliknij przycisk **Nowy** (+) znajdujący się w lewym górnym rogu witryny Azure Portal.
2.  Na stronie **Nowy** wybierz pozycję **Bazy danych**, a następnie na stronie **Bazy danych** wybierz pozycję **Azure Database for PostgreSQL**.
 ![Usługa Azure Database for PostgreSQL — tworzenie bazy danych](./media/quickstart-create-database-portal/1-create-database.png)

3.  Wypełnij formularz informacjami o szczegółach nowego serwera w sposób pokazany na wcześniejszej ilustracji, używając następujących informacji:

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    Nazwa serwera |*mypgserver-20170401*|Wybierz unikatową nazwę, która identyfikuje serwer usługi Azure Database for PostgreSQL. Nazwa domeny *postgres.database.azure.com* jest dołączana do nazwy serwera podawanej w aplikacjach w celu nawiązania połączenia. Nazwa serwera może zawierać tylko małe litery, cyfry i znaki łącznika (-) oraz musi zawierać od 3 do 63 znaków.
    Subskrypcja|*Twoja subskrypcja*|Subskrypcja platformy Azure, która ma być używana dla serwera. Jeśli masz wiele subskrypcji, wybierz odpowiednią subskrypcję, w ramach której są naliczane opłaty za ten zasób.
    Grupa zasobów|*myresourcegroup*| Możesz wprowadzić nową nazwę grupy zasobów lub użyć istniejącej nazwy z subskrypcji.
    Identyfikator logowania administratora serwera |*mylogin*| Utwórz własne konto logowania do użycia podczas łączenia się z serwerem. Nazwą logowania administratora nie może być „azure_superuser”, „azure_pg_admin”, „admin”, „administrator”, „root”, „guest” ani „public”. Nazwa nie może rozpoczynać się od „pg_”.
    Hasło |*Wartość wybrana przez użytkownika* | Utwórz nowe hasło dla konta administratora serwera. Musi ono zawierać od 8 do 128 znaków. Hasło musi zawierać znaki z trzech z następujących kategorii: wielkie litery angielskie, małe litery angielskie, cyfry (0–9) i znaki inne niż alfanumeryczne (!, $, #, % itp.).
    Lokalizacja|*Region najbliżej Twoich użytkowników*| Wybierz lokalizację znajdującą się najbliżej użytkowników.
    Wersja PostgreSQL|*Wybierz najnowszą wersję*| Wybierz najnowszą wersję, o ile nie masz określonych wymagań.
    Warstwa cenowa | **Podstawowa**, **50 jednostek obliczeniowych** **50 GB** | Kliknij pozycję **Warstwa cenowa**, aby określić warstwę usługi i poziom wydajności dla nowej bazy danych. Na karcie w górnej części wybierz warstwę Podstawowa. Kliknij lewy koniec suwaka Jednostki obliczeniowe, aby dopasować wartość do najmniejszą dostępnej dla tego przewodnika Szybki start. Kliknij przycisk **OK**, aby zapisać wybraną warstwę cenową. Zobacz poniższy zrzut ekranu.
    | Przypnij do pulpitu nawigacyjnego | Zaznacz | Zaznacz pole opcji **Przypnij do pulpitu nawigacyjnego**, aby umożliwiać łatwe monitorowanie serwera na pierwszej stronie pulpitu nawigacyjnego witryny Azure Portal.

  > [!IMPORTANT]
  > Login i hasło administratora serwera określone w tym miejscu będą wymagane do logowania do serwera i jego baz danych w późniejszej części tego przewodnika Szybki start. Zapamiętaj lub zapisz te informacje do wykorzystania w przyszłości.

    ![Azure Database for PostgreSQL — wybór warstwy cenowej](./media/quickstart-create-database-portal/2-service-tier.png)

4.  Kliknij przycisk **Utwórz**, aby aprowizować serwer. Aprowizowanie zajmuje kilka minut, maksymalnie może potrwać do 20 minut.

5.  Na pasku narzędzi kliknij pozycję **Powiadomienia**, aby monitorować proces wdrażania.
 ![Usługa Azure Database for PostgreSQL — patrz Powiadomienia](./media/quickstart-create-database-portal/3-notifications.png)
   
  Domyślnie baza danych **postgres** zostanie utworzona na Twoim serwerze. Baza danych [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) to domyślna baza danych przeznaczona do użycia dla użytkowników oraz na potrzeby narzędzi i aplikacji innych firm. 

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurowanie reguły zapory na poziomie serwera

Usługa Azure Database for PostgreSQL tworzy zaporę na poziomie serwera. Ta zapora uniemożliwia zewnętrznym aplikacjom i narzędziom łączenie się z serwerem i wszelkimi bazami danych na tym serwerze, chyba że zostanie utworzona reguła zapory otwierająca zaporę dla konkretnych adresów IP. 

1.  Zlokalizuj serwer po ukończeniu wdrażania. W razie potrzeby wyszukaj go. Na przykład kliknij pozycję **Wszystkie zasoby** w menu po lewej stronie i wpisz nazwę ( taką jak przykładowa nazwa mypgserver-20170401), aby wyszukać nowo utworzony serwer. Kliknij nazwę serwera wyświetloną w wynikach wyszukiwania. Zostanie otwarta strona Przegląd, która zawiera dodatkowe opcje konfiguracji.
 
    ![Azure Database for PostgreSQL — wyszukiwanie serwera ](./media/quickstart-create-database-portal/4-locate.png)

2.  Na stronie serwera wybierz pozycję **Zabezpieczenia połączeń**. 
    ![Azure Database for PostgreSQL — tworzenie reguły zapory](./media/quickstart-create-database-portal/5-firewall-2.png)

3.  W obszarze z nagłówkiem **Reguły zapory** kliknij puste pole tekstowe w kolumnie **Nazwa reguły**, aby rozpocząć tworzenie reguły zapory. 

    W tym przewodniku Szybki start zezwolimy na wszystkie adresy IP na serwerze, wypełniając pole tekstowe w każdej kolumnie następującymi wartościami:

    Nazwa reguły | Początkowy adres IP | Końcowy adres IP 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Na górnym pasku narzędzi strony Zabezpieczenia połączeń kliknij pozycję **Zapisz**. Następnie kliknij przycisk **X**, aby zamknąć stronę Zabezpieczenia połączeń.

    > [!NOTE]
    > Serwer Azure PostgreSQL komunikuje się przez port 5432. Jeśli próbujesz nawiązać połączenie z sieci firmowej, ruch wychodzący na porcie 5432 może być zablokowany przez zaporę sieciową. Jeśli zachodzi taka sytuacja, nie będzie można nawiązać połączenia z serwerem usługi Azure SQL Database, chyba że dział IT otworzy port 5432.
  >

## <a name="get-the-connection-information"></a>Pobieranie informacji o połączeniu

Podczas tworzenia serwera usługi Azure Database for PostgreSQL jest także tworzona domyślna baza danych o nazwie **postgres**. Aby nawiązać połączenie z serwerem bazy danych, należy przywołać pełną nazwę serwera i poświadczenia logowania administratora. Być może te wartości zostały zanotowane wcześniej podczas pracy z artykułem Szybki start. W przeciwnym razie łatwo odnajdziesz nazwę nazwy i informacje dotyczące logowania na stronie przeglądu serwera w witrynie Azure Portal.

1. Otwórz stronę **Przegląd** serwera. Zanotuj wartości **Nazwa serwera** i **Identyfikator logowania administratora serwera**.
    Umieszczaj kursor nad poszczególnymi polami, a po prawej stronie tekstu pojawi się ikona kopiowania. Kliknij ikonę kopiowania, aby w razie potrzeby skopiować wartości.

 ![Azure Database for PostgreSQL — dane logowania administratora serwera](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a>Nawiązywanie połączenia z bazą danych PostgreSQL w powłoce Cloud Shell za pomocą narzędzia psql

Istnieje wiele aplikacji, za pomocą których można nawiązać połączenie z serwerem usługi Azure Database for PostgreSQL. Użyjmy teraz narzędzia wiersza polecenia psql, aby zilustrować procedurę nawiązywania połączenia z serwerem.  Możesz użyć przeglądarki internetowej i usługi Azure Cloud Shell w opisany sposób bez konieczności instalowania żadnego dodatkowego oprogramowania. Jeśli masz narzędzie psql zainstalowane lokalnie na własnym komputerze, możesz również nawiązać połączenie w ten sposób.

1. Uruchom powłokę Azure Cloud Shell za pośrednictwem ikony terminala w górnym okienku nawigacji.

   ![Azure Database for PostgreSQL — ikona terminala powłoki Azure Cloud Shell](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Powłoka Azure Cloud Shell zostanie otwarta w przeglądarce, umożliwiając wpisywanie poleceń powłoki bash.

   ![Azure Database for PostgreSQL — znak zachęty powłoki Azure Shell Bash](./media/quickstart-create-database-portal/8-bash.png)

3. W wierszu polecenia usługi Cloud Shell nawiąż połączenie z bazą danych na serwerze usługi Azure Database for PostgreSQL, wpisując polecenie psql w zielonym wierszu polecenia.

    Następujący format służy do łączenia z serwerem usługi Azure Database for PostgreSQL przy użyciu narzędzia [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html):
    ```bash
    psql --host=<yourserver> --port=<port> --username=<server admin login> --dbname=<database name>
    ```

    Na przykład następujące polecenie powoduje połączenie z przykładowym serwerem:

    ```bash
    psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
    ```

    Parametr psql |Sugerowana wartość|Opis
    ---|---|---
    --host | *nazwa serwera* | Określ wartość nazwy serwera, która była używana wcześniej podczas tworzenia bazy danych Azure Database for PostgreSQL. Pokazany przykładowy serwer to mypgserver-20170401.postgres.database.azure.com. Użyj w pełni kwalifikowanej nazwy domeny (\*. postgres.database.azure.com), jak pokazano w przykładzie. Postępuj zgodnie z instrukcjami z poprzedniej sekcji, aby uzyskać informacje dotyczące połączenia, jeśli nie pamiętasz nazwy serwera. 
    --port | **5432** | Zawsze używaj portu 5432 podczas łączenia z usługą Azure Database for PostgreSQL. 
    --username | *nazwa logowania administratora serwera* |Wpisz nazwę użytkownika logowania administratora serwera, którą podano wcześniej podczas tworzenia bazy danych Azure Database for PostgreSQL. Postępuj zgodnie z instrukcjami z poprzedniej sekcji, aby uzyskać informacje dotyczące połączenia, jeśli nie pamiętasz nazwy użytkownika.  Format to *username@servername*.
    --dbname | **postgres** | Użyj domyślnej, wygenerowanej przez system nazwy bazy danych *postgres* dla pierwszego połączenia. Później możesz utworzyć własną bazę danych.

    Po uruchomieniu polecenia psql przy użyciu własnych wartości parametrów pojawi się monit o wpisanie hasła administratora serwera. Jest to hasło podane podczas tworzenia serwera. 

    Parametr psql |Sugerowana wartość|Opis
    ---|---|---
    hasło | *hasło administratora* | Uwaga: wpisane znaki hasła nie będą widoczne w wierszu polecenia powłoki bash. Naciśnij klawisz Enter po wpisaniu wszystkich znaków w celu uwierzytelnienia i nawiązania połączenia.

4.  Po nawiązaniu połączenia z serwerem utwórz pustą bazę danych za pomocą wiersza polecenia, wpisując następujące polecenie:
    ```bash
    CREATE DATABASE mypgsqldb;
    ```

5.  W wierszu polecenia wykonaj poniższe polecenie, aby przełączyć połączenie na nowo utworzoną bazę danych **mypgsqldb**.
    ```bash
    \c mypgsqldb
    ```

6.  Wpisz \q, a następnie naciśnij klawisz ENTER, aby zamknąć narzędzie psql. Możesz zamknąć usługę Azure Cloud Shell.

Teraz nawiązano połączenie z bazą danych Azure Database for PostgreSQL i utworzono pustą bazę danych użytkownika. Przejdź do następnej sekcji, aby połączyć się przy użyciu innego popularnego narzędzia pgAdmin.

## <a name="connect-to-postgresql-database-using-pgadmin"></a>Nawiązywanie połączenia z bazą danych PostgreSQL za pomocą narzędzia pgAdmin

Aby nawiązać połączenie z serwerem usługi Azure PostgreSQL za pomocą narzędzia z graficznym interfejsem użytkownika _pgAdmin_
1.  Uruchom aplikację _pgAdmin_ na swoim komputerze klienckim. Aplikację _pgAdmin_ można zainstalować ze strony http://www.pgadmin.org/.
2.  Wybierz pozycję **Add New Server** (Dodaj nowy serwer) w menu **Quick Links** (Szybkie linki).
3.  W oknie dialogowym **Create - Server** (Tworzenie — Serwer) na karcie **General** (Ogólne) wprowadź unikatową przyjazną nazwę serwera, np. **Serwer Azure PostgreSQL**.
![Narzędzie pgAdmin — Tworzenie — Serwer](./media/quickstart-create-database-portal/9-pgadmin-create-server.png)
4.  W oknie dialogowym **Create - Server** (Tworzenie — Serwer) na karcie **Connection** (Połączenie) użyj podanych ustawień i kliknij przycisk **Save** (Zapisz).
   ![Narzędzie pgAdmin — Tworzenie — Serwer](./media/quickstart-create-database-portal/10-pgadmin-create-server.png)
    - **Host Name/Address** (Nazwa i adres hosta): mypgserver 20170401.postgres.database.azure.com 
        - W pełni kwalifikowana nazwa serwera.
    - **Port:** 5432
        - Numerem portu używanym przez ten serwer bazy danych jest 5432.
    - **Maintenance Database** (Baza danych konserwacji): postgres 
        - Domyślna nazwa bazy danych wygenerowana przez system.
    - **User Name** (Nazwa użytkownika) mylogin@mypgserver-20170401 
        - Login administratora serwera (user@mypgserver) uzyskany wcześniej w tym przewodniku Szybki start.
    - **Password** (Hasło): hasło wybrane podczas tworzenia serwera wcześniej w tym przewodniku Szybki start.
    - **SSL Mode** (Tryb SSL): Wymagaj
        - Domyślnie wszystkie serwery Azure PostgreSQL są tworzone z włączonym wymuszaniem protokołu SSL. Aby wyłączyć wymuszanie protokołu SSL, zobacz szczegółowe informacje w artykule [Wymuszanie protokołu SSL](./concepts-ssl-connection-security.md).
5.  Kliknij pozycję **Zapisz**.
6.  W lewym okienku Browser (Przeglądarka) rozwiń węzeł **Server Groups** (Grupy serwerów). Wybierz swój serwer **Serwer Azure PostgreSQL**.
7.  Wybierz **serwer**, z którym nawiązano połączenie, a następnie wybierz w jego obszarze pozycję **Databases** (Bazy danych). 
8.  Kliknij prawym przyciskiem myszy pozycję **Databases** (Bazy danych), aby utworzyć bazę danych.
9.  Wybierz nazwę bazy danych **mypgsqldb** oraz jej właściciela: identyfikator logowania administratora serwera **mylogin**.
10. Kliknij przycisk **Save** (Zapisz), aby utworzyć pustą bazę danych.
11. W oknie **Browser** (Przeglądarka) rozwiń grupę **Server** (Serwer). Rozwiń utworzony serwer. Będzie widoczna baza danych **mypgsqldb**.
 ![Narzędzie pgAdmin — Tworzenie — Baza danych](./media/quickstart-create-database-portal/11-pgadmin-database.png)


## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Wyczyść wszystkie zasoby utworzone w tym przewodniku Szybki start, usuwając [grupę zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md).

> [!TIP]
> Inne przewodniki Szybki start w tej kolekcji bazują na tym przewodniku. Jeśli planujesz kontynuować pracę z kolejnymi przewodnikami Szybki start, nie usuwaj zasobów utworzonych w tym przewodniku Szybki start. Jeśli nie planujesz kontynuować pracy, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego przewodnika Szybki start.

1.  W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij pozycję **myresourcegroup**.
2.  Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę **myresourcegroup**, a następnie kliknij pozycję Usuń.

Jeśli chcesz usunąć nowo utworzony serwer:
1.  Z menu po lewej stronie w witrynie Azure Portal, kliknij pozycję serwerów PostgreSQL i wyszukaj właśnie utworzony serwer
2.  Na stronie Przegląd kliknij przycisk Usuń w górnym okienku ![Azure Database for PostgreSQL — usuń serwer](./media/quickstart-create-database-portal/12-delete.png)
3.  Potwierdź nazwę serwera, który chcesz usunąć, i wyświetl znajdujące się pod nim bazy danych. W polu tekstowym wpisz **mypgserver 20170401**, a następnie kliknij przycisk Usuń.

## <a name="next-steps"></a>Następne kroki
> [!div class="nextstepaction"]
> [Migrowanie bazy danych przy użyciu funkcji eksportowania i importowania](./howto-migrate-using-export-and-import.md)

