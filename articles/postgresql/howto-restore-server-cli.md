---
title: "Sposób wykonywania kopii zapasowej i przywracania dla PostgreSQL serwera w bazie danych Azure"
description: "Dowiedz się, jak kopie zapasowe i przywracanie serwera w bazie danych Azure dla PostgreSQL przy użyciu wiersza polecenia platformy Azure."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 69dfde7e54a271caabc6d0909565165fb219c7f2
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-by-using-the-azure-cli"></a>Sposób wykonywania kopii zapasowej i przywracania serwera w bazie danych Azure dla PostgreSQL przy użyciu wiersza polecenia platformy Azure

Użyj bazy danych Azure dla PostgreSQL, aby przywrócić serwer bazy danych do wcześniejszego stanu, który jest liczony od 7 do 35 dni.

## <a name="prerequisites"></a>Wymagania wstępne
Aby ukończyć ten przewodnik, potrzebne są:
- [Bazy danych Azure PostgreSQL serwera i bazy danych](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


> [!IMPORTANT]
> Jeśli został zainstalowany, użyj interfejsu wiersza polecenia Azure lokalnie, ten przewodnik wymaga użycia interfejsu wiersza polecenia Azure w wersji 2.0 lub nowszej. Aby upewnić się, wersja, w wierszu polecenia interfejsu wiersza polecenia Azure, wprowadź `az --version`. Aby zainstalować lub uaktualnić, zobacz [zainstalować Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="backup-happens-automatically"></a>Kopia zapasowa jest wykonywana automatycznie
Gdy używasz bazy danych Azure PostgreSQL, usługa bazy danych automatycznie tworzy kopię zapasową usługi co 5 minut. 

Dla warstwy Basic kopie zapasowe są dostępne przez 7 dni. Dla warstwy standardowa kopie zapasowe są dostępne dla 35 dni. Aby uzyskać więcej informacji, zobacz [bazą danych Azure dla PostgreSQL warstw cenowych](concepts-pricing-tiers.md).

Z automatycznej funkcji Kopia zapasowa Przywróć serwera i jej baz danych do wcześniejszego stanu lub punktu w czasie.

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a>Przywracanie bazy danych z wcześniejszego punktu w czasie przy użyciu wiersza polecenia platformy Azure
Użyj bazy danych Azure dla PostgreSQL, aby przywrócić serwer z wcześniejszego punktu w czasie. Przywrócone dane zostaną skopiowane do nowego serwera, a istniejący serwer pozostanie niezmieniona. Na przykład jeśli przypadkowo upuszczeniu tabeli w południe dzisiaj, można przywrócić na czas bezpośrednio przed południe. Następnie można pobrać wartości Brak tabeli i dane z przywróconą kopią serwera. 

Aby przywrócić działanie serwera, należy użyć interfejsu wiersza polecenia Azure [przywracania serwera postgres az](/cli/azure/postgres/server#az_postgres_server_restore) polecenia.

### <a name="run-the-restore-command"></a>Uruchom polecenie restore

Aby przywrócić serwer, w wierszu polecenia interfejsu wiersza polecenia Azure, wprowadź następujące polecenie:

```azurecli-interactive
az postgres server restore --resource-group myresourcegroup --server mydemoserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mydemoserver
```

`az postgres server restore` Polecenie wymaga następujących parametrów:
| Ustawienie | Sugerowana wartość | Opis  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Grupy zasobów, w której serwer źródłowy istnieje.  |
| name | mydemoserver-restored | Nazwa nowego serwera, który jest tworzony przez polecenie restore. |
| Przywracanie punktu w czasie | 2017-04-13T13:59:00Z | Wybierz punkt w czasie, aby przywrócić. Ta data i godzina musi być w okresie przechowywania kopii zapasowej serwera źródłowego. Użyj formatu ISO8601 daty i godziny. Na przykład można własnej lokalnej strefie czasowej, takich jak `2017-04-13T05:59:00-08:00`. Można też używać formatu UTC Zulu, na przykład `2017-04-13T13:59:00Z`. |
| source-server | mydemoserver | Nazwa lub identyfikator serwera źródłowego, aby przywrócić z. |

Po przywróceniu serwera do wcześniejszego punktu w czasie, jest tworzony nowy serwer. Oryginalny serwer i jej baz danych z określonego punktu w czasie są kopiowane do nowego serwera.

Lokalizacji i wartości warstwy cenowej przywróconej serwera pozostać taka sama jak oryginalny serwer. 

`az postgres server restore` Polecenie jest synchroniczne. Po przywróceniu serwera, można go ponownie powtórzyć ten proces dla różnych punktu w czasie. 

Po ukończeniu procesu przywracania, zlokalizuj nowy serwer i sprawdź, czy dane są przywracane zgodnie z oczekiwaniami.

## <a name="next-steps"></a>Kolejne kroki
[Biblioteki połączeń dla bazy danych Azure dla PostgreSQL](concepts-connection-libraries.md)
