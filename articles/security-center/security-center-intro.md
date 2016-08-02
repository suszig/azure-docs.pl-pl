<properties
   pageTitle="Wprowadzenie do Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Uzyskaj informacje na temat Centrum zabezpieczeń Azure, jego kluczowych możliwości i sposobu działania."
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

# Wprowadzenie do Centrum zabezpieczeń Azure

Uzyskaj informacje na temat Centrum zabezpieczeń Azure, jego kluczowych możliwości i sposobu działania.

> [AZURE.NOTE] Podane tu informacje odnoszą się do wersji zapoznawczej Centrum zabezpieczeń Azure. Informacje na temat usługi przedstawiono w tym dokumencie za pomocą przykładowego wdrożenia.

## Co to jest Centrum zabezpieczeń Azure?
 Centrum zabezpieczeń Azure ułatwia zapobieganie zagrożeniom, ich wykrywanie i reagowanie na nie, a przy tym zapewnia lepszy wgląd i większą kontrolę w zakresie bezpieczeństwa zasobów na platformie Azure. Zapewnia zintegrowane monitorowanie zabezpieczeń i zarządzanie zasadami subskrypcji platformy Azure, pomaga wykrywać zagrożenia, które w przeciwnym razie mogłyby pozostać niezauważone, a także współpracuje z szerokim ekosystemem rozwiązań z zakresu zabezpieczeń.

##  Najważniejsze możliwości
 Centrum zabezpieczeń zapewnia łatwe i skuteczne zapobieganie zagrożeniom, ich wykrywanie oraz wbudowane w platformę Azure możliwości reakcji na nie. Do najważniejszych możliwości należą:

| | |
|----- |-----|
| Zapobieganie | Monitoruje stan zabezpieczeń zasobów platformy Azure |
| | Definiuje zasady dla subskrypcji platformy Azure i grup zasobów w oparciu o firmowe wymagania w zakresie zabezpieczeń, typy używanych aplikacji i wrażliwość danych |
| | Stosuje zalecenia dotyczące zabezpieczeń oparte na zasadach, przeprowadzając właścicieli usług przez proces wdrażania potrzebnych elementów sterujących |
| | Szybko wdraża usługi oraz urządzenia zabezpieczające firmy Microsoft i partnerów |
| Wykrywanie |Automatycznie zbiera i analizuje dane dotyczące zabezpieczeń z zasobów platformy Azure, sieci i rozwiązań partnerskich, takich jak programy ochrony przed złośliwym oprogramowaniem i zapory |
| | Wykorzystuje globalne dane dotyczące analizy zagrożeń pochodzące z produktów i usług firmy Microsoft, jednostki ds. przestępstw cyfrowych (DCU) firmy Microsoft, Centrum zabezpieczeń firmy Microsoft (MSRC) i zewnętrznych źródeł danych |
| | Stosuje zaawansowane metody analizy, w tym uczenie maszynowe i analizę behawioralną |
| Reakcja | Szereguje incydenty/alerty zabezpieczeń według priorytetów |
| | Oferuje wgląd w źródło ataku i zasoby, na które on wpływa |
| | Podpowiada sposoby powstrzymania bieżącego ataku i pomaga w zapobieganiu atakom w przyszłości |

## Przewodnik wprowadzający
 Centrum zabezpieczeń jest dostępne z [portalu Azure](https://azure.microsoft.com/features/azure-portal/). [Zaloguj się do portalu](https://portal.azure.com), wybierz opcję **Przeglądaj**, a następnie przewiń do opcji **Centrum zabezpieczeń** lub wybierz kafelek **Centrum zabezpieczeń**, przypięty wcześniej do pulpitu nawigacyjnego w portalu.

![Kafelek Zabezpieczenia w portalu Azure][1]

W Centrum zabezpieczeń można ustawiać zasady zabezpieczeń, monitorować konfigurację zabezpieczeń i wyświetlać alerty zabezpieczeń.

### Zasady zabezpieczeń

Można określić zasady dla subskrypcji platformy Azure i grup zasobów zgodnie z wymaganiami bezpieczeństwa w firmie. Można również dostosować je do rodzajów używanych aplikacji lub do wrażliwości danych w każdej subskrypcji. Na przykład zasoby używane dla rozwoju lub testowania mogą mieć inne wymagania dotyczące zabezpieczeń niż te, które są wykorzystywane przez aplikacje produkcyjne. Podobnie aplikacje z danymi podlegającymi ochronie, jak dane osobowe, mogą wymagać wyższego poziomu zabezpieczeń.

> [AZURE.NOTE] Aby zmodyfikować zasady zabezpieczeń na poziomie subskrypcji lub grupy zasobów, użytkownik musi być właścicielem lub współpracownikiem subskrypcji.

W bloku **Centrum zabezpieczeń** wybierz kafelek **Zasady**, aby uzyskać listę subskrypcji i grup zasobów.   

![Blok Centrum zabezpieczeń][2]

W bloku **Zasady zabezpieczeń** wybierz subskrypcję, aby wyświetlić szczegóły zasad.

![Subskrypcja bloku Zasady zabezpieczeń][3]

Funkcja **Zbieranie danych** (zobacz powyżej) umożliwia zbieranie danych dla zasad zabezpieczeń. Włączenie umożliwia:

- Codzienne skanowanie wszystkich obsługiwanych maszyn wirtualnych w celu monitorowania zabezpieczeń i tworzenia zaleceń.
- Zbieranie zdarzeń zabezpieczeń w celu analizy i wykrywania zagrożeń.

Funkcja **Wybierz konto magazynu na region** (zobacz powyżej) pozwala na wybór dla każdego regionu, w którym znajdują się uruchomione maszyny wirtualne, konta magazynu, w którym będą przechowywane dane pochodzące z tych maszyn wirtualnych. Jeśli nie zostanie wybrane konto magazynu dla każdego regionu, zostanie ono utworzone automatycznie. Ze względów bezpieczeństwa zbierane dane są logicznie odizolowane od danych innych klientów.

> [AZURE.NOTE] Funkcje zbierania danych i wyboru konta magazynu dla regionu są konfigurowane na poziomie subskrypcji.

Funkcja **Pokaż zalecenia dotyczące** (zobacz powyżej) pozwala na wybór elementów sterujących zabezpieczeń, które chcesz monitorować i zalecać, w oparciu o wymagania bezpieczeństwa zasobów w ramach subskrypcji.

Następnie wybierz grupę zasobów, aby wyświetlić szczegóły zasad.

![Grupa zasobów bloku Zasady zabezpieczeń][4]

Funkcja **Dziedziczenie** (zobacz powyżej) pozwala na zdefiniowanie grupy zasobów jako:

- Dziedziczona (domyślnie), co oznacza, że wszystkie zasady zabezpieczeń dla tej grupy zasobów są dziedziczone z poziomu subskrypcji.
- Unikatowa, co oznacza, że grupa zasobów będzie miała niestandardowe zasady zabezpieczeń. Musisz wprowadzić zmiany w obszarze **Pokaż zalecenia dotyczące**.

> [AZURE.NOTE] Jeśli między zasadami poziomu subskrypcji i zasadami poziomu grupy zasobów występuje konflikt, pierwszeństwo ma poziom zasad grupy zasobów.

### Zalecenia dotyczące zabezpieczeń

 Centrum zabezpieczeń analizuje stan zabezpieczeń zasobów platformy Azure w celu identyfikowania potencjalnych luk w zabezpieczeniach. Lista zaleceń prowadzi użytkownika przez proces konfigurowania wymaganych elementów sterujących. Przykłady obejmują:

- Inicjowanie ochrony przed złośliwym oprogramowaniem w celu identyfikacji i usuwania złośliwego oprogramowania
- Konfigurowanie grup zabezpieczeń sieciowych i reguł sterujących ruchem do maszyn wirtualnych
- Inicjowanie zapór aplikacji sieci web pomagających chronić przed atakami na aplikacje sieci web użytkownika
- Wdrażanie brakujących aktualizacji systemu
- Modyfikowanie konfiguracji systemu operacyjnego, które odbiegają od zalecanych standardów

Kliknij kafelek **Zalecenia**, aby uzyskać listę zaleceń. Kliknij każde zalecenie, aby wyświetlić dodatkowe informacje lub podjąć działanie w celu rozwiązania problemu.

![Zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure][5]

### Kondycja zasobów

Kafelek **Kondycja zabezpieczeń zasobów** przedstawia ogólny stan zabezpieczeń środowiska według typów zasobów, w tym maszyn wirtualnych, aplikacji sieci web i innych zasobów.   

Wybierz typ zasobu na kafelku **Kondycja zabezpieczeń zasobów**, aby wyświetlić więcej informacji, w tym listę wszelkich zidentyfikowanych potencjalnych luk w zabezpieczeniach. (W poniższym przykładzie zaznaczono opcję **Maszyny wirtualne**).

![Kafelek Kondycja zasobów][6]

### Alerty zabezpieczeń

 Centrum zabezpieczeń automatycznie gromadzi, analizuje i integruje dane dzienników z zasobów platformy Azure, sieci i rozwiązań partnerskich, takich jak programy chroniące przed złośliwym oprogramowaniem i zapory. Po wykryciu zagrożenia tworzony jest alert zabezpieczeń. Przykłady obejmują wykrywanie:

- Zagrożonych maszyn wirtualnych komunikujących się ze znanymi złośliwymi adresami IP
- Zaawansowanego złośliwego oprogramowania wykrytego za pomocą funkcji raportowania błędów systemu Windows
- Ataków siłowych wobec maszyn wirtualnych
- Alerty zabezpieczeń ze zintegrowanych programów chroniących przed złośliwym oprogramowaniem i zapór

Kliknięcie kafelka **Alerty zabezpieczeń** powoduje wyświetlenie listy alertów uszeregowanych według priorytetów.

![Alerty zabezpieczeń][7]

Wybór alertu powoduje wyświetlenie dokładniejszych informacji na temat ataku oraz sugestii dotyczących sposobu jego odparcia.

![Szczegóły alertu zabezpieczeń][8]

### Rozwiązania partnerskie

Kafelek **Rozwiązania partnerskie** umożliwia szybkie i łatwe monitorowanie stanu kondycji rozwiązań partnerskich zintegrowanych z subskrypcją platformy Azure. W Centrum zabezpieczeń wyświetlane są alerty pochodzące z rozwiązań.

Wybierz kafelek **Rozwiązania partnerskie**. Zostanie otwarty blok zawierający listę wszystkich połączonych rozwiązań partnerskich.

![Rozwiązania partnerskie][9]

## Rozpoczynanie pracy
Do rozpoczęcia pracy z Centrum zabezpieczeń konieczna jest subskrypcja platformy Microsoft Azure. Centrum zabezpieczeń jest włączone w ramach subskrypcji platformy Azure. Jeśli nie masz subskrypcji, możesz zarejestrować się, aby uzyskać dostęp do [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

 Centrum zabezpieczeń jest dostępne z [portalu Azure](https://azure.microsoft.com/features/azure-portal/). Aby uzyskać więcej informacji, zobacz [dokumentację portalu](https://azure.microsoft.com/documentation/services/azure-portal/).

[Wprowadzenie do korzystania z Centrum zabezpieczeń Azure](security-center-get-started.md) szybko przeprowadzi Cię przez składniki monitorowania zabezpieczeń i zarządzania zasadami Centrum zabezpieczeń.

## Następne kroki
Ten dokument zawiera wprowadzenie do Centrum zabezpieczeń, jego najważniejszych możliwości, a także informacje o sposobie rozpoczęcia pracy. Aby dowiedzieć się więcej, zobacz następujące tematy:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — informacje na temat konfigurowania zasad zabezpieczeń dla subskrypcji i grup zasobów na platformie Azure.
- [Zarządzanie zaleceniami dotyczącymi zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — informacje o tym, w jaki sposób zalecenia ułatwiają ochronę zasobów platformy Azure.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure.
- [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — informacje na temat reagowania na alerty zabezpieczeń i zarządzania nimi.
- [Monitorowanie rozwiązań partnerskich w Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
- [Centrum zabezpieczeń Azure — często zadawane pytania](security-center-faq.md) — odpowiedzi na najczęstsze pytania dotyczące korzystania z usługi.
- [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — najnowsze informacje na temat zabezpieczeń platformy Azure.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.PNG
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png



<!--HONumber=Jun16_HO2-->


