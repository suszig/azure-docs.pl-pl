---
title: "Bezpieczeństwo danych i operacji w usłudze Azure Search | Dokumentacja firmy Microsoft"
description: "Azure zabezpieczenia wyszukiwania jest oparte na zgodności SOC 2, szyfrowania, uwierzytelnianie i tożsamość dostęp za pośrednictwem użytkowników i identyfikatorach grup zabezpieczeń w filtrach usługi Azure Search."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 12/14/2017
ms.author: heidist
ms.openlocfilehash: 23616c70a5fd336b743f5acfad2601a6c3e23fc4
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="data-security-and-controlled-access-to-azure-search-operations"></a>Bezpieczeństwo danych i kontroli dostępu do operacji usługi Azure Search

Usługa Azure Search jest [SOC 2 zgodne](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports)z zabezpieczeń kompleksowe Architektura rozszerzania zabezpieczeń fizycznych, zaszyfrowane transmisje zaszyfrowanych magazynów i zabezpieczenia całej platformy oprogramowania. Pod względem operacyjnym usługi Azure Search akceptuje tylko uwierzytelnione żądania. Opcjonalnie można dodać użytkownika kontroli dostępu do zawartości. W tym artykule dotyczące zabezpieczeń w każdej warstwie, ale koncentruje się przede wszystkim na jak są zabezpieczone dane i operacji w usłudze Azure Search.

![Diagram blokowy serwera warstwy zabezpieczeń](media/search-security-overview/azsearch-security-diagram.png)

Usługi Azure Search dziedziczy ochronę i zabezpieczeń platformy Azure, podstawowego mechanizmu używanego przez sama usługa jest uwierzytelniania opartego na kluczach, której typ klucza określa poziom dostępu. Klucz jest klucz administratora i klucz zapytania, aby uzyskać dostęp tylko do odczytu.

Dostęp do usługi jest oparty na szereg uprawnienia przekazywanych przez klucz (pełna lub tylko do odczytu), a także kontekstu, który definiuje zakres działań. Każde żądanie składa się z klucza obowiązkowe, operacji i obiektu. Gdy połączonych ze sobą, dwa poziomy uprawnień oraz kontekst są wystarczające dla zapewnianie bezpieczeństwa full spektrum na nich operacji usługi. 

## <a name="physical-security"></a>Zabezpieczenia fizyczne

Centrach danych firmy Microsoft Podaj branży zabezpieczeń fizycznych i są zgodne z szeroką gamę portfolio standardów i przepisami. Aby dowiedzieć się więcej, przejdź do [centrów danych globalnych](https://www.microsoft.com/cloud-platform/global-datacenters) strony lub Obejrzyj krótki klip wideo na danych Centrum zabezpieczeń.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]

## <a name="encrypted-transmission-and-storage"></a>Zaszyfrowane transmisji i przechowywania

Usługa Azure Search nasłuchuje na porcie protokołu HTTPS 443. Na platformie połączenia do usług platformy Azure są szyfrowane. 

Indeksy i innych konstrukcji magazynu wewnętrznej bazy danych usługi Azure Search korzysta z możliwości szyfrowania tych platform. Pełna [AICPA SOC 2 zgodności](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) jest dostępna dla wszystkich wyszukiwania usług (nowych i istniejących), wszystkie centrum danych w ofercie usługi Azure Search. Aby przejrzeć pełny raport, przejdź do [Azure - i Azure dla instytucji rządowych SOC 2 typu II raport](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports).

Szyfrowanie jest niezauważalne, z kluczami szyfrowania zarządzane wewnętrznie i powszechnie stosowane. Nie można wyłączyć dla określonych wyszukiwania usług lub indeksów, ani zarządzać kluczami bezpośrednio ani podać własne. 

## <a name="azure-wide-logical-security"></a>Azure zabezpieczeń logicznych

Kilka mechanizmów zabezpieczeń są dostępne w stosie Azure i w związku z tym automatycznie dostępne zasoby usługi Azure Search, które można utworzyć.

+ [Blokad na poziomie zasobów, aby zapobiec usunięciu lub subskrypcji](../azure-resource-manager/resource-group-lock-resources.md)
+ [Oparta na rolach kontroli dostępu (RBAC) do kontrolowania dostępu do informacji i wykonywanie operacji administracyjnych](../active-directory/role-based-access-control-what-is.md)

Wszystkie usługi Azure obsługę kontroli dostępu opartej na rolach (RBAC) ustawienie poziomy dostępu konsekwentnie we wszystkich usługach. Na przykład wyświetlanie poufnych danych, takich jak klucz administratora jest ograniczone do właściciela i współautor role, wyświetlanie stanu usługi jest dostępne dla członków żadnej roli. RBAC dostępnych ról właściciela, współautora ani czytnika. Domyślnie w przypadku wszystkich administratorów usługi są członkami roli właściciela.

## <a name="service-authentication"></a>Usługi uwierzytelniania

Usługa Azure Search udostępnia własnej metody uwierzytelniania. Uwierzytelnianie występuje na każde żądanie i jest oparta na klucz dostępu, która określa zakres operacji. Klucz dostępu prawidłowy jest uznawany za dowód żądanie pochodzi z zaufanego jednostki. 

Uwierzytelnianie na usługi istnieje na dwa poziomy: pełne prawa, tylko do zapytania. Typ klucza określa poziom dostępu jest włączona.

|Klucz|Opis|Limity|  
|---------|-----------------|------------|  
|Administrator|Przyznaje pełne prawa do wszystkich operacji, łącznie z możliwością zarządzania usługą oraz tworzenia i usuwania **indeksów**, **indeksatory**, i **źródeł danych**.<br /><br /> Dwa admin **klucze interfejsu api**, nazywany *głównej* i *dodatkowej* klucze w portalu, są generowane, gdy usługa jest tworzony i można osobno ponownie wygenerowane na żądanie . Mając dwa klucze umożliwia przerzucane jednego klucza podczas używania drugi klucz dla nieprzerwanego dostępu do usługi.<br /><br /> Kluczy administratora są tylko określone w nagłówkach żądania HTTP. Nie można umieścić administrator **klucz interfejsu api** w adresie URL.|Maksymalnie 2 dla usługi|  
|Zapytanie|Zezwala na dostęp tylko do odczytu do indeksów oraz dokumentów i są zazwyczaj dystrybuowane do aplikacji klienckich, które wysyłają żądania wyszukiwania.<br /><br /> Klucze zapytania są tworzone na żądanie. Można je utworzyć ręcznie w portalu lub programowo przy użyciu [interfejsu API REST zarządzania](https://docs.microsoft.com/rest/api/searchmanagement/).<br /><br /> Klucze zapytania można określić w nagłówku żądania HTTP do wyszukiwania, sugestię lub operacji wyszukiwania. Alternatywnie można przekazać klucz zapytania jako parametr przy użyciu adresu URL. W zależności od tego, jak aplikacja kliencka formulates żądania może być łatwiejsze do przekazania klucza jako parametr zapytania:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01&api-key=A8DA81E03F809FE166ADDB183E9ED84D`|50 dla usługi|  

 Efekty wizualne nie ma ma różnicy między klucz administratora i klucz zapytania. Oba klucze są ciągi składa się z 32 losowo wygenerowany znaków alfanumerycznych. Jeśli zgubisz track określono typ klucza w aplikacji, możesz [Sprawdź wartości klucza w portalu](https://portal.azure.com) lub użyj [interfejsu API REST](https://docs.microsoft.com/rest/api/searchmanagement/) aby zwrócić wartości i typ klucza.  

> [!NOTE]  
>  Jest on uznawany za rozwiązanie w zakresie zabezpieczeń niską do przekazywania poufnych danych, takich jak `api-key` w identyfikatorze URI żądania. Z tego powodu usługa Azure Search akceptuje tylko klucz zapytania jako `api-key` w zapytaniu ciąg, na które należy unikać chyba, że zawartość indeksu powinny być publicznie dostępne w ten sposób. Ogólną zasadą jest, zalecamy przekazywanie Twojej `api-key` jako nagłówek żądania.  

### <a name="how-to-find-the-access-keys-for-your-service"></a>Jak znaleźć klucze dostępu dla usługi

Możesz uzyskać dostępu do kluczy w portalu lub za pomocą [interfejsu API REST zarządzania](https://docs.microsoft.com/rest/api/searchmanagement/). Aby uzyskać więcej informacji, zobacz [zarządzanie kluczami](search-manage.md#manage-api-keys).

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Lista [usługi wyszukiwania](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) dla Twojej subskrypcji.
3. Wybierz usługę, a następnie odnaleźć na stronie usługi **ustawienia** >**klucze** do wyświetlania kluczy administratora, jak i zapytania.

![Sekcję klucze strony portalu, ustawienia](media/search-security-overview/settings-keys.png)

## <a name="index-access"></a>Dostęp do indeksu

W usłudze Azure Search konkretnego indeksu nie jest zabezpieczanego obiektu. Zamiast tego dostęp do indeksu jest określany w warstwie usług (odczytu lub zapisu), wraz z kontekstu operacji.

W przypadku dostępu użytkownika końcowego można tworzyć struktury żądań zapytań w aplikacji nawiązywanie połączeń za pomocą klucza zapytania, co sprawia, że wszystkie żądania tylko do odczytu i obejmują określonego indeksu używany przez aplikację. W żądanie kwerendy nie ma żadnych koncepcji indeksy lub jednocześnie dostęp do wielu indeksów, więc wszystkie żądania target jeden indeks przez definicję. Tak struktura żądania zapytania sam (klucz oraz indeksu pojedynczym elementem docelowym) definiuje granicy zabezpieczeń.

Jest niesortowalne administratorów i deweloperów do indeksów: zarówno musi uzyskać dostęp do tworzenia, usuwania i aktualizowania obiektów zarządzanych przez usługę. Każda osoba mająca klucz administratora do usługi można odczytać, zmodyfikować lub usunąć indeksu w tej samej usługi. Do ochrony przed przypadkowym lub złośliwymi usuwania indeksów do kontroli źródła wewnętrznych zasobów kodu jest remedy dla cofania niechciane indeksu usunięcie lub zmiana. Usługa wyszukiwanie Azure ma trybu failover w klastrze, aby zapewnić dostępność, ale nie przechowuje ani nie wykonać zastrzeżonych kod służący do tworzenia lub załadować indeksów.

Wielodostępności rozwiązań wymagających granic zabezpieczeń na poziomie indeksu takich rozwiązań są zwykle zawierają warstwy środkowej, którzy klienci używa do obsługi izolacji indeksu. Aby uzyskać więcej informacji na temat przypadek użycia wielodostępnym, zobacz [wzorce dla wielodostępnych aplikacji SaaS i usługi Azure Search projektowe](search-modeling-multitenant-saas-applications.md).

## <a name="admin-access-from-client-apps"></a>Dostęp administratora z aplikacji klienta

Interfejsu API REST usługi Azure Search zarządzania jest rozszerzeniem z usługi Azure Resource Manager i udostępnia jego zależności. W efekcie usługi Active Directory jest wymagana do zarządzania usługi Azure Search. Wszystkie żądania administracyjnych z kodu klienta musi zostać uwierzytelniony przy użyciu usługi Azure Active Directory, zanim żądanie dotrze Menedżera zasobów.

Dane żądania do punktu końcowego usługi Azure Search, takie jak Create Index (wyszukiwania usługi interfejsu API REST Azure) lub dokumenty wyszukiwania (wyszukiwania usługi interfejsu API REST Azure), użyj klucz interfejsu api w nagłówku żądania.

Kod aplikacji obsługuje operacje administracyjne usługi, a także operacje na danych w dokumentach lub indeksy wyszukiwania, należy wdrożyć dwa podejścia do uwierzytelniania w kodzie: klucz dostępu kodu natywnego do usługi Azure Search i uwierzytelniania usługi Active Directory metody wymagane przez Menedżera zasobów. 

Aby dowiedzieć się, jak struktury żądania w usłudze Azure Search, zobacz [REST usługi wyszukiwanie Azure](https://docs.microsoft.com/rest/api/searchservice/). Aby uzyskać więcej informacji na temat wymagań dotyczących uwierzytelniania dla Menedżera zasobów, zobacz [interfejs API uwierzytelniania Użyj Menedżera zasobów do subskrypcji dostępu](../azure-resource-manager/resource-manager-api-authentication.md).

## <a name="user-access-to-index-content"></a>Dostęp użytkownika do indeksu zawartości

Dostęp użytkownika do zawartości indeksu jest implementowane za pośrednictwem filtrów zabezpieczeń na zapytań, zwracając dokumenty skojarzone z tożsamością zabezpieczeń. Zamiast wstępnie zdefiniowanych ról i przypisania ról kontroli dostępu na podstawie tożsamości jest implementowany jako filtr czy TRIM wyszukiwanie wyników dokumentów i zawartości na podstawie tożsamości. W poniższej tabeli opisano dwa podejścia do wyniki wyszukiwania przycinanie nieautoryzowanego zawartości.

| Podejście | Opis |
|----------|-------------|
|[Przycinanie zabezpieczeń oparte na tożsamości, filtry](search-security-trimming-for-azure-search.md)  | Dokumenty podstawowy przepływ pracy do wykonywania kontroli dostępu tożsamości użytkownika. Obejmuje dodawanie identyfikatorów zabezpieczeń do indeksu i wyjaśniono, filtrowania pod kątem tego pola, aby przyciąć wyniki zabronione zawartości. |
|[Przycinanie zabezpieczeń oparte na tożsamości usługi Azure Active Directory](search-security-trimming-for-azure-search-with-aad.md)  | W tym artykule jest rozwijane w poprzednim artykule etapy pobierania tożsamości z usługi Azure Active Directory (AAD), jeden z [wolne usług](https://azure.microsoft.com/free/) na platformie Azure chmury. |

## <a name="table-permissioned-operations"></a>Tabeli: Operacje Permissioned

W poniższej tabeli przedstawiono operacje dozwolone w usłudze Azure Search i klucz, który umożliwia odblokowanie dostępu do określonej operacji.

| Operacja | Uprawnienia |
|-----------|-------------------------|
| Tworzenie usługi | Posiadacz subskrypcji platformy Azure|
| Skalowanie usługi | Klucz administratora, RBAC właścicielem lub współautorem zasobu  |
| Usuwanie usługi | Klucz administratora, RBAC właścicielem lub współautorem zasobu |
| Tworzenie, modyfikowanie i usuwanie obiektów usługi: <br>Indeksy i składniki (takie jak definicje analizatora, oceniania profile, opcje mechanizmu CORS), indeksatorów, źródła danych, synonimy, sugestorów. | Klucz administratora, RBAC właścicielem lub współautorem zasobu  |
| Tworzenie zapytań względem indeksu | Klucz administratora lub zapytania (nie dotyczy RBAC) |
| Wyślij zapytanie do informacji o systemie, takich jak zwracanie statystyk, liczby i listy obiektów. | Klucz administratora, RBAC zasobu (czytnik właściciela, współautora) |
| Zarządzanie kluczami administratora | Klucz administratora, RBAC właścicielem lub współautorem zasobu. |
| Zarządzanie kluczami zapytań |  Klucz administratora, RBAC właścicielem lub współautorem zasobu. Czytnik RBAC można wyświetlić klucze zapytania. |


## <a name="see-also"></a>Zobacz też

+ [Pobierz uruchomiona .NET (pokazano tworzenie indeksu za pomocą klucza administratora)](search-create-index-dotnet.md)
+ [Pobierz uruchomiona REST (pokazano tworzenie indeksu za pomocą klucza administratora)](search-create-index-rest-api.md)
+ [Kontrola dostępu oparta na tożsamości, za pomocą usługi Azure Search filtrów](search-security-trimming-for-azure-search.md)
+ [Kontrola dostępu na podstawie tożsamości katalogu Active za pomocą usługi Azure Search filtrów](search-security-trimming-for-azure-search-with-aad.md)
+ [Filtry w usłudze Azure Search](search-filters.md)