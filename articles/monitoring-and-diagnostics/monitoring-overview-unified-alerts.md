---
title: "Poznaj nowe środowisko alerty (wersja zapoznawcza) w monitorze Azure | Dokumentacja firmy Microsoft"
description: "Zrozumienie, jak nowe alerty proste i skalowalne doświadczenie w Azure ułatwia tworzenia, przeglądania i Zarządzanie alertami łatwiejsze"
author: manishsm-msft
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: mamit
ms.custom: 
ms.openlocfilehash: 316dcd53509897a6efc387749ca6f9ec268cb7ac
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="explore-the-new-alerts-preview-experience-in-azure-monitor"></a>Poznaj nowe środowisko alerty (wersja zapoznawcza) w monitorze Azure

## <a name="overview"></a>Przegląd
 Alerty środowisko na platformie Azure ma świeża wygląd i zaktualizowanych funkcji. Nowe środowisko jest dostępna z **alertów (wersja zapoznawcza)** kartę w obszarze monitorowania Azure. Poniżej przedstawiono niektóre zalety korzystania z nowego środowiska alerty (wersja zapoznawcza):

 - **Rozdzielenie Fired alertów i reguły alertu** — środowisko Alerts(Preview) alertów reguły (definicja warunku wyzwalania alertu) i są zróżnicowane generowane alerty (wystąpienia uruchamiania reguły alertu), więc operacyjne i Widoki konfiguracji są rozdzielone.
 - **A unified środowisko tworzenia metryki i dziennika alertów** - nowe alerty (wersja zapoznawcza), tworzenie przewodniki środowisko użytkownika wzdłuż przez proces konfigurowania reguły alertu, dzięki czemu łatwiej odnajdywać prawo czynności, aby otrzymywać alerty w.
 - **Widok generowane alerty Log Analytics w portalu Azure** -doświadczenia w alerty (wersja zapoznawcza), można teraz również Zobacz generowane alerty analizy dzienników w ramach subskrypcji.  
 - **Ujednolicone tworzenia alertów dziennika aktywności** — można teraz tworzyć alerty dziennika aktywności bezpośrednio z **Monitor** > **alertów (wersja zapoznawcza)**. Wcześniej można je utworzyć tylko za pomocą **Monitor** > **dziennik aktywności**.

W poniższych sekcjach opisano, bardziej szczegółowo, jak działa nowego środowiska.

## <a name="taxonomy"></a>Taksonomii
Środowisko Alerts(preview) użyje następujących koncepcji oddzielającego obiekty reguły alertów i Alert wywoływane podczas jednorodnej obsługi tworzenia we wszystkich różnych typów alertów.

- **Zasób docelowy** — element docelowy może być dowolnym zasobem Azure. Zasób docelowy definiuje zakres i sygnałów, które są dostępne dla alertów. Przykładowe elementy docelowe: maszyny wirtualne, konta magazynu, zestaw skali maszyny wirtualnej, obszar roboczy analizy dzienników lub rozwiązanie.

- **Kryteria** — kryteria jest kombinacją sygnału i logiki na zasób docelowy. Przykłady: Procent procesora CPU > 70%, czas odpowiedzi serwera > 4 ms, liczba wyników dziennika zapytania > 100 itp. 

- **Sygnał** — sygnały są emitowane przez zasób docelowy i mogą być różne. Ta wersja zapoznawcza obsługuje **Metryka**, **dziennik aktywności**, **usługi Application Insights** i **dziennika** jako typy sygnału.

- **Logika** -zdefiniowane przez użytkownika logikę, sprawdź, czy w ramach sygnał Oczekiwano wartości/zakresu.  
 
- **Akcja** -wywołań wysyłane do odbiorcy powiadomienia (na przykład wysyłanie wiadomości e-mail adres lub publikowanie do adresu URL elementu webhook). Powiadomienia zwykle można wyzwolić wiele akcji. Typy alertów, obsługiwane w tej wersji zapoznawczej, grupy akcji pomocy technicznej.  
 
- **Reguła alertu** — definicja warunku, który może powodować Wyzwalanie alertów. W tej wersji zapoznawczej reguły alertu przechwytuje docelowy i kryteria alertów. Reguła alertu może być stan wyłączone lub włączone.
 
- **Wywoływane Alert** — utworzone po włączone reguły alertu. Wypalane obiekt alertów może być w stanie nierozwiązanych lub Fired.

    > [!NOTE]
    > Różni się to od bieżącego środowiska alerty, gdy alert reprezentuje reguły i wypalane alert i dlatego może być w jednym ze stanów ostrzeżenie, aktywna lub wyłączona.
    >

## <a name="single-place-to-view-and-manage-alerts"></a>Jednego miejsca, aby wyświetlić alerty i zarządzaj nimi
Celem środowisko alerty (wersja zapoznawcza) jest za jednego miejsca do wyświetlania i zarządzania wszystkie alerty platformy Azure. Poniższe podpunkty opisują funkcje każdego pojedynczego ekranu nowego środowiska.

### <a name="alerts-preview-overview-page"></a>Strony Przegląd alertów (wersja zapoznawcza)
**Monitor — alerty (wersja zapoznawcza)** strony Przegląd pokazuje zagregowane podsumowanie wszystkich wypalane alertów i sumy skonfigurowane lub nie włączono reguł alertów. Zawiera on również lista wszystkich alertów wypalane. Zmiana subskrypcji lub parametrów filtru aktualizuje agregacji i alerty generowane listy.

> [!NOTE]
> Wypalane alertów przedstawianych w alertach (wersja zapoznawcza) są ograniczone do obsługiwanych metryki i dziennika alerty; Azure Monitor pokazuje liczbę alertów fire, włącznie z zawartymi w starszych alerty Azure

 ![alerts-preview-overview](./media/monitoring-overview-unified/alerts-preview-overview.png) 

### <a name="alert-rules-management"></a>Zarządzania regułami alertów
**Monitor — alerty (wersja zapoznawcza) > reguły** jest pojedynczej strony do zarządzania wszystkie reguły alertu subskrypcji platformy Azure. Wyświetla listę wszystkich reguł alertów (włączona lub wyłączona) i można sortować na podstawie zasobów docelowych, grupy zasobów, nazwa reguły lub stanu. Reguły alertów można również włączony/wyłączony lub edytować z poziomu tej strony.  

 ![alerty — wersja zapoznawcza — zasady](./media/monitoring-overview-unified/alerts-preview-rules.png)


## <a name="one-alert-authoring-experience-across-all-monitoring-sources"></a>Jeden alert tworzenia we wszystkich źródłach monitorowania
Obsługa w alerty (wersja zapoznawcza), alerty można tworzyć w sposób ciągły, niezależnie od usługi monitorowania lub sygnału typu. Wszystkie alerty generowane i powiązanych szczegółów są dostępne na jednej stronie.  
 
Tworzenia alertu jest trzech etapów zadania, gdy użytkownik najpierw wybiera element docelowy dla alertu, następuje zaznaczenie prawym sygnału, a następnie określając logiki mają być stosowane na sygnał jako część reguły alertów. Uproszczony proces tworzenia już wymaga od użytkownika znajomości monitorowania źródła lub sygnały obsługiwane przed wybraniem zasobów platformy Azure. Wspólne środowisko tworzenia automatycznie filtruje listę dostępnych sygnały oparte na wybrany zasób docelowy i przeprowadza tworzenia logiki alertu

Dowiedz się więcej na temat tworzenia następujące typy alertów [tutaj](monitor-alerts-unified-usage.md).
- Metryki alerty (nazywane alerty Metryka niemal czasie rzeczywistym w bieżącym środowisku)
- Alerty dzienników (Log Analytics)
- Alerty dziennika (Dzienniki aktywności)
- Alerty dziennika (usługi Application Insights)

 

## <a name="alert-types-supported-in-this-preview"></a>Typy alertów są obsługiwane w tej wersji zapoznawczej


| **Typ sygnału** | **Źródło monitora** | **Opis** | 
|-------------|----------------|-------------|
| Metryka | Monitor systemu Azure | Wywołuje się [ **alerty Metryka niemal czasie rzeczywistym** ](monitoring-near-real-time-metric-alerts.md) w bieżącym środowisku te metryki alerty obsługuje sprawdzanie warunków metryki nawet 1 min i umożliwiają wielu metryki reguły. Dostępna jest lista obsługiwane typy zasobów [tutaj](monitoring-near-real-time-metric-alerts.md#what-resources-can-i-create-near-real-time-metric-alerts-for). Metryka innych alertów, zgodnie z definicją [tutaj](monitoring-overview-alerts.md#alerts-in-different-azure-services) nie są obsługiwane w środowisku alerty (wersja zapoznawcza).|
| Dzienniki  | Log Analytics | Otrzymywać powiadomienia, lub Uruchom akcje automatyczne, gdy dziennik zapytania wyszukiwania danych metryki i/lub zdarzeń spełnia określone kryteria.|
| Dzienniki  | Dzienniki aktywności | Ta kategoria zawiera rekordy wszystkich tworzenia, aktualizacji i usunąć akcje wykonywane za pomocą wybranego celu (Grupa zasobów/zasobów/subskrypcji). |
| Dzienniki  | Dzienniki usługi kondycji | Nieobsługiwane w środowisku alerty (wersja zapoznawcza).   |
| Dzienniki  | Application Insights | Ta kategoria zawiera dzienniki o szczegółach wydajności aplikacji. Zapytanie analytics można zdefiniować warunki dla działań, które mają być wykonane — na podstawie danych aplikacji. |
| Metryka | Application Insights | Nieobsługiwane w środowisku alerty (wersja zapoznawcza). |
| Badania dostępności | Application Insights | Nieobsługiwane w środowisku alerty (wersja zapoznawcza). |


## <a name="next-steps"></a>Kolejne kroki
- [Dowiedz się, jak tworzyć, wyświetlać, alerty i zarządzaj nimi za pomocą nowego środowiska alerty (wersja zapoznawcza)](monitor-alerts-unified-usage.md)
- [Dowiedz się więcej o alertach dziennika w środowisku alerty (wersja zapoznawcza)](monitor-alerts-unified-log.md)
- [Dowiedz się więcej o alertach metryki środowisko alerty (wersja zapoznawcza)](monitoring-near-real-time-metric-alerts.md)
- [Dowiedz się więcej o alertach dziennika aktywności w środowisku alerty (wersja zapoznawcza)](monitoring-activity-log-alerts-new-experience.md)
