---
title: "Omówienie standardowe ochrony przed atakami DDoS Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o usłudze Azure ochrona przed atakami DDoS."
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: kumud
ms.openlocfilehash: 76da0d4e805c732d40a7bd02e5c70973c792e26c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="azure-ddos-protection-standard-overview"></a>Omówienie usługi Azure Standard ochrony przed atakami DDoS

>[!IMPORTANT]
>Standardowa ochrony przed atakami DDoS Azure jest obecnie w wersji zapoznawczej. Ograniczoną liczbą standardowe ochrony przed atakami DDoS obsługi zasobów platformy Azure i wybierz wiele regionów. Musisz [zarejestrować usługi](http://aka.ms/ddosprotection) ograniczone wersji zapoznawczej można pobrać włączona dla Twojej subskrypcji przed atakami DDoS ochrony standardowa. Kontaktować się ze przez zespół Azure DDoS rejestracji do przeprowadzenia procesu aktywacji. Standardowa ochrony przed atakami DDoS jest dostępna w regionach nam wschodnie, zachodnie nam i zachodnie środkowe Stany. Podczas udostępniania wersji zapoznawczej są nie naliczane opłaty za korzystanie z usługi.

Rozproszone ataków przeprowadzenie ataku typu "odmowa usługi" (DDoS) są jednym z największych dostępności i zabezpieczeń dotyczy klientów połączonej przenoszenie swoje aplikacje w chmurze. Próbuje takiego ataku na wyczerpanie zasobów aplikacji, co aplikacja niedostępne do autoryzowanych użytkowników. Może być celem ataków DDoS dowolnego punktu końcowego, który jest publicznie dostępny za pośrednictwem Internetu.

Azure ochrona przed atakami DDoS połączone ze sobą, z najlepszymi rozwiązaniami projektowania aplikacji zapewniają ochronę przed atakami te. Dostępne są te dwie usługi warstwy: 

- **Podstawowa ochrona przed atakami DDoS Azure** — jest automatycznie włączone w ramach platformy Azure bez dodatkowych opłat. Zawsze włączone ruchu monitorowania i w czasie rzeczywistym zmniejszenie typowych atak z poziomu sieci zapewnia tego samego zabezpieczenia, wykorzystywane przez usługi online firmy Microsoft.  Cały skali globalnej sieci platformy Azure może służyć do rozpowszechniania i ograniczyć ruch ataku w regionach. 
- **Standardowa ochrony przed atakami DDoS Azure** — zapewnia dodatkowe ograniczenie możliwości dostosowana specjalnie do zasobów sieci wirtualnej. Jest prosta włączyć i nie wymaga żadnych zmian w aplikacji. Zasady ochrony są dopasowane za pomocą dedykowanego ruchu monitorowania i komputer, algorytmów uczenia i zastosowane do publicznych adresów IP skojarzonych z zasobami sieci wirtualnej, takie jak wystąpień usługi równoważenia obciążenia, bramę aplikacji i sieci szkieletowej usług.  Dane telemetryczne w czasie rzeczywistym jest dostępna za pośrednictwem widoków Azure Monitor, podczas ataku i historii. Ochronę warstwy aplikacji można dodać za pomocą [zapory aplikacji sieci Web bramy aplikacji](https://azure.microsoft.com/services/application-gateway/). 

![Standard ochrony przed atakami DDoS Azure](./media/ddos-protection-overview/ddos-protection-overview-fig2.png)

Zachęcamy do wypróbowania DDoS ochrony standardowe w środowiskach rozwoju, testów lub produkcji. Aby przesłać opinię dotyczącą komfort pracy, należy korzystać z następujących zasobów:
- [Ochrona przed atakami DDoS Azure na Forum usługi Microsoft Azure](https://feedback.azure.com/forums/905032-azure-ddos-protection). 
- [Ochrona przed atakami DDoS Azure na MSDN Forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azureddosprotection)
- [Ochrona przed atakami DDos Azure w przepełnienie stosu](https://stackoverflow.com/tags/azure-ddos/info)

W przypadku problemów dotyczących obsługi, możesz [Otwórz bilet pomocy technicznej platformy Azure](../azure-supportability/how-to-create-azure-support-request.md). Podczas ochrony przed atakami DDoS — standardowe jest w wersji zapoznawczej, jest obsługiwane w sposób optymalny.

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>Typy ataków DDoS zmniejsza standardowe ochrony przed atakami DDoS

Ochrona przed atakami DDoS — standardowe ograniczania ataki tego rodzaju:

- **Ataki pomiarowej** -celem ataku jest wypełniania warstwy sieci o rozległe pozornie potrzebnego ruchu. Zawiera powodzie, powodzie wzmocnienia i innych powodzie sfałszowane pakietów UDP. Standardowa ochrony przed atakami DDoS zmniejsza tych potencjalnych ataków wielu gigabajtów przez pochłaniające & czyszczenia je automatycznie wykorzystaniu skali globalnej sieci platformy Azure. 
- **Protokół ataków** -ataki renderować element docelowy niedostępny dzięki wykorzystaniu osłabienie stosu protokołu warstwy 3 i 4 warstwy. Zawiera on, SYN powódź ataków, ataki odbicia, a inne ataki protokołu. Standardowa ochrony przed atakami DDoS zmniejsza zagrożenie tego rodzaju ataki rozróżnianie między ruchu złośliwego i uzasadnionych interakcji z klienta i blokowania złośliwego ruchu. 
- **Ataki w warstwie aplikacji** -ataki target pakiety aplikacji sieci web do zakłócać przekazywania danych między hostami. Obejmuje on naruszenia protokołu HTTP, SQL iniekcji, skryptów krzyżowych i inne ataki warstwy 7. Aby zapewnić ochronę przed atakami opartymi na tych przy użyciu bramy aplikacji zapory aplikacji sieci Web przy użyciu ochrony przed atakami DDoS — Standard. 

Ochrona przed atakami DDoS — standardowe chroni zasobów w sieci wirtualnej, włączając publiczne adresy IP skojarzone z maszynami wirtualnymi, wewnętrzne moduły równoważenia obciążenia i bramy aplikacji. W połączeniu z jednostka SKU aplikacji bramy zapory aplikacji sieci Web, standardowe ochrony przed atakami DDoS zapewniają pełną L3 do P7 ograniczenie możliwości.

## <a name="ddos-protection-standard-features"></a>Funkcje ochrony przed atakami DDoS — standardowe

![Funkcje DDoS](./media/ddos-protection-overview/ddos-overview-fig1.png)

Ochrona przed atakami DDoS — standardowe funkcje: 

- **Integracja platformy Native:** standardowe ochrony przed atakami DDoS natywnej integracji na platformie Azure i zawiera konfiguracji za pomocą portalu Azure i programu PowerShell. Standardowa ochrony przed atakami DDoS rozumie, wszystkie zasoby i konfiguracji zasobu.
- **Monitorowanie ruchu zawsze włączone:** Twoich wzorców ruchu aplikacji są monitorowane 24 x 7, wyszukiwanie wskaźników atakami DDoS. Środki zaradcze odbywa się po przekroczeniu zasad ochrony.
- **Ochrona gotowe:** konfiguracji uproszczony natychmiast chroni wszystkie zasoby w sieci wirtualnej, jak standardowe ochrony przed atakami DDoS jest włączona. Brak definicji interwencji lub użytkownika jest wymagany — standardowe ochrony przed atakami DDoS natychmiast i automatycznie zmniejsza ataku po została wykryta.
- **Dostrajanie adaptacyjną:** profilowania inteligentnego ruchu uzyskuje informacje o ruchu aplikacji wraz z upływem czasu i wybiera i aktualizuje profil, który jest najbardziej odpowiednie dla Twojej usługi. Profil można dostosować zgodnie z ruchem zmienia się wraz z upływem czasu.
- **L3 P7 ochrony z bramy aplikacji:** udostępnienie ochrona przed atakami DDoS pełnego stosu funkcji aplikacji bramy zapory aplikacji sieci Web.
- **Ograniczenie dużej skali:** globalne pojemności do ochrony przez największy znane atakami DDoS można zminimalizować przez 60 ataku różnych typów. 
- **Ataki metryki:** Summarized metryki na każdym ataki są dostępne za pośrednictwem Monitora Azure.
- **Alerty ataku:** alertów można skonfigurować na rozpoczęcie i zakończenie ataku i za pośrednictwem ataku na czas trwania przy użyciu wbudowanych ataku metryki. Alerty zintegrować oprogramowanie operacyjne jak OMS, Splunk usługi Azure Storage, poczty E-mail i portalu Azure.
- **Koszt gwarancji:** transferu danych i aplikacji skalowalnych w poziomie zniżki w ramach ataków DDoS udokumentowane.

## <a name="ddos-protection-standard-mitigation"></a>Środki zaradcze standardowe ochrony przed atakami DDoS

Usługa ochrony przed atakami DDoS firmy Microsoft monitoruje ruch rzeczywiste użycie i stale porównuje ją z progów zdefiniowane w zasadzie DDoS. Po przekroczeniu progu ruchu DDoS ograniczenie jest inicjowany automatycznie. Gdy ruch zwraca poniżej progu, ograniczenie zostanie usunięty.

Podczas środki zaradcze ruch do chronionego zasobu jest przekierowywany przez usługę ochrona przed atakami DDoS i kilka testy są wykonywane. Kontrole zwykle wykonać następującą funkcję:

- Upewnij się, pakiety są zgodne z specyfikacji internetowych i nie są źle sformułowane.
- Interakcje z klienta w celu określenia, czy jest potencjalnie fałszywych pakietów (np.: SYN uwierzytelniania lub plik Cookie SYN lub poprzez upuszczenie pakietów dla źródła ponownego przesłania go).
- Limit szybkości pakietów, jeśli można wykonać żadnej innej metody wymuszania.

Ochrona przed atakami DDoS bloki ataku ruchu i do przodu pozostały ruch do danego przeznaczenia. W ciągu kilku minut wykrywanie ataków użytkownik jest powiadamiany, przy użyciu metryk Azure Monitor. Konfigurowanie rejestrowania na standardowe ochrony przed atakami DDoS telemetrii, może zapisać dzienniki dostępne opcje dla przyszłych analizy. Dane w monitorze Azure dla ochrony przed atakami DDoS — Standard obecnie są przechowywane przez 30 dni.

Klientów, aby symulować własnych ataków DDoS nie jest zalecane. Zamiast tego, klienci mogą użyć kanału pomocy technicznej aby żądania DDoS ataki symulacji wykonywane przez sieć platformy Azure. Inżynier skontaktuje się z rozmieszczania szczegóły takiego ataku (portów, protokołów, docelowych adresów IP) i Rozmieść czas, aby zaplanować uruchomienie testu.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o zarządzaniu przy użyciu standardowych ochrony przed atakami DDoS [programu Azure PowerShell](ddos-protection-manage-ps.md) lub [portalu Azure](ddos-protection-manage-portal.md).
