---
title: "Projektowanie pierwszej bazy danych usługi Azure Database for MySQL — witryna Azure Portal | Microsoft Docs"
description: "W tym samouczku wyjaśniono, jak utworzyć serwer oraz bazę danych usługi Azure Database for MySQL i zarządzać nimi przy użyciu witryny Azure Portal."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: tutorial
ms.date: 11/03/2017
ms.custom: mvc
ms.openlocfilehash: 76cccf9e2ce0a1e59b43646c43ac165d46dade4a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Projektowanie pierwszej bazy danych usługi Azure Database for MySQL
Azure Database for MySQL to usługa zarządzana, która umożliwia uruchamianie i skalowanie w chmurze baz danych MySQL o wysokiej dostępności, a także zarządzanie nimi. Za pomocą witryny Azure Portal możesz łatwo zarządzać serwerem i zaprojektować bazę danych.

W tym samouczku nauczysz się wykonywać następujące czynności, używając witryny Azure Portal:

> [!div class="checklist"]
> * Tworzenie usługi Azure Database for MySQL
> * Konfigurowanie zapory serwera
> * Tworzenie bazy danych za pomocą narzędzia wiersza polecenia mysql
> * Ładowanie przykładowych danych
> * Zapytania o dane
> * Aktualizowanie danych
> * Przywracanie danych

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal
Otwórz ulubioną przeglądarkę internetową i przejdź do witryny [Microsoft Azure Portal](https://portal.azure.com/). Wprowadź swoje poświadczenia, aby zalogować się do portalu. Widok domyślny to pulpit nawigacyjny usług.

## <a name="create-an-azure-database-for-mysql-server"></a>Tworzenie serwera usługi Azure Database for MySQL
Serwer usługi Azure Database for MySQL jest tworzony za pomocą zdefiniowanego zestawu [zasobów obliczeniowych i przestrzeni dyskowej](./concepts-compute-unit-and-storage.md). Serwer jest tworzony w ramach [grupy zasobów Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

1. Wybierz pozycję **Bazy danych** > **Azure Database for MySQL**. Jeśli nie możesz znaleźć pozycji Serwer MySQL w kategorii **Bazy danych**, kliknij pozycję **Zobacz wszystko**, aby wyświetlić wszystkie dostępne usługi baz danych. Możesz również wpisać frazę **Azure Database for MySQL** w polu wyszukiwania, aby szybko znaleźć tę usługę.
![2-1 Przechodzenie do bazy danych MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. Kliknij kafelek **Azure Database for MySQL**, a następnie kliknij pozycję **Utwórz**.

W tym przykładzie wprowadź w formularzu usługi Azure Database for MySQL poniższe informacje:

| **Ustawienie** | **Sugerowana wartość** | **Opis pola** |
|---|---|---|
| *Nazwa serwera* | myserver4demo  | Nazwa serwera musi być unikatowa w skali globalnej. |
| *Subskrypcja* | mysubscription | Wybierz subskrypcję z listy rozwijanej. |
| *Grupa zasobów* | myresourcegroup | Utwórz grupę zasobów lub wybierz istniejącą. |
| *Identyfikator logowania administratora serwera* | myadmin | Skonfiguruj nazwę konta administratora. |
| *Hasło* |  | Ustaw silne hasło do konta administratora. |
| *Potwierdź hasło* |  | Potwierdź hasło do konta administratora. |
| *Lokalizacja* |  | Wybierz dostępny region. |
| *Wersja* | 5.7 | Wybierz najnowszą wersję. |
| *Konfigurowanie wydajności* | Podstawowa, 50 jednostek obliczeniowych, 50 GB  | Wybierz pozycje **Warstwa cenowa**, **Jednostki obliczeniowe** i **Magazyn (GB)**, a następnie kliknij przycisk **OK**. |
| *Przypnij do pulpitu nawigacyjnego* | Zaznacz | Zaleca się zaznaczenie tego pola w celu ułatwienia znajdowania serwera. |
Następnie kliknij pozycję **Utwórz**. Po około dwóch minutach nowy serwer usługi Azure Database for MySQL będzie działać w chmurze. Aby monitorować proces wdrażania, możesz kliknąć przycisk **Powiadomienia** na pasku narzędzi.

## <a name="configure-firewall"></a>Konfigurowanie zapory
Bazy danych usługi Azure Database for MySQL są chronione przez zaporę. Domyślnie wszystkie połączenia z serwerem i znajdującymi się na nim bazami danych są odrzucane. Przed nawiązaniem pierwszego połączenia z usługą Azure Database for MySQL skonfiguruj zaporę, aby dodać publiczny adres IP (lub zakres adresów IP) sieci maszyny klienta.

1. Kliknij nowo utworzony serwer, a następnie kliknij pozycję **Zabezpieczenia połączeń**.
   ![3-1 Zabezpieczenia połączeń](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. W tym miejscu możesz skorzystać z funkcji **Dodaj mój adres IP** lub skonfigurować reguły zapory. Pamiętaj, aby po utworzeniu reguł kliknąć przycisk **Zapisz**.
Teraz możesz nawiązać połączenie z serwerem za pomocą narzędzia wiersza polecenia mysql lub narzędzia z graficznym interfejsem użytkownika MySQL Workbench.

> [!TIP]
> Serwer usługi Azure Database for MySQL komunikuje się przez port 3306. Jeśli próbujesz nawiązać połączenie z sieci firmowej, ruch wychodzący na porcie 3306 może być zablokowany przez zaporę sieciową. Jeśli tak się stanie, nie będzie można nawiązać połączenia z serwerem usługi Azure MySQL, chyba że dział IT otworzy port 3306.

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu
Uzyskaj z witryny Azure Portal w pełni kwalifikowaną **Nazwę serwera** i **Nazwę logowania administratora serwera** dla serwera usługi Azure Database for MySQL. W pełni kwalifikowana nazwa serwera służy do nawiązywania połączenia z serwerem przy użyciu narzędzia wiersza polecenia mysql. 

1. W witrynie [Azure Portal](https://portal.azure.com/) kliknij pozycję **Wszystkie zasoby** w menu po lewej stronie, wpisz nazwę, a następnie wyszukaj serwer usługi Azure Database for MySQL. Wybierz nazwę serwera, aby wyświetlić szczegóły.

2. W obszarze pod nagłówkiem Ustawienia kliknij pozycję **Właściwości**. Zanotuj wartości **NAZWA SERWERA** i **NAZWA LOGOWANIA ADMINISTRATORA SERWERA**. Aby skopiować zawartość do schowka, możesz nacisnąć przycisk kopiowania obok każdego pola.
   ![4-2 Właściwości serwera](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

W tym przykładzie nazwa serwera to *myserver4demo.mysql.database.azure.com*, a identyfikator logowania administratora serwera to *myadmin@myserver4demo*.

## <a name="connect-to-the-server-using-mysql"></a>Nawiązywanie połączenia z serwerem za pomocą narzędzia wiersza polecenia mysql
Nawiąż połączenie z serwerem usługi Azure Database for MySQL za pomocą [narzędzia wiersza polecenia mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html). Narzędzie wiersza polecenia mysql możesz uruchomić z poziomu usługi Azure Cloud Shell w przeglądarce lub z poziomu własnej maszyny, korzystając z zainstalowanych lokalnie narzędzi mysql. Aby uruchomić usługę Azure Cloud Shell, kliknij przycisk `Try It` w bloku kodu w tym artykule lub przejdź do witryny Azure Portal i kliknij ikonę `>_` na pasku narzędzi w prawym górnym rogu. 

Aby nawiązać połączenie, wpisz poniższe polecenie:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a>Tworzenie pustej bazy danych
Po nawiązaniu połączenia z serwerem utwórz pustą bazę danych, z którą będziesz pracować.
```sql
CREATE DATABASE mysampledb;
```

W wierszu polecenia uruchom poniższe polecenie, aby przełączyć połączenie na tę nowo utworzoną bazę danych:
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Tworzenie tabel w bazie danych
Teraz, gdy wiesz, jak nawiązać połączenie z bazą danych usługi Azure Database for MySQL, możesz wykonać niektóre podstawowe zadania:

Najpierw utwórz tabelę i załaduj do niej dane. Utwórz tabelę z informacjami o spisie.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Ładowanie danych do tabel
Teraz, gdy masz tabelę, wstaw do niej dane. W otwartym oknie wiersza polecenia uruchom następujące zapytanie, aby wstawić wiersze danych.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

W utworzonej wcześniej tabeli masz teraz dwa wiersze przykładowych danych.

## <a name="query-and-update-the-data-in-the-tables"></a>Wykonywanie zapytania względem danych w tabelach i aktualizowanie tych danych
Wykonaj następujące zapytanie, aby pobrać informacje z tabeli bazy danych.
```sql
SELECT * FROM inventory;
```

Możesz także zaktualizować dane w tabelach.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Wiersz jest aktualizowany podczas pobierania danych.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Przywracanie bazy danych do określonego punktu w czasie
Załóżmy, że przypadkowo usunięto ważną tabelę bazy danych i danych nie można w prosty sposób odzyskać. Usługa Azure Database for MySQL umożliwia przywrócenie serwera do punktu w czasie i utworzenie kopii baz danych na nowym serwerze. Przy użyciu tego nowego serwera można odzyskać usunięte dane. Poniższa procedura opisuje przywrócenie przykładowego serwera do punktu przed dodaniem tabeli.

1. W witrynie Azure Portal znajdź usługę Azure Database for MySQL. Na stronie **Przegląd** kliknij pozycję **Przywróć** na pasku narzędzi. Zostanie otwarta strona Przywracanie.

   ![10-1 Przywracanie bazy danych](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. Wypełnij formularz **Przywracanie** wymaganymi informacjami.
   
   ![10-2 Formularz Przywracanie](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Punkt przywracania**: wybierz punkt w czasie, do którego chcesz wykonać przywrócenie, w podanym przedziale czasowym. Pamiętaj o przekonwertowaniu lokalnej strefy czasowej na czas UTC.
   - **Przywróć na nowy serwer**: podaj nazwę nowego serwera, do którego chcesz wykonać przywrócenie.
   - **Lokalizacja**: region jest taki sam, jak w przypadku serwera źródłowego, i nie można go zmienić.
   - **Warstwa cenowa**: warstwa cenowa jest taka sama, jak w przypadku serwera źródłowego, i nie można jej zmienić.
   
3. Kliknij przycisk **OK**, aby przywrócić serwer do [punktu w czasie](./howto-restore-server-portal.md) przed usunięciem tabeli. Przywrócenie serwera powoduje utworzenie nowej kopii serwera od określonego punktu w czasie. 

## <a name="next-steps"></a>Następne kroki
W tym samouczku opisano, jak wykonywać następujące czynności, używając witryny Azure Portal:

> [!div class="checklist"]
> * Tworzenie usługi Azure Database for MySQL
> * Konfigurowanie zapory serwera
> * Tworzenie bazy danych za pomocą narzędzia wiersza polecenia mysql
> * Ładowanie przykładowych danych
> * Zapytania o dane
> * Aktualizowanie danych
> * Przywracanie danych

> [!div class="nextstepaction"]
> [Jak połączyć aplikacje z usługą Azure Database for MySQL](./howto-connection-string.md)
