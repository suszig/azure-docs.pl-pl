---
title: "Szybki start: Tworzenie serwera usługi Azure Database for MySQL za pomocą witryny Azure Portal | Microsoft Docs"
description: "W tym artykule podano instrukcje pozwalające utworzyć przykładowy serwer usługi Azure Database for MySQL za pomocą witryny Azure Portal w czasie około pięciu minut."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/15/2017
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 829c7e73cbf22d866bbd6fd54edc7a954ad7174c
ms.contentlocale: pl-pl
ms.lasthandoff: 08/16/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Tworzenie serwera usługi Azure Database for MySQL za pomocą witryny Azure Portal
Azure Database for MySQL to usługa zarządzana, która umożliwia uruchamianie i skalowanie w chmurze baz danych MySQL o wysokiej dostępności, a także zarządzanie nimi. W tym przewodniku Szybki start przedstawiono, jak utworzyć serwer usługi Azure Database for MySQL za pomocą witryny Azure Portal w ciągu okołu pięciu minut. 

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.
Otwórz ulubioną przeglądarkę internetową i przejdź do witryny [Microsoft Azure Portal](https://portal.azure.com/). Wprowadź swoje poświadczenia, aby zalogować się do portalu. Widok domyślny to pulpit nawigacyjny usług.

## <a name="create-azure-database-for-mysql-server"></a>Tworzenie serwera usługi Azure Database for MySQL
Serwer usługi Azure Database for MySQL jest tworzony za pomocą zdefiniowanego zestawu [zasobów obliczeniowych i przestrzeni dyskowej](./concepts-compute-unit-and-storage.md). Serwer jest tworzony w ramach [grupy zasobów Azure](../azure-resource-manager/resource-group-overview.md).

Wykonaj następujące kroki, aby utworzyć serwer usługi Azure Database for MySQL:

1. Kliknij przycisk **Nowy** (+) znajdujący się w lewym górnym rogu witryny Azure Portal.

2. Na stronie **Nowy** wybierz pozycję **Bazy danych**, a następnie na stronie **Bazy danych** wybierz pozycję **Azure Database for MySQL**. Aby znaleźć tę usługę, możesz również wpisać hasło **MySQL** w polu wyszukiwania na stronie Nowy.
![Witryna Azure Portal — nowa — baza danych — MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Wypełnij formularz informacjami o szczegółach nowego serwera w sposób pokazany na wcześniejszej ilustracji, używając następujących informacji:

    **Ustawienie** | **Sugerowana wartość** | **Opis pola** 
    ---|---|---
    Nazwa serwera | myserver4demo | Wybierz unikatową nazwę, która identyfikuje serwer usługi Azure Database for MySQL. Nazwa domeny *mysql.database.azure.com* jest dołączana do nazwy serwera podawanej w aplikacjach w celu nawiązania połączenia. Nazwa serwera może zawierać tylko małe litery, cyfry i znaki łącznika (-) oraz musi zawierać od 3 do 63 znaków.
    Subskrypcja | Twoja subskrypcja | Subskrypcja platformy Azure, która ma być używana dla serwera. Jeśli masz wiele subskrypcji, wybierz odpowiednią subskrypcję, w ramach której są naliczane opłaty za ten zasób.
    Grupa zasobów | myresourcegroup | Możesz wprowadzić nową nazwę grupy zasobów lub użyć istniejącej nazwy z subskrypcji.
    Identyfikator logowania administratora serwera | myadmin | Utwórz własne konto logowania do użycia podczas łączenia z serwerem. Nazwą logowania administratora nie może być „azure_superuser”, „admin”, „administrator”, „root”, „guest” ani „public”.
    Hasło | *Wartość wybrana przez użytkownika* | Utwórz nowe hasło dla konta administratora serwera. Musi ono zawierać od 8 do 128 znaków. Hasło musi zawierać znaki z trzech z następujących kategorii: wielkie litery angielskie, małe litery angielskie, cyfry (0–9) i znaki inne niż alfanumeryczne (!, $, #, % itp.).
    Potwierdź hasło | *Wartość wybrana przez użytkownika*| Potwierdź hasło do konta administratora.
    Lokalizacja | *Region najbliżej Twoich użytkowników*| Wybierz lokalizację najbliżej użytkowników lub innych aplikacji Azure.
    Wersja | *Wybierz najnowszą wersję*| Wybierz najnowszą wersję, o ile nie masz określonych wymagań.
    Warstwa cenowa | **Podstawowa**, **50 jednostek obliczeniowych** **50 GB** | Kliknij pozycję **Warstwa cenowa**, aby określić warstwę usługi i poziom wydajności dla nowej bazy danych. Na karcie w górnej części wybierz pozycję **Warstwa Podstawowa**. Kliknij lewy koniec suwaka **Jednostki obliczeniowe**, aby dopasować wartość do najmniejszej dostępnej dla tego przewodnika Szybki start. Kliknij przycisk **OK**, aby zapisać wybraną warstwę cenową. Zobacz poniższy zrzut ekranu.
    Przypnij do pulpitu nawigacyjnego | Zaznacz | Zaznacz pole opcji **Przypnij do pulpitu nawigacyjnego**, aby umożliwiać łatwe monitorowanie serwera na pierwszej stronie pulpitu nawigacyjnego witryny Azure Portal.

    > [!IMPORTANT]
    > Nazwa logowania i hasło administratora serwera określone w tym miejscu będą wymagane do logowania do serwera i jego baz danych w późniejszej części tego przewodnika Szybki start. Zapamiętaj lub zapisz te informacje do wykorzystania w przyszłości.
    > 

    ![Witryna Azure Portal — tworzenie serwera MySQL przez podanie w formularzu wymaganych danych wejściowych](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

4.  Kliknij przycisk **Utwórz**, aby aprowizować serwer. Aprowizowanie zajmuje kilka minut, maksymalnie może potrwać do 20 minut.
   
5.  Na pasku narzędzi kliknij pozycję **Powiadomienia** (ikonę dzwonka), aby monitorować proces wdrażania.

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurowanie reguły zapory na poziomie serwera

Usługa Azure Database for MySQL tworzy zaporę na poziomie serwera. Ta zapora uniemożliwia zewnętrznym aplikacjom i narzędziom łączenie się z serwerem i wszelkimi bazami danych na tym serwerze, chyba że zostanie utworzona reguła zapory otwierająca zaporę dla konkretnych adresów IP. 

1.  Zlokalizuj serwer po ukończeniu wdrażania. W razie potrzeby wyszukaj go. Na przykład kliknij pozycję **Wszystkie zasoby** w menu po lewej stronie i wpisz nazwę serwera (takiego jak przykładowy serwer *myserver4demo*), aby wyszukać nowo utworzony serwer. Kliknij nazwę serwera wyświetloną w wynikach wyszukiwania. Zostanie otwarta strona **Przegląd**, która zawiera dalsze opcje konfiguracji.

2. Na stronie serwera wybierz pozycję **Zabezpieczenia połączeń**.

3.  W obszarze z nagłówkiem **Reguły zapory** kliknij puste pole tekstowe w kolumnie **Nazwa reguły**, aby rozpocząć tworzenie reguły zapory. 

    W tym przewodniku Szybki start zezwolimy na wszystkie adresy IP na serwerze, wypełniając pole tekstowe w każdej kolumnie następującymi wartościami:

    Nazwa reguły | Początkowy adres IP | Końcowy adres IP 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Na górnym pasku narzędzi strony **Zabezpieczenia połączeń** kliknij pozycję **Zapisz**. Przed kontynuowaniem poczekaj chwilę i zwróć uwagę na powiadomienie wskazujące, że aktualizacja zabezpieczeń połączeń została zakończona pomyślnie.

    > [!NOTE]
    > Połączenia z usługą Azure Database for MySQL korzystają z portu 3306. Jeśli próbujesz nawiązać połączenie z sieci firmowej, ruch wychodzący na porcie 3306 może być zablokowany przez zaporę sieciową. W takim przypadku nie będzie można nawiązać połączenia z serwerem, chyba że dział IT otworzy port 3306.
    > 

## <a name="get-the-connection-information"></a>Pobieranie informacji o połączeniu
Aby nawiązać połączenie z serwerem bazy danych, należy pamiętać pełną nazwę serwera i poświadczenia logowania administratora. Być może te wartości zostały zanotowane wcześniej podczas pracy z artykułem Szybki start. W przeciwnym razie możesz łatwo odnaleźć nazwę serwera i informacje dotyczące logowania na stronie **Przegląd** lub **Właściwości** serwera w witrynie Azure Portal.

1. Otwórz stronę **Przegląd** serwera. Zanotuj wartości **Nazwa serwera** i **Identyfikator logowania administratora serwera**. 
    Umieszczaj kursor nad poszczególnymi polami, a po prawej stronie tekstu pojawi się ikona kopiowania. Kliknij ikonę kopiowania, aby w razie potrzeby skopiować wartości.

    W tym przykładzie nazwa serwera to *myserver4demo.mysql.database.azure.com*, a identyfikator logowania administratora serwera to *myadmin@myserver4demo*.

## <a name="connect-to-mysql-using-mysql-command-line-tool"></a>Nawiązywanie połączenia z serwerem MySQL za pomocą narzędzia wiersza polecenia mysql
Istnieje wiele aplikacji, za pomocą których można nawiązać połączenie z serwerem usługi Azure Database for MySQL. Użyjmy najpierw narzędzia wiersza polecenia [mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html), aby zilustrować procedurę nawiązywania połączenia z serwerem.  Możesz użyć przeglądarki internetowej i usługi Azure Cloud Shell w opisany sposób bez konieczności instalowania żadnego dodatkowego oprogramowania. Jeśli masz narzędzie mysql zainstalowane lokalnie na własnym komputerze, możesz nawiązać połączenie także z niego.

1. Uruchom usługę Azure Cloud Shell za pomocą ikony terminalu (>_) w prawej górnej części strony internetowej witryny Azure Portal.

2. Powłoka Azure Cloud Shell zostanie otwarta w przeglądarce, umożliwiając wpisywanie poleceń powłoki bash.

    ![Wiersz polecenia — przykładowy wiersz polecenia mysql](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

3. W wierszu polecenia usługi Cloud Shell nawiąż połączenie z serwerem usługi Azure Database for MySQL, wpisując wiersz polecenia mysql za zielonym monitem.

    Następujący format służy do łączenia z serwerem usługi Azure Database for MySQL przy użyciu narzędzia mysql:
    ```bash
    mysql --host <yourserver> --user <server admin login> --password
    ```

    Na przykład następujące polecenie umożliwia połączenie z przykładowym serwerem:
    ```azurecli-interactive
    mysql --host myserver4demo.mysql.database.azure.com --user myadmin@myserver4demo --password
    ```

    Parametr narzędzia mysql |Sugerowana wartość|Opis
    ---|---|---
    --host | *nazwa serwera* | Określ wartość nazwy serwera, która została użyta wcześniej podczas tworzenia usługi Azure Database for MySQL. Pokazany przykładowy serwer to myserver4demo.mysql.database.azure.com. Użyj w pełni kwalifikowanej nazwy domeny (\*.mysql.database.azure.com) tak jak pokazano w przykładzie. Postępuj zgodnie z instrukcjami w poprzedniej sekcji, aby uzyskać informacje dotyczące połączenia, jeśli nie pamiętasz nazwy serwera. 
    --user | *nazwa logowania administratora serwera* |Wpisz nazwę logowania administratora serwera, którą podano wcześniej podczas tworzenia usługi Azure Database for MySQL. Postępuj zgodnie z instrukcjami w poprzedniej sekcji, aby uzyskać informacje dotyczące połączenia, jeśli nie pamiętasz nazwy użytkownika.  Format to *username@servername*.
    --password | *Zaczekaj, aż zostanie wyświetlony monit* | Po uruchomieniu polecenia zostanie wyświetlony monit o wprowadzenie hasła. Wtedy wpisz hasło podane podczas tworzenia serwera.  Weź pod uwagę, że wpisywane znaki nie są wyświetlane w wierszu polecenia powłoki Bash. Naciśnij klawisz Enter po wpisaniu wszystkich znaków w celu uwierzytelnienia i nawiązania połączenia.

   Po nawiązaniu połączenia narzędzie mysql wyświetli monit `mysql>` umożliwiający wpisywanie poleceń. 

    Przykład danych wyjściowych polecenia mysql:
    ```bash
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 65505
    Server version: 5.6.26.0 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql>
    ```
    > [!TIP]
    > Jeśli konfiguracja zapory nie umożliwia dostępu do adresu IP usługi Azure Cloud Shell, wystąpi następujący błąd:
    >
    > BŁĄD 2003 (28000): Klient o adresie IP 123.456.789.0 nie może uzyskać dostępu do serwera.
    >
    > Aby naprawić błąd, upewnij się, że konfiguracja serwera jest zgodna z krokami opisanymi w sekcji *Konfigurowanie reguły zapory na poziomie serwera* tego artykułu.

4. Upewnij się, że połączenie nawiązano poprawnie, wyświetlając stan serwera. Po nawiązaniu połączenia wpisz `status` w wierszu polecenia mysql>.
    ```sql
    status
    ```

   > [!TIP]
   > Aby zapoznać się z dodatkowymi poleceniami, zobacz [MySQL 5.7 Reference Manual - Chapter 4.5.1 (Podręcznik programu MySQL 5.7 — Rozdział 4.5.1)](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

5.  Utwórz pustą bazę danych, wpisując następujące polecenie w wierszu polecenia mysql>:
    ```sql
    CREATE DATABASE quickstartdb;
    ```
    Wykonanie polecenia może potrwać kilka chwil. 

    Na serwerze usługi Azure Database for MySQL można utworzyć jedną lub wiele baz danych. Można wybrać opcję utworzenia jednej bazy danych na serwer w celu wykorzystania wszystkich zasobów. Można też utworzyć wiele baz danych, współdzielących zasoby. Nie istnieje limit dotyczący maksymalnej liczby baz danych, które można utworzyć, ale w przypadku wielu baz danych są współdzielone te same zasoby serwera. 

6. Wyświetl bazy danych, wpisując następujące polecenie w wierszu polecenia mysql>:

    ```sql
    SHOW DATABASES;
    ```

7.  Wpisz polecenie `\q`, a następnie naciśnij klawisz ENTER, aby zamknąć narzędzie mysql. Po wykonaniu tych czynności możesz zamknąć usługę Azure Cloud Shell.

Nawiązano połączenie z usługą Azure Database for MySQL i utworzono pustą bazę danych użytkownika. Przejdź do następnej sekcji, aby powtórzyć podobne ćwiczenie polegające na nawiązaniu połączenia z tym samym serwerem przy użyciu innego popularnego narzędzia — MySQL Workbench.

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Nawiązywanie połączenia z serwerem za pomocą narzędzia z graficznym interfejsem użytkownika MySQL Workbench
Aby nawiązać połączenie z serwerem usługi Azure MySQL za pomocą narzędzia z graficznym interfejsem użytkownika MySQL Workbench:

1.  Uruchom aplikację MySQL Workbench na swoim komputerze klienckim. Aplikację MySQL Workbench możesz pobrać i zainstalować [stąd](https://dev.mysql.com/downloads/workbench/).

2.  W oknie dialogowym **Konfigurowanie nowego połączenia** wprowadź poniższe informacje na karcie **Parametry**:

    ![konfigurowanie nowego połączenia](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

    | **Ustawienie** | **Sugerowana wartość** | **Opis pola** |
    |---|---|---|
    |   Nazwa połączenia | Połączenie demonstracyjne | Podaj etykietę dla tego połączenia. |
    | Metoda połączenia | Standardowa (TCP/IP) | Metoda Standardowa (TCP/IP) jest wystarczająca. |
    | Nazwa hosta | *nazwa serwera* | Określ wartość nazwy serwera, która została użyta wcześniej podczas tworzenia usługi Azure Database for MySQL. Pokazany przykładowy serwer to myserver4demo.mysql.database.azure.com. Użyj w pełni kwalifikowanej nazwy domeny (\*.mysql.database.azure.com) tak jak pokazano w przykładzie. Postępuj zgodnie z instrukcjami w poprzedniej sekcji, aby uzyskać informacje dotyczące połączenia, jeśli nie pamiętasz nazwy serwera.  |
    | Port | 3306 | Zawsze używaj portu 3306 podczas łączenia z usługą Azure Database for MySQL. |
    | Nazwa użytkownika |  *nazwa logowania administratora serwera* | Wpisz nazwę logowania administratora serwera, którą podano wcześniej podczas tworzenia usługi Azure Database for MySQL. Przykładowa nazwa użytkownika to myadmin@myserver4demo. Postępuj zgodnie z instrukcjami w poprzedniej sekcji, aby uzyskać informacje dotyczące połączenia, jeśli nie pamiętasz nazwy użytkownika. Format to *username@servername*.
    | Hasło | Twoje hasło | Aby zapisać hasło, kliknij przycisk Store in Vault... (Zapisz w magazynie...). |

    Kliknij przycisk **Testuj połączenie**, aby sprawdzić, czy wszystkie parametry zostały prawidłowo skonfigurowane. Kliknij przycisk OK, aby zapisać połączenie. 

    > [!NOTE]
    > Na serwerze domyślnie jest wymuszane używanie protokołu SSL. W związku z tym do pomyślnego nawiązania połączenia jest wymagana dodatkowa konfiguracja. Zobacz [Konfigurowanie łączności SSL w aplikacji w celu bezpiecznego nawiązywania połączeń z usługą Azure Database for MySQL](./howto-configure-ssl.md).  Jeśli chcesz wyłączyć protokół SSL na czas wykonywania instrukcji w tym przewodniku Szybki start, przejdź do witryny Azure Portal, kliknij stronę Zabezpieczenia połączeń i kliknij przycisk przełączania Wymuszaj połączenie SSL.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Oczyszczanie zasobów utworzonych w ramach przewodnika Szybki start odbywa się przez usunięcie [grupy zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md), co obejmuje usunięcie wszystkich zasobów w grupie zasobów, lub przez usunięcie jednego zasobu serwera, jeśli chcesz pozostawić inne zasoby bez zmian.

> [!TIP]
> Inne przewodniki Szybki start w tej kolekcji bazują na tym przewodniku. Jeśli planujesz kontynuować pracę z kolejnymi przewodnikami Szybki start, nie usuwaj zasobów utworzonych w tym przewodniku Szybki start. Jeśli nie planujesz kontynuować pracy, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego przewodnika Szybki start.
>

Aby usunąć całą grupę zasobów łącznie z nowo utworzonym serwerem:
1.  Znajdź grupę zasobów w witrynie Azure Portal. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij nazwę grupy zasobów, np. przykładowej grupy **myresourcegroup**.
2.  Na stronie grupy zasobów kliknij polecenie **Usuń**. Następnie wpisz nazwę grupy zasobów, np. przykładowej grupy **myresourcegroup**, w polu tekstowym, aby potwierdzić usunięcie, i kliknij polecenie **Usuń**.

Jeśli zamiast tego chcesz usunąć nowo utworzony serwer:
1.  Znajdź serwer w witrynie Azure Portal, jeśli nie jest otwarty. W menu po lewej stronie w witrynie Azure Portal kliknij pozycję **Wszystkie zasoby** i wyszukaj utworzony serwer.
2.  Na stronie **Przegląd** kliknij polecenie **Usuń** w górnym okienku.
![Azure Database for MySQL — usuwanie serwera](./media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png)
3.  Potwierdź nazwę serwera do usunięcia i wyświetl jego bazy danych, których dotyczy ta operacja. W polu tekstowym wpisz nazwę serwera, np. przykładowego serwera **myserver4demo**, a następnie kliknij polecenie **Usuń**.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Projektowanie pierwszej bazy danych usługi Azure Database for MySQL](./tutorial-design-database-using-portal.md)


