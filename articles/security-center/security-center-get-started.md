<properties
   pageTitle="Przewodnik Szybki start dotyczący Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pomaga szybko rozpocząć pracę z Centrum zabezpieczeń Azure. Omówiono w nim składniki monitorowania zabezpieczeń i zarządzania zasadami oraz podano linki do następnych kroków."
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
   ms.date="04/26/2016"
   ms.author="terrylan"/>

# Przewodnik Szybki start dotyczący Centrum zabezpieczeń Azure

Ten dokument pomaga szybko rozpocząć pracę z Centrum zabezpieczeń Azure. Omówiono w nim składniki monitorowania zabezpieczeń i zarządzania zasadami oraz podano linki do następnych kroków.

> [AZURE.NOTE] Podane tu informacje odnoszą się do wersji zapoznawczej Centrum zabezpieczeń Azure. Informacje na temat usługi przedstawiono w tym dokumencie za pomocą przykładowego wdrożenia. Nie jest to przewodnik krok po kroku.

## Co to jest Centrum zabezpieczeń Azure?
 Centrum zabezpieczeń Azure ułatwia zapobieganie zagrożeniom, ich wykrywanie i reagowanie na nie, a przy tym zapewnia lepszy wgląd i większą kontrolę w zakresie bezpieczeństwa zasobów na platformie Azure. Umożliwia zintegrowane monitorowanie zabezpieczeń i zarządzanie zasadami dla wszystkich subskrypcji, pomaga wykrywać zagrożenia, które w przeciwnym razie mogłyby pozostać niezauważone, a także współpracuje z szerokim ekosystemem rozwiązań zabezpieczających.

## Wymagania wstępne

Do rozpoczęcia korzystania z Centrum zabezpieczeń wymagana jest subskrypcja usługi Microsoft Azure. Centrum zabezpieczeń jest włączone w ramach subskrypcji. Jeśli nie masz subskrypcji, możesz zarejestrować się, aby uzyskać dostęp do [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

 Centrum zabezpieczeń jest dostępne w [Portalu Azure](https://azure.microsoft.com/features/azure-portal/). Aby uzyskać więcej informacji, zobacz [dokumentację portalu](https://azure.microsoft.com/documentation/services/azure-portal/).


## Dostęp do Centrum zabezpieczeń

Aby uzyskać dostęp do Centrum zabezpieczeń, wykonaj następujące kroki w portalu:

1. Wybierz pozycję **Przeglądaj**, a następnie przewiń do pozycji **Centrum zabezpieczeń**.
![Dostęp do Centrum zabezpieczeń Azure w portalu][1]

2. Wybierz pozycję **Centrum zabezpieczeń**. Spowoduje to otwarcie bloku **Centrum zabezpieczeń**.
3. Aby mieć w przyszłości łatwiejszy dostęp do bloku **Centrum zabezpieczeń**, wybierz opcję **Przypnij blok do pulpitu nawigacyjnego** (w prawym górnym rogu).
![Opcja Przypnij blok do pulpitu nawigacyjnego][2]

## Korzystanie z Centrum zabezpieczeń

Skonfigurować można zasady zabezpieczeń dla subskrypcji i grup zasobów platformy Azure. Skonfigurujmy **zasady** zabezpieczeń dla subskrypcji:

1. Wybierz kafelek **Zasady** w bloku **Centrum zabezpieczeń**.
![Centrum zabezpieczeń][3]

2. W bloku **Zasady zabezpieczeń — Zdefiniuj zasady dla subskrypcji lub grupy zasobów** wybierz subskrypcję.
![Blok Zasady zabezpieczeń w Centrum zabezpieczeń Azure][4]

3. W bloku **Zasady zabezpieczeń** włącz opcję **Zbieranie danych**, aby dzienniki były zbierane automatycznie. Włączenie opcji **Zbieranie danych** spowoduje również aprowizację rozszerzonego monitorowania na wszystkie bieżące oraz nowe maszyny wirtualne w subskrypcji.
4. Wybierz opcję **Wybierz konto magazynu dla regionu**. Dla każdego regionu, w którym zostały uruchomione maszyny wirtualne, wybierz konto magazynu, w którym będą przechowywane dane z tych maszyn. Jeśli nie zostanie wybrane konto magazynu dla każdego regionu, zostanie ono utworzone automatycznie. Ze względów bezpieczeństwa zbierane dane są logicznie odizolowane od danych innych klientów.

     > [AZURE.NOTE] Firma Microsoft zaleca włączenie opcji zbierania danych i wybór konta magazynu najpierw na poziomie subskrypcji.  Zasady zabezpieczeń można ustawić na poziomie subskrypcji i grupy zasobów platformy Azure, ale konfigurowanie zbierania danych i konta magazynu odbywa się wyłącznie na poziomie subskrypcji.

5. Włącz **Zalecenia**, które chcesz zobaczyć jako część zasad zabezpieczeń. Przykłady:

 - Włączenie opcji **Aktualizacje systemu** spowoduje przeskanowanie wszystkich obsługiwanych maszyn wirtualnych w celu znalezienia brakujących aktualizacji systemu operacyjnego.
 - Włączenie opcji **Reguły linii bazowej** spowoduje przeskanowanie wszystkich obsługiwanych maszyn wirtualnych w celu znalezienia konfiguracji systemu operacyjnego, które mogą osłabiać ochronę maszyny wirtualnej przed atakami.

Postępowanie z **Zaleceniami**:

1. Wróć do bloku **Centrum zabezpieczeń** i wybierz kafelek **Zalecenia**. Centrum zabezpieczeń okresowo analizuje stan zabezpieczeń zasobów platformy Azure. Po znalezieniu potencjalnych luk w zabezpieczeniach w tym miejscu jest wyświetlane zalecenie.
2.  Wybierz każde zalecenie, aby wyświetlić więcej informacji lub podjąć działania w celu rozwiązania problemu.
![Zalecenia w Centrum zabezpieczeń Azure][5]

Wyświetl stan kondycji i zabezpieczeń zasobów za pomocą kafelka **Kondycja zabezpieczeń zasobów**:

1.  Wróć do bloku **Centrum zabezpieczeń**.
2.  Kafelek **Kondycja zabezpieczeń zasobów** zawiera wskaźniki stanu zabezpieczeń następujących elementów: **Maszyny wirtualne**, **Sieć**, **SQL** i **Aplikacje**.
3.  Wybierz blok **Maszyny wirtualne**, aby wyświetlić więcej informacji.
4.  Blok **Maszyny wirtualne** wyświetla podsumowanie, które zawiera informacje o stanie programów chroniących przed złośliwym kodem, aktualizacji systemu oraz reguł linii bazowej maszyn wirtualnych.
5.  Wybierz element w obszarze **ZALECENIA DOTYCZĄCE MASZYNY WIRTUALNEJ**, aby wyświetlić więcej informacji lub podjąć działania w celu skonfigurowania niezbędnych opcji.
6.  Przejrzyj dodatkowe informacje o określonych maszynach wirtualnych.
![Kafelek Kondycja zasobów w Centrum zabezpieczeń Azure][6]

Postępowanie z **Alertami zabezpieczeń**:

1.  Wróć do bloku **Centrum zabezpieczeń** i wybierz kafelek **Alerty zabezpieczeń**. W bloku **Alerty zabezpieczeń** jest wyświetlana lista alertów. Alerty są generowane na podstawie analizy Centrum zabezpieczeń uwzględniającej dzienniki zabezpieczeń i aktywność sieciową. Uwzględniane są również alerty zintegrowanych rozwiązań partnerskich.
![Alerty zabezpieczeń w Centrum zabezpieczeń Azure][7]

2.  Wybierz alert, aby wyświetlić dodatkowe informacje.
![Szczegóły alertów zabezpieczeń w Centrum zabezpieczeń Azure][8]

Wyświetlanie informacji o kondycji **Rozwiązań partnerskich**:

1. Wróć do bloku **Centrum zabezpieczeń**. Kafelek **Rozwiązania partnerskie** umożliwia szybkie i łatwe monitorowanie stanu kondycji rozwiązań partnerskich zintegrowanych z subskrypcją platformy Azure.
2. Wybierz kafelek **Rozwiązania partnerskie**. Zostanie otwarty blok zawierający listę rozwiązań partnerskich połączonych z Centrum zabezpieczeń.
![Rozwiązania partnerskie][9]

3. Wybierz rozwiązanie partnerskie. W tym przykładzie wybierzemy rozwiązanie **F5 WAF2**.  Zostanie otwarty blok, który wyświetla stan rozwiązania partnerskiego oraz zasoby skojarzone z rozwiązaniem. Wybierz opcję **Konsola rozwiązań**, aby otworzyć narzędzie do zarządzania rozwiązaniem partnerskim.
![Szczegóły rozwiązania partnerskiego][10]

## Następne kroki
W tym dokumencie zostały opisane składniki monitorowania zabezpieczeń i zarządzania zasadami w Centrum zabezpieczeń. Aby dowiedzieć się więcej, zobacz następujące tematy:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — informacje na temat konfigurowania zasad zabezpieczeń dla subskrypcji i grup zasobów na platformie Azure.
- [Zarządzanie zaleceniami dotyczącymi zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — informacje o tym, w jaki sposób zalecenia ułatwiają ochronę zasobów platformy Azure.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure.
- [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi.
- [Monitorowanie rozwiązań partnerskich w Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
- [Centrum zabezpieczeń Azure — często zadawane pytania](security-center-faq.md) — odpowiedzi na najczęstsze pytania dotyczące korzystania z usługi.
- [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — najnowsze informacje na temat zabezpieczeń platformy Azure.

<!--Image references-->
[1]: ./media/security-center-get-started/security-tile.png
[2]: ./media/security-center-get-started/pin-blade.png
[3]: ./media/security-center-get-started/security-center.png
[4]: ./media/security-center-get-started/security-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/partner-solutions-detail.png



<!--HONumber=Jun16_HO2-->


