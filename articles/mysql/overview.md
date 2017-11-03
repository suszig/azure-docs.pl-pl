---
title: "Omówienie bazy danych Azure usługa relacyjnej bazy danych MySQL | Dokumentacja firmy Microsoft"
description: "Omówienie bazy danych Azure usługa relacyjnej bazy danych MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/20/2017
ms.custom: mvc
ms.openlocfilehash: 2a9efdd9285dfa5fca450ede64e5f6ee54cbc72b
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2017
---
# <a name="what-is-azure-database-for-mysql"></a>Co to jest Azure bazy danych MySQL?
Bazy danych platformy Azure dla programu MySQL to usługa relacyjnej bazy danych w chmurze firmy Microsoft, na podstawie [MySQL Community Edition](https://www.mysql.com/products/community/) aparatu bazy danych. Ta usługa jest w wersji zapoznawczej. Bazy danych platformy Azure dla programu MySQL zapewnia:

- Wbudowaną wysoką dostępność bez dodatkowych kosztów.
- Przewidywalną wydajność, przy użyciu włącznie z cennik.
- Skalowanie na bieżąco w ciągu kilku sekund.
- Zabezpieczony, aby chronić poufne dane w rest i w ruchu.
- Automatycznego tworzenia kopii zapasowych i punkt w — czas — Przywracanie do 35 dni.
- Korporacyjnej zabezpieczeń i zgodności.

Te możliwości wymagają prawie nie administracji i wszystkie są udostępniane bez ponoszenia dodatkowych kosztów. Umożliwiają one skupić się na tworzenie aplikacji szybki i skraca czas na rynek zamiast przydzielania cenny czas i zasoby do zarządzania maszynami wirtualnymi i infrastruktury. Ponadto można utworzyć aplikację za pomocą narzędzi typu open source i platformy wybór dostarczać o szybkości i wydajności wymaga dane biznesowe, wszystko to bez konieczności rozwijania nowych umiejętności.

![Azure bazy danych MySQL diagram koncepcyjny](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Ten artykuł zawiera wprowadzenie do bazy danych Azure dla programu MySQL podstawowych pojęć i funkcje dotyczące wydajności, skalowalności i możliwości zarządzania, wraz z łączami, aby poznać szczegóły. Zobacz te przewodniki Szybki Start ułatwiających rozpoczęcie pracy:
- [Tworzenie serwera usługi Azure Database for MySQL za pomocą witryny Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Tworzenie serwera usługi Azure Database for MySQL za pomocą interfejsu wiersza polecenia platformy Azure](quickstart-create-mysql-server-database-using-azure-cli.md)

Zestaw przykładów wiersza polecenia platformy Azure zobacz:
- [Przykładów dla interfejsu wiersza polecenia platformy Azure dla bazy danych Azure dla programu MySQL](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Dostosowanie wydajności i skalowanie w ciągu kilku sekund
W wersji zapoznawczej, baza danych Azure dla usługi MySQL zapewnia dwie warstwy usług: Basic i Standard. Każda warstwa oferuje różne wydajności i funkcji do obsługi niewielkich do ogromnych obciążeń bazy danych. Można utworzyć pierwszą aplikację na małej bazie danych dla kilku dolarów miesięcznie, a następnie Dostosuj skalę na potrzeby rozwiązania. Skalowalność dynamiczne umożliwia bazy danych odpowiada przezroczysty na szybko ulegają zmianie wymagań dotyczących zasobów. Płacisz tylko dla zasobów możesz konieczności i tylko wtedy, gdy są potrzebne. Zobacz [warstw cenowych](concepts-service-tiers.md) szczegółowe informacje.

## <a name="monitoring-and-alerting"></a>Monitorowanie i zgłaszanie alertów
Jak można podczas wybierania w górę i w dół decyduje? Możesz użyć wbudowanych wydajności monitorowania oraz alertów funkcji, z ocen wydajności na podstawie obliczeniowe jednostek. Za pomocą tych narzędzi, może szybko ocenić wpływ skalowania obliczeniowe jednostki lub w dół na podstawie potrzeb bieżącego lub planowane wydajności. Zobacz [alerty](howto-alert-on-metric.md) szczegółowe informacje.

## <a name="keep-your-app-and-business-running"></a>Zapewnienie działania aplikacji i firmy
Azure w branży 99,99% dostępności Umowa dotycząca poziomu usług (SLA), obsługiwane przez usługę globalnej sieci centrów danych zarządzany przez firmę Microsoft, pomaga zapewnić działanie aplikacji 24/7. Z każdym Azure bazy danych MySQL serwera możesz korzystać z zabezpieczeń, odporność na uszkodzenia i ochrony danych, w przeciwnym razie należy kupić lub projektowanie, tworzenie i zarządzanie. Z bazy danych Azure dla programu MySQL w momencie przywracania służy do odzyskania serwera do wcześniejszego stanu, nawet sprzed 35 dni.

## <a name="secure-your-data"></a>Zabezpieczanie danych
Usługi Azure bazy danych ma tradycję zabezpieczeń danych, który podtrzymuje bazy danych Azure dla programu MySQL, dzięki funkcjom ograniczania dostępu, ochrony danych na rest i w ruchu, które ułatwiają monitorowanie aktywności. Odwiedź stronę [Centrum zaufania Azure](https://www.microsoft.com/en-us/TrustCenter/Security/default.aspx) informacji o zabezpieczeniach platformy Azure.

Bazy danych Azure dla usługi MySQL używa szyfrowania magazynu dla danych na rest. Dane, w tym kopie zapasowe, są szyfrowane na dysku (z wyjątkiem plików tymczasowych, które są tworzone przez aparat podczas uruchamiania zapytań). Usługa używa szyfrowania AES 256-bitowy, należący do szyfrowania magazynu Azure, a klucze są kontrolowany przez system. Szyfrowanie magazynu jest zawsze włączone i nie można wyłączyć.

Domyślnie program Azure bazy danych dla usługi MySQL jest skonfigurowany do żądania [zabezpieczeń połączenia SSL](./concepts-ssl-connection-security.md) dla danych w ruchu w sieci. Wymuszanie połączenia SSL między serwerem bazy danych i aplikacji klienckich pomaga chronić przed atakami "man w środku" szyfrując strumienia danych między serwerem i aplikacji.  Opcjonalnie możesz wyłączyć wymaganie protokołu SSL do łączenia się z usługą baza danych, jeśli aplikacja kliencka nie obsługuje łączności SSL.

## <a name="next-steps"></a>Następne kroki
Po przeczytaniu wprowadzenia do bazy danych platformy Azure dla programu MySQL i udzielenie odpowiedzi na pytanie "Co to jest Azure bazy danych na MySQL?", możesz przystąpić do:
- Zobacz stronę cen. koszt porównania i kalkulatory. [Cennik](https://azure.microsoft.com/pricing/details/mysql/)
- Rozpoczynanie pracy przez utworzenie pierwszego serwera. [Tworzenie serwera usługi Azure Database for MySQL za pomocą witryny Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- Tworzenie pierwszej aplikacji za pomocą preferowany język: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md)  |  [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [przejdź](./connect-go.md)
