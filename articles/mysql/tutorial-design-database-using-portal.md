---
title: "Projektowanie pierwszej bazy danych Azure, aby baza danych MySQL — Azure Portal | Dokumentacja firmy Microsoft"
description: "W tym samouczku opisano sposób tworzenia i zarządzania nimi Azure bazy danych MySQL serwera i bazy danych przy użyciu portalu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.topic: tutorial
ms.date: 11/03/2017
ms.custom: mvc
ms.openlocfilehash: a7f38484e000b05a57cad9bc95abb255414d0162
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Projektowanie pierwszej bazy danych Azure, aby baza danych MySQL
Azure Database for MySQL to usługa zarządzana, która umożliwia uruchamianie i skalowanie w chmurze baz danych MySQL o wysokiej dostępności, a także zarządzanie nimi. Przy użyciu portalu Azure, można łatwo zarządzać serwerem i projektowanie bazy danych.

W tym samouczku, użyj portalu Azure Aby dowiedzieć się, jak:

> [!div class="checklist"]
> * Utwórz bazę danych systemu Azure dla programu MySQL
> * Konfigurowanie zapory serwera
> * Utwórz bazę danych za pomocą narzędzia wiersza polecenia mysql
> * Ładuj dane przykładowe
> * Zapytania o dane
> * Aktualizowanie danych
> * Przywracanie danych

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal
Otwórz przeglądarkę sieci web Ulubione, a następnie odwiedź [portalu Microsoft Azure](https://portal.azure.com/). Wprowadź swoje poświadczenia, aby zalogować się do portalu. Widok domyślny to pulpit nawigacyjny usług.

## <a name="create-an-azure-database-for-mysql-server"></a>Tworzenie serwera usługi Azure Database for MySQL
Serwer usługi Azure Database for MySQL jest tworzony za pomocą zdefiniowanego zestawu [zasobów obliczeniowych i przestrzeni dyskowej](./concepts-compute-unit-and-storage.md). Serwer jest tworzony w ramach [grupy zasobów Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).

1. Przejdź do **baz danych** > **bazy danych platformy Azure dla programu MySQL**. Jeśli nie można znaleźć serwera MySQL w obszarze **baz danych** kategorii, kliknij przycisk **zobaczyć wszystkie** można wyświetlić wszystkie dostępne bazy danych usługi. Możesz także wpisać **bazy danych Azure dla programu MySQL** w polu wyszukiwania, aby szybko znaleźć usługi.
![2-1, przejdź do MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. Kliknij przycisk **bazy danych Azure dla programu MySQL** Kafelek, a następnie kliknij przycisk **Utwórz**.

W tym przykładzie Wypełnianie bazy danych Azure MySQL formularza z następującymi informacjami:

| **Ustawienie** | **Sugerowana wartość** | **Opis pola** |
|---|---|---|
| *Nazwa serwera* | myserver4demo  | Nazwa serwera musi być unikatowa w skali globalnej. |
| *Subskrypcja* | mysubscription | Wybierz subskrypcję z listy rozwijanej. |
| *Grupa zasobów* | myresourcegroup | Utwórz grupę zasobów lub wybierz istniejącą. |
| *Identyfikator logowania administratora serwera* | myadmin | Konfiguracja nazwy konta administratora. |
| *Hasło* |  | Ustaw silne hasło do konta administratora. |
| *Potwierdź hasło* |  | Potwierdź hasło do konta administratora. |
| *Lokalizacja* |  | Wybierz dostępny region. |
| *Wersja* | 5.7 | Wybierz najnowszą wersję. |
| *Konfigurowanie wydajności* | Podstawowe, 50 obliczeniowe jednostki, 50 GB  | Wybierz pozycje **Warstwa cenowa**, **Jednostki obliczeniowe** i **Magazyn (GB)**, a następnie kliknij przycisk **OK**. |
| *Przypnij do pulpitu nawigacyjnego* | Zaznacz | Zaleca się zaznaczenie tego pola w celu ułatwienia znajdowania serwera. |
Następnie kliknij pozycję **Utwórz**. Po około dwóch minutach nowy serwer usługi Azure Database for MySQL będzie działać w chmurze. Możesz kliknąć **powiadomienia** przycisk na pasku narzędzi do monitorowania procesu wdrażania.

## <a name="configure-firewall"></a>Konfigurowanie zapory
Azure bazy danych dla programu MySQL są chronione przez zaporę. Domyślnie wszystkie połączenia z serwerem i bazy danych w obrębie serwera są odrzucane. Przed połączeniem się z bazą danych Azure dla programu MySQL po raz pierwszy, należy skonfigurować zaporę tak, aby dodać adres IP komputera klienckiego sieci publicznej (lub zakres adresów IP).

1. Kliknij nowo utworzonego serwera, a następnie kliknij przycisk **zabezpieczenia połączeń**.
   ![Zabezpieczenia połączeń 3-1](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. Możesz **dodać Moje IP**, lub skonfigurować reguły zapory w tym miejscu. Pamiętaj, aby po utworzeniu reguł kliknąć przycisk **Zapisz**.
Teraz można podłączyć do serwera przy użyciu narzędzia wiersza polecenia mysql lub MySQL Workbench graficznego interfejsu użytkownika.

> [!TIP]
> Azure bazy danych MySQL serwera komunikuje się za pośrednictwem portu 3306. Jeśli próbujesz nawiązać połączenie z sieci firmowej, ruch wychodzący na porcie 3306 może być zablokowany przez zaporę sieciową. Jeśli tak, nie można nawiązać serwera Azure MySQL, chyba że dział IT otwiera port 3306.

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu
Pobierz w pełni kwalifikowaną **nazwy serwera** i **nazwę logowania administratora serwera** bazy danych Azure, aby serwer MySQL z portalu Azure. Nazwa FQDN serwera umożliwia łączenie się z serwerem za pomocą narzędzia wiersza polecenia mysql. 

1. W [portalu Azure](https://portal.azure.com/), kliknij przycisk **wszystkie zasoby** z menu po lewej stronie, wpisz nazwę, a wyszukiwanie Azure bazy danych MySQL serwera. Wybierz nazwę serwera, aby wyświetlić szczegóły.

2. W obszarze Ustawienia kliknij pozycję **właściwości**. Należy zanotować **nazwy serwera** i **nazwę logowania administratora serwera**. Może kliknij przycisk Kopiuj obok każdego pola, aby skopiować do Schowka.
   ![właściwości serwera 4-2](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

W tym przykładzie nazwa serwera to *myserver4demo.mysql.database.azure.com*, a identyfikator logowania administratora serwera to *myadmin@myserver4demo*.

## <a name="connect-to-the-server-using-mysql"></a>Połącz z serwerem przy użyciu mysql
Nawiąż połączenie z serwerem usługi Azure Database for MySQL za pomocą [narzędzia wiersza polecenia mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html). Z poziomu powłoki chmury Azure w przeglądarce lub z własnych komputera przy użyciu narzędzia mysql zainstalowane lokalnie, można uruchomić narzędzie wiersza polecenia mysql. Aby uruchomić powłoki chmury Azure, kliknij przycisk `Try It` przycisk blokiem kodu w tym artykule, lub odwiedź Azure portal i kliknij przycisk `>_` ikonę w prawym górnym pasku narzędzi. 

Aby nawiązać połączenie, wpisz poniższe polecenie:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a>Tworzenie pustej bazy danych
Po nawiązaniu połączenia z serwerem, Utwórz pustą bazę danych do pracy z.
```sql
CREATE DATABASE mysampledb;
```

W wierszu polecenia Uruchom następujące polecenie, aby przełączyć połączenia do nowo utworzonej bazy danych:
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Tworzenie tabel w bazie danych
Teraz, gdy wiesz, jak nawiązać połączenia z bazą danych Azure dla bazy danych MySQL, można wykonać niektóre podstawowe zadania:

Najpierw utwórz tabelę i załaduj go z niektórych danych. Teraz utworzyć tabelę, która przechowuje informacje dotyczące spisu.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Ładowanie danych do tabel
Teraz, gdy masz tabeli wstawić dane do niego. W oknie Otwórz okno wiersza polecenia Uruchom następujące zapytanie, aby wstawić niektórych wierszy danych.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Masz teraz dwa wiersze przykładowych danych do tabeli utworzony wcześniej.

## <a name="query-and-update-the-data-in-the-tables"></a>Zapytania i zaktualizować dane w tabelach
Wykonaj następujące zapytanie, aby pobrać informacje z tabeli bazy danych.
```sql
SELECT * FROM inventory;
```

Należy również zaktualizować dane w tabelach.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Wiersz jest odpowiednio aktualizowany podczas pobierania danych.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Przywracanie bazy danych do określonego punktu w czasie
Wyobraź sobie przypadkowo usunięto tabelę ważne bazy danych i nie można łatwo odzyskać dane. Bazy danych platformy Azure dla programu MySQL umożliwia przywrócenie serwera do punktu w czasie, tworząc kopię bazy danych do nowego serwera. Możesz użyć tego nowego serwera, aby odzyskać usunięte dane. Poniższe kroki należy przywrócić działanie serwera próbki do punktu przed tabeli został dodany.

1. W portalu Azure Znajdź bazy danych Azure dla programu MySQL. Na **omówienie** kliknij przycisk **przywrócić** na pasku narzędzi. Zostanie otwarta strona przywracania.

   ![10-1 przywracania bazy danych](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. Wypełnianie **przywrócić** formularza z wymaganych informacji.
   
   ![Formularz przywracania 10-2](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Punkt przywracania**: Wybierz w momencie, który chcesz przywrócić, w ramach czasowych na liście. Upewnij się przekonwertować sieci lokalnej strefie czasowej UTC.
   - **Przywracanie do nowego serwera**: Podaj nową nazwę serwera mają zostać przywrócone.
   - **Lokalizacja**: region jest taki sam jak serwer źródłowy i nie można zmienić.
   - **Warstwa cenowa**: warstwa cenowa jest taka sama, jak na serwerze źródłowym i nie można zmienić.
   
3. Kliknij przycisk **OK** Aby przywrócić serwer do [Przywracanie do punktu w czasie](./howto-restore-server-portal.md) przed tabeli został usunięty. Przywracanie serwera tworzy nową kopię serwera, począwszy od punktu w czasie, które określisz. 

## <a name="next-steps"></a>Następne kroki
W tym samouczku można użyć Azure portalu do zapamiętanych jak:

> [!div class="checklist"]
> * Utwórz bazę danych systemu Azure dla programu MySQL
> * Konfigurowanie zapory serwera
> * Utwórz bazę danych za pomocą narzędzia wiersza polecenia mysql
> * Ładuj dane przykładowe
> * Zapytania o dane
> * Aktualizowanie danych
> * Przywracanie danych

> [!div class="nextstepaction"]
> [Jak połączyć aplikacje do bazy danych Azure dla programu MySQL](./howto-connection-string.md)
