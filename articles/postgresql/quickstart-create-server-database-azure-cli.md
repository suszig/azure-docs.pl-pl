---
title: "Tworzenie serwera usługi Azure Database for PostgreSQL za pomocą interfejsu wiersza polecania platformy Azure | Microsoft Docs"
description: "Przewodnik Szybki start dotyczący tworzenia serwera usługi Azure Database for PostgreSQL i zarządzania nim przy użyciu interfejsu wiersza polecenia platformy Azure."
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: hero-article
ms.date: 06/13/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: b2be9e265075c58ed53a0a49c01a08e05db35a06
ms.contentlocale: pl-pl
ms.lasthandoff: 06/20/2017

---
# <a name="create-an-azure-database-for-postgresql-using-the-azure-cli"></a>Tworzenie serwera usługi Azure Database for PostgreSQL za pomocą interfejsu wiersza polecania platformy Azure
Azure Database for PostgreSQL to usługa zarządzana, która umożliwia uruchamianie i skalowanie w chmurze baz danych PostgreSQL o wysokiej dostępności, a także zarządzanie nimi. Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. W tym przewodniku Szybki start przedstawiono, jak utworzyć serwer usługi Azure Database for PostgreSQL w [grupie zasobów platformy Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) za pomocą interfejsu wiersza polecenia platformy Azure.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten temat będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

Jeśli masz wiele subskrypcji, wybierz odpowiednią subskrypcję, w ramach której będą naliczane opłaty za ten zasób. Wybierz określony identyfikator subskrypcji na Twoim koncie za pomocą polecenia [az account set](/cli/azure/account#set).
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

W poniższym przykładzie zostanie utworzony serwer o nazwie `mypgserver-20170401` w grupie zasobów `myresourcegroup` z identyfikatorem logowania administratora serwera `mylogin`. Nazwa serwera jest mapowana na nazwę DNS i dlatego musi być globalnie unikatowa w ramach platformy Azure. Zastąp zmienną `<server_admin_password>` swoją własną wartością.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> Login i hasło administratora serwera określone w tym miejscu będą wymagane do logowania do serwera i jego baz danych w późniejszej części tego przewodnika Szybki start. Zapamiętaj lub zapisz te informacje do wykorzystania w przyszłości.

Domyślnie baza danych **postgres** zostanie utworzona na Twoim serwerze. Baza danych [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) to domyślna baza danych przeznaczona do użycia dla użytkowników oraz na potrzeby narzędzi i aplikacji innych firm. 


## <a name="configure-a-server-level-firewall-rule"></a>Konfigurowanie reguły zapory na poziomie serwera

Utwórz regułę zapory na poziomie serwera Azure PostgreSQL za pomocą polecenia [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create). Reguła zapory na poziomie serwera pozwala aplikacji zewnętrznej, takiej jak narzędzie [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) lub [PgAdmin](https://www.pgadmin.org/), na nawiązywanie połączeń z Twoim serwerem przez zaporę usługi Azure PostgreSQL. 

Możesz ustawić regułę zapory uwzględniającą zakres adresów IP, aby mieć możliwość nawiązywania połączeń z Twojej sieci. W poniższym przykładzie użyto polecenia [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) w celu utworzenia reguły zapory `AllowAllIps` dla zakresu adresów IP. Aby otworzyć wszystkie adresy IP, użyj wartości 0.0.0.0 jako początkowego adresu IP i wartości 255.255.255.255 jako adresu końcowego.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Serwer Azure PostgreSQL komunikuje się przez port 5432. Podczas nawiązywania połączenia z sieci firmowej ruch wychodzący przez port 5432 może być blokowany przez zaporę sieciową. Aby było możliwe nawiązywanie połączenia z serwerem usługi Azure SQL Database, poproś dział IT o otwarcie portu 5432.

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

## <a name="connect-to-postgresql-database-using-psql"></a>Nawiązywanie połączenia z bazą danych PostgreSQL za pomocą narzędzia psql

Jeśli na Twoim komputerze klienckim jest zainstalowany program PostgreSQL, możesz użyć lokalnego wystąpienia narzędzia [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), aby nawiązać połączenie z serwerem Azure PostgreSQL. Użyj teraz narzędzia wiersza polecenia psql, aby nawiązać połączenie z serwerem Azure PostgreSQL.

1. Uruchom poniższe polecenie psql w celu nawiązania połączenia z serwerem usługi Azure Database for PostgreSQL
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Na przykład poniższe polecenie nawiązuje połączenie z domyślną bazą danych o nazwie **postgres** na Twoim serwerze PostgreSQL **mypgserver-20170401.postgres.database.azure.com** za pomocą poświadczeń dostępu. Gdy zostanie wyświetlony monit o podanie hasła, wprowadź wybrane hasło `<server_admin_password>`.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  Po nawiązaniu połączenia z serwerem utwórz pustą bazę danych za pomocą wiersza polecenia.
```sql
CREATE DATABASE mypgsqldb;
```

3.  W wierszu polecenia wykonaj poniższe polecenie, aby przełączyć połączenie na nowo utworzoną bazę danych **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="connect-to-postgresql-database-using-pgadmin"></a>Nawiązywanie połączenia z bazą danych PostgreSQL za pomocą narzędzia pgAdmin

Aby nawiązać połączenie z serwerem usługi Azure PostgreSQL za pomocą narzędzia z graficznym interfejsem użytkownika _pgAdmin_
1.  Uruchom aplikację _pgAdmin_ na swoim komputerze klienckim. Aplikację _pgAdmin_ można zainstalować ze strony http://www.pgadmin.org/.
2.  Wybierz pozycję **Dodaj nowy serwer** w menu **Szybkie linki**.
3.  W oknie dialogowym **Tworzenie — Serwer** na karcie **Ogólne** wprowadź unikatową przyjazną nazwę serwera. Na przykład **Serwer Azure PostgreSQL**.
 ![Narzędzie pgAdmin — Tworzenie — Serwer](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)
4.  W oknie dialogowym **Tworzenie — Serwer** na karcie **Połączenie**:
    - Wprowadź w pełni kwalifikowaną nazwę serwera (na przykład **mypgserver-20170401.postgres.database.azure.com**) w polu **Nazwa/adres hosta**. 
    - Wprowadź port 5432 w polu **Port**. 
    - Wprowadź **identyfikator logowania administratora serwera (user@mypgserver)** uzyskany wcześniej w tym przewodniku Szybki start oraz hasło wprowadzone podczas tworzenia serwera odpowiednio w polach **Użytkownik** i **Hasło**.
    - W polu **Tryb SSL** wybierz wartość **Wymagany**. Domyślnie wszystkie serwery Azure PostgreSQL są tworzone z włączonym wymuszaniem protokołu SSL. Aby wyłączyć wymuszanie protokołu SSL, zobacz szczegółowe informacje w artykule [Wymuszanie protokołu SSL](./concepts-ssl-connection-security.md).

    ![Narzędzie pgAdmin — Tworzenie — Serwer](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  Kliknij pozycję **Zapisz**.
6.  W lewym okienku Browser (Przeglądarka) rozwiń węzeł **Server Groups** (Grupy serwerów). Wybierz swój serwer **Serwer Azure PostgreSQL**.
7.  Wybierz **serwer**, z którym nawiązano połączenie, a następnie wybierz w jego obszarze pozycję **Databases** (Bazy danych). 
8.  Kliknij prawym przyciskiem myszy pozycję **Databases** (Bazy danych), aby utworzyć bazę danych.
9.  Wybierz nazwę bazy danych **mypgsqldb** oraz jej właściciela: identyfikator logowania administratora serwera **mylogin**.
10. Kliknij przycisk **Zapisz**, aby utworzyć pustą bazę danych.
11. W oknie **Przeglądarka** rozwiń grupę **Serwery**. Rozwiń utworzony serwer. Będzie widoczna baza danych **mypgsqldb**.
 ![Narzędzie pgAdmin — Tworzenie — Baza danych](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)


## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Wyczyść wszystkie zasoby utworzone w tym przewodniku Szybki start, usuwając [grupę zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md).

> [!TIP]
> Inne przewodniki Szybki start w tej kolekcji bazują na tym przewodniku. Jeśli planujesz kontynuować pracę z kolejnymi przewodnikami Szybki start, nie usuwaj zasobów utworzonych w tym przewodniku Szybki start. Jeśli nie planujesz kontynuować pracy, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone za pomocą interfejsu wiersza polecenia platformy Azure w ramach tego przewodnika Szybki start.

```azurecli-interactive
az group delete --name myresourcegroup
```

Jeśli po prostu chcesz usunąć jeden z nowo utworzonych serwerów, możesz uruchomić polecenie [az postgres server delete](/cli/azure/postgres/server#delete).
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a>Następne kroki
> [!div class="nextstepaction"]
> [Migrowanie bazy danych przy użyciu funkcji eksportowania i importowania](./howto-migrate-using-export-and-import.md)

