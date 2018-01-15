---
title: "Tworzenie usługi Azure Search w portalu | Dokumentacja firmy Microsoft"
description: "Alokowanie usługi Azure Search w portalu."
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.assetid: c8c88922-69aa-4099-b817-60f7b54e62df
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/07/2017
ms.author: heidist
ms.openlocfilehash: b31844dd0dad1168b12d91ce536589099ab16a7e
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/13/2018
---
# <a name="create-an-azure-search-service-in-the-portal"></a>Tworzenie usługi Azure Search w portalu

Informacje o sposobie tworzenia i inicjowania obsługi administracyjnej usługi Azure Search w portalu. 

Preferowane PowerShell? Użyj usługi Azure Resource Manager [szablonu usługi](https://azure.microsoft.com/resources/templates/101-azure-search-create/). Aby uzyskać pomoc, wprowadzenie, [zarządzania usługi Azure Search przy użyciu programu PowerShell](search-manage-powershell.md) w tle.

## <a name="subscribe-free-or-paid"></a>Subskrypcja (wolne lub płatną)

[Załóż bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) i wykorzystać do wypróbowania bezpłatnych środków płatnej usług platformy Azure. Po wyczerpaniu kredytu, należy zachować konto i nadal korzystać z bezpłatnych usług platformy Azure, takich jak witryny sieci Web. Karta kredytowa nie jest obciążona, chyba że jawnie zmienić ustawienia i poprosisz o jej obciążenie.

Alternatywnie [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). Subskrypcję MSDN otrzymasz kredyt, co miesiąc, w której można skorzystać w celu płatnych usług Azure. 

## <a name="find-azure-search"></a>Znajdź wyszukiwanie Azure
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. Kliknij znak plus ("+") w lewym górnym rogu.
3. Wybierz **sieci Web i mobilność** > **wyszukiwanie Azure**.

![](./media/search-create-service-portal/find-search3.png)

## <a name="name-the-service-and-url-endpoint"></a>Nazwa usługi i punktu końcowego adresu URL

Nazwa usługi jest częścią punktu końcowego adresu URL, względem których interfejsu API są wydawane wywołania: `https://your-service-name.search.windows.net`. Wprowadź nazwę usługi w **adres URL** pola. 

Wymagania dotyczące nazw usługi:
   * Muszą być unikatowe w przestrzeni nazw search.windows.net
   * 2 do 60 znaków
   * Użyj małe litery, cyfry i łączniki ("-")
   * Unikaj kreski ("-") w najpierw 2 znaki lub jako ostatni znak pojedynczego
   * Nie następujących po sobie kresek ("--") dowolnego miejsca

## <a name="select-a-subscription"></a>Wybierz subskrypcję
Jeśli masz więcej niż jedną subskrypcję, wybierz jedną z usług magazynu danych lub pliku. Usługa wyszukiwanie Azure można Autowykrywanie magazynu tabel Azure i obiektów Blob, bazy danych SQL i bazy danych Azure rozwiązania Cosmos indeksowania za pośrednictwem [ *indeksatory*](search-indexer-overview.md), ale tylko w przypadku usług w tej samej subskrypcji.

## <a name="select-a-resource-group"></a>Wybierz grupę zasobów
Grupa zasobów to zbiór usług platformy Azure i zasoby używane razem. Na przykład jeśli używasz usługi Azure Search do indeksowania bazy danych SQL, obie te usługi należy częścią tej samej grupie zasobów.

> [!TIP]
> Usunięcie grupy zasobów powoduje usunięcie usług w niej. Prototyp projektów przy użyciu wielu usług umieszczenie wszystkich z nich w tej samej grupie zasobów ułatwia oczyszczania po zakończeniu projektu. 

## <a name="select-a-hosting-location"></a>Wybierz lokalizację hostingu 
Jak usługa Azure usługi Azure Search może być hostowana w centrach danych na całym świecie. Należy pamiętać, że [ceny może się różnić](https://azure.microsoft.com/pricing/details/search/) według ich położenia.

## <a name="select-a-pricing-tier-sku"></a>Wybierz warstwę cenową (SKU)
[Usługa Azure Search jest dostępne w wielu warstw cenowych](https://azure.microsoft.com/pricing/details/search/): wolnego, podstawowa lub standardowa. Każda warstwa ma własną [pojemności i limity](search-limits-quotas-capacity.md). Zobacz [wybierz warstwę cenową lub jednostki SKU](search-sku-tier.md) orientacji.

W tym przewodniku Wybraliśmy warstwy standardowa naszej usługi.

Nie można zmienić warstwy cenowej, po utworzeniu usługi. Warstwy wyższe lub niższe potrzebne później, należy ponownie utworzyć usługę.

## <a name="create-your-service"></a>Tworzenie usługi

Pamiętaj, aby przypiąć do pulpitu nawigacyjnego usługi łatwy dostęp przy każdym logowaniu.

![](./media/search-create-service-portal/new-service3.png)

## <a name="scale-your-service"></a>Skalowanie usługi
Może upłynąć kilka minut, aby utworzyć usługę (15 minut lub dłużej w zależności od warstwy). Po zainicjowaniu obsługi usługi, można też skalować, zgodnie z potrzebami. Ponieważ wybrano warstwy standardowa dla usługi Azure Search w dwóch wymiarów można skalować usługi: repliki i partycji. Czy wybierze warstwie podstawowej, można dodać tylko repliki. Jeśli elastycznie bezpłatnej usługi, skalowanie jest niedostępne.

***Partycje*** Zezwalaj usłudze wyszukiwania za pomocą więcej dokumentów i przechowywania.

***Repliki*** Zezwalaj usługi do obsługi większych obciążeń zapytań wyszukiwania.

> [!Important]
> Usługa musi mieć [2 replik do umowy SLA tylko do odczytu i 3 repliki do odczytu/zapisu SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Przejdź do strony usługi wyszukiwania w portalu Azure.
2. W okienku nawigacji po lewej, wybierz **ustawienia** > **skali**.
3. Slidebar umożliwia dodawanie repliki lub partycji.

![](./media/search-create-service-portal/settings-scale.png)

> [!Note] 
> Każda warstwa ma inną [limity](search-limits-quotas-capacity.md) całkowitej liczby jednostek wyszukiwania dozwolonych w jednej usługi (replik * partycje = Suma jednostek wyszukiwania).

## <a name="when-to-add-a-second-service"></a>Kiedy należy dodać drugi usługi

Duże większość klientów używać tylko jednej usługi zainicjowana dla warstwy, która zapewnia [prawym przyciskiem myszy salda środków](search-sku-tier.md). Jedna usługa może obsługiwać wiele indeksów, podlegają [maksymalnych wybrania warstwy](search-capacity-planning.md), każdy indeks odizolowane od siebie. W usłudze Azure Search żądań może zostać skierowany tylko do jednego indeksu, minimalizując ryzyko pobieranie przypadkowym lub zamierzone danych z innych indeksy w tej samej usługi.

Mimo że większość klientów używają tylko jedną usługę, nadmiarowość usługi może być konieczne, jeśli wymagań operacyjnych są następujące:

+ Odzyskiwanie po awarii (awarii centrum danych). Usługa Azure Search nie zapewnia błyskawiczne trybu failover w przypadku awarii. Aby uzyskać wskazówki i zalecenia, zobacz [usługi administracyjnej](search-manage.md).
+ Badania modelowania wielodostępność stwierdził, że dodatkowe usługi jest optymalne projektu. Aby uzyskać więcej informacji, zobacz [projektu dla wielu dzierżawców](search-modeling-multitenant-saas-applications.md).
+ Globalny wdrożonej aplikacji może wymagać wystąpienia usługi Azure Search w wielu regionach, aby zminimalizować opóźnienie aplikacji międzynarodowych ruchu.

> [!NOTE]
> W usłudze Azure Search nie może też oddzielić obciążeń indeksowania i wykonywanie zapytania; w związku z tym nigdy nie spowodowałoby utworzenie wielu usług dla obciążeń rozdzielone. Indeks jest zawsze wykonywana kwerenda w usłudze w którym został utworzony (nie można utworzyć indeksu w jedną usługę i skopiuj go do innej).
>

Drugi usługi nie jest wymagane w celu zapewnienia wysokiej dostępności. Wysoka dostępność dla zapytania odbywa się przy użyciu co najmniej 2 replik w tej samej usługi. Repliki są aktualizacje sekwencyjnych, co oznacza, że co najmniej jeden działa podczas aktualizacji usługi jest wdrażana. Aby uzyskać więcej informacji o dostępności, zobacz [umowy dotyczące poziomu usług](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

## <a name="next-steps"></a>Kolejne kroki
Po zainicjowaniu obsługi administracyjnej usługi Azure Search, możesz przystąpić do [zdefiniuj indeks](search-what-is-an-index.md) , można przekazać i wyszukiwać dane. 

> [!div class="nextstepaction"]
> [Jak używać usługi Azure Search w .NET](search-howto-dotnet-sdk.md)
