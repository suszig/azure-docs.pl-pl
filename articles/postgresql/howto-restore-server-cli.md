---
title: "Jak wykonać kopię zapasową i przywrócić serwera w bazie danych Azure PostgreSQL | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wykonać kopię zapasową i przywrócić serwera w bazie danych Azure PostgreSQL przy użyciu wiersza polecenia platformy Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 871887e67d686a965a0648d2c6f0c72b3008db05
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-the-azure-cli"></a>Jak wykonać kopię zapasową i przywrócić serwera w bazie danych Azure PostgreSQL przy użyciu wiersza polecenia platformy Azure

Użyj bazy danych Azure dla PostgreSQL, aby przywrócić serwer bazy danych do wcześniejszego stanu, który jest liczony od 7 do 35 dni.

## <a name="prerequisites"></a>Wymagania wstępne
Aby ukończyć ten przewodnik, potrzebne są:
- [Bazy danych Azure PostgreSQL serwera i bazy danych](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Jeśli został zainstalowany, użyj interfejsu wiersza polecenia Azure lokalnie, ten przewodnik wymaga użycia interfejsu wiersza polecenia Azure w wersji 2.0 lub nowszej. Aby upewnić się, wersja, w wierszu polecenia interfejsu wiersza polecenia Azure, wprowadź `az --version`. Aby zainstalować lub uaktualnić, zobacz [zainstalować Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="back-up-happens-automatically"></a>Tworzenie kopii zapasowej odbywa się automatycznie
Gdy używasz bazy danych Azure PostgreSQL, usługa bazy danych automatycznie tworzy kopię zapasową usługi co 5 minut. 

Dla warstwy Basic kopie zapasowe są dostępne przez 7 dni. Dla warstwy standardowa kopie zapasowe są dostępne dla 35 dni. Aby uzyskać więcej informacji, zobacz [bazą danych Azure dla PostgreSQL warstw cenowych](concepts-service-tiers.md).

Z automatycznej funkcji Kopia zapasowa Przywróć serwera i jej baz danych do wcześniejszego stanu lub punktu w czasie.

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a>Przywracanie bazy danych z wcześniejszego punktu w czasie przy użyciu wiersza polecenia platformy Azure
Użyj bazy danych Azure dla PostgreSQL, aby przywrócić serwer z wcześniejszego punktu w czasie. Przywrócone dane zostaną skopiowane do nowego serwera, a istniejący serwer pozostanie niezmieniona. Na przykład jeśli przypadkowo upuszczeniu tabeli w południe dzisiaj, można przywrócić na czas bezpośrednio przed południe. Następnie można pobrać wartości Brak tabeli i dane z przywróconą kopią serwera. 

Aby przywrócić działanie serwera, należy użyć interfejsu wiersza polecenia Azure [przywracania serwera postgres az](/cli/azure/postgres/server#restore) polecenia.

### <a name="run-the-restore-command"></a>Uruchom polecenie restore

Aby przywrócić serwer, w wierszu polecenia interfejsu wiersza polecenia Azure, wprowadź następujące polecenie:

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

`az postgres server restore` Polecenie wymaga następujących parametrów:
| Ustawienie | Sugerowana wartość | Opis  |
| --- | --- | --- |
| grupy zasobów |  myResourceGroup |  Grupy zasobów, w której serwer źródłowy istnieje.  |
| name | przywrócona mypgserver | Nazwa nowego serwera, który jest tworzony przez polecenie restore. |
| Przywracanie punktu w czasie | 2017-04-13T13:59:00Z | Wybierz punkt w czasie, aby przywrócić. To data i godzina musi być w ramach serwera źródłowego kopii zapasowych okresu przechowywania. Użyj formatu ISO8601 daty i godziny. Na przykład można własnej lokalnej strefie czasowej, takich jak `2017-04-13T05:59:00-08:00`. Można też używać formatu UTC Zulu, na przykład `2017-04-13T13:59:00Z`. |
| Serwer źródłowy | mypgserver 20170401 | Nazwa lub identyfikator serwera źródłowego, aby przywrócić z. |

Po przywróceniu serwera do wcześniejszego punktu w czasie, jest tworzony nowy serwer. Oryginalny serwer i jej baz danych z określonego punktu w czasie są kopiowane do nowego serwera.

Lokalizacji i wartości warstwy cenowej przywróconej serwera pozostać taka sama jak oryginalny serwer. 

`az postgres server restore` Polecenie jest synchroniczne. Po przywróceniu serwera, można go ponownie powtórzyć ten proces dla różnych punktu w czasie. 

Po ukończeniu procesu przywracania, zlokalizuj nowy serwer i sprawdź, czy dane są przywracane zgodnie z oczekiwaniami.

## <a name="next-steps"></a>Następne kroki
[Biblioteki połączeń dla bazy danych Azure dla PostgreSQL](concepts-connection-libraries.md)
