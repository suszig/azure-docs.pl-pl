---
title: "Azure Security Center — bezpieczeństwo danych | Microsoft Docs"
description: "W tym dokumencie wyjaśniono, jak zarządzane i chronione są dane w usłudze Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/06/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 9852981e530cd147c2d34ac2ede251b58a167a0a
ms.openlocfilehash: 5c030f463b21284c15752cf95aa1f9a75f17ffb0


---
# <a name="azure-security-center-data-security"></a>Azure Security Center — bezpieczeństwo danych
Aby ułatwić klientom zapobieganie zagrożeniom, wykrywanie ich i reagowanie na nie, usługa Azure Security Center zbiera i przetwarza dane dotyczące zabezpieczeń, w tym informacje o konfiguracji, metadane, dzienniki zdarzeń, pliki zrzutu awaryjnego i inne. Firma Microsoft przestrzega surowych wymogów z zakresu zabezpieczeń i zgodności — od kodu po działanie usługi.

W tym artykule wyjaśniono, jak zarządzane i chronione są dane w usłudze Azure Security Center.


## <a name="data-sources"></a>Źródła danych
Usługa Azure Security Center analizuje dane z następujących źródeł, aby zapewnić wgląd w stan zabezpieczeń, zidentyfikować luki w zabezpieczeniach i polecić rozwiązania oraz wykryć aktywne zagrożenia:

- Usługi platformy Azure: używa informacji o konfiguracji wdrożonych usług platformy Azure, komunikując się z dostawcą zasobów tej usługi.
- Ruch sieciowy: używa próbkowanych metadanych ruchu sieciowego z infrastruktury firmy Microsoft, takich jak źródłowy i docelowy adres IP, źródłowy i docelowy port, rozmiar pakietu i protokół sieciowy.
- Rozwiązania partnerów: używa alertów zabezpieczeń z rozwiązań zintegrowanych partnerów, takich jak zapory i rozwiązania do ochrony przed złośliwym oprogramowaniem.
- Maszyny wirtualne: używa informacji dotyczących konfiguracji oraz zdarzeń związanych z zabezpieczeniami, takich jak zdarzenia systemu Windows i dzienniki inspekcji, dzienniki programu IIS, komunikaty dziennika systemu i pliki zrzutu awaryjnego, z maszyn wirtualnych.


## <a name="data-protection"></a>Ochrona danych
**Podział danych**: dane są logicznie oddzielone dla każdego składnika w całej usłudze. Wszystkie dane są otagowane informacjami o organizacji. To tagowanie jest obecne przez cały cykl życia danych i jest wymuszane w każdej warstwie usługi.

**Dostęp do danych**: aby oferować rekomendacje dotyczące zabezpieczeń i badać potencjalne zagrożenia bezpieczeństwa, personel firmy Microsoft może uzyskiwać dostęp do informacji zbieranych lub analizowanych przez usługi platformy Azure, w tym plików zrzutu awaryjnego, zdarzeń tworzenia procesów, artefaktów i migawek dysków maszyny wirtualnej, które przypadkowo mogą zawierać dane klientów lub dane osobowe z maszyn wirtualnych. Stosujemy się do [warunków korzystania z usług Microsoft Online Services i zasad zachowania poufności informacji](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), które stanowią, że firma Microsoft nie będzie używać danych klienta ani informacji pochodnych do żadnych celów reklamowych ani zbliżonych celów komercyjnych. Danych klienta używamy tylko w razie potrzeby w celu świadczenia usług platformy Azure, włączając w to cele zgodne ze świadczeniem tych usług. Użytkownik zachowuje wszystkie uprawnienia do danych klienta.

**Użycie danych**: firma Microsoft używa wzorców i analizy zagrożeń obecnych w wielu dzierżawach, aby zwiększyć możliwości wykrywania zagrożeń i zapobiegania im — robimy to zgodnie ze zobowiązaniami co do prywatności opisanymi w [zasadach zachowania poufności informacji](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

## <a name="data-location"></a>Lokalizacja danych
**Konta magazynu**: konto magazynu jest określone dla każdego regionu, w którym działają maszyny wirtualne. Dzięki temu można przechowywać dane w tym samym regionie co maszyna wirtualna, z której zbierane są dane. Te dane, w tym pliki zrzutu awaryjnego, będą trwale przechowywane na koncie magazynu. Migawki dysków maszyny wirtualnej są przechowywane na tym samym koncie magazynu jako dysk maszyny wirtualnej.

**Magazyn usługi Azure Security Center Storage**: informacje dotyczące alertów zabezpieczeń, w tym alerty partnerów, zalecenia oraz stan kondycji zabezpieczeń, są przechowywane centralnie, obecnie na terenie Stanów Zjednoczonych. Do informacji zebranych z maszyn wirtualnych mogą należeć informacje dotyczące konfiguracji oraz zdarzeń związanych z zabezpieczeniami, dzięki którym możliwe jest przekazanie użytkownikowi danych na temat alertu zabezpieczeń, ewentualnych zaleceń postępowania oraz stanu kondycji zabezpieczeń.

Usługa Azure Security Center zbiera efemeryczne kopie plików zrzutu awaryjnego i analizuje je pod kątem dowodów na próby ich naruszenia i pomyślnie przeprowadzonych ataków. Usługa Azure Security Center dokonuje tej analizy w ramach tego samego obszaru geograficznego, co obszar roboczy, i usuwa efemeryczne kopie po zakończeniu analizy.

Artefakty maszyny są przechowywane centralnie w tym samym regionie, co maszyna wirtualna.


## <a name="managing-data-collection-from-virtual-machines"></a>Zarządzanie zbieraniem danych z maszyn wirtualnych
W przypadku włączenia usługi Azure Security Center zbieranie danych jest włączone dla każdej subskrypcji. Zbieranie danych można również wyłączyć w sekcji Zasady zabezpieczeń usługi Azure Security Center. Jeśli zbieranie danych jest włączone, usługa Azure Security Center aprowizuje agenta usługi Azure Monitoring na wszystkich istniejących obsługiwanych maszynach wirtualnych i tych nowo utworzonych. Rozszerzenie Azure Security Monitoring skanuje w poszukiwaniu różnych konfiguracji związanych z zabezpieczeniami i przekazuje zdarzenia do podsystemu [Śledzenie zdarzeń systemu Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx). Ponadto system operacyjny będzie zgłaszać zdarzenia dziennika zdarzeń w trakcie pracy maszyny. Przykłady takich danych to typ systemu operacyjnego i jego wersja, dzienniki systemu operacyjnego (dzienniki zdarzeń systemu Windows), uruchomione procesy, nazwa maszyny, adresy IP, zalogowany użytkownik i identyfikator dzierżawy. Agent usługi Azure Monitoring odczytuje wpisy dziennika zdarzeń, a narzędzie Śledzenie zdarzeń systemu Windows śledzi i kopiuje je na konto magazynu w celu dokonania analizy.

Zbieranie danych z maszyn wirtualnych można w dowolnym momencie wyłączyć, co spowoduje usunięcie wszystkich agentów monitorowania zainstalowanych wcześniej przez usługę Azure Security Center. Kolekcja artefaktów i migawki dysków maszyny wirtualnej będzie nadal włączona, nawet jeśli wyłączono zbieranie danych.


## <a name="see-also"></a>Zobacz też
W tym dokumencie wyjaśniono, jak zarządzane i chronione są dane w usłudze Azure Security Center. Aby dowiedzieć się więcej na temat usługi Azure Security Center, zobacz:

* [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md)— informacje na temat planowania i zagadnień projektowych podczas wdrażania usługi Azure Security Center.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure



<!--HONumber=Feb17_HO1-->


