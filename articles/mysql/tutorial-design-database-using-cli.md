---
title: "Projektowanie pierwszej bazy danych usługi Azure Database for MySQL — interfejs wiersza polecenia platformy Azure | Microsoft Docs"
description: "W tym samouczku wyjaśniono, jak utworzyć serwer oraz bazę danych usługi Azure Database for MySQL i zarządzać nimi przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 11/28/2017
ms.custom: mvc
ms.openlocfilehash: 5f323086ce66a504188c1834d20873a52a990311
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Projektowanie pierwszej bazy danych usługi Azure Database for MySQL

Usługa Azure Database for MySQL jest usługą relacyjnej bazy danych w chmurze firmy Microsoft opartą na aparacie bazy danych MySQL Community Edition. W tym samouczku dowiesz się, jak wykonywać następujące czynności, używając interfejsu wiersza polecenia platformy Azure:

> [!div class="checklist"]
> * Tworzenie usługi Azure Database for MySQL
> * Konfigurowanie zapory serwera
> * Tworzenie bazy danych za pomocą [narzędzia wiersza polecenia mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html)
> * Ładowanie przykładowych danych
> * Zapytania o dane
> * Aktualizowanie danych
> * Przywracanie danych

Aby uruchamiać bloki kodu z tego samouczka, możesz użyć usługi Azure Cloud Shell w przeglądarce lub [zainstalować interfejs wiersza polecenia platformy Azure w wersji 2.0]( /cli/azure/install-azure-cli) na swoim komputerze.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten artykuł będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

Jeśli masz wiele subskrypcji, wybierz odpowiednią subskrypcję, w której zasób istnieje lub dla której są za niego naliczane opłaty. Wybierz określony identyfikator subskrypcji na Twoim koncie za pomocą polecenia [az account set](/cli/azure/account#az_account_set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
[Grupę zasobów platformy Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) można utworzyć za pomocą polecenia [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi w formie grupy.

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `mycliresource` w lokalizacji `westus`.

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Tworzenie serwera usługi Azure Database for MySQL
Serwer usługi Azure Database for MySQL można utworzyć za pomocą polecenia az mysql server create. Serwer umożliwia zarządzanie wieloma bazami danych. Zwykle dla każdego projektu lub użytkownika używana jest oddzielna baza danych.

W poniższym przykładzie w regionie `westus` w grupie zasobów `mycliresource` jest tworzony serwer usługi Azure Database for MySQL o nazwie `mycliserver`. Serwer ma identyfikator logowania administratora o nazwie `myadmin` i hasło `Password01!`. Serwer jest tworzony w ramach warstwy wydajności **Podstawowa** i z użyciem **50** jednostek obliczeniowych współdzielonych między wszystkimi bazami danych na tym serwerze. Możesz skalować zasoby obliczeniowe i magazyn w górę lub w dół w zależności od potrzeb aplikacji.

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Konfigurowanie reguły zapory
Utwórz regułę zapory na poziomie serwera usługi Azure Database for MySQL za pomocą polecenia az mysql server firewall-rule create. Reguła zapory na poziomie serwera pozwala aplikacji zewnętrznej, takiej jak narzędzie wiersza polecenia **mysql** lub program MySQL Workbench, na nawiązywanie połączeń z Twoim serwerem przez zaporę usługi Azure MySQL. 

Poniższy przykład obejmuje tworzenie reguły zapory dla wstępnie zdefiniowanego zakresu adresów. W tym przykładzie przedstawiono cały możliwy zakres adresów IP.

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

## <a name="connect-to-the-server-using-mysql"></a>Nawiązywanie połączenia z serwerem za pomocą narzędzia wiersza polecenia mysql
Nawiąż połączenie z serwerem usługi Azure Database for MySQL za pomocą [narzędzia wiersza polecenia mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html). W tym przykładzie polecenie to:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Tworzenie pustej bazy danych
Po nawiązaniu połączenia z serwerem utwórz pustą bazę danych.
```sql
mysql> CREATE DATABASE mysampledb;
```

W wierszu polecenia uruchom poniższe polecenie, aby przełączyć połączenie na tę nowo utworzoną bazę danych:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Tworzenie tabel w bazie danych
Teraz, gdy wiesz, jak nawiązać połączenie z bazą danych usługi Azure Database for MySQL, wykonaj niektóre podstawowe zadania:

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
Załóżmy, że ta tabela została przypadkowo usunięta. W takiej sytuacji niełatwo jest odzyskać dane. Usługa Azure Database for MySQL umożliwia powrót do dowolnego punktu w czasie w ciągu maksymalnie 35 ostatnich dni i przywrócenie tego punktu w czasie do nowego serwera. Przy użyciu tego nowego serwera można odzyskać usunięte dane. Poniższa procedura opisuje przywrócenie przykładowego serwera do punktu przed dodaniem tabeli.

Do wykonania przywrócenia potrzebne będą następujące informacje:

- Punkt przywracania: wybierz punkt w czasie przed zmianą serwera. Musi mieć wartość większą od lub równą wartości Najstarsza kopia zapasowa źródłowej bazy danych.
- Serwer docelowy: podaj nazwę nowego serwera, do którego chcesz wykonać przywrócenie
- Serwer źródłowy: podaj nazwę serwera, z którego chcesz wykonać przywrócenie
- Lokalizacja: nie można wybrać regionu; domyślnie wartość jest taka sama jak w przypadku serwera źródłowego

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

Przywróć serwer do [punktu w czasie](./howto-restore-server-portal.md) przed usunięciem tabeli. Przywrócenie serwera do innego punktu w czasie spowoduje utworzenie duplikatu nowego serwera jako oryginalnego serwera od określonego przez Ciebie punkcie w czasie, o ile zawiera się on w okresie przechowywania dla Twojej [warstwy usług](./concepts-service-tiers.md).

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Tworzenie usługi Azure Database for MySQL
> * Konfigurowanie zapory serwera
> * Tworzenie bazy danych za pomocą [narzędzia wiersza polecenia mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html)
> * Ładowanie przykładowych danych
> * Zapytania o dane
> * Aktualizowanie danych
> * Przywracanie danych

> [!div class="nextstepaction"]
> [Usługa Azure Database for MySQL — przykłady interfejsu wiersza polecenia platformy Azure](./sample-scripts-azure-cli.md)
