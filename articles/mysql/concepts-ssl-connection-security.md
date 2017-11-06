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
ms.date: 11/02/2017
ms.openlocfilehash: 37e2bbbe1ed4b6a9cca0e0b001e5e632b9b73be2
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>Połączenie SSL w bazie danych Azure dla programu MySQL
Bazy danych platformy Azure dla programu MySQL obsługuje łączenie serwer bazy danych dla aplikacji klienckich przy użyciu protokołu Secure Sockets Layer (SSL). Wymuszanie połączenia SSL między serwerem bazy danych i aplikacji klienckich pomaga chronić przed atakami "man w środku" szyfrując strumienia danych między serwerem i aplikacji.

## <a name="default-settings"></a>Ustawienia domyślne
Domyślnie usługa bazy danych należy skonfigurować do Wymagaj połączeń SSL, podczas nawiązywania połączenia MySQL.  Firma Microsoft zaleca, aby uniknąć wyłączenie opcji SSL, jeśli to możliwe. 

Podczas inicjowania obsługi administracyjnej nowej bazy danych Azure MySQL serwerem za pośrednictwem portalu Azure i interfejsu wiersza polecenia, wymuszanie połączeń SSL jest domyślnie włączona. 

Parametry połączenia dla różnych języków programowania są wyświetlane w portalu Azure. Te parametry połączenia zawierają parametry SSL wymagane do połączenia z bazą danych. W portalu Azure wybierz serwer. W obszarze **ustawienia** nagłówek, wybierz **parametry połączenia**. Parametr SSL w zależności od łącznika, na przykład "ssl = true" lub "sslmode = wymagają" lub "sslmode = wymagane" i innych zmian.

Aby dowiedzieć się, jak włączyć lub wyłączyć połączenia SSL, podczas tworzenia aplikacji, zapoznaj się [Konfigurowanie SSL](howto-configure-ssl.md). 

## <a name="next-steps"></a>Następne kroki
[Biblioteki połączeń dla bazy danych Azure dla programu MySQL](concepts-connection-libraries.md)
