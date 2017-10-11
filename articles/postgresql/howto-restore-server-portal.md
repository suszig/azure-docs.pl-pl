---
title: Przywracanie serwera w bazie danych systemu Azure dla PostgreSQL | Dokumentacja firmy Microsoft
description: "W tym artykule opisano sposób przywracania serwera w bazie danych Azure dla PostgreSQL przy użyciu portalu Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 07/20/2017
ms.openlocfilehash: 3fbdb7741481bd3620466c3489d3609f9ea6961f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a>Sposób wykonywania kopii zapasowej i przywracania serwera w bazie danych Azure dla PostgreSQL przy użyciu portalu Azure

## <a name="backup-happens-automatically"></a>Kopia zapasowa jest wykonywana automatycznie
W przypadku używania bazy danych platformy Azure dla PostgreSQL, usługa bazy danych automatycznie tworzy kopię zapasową usługi co 5 minut. 

Kopie zapasowe są dostępne przez 7 dni, korzystając z warstwy podstawowej i 35 dni po użyciu warstwy standardowa. Aby uzyskać więcej informacji, zobacz [bazą danych Azure dla warstwy usług PostgreSQL](concepts-service-tiers.md)

Tej funkcji automatycznego tworzenia kopii zapasowej można przywrócić serwer i wszystkie jej baz danych do nowego serwera do wcześniejszego punktu w stanu.

## <a name="restore-in-the-azure-portal"></a>Przywracanie w portalu Azure
Bazy danych platformy Azure dla PostgreSQL służy do przywrócenia serwera do punktu w czasie, a do z uprawnieniami do nowego serwera. Aby odzyskać dane, można użyć tego nowego serwera. 

Na przykład jeśli przypadkowo tabeli w południe dzisiaj, można przywrócenie na czas bezpośrednio przed południe i pobieranie Brak tabeli i danych z tej kopii nowego serwera.

Poniższe kroki przywrócenie serwera próbki do punktu w czasie:
1. Zaloguj się do [portalu Azure](https://portal.azure.com/)
2. Zlokalizuj PostgreSQL serwera bazy danych Azure. W portalu Azure kliknij **wszystkie zasoby** z menu po lewej stronie i wpisz nazwę, taką jak **mypgserver 20170401**, aby wyszukać istniejącego serwera. Kliknij nazwę serwera wyświetlaną w wynikach wyszukiwania. Zostanie otwarta strona **Przegląd**, która zawiera dalsze opcje konfiguracji.

   ![Portal Azure — Wyszukaj, aby zlokalizować serwera](media/postgresql-howto-restore-server-portal/1-locate.png)

3. W górnej części bloku Omówienie serwera, kliknij przycisk **przywrócić** na pasku narzędzi. Zostanie otwarty blok przywracania.

   ![Bazy danych platformy Azure dla przycisku PostgreSQL — Przegląd — przywracania](./media/postgresql-howto-restore-server-portal/2_server.png)

4. Wypełnij formularz przywracania wymagane informacje:

   ![Bazy danych platformy Azure dla PostgreSQL — informacji dotyczące przywracania ](./media/postgresql-howto-restore-server-portal/3_restore.png)
  - **Punkt przywracania**: Wybierz w momencie po serwer został zmieniony
  - **Serwer docelowy**: Podaj nową nazwę serwera mają zostać przywrócone
  - **Lokalizacja**: nie można wybrać region, domyślnie jest taki sam jak serwer źródłowy
  - **Warstwa cenowa**: nie można zmienić tę wartość podczas przywracania serwera. Jest taki sam jak serwer źródłowy. 

5. Kliknij przycisk **OK** do przywrócenia serwera, aby wykonać przywracanie do punktu w czasie. 

6. Po zakończeniu przywracania, zlokalizuj nowy serwer, który jest tworzony w celu sprawdzenia, czy dane została przywrócona, zgodnie z oczekiwaniami.

## <a name="next-steps"></a>Następne kroki
- [Biblioteki połączeń dla bazy danych Azure dla PostgreSQL](concepts-connection-libraries.md)
