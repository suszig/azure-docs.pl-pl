---
title: "Omówienie bazy danych Azure usługa relacyjnej bazy danych PostgreSQL | Dokumentacja firmy Microsoft"
description: "Omówienie bazy danych Azure dla PostgreSQL usługa relacyjnej bazy danych."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 10/20/2017
ms.openlocfilehash: 5b5da758e966cc5ca536d7b291be74409f02ca73
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/21/2017
---
# <a name="what-is-azure-database-for-postgresql"></a>Co to jest Azure bazy danych PostgreSQL?

Bazy danych platformy Azure dla PostgreSQL jest usługą relacyjnych baz danych w chmurze firmy Microsoft, przeznaczony dla deweloperów w wersji community typu open source [PostgreSQL](https://www.postgresql.org/) aparatu bazy danych. Ta usługa jest w wersji zapoznawczej. Bazy danych platformy Azure dla PostgreSQL zapewnia:

- Wbudowaną wysoką dostępność bez dodatkowych kosztów.
- Przewidywalną wydajność, przy użyciu włącznie z cennik.
- Skalowanie na bieżąco w ciągu kilku sekund.
- Zabezpieczony, aby chronić poufne dane w rest i w ruchu.
- Automatycznego tworzenia kopii zapasowych i punkt w — czas — Przywracanie do 35 dni.
- Korporacyjnej zabezpieczeń i zgodności.

Wszystkie te funkcje wymagają prawie nie administracji, a wszystkie znajdują się bez ponoszenia dodatkowych kosztów. Te możliwości pozwalają skupić się na szybkie programowanie aplikacji i skraca czas na rynek, a nie przydzielanie cenny czas i zasoby do zarządzania maszynami wirtualnymi i infrastruktury. Ponadto można utworzyć aplikację za pomocą narzędzi typu open source i platform wybranych przez użytkownika i dostarczanie o szybkości i wydajności firmy wymaga bez konieczności rozwijania nowych umiejętności. 

Ten artykuł zawiera wprowadzenie do bazy danych platformy Azure dla PostgreSQL podstawowych pojęć i funkcje dotyczące wydajności, skalowalności i możliwości zarządzania. Zobacz te przewodniki Szybki Start ułatwiających rozpoczęcie pracy:

- [Tworzenie bazy danych Azure dla PostgreSQL przy użyciu portalu Azure](quickstart-create-server-database-portal.md)
- [Tworzenie bazy danych Azure dla PostgreSQL przy użyciu wiersza polecenia platformy Azure](quickstart-create-server-database-azure-cli.md)

Zestaw przykładów wiersza polecenia platformy Azure zobacz:

- [Przykładów dla interfejsu wiersza polecenia platformy Azure dla bazy danych Azure dla PostgreSQL](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Dostosowanie wydajności i skalowanie w ciągu kilku sekund
W wersji zapoznawczej, baza danych Azure dla usługi MySQL zapewnia dwie warstwy usług: Basic i Standard. Każda warstwa oferuje różne wydajności i funkcji do obsługi niewielkich do ogromnych obciążeń bazy danych. Można utworzyć pierwszą aplikację na małej bazie danych dla kilku dolarów miesięcznie, a następnie Dostosuj skalę na potrzeby rozwiązania. Skalowalność dynamiczne umożliwia bazy danych odpowiada przezroczysty na szybko ulegają zmianie wymagań dotyczących zasobów. Płacisz tylko dla zasobów możesz konieczności i tylko wtedy, gdy są potrzebne. Zobacz [warstw cenowych](concepts-service-tiers.md) szczegółowe informacje.

## <a name="monitoring-and-alerting"></a>Monitorowanie i zgłaszanie alertów
Jak można podczas wybierania w górę i w dół decyduje? Możesz użyć wbudowanych wydajności monitorowania oraz alertów funkcji, z ocen wydajności na podstawie obliczeniowe jednostek. Za pomocą tych narzędzi, może szybko ocenić wpływ skalowania obliczeniowe jednostki lub w dół na podstawie potrzeb bieżącego lub planowane wydajności. Zobacz [alerty](howto-alert-on-metric.md) szczegółowe informacje.

## <a name="keep-your-app-and-business-running"></a>Zapewnienie działania aplikacji i firmy
Azure w branży 99,99% dostępności (niedostępne w wersji zapoznawczej) umowę dotyczącą poziomu usług (SLA), obsługiwane przez usługę globalnej sieci centrów danych zarządzany przez firmę Microsoft, pomaga zapewnić działanie aplikacji 24/7. Z każdym Azure bazy danych PostgreSQL serwera możesz korzystać z zabezpieczeń, odporność na uszkodzenia i ochrony danych, w przeciwnym razie należy kupić lub projektowanie, tworzenie i zarządzanie. Z bazy danych Azure PostgreSQL każda warstwa usług oferuje kompleksowe zbiór funkcjach ciągłości biznesowej i opcje używane do uruchomienia i kontynuować w ten sposób. Możesz użyć [przywracania do punktu w czasie](howto-restore-server-portal.md), aby przywrócić bazę danych do wcześniejszego stanu — nawet sprzed 35 dni. Ponadto jeśli Centrum danych hostujące bazy danych ulegnie awarii, można przywrócić bazy danych z geograficznie nadmiarowego kopie ostatnie kopie zapasowe.

## <a name="secure-your-data"></a>Zabezpieczanie danych
Usługi Azure bazy danych ma tradycję bezpieczeństwa danych, czy baza danych Azure dla PostgreSQL podtrzymuje dzięki funkcjom ograniczania dostępu, ochrony danych na rest i w ruchu i monitorowania aktywności. Odwiedź stronę [Centrum zaufania Azure](https://www.microsoft.com/TrustCenter/Security/default.aspx) informacji o zabezpieczeniach platformy Azure.

Bazy danych Azure dla usługi PostgreSQL używa szyfrowania magazynu dla danych na rest. W tym kopie zapasowe danych są szyfrowane na dysku (z wyjątkiem plików tymczasowych utworzonych przez aparat podczas uruchamiania zapytań). Usługa używa szyfrowania AES 256-bitowy, należący do szyfrowania magazynu Azure, a klucze są kontrolowany przez system. Szyfrowanie magazynu jest zawsze włączone i nie można wyłączyć.

Domyślnie program Azure bazy danych dla usługi PostgreSQL jest skonfigurowany do żądania [zabezpieczeń połączenia SSL](./concepts-ssl-connection-security.md) dla danych w ruchu w sieci. Wymuszanie połączenia SSL między serwerem bazy danych i aplikacji klienckich pomaga chronić przed atakami "man w środku" szyfrując strumienia danych między serwerem i aplikacji.  Opcjonalnie możesz wyłączyć wymaganie protokołu SSL do łączenia się z usługą baza danych, jeśli aplikacja kliencka nie obsługuje łączności SSL.

## <a name="next-steps"></a>Następne kroki
- Zobacz [cennikiem](https://azure.microsoft.com/pricing/details/postgresql/) koszt porównania i kalkulatory.
- Rozpoczynanie pracy przez [tworzenie pierwszej bazy danych Azure dla PostgreSQL](./quickstart-create-server-database-portal.md).
- Tworzenie pierwszej aplikacji w języku Python, PHP, Ruby, C\#, Java, Node.js: [biblioteki połączeń](./concepts-connection-libraries.md)
