---
title: "Połączenie SSL w bazie danych Azure dla programu MySQL | Dokumentacja firmy Microsoft"
description: "Informacje dotyczące konfiguracji bazy danych Azure MySQL i skojarzone aplikacje prawidłowe korzystanie z połączeń SSL"
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b03b3a2dbfad92cc0cfa84777b38ddff90452cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>Połączenie SSL w bazie danych Azure dla programu MySQL
Bazy danych platformy Azure dla programu MySQL obsługuje łączenie serwer bazy danych dla aplikacji klienckich przy użyciu protokołu Secure Sockets Layer (SSL). Wymuszanie połączenia SSL między serwerem bazy danych i aplikacji klienckich pomaga chronić przed atakami "man w środku" szyfrując strumienia danych między serwerem i aplikacji.

## <a name="default-settings"></a>Ustawienia domyślne
Domyślnie usługa bazy danych należy skonfigurować do Wymagaj połączeń SSL, podczas nawiązywania połączenia MySQL.  Zaleca się unikać wyłączenie opcji SSL, jeśli to możliwe. 

Podczas inicjowania obsługi administracyjnej nowej bazy danych Azure MySQL serwerem za pośrednictwem portalu Azure i interfejsu wiersza polecenia, wymuszanie połączeń SSL jest domyślnie włączona. 

Podobnie parametry połączenia, które są wstępnie zdefiniowane w ustawieniach "Parametry połączenia" w obszarze serwera w portalu Azure obejmują wymaganych parametrów dla typowych języków nawiązać połączenia z serwerem bazy danych przy użyciu protokołu SSL. Parametr SSL w zależności od łącznika, na przykład "ssl = true" lub "sslmode = wymagają" lub "sslmode = wymagane" i innych zmian.

Aby dowiedzieć się, jak włączyć lub wyłączyć połączenia SSL, podczas tworzenia aplikacji, zapoznaj się [Konfigurowanie SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Następne kroki
[Biblioteki połączeń dla bazy danych Azure dla programu MySQL](concepts-connection-libraries.md)
