---
title: "Interfejs wiersza polecenia platformy Azure: tworzenie pojedynczej bazy danych SQL i wysyłanie do niej zapytań | Microsoft Docs"
description: "Dowiedz się, jak utworzyć serwer logiczny, regułę zapory na poziomie serwera i bazy danych usługi SQL Database przy użyciu interfejsu wiersza polecenia platformy Azure."
keywords: "samouczek usługi sql database, tworzenie bazy danych sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: quick start
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: cli
ms.topic: hero-article
ms.date: 03/13/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 8322e46f462b6c940f9808411d99aa1cee0beea5
ms.lasthandoff: 03/15/2017

---

# <a name="create-and-query-a-single-azure-sql-database-with-the-azure-cli"></a>Tworzenie pojedynczej bazy danych Azure SQL Database i wysyłanie do niej zapytań za pomocą interfejsu wiersza polecenia platformy Azure

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. W tym przewodniku znajdują się szczegółowe informacje dotyczące użycia interfejsu wiersza polecenia platformy Azure w celu wdrożenia bazy danych Azure SQL Database.

Przed rozpoczęciem upewnij się, że interfejs wiersza polecenia platformy Azure został zainstalowany. Aby uzyskać więcej informacji, zobacz [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli) (Przewodnik instalacji interfejsu wiersza polecenia platformy Azure). 

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `myResourceGroup` w lokalizacji `westeurope`.

```azurecli
az group create --name myResourceGroup --location westeurope
```
## <a name="create-a-logical-server"></a>Tworzenie serwera logicznego

Utwórz serwer logiczny za pomocą polecenia [az sql server create](/cli/azure/sql/server#create). Poniższy przykład obejmuje tworzenie serwera o losowo wybranej nazwie w grupie zasobów za pomocą identyfikatora logowania administratora o nazwie `ServerAdmin` i z hasłem `ChangeYourAdminPassword1`. Zastąp te wstępnie zdefiniowane wartości zgodnie z potrzebami.

```azurecli
servername=server-$RANDOM
az sql server create --name $servername --resource-group myResourceGroup --location westeurope \
    --admin-user ServerAdmin --admin-password ChangeYourAdminPassword1
```

## <a name="configure-a-server-firewall-rule"></a>Konfigurowanie reguły zapory serwera

Utwórz regułę zapory na poziomie serwera za pomocą polecenia [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create). Reguła zapory na poziomie serwera umożliwia zewnętrznej aplikacji, takiej jak SQL Server Management Studio lub narzędzie SQLCMD, nawiązanie połączenia z bazą danych SQL za pośrednictwem zapory usługi SQL Database. Poniższy przykład obejmuje tworzenie reguły zapory dla wstępnie zdefiniowanego zakresu adresów, który w tym przypadku jest całym możliwym zakresem adresów IP. Zastąp te wstępnie zdefiniowane wartości wartościami dla zewnętrznego adresu IP lub zakresu adresów IP. 

```azurecli
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="create-a-database-in-the-server"></a>Tworzenie bazy danych na serwerze

Utwórz bazę danych na serwerze za pomocą polecenia [az sql db create](/cli/azure/sql/db#create). Poniższy przykład obejmuje tworzenie pustej bazy danych o nazwie `mySampleDatabase`. Zastąp tę wstępnie zdefiniowaną wartość zgodnie z potrzebami.

```azurecli
az sql db create --resource-group myResourceGroup --server $servername \
    --name mySampleDatabase --service-objective S0
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Aby usunąć wszystkie zasoby utworzone przez ten przewodnik szybkiego startu, uruchom następujące polecenie:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

- Aby nawiązywać połączenia i wykonywać zapytania za pomocą programu SQL Server Management Studio, zobacz [Connect and query with SSMS](sql-database-connect-query-ssms.md) (Nawiązywanie połączeń i wykonywanie zapytań za pomocą programu SSMS).
- Aby nawiązywać połączenia przy użyciu programu Visual Studio, zobacz [Connect and query with Visual Studio](sql-database-connect-query.md) (Nawiązywanie połączeń i wykonywanie zapytań przy użyciu programu Visual Studio).
- Opis techniczny usługi SQL Database można znaleźć w temacie [About the SQL Database service](sql-database-technical-overview.md) (Informacje o usłudze SQL Database).

