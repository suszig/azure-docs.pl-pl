---
title: "Azure Security Center — bezpieczeństwo danych | Microsoft Docs"
description: "W tym dokumencie wyjaśniono, jak zarządzane i chronione są dane w usłudze Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f20d77a43185a6b22a5da1ee1c2744707df13dae


---
# <a name="azure-security-center-data-security"></a>Azure Security Center — bezpieczeństwo danych
Aby ułatwić klientom zapobieganie zagrożeniom, wykrywanie ich i reagowanie na nie, usługa Azure Security Center zbiera i przetwarza dane dotyczące zasobów platformy Azure, w tym informacje o konfiguracji, metadane, dzienniki zdarzeń, pliki zrzutu awaryjnego i inne. Firma Microsoft zobowiązuje się chronić poufność i bezpieczeństwo tych danych. Firma Microsoft przestrzega surowych wymogów z zakresu zabezpieczeń i zgodności — od kodu po działanie usługi. 

W tym artykule wyjaśniono, jak zarządzane i chronione są dane w usłudze Azure Security Center.

## <a name="data-sources"></a>Źródła danych
Usługa Azure Security Center analizuje dane z następujących źródeł:

* Usługi platformy Azure: odczytuje informacje o konfiguracji wdrożonych usług platformy Azure, komunikując się z dostawcą zasobów tej usługi.
* Ruch sieciowy: odczytuje próbkowane metadane ruchu sieciowego z infrastruktury firmy Microsoft, takie jak źródłowy i docelowy adres IP, źródłowy i docelowy port, rozmiar pakietu i protokół sieciowy.
* Rozwiązania partnerów: zbiera alerty zabezpieczeń z rozwiązań zintegrowanych partnerów, takich jak zapory i rozwiązania do ochrony przed złośliwym oprogramowaniem. Dane te są przechowywane w magazynie usługi Azure Security Center, obecnie znajdującym się na terenie Stanów Zjednoczonych.
* Maszyny wirtualne: dzięki użyciu agentów zbierania danych usługa Azure Security Center może zbierać z maszyn wirtualnych informacje dotyczące konfiguracji oraz zdarzeń związanych z zabezpieczeniami, takich jak zdarzenia systemu Windows i dzienniki inspekcji, dzienniki programu IIS, komunikaty dziennika systemu i pliki zrzutu awaryjnego. Szczegółowe informacje znajdują się w umieszczonej poniżej sekcji „Zarządzanie zbieraniem danych”.  

Ponadto informacje dotyczące alertów zabezpieczeń, zalecenia oraz stan kondycji zabezpieczeń są przechowywane w magazynie usługi Azure Security Center, obecnie znajdującym się na terenie Stanów Zjednoczonych. Do informacji zebranych z maszyn wirtualnych mogą należeć informacje dotyczące konfiguracji oraz zdarzeń związanych z zabezpieczeniami, dzięki którym możliwe jest przekazanie użytkownikowi danych na temat alertu zabezpieczeń, ewentualnych zaleceń postępowania oraz stanu kondycji zabezpieczeń.

## <a name="data-protection"></a>Ochrona danych
**Podział danych**: dane są logicznie oddzielone dla każdego składnika w całej usłudze. Wszystkie dane są otagowane informacjami o organizacji. To tagowanie jest obecne przez cały cykl życia danych i jest wymuszane w każdej warstwie usługi. Ponadto dane zbierane z maszyn wirtualnych są przechowywane na koncie lub kontach magazynu.

**Dostęp do danych**: aby móc przedstawić zalecenia w zakresie zabezpieczeń i zbadać potencjalne zagrożenia dla bezpieczeństwa, personel firmy Microsoft może uzyskać dostęp do informacji zbieranych lub analizowanych przez usługi Azure, w tym do plików zrzutu awaryjnego. Dane klienta lub dane osobowe z maszyn wirtualnych mogą niezamierzenie znaleźć się w plikach zrzutu awaryjnego i zdarzeniach tworzenia procesu. Stosujemy się do [warunków korzystania z usług Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) i [zasad zachowania poufności informacji](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), które stanowią, że firma Microsoft nie będzie używać danych klienta ani informacji pochodnych do żadnych celów reklamowych ani zbliżonych celów komercyjnych. Danych klienta używamy tylko w razie potrzeby w celu świadczenia usług platformy Azure, włączając w to cele zgodne ze świadczeniem tych usług. Użytkownik zachowuje wszystkie uprawnienia do danych klienta.

**Użycie danych**: firma Microsoft używa wzorców i analizy zagrożeń obecnych w wielu dzierżawach, aby zwiększyć możliwości wykrywania zagrożeń i zapobiegania im — robimy to zgodnie ze zobowiązaniami co do prywatności opisanymi w [zasadach zachowania poufności informacji](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

**Lokalizacja danych**: konto magazynu jest określone dla każdego regionu, w którym działają maszyny wirtualne. Dzięki temu można przechowywać dane w tym samym regionie co maszyna wirtualna, z której zbierane są dane. Te dane, w tym pliki zrzutu awaryjnego, będą trwale przechowywane na koncie magazynu. Ponadto informacje dotyczące alertów zabezpieczeń, w tym alertów ze zintegrowanych rozwiązań firm partnerskich, zaleceń oraz stanu kondycji zabezpieczeń są przechowywane w magazynie usługi Azure Security Center, obecnie znajdującym się na terenie Stanów Zjednoczonych.

## <a name="managing-data-collection-from-virtual-machines"></a>Zarządzanie zbieraniem danych z maszyn wirtualnych
W przypadku włączenia usługi Azure Security Center zbieranie danych jest włączone dla każdej subskrypcji. Zbieranie danych może zostać wyłączone w sekcji „Zasady zabezpieczeń” pulpitu nawigacyjnego usługi Azure Security Center. Jeśli zbieranie danych jest włączone, usługa Azure Security Center aprowizuje agenta usługi Azure Monitoring na wszystkich istniejących obsługiwanych maszynach wirtualnych i tych nowo utworzonych. Rozszerzenie Azure Security Monitoring skanuje w poszukiwaniu różnych konfiguracji związanych z zabezpieczeniami i przekazuje zdarzenia do podsystemu [Śledzenie zdarzeń systemu Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx). Ponadto system operacyjny będzie zgłaszać zdarzenia dziennika zdarzeń w trakcie pracy maszyny. Przykłady takich danych to typ systemu operacyjnego i jego wersja, dzienniki systemu operacyjnego (dzienniki zdarzeń systemu Windows), uruchomione procesy, nazwa maszyny, adresy IP, zalogowany użytkownik i identyfikator dzierżawy. Agent usługi Azure Monitoring odczytuje wpisy dziennika zdarzeń, a narzędzie Śledzenie zdarzeń systemu Windows śledzi i kopiuje je na konto magazynu w celu dokonania analizy. 

Dla każdego regionu, w którym zostały uruchomione maszyny wirtualne, określane jest konto magazynu, które przechowuje dane z tych maszyn w tym samym regionie. Dzięki temu można łatwo przechowywać dane w tym samym obszarze geograficznym w celu zachowania prywatności i poufności danych. Konta magazynu dla każdego regionu można konfigurować w sekcji „Zasady zabezpieczeń” pulpitu nawigacyjnego usługi Azure Security Center.

Agent usługi Azure Monitoring kopiuje również pliki zrzutu awaryjnego na konto magazynu.  Usługa Azure Security Center zbiera efemeryczne kopie plików zrzutu awaryjnego i analizuje je pod kątem dowodów na próby ich naruszenia i pomyślnie przeprowadzonych ataków.  Usługa Azure Security Center dokonuje tej analizy w ramach tego samego regionu geograficznego, co konto magazynu, i usuwa efemeryczne kopie po zakończeniu analizy.

Zbieranie danych z maszyn wirtualnych można w dowolnym momencie wyłączyć, co spowoduje usunięcie wszystkich agentów monitorowania zainstalowanych wcześniej przez usługę Azure Security Center.

## <a name="see-also"></a>Zobacz też
W tym dokumencie wyjaśniono, jak zarządzane i chronione są dane w usłudze Azure Security Center. Aby dowiedzieć się więcej na temat usługi Azure Security Center, zobacz:

* [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md)— informacje na temat planowania i zagadnień projektowych podczas wdrażania usługi Azure Security Center.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure




<!--HONumber=Dec16_HO1-->


