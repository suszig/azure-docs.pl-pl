---
title: "Przegląd i kluczowe pojęcia usługi Azure API Management | Microsoft Docs"
description: "Poznaj interfejsy API, produkty, role, grupy i inne ważne pojęcia dotyczące usługi API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: e71da405-835a-48f3-956f-45c1a85698d7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: becffc6011ef1dd49e07d22880d3346036629393
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="what-is-api-management"></a>Co to jest API Management?
Usługa API Management pomaga organizacjom publikować interfejsy API dla deweloperów zewnętrznych, partnerskich i wewnętrznych, aby w pełni wykorzystać potencjał danych i usług. Firmy na całym świecie dążą do rozszerzenia operacji w postaci platformy cyfrowej, tworząc nowe kanały, znajdując nowych klientów i nakłaniając obecnych do głębszego zaangażowania. Usługa API Management zapewnia podstawowe możliwości, które pozwalają na pomyślne programowanie interfejsów API przez zaangażowanych deweloperów, a także udostępnia wgląd w kwestie biznesowe, analizy, zabezpieczenia i ochronę.

Obejrzyj poniższy film przedstawiający przegląd usługi Azure API Management i dowiedz się, jak używać usługi API Management, aby przy minimalnym nakładzie pracy dodać do interfejsu API wiele funkcji obejmujących kontrolę dostępu, ograniczanie liczby wywołań, monitorowanie, rejestrowanie zdarzeń i buforowanie odpowiedzi.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-API-Management-Overview/player]
> 
> 

Aby użyć usługi API Management, administratorzy tworzą interfejsy API. Każdy interfejs API składa się z co najmniej jednej operacji i można go dodać do co najmniej jednego produktu. Aby użyć interfejsu API, deweloperzy subskrybują produkt, który zawiera ten interfejs API, a następnie wywołują operację interfejsu API, która podlega obowiązującym dla niej zasadom użycia.

Ten temat zawiera przegląd najważniejszych założeń usługi API Management.

> [!NOTE]
> Aby uzyskać więcej informacji, zobacz oficjalny dokument PDF [Cloud-based API Management: Harnessing the Power of APIs](http://j.mp/ms-apim-whitepaper) (Oparta na chmurze usługa API Management: wykorzystanie możliwości interfejsów API). Ten wprowadzający oficjalny dokument dotyczący usługi API Management opracowany przez firmę CITO Research obejmuje następujące tematy: 
> 
> * Typowe wymagania i wyzwania związane z interfejsem API
> * Rozdzielanie interfejsów API i prezentowanie fasad
> * Szybkie rozpoczęcie pracy przez deweloperów
> * Zabezpieczanie dostępu
> * Analizy i metryki
> * Uzyskiwanie kontroli i wglądu dzięki platformie usługi API Management
> * Porównanie rozwiązań w chmurze i rozwiązań lokalnych
> * Usługa Azure API Management
> 
> 

## <a name="apis"> </a>Interfejsy API i operacje
Interfejsy API są podstawą wystąpienia usługi API Management. Każdy interfejs API reprezentuje zestaw operacji dostępnych dla deweloperów. Każdy interfejs API zawiera odwołanie do usługi zaplecza, która implementuje interfejs API, oraz mapę odwzorowań operacji interfejsu API na operacje zaimplementowane w usłudze zaplecza. Operacje usługi API Management są wysoce konfigurowalne, pozwalają na kontrolę mapowania adresu URL, parametrów zapytania i ścieżki, zawartości żądania i odpowiedzi oraz buforowanie odpowiedzi operacji. Ograniczenia liczby wywołań, przydziały i zasady ograniczeń adresów IP mogą być implementowane zarówno na poziomie interfejsu API, jak i na poziomie poszczególnych operacji.

Aby uzyskać więcej informacji, zobacz [How to create APIs][How to create APIs] (Jak utworzyć interfejsy API) i [How to add operations to an API][How to add operations to an API] (Jak dodać operacje do interfejsu API).

## <a name="products"> </a> Produkty
Produkty stanowią sposób udostępniania interfejsów API deweloperom. Produkty w usłudze API Management mają co najmniej jeden interfejs API oraz skonfigurowany tytuł, opis i warunki użytkowania. Produkty mogą być **otwarte** lub **chronione**. Produkty chronione trzeba subskrybować przed użyciem, a produkty otwarte mogą być używane bez subskrypcji. Kiedy produkt jest gotowy do użycia przez deweloperów, można go opublikować. Po opublikowaniu produktu deweloperzy mogą go wyświetlać (a w przypadku produktów chronionych również subskrybować). Zatwierdzenie subskrypcji jest konfigurowane na poziomie produktu i może wymagać zatwierdzenia przez administratora lub odbywać się automatycznie.

Grupy służą do zarządzania widocznością produktów dla deweloperów. Widoczność produktów jest przydzielana według grup, a deweloperzy mogą wyświetlać i subskrybować produkty, które są widoczne dla grup, do których należą. 

Aby uzyskać więcej informacji, zobacz [How to create and publish a product][How to create and publish a product] (Jak utworzyć i opublikować produkt) oraz następujący film.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="groups"> </a> Grupy
Grupy służą do zarządzania widocznością produktów dla deweloperów. Usługa API Management ma następujące niezmienne grupy systemowe.

* **Administratorzy** — do tej grupy należą administratorzy subskrypcji platformy Azure. Administratorzy zarządzają wystąpieniami usługi API Management, tworząc interfejsy API, operacje i produkty, które są używane przez deweloperów.
* **Deweloperzy** — do tej grupy należą uwierzytelnieni użytkownicy portalu dla deweloperów. Deweloperzy to klienci, którzy tworzą aplikacje przy użyciu interfejsów API. Deweloperzy otrzymują dostęp do portalu dla deweloperów i tworzą aplikacje, które wywołują operacje interfejsów API.
* **Goście** — do tej grupy należą nieuwierzytelnieni użytkownicy portalu dla deweloperów, tacy jak potencjalni klienci odwiedzający portal dla deweloperów wystąpienia usługi API Management. Mogą mieć przyznany pewien zakres dostępu tylko do odczytu, czyli np. możliwość wyświetlania interfejsów API, ale nie ich wywoływania.

Oprócz tych grup systemowych administratorzy mogą tworzyć niestandardowe grupy lub [wykorzystać zewnętrzne grupy w skojarzonych dzierżawach usługi Azure Active Directory](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Niestandardowe i zewnętrzne grupy mogą być używane razem z grupami systemowymi, zapewniając deweloperom widoczność produktów interfejsu API i dostęp do nich. Można na przykład utworzyć jedną grupę niestandardową dla deweloperów powiązanych z konkretną organizacją partnera i zezwolić im na dostęp do interfejsów API z produktów zawierających tylko odpowiednie interfejsy API. Użytkownik może należeć do więcej niż jednej grupy.

Aby uzyskać więcej informacji, zobacz [How to create and use groups][How to create and use groups] (Jak utworzyć grupy i używać ich).

## <a name="developers"> </a> Deweloperzy
Deweloperzy reprezentują konta użytkowników w wystąpieniu usługi API Management. Konta deweloperów mogą być tworzone lub dołączane na zaproszenie administratorów. Ponadto deweloperzy mogą się zarejestrować w [Portalu dla deweloperów][Developer portal]. Każdy deweloper jest członkiem jednej lub wielu grup i może subskrybować produkty, które mają przyznaną widoczność w tych grupach.

Kiedy deweloperzy subskrybują produkt, otrzymują klucz podstawowy i pomocniczy produktu. Ten klucz jest używany podczas wywołań interfejsów API produktu.

Aby uzyskać więcej informacji, zobacz [How to create or invite developers][How to create or invite developers] (Jak utworzyć i zaprosić deweloperów) i [How to associate groups with developers][How to associate groups with developers] (Jak skojarzyć grupy z deweloperami).

## <a name="policies"> </a> Zasady
Zasady są zaawansowaną możliwością usługi API Management, która pozwala wydawcy zmieniać zachowanie interfejsu API za pomocą konfiguracji. Zasady to zbiór instrukcji, które są wykonywane sekwencyjnie podczas żądania lub odpowiedzi interfejsu API. Popularne instrukcje obejmują konwersję z formatu XML do JSON i ograniczanie szybkości połączenia, aby ograniczyć liczbę wywołań przychodzących od dewelopera. Ponadto dostępnych jest wiele innych zasad.

Wyrażenia zasad mogą służyć jako wartości atrybutów lub wartości tekstowe w dowolnej z zasad usługi API Management, o ile w zasadach nie określono inaczej. Niektóre zasady, np. [Przepływ sterowania](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) i [Ustawianie zmiennej](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable), są oparte na wyrażeniach zasad. Aby uzyskać więcej informacji, zobacz tematy [Advanced policies](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies) (Zaawansowane zasady) i [Policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx) (Wyrażenia zasad) oraz obejrzyj następujący film.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

Pełna lista zasad usługi API Management znajduje się w artykule [Policy reference][Policy reference] (Dokumentacja zasad). Aby uzyskać więcej informacji na temat korzystania z zasad i konfigurowania ich, zobacz [API Management policies][API Management policies] (Zasady usługi API Management). Samouczek dotyczący tworzenia produktu z zasadami dotyczącymi ograniczania liczby wywołań i przydziałów znajduje się w artykule [Jak utworzyć i skonfigurować zaawansowane ustawienia produktów][How create and configure advanced product settings]. Demonstracja jest pokazana w następującym filmie.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="developer-portal"> </a> Portal dla deweloperów
Portal dla deweloperów jest miejscem, gdzie deweloperzy mogą poznawać interfejsy API, wyświetlać i wywoływać operacje oraz subskrybować produkty. Potencjalni klienci mogą odwiedzić portal dla deweloperów, wyświetlić interfejsy API i operacje oraz zarejestrować się. Adres URL do portalu dla deweloperów znajduje się na pulpicie nawigacyjnym w klasycznym portalu Azure danego wystąpienia usługi API Management.

Wygląd swojego portalu dla deweloperów możesz dostosować, dodając niestandardową zawartość, dostosowując style i dodając znak marki.

## <a name="api-management-and-the-api-economy"></a>Usługa API Management i ekonomia interfejsów API
Aby dowiedzieć się więcej na temat usługi API Management, obejrzyj następującą prezentację z konferencji Microsoft Ignite 2015.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3708/player]
> 
> 

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Developer portal]: #developer-portal

[How to create APIs]: api-management-howto-create-apis.md
[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer
[How create and configure advanced product settings]: api-management-howto-product-with-rules.md
[How to create or invite developers]: api-management-howto-create-or-invite-developers.md
[Policy reference]: api-management-policy-reference.md
[API Management policies]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance




