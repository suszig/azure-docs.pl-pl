---
title: "Projektowanie pierwszą bazę danych Azure do PostgreSQL przy użyciu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Ten samouczek pokazuje, jak projektować pod kątem PostgreSQL pierwszą bazę danych Azure przy użyciu wiersza polecenia platformy Azure."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 06/13/2017
ms.openlocfilehash: d753772adeb9064f391f1e3846de654bdb60facf
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-azure-cli"></a>Projektowanie pierwszą bazę danych Azure do PostgreSQL przy użyciu wiersza polecenia platformy Azure 
W tym samouczku używasz interfejsu wiersza polecenia Azure (interfejsu wiersza polecenia) i inne narzędzia Aby dowiedzieć się, jak:
> [!div class="checklist"]
> * Tworzenie serwera usługi Azure Database for PostgreSQL
> * Konfigurowanie zapory serwera
> * Użyj [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) narzędzie do tworzenia bazy danych
> * Ładuj dane przykładowe
> * Zapytania o dane
> * Aktualizowanie danych
> * Przywracanie danych

Można użyć powłoki chmury Azure w przeglądarce lub [zainstalować Azure CLI 2.0]( /cli/azure/install-azure-cli) na komputerze do uruchomienia poleceń w tym samouczku.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten temat będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

Jeśli masz wiele subskrypcji, wybierz odpowiednią subskrypcję, w której zasób istnieje lub dla której są za niego naliczane opłaty. Wybierz określony identyfikator subskrypcji na Twoim koncie za pomocą polecenia [az account set](/cli/azure/account#set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Utwórz [grupę zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi w formie grupy. Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `myresourcegroup` w lokalizacji `westus`.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a>Tworzenie serwera usługi Azure Database for PostgreSQL
Utwórz [serwer usługi Azure Database for PostgreSQL](overview.md) za pomocą polecenia [az postgres server create](/cli/azure/postgres/server#create). Serwer zawiera grupę baz danych zarządzanych jako grupa. 

Poniższy przykład tworzy serwer o nazwie `mypgserver-20170401` w grupie zasobów `myresourcegroup` z identyfikator logowania administratora serwera `mylogin`. Nazwa serwera jest mapowana na nazwę DNS i dlatego musi być globalnie unikatowa w ramach platformy Azure. Zastąp zmienną `<server_admin_password>` swoją własną wartością.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401 --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> Login i hasło administratora serwera określone w tym miejscu będą wymagane do logowania do serwera i jego baz danych w późniejszej części tego przewodnika Szybki start. Zapamiętaj lub zapisz te informacje do wykorzystania w przyszłości.

Domyślnie baza danych **postgres** zostanie utworzona na Twoim serwerze. Baza danych [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) to domyślna baza danych przeznaczona do użycia dla użytkowników oraz na potrzeby narzędzi i aplikacji innych firm. 


## <a name="configure-a-server-level-firewall-rule"></a>Konfigurowanie reguły zapory na poziomie serwera

Utwórz regułę zapory na poziomie serwera Azure PostgreSQL za pomocą polecenia [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create). Reguła zapory na poziomie serwera pozwala aplikacji zewnętrznej, takiej jak narzędzie [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) lub [PgAdmin](https://www.pgadmin.org/), na nawiązywanie połączeń z Twoim serwerem przez zaporę usługi Azure PostgreSQL. 

Możesz ustawić regułę zapory uwzględniającą zakres adresów IP, aby mieć możliwość nawiązywania połączeń z Twojej sieci. W poniższym przykładzie użyto [az postgres reguły zapory serwera — Utwórz](/cli/azure/postgres/server/firewall-rule#create) do utworzenia reguły zapory `AllowAllIps` umożliwiająca połączenie z dowolnego adresu IP. Aby otworzyć wszystkie adresy IP, użyj wartości 0.0.0.0 jako początkowego adresu IP i wartości 255.255.255.255 jako adresu końcowego.

Aby ograniczyć dostęp do serwera Azure PostgreSQL tylko z siecią, można ustawić reguły zapory, która obejmuje tylko zakres adresów IP sieci firmowej.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Serwer Azure PostgreSQL komunikuje się przez port 5432. Podczas nawiązywania połączenia z sieci firmowej ruch wychodzący przez port 5432 może być blokowany przez zaporę sieciową. Aby było możliwe nawiązywanie połączenia z serwerem usługi Azure SQL Database, poproś dział IT o otwarcie portu 5432.
>

## <a name="get-the-connection-information"></a>Uzyskiwanie informacji o połączeniu

Aby nawiązać połączenie z serwerem, musisz podać informacje o hoście i poświadczenia dostępu.
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

Wynik jest w formacie JSON. Zanotuj wartości **administratorLogin** i **fullyQualifiedDomainName**.
```json
{
  "administratorLogin": "mylogin",
  "fullyQualifiedDomainName": "mypgserver-20170401.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mypgserver-20170401",
  "location": "westus",
  "name": "mypgserver-20170401",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "PGSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 51200,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-to-azure-database-for-postgresql-database-using-psql"></a>Łączenia z bazą danych Azure PostgreSQL bazy danych przy użyciu psql
Jeśli komputer kliencki ma zainstalowany PostgreSQL, można użyć lokalnego wystąpienia programu [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), lub Azure Cloud Console do nawiązywania połączenia z serwerem Azure PostgreSQL. Użyj teraz narzędzia wiersza polecenia psql, aby nawiązać połączenie z serwerem usługi Azure Database for PostgreSQL.

1. Uruchom następujące polecenie psql nawiązać połączenia z bazą danych Azure dla bazy danych PostgreSQL:
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Na przykład poniższe polecenie nawiązuje połączenie z domyślną bazą danych o nazwie **postgres** na Twoim serwerze PostgreSQL **mypgserver-20170401.postgres.database.azure.com** za pomocą poświadczeń dostępu. Gdy zostanie wyświetlony monit o podanie hasła, wprowadź wybrane hasło `<server_admin_password>`.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 ---dbname=postgres
```

2.  Po nawiązaniu połączenia z serwerem, Utwórz pustą bazę danych w wierszu:
```sql
CREATE DATABASE mypgsqldb;
```

3.  W wierszu polecenia wykonaj poniższe polecenie, aby przełączyć połączenie na nowo utworzoną bazę danych **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="create-tables-in-the-database"></a>Tworzenie tabel w bazie danych
Teraz, Znając sposób nawiązywania połączenia z bazą danych Azure dla PostgreSQL możemy przekazywane sposób wykonania zadania podstawowe.

Firma Microsoft najpierw utwórz tabelę i załaduj go z niektórych danych. Ta funkcja pozwala utworzyć tabelę, która śledzi informacje o spisie:
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

Teraz wyświetlić nowo utworzona tabela listy tabel, wpisując:
```sql
\dt
```

## <a name="load-data-into-the-table"></a>Ładowanie danych do tabeli
Teraz, gdy mamy tabeli możemy wstawić niektóre dane do niego. W oknie Otwórz okno wiersza polecenia Uruchom następującą kwerendę, aby wstawić niektórych wierszy danych:
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Dwa wiersze przykładowych danych zostało dodane do tabeli utworzony wcześniej.

## <a name="query-and-update-the-data-in-the-tables"></a>Zapytania i zaktualizować dane w tabelach
Wykonaj następujące zapytanie, aby pobrać informacje z tabeli magazynu: 
```sql
SELECT * FROM inventory;
```

Można także zaktualizować dane w tabeli spisu:
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Podczas pobierania danych, można wyświetlić zaktualizowane wartości:
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Przywracanie bazy danych do określonego punktu w czasie
Załóżmy, że przypadkowo usunięto tabelę. Jest to coś, co nie można łatwo odzyskać z. Bazy danych platformy Azure dla PostgreSQL umożliwia wróć do dowolnego punktu w czasie (maksymalnie 7 dni w Basic) i 35 dni w standardzie i przywrócić tego punktu w czasie na nowy serwer. Możesz użyć tego nowego serwera, aby odzyskać usunięte dane. 

Polecenie przywraca przykładowym serwerem punktu przed tabeli został dodany:
```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

`az postgres server restore` Polecenia wymaga następujących parametrów:
| Ustawienie | Sugerowana wartość | Opis  |
| --- | --- | --- |
| --grupy zasobów |  myResourceGroup |  Grupy zasobów, w której serwer źródłowy istnieje.  |
| — Nazwa | przywrócona mypgserver | Nazwa nowego serwera, który jest tworzony przez polecenie restore. |
| Przywracanie punktu w czasie | 2017-04-13T13:59:00Z | Wybierz w momencie przywrócić. Ta data i godzina musi być w okresie przechowywania kopii zapasowej serwera źródłowego. Użyj formatu ISO8601 daty i godziny. Na przykład możesz może używać własnych lokalnej strefie czasowej, takich jak `2017-04-13T05:59:00-08:00`, lub użyj formatu czasu UTC Zulu `2017-04-13T13:59:00Z`. |
| --serwera źródłowego | mypgserver 20170401 | Nazwa lub identyfikator serwera źródłowego, aby przywrócić z. |

Przywracanie do punktu w czasie serwer tworzy nowego serwera, skopiować jako oryginalnego serwera, począwszy od punktu w czasie, które określisz. Lokalizacja i wartości warstwy cenowej przywróconej serwera są takie same, jak na serwerze źródłowym.

Polecenie jest synchroniczne i zwróci po przywróceniu serwera. Po zakończeniu przywracania, zlokalizuj nowy serwer, który został utworzony. Sprawdź, czy dane została przywrócona, zgodnie z oczekiwaniami.


## <a name="next-steps"></a>Następne kroki
W tym samouczku przedstawiono sposób użycia interfejsu wiersza polecenia Azure (interfejsu wiersza polecenia) i inne narzędzia do:
> [!div class="checklist"]
> * Tworzenie serwera usługi Azure Database for PostgreSQL
> * Konfigurowanie zapory serwera
> * Użyj [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) narzędzie do tworzenia bazy danych
> * Ładuj dane przykładowe
> * Zapytania o dane
> * Aktualizowanie danych
> * Przywracanie danych

Następnie Dowiedz się, jak używać portalu Azure do wykonywania zadań podobne, zapoznaj się w tym samouczku: [projektowanie pierwszą bazę danych Azure do PostgreSQL przy użyciu portalu Azure](tutorial-design-database-using-azure-portal.md)
