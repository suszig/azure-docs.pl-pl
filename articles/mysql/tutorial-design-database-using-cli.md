---
title: "Projektowanie pierwszej bazy danych Azure, aby baza danych MySQL — wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "W tym samouczku opisano sposób tworzenia i zarządzania nimi Azure bazy danych MySQL serwer i bazę danych, korzystając z wiersza polecenia Azure CLI 2.0."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 11/28/2017
ms.custom: mvc
ms.openlocfilehash: 2d23c37688ab7f19beba920f7ddd4043cd117503
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Projektowanie pierwszej bazy danych Azure, aby baza danych MySQL

Bazy danych platformy Azure dla programu MySQL jest usługą relacyjnych baz danych w chmurze firmy Microsoft, oparte na aparacie bazy danych MySQL Community Edition. W tym samouczku używasz interfejsu wiersza polecenia Azure (interfejsu wiersza polecenia) i inne narzędzia Aby dowiedzieć się, jak:

> [!div class="checklist"]
> * Utwórz bazę danych systemu Azure dla programu MySQL
> * Konfigurowanie zapory serwera
> * Użyj [narzędzia wiersza polecenia mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) tworzenia bazy danych
> * Ładuj dane przykładowe
> * Zapytania o dane
> * Aktualizowanie danych
> * Przywracanie danych

Można użyć powłoki chmury Azure w przeglądarce lub [zainstalować Azure CLI 2.0]( /cli/azure/install-azure-cli) na komputerze do uruchomienia bloki kodu w tym samouczku.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten artykuł będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

Jeśli masz wiele subskrypcji, wybierz odpowiednią subskrypcję, w której zasób istnieje lub dla której są za niego naliczane opłaty. Wybierz określony identyfikator subskrypcji na Twoim koncie za pomocą polecenia [az account set](/cli/azure/account#az_account_set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Utwórz [grupy zasobów platformy Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) z [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) polecenia. Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi w formie grupy.

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `mycliresource` w lokalizacji `westus`.

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Tworzenie serwera usługi Azure Database for MySQL
Utwórz bazę danych Azure, MySQL serwera z serwerem mysql az Utwórz polecenie. Serwer umożliwia zarządzanie wieloma bazami danych. Zwykle dla każdego projektu lub użytkownika używana jest oddzielna baza danych.

W poniższym przykładzie w regionie `westus` w grupie zasobów `mycliresource` jest tworzony serwer usługi Azure Database for MySQL o nazwie `mycliserver`. Serwer ma identyfikator logowania administratora o nazwie `myadmin` i hasło `Password01!`. Serwer jest tworzony w ramach warstwy wydajności **Podstawowa** i z użyciem **50** jednostek obliczeniowych współdzielonych między wszystkimi bazami danych na tym serwerze. Możesz skalować zasoby obliczeniowe i magazyn w górę lub w dół w zależności od potrzeb aplikacji.

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Konfigurowanie reguły zapory
Utwórz bazę danych Azure, aby utworzyć regułę zapory poziomu serwera MySQL z az mysql reguły zapory serwera — polecenie. Reguły zapory poziomu serwera umożliwia aplikacji zewnętrznych, takich jak **mysql** narzędzia wiersza polecenia lub MySQL Workbench, aby nawiązać połączenie z serwerem za pośrednictwem zapory usługi MySQL na platformie Azure. 

Poniższy przykład powoduje utworzenie reguły zapory dla zakresu adresów wstępnie zdefiniowane. Ten przykład przedstawia całą możliwe zakres adresów IP.

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-the-connection-information"></a>Pobieranie informacji o połączeniu

Aby nawiązać połączenie z serwerem, musisz podać informacje o hoście i poświadczenia dostępu.
```azurecli-interactive
az mysql server show --resource-group mycliresource --name mycliserver
```

Wynik jest w formacie JSON. Zanotuj wartości **fullyQualifiedDomainName** i **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-to-the-server-using-mysql"></a>Połącz z serwerem przy użyciu mysql
Użyj [narzędzia wiersza polecenia mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) nawiązanie połączenia do bazy danych Azure, serwer MySQL. W tym przykładzie to polecenie:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Tworzenie pustej bazy danych
Po nawiązaniu połączenia z serwerem, należy utworzyć pustą bazę danych.
```sql
mysql> CREATE DATABASE mysampledb;
```

W wierszu polecenia Uruchom następujące polecenie, aby przełączyć połączenia do nowo utworzonej bazy danych:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Tworzenie tabel w bazie danych
Teraz, Znając sposób nawiązywania połączenia z bazą danych Azure dla bazy danych MySQL, należy wykonać kilka podstawowych zadań:

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
Załóżmy, że zostanie przypadkowo usunięte w tej tabeli. Jest to coś, co nie można łatwo odzyskać z. Bazy danych platformy Azure dla programu MySQL umożliwia wróć do dowolnego punktu w czasie w górę ostatnich 35 dni i przywrócić ten punkt w czasie na nowy serwer. Możesz użyć tego nowego serwera, aby odzyskać usunięte dane. Poniższe kroki należy przywrócić działanie serwera próbki do punktu przed tabeli został dodany.

Do przywrócenia potrzebne są następujące informacje:

- Punkt przywracania: Wybierz w momencie po serwer został zmieniony. Musi być większa niż lub równa wartości kopii zapasowej najstarsze źródłowej bazy danych.
- Serwer docelowy: Podaj nową nazwę serwera mają zostać przywrócone
- Serwer źródłowy: Podaj nazwę serwera, aby przywrócić z
- Lokalizacja: Nie można wybrać region, domyślnie jest taki sam jak serwer źródłowy

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

Aby przywrócić serwer i [Przywracanie do punktu w czasie](./howto-restore-server-portal.md) przed tabeli został usunięty. Przywracanie serwera do innego punktu w czasie tworzy zduplikowane nowy serwer jako oryginalnego serwera, począwszy od punktu w czasie, możesz określić, pod warunkiem, że w okresie przechowywania dla Twojego [warstwy usług](./concepts-service-tiers.md).

## <a name="next-steps"></a>Następne kroki
W tym samouczku przedstawiono do:
> [!div class="checklist"]
> * Utwórz bazę danych systemu Azure dla programu MySQL
> * Konfigurowanie zapory serwera
> * Użyj [narzędzia wiersza polecenia mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) tworzenia bazy danych
> * Ładuj dane przykładowe
> * Zapytania o dane
> * Aktualizowanie danych
> * Przywracanie danych

> [!div class="nextstepaction"]
> [Bazy danych platformy Azure dla programu MySQL — przykłady wiersza polecenia platformy Azure](./sample-scripts-azure-cli.md)
