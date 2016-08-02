<properties
   pageTitle="Zarządzanie rozwiązaniami partnerskimi w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument przedstawia sposób, w jaki Centrum zabezpieczeń Azure umożliwia ogólne monitorowanie kondycji rozwiązań partnerskich zintegrowanych z subskrypcją platformy Azure."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/22/2016"
   ms.author="terrylan"/>

# Monitorowanie rozwiązań partnerskich w Centrum zabezpieczeń Azure

Ten dokument zawiera informacje na temat sposobu monitorowania kondycji rozwiązań partnerskich w Centrum zabezpieczeń Azure.

> [AZURE.NOTE] Podane tu informacje odnoszą się do wersji zapoznawczej Centrum zabezpieczeń Azure. Informacje na temat usługi przedstawiono w tym dokumencie za pomocą przykładowego wdrożenia. Nie jest to przewodnik krok po kroku.

## Czym jest Centrum zabezpieczeń?
 Centrum zabezpieczeń Azure ułatwia zapobieganie zagrożeniom, ich wykrywanie i reagowanie na nie, a przy tym zapewnia lepszy wgląd i większą kontrolę w zakresie bezpieczeństwa zasobów na platformie Azure. Umożliwia zintegrowane monitorowanie zabezpieczeń i zarządzanie zasadami dla wszystkich subskrypcji, pomaga wykrywać zagrożenia, które w przeciwnym razie mogłyby pozostać niezauważone, a także współpracuje z szerokim ekosystemem rozwiązań zabezpieczających.

## Monitorowanie rozwiązań partnerskich

Kafelek **Rozwiązania partnerskie** w bloku **Centrum zabezpieczeń** umożliwia ogólne monitorowanie kondycji rozwiązań partnerskich zintegrowanych z subskrypcją platformy Azure.
![Kafelek Rozwiązania partnerskie][1]

Kafelek **Rozwiązania partnerskie** przedstawia liczbę rozwiązań partnerskich i podsumowanie ich stanu.

Rozwiązanie partnerskie może mieć następujący **STAN**:

- Chronione (kolor zielony) — brak problemów dotyczących kondycji.
- W złej kondycji (kolor czerwony) — istnieje problem z kondycją wymagający natychmiastowej uwagi.
- Raportowanie zatrzymane (kolor pomarańczowy) — raportowanie dotyczące kondycji rozwiązania zostało zatrzymane.
- Nieznany stan ochrony (kolor pomarańczowy) — kondycja rozwiązania jest w tej chwili nieznana, ponieważ proces dodawania nowego zasobu do istniejącego rozwiązania zakończył się niepowodzeniem.
- Nie zgłoszono (kolor szary) — rozwiązanie nie zgłosiło jeszcze żadnych informacji. Stan rozwiązania może nie zostać zgłoszony, jeśli rozwiązanie zostało właśnie podłączone i jego wdrażanie nadal trwa.

Jeśli nie masz rozwiązań zintegrowanych z subskrypcją, kafelek stanu będzie zawierać informację o braku rozwiązań. Wybranie kafelka **Rozwiązania partnerskie** umożliwi otwarcie bloku **Zalecenia** w celu wdrożenia partnerskich rozwiązań zabezpieczeń.
![Brak rozwiązań partnerskich][2]

Aby wyświetlić informacje o kondycji rozwiązań partnerskich:

1. Wybierz kafelek **Rozwiązania partnerskie**. Zostanie otwarty blok zawierający listę rozwiązań partnerskich połączonych z Centrum zabezpieczeń.
![Rozwiązania partnerskie][3]

2. Wybierz rozwiązanie partnerskie. W tym przykładzie wybierzemy rozwiązanie **F5 WAF2**.  Zostanie otwarty blok, który wyświetla stan rozwiązania partnerskiego oraz zasoby skojarzone z rozwiązaniem. Wybierz opcję **Konsola rozwiązań**, aby otworzyć narzędzie do zarządzania rozwiązaniem partnerskim.
![Szczegóły rozwiązania partnerskiego][4]

3. Wróć do bloku **F5-WAF2**, a następnie wybierz pozycję **Połącz aplikację**. Zostanie otwarty blok **Łączenie aplikacji**. W tym miejscu możesz połączyć zasoby z rozwiązaniem partnerskim.
![Łączenie zasobów z rozwiązaniem partnerskim][5]

## Następne kroki
W tym dokumencie przedstawiliśmy kafelek **Rozwiązania partnerskie** w Centrum zabezpieczeń. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — informacje na temat konfigurowania zasad zabezpieczeń dla subskrypcji i grup zasobów na platformie Azure.
- [Zarządzanie zaleceniami dotyczącymi zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — informacje o tym, w jaki sposób zalecenia ułatwiają ochronę zasobów platformy Azure.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure.
- [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi.
- [Centrum zabezpieczeń Azure — często zadawane pytania](security-center-faq.md) — odpowiedzi na najczęstsze pytania dotyczące korzystania z usługi.
- [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — najnowsze informacje na temat zabezpieczeń platformy Azure.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png



<!--HONumber=Jun16_HO2-->


