---
title: Korzystam z usługi Mobile Services. Jak usługa App Service może mi pomóc?
description: Dowiedz się, jakie korzyści z usługi App Service można osiągać podczas pracy z istniejącymi projektami usługi Mobile Services.
services: app-service\mobile
documentationcenter: ios
author: adrianhall
manager: dwrede
editor: ''

ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/01/2016
ms.author: adrianha

---
# <a name="getting-started"> </a>Korzystam z usługi Mobile Services. Jak usługa App Service może mi pomóc?
## Omówienie
Istniejąca Usługa mobilna jest bezpieczna i pozostanie objęta pomocą techniczną. Platforma *usługi Azure App Service* oferuje jednak pewne korzyści związane z aplikacją mobilną, które nie są obecnie dostępne w usłudze Mobile Services:

* Prostsze, łatwiejsze i bardziej ekonomiczne oferty aplikacji obejmujących klientów sieci Web i mobilnych
* Nowe funkcje hosta, w tym zadania Web Job, niestandardowe nazwy CName, skuteczniejsze monitorowanie
* Gotowa do użycia funkcja integracji z usługą Traffic Manager
* Łączność z lokalnymi zasobami i sieciami VPN przy użyciu nie tylko połączeń hybrydowych, ale i sieci wirtualnej
* Monitorowanie, alerty i rozwiązywanie problemów dotyczących aplikacji przy użyciu usługi NewRelic lub AppInsights
* Bogatsze spektrum podstawowych zasobów obliczeniowych i cen
* Wbudowane automatyczne skalowanie, równoważenie obciążenia i monitorowanie wydajności
* Wbudowane możliwości wdrażania przejściowego, tworzenia kopii zapasowych, wycofywania i testowania w produkcji

## Nowe funkcje hostingu
W *usłudze Azure App Service* kod zaplecza *plikacji mobilnej* działa w tym samym kontenerze co aplikacja sieci Web i aplikacja interfejsu API. W związku z tym możesz korzystać ze wszystkich funkcji w tym kontenerze, w tym z niektórych funkcji, które nie są aktualnie dostępne w usłudze Mobile Services:

* Dodawanie działającej w sposób ciągły logiki zaplecza za pośrednictwem zadań Web Job.
* Sprawdzanie, czy kod zaplecza zawsze działa.
* Nadawanie punktom końcowym zaplecza mobilnego przyjaznych i stabilnych nazw dzięki niestandardowym nazwom CName.
* Geograficzne skalowanie aplikacji przy użyciu usługi Traffic Manager.
* Uwzględnianie wszystkich wybranych bibliotek i pakietów.
* (.NET) Korzystanie z dowolnej funkcji programu ASP.NET, w tym MVC.
* (Node.js) Korzystanie z dowolnej czystej biblioteki języka JavaScript ekosystemu języka Node, w tym wspólnych bibliotek MVC.

## Dostęp do danych lokalnych przy użyciu sieci wirtualnej
Obecnie w usłudze Mobile Services można już uzyskiwać dostęp do zasobów lokalnych za pomocą połączeń hybrydowych. W niektórych sytuacjach preferowane jest jednak użycie rozwiązania VPN. *Usługa Azure App Service* umożliwia korzystanie z sieci wirtualnej platformy Azure na potrzeby kodu zaplecza aplikacji mobilnej.

## Korzystanie z ulubionego języka zaplecza
*Usługa Azure App Service* oferuje bardziej rozbudowaną i zaawansowaną obsługę platform ASP.NET i Node.js, w tym dostęp do najnowszych środowisk uruchomieniowych.

## Ustawianie skalowania automatycznego
W przypadku usługi Mobile Services wszystkie wystąpienia kodu zaplecza były uruchamiane na małych maszynach wirtualnych. *Usługa Azure App Service* umożliwia wybranie rozmiaru maszyn wirtualnych ze znacznie większego zestawu opcji. Możesz szybko skalować w górę lub w poziomie, co pozwala na obsługę dowolnych przychodzących obciążeń klientów w oparciu o różne metryki wydajności.

## Trzymaj rękę na pulsie
Reaguj na problemy w czasie rzeczywistym dzięki możliwościom automatycznego powiadamiania Ciebie i Twojego zespołu za pomocą funkcji monitorowania i alertów. Integruj zaawansowane funkcje monitorowania i analizy aplikacji z usług New Relic i AppInsights w celu uzyskania bardziej szczegółowego wglądu w sposób działania aplikacji mobilnej. W *usłudze Azure App Service* możesz teraz konfigurować alerty oparte na różnych metrykach wydajności — programowo i za pośrednictwem witryny Azure Portal.

## Zabezpieczanie zasobów
Automatycznie twórz kopię zapasową zaplecza i bazy danych. Kod i dane są chronione przed awariami. Można je też łatwo przywrócić, aby zapewnić firmie pewność działania.

## Gotowe, przejściowe, już!
Dzięki *usłudze Azure App Service* można teraz tworzyć wiele prywatnych środowisk testowania i przejściowych dla aplikacji mobilnych. Używaj ich do testowania aplikacji przed wdrożeniem. Przechodź do środowiska produkcyjnego bez przestojów. Aplikacje sieci Web są wstępnie załadowane, dzięki czemu można pracować w najlepszym środowisku klienta.

Aby rozpocząć korzystanie z zalet *usługi App Service* dla istniejącej Usługi mobilnej, wykonaj czynności opisane w tym [samouczku](app-service-mobile-migrating-from-mobile-services.md).

<!--HONumber=Oct16_HO1-->


