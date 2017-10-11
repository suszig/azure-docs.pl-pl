---
title: "Techniczne wstępne do tworzenia usługi danych dla witryny Marketplace | Dokumentacja firmy Microsoft"
description: "Poznaj wymagania dotyczące tworzenia usługi danych do wdrożenia i sprzedawać w witrynie Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: aaff609a-1cd1-4146-98f4-d04166b0fce0
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 52827723477677bc292c645e2390c435fbad3ee4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Oferuje techniczne wymagania wstępne dotyczące tworzenia usługi danych dla portalu Azure Marketplace
> [!IMPORTANT]
> **W tym momencie firma Microsoft nie są już dołączania żadnych nowych wydawców danych usługi. Nowe dataservices nie zostanie zatwierdzenia dla listy.** Jeśli masz aplikacji biznesowych SaaS chcesz publikować w AppSource więcej informacji można znaleźć [tutaj](https://appsource.microsoft.com/partners). Jeśli masz aplikacje IaaS lub więcej informacji można znaleźć usługi developer chcesz publikować w witrynie Azure Marketplace [tutaj](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Przeczytaj dokładnie przed rozpoczęciem procesu i zrozumienie, dlaczego i w którym każdy krok jest wykonywane. Jak to możliwe, należy należy przygotować informacji o swojej firmie i innych danych, Pobierz niezbędne narzędzia i/lub tworzenia składników techniczne przed rozpoczęciem procesu tworzenia oferty.

Musisz mieć następujące elementy gotowe przed rozpoczęciem procesu:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Podjęcie decyzji, w jakiej technologii będzie używany do publikowania tej oferty usługi danych
Wydawcą może wybór między wiele technologii podczas publikowania danych usługi w portalu Azure Marketplace. Główne technologii, które są obsługiwane opisane poniżej. Niezależnie od tego jaki technologii jest używana do publikowania usług danych, użytkownik końcowy używa danych za pośrednictwem **źródła strumieniowego OData** udostępnianych przez usługę Azure Marketplace. Pełne informacje dotyczące usługi OData, można znaleźć w [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL bazy danych platformy Azure
Zestaw danych jest gotowy w usługach SQL Azure jest odpowiedzialność wydawcy. Należy do subskrypcji Azure, udostępnić odpowiedni rozmiar bazy danych i przekazywanie danych do bazy danych SQL Azure. Wydawca odpowiada również przechowywać jego dane są zawsze aktualne. Więcej informacji o subskrypcji usługi Azure można znaleźć w [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)

Podczas przenoszenia danych do usługi SQL Azure, Azure Marketplace można ujawnić tabel i widoków. Wydawcy można określić, które tabele i widoki i kolumn są widoczne dla użytkownika końcowego. Dodatkowo dostawcy zawartości można również określić kolumny, które mogą być przeszukiwane przez użytkownika końcowego i zwracane są tylko te, które w ładunku. Zapewnia wysoki poziom elastyczności o tym, które powinny zostać ujawnione w bazie danych. Kolumny, które mogą być przeszukiwane należy wykonać kopię przez jeden lub więcej indeksów bazy danych.

## <a name="rest-based-web-service"></a>Usługi sieci web REST na podstawie
Obsługiwany protokół: **tylko protokołu HTTPS**

Istniejące usługi REST na podstawie można ujawnić za pośrednictwem portalu Azure Marketplace. Ponieważ zestaw danych jest zawsze widoczne dla użytkownika końcowego jako źródła strumieniowego OData, portalu Azure Marketplace wymaga usługi mogą mapować usługi do OData na podstawie usługi. Aby to zrobić, pozostałe na podstawie punktów końcowych muszą ujawnić wszystkie parametry jako parametry HTTP.

Ładunek musi mieć postać, które mogą być mapowane do odpowiedzi ATOM. Dlatego odpowiedzi z usług musi być w formacie XML i może tylko zawierać jeden element identycznych, zawierającą wartości ładunku (na przykład zestawu rekordów). Usługa Azure Marketplace przypisze węźle powtarzających się do węzła wpis ATOM i wartości ładunku w węzłach właściwość w węźle wpisu.

Informacje dotyczące autoryzacji (np. interfejsu API klucza, uwierzytelniania tokenu, itp.) musi zostać podane jako parametr HTTP lub w nagłówku HTTP (para klucz-wartość) — obsługiwana jest również uwierzytelnianie podstawowe. Prawidłowy klucz musi zostać zapewniony, a wszystkie żądania za pośrednictwem portalu Azure Marketplace są wykonywane za pośrednictwem tego klucza. W warstwie Azure Marketplace odbywa się użytkownika, monitorowanie i fakturowania.

Błędy zwrócone przez usługę muszą być mapowane do kody stanu HTTP. W przypadku, gdy usługa zwraca kod XML, który zawiera błąd te mają być mapowane przez usługę Azure Marketplace do kody stanu HTTP.

## <a name="soap-based-web-services"></a>Na podstawie protokołu SOAP usług sieci web
Protokół: **tylko protokołu HTTPS**

Wymagania są takie same jak pozostałe na podstawie sekcji usługi. Jedyna różnica polega na tym, że parametry można również podać w treści XML, która jest ogłaszany usługi wydawcy z każde żądanie za pośrednictwem portalu Azure Marketplace. Oznacza to, że parametry HTTP, które użytkownik udostępnia w frontonu są trwa przetłumaczyć elementów XML dokumentu XML publikowanego z żądaniem usługi sieci web dostawcy zawartości.

## <a name="odata-based-web-services"></a>Usługi sieci web OData na podstawie
Protokół: **tylko protokołu HTTPS**

Dane mogą być udostępniane jako usługi OData do portalu Azure Marketplace. System przechodzi do przekazania usługi za pośrednictwem i zastępuje katalog główny usługi katalogu usługi Azure Marketplace — aby upewnić się, że wszystkie kolejne wywołania go za pośrednictwem portalu Azure Marketplace.

Tylko usługi OData nie trzeba go w bazie danych w wewnętrznej bazie danych. OData obsługuje żadnego rodzaju magazynu lub logikę biznesową do usługi.

## <a name="next-steps"></a>Następne kroki
Po przejrzeniu wymagania wstępne i niezbędnych zadań zakończonych można przenieść do przodu z tej oferty usługi danych zgodnie z opisem w tworzeniu [Podręcznik publikowania danych usługi](marketplace-publishing-data-service-creation.md).

Lub, jeśli chcesz przejrzeć cały proces oraz odpowiednich artykułów dla każdej fazy publikowania, odwiedź stronę artykuł [wprowadzenie: jak publikowanie oferty w portalu Azure Marketplace](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
