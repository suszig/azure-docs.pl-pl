---
title: "Przegląd zasad Azure | Dokumentacja firmy Microsoft"
description: "Zasady usługi Azure to usługa na platformie Azure, którego używasz do tworzenia, przypisywania i zarządzanie definicje zasad w środowisku platformy Azure."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark; nini
ms.date: 11/06/2017
ms.topic: overview
ms.service: azure-policy
manager: jochan
ms.custom: mvc
ms.openlocfilehash: b784c79166eb614988d84b4553bb95e4d1e457af
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="what-is-azure-policy"></a>Co to jest Azure zasady?

Zarządzanie IT tworzy jasności między cele biznesowe i projektach. Dobrym informatyczną obejmuje Planowanie z inicjatywy i ustawianie priorytetów na poziomie strategicznych. Firma wystąpić znaczących problemów IT, które nie wydają się pobrać rozwiązany? Wdrażanie zasad pomaga lepiej zarządzać i uniemożliwić ich. Wdrażanie zasad jest, gdzie zasadami Azure jest dostarczany.

Zasady usługi Azure to usługa na platformie Azure, używanej do tworzenia, przypisywania i definicje zasad zarządzania. Definicje zasad wymusza różnych zasad i akcje za pośrednictwem zasobów, dlatego te zasoby pozostają zgodne ze standardami firmy, a umów dotyczących poziomu usług. Zasady usługi Azure uruchomienie ocenę zasobów, skanowanie w poszukiwaniu tych, które nie są zgodne z definicje zasad, do których masz. Na przykład można mieć zasad, aby zezwolić tylko niektóre maszyny wirtualne są. Inny musi mieć wszystkie zasoby określony tag. Te zasady są oceniane oddzielnie podczas tworzenia i aktualizowania zasobów.

## <a name="how-is-it-different-from-rbac"></a>Czym się różni od RBAC?

Istnieje kilka podstawowych różnic między zasadami i kontroli dostępu opartej na rolach (RABC). RBAC koncentruje się na działania użytkownika w innych zakresach. Na przykład możesz mogą być dodane do roli współautora dla grupy zasobów na żądany zakres. Rola pozwala wprowadzać zmiany do tej grupy zasobów. Zasady skupia się wokół właściwości zasobów podczas wdrażania i dla już istniejących zasobów. Na przykład za pomocą zasad, można kontrolować typów zasobów, które mogą być udostępniane. Alternatywnie można ograniczyć lokalizacje, w których można udostępnić zasoby. W odróżnieniu od RBAC, zasady są domyślnie Zezwól jawne i odmawianie systemu.

Aby użyć zasad, użytkownik musi zostać uwierzytelniony za pośrednictwem RBAC. W szczególności, Twoje konto musi:

- `Microsoft.Authorization/policydefinitions/write`uprawnienia do definiowania zasad.
- `Microsoft.Authorization/policyassignments/write`uprawnienie do przypisywania zasad.

Te uprawnienia nie są uwzględnione w **współautora** roli.


## <a name="policy-definition"></a>Definicja zasad

Każdej definicji zasad zawiera warunki, w których jest wymuszana. I ma towarzyszącej akcji, która ma miejsce, jeśli warunki są spełnione.

W zasadach Azure oferujemy niektórych wbudowanych zasad, które są domyślnie dostępne. Na przykład:

- **Wymaga programu SQL Server 12.0**: definicja zasad ma warunki/reguły, aby upewnić się, że wszystkie serwery SQL, użyj wersji 12.0. Jego działania jest odrzucanie wszystkich serwerów, które nie spełniają te kryteria.
- **Dozwolone jednostki SKU konta magazynu**: definicja zasad ma zestaw warunków/reguł, które określa, czy konto magazynu, który jest wdrażany jest w zestawie, rozmiar jednostki SKU. Jego działania jest odrzucanie wszystkich serwerów, które nie są zgodne do zestawu zdefiniowanego rozmiary jednostki SKU.
- **Dozwolony typ zasobu**: definicja zasad ma zestaw warunków/reguł, aby określić typy zasobów, które można wdrożyć w organizacji. Jego działania jest odrzucanie wszystkie zasoby, które nie są częścią tego zdefiniowanej listy.
- **Dozwolone lokalizacje**: ta zasada umożliwia ograniczyć lokalizacje, które organizacji można określić podczas wdrażania zasobów. Jego działania jest używany do wymuszenia wymagania dotyczące zgodności geo.
- **Dozwolone jednostki SKU maszyny wirtualnej**: tych zasad można określić zestaw maszyny wirtualnej jednostki magazynowe, które można wdrożyć w organizacji.
- **Stosuje tag i wartość domyślną**: ta zasada dotyczy wymaganego tagu i wartość domyślną, jeśli nie jest określona przez użytkownika.
- **Wymuszanie tagu i jej wartość**: te zasady wymuszają zastosowanie wymaganego tagu i jej wartość do zasobu.
- **Niedozwolone typy zasobów**: ta zasada umożliwia określenie typów zasobów, których nie można wdrożyć w organizacji.

Można przypisać dowolnej z tych zasad za pośrednictwem portalu Azure, programu PowerShell lub wiersza polecenia platformy Azure.

Aby dowiedzieć się więcej na temat struktury definicje zasad, Szukaj w tym artykule - [struktura definicji zasad](../azure-resource-manager/resource-manager-policy.md#policy-definition-structure).

## <a name="policy-assignment"></a>Przypisania zasad

Przypisania zasad jest sformułowanie zasad przypisany została wykonana w określonym zakresie. Ten zakres może należeć do zakresu od grupy zarządzania do grupy zasobów. Termin *zakres* odwołuje się do wszystkich grup zasobów, subskrypcji lub grup zarządzania, które przypisano definicji zasad. Przypisania zasad są dziedziczone przez wszystkie zasoby podrzędne. Tak Jeśli zasady są stosowane do grupy zasobów, jest stosowane do wszystkich zasobów w danej grupie zasobów. Jednak można wykluczyć subscope z przypisania zasad. Na przykład w zakresie subskrypcji można przypisać zasady, która uniemożliwia tworzenie zasobów sieciowych. Jednak możesz wykluczyć jednej grupy zasobów w ramach subskrypcji, która jest przeznaczona dla infrastruktury sieci. Można przyznać dostęp do tej grupy zasobów sieciowych, użytkownikom, którym ufasz z tworzeniem zasobów sieciowych.

Aby uzyskać więcej informacji na definicji ustawień zasad i przypisań, zobacz [Utwórz przypisanie zasad, aby zidentyfikować niezgodnych zasobów w środowisku Azure](assign-policy-definition.md).

## <a name="policy-parameters"></a>Parametry zasad

Parametry zasad ułatwić zarządzanie zasadami dzięki zmniejszeniu liczby definicje zasad, które należy utworzyć. Podczas tworzenia definicji zasad, aby był bardziej ogólnym, można zdefiniować parametrów. Następnie można ponownie użyć tej definicji zasad dla różnych scenariuszy. Można to zrobić przez przekazywanie różne wartości podczas przypisywania definicji zasad. Na przykład określenie jeden zestaw lokalizacji dla subskrypcji.

Parametry są zdefiniowane/tworzone podczas tworzenia definicji zasad. Jeśli parametr jest zdefiniowana, otrzymuje nazwę i opcjonalnie danej wartości. Na przykład można zdefiniować parametr dla zasad zatytułowany *lokalizacji*. Następnie można określić różne wartości takich jak *EastUS* lub *WestUS* podczas przypisywania zasad.

<!--
Next link should point to new Concept page for Parameters
-->
Aby uzyskać więcej informacji na temat parametrów zasad, zobacz [Przegląd zasad zasobów — parametry](../azure-resource-manager/resource-manager-policy.md#parameters).

## <a name="initiative-definition"></a>Inicjatywy definicji
Definicja inicjatywy jest kolekcja definicji zasad, które są dostosowane do osiągnięcia pojedynczej celem nadrzędna. Definicje inicjatywy uprościć zarządzanie i przypisywanie definicje zasad. Uprościć ich grupowanie zestawu zasad jako jedną pojedynczy element. Na przykład można utworzyć inicjatywą zatytułowany **Włączanie monitorowania w Centrum zabezpieczeń Azure**, mając na celu monitorować wszystkie dostępne zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure.

W obszarze tej inicjatywy trzeba definicje zasad takich jak:

1. **Monitor bez szyfrowania bazy danych SQL w Centrum zabezpieczeń** — w przypadku monitorowania niezaszyfrowane baz danych i serwerów.
2. **Monitorowanie luk w zabezpieczeniach systemu operacyjnego w Centrum zabezpieczeń** — monitorowania serwerów, które nie spełniają skonfigurowanych linii bazowej.
3. **Monitorować brakujący program Endpoint Protection w Centrum zabezpieczeń** — monitorowania serwerów bez agenta ochrony zainstalowanego punktu końcowego.

<!--
For more information about initiative definitions, see Initiative Definitions.+ (instead of linking to this, link out to Concept page on Initiative Definitions)
-->

## <a name="initiative-assignment"></a>Inicjatywy przypisania
Podobnie jak przypisanie zasad inicjatywy przydziału jest inicjatywy definicji przypisane do określonego zakresu. Przydziały inicjatywy zmniejszenia konieczności przeprowadzania kilku inicjatywy definicje dla każdego zakresu. Ten zakres można również należeć do zakresu od grupy zarządzania do grupy zasobów.

W powyższym przykładzie **Włączanie monitorowania w Centrum zabezpieczeń Azure** inicjatywy mogą być przypisane do różnych zakresów. Na przykład można przypisać do jednego przypisania **subscriptionA**. Inny można przypisać do **subscriptionB**.

## <a name="initiative-parameters"></a>Parametry inicjatywy
Podobnie jak parametry zasad inicjatywy parametry uproszczenia inicjatywy zarządzania zmniejszając nadmiarowości. Parametry inicjatywy są zasadniczo listę parametrów używanych przez definicje zasad w ramach tej inicjatywy.

Na przykład wykonać scenariusz, w którym masz inicjatywy definicji - **initiativeC**, z dwie definicje zasad. Każda definicja zasad o jeden parametr zdefiniowane:

| Zasady | Nazwa parametru |Typ parametru  |Uwaga |
|---|---|---|---|
| policyA | allowedLocations | Tablica  |Ten parametr oczekuje listę ciągów dla wartości, ponieważ typ parametru została zdefiniowana jako tablica |
| policyB | allowedSingleLocation |Ciąg |Ten parametr oczekuje wartości o jedno słowo, ponieważ typ parametru została zdefiniowana jako ciąg |

W tym scenariuszu podczas definiowania inicjatywy parametry **initiativeC**, dostępne są trzy opcje:

1. Użyj parametrów definicje zasad w ramach tej inicjatywy: W tym przykładzie *allowedLocations* i *allowedSingleLocation* stają się inicjatywy parametrów **initiativeC** .
2. Podaj wartości parametrów definicje zasad w ramach tej inicjatywy definicji. W tym przykładzie można udostępnić listę lokalizacje **policyA przez parametr — allowedLocations** i **policyB przez parametr — allowedSingleLocation**.
Można również podać wartości podczas przypisywania tej inicjatywy.
3. Podaj listę *wartość* opcje, które mogą być używane podczas przypisywania tej inicjatywy. Po przypisaniu tej inicjatywy parametry dziedziczone z definicji zasad w tej inicjatywy może zawierać tylko wartości z tej listy podana.

Na przykład można utworzyć listę opcji wartość w definicji inicjatywy, która zawiera *EastUS*, *WestUS*, *CentralUS*, i *WestEurope*. Jeśli tak, nie jako danych wejściowych innej wartości, takich jak *Azja południowo-wschodnia* inicjatywy przypisania, ponieważ nie jest częścią listy.

## <a name="recommendations-for-managing-policies"></a>Zalecenia dotyczące zarządzania zasadami

Podczas tworzenia i zarządzania definicje zasad i przypisań, poniżej przedstawiono kilka wskazówek, zaleca się wykonanie:

- Jeśli tworzysz definicje zasad w środowisku, zalecamy Rozpoczynanie od efekt inspekcji, w przeciwieństwie do efekt Odmów, aby śledzić wpływ z definicji zasad zasobów w danym środowisku. Jeśli masz skrypty już w miejscu, aby stosować automatyczne skalowanie zapasowych aplikacji, ustawienie efektu Odmów może utrudniać tych zadań automatyzacji, które już zostały spełnione.
- Należy pamiętać o hierarchii organizacyjnej podczas tworzenia definicji i przypisania. Zaleca się tworzenie definicji na poziomie subskrypcji lub wyższego poziomu, na przykład w grupie zarządzania i przypisywanie na poziomie podrzędnym dalej. Na przykład w przypadku utworzenia definicji zasad na poziomie grupy zarządzania, przypisanie zasad tej definicji można zakresu do poziomu subskrypcji, w grupie zarządzania.
- Firma Microsoft zachęca przy użyciu standardu cenowym, aby lepiej zrozumieć stan zgodności danego środowiska. Aby uzyskać więcej informacji o naszych cenową modeli i jakie każdego z nich oferty, Przyjrzyjmy się [cennik](https://azure.microsoft.com/pricing/details/azure-policy).
- Firma Microsoft zaleca zawsze inicjatywy definicje zamiast definicje zasad, nawet jeśli masz tylko jedną zasadę pamiętać. Na przykład, jeśli masz definicję zasad — *policyDefA* i utwórz go w definicji inicjatywy - *initiativeDefC*, jeśli użytkownik chce utworzyć inną definicję zasad później dla *policyDefB* z podobny do celów *policyDefA*, można dodać go w obszarze *initiativeDefC* i śledzenie ich lepsze w ten sposób.

   Należy pamiętać, że po utworzeniu inicjatywy przypisania z inicjatywy definicji żadnych nowych definicji zasad automatycznie dodawane do definicji inicjatywy zbiorczo w obszarze inicjatywy przypisań w obszarze tej inicjatywy definicji. Jednak w przypadku nowego parametru wprowadzone do nowej definicji zasad, musisz zaktualizować inicjatywy definicji i przydziały, edytując definicję inicjatywy lub przypisania.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy masz Przegląd zasad usługi Azure, a niektóre kluczowe pojęcia, które związku Oto Sugerowane następne kroki:

- [Przypisz definicji zasad](./assign-policy-definition.md)
- [Przypisz definicję zasad przy użyciu wiersza polecenia platformy Azure](./assign-policy-definition-cli.md)
- [Przypisz definicję zasad przy użyciu programu PowerShell](./assign-policy-definition-ps.md)
