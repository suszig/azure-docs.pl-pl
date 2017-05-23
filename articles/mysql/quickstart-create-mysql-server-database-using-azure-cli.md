---
title: "Szybki start: tworzenie serwera usługi Azure Database for MySQL za pomocą interfejsu wiersza polecania platformy Azure | Microsoft Docs"
description: "W tym przewodniku Szybki start opisano, jak utworzyć serwer usługi Azure Database for MySQL w grupie zasobów platformy Azure za pomocą interfejsu wiersza polecenia platformy Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql-database
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: portal
ms.workload: 
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 574299dd64120d75a1a36cb2ded0fdd269292570
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Tworzenie serwera usługi Azure Database for MySQL za pomocą interfejsu wiersza polecenia platformy Azure
W tym przewodniku Szybki start opisano, jak utworzyć serwer usługi Azure Database for MySQL w grupie zasobów platformy Azure za pomocą interfejsu wiersza polecenia platformy Azure w czasie około pięciu minut. Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów.

Aby wykonać instrukcje podane w tym przewodniku Szybki start, upewnij się, że zainstalowano najnowszy [interfejs wiersza polecenia platformy Azure w wersji 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```azurecli
az login
```
Postępuj zgodnie z instrukcjami wiersza polecenia, aby otworzyć stronę https://aka.ms/devicelog w przeglądarce, a następnie wprowadź kod wygenerowany w **wierszu polecenia**.

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Utwórz [grupę zasobów platformy Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) za pomocą polecenia [az group create](https://docs.microsoft.com/cli/azure/group#create). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi w formie grupy.

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `mycliresource` w lokalizacji `westus`.

```azurecli
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Tworzenie serwera usługi Azure Database for MySQL
Utwórz serwer usługi Azure Database for MySQL za pomocą polecenia **az mysql server create**. Serwer umożliwia zarządzanie wieloma bazami danych. Zwykle dla każdego projektu lub użytkownika używana jest oddzielna baza danych.

W poniższym przykładzie w regionie `westus` w grupie zasobów `mycliresource` jest tworzony serwer usługi Azure Database for MySQL o nazwie `mycliserver`. Serwer ma identyfikator logowania administratora o nazwie `myadmin` i hasło `Password01!`. Serwer jest tworzony w ramach warstwy wydajności **Podstawowa** i z użyciem **50** jednostek obliczeniowych współdzielonych między wszystkimi bazami danych na tym serwerze. Możesz skalować zasoby obliczeniowe i magazyn w górę lub w dół w zależności od potrzeb aplikacji.

```azurecli
az mysql server create --resource-group mycliresource --name mycliserver--location westus --user myadmin --password Password01! --performance-tier Basic --compute-units 50
```

![Tworzenie serwera usługi Azure Database for MySQL za pomocą interfejsu wiersza polecenia platformy Azure](./media/quickstart-create-mysql-server-database-using-azure-cli/3_az-mysq-server-create.png)

## <a name="configure-firewall-rule"></a>Konfigurowanie reguły zapory
Utwórz regułę zapory na poziomie serwera usługi Azure Database for MySQL za pomocą polecenia **az mysql server firewall-rule create**. Reguła zapory na poziomie serwera pozwala aplikacji zewnętrznej, takiej jak narzędzie wiersza polecenia **mysql.exe** lub program MySQL Workbench, na nawiązywanie połączeń z Twoim serwerem przez zaporę usługi Azure MySQL. 

W poniższym przykładzie jest tworzona reguła zapory dla wstępnie zdefiniowanego zakresu adresów, który w tym przypadku jest całym możliwym zakresem adresów IP.

```azurecli
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a>Konfigurowanie ustawień SSL
Domyślnie połączenia SSL między Twoim serwerem i aplikacjami klienckimi są wymuszane.  Zapewnia to bezpieczeństwo danych „w ruchu” przez szyfrowanie strumienia danych przesyłanych przez Internet.  Aby uprościć ten przewodnik Szybki start, wyłączymy połączenia SSL dla Twojego serwera.  Nie jest to zalecane w przypadku serwerów produkcyjnych.  Aby uzyskać więcej informacji, zobacz [Konfigurowanie łączności SSL w aplikacji w celu bezpiecznego nawiązywania połączeń z usługą Azure Database for MySQL](./howto-configure-ssl.md).

W poniższym przykładzie jest wyłączane wymuszanie protokołu SSL na Twoim serwerze MySQL.
 
 ```azurecli
 az mysql server update --resource-group mycliresource --name mycliserver -g -n --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Pobieranie informacji o połączeniu

Aby nawiązać połączenie z serwerem, musisz podać informacje o hoście i poświadczenia dostępu.

```azurecli
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

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a>Nawiązywanie połączenia z serwerem za pomocą narzędzia wiersza polecenia mysql.exe
Aby nawiązać połączenie z serwerem za pomocą narzędzia wiersza polecenia **mysql.exe**, upewnij się, że na Twoim komputerze jest zainstalowany program MySQL.  Program MySQL możesz pobrać [tutaj](https://dev.mysql.com/downloads/).

Otwórz wiersz polecenia i wpisz następujące polecenie: 

1. Nawiąż połączenie z serwerem za pomocą narzędzia wiersza polecenia **mysql**:
```dos
 mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

2. Wyświetl stan serwera:
```dos
 mysql> status
```
Jeśli wszystko pójdzie dobrze, narzędzie wiersza polecenia powinno zwrócić następujące dane wyjściowe:

```dos
C:\Users\v-chenyh>mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             mycliserver.database.windows.net via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> Aby zapoznać się z dodatkowymi poleceniami, zobacz [Podręcznik programu MySQL 5.6 — Rozdział 4.5.1](https://dev.mysql.com/doc/refman/5.6/en/mysql.html).

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Nawiązywanie połączenia z serwerem za pomocą narzędzia z graficznym interfejsem użytkownika MySQL Workbench
1.    Uruchom aplikację MySQL Workbench na swoim komputerze klienckim. Aplikację MySQL Workbench możesz pobrać i zainstalować [stąd](https://dev.mysql.com/downloads/workbench/).

2.    W oknie dialogowym **Konfigurowanie nowego połączenia** wprowadź poniższe informacje na karcie **Parametry**:

| **Parametry** | **Opis** |
|----------------|-----------------|
|    *Nazwa połączenia* | podaj nazwę tego połączenia (może to być dowolna nazwa) |
| *Metoda połączenia* | wybierz standard (TCP/IP) |
| *Nazwa hosta* | mycliserver.database.windows.net (NAZWA SERWERA zanotowana przez Ciebie wcześniej) |
| *Port* | 3306 |
| *Nazwa użytkownika* | myadmin@mycliserver (IDENTYFIKATOR LOGOWANIA ADMINISTRATORA SERWERA zanotowany przez Ciebie wcześniej) |
| *Hasło* | możesz przechowywać hasło konta administratora w magazynie |

![konfiguruj nowe połączenie](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

3.    Kliknij przycisk **Testuj połączenie**, aby sprawdzić, czy wszystkie parametry zostały prawidłowo skonfigurowane.

4.    Teraz możesz kliknąć właśnie utworzone połączenie, aby pomyślnie nawiązać połączenie z serwerem.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli te zasoby nie są Ci potrzebne do pracy z innym przewodnikiem Szybki start lub samouczkiem, możesz je usunąć, wykonując następujące czynności: 

```azurecli
az group delete --name mycliresource
```

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Projektowanie bazy danych MySQL za pomocą interfejsu wiersza polecenia platformy Azure](./tutorial-design-database-using-cli.md).

