<properties 
    pageTitle="Najważniejsze pojęcia dotyczące usługi API Management" 
    description="Poznaj interfejsy API, produkty, role, grupy i inne ważne pojęcia dotyczące usługi API Management." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="05/24/2016" 
    ms.author="sdanie"/>

#Co to jest API Management?

Usługa API Management pomaga organizacjom publikować interfejsy API dla deweloperów zewnętrznych, partnerskich i wewnętrznych, aby w pełni wykorzystać potencjał danych i usług. Firmy na całym świecie dążą do rozszerzenia operacji w postaci platformy cyfrowej, tworząc nowe kanały, znajdując nowych klientów i nakłaniając obecnych do głębszego zaangażowania. Usługa API Management zapewnia podstawowe możliwości, które pozwalają na pomyślne programowanie interfejsów API przez zaangażowanych deweloperów, a także udostępnia wgląd w kwestie biznesowe, analizy, zabezpieczenia i ochronę.

Obejrzyj poniższy film przedstawiający przegląd usługi Azure API Management i dowiedz się, jak używać usługi API Management, aby przy minimalnym nakładzie pracy dodać do interfejsu API wiele funkcji obejmujących kontrolę dostępu, ograniczanie liczby wywołań, monitorowanie, rejestrowanie zdarzeń i buforowanie odpowiedzi.

> [AZURE.VIDEO azure-api-management-overview]

Aby użyć usługi API Management, administratorzy tworzą interfejsy API. Każdy interfejs API składa się z co najmniej jednej operacji i można go dodać do co najmniej jednego produktu. Aby użyć interfejsu API, deweloperzy subskrybują produkt, który zawiera ten interfejs API, a następnie wywołują operację interfejsu API, która podlega obowiązującym dla niej zasadom użycia.

Ten temat zawiera przegląd najważniejszych założeń usługi API Management.

>[AZURE.NOTE] Aby uzyskać więcej informacji, zobacz oficjalny dokument PDF [Cloud-based API Management: Harnessing the Power of APIs](http://j.mp/ms-apim-whitepaper) (Oparta na chmurze usługa API Management: wykorzystanie możliwości interfejsów API). Ten wprowadzający oficjalny dokument dotyczący usługi API Management opracowany przez firmę CITO Research obejmuje następujące tematy: 
>
> - Typowe wymagania i wyzwania związane z interfejsem API
>     - Rozdzielanie interfejsów API i prezentowanie fasad
>     - Szybkie rozpoczęcie pracy przez deweloperów
>     - Zabezpieczanie dostępu
>     - Analizy i metryki
> - Uzyskiwanie kontroli i wglądu dzięki platformie usługi API Management
> - Porównanie rozwiązań w chmurze i lokalnych
> - Usługa Azure API Management

## <a name="apis"> </a>Interfejsy API i operacje

Interfejsy API są podstawą wystąpienia usługi API Management. Każdy interfejs API reprezentuje zestaw operacji dostępnych dla deweloperów. Każdy interfejs API zawiera odwołanie do usługi zaplecza, która implementuje interfejs API, oraz mapę odwzorowań operacji interfejsu API na operacje zaimplementowane w usłudze zaplecza. Operacje usługi API Management są wysoce konfigurowalne, pozwalają na kontrolę mapowania adresu URL, parametrów zapytania i ścieżki, zawartości żądania i odpowiedzi oraz buforowanie odpowiedzi operacji. Ograniczenia liczby wywołań, przydziały i zasady ograniczeń adresów IP mogą być implementowane zarówno na poziomie interfejsu API, jak i na poziomie poszczególnych operacji.

Aby uzyskać więcej informacji, zobacz artykuły [How to create APIs][] (Tworzenie interfejsów API) i [How to add operations to an API][] (Dodawanie operacji do interfejsu API).


## <a name="products"> </a> Produkty

Produkty stanowią sposób udostępniania interfejsów API deweloperom. Produkty w usłudze API Management mają co najmniej jeden interfejs API oraz skonfigurowany tytuł, opis i warunki użytkowania. Produkty mogą być **otwarte** lub **chronione**. Produkty chronione trzeba subskrybować przed użyciem, a produkty otwarte mogą być używane bez subskrypcji. Kiedy produkt jest gotowy do użycia przez deweloperów, można go opublikować. Po opublikowaniu produktu deweloperzy mogą go wyświetlać (a w przypadku produktów chronionych również subskrybować). Zatwierdzenie subskrypcji jest konfigurowane na poziomie produktu i może wymagać zatwierdzenia przez administratora lub odbywać się automatycznie.

Grupy służą do zarządzania widocznością produktów dla deweloperów. Widoczność produktów jest przydzielana według grup, a deweloperzy mogą wyświetlać i subskrybować produkty, które są widoczne dla grup, do których należą. 

Aby uzyskać więcej informacji, zobacz artykuł [How to create and publish a product][] (Tworzenie i publikowanie produktu) i następujący film.

> [AZURE.VIDEO using-products]

## <a name="groups"> </a> Grupy

Grupy służą do zarządzania widocznością produktów dla deweloperów. Usługa API Management ma następujące niezmienne grupy systemowe.

-   **Administratorzy** — do tej grupy należą administratorzy subskrypcji platformy Azure. Administratorzy zarządzają wystąpieniami usługi API Management, tworząc interfejsy API, operacje i produkty, które są używane przez deweloperów.
-   **Deweloperzy** — do tej grupy należą uwierzytelnieni użytkownicy portalu dla deweloperów. Deweloperzy to klienci, którzy tworzą aplikacje przy użyciu interfejsów API. Deweloperzy otrzymują dostęp do portalu dla deweloperów i tworzą aplikacje, które wywołują operacje interfejsów API.
-   **Goście** — do tej grupy należą nieuwierzytelnieni użytkownicy portalu dla deweloperów, tacy jak potencjalni klienci odwiedzający portal dla deweloperów wystąpienia usługi API Management. Mogą mieć przyznany pewien zakres dostępu tylko do odczytu, czyli np. możliwość wyświetlania interfejsów API, ale nie ich wywoływania.

Oprócz tych grup systemowych administratorzy mogą tworzyć niestandardowe grupy lub [wykorzystać zewnętrzne grupy w skojarzonych dzierżawach usługi Azure Active Directory](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Niestandardowe i zewnętrzne grupy mogą być używane razem z grupami systemowymi, zapewniając deweloperom widoczność produktów interfejsu API i dostęp do nich. Można na przykład utworzyć jedną grupę niestandardową dla deweloperów powiązanych z konkretną organizacją partnera i zezwolić im na dostęp do interfejsów API z produktów zawierających tylko odpowiednie interfejsy API. Użytkownik może należeć do więcej niż jednej grupy.

Aby uzyskać więcej informacji, zobacz artykuł [How to create and use groups][] (Tworzenie i używanie grup).

## <a name="developers"> </a> Deweloperzy

Deweloperzy reprezentują konta użytkowników w wystąpieniu usługi API Management. Konta deweloperów mogą być tworzone lub dołączane na zaproszenie administratorów. Ponadto deweloperzy mogą się zarejestrować w [Portalu dla deweloperów][]. Każdy deweloper jest członkiem jednej lub wielu grup i może subskrybować produkty, które mają przyznaną widoczność w tych grupach.

Kiedy deweloperzy subskrybują produkt, otrzymują klucz podstawowy i pomocniczy produktu. Ten klucz jest używany podczas wywołań interfejsów API produktu.

Aby uzyskać więcej informacji, zobacz artykuły [How to create or invite developers][] (Tworzenie i zapraszanie deweloperów) i [How to associate groups with developers][] (Kojarzenie grup z deweloperami).

## <a name="policies"> </a> Zasady

Zasady są zaawansowaną możliwością usługi API Management, która pozwala wydawcy zmieniać zachowanie interfejsu API za pomocą konfiguracji. Zasady to zbiór instrukcji, które są wykonywane sekwencyjnie podczas żądania lub odpowiedzi interfejsu API. Popularne instrukcje obejmują konwersję z formatu XML do JSON i ograniczanie szybkości połączenia, aby ograniczyć liczbę wywołań przychodzących od dewelopera. Ponadto dostępnych jest wiele innych zasad.

Wyrażenia zasad mogą służyć jako wartości atrybutów lub wartości tekstowe w dowolnej z zasad usługi API Management, o ile w zasadach nie określono inaczej. Niektóre zasady, np. [Przepływ sterowania](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) i [Ustawianie zmiennej](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable), są oparte na wyrażeniach zasad. Aby uzyskać więcej informacji, zobacz tematy [Advanced policies](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies) (Zaawansowane zasady) i [Policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx) (Wyrażenia zasad) oraz obejrzyj następujący film.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

Pełna lista zasad usługi API Management jest zawarta w artykule [Policy reference][] (Dokumentacja zasad). Aby uzyskać więcej informacji na temat korzystania z zasad i konfigurowania ich, zobacz artykuł [API Management policies][] (Zasady usługi API Management). Samouczek dotyczący tworzenia produktu z zasadami dotyczącymi ograniczania liczby wywołań i przydziałów jest zawarty w artykule [Tworzenie i konfigurowanie zaawansowanych ustawień produktów][]. Demonstracja jest pokazana w następującym filmie.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"> </a> Portalu dla deweloperów

Portal dla deweloperów jest miejscem, gdzie deweloperzy mogą poznawać interfejsy API, wyświetlać i wywoływać operacje oraz subskrybować produkty. Potencjalni klienci mogą odwiedzić portal dla deweloperów, wyświetlić interfejsy API i operacje oraz zarejestrować się. Adres URL do portalu dla deweloperów znajduje się na pulpicie nawigacyjnym w klasycznym portalu Azure danego wystąpienia usługi API Management.

Wygląd swojego portalu dla deweloperów możesz dostosować, dodając niestandardową zawartość, dostosowując style i dodając znak marki.

## Usługa API Management i ekonomia interfejsów API

Aby dowiedzieć się więcej na temat usługi API Management, obejrzyj następującą prezentację z konferencji Microsoft Ignite 2015.

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[Interfejsy API i operacje]: #apis
[Produkty]: #products
[Grupy]: #groups
[Deweloperzy]: #developers
[Zasady]: #policies
[Portalu dla deweloperów]: #developer-portal

[How to create APIs]: api-management-howto-create-apis.md
[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer
[Tworzenie i konfigurowanie zaawansowanych ustawień produktów]: api-management-howto-product-with-rules.md
[How to create or invite developers]: api-management-howto-create-or-invite-developers.md
[Policy reference]: api-management-policy-reference.md
[API Management policies]: api-management-howto-policies.md
[Tworzenie wystąpienia usługi API Management]: api-management-get-started.md#create-service-instance



 



<!--HONumber=jun16_HO2-->


