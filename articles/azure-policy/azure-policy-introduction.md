---
title: "Przegląd zasad Azure | Dokumentacja firmy Microsoft"
description: "Zasady usługi Azure to usługa na platformie Azure, którego używasz do tworzenia, przypisywania i zarządzanie definicje zasad w środowisku platformy Azure."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark; nini
ms.date: 10/06/2017
ms.topic: overview
ms.service: azure-policy
manager: jochan
ms.custom: mvc
ms.openlocfilehash: 01b0915cdf37ddc00a4e6a38c99ce7f6f8c4f9c9
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-policy"></a>Co to jest Azure zasady?

Zarządzanie IT tworzy jasności między cele biznesowe i projektach. Dobrym informatyczną obejmuje Planowanie z inicjatywy i ustawianie priorytetów na poziomie strategicznych. Jeśli firma napotyka znaczących problemów IT, które nie wydają się pobrać rozwiązany, wdrażanie zasad pomaga lepiej zarządzać i uniemożliwić ich. Jest to, gdzie zasadami Azure jest dostarczany.

Zasady usługi Azure to usługa na platformie Azure, używanej do tworzenia, przypisywania i definicje zasad zarządzania. Definicje zasad wymusza różnych zasad i akcje za pośrednictwem zasobów, dlatego te zasoby pozostają zgodne ze standardami firmy, a umów dotyczących poziomu usług. Dzieje się tak, uruchamiając ocenę zasobów, aby przeprowadzić skanowanie których te nie są zgodne z definicjami zasad, które zostały spełnione.

## <a name="policy-definition"></a>Definicja zasad

Każdy definicji zasad ma warunków jest wymuszone i towarzyszącej akcji, która przyjmuje Umieść, jeśli warunki są spełnione.

W zasadach Azure oferujemy niektórych wbudowanych zasad, które są domyślnie dostępne. Na przykład:

- **Wymaga programu SQL Server 12.0**: definicja zasad ma warunki/reguły, aby upewnić się, że wszystkie serwery SQL, użyj wersji 12.0. Jego działania jest odrzucanie wszystkich serwerów, które nie spełniają te kryteria.
- **Dozwolone jednostki SKU konta magazynu**: definicja zasad ma zestaw warunków/reguł, które określa, czy konto magazynu, który jest wdrażany jest w zestawie, rozmiar jednostki SKU. Jego działania jest odrzucanie wszystkich serwerów, które nie są zgodne do zestawu zdefiniowanego rozmiary jednostki SKU.
- **Dozwolony typ zasobu**: definicja zasad ma zestaw warunków/reguł, aby określić typy zasobów, które można wdrożyć w organizacji. Jego działania jest odrzucanie wszystkie zasoby, które nie są częścią tego zdefiniowanej listy.

Aby dowiedzieć się więcej na temat struktury definicje zasad, Szukaj w tym artykule - [struktura definicji zasad](../azure-resource-manager/resource-manager-policy.md#policy-definition-structure).

## <a name="policy-assignment"></a>Przypisania zasad
Przypisania zasad jest sformułowanie zasad przypisany została wykonana w określonym zakresie. Ten zakres może należeć do zakresu od grupy zarządzania do grupy zasobów.

Aby uzyskać więcej informacji na definicji ustawień zasad i przypisań, zobacz [Przegląd zasad zasobów](../azure-resource-manager/resource-manager-policy.md).

## <a name="policy-parameters"></a>Parametry zasad
Parametry zasad ułatwić zarządzanie zasadami dzięki zmniejszeniu liczby definicje zasad, które należy utworzyć. Podczas tworzenia definicji zasad, aby był bardziej ogólnym, można zdefiniować parametrów. Następnie można ponownie użyć tej definicji zasad dla różnych scenariuszy przez przekazywanie różne wartości (na przykład określenie jeden zestaw lokalizacji dla subskrypcji) podczas przypisywania zasad.

Parametry są zdefiniowane/tworzone podczas tworzenia definicji zasad. Jeśli parametr jest zdefiniowana, otrzymuje nazwę i opcjonalnie danej wartości. Na przykład można zdefiniować parametr dla zasad jako lokalizacji i następnie nadaj różne wartości takich jak *EastUS* lub *WestUS* podczas przypisywania zasad.

Aby uzyskać więcej informacji na temat parametrów zasad, zobacz [Przegląd zasad zasobów — parametry](../azure-resource-manager/resource-manager-policy.md#parameters).

## <a name="initiative-definition"></a>Inicjatywy definicji
Definicja inicjatywy jest kolekcja definicji zasad, które są dostosowane do osiągnięcia pojedynczej celem nadrzędna. Na przykład można utworzyć inicjatywą zatytułowany **Włączanie monitorowania w Centrum zabezpieczeń Azure**, mając na celu monitorować wszystkie dostępne zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure.

W obszarze tej inicjatywy trzeba definicje zasad takich jak:

1. **Monitor bez szyfrowania bazy danych SQL w Centrum zabezpieczeń** — w przypadku monitorowania niezaszyfrowane baz danych i serwerów.
2. **Monitorowanie luk w zabezpieczeniach systemu operacyjnego w Centrum zabezpieczeń** — monitorowania serwerów, które nie spełniają skonfigurowanych linii bazowej.
3. **Monitorować brakujący program Endpoint Protection w Centrum zabezpieczeń** — monitorowania serwerów bez agenta ochrony zainstalowanego punktu końcowego.

## <a name="initiative-assignment"></a>Inicjatywy przypisania
Podobnie jak przypisanie zasad inicjatywy przydziału jest inicjatywy definicji przypisane do określonego zakresu. Przydziały inicjatywy zmniejszenia konieczności przeprowadzania kilku inicjatywy definicje dla każdego zakresu. Ten zakres można również należeć do zakresu od grupy zarządzania do grupy zasobów.

W powyższym przykładzie **Włączanie monitorowania w Centrum zabezpieczeń Azure** inicjatywy mogą być przypisane do różnych zakresów. Na przykład można przypisać do jednego przypisania **subscriptionA**, podczas gdy inny można przypisać do **subscriptionB**.

## <a name="initiative-parameters"></a>Parametry inicjatywy
Podobnie jak parametry zasad inicjatywy parametry uproszczenia inicjatywy zarządzania zmniejszając nadmiarowości. Parametry inicjatywy są zasadniczo listę parametrów używanych przez definicje zasad w ramach tej inicjatywy.

Na przykład wykonać scenariusz, w którym masz inicjatywy definicji - **initiativeC**, z dwie definicje zasad. Każda definicja zasad o jeden parametr zdefiniowane:

|Zasady  |Nazwa parametru     |Typ parametru  |Uwaga                                                                                                |
|--------|----------------------|-------|----------------------------------------------------------------------------------------------------------------|
|policyA |allowedLocations      |Tablica  |Ten parametr oczekuje listę ciągów dla wartości, ponieważ typ parametru została zdefiniowana jako tablica |
|policyB |allowedSingleLocation |Ciąg |Ten parametr oczekuje wartości o jedno słowo, ponieważ typ parametru została zdefiniowana jako ciąg          |

W tym scenariuszu podczas definiowania inicjatywy parametry **initiativeC**, dostępne są trzy opcje:

1. Korzystać z parametrów w ramach tej inicjatywy definicje zasad: W tym przykładzie *allowedLocations* i *allowedSingleLocation* stają się inicjatywy parametrów  **initiativeC**.
2. Podaj wartości parametrów definicje zasad w ramach tej inicjatywy definicji. W tym przykładzie można udostępnić listę lokalizacje **policyA przez parametr — allowedLocations** i **policyB przez parametr — allowedSingleLocation**.
Można również podać wartości podczas przypisywania tej inicjatywy.
3. Podaj listę *wartość* opcje, które mogą być używane podczas przypisywania tej inicjatywy. Oznacza to, że po przypisaniu tej inicjatywy parametry dziedziczone z definicji zasad w tej inicjatywy może mieć tylko wartości z tej listy podana.

Na przykład, jeśli podana listy wartości opcji podczas tworzenia definicji inicjatywy, ma — *EastUS*, *WestUS*, *CentralUS*, i *WestEurope* , użytkownik będzie mógł wejściowych innej wartości, takich jak *Azja południowo-wschodnia* inicjatywy przypisania, ponieważ nie jest częścią listy.

## <a name="recommendations-for-managing-policies"></a>Zalecenia dotyczące zarządzania zasadami

Podczas tworzenia i zarządzania definicje zasad i przypisań, poniżej przedstawiono kilka wskazówek, zaleca się wykonanie:

- W przypadku tworzenia zasad definicje w danym środowisku, firma Microsoft zaleca, począwszy od Śledź efekt, w przeciwieństwie do efekt Odmów, aby zachować inspekcji wpływ definicję zasad w środowisku. Jeśli masz skrypty już w miejscu, aby stosować automatyczne skalowanie zapasowych aplikacji, ustawienie efektu Odmów może utrudniać tych zadań automatyzacji, które już zostały spełnione.
- Należy pamiętać o hierarchii organizacyjnej podczas tworzenia definicji i przypisania. Zaleca się tworzenie definicji na poziomie subskrypcji lub wyższego poziomu, na przykład w grupie zarządzania i przypisywanie na poziomie podrzędnym dalej. Na przykład w przypadku utworzenia definicji zasad na poziomie grupy zarządzania, przypisanie zasad tej definicji można zakresu do poziomu subskrypcji.
- Firma Microsoft zachęca przy użyciu standardu cenowym, aby lepiej zrozumieć stan zgodności danego środowiska.
- Firma Microsoft zaleca zawsze inicjatywy definicje zamiast definicje zasad, nawet jeśli masz tylko jedną zasadę pamiętać. Na przykład, jeśli masz definicję zasad — *policyDefA* i utwórz go w definicji inicjatywy - *initiativeDefC*, jeśli użytkownik chce utworzyć inną definicję zasad z celów, takich jak te *policyDefA*, po prostu można dodać go w obszarze *initiativeDefC* i śledzenie ich lepsze w ten sposób.

   Należy pamiętać, że po utworzeniu inicjatywy przypisania z inicjatywy definicji żadnych nowych definicji zasad dodane do definicji inicjatywy zostanie automatycznie zbiorczo w obszarze inicjatywy przypisań w obszarze tej inicjatywy definicji. Jednak w przypadku nowego parametru wprowadzone do nowej definicji zasad, musisz zaktualizować inicjatywy definicji i przypisania, aby odzwierciedlić, edytując definicję lub przypisania.

## <a name="next-steps"></a>Następne kroki:
Teraz, gdy masz Przegląd zasad usługi Azure, a niektóre kluczowe pojęcia, które związku Oto Sugerowane następne kroki:

- [Przypisz definicji zasad](./assign-policy-definition.md)
- [Przypisz definicję zasad przy użyciu wiersza polecenia platformy Azure](./assign-policy-definition-cli.md)
- [Przypisz definicję zasad przy użyciu programu PowerShell](./assign-policy-definition-ps.md)
