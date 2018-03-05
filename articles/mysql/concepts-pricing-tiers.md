---
title: Warstw cenowych w bazie danych systemu Azure dla programu MySQL
description: W tym artykule opisano warstw cenowych w bazie danych Azure dla programu MySQL.
services: mysql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 6bd24da05c337a902ce0e4a2b9acf22a809eb653
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="azure-database-for-mysql-pricing-tiers"></a>Bazy danych platformy Azure dla programu MySQL warstw cenowych

Baza danych Azure dla serwera MySQL mogą być tworzone w jednym z trzech różnych warstw cenowych — podstawowe, ogólnego przeznaczenia i zoptymalizowanych pod kątem pamięci. Warstw cenowych są zróżnicowane według ilości zasobów obliczeniowych w vCores, które można udostępnić, ilość pamięci na vCore i technologii magazynowania używany do przechowywania danych. Wszystkie zasoby są udostępniane na poziomie serwera MySQL. Serwer może mieć jedną lub wiele baz danych.

|    | **Podstawowa** | **Ogólnego przeznaczenia** | **Zoptymalizowana pod kątem pamięci** |
|:---|:----------|:--------------------|:---------------------|
| Generowanie obliczeniowe | Gen 4, 5 Gen | Gen 4, 5 Gen | Gen 5 |
| vCores | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16, 32 |
| Ilość pamięci na vCore | 1x | 2 x Basic | 2 x ogólnego przeznaczenia |
| Rozmiar magazynu | 5 GB do 1 TB | 5 GB do 1 TB | 5 GB do 1 TB |
| Typ magazynu | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| Okres przechowywania kopii zapasowych bazy danych | 7 - 35 dni | 7 - 35 dni | 7 - 35 dni |

Poniższa tabela może służyć jako punkt początkowy dla wybieranie warstwy cenowej:

| Warstwa cenowa | Docelowe obciążenia |
|:-------------|:-----------------|
| Podstawowa | Obciążenia wymagające niewielkiej wydajności obliczeniowej i operacji wejścia/wyjścia. Są to na przykład serwery używane do programowania lub testów lub niewielkie, rzadko używane aplikacje. |
| Ogólne zastosowanie | Większość obciążeń biznesowych, wymagających zrównoważonych zasobów obliczeniowych i pamięci, ze skalowalną przepływnością wejścia/wyjścia. Przykładami serwera hostingu sieci Web i aplikacji mobilnych i inne aplikacje przedsiębiorstwa.|
| Pamięć | Wymaganie wydajności w pamięci do szybsze przetwarzanie transakcji i wyższe współbieżności obciążeń wysokiej wydajności bazy danych. Przykładami serwera przetwarzania danych w czasie rzeczywistym i wysoką wydajność aplikacji transakcyjnej lub analitycznych.|

Po utworzeniu serwera liczba vCores można zmienić w górę lub w dół w ciągu kilku sekund. Można również niezależnie dostosować ilość pamięci masowej w górę i okres przechowywania kopii zapasowych w górę lub w dół bez przestojów aplikacji. Zobacz sekcję skalowania poniżej, aby uzyskać więcej informacji.

## <a name="compute-generations-vcores-and-memory"></a>Generacje obliczeń, vCores i pamięci

Zasoby obliczeniowe są dostarczane jako vCores reprezentujący Procesora logicznego podstawowej sprzętu. Obecnie dwie generacje obliczeń Gen 4 i Gen 5 są dostępne do wyboru z. Logiczne procesory CPU 4. generacji wykorzystują procesory Intel E5-2673 v3 (Haswell) z zegarem 2,4 GHz. Logiczne procesory CPU 5. generacji wykorzystują procesory Intel E5-2673 v4 (Broadwell) z zegarem 2,3 GHz.

W zależności od warstwy cenowej każdego vCore jest udostępniane z określoną ilością pamięci. Gdy zwiększyć lub zmniejszyć liczbę vCores serwera, ilość pamięci zwiększa lub zmniejsza proporcjonalnie. Warstwa ogólnego przeznaczenia zapewnia podwójne ilość pamięci na vCore w porównaniu do warstwy podstawowa. Warstwa zoptymalizowanych pod kątem pamięci miejsce double ilość pamięci w porównaniu do warstwy ogólnego przeznaczenia.

## <a name="storage"></a>Magazyn

Zainicjowanie obsługi magazynu jest pojemności magazynu dostępnych do bazy danych Azure dla serwera MySQL. Magazyn jest używany dla plików bazy danych, pliki tymczasowe, dzienników transakcji i serwer MySQL dzienników. Łączną ilość pamięci masowej, które należy udostępnić definiuje również dostępne wydajność We/Wy na serwerze:

|    | **Podstawowa** | **Ogólnego przeznaczenia** | **Zoptymalizowana pod kątem pamięci** |
|:---|:----------|:--------------------|:---------------------|
| Typ magazynu | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| Rozmiar magazynu | 5 GB do 1 TB | 5 GB do 1 TB | 5 GB do 1 TB |
| Rozmiar przyrost magazynu | 1 GB | 1 GB | 1 GB |
| Operacje wejścia/wyjścia | Zmienna |3 IOPS/GB<br/>Min 100 IOPS | 3 IOPS/GB<br/>Min 100 IOPS |

Podczas i po utworzeniu serwera można dodać dodatkowej pojemności. Warstwa podstawowa nie ma gwarancji IOPS. Ogólnego przeznaczenia i zoptymalizowanych pod kątem pamięci, warstw cenowych IOPS skalowania o rozmiarze zainicjowanego magazynu w stosunku 3:1.

Można monitorować użycia we/wy w portalu Azure lub przy użyciu poleceń wiersza polecenia platformy Azure. Odpowiednich metryk do monitorowania są [limit magazynu, procent użycia magazynu, Magazyn używany i procent We/Wy](concepts-monitoring.md).

## <a name="backup"></a>Backup

Usługa automatycznie wykonuje kopie zapasowe serwera. Minimalny okres przechowywania kopii zapasowych wynosi siedem dni. Można ustawić okresu przechowywania, do 35 dni. W dowolnym momencie można dostosować zachowanie przez cały okres istnienia serwera. Można wybrać tworzenie kopii zapasowych lokalnie nadmiarowego i geograficznie nadmiarowy. Geograficznie nadmiarowego kopie zapasowe są także przechowywane w [łączyć geograficznie regionu](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) regionu serwera jest tworzony w. Zapewnia poziom ochrony w przypadku awarii. Możesz również uzyskać możliwość przywrócenia serwera do innego regionu Azure, w którym usługa jest dostępna z geograficznie nadmiarowego kopii zapasowych. Należy pamiętać, że nie można zmienić po utworzeniu serwera z dwóch opcji magazynu kopii zapasowej.

## <a name="scale-resources"></a>Skalowanie zasobów

Po utworzeniu serwera, można zmienić niezależnie vCores, ilość miejsca do magazynowania i okres przechowywania kopii zapasowych. Po utworzeniu serwera nie można zmienić warstwy cenowej lub typ magazynu kopii zapasowej. vCores i okres przechowywania kopii zapasowych można skalować w górę lub w dół. Tylko można zwiększyć rozmiar magazynu. Skalowanie zasobów można zrobić za pomocą portalu lub wiersza polecenia platformy Azure. Przykład odbierającej za pomocą interfejsu wiersza polecenia można znaleźć [tutaj](scripts/sample-scale-server.md).

W przypadku zmiany liczby vCores, z nowej alokacji obliczeniowego jest utworzona kopia oryginalnego serwera. Po skonfigurowaniu i uruchomieniu nowego serwera połączeń są przełączono się na nowym serwerze. Podczas krótki moment, gdy system przełącza się na nowym serwerze można ustalić bez nowych połączeń i wycofać wszystkie niezatwierdzone transakcje. To okno jest różny, ale w większości przypadków jest mniej niż minutę.

Skalowanie magazynu i zmienić okres przechowywania kopii zapasowych to PRAWDA operacje online. Brak Brak przestoju i wpływ na aplikację. IOPS skalowania o rozmiarze zainicjowanego magazynu, można zwiększyć IOPS dostępne z serwerem, skalując w górę magazynu.

## <a name="pricing"></a>Cennik

Zapoznaj się z tematem usługi [cennikiem](https://azure.microsoft.com/pricing/details/mysql/) dla najbardziej aktualne informacje o cenach. Aby zobaczyć, jakie wymaganą konfiguracją kosztów, [portalu Azure](https://portal.azure.com/#create/Microsoft.MySQLServer) pokazuje miesięczny koszt w **warstwa cenowa** kartę na podstawie opcji wybranych. Jeśli nie masz subskrypcji platformy Azure, można użyć Azure Kalkulator cen można pobrać szacowanej ceny. Odwiedź [Kalkulator cen platformy Azure](https://azure.microsoft.com/pricing/calculator/) witryny sieci Web, następnie kliknij przycisk **Dodaj elementy**, rozwiń węzeł **baz danych** kategorii i wybierz polecenie **bazy danych Azure dla programu MySQL** Aby dostosować opcje.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [utworzyć serwer MySQL w portalu](howto-create-manage-server-portal.md)
- Dowiedz się, jak [monitorować i skalowania bazy danych Azure programu MySQL serwera przy użyciu wiersza polecenia platformy Azure](scripts/sample-scale-server.md)
- Dowiedz się więcej o [usługi ograniczenia](concepts-limits.md)
