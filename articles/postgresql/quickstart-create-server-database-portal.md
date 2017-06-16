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
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: c67ada15c11b81021ff5e6f6e5edc5cb530ece98
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017

---

# <a name="create-an-azure-database-for-postgresql-in-the-azure-portal"></a>Tworzenie serwera usługi Azure Database for PostgreSQL w witrynie Azure Portal

Azure Database for PostgreSQL to usługa zarządzana, która umożliwia uruchamianie i skalowanie w chmurze baz danych PostgreSQL o wysokiej dostępności, a także zarządzanie nimi. W tym przewodniku Szybki start przedstawiono, jak utworzyć serwer usługi Azure Database for PostgreSQL za pomocą witryny Azure Portal.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-postgresql"></a>Tworzenie serwera usługi Azure Database for PostgreSQL

Serwer usługi Azure Database for PostgreSQL jest tworzony ze zdefiniowanym zestawem [zasobów obliczeniowych i przestrzeni dyskowej](./concepts-compute-unit-and-storage.md). Serwer jest tworzony w ramach [grupy zasobów Azure](../azure-resource-manager/resource-group-overview.md).

Wykonaj następujące kroki, aby utworzyć serwer usługi Azure Database for PostgreSQL:
1.  Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.
2.  Na stronie **Nowy** wybierz pozycję **Bazy danych**, a następnie na stronie **Bazy danych** wybierz pozycję **Azure Database for PostgreSQL**.
 ![Usługa Azure Database for PostgreSQL — tworzenie bazy danych](./media/quickstart-create-database-portal/1-create-database.png)

3.  Wypełnij formularz informacjami o szczegółach nowego serwera w sposób pokazany na wcześniejszej ilustracji, używając następujących informacji:
    - Nazwa serwera: **mypgserver-20170401** (nazwa serwera jest mapowana na nazwę DNS i dlatego musi być globalnie unikatowa) 
    - Subskrypcja: jeśli masz wiele subskrypcji, wybierz odpowiednią subskrypcję, w ramach której istnieje zasób lub będą za niego naliczane opłaty.
    - Grupa zasobów: **myresourcegroup**
    - Wybrane przez Ciebie login i hasło administratora serwera
    - Lokalizacja
    - Wersja PostgreSQL

  > [!IMPORTANT]
  > Login i hasło administratora serwera określone w tym miejscu będą wymagane do logowania do serwera i jego baz danych w późniejszej części tego przewodnika Szybki start. Zapamiętaj lub zapisz te informacje do wykorzystania w przyszłości.

4.  Kliknij pozycję **Warstwa cenowa**, aby określić warstwę usługi i poziom wydajności dla nowej bazy danych. W tym przewodniku Szybki start wybierz warstwę **Podstawowa**, **50 jednostek obliczeniowych** i **50 GB** dołączonej pamięci.
 ![Usługa Azure Database for PostgreSQL — wybór warstwy usług](./media/quickstart-create-database-portal/2-service-tier.png)
5.  Kliknij przycisk **OK**.
6.  Kliknij przycisk **Utwórz**, aby aprowizować serwer. Aprowizacja zajmuje kilka minut.

  > [!TIP]
  > Zaznacz opcję **Przypnij do pulpitu nawigacyjnego**, aby łatwo śledzić wdrożenia.

7.  Na pasku narzędzi kliknij pozycję **Powiadomienia**, aby monitorować proces wdrażania.
 ![Usługa Azure Database for PostgreSQL — patrz Powiadomienia](./media/quickstart-create-database-portal/3-notifications.png)
   
  Domyślnie baza danych **postgres** zostanie utworzona na Twoim serwerze. Baza danych [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) to domyślna baza danych przeznaczona do użycia dla użytkowników oraz na potrzeby narzędzi i aplikacji innych firm. 

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurowanie reguły zapory na poziomie serwera

Usługa Azure Database for PostgreSQL tworzy zaporę na poziomie serwera. Ta zapora uniemożliwia zewnętrznym aplikacjom i narzędziom łączenie się z serwerem i wszelkimi bazami danych na tym serwerze, chyba że zostanie utworzona reguła zapory otwierająca zaporę dla konkretnych adresów IP. 

1.  Po zakończeniu wdrożenia kliknij pozycję **Wszystkie zasoby** w menu po lewej stronie i wpisz nazwę **mypgserver-20170401**, aby wyszukać nowo utworzony serwer. Kliknij nazwę serwera wyświetlaną w wynikach wyszukiwania. Zostanie otwarta strona **Przegląd**, która zawiera dalsze opcje konfiguracji.
 
 ![Azure Database for PostgreSQL — wyszukiwanie serwera ](./media/quickstart-create-database-portal/4-locate.png)

2.  W bloku serwera wybierz opcję **Zabezpieczenia połączeń**. 
3.  Kliknij w polu tekstowym w obszarze **Nazwa reguły** i dodaj nową regułę zapory, aby na liście dozwolonych umieścić zakres adresów IP służących do łączności. W tym przewodniku Szybki start przyjmijmy, że zezwalamy na wszystkie adresy IP, wpisując w polach **Nazwa reguły = AllowAllIps**, **Początkowy adres IP = 0.0.0.0** i **Końcowy adres IP = 255.255.255.255**, a następnie klikając przycisk **Zapisz**. Możesz ustawić regułę zapory uwzględniającą zakres adresów IP, aby mieć możliwość nawiązywania połączeń z Twojej sieci.

 ![Usługa Azure Database for PostgreSQL — tworzenie reguły zapory](./media/quickstart-create-database-portal/5-firewall-2.png)

4.  Kliknij przycisk **Zapisz**, a następnie kliknij przycisk **X**, aby zamknąć stronę **Zabezpieczenia połączeń**.

  > [!NOTE]
  > Serwer Azure PostgreSQL komunikuje się przez port 5432. Jeśli próbujesz nawiązać połączenie z sieci firmowej, ruch wychodzący na porcie 5432 może być zablokowany przez zaporę sieciową. Jeśli zachodzi taka sytuacja, nie będzie można nawiązać połączenia z serwerem usługi Azure SQL Database, chyba że dział IT otworzy port 5432.
  >

## <a name="get-the-connection-information"></a>Pobieranie informacji o połączeniu

Podczas tworzenia serwera usługi Azure Database for PostgreSQL, domyślnie jest także tworzona baza danych **postgres**. Aby nawiązać połączenie z serwerem bazy danych, musisz podać informacje o hoście i poświadczenia dostępu.

1. W menu po lewej stronie w witrynie Azure Portal kliknij pozycję **Wszystkie zasoby** i wyszukaj właśnie utworzony serwer **mypgserver 20170401**.

  ![Azure Database for PostgreSQL — wyszukiwanie serwera ](./media/quickstart-create-database-portal/4-locate.png)

2. Kliknij nazwę serwera **mypgserver 20170401**.
3. Wybierz stronę serwera **Przegląd**. Zanotuj wartości **Nazwa serwera** i **Identyfikator logowania administratora serwera**.

 ![Azure Database for PostgreSQL — login administratora serwera](./media/quickstart-create-database-portal/6-server-name.png)

## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a>Nawiązywanie połączenia z bazą danych PostgreSQL w powłoce Cloud Shell za pomocą narzędzia psql

Użyj teraz narzędzia wiersza polecenia psql, aby nawiązać połączenie z serwerem usługi Azure Database for PostgreSQL. 
1. Uruchom powłokę Azure Cloud Shell za pośrednictwem ikony terminala w górnym okienku nawigacji.

   ![Azure Database for PostgreSQL — ikona terminala powłoki Azure Cloud Shell](./media/quickstart-create-database-portal/7-cloud-console.png)

2. Powłoka Azure Cloud Shell zostanie otwarta w przeglądarce, umożliwiając wpisywanie poleceń powłoki bash.

   ![Azure Database for PostgreSQL — znak zachęty powłoki Azure Shell Bash](./media/quickstart-create-database-portal/8-bash.png)

3. W wierszu polecenia powłoki Cloud Shell nawiąż połączenie z serwerem usługi Azure Database for PostgreSQL za pomocą poleceń psql. Następujący format służy do łączenia z serwerem usługi Azure Database for PostgreSQL przy użyciu narzędzia [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html):
   ```bash
   psql --host=<myserver> --port=<port> --username=<server admin login> --dbname=<database name>
   ```

   Na przykład poniższe polecenie nawiązuje połączenie z domyślną bazą danych o nazwie **postgres** na Twoim serwerze PostgreSQL **mypgserver-20170401.postgres.database.azure.com** za pomocą poświadczeń dostępu. Po wyświetleniu monitu wprowadź hasło administratora serwera.

   ```bash
   psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
   ```
4.  Po nawiązaniu połączenia z serwerem utwórz pustą bazę danych za pomocą wiersza polecenia.
```bash
CREATE DATABASE mypgsqldb;
```

5.  W wierszu polecenia wykonaj poniższe polecenie, aby przełączyć połączenie na nowo utworzoną bazę danych **mypgsqldb**.
```bash
\c mypgsqldb
```

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
    - **Port:**  5432
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
- Przeprowadź migrację bazy danych zgodnie z instrukcjami w artykule [Eksportowanie i importowanie](./howto-migrate-using-export-and-import.md) lub [Zrzucanie i przywracanie](./howto-migrate-using-dump-and-restore.md).
- Aby utworzyć usługę Azure Database for PostgreSQL przy użyciu interfejsu wiersza polecenia platformy Azure, zobacz [Tworzenie serwera PostgreSQL — interfejs wiersza polecenia](./quickstart-create-server-database-azure-cli.md).
- Aby zapoznać się z opisem technicznym, zobacz artykuł [About the Azure Database for PostgreSQL service](./overview.md) (Usługa Azure Database for PostgreSQL — informacje).
