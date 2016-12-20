---
title: "Bezpieczeństwo danych w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite | Microsoft Docs"
description: "Ten dokument przedstawia sposób zarządzania danymi i ich ochrony w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 9cdf7deb-2a30-4672-b89f-71179ee8326a
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 5001cd47b6ee51967d1286414ccefedd8e7e7813
ms.openlocfilehash: 6af6c10cef317453131197db74a9380518afadf0


---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Bezpieczeństwo danych w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite
Aby ułatwić klientom zapobieganie zagrożeniom, wykrywanie ich i reagowanie na nie, [rozwiązanie Zabezpieczenia i inspekcja w pakiecie Operations Management Suite (OMS)](operations-management-suite-overview.md) zbiera i przetwarza dane dotyczące zasobów, takie jak:

* Dziennik zdarzeń zabezpieczeń
* Zdarzenia funkcji Śledzenie zdarzeń systemu Windows (ETW)
* Zdarzenia inspekcji funkcji AppLocker
* Dziennik zapory systemu Windows
* Zdarzenia rozwiązania Advanced Threat Analytics
* Wyniki oceny linii bazowej
* Wyniki oceny oprogramowania chroniącego przed złośliwym kodem
* Wyniki oceny aktualizacji/poprawek
* Strumienie dzienników systemowych jawnie włączone dla agenta

Firma Microsoft zobowiązuje się chronić poufność i bezpieczeństwo tych danych. Firma Microsoft przestrzega surowych wymogów z zakresu zabezpieczeń i zgodności — od kodu po działanie usługi.
Ten artykuł przedstawia sposób zarządzania danymi i ich ochrony w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie OMS.

## <a name="data-sources"></a>Źródła danych
Rozwiązanie Zabezpieczenia i inspekcja w pakiecie OMS analizuje dane z maszyn wirtualnych i fizycznych komputerów, na których jest zainstalowany Agent pakietu OMS. Rozwiązanie Zabezpieczenia i inspekcja w pakiecie OMS może zbierać informacje o konfiguracji dotyczące zdarzeń zabezpieczeń, takich jak zdarzenia systemu Windows, dzienniki inspekcji, dzienniki usług IIS i komunikaty dziennika systemowego. Przykłady takich danych to: typ i wersja systemu operacyjnego, uruchomione procesy, nazwa maszyny, adresy IP, zalogowany użytkownik i identyfikator dzierżawy.  

## <a name="data-protection"></a>Ochrona danych
**Podział danych**: dane są logicznie oddzielone dla każdego składnika w całej usłudze. Wszystkie dane są otagowane informacjami o organizacji. To tagowanie jest obecne przez cały cykl życia danych i jest wymuszane w każdej warstwie usługi. 

**Dostęp do danych**: aby móc przedstawić zalecenia w zakresie zabezpieczeń i zbadać potencjalne zagrożenia dla bezpieczeństwa, personel firmy Microsoft może uzyskać dostęp do informacji zbieranych lub analizowanych przez usługi. Stosujemy się do [warunków korzystania z usług Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) i [zasad zachowania poufności informacji](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), które stanowią, że firma Microsoft nie będzie używać danych klienta ani informacji pochodnych do żadnych celów reklamowych ani zbliżonych celów komercyjnych. Aby móc przedstawić zalecenia w zakresie zabezpieczeń i zbadać potencjalne zagrożenia dla bezpieczeństwa, personel firmy Microsoft może uzyskać dostęp do informacji zbieranych lub analizowanych przez usługi. Danych klienta używamy tylko w razie potrzeby w celu świadczenia usług platformy Azure, włączając w to cele zgodne ze świadczeniem tych usług. Klient zachowuje wszystkie prawa do swoich danych.

**Użycie danych**: firma Microsoft używa wzorców i analizy zagrożeń obecnych w wielu dzierżawach, aby zwiększyć możliwości wykrywania zagrożeń i zapobiegania im — robimy to zgodnie ze zobowiązaniami co do prywatności opisanymi w [zasadach zachowania poufności informacji](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [!NOTE]
> Lokalizacja danych jest konfigurowana na poziomie obszaru roboczego pakietu OMS, podczas tworzenia obszaru roboczego. Proces ten jest częścią początkowej konfiguracji rozwiązania Zabezpieczenia i inspekcja w pakiecie OMS.
> 
> 

## <a name="see-also"></a>Zobacz też
W tym dokumencie przedstawiono sposób zarządzania danymi i ich ochrony w pakiecie OMS. Więcej informacji na temat rozwiązania Zabezpieczenia i inspekcja w pakiecie OMS zawierają artykuły:

* [Omówienie pakietu Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorowanie alertów zabezpieczeń i reagowanie na nie w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-responding-alerts.md)
* [Monitorowanie zasobów w rozwiązaniu Zabezpieczenia i inspekcja w pakiecie Operations Management Suite](oms-security-monitoring-resources.md)




<!--HONumber=Dec16_HO1-->


