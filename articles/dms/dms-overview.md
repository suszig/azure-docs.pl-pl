---
title: "Omówienie podglądu usługi migracji bazy danych platformy Azure | Dokumentacja firmy Microsoft"
description: "Omówienie usługi Azure bazy danych migracji, które zapewnia bezproblemowe migracji z wielu źródeł bazy danych do platformy Azure danych."
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.topic: article
ms.date: 12/13/2017
ms.openlocfilehash: 2aae105b7454209131db79c60d74740ce97c21ce
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="what-is-the-azure-database-migration-service-preview"></a>Co to jest podgląd z migracji bazy danych Azure?
Usługa Azure bazy danych migracji jest pełni zarządzana usługa zaprojektowana w celu umożliwienia bezproblemowego migracji z wielu źródeł bazy danych do platformy Azure danych z minimalnym czasem przestojów. Usługa jest obecnie w wersji zapoznawczej, z programistycznych koncentruje się na:

- Niezawodność i wydajność.
- Iteracyjny dodanie par źródłowy i docelowy.
- Dalsze inwestycje w bez tarcia migracji.

## <a name="use-familiar-tools"></a>Wykorzystanie znanych narzędzi
Usługa Azure bazy danych migracji integruje się niektórych funkcji naszych istniejących narzędzi i usług.  Zapewnia klientom z rozwiązaniem kompleksowe, wysokiej dostępności. Używane przez usługę [Asystenta migracji danych](http://aka.ms/dma) do generowania raportów do oceny, które przedstawienia rekomendowanych do przeprowadzenia zmiany wymagane przed wykonaniem migracji. To umożliwia przeprowadzanie korekty, wszelkie wymagane. Gdy wszystko będzie gotowe do rozpoczęcia procesu migracji usługi migracji bazy danych Azure wykonuje wszystkie skojarzone kroki. Można uruchomić i zapomni swoje projekty migracji z poczucie zdanie, wiedząc, że proces korzysta z najlepszych rozwiązań, zgodnie z ustaleniami firmy Microsoft.

## <a name="regional-availability-during-public-preview"></a>Regionalne dostępność w publicznej wersji zapoznawczej
W publicznej wersji zapoznawczej usługi Azure migracji bazy danych jest obecnie dostępna w następujących regionach:
- Wschodnie stany USA
- Środkowo-południowe stany USA
- Zachodnie stany USA
- Brazylia Południowa
- Europa Zachodnia
- Europa Północna
- Azja Południowo-Wschodnia
- Indie Zachodnie

## <a name="next-steps"></a>Kolejne kroki
- [Utwórz wystąpienie usługi migracji bazy danych Azure przy użyciu portalu Azure](quickstart-create-data-migration-service-portal.md).
- [Migrowanie serwera SQL z bazą danych Azure SQL](tutorial-sql-server-to-azure-sql.md).
- [Przegląd wymagań wstępnych dotyczących używania usługi Azure bazy danych migracji](pre-reqs.md).
- [Często zadawane pytania dotyczące korzystania z usługi migracji bazy danych Azure](faq.md).
