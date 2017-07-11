---
title: "Przewodnik planowania i obsługi usługi Security Center | Microsoft Docs"
description: "Ten dokument ułatwia planowanie przed wdrożeniem Centrum zabezpieczeń Azure oraz zawiera informacje o jego codziennych operacjach."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f984e4a2-ac97-40bf-b281-2f7f473494c4
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: c502e4363dbaa37455d1aad90d1e9fa855fd09b0
ms.contentlocale: pl-pl
ms.lasthandoff: 06/17/2017


---
<a id="azure-security-center-planning-and-operations-guide" class="xliff"></a>

# Przewodnik planowania i obsługi usługi Azure Security Center
Ten przewodnik jest przeznaczony dla specjalistów IT, architektów IT, analityków zabezpieczeń informacji i administratorów chmury, których organizacje planują wdrożenie usługa Azure Security Center.

>[!NOTE] 
>Począwszy od początku czerwca 2017 roku, usługa Security Center będzie używać programu Microsoft Monitoring Agent do gromadzenia i przechowywania danych. Aby dowiedzieć się więcej, zobacz [Migracja platformy usługi Azure Security Center](security-center-platform-migration.md). Informacje przedstawione w tym artykule reprezentują funkcję Security Center po przejściu do programu Microsoft Monitoring Agent.
>

<a id="planning-guide" class="xliff"></a>

## Przewodnik planowania
Ten przewodnik obejmuje zestaw kroków i zadań, które można wykonać, aby zoptymalizować korzystanie z usługi Security Center w zależności od wymagań dotyczących zabezpieczeń oraz modelu zarządzania chmurą w organizacji. Aby w pełni wykorzystać zalety usługi Security Center, trzeba zrozumieć, w jak różny sposób będą używać usługi poszczególne osoby i zespoły w organizacji, gdyż umożliwi to spełnienie wymagań związanych z bezpieczeństwem programowania i obsługi, monitorowaniem, zarządzaniem i reagowaniem na zdarzenia. Kluczowe obszary, które należy wziąć pod uwagę podczas planowania korzystania z usługi Security Center:

* Role zabezpieczeń i kontrola dostępu
* Zasady zabezpieczeń i zalecenia w tym zakresie
* Zbieranie i przechowywanie danych
* Bieżące monitorowanie zabezpieczeń
* Reagowanie na zdarzenia

W następnej sekcji dowiesz się, jak utworzyć plan dla każdego z tych obszarów i zastosować te zalecenia w zależności od wymagań.

> [!NOTE]
> Artykuł [Azure Security Center frequently asked questions (FAQ)](security-center-faq.md) (Centrum zabezpieczeń Azure — często zadawane pytania) zawiera listę często zadawanych pytań, która również może być przydatna w fazie projektowania i planowania.
> 

<a id="security-roles-and-access-controls" class="xliff"></a>

## Role zabezpieczeń i kontrola dostępu
W zależności od rozmiaru i struktury organizacji wiele osób oraz zespołów może korzystać z Centrum zabezpieczeń, aby wykonywać różne zadania związane z zabezpieczeniami. Poniższy diagram przedstawia przykład fikcyjnych osób oraz ich ról i obowiązków związanych z zabezpieczeniami:

![Role](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

Usługa Security Center umożliwia tym osobom wypełnianie różnych obowiązków. Na przykład:

**Jan (właściciel obciążenia chmury)**

* Zarządza obciążeniem chmury i powiązanymi zasobami
* Odpowiada za zaimplementowanie i utrzymanie ochrony zgodnie z zasadami zabezpieczeń firmy

**Aneta (CISO/CIO)**

* Odpowiada za wszystkie aspekty zabezpieczeń firmy
* Chce zrozumieć poziom zabezpieczeń firmy w przypadku różnych obciążeń chmury
* Potrzebuje informacji o najpoważniejszych atakach i zagrożeniach

**Daniel (zabezpieczenia informatyczne)**

* Określa firmowe zasady zabezpieczeń w celu zapewnienia odpowiedniej ochrony
* Monitoruje zgodność z zasadami
* Generuje raporty dla kierownictwa lub audytorów

**Magda (operacje zabezpieczeń)**

* Bez przerwy monitoruje alerty zabezpieczeń i reaguje na nie
* Eskaluje do właściciela obciążenia chmury lub analityka zabezpieczeń informatycznych

**Stanisław (analityk zabezpieczeń)**

* Bada ataki
* Praca z właścicielem obciążenia chmury w celu zastosowania rozwiązania 

Usługa Security Center używa [kontroli dostępu opartej na rolach](../active-directory/role-based-access-control-configure.md) udostępniającej [wbudowane role](../active-directory/role-based-access-built-in-roles.md), które można przypisać do użytkowników, grup i usług na platformie Azure. Gdy użytkownik otwiera usługę Security Center, widzi tylko informacje dotyczące zasobów, do których ma dostęp. Oznacza to, że użytkownik ma przypisaną rolę właściciela, współautora lub czytelnika subskrypcji albo grupy zasobów, do której należy zasób. Oprócz tych ról istnieją dwie określone role usługi Security Center:

- **Czytelnik zabezpieczeń**: użytkownik, który należy do tej roli, ma możliwość wyświetlania praw do usługi Security Center, co obejmuje zalecenia, alerty, zasady i kondycję, ale nie ma możliwości wprowadzania zmian.
- **Administrator zabezpieczeń**: użytkownik ma takie same uprawnienia, jak w przypadku roli Czytelnik zabezpieczeń, ale ma również możliwość aktualizowania zasad zabezpieczeń oraz odrzucania zaleceń i alertów.

Opisane powyżej role usługi Security Center nie mają dostępu do innych obszarów usług platformy Azure, takich jak magazyn, sieć internetowa i aplikacje mobilne oraz Internet rzeczy.  

> [!NOTE]
> Użytkownik musi posiadać subskrypcję, być właścicielem grupy zasobów lub współautorem, aby móc korzystać z usługi Security Center na platformie Azure. 
> 
> 

W przypadku osób opisanych na poprzednim diagramie będzie potrzebna następująca kontrola dostępu oparta na rolach:

**Jan (właściciel obciążenia chmury)**

* Właściciel/Współautor grupy zasobów

**Daniel (zabezpieczenia informatyczne)**

* Właściciel/współautor subskrypcji lub administrator zabezpieczeń

**Magda (operacje zabezpieczeń)**

* Czytelnik subskrypcji lub czytelnik zabezpieczeń przeglądający alerty
* Właściciel/współautor subskrypcji lub administrator zabezpieczeń, który musi odrzucać alerty

**Stanisław (analityk zabezpieczeń)**

* Czytelnik subskrypcji przeglądający alerty
* Właściciel/współautor subskrypcji, który musi odrzucać alerty
* Może być wymagany dostęp do obszaru roboczego

Niektóre inne istotne informacje, które należy wziąć pod uwagę:

* Tylko właściciele lub współautorzy subskrypcji i administratorzy zabezpieczeń mogą edytować zasady zabezpieczeń
* Tylko właściciele i współautorzy subskrypcji i grupy zasobów mogą stosować zalecenia dotyczące zabezpieczeń zasobu

Podczas planowania kontroli dostępu przy użyciu kontroli dostępu opartej na rolach dla usługi Security Center musisz wiedzieć, kto w Twojej organizacji będzie korzystał z usługi. Ponadto musisz wiedzieć, jakiego rodzaju zadania będą wykonywały te osoby, a następnie odpowiednio skonfigurować kontrolę dostępu opartą na rolach.

> [!NOTE]
> Zaleca się przypisanie użytkownikom najbardziej ograniczonej roli wystarczającej do wykonywania zadań. Na przykład użytkownikom, którzy muszą tylko przeglądać informacje o stanie zabezpieczeń zasobów, ale nie muszą podejmować działań, np. stosować zaleceń ani edytować zasad, należy przypisać rolę czytelnika.
> 
> 

<a id="security-policies-and-recommendations" class="xliff"></a>

## Zasady zabezpieczeń i zalecenia w tym zakresie
Zasady zabezpieczeń określają zestaw mechanizmów kontrolnych, które są zalecane dla zasobów w określonej subskrypcji. W usłudze Security Center możesz zdefiniować zasady zgodnie z wymaganiami w zakresie zabezpieczeń firmy oraz typem aplikacji lub stopniem poufności danych.

Zasady włączone na poziomie subskrypcji są automatycznie propagowane wśród wszystkich grup zasobów w ramach subskrypcji, jak pokazano na poniższym diagramie:

![Zasady zabezpieczeń](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-newUI.png)

> [!NOTE]
> Jeśli musisz sprawdzić, które zasady zostały zmienione, możesz użyć [Dzienników inspekcji Azure](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). Zmiany zasad są zawsze rejestrowane w Dziennikach inspekcji Azure.
> 
> 

<a id="security-recommendations" class="xliff"></a>

### Zalecenia dotyczące zabezpieczeń
Przed skonfigurowaniem zasad zabezpieczeń przejrzyj poszczególne [zalecenia dotyczące zabezpieczeń](security-center-recommendations.md) i określ, czy te zasady są właściwe dla różnych subskrypcji i grup zasobów. Ważne jest również, aby zrozumieć, jakie działania powinny zostać podjęte w celu wypełnienia [zaleceń dotyczących zabezpieczeń](https://docs.microsoft.com/en-us/azure/security-center/security-center-recommendations), oraz kto w organizacji będzie odpowiedzialny za monitorowanie pod kątem nowych zaleceń i podejmowanie wymaganych działań.

Usługa Security Center zaleci podanie szczegółowych informacji dotyczących kontaktu zabezpieczeń dla Twojej subskrypcji platformy Azure. Te informacje będą używane przez firmę Microsoft do kontaktowania się z Tobą, gdy centrum Microsoft Security Response Center (MSRC) wykryje, że osoby nieupoważnione lub działające niezgodnie z prawem uzyskały dostęp do Twoich danych klienta. Aby uzyskać więcej informacji na temat sposobu włączania tego zalecenia, przeczytaj [Provide security contact details in Azure Security Center](security-center-provide-security-contact-details.md) (Wprowadzanie danych kontaktowych na potrzeby zabezpieczeń w usłudze Azure Security Center).

<a id="data-collection-and-storage" class="xliff"></a>

## Zbieranie i przechowywanie danych
Do zbierania danych zabezpieczeń z maszyn wirtualnych usługa Azure Security Center korzysta z programu Microsoft Monitoring Agent — jest to ten sam agent, który jest używany przez pakiet Operations Management Suite i usługę Log Analytics. Dane zbierane z tego agenta będą przechowywane w obszarach roboczych usługi Log Analytics.

<a id="agent" class="xliff"></a>

### Agent

Po włączeniu funkcji zbierania danych w zasadach zabezpieczeń program Microsoft Monitoring Agent (dla systemu [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) lub [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)) zostanie automatycznie zainstalowany na wszystkich obsługiwanych maszynach wirtualnych platformy Azure. Program ten będzie również automatycznie instalowany na nowych maszynach wirtualnych.  Jeśli na maszynie wirtualnej jest już zainstalowany program Microsoft Monitoring Agent, usługa Azure Security Center będzie korzystać z zainstalowanego agenta. Proces agenta został tak zaprojektowany, aby był nieinwazyjny i miał minimalny wpływ na wydajność maszyny wirtualnej.

Program Microsoft Monitoring Agent dla systemu Windows wymaga użycia portu TCP 443. Przeczytaj [artykuł na temat rozwiązywania problemów](security-center-troubleshooting-guide.md), aby uzyskać więcej informacji.

Jeśli w dowolnym momencie chcesz wyłączyć funkcję zbierania danych, możesz to zrobić w zasadach zabezpieczeń. Jednak ze względu na to, że program Microsoft Monitoring Agent może być używany przez inne usługi zarządzania i monitorowania platformy Azure, agent ten nie zostanie odinstalowany automatycznie po wyłączeniu zbierania danych w usłudze Security Center. W razie potrzeby można odinstalować agenta ręcznie.

> [!NOTE]
> Lista obsługiwanych maszyn wirtualnych znajduje się w artykule [Azure Security Center frequently asked questions (FAQ)](security-center-faq.md) (Azure Security Center — często zadawane pytania [FAQ]).
> 

<a id="workspace" class="xliff"></a>

### Obszar roboczy

Dane zbierane z programu Microsoft Monitoring Agent (w imieniu usługi Security Center) będą przechowywane w istniejących obszarach roboczych usługi Log Analytics skojarzonych z subskrypcją platformy Azure lub w nowych obszarach roboczych, uwzględniając lokalizację geograficzną maszyny wirtualnej. 

W witrynie Azure Portal można wyświetlić listę swoich obszarów roboczych usługi Log Analytics, w tym wszystkich utworzonych przez usługę Azure Security Center. W przypadku nowych obszarów roboczych zostanie utworzona powiązana grupa zasobów. W obu przypadkach stosowana będzie następująca konwencja nazewnictwa: 

* Obszar roboczy: *DefaultWorkspace-[identyfikator-subskrypcji]-[lokalizacja-geograficzna]*
* Grupa zasobów: *DefaultResouceGroup-[lokalizacja-geograficzna]*

W przypadku obszarów roboczych utworzonych przez usługę Azure Security Center dane są przechowywane przez 30 dni. W przypadku istniejących obszarów roboczych przechowywanie zależy od warstwy cenowej obszaru roboczego.

> [!NOTE]
> Firma Microsoft zobowiązuje się chronić poufność i bezpieczeństwo tych danych. Firma Microsoft przestrzega surowych wymogów z zakresu zabezpieczeń i zgodności — od kodu po działanie usługi. Aby uzyskać więcej informacji na temat obsługi danych i poufności, należy przeczytać artykuł [Azure Security Center — bezpieczeństwo danych](security-center-data-security.md).
> 

<a id="ongoing-security-monitoring" class="xliff"></a>

## Bieżące monitorowanie zabezpieczeń
Po wstępnej konfiguracji i zastosowaniu zaleceń usługi Security Center następny krok polega na uwzględnieniu procesów operacyjnych usługi Security Center.

Aby uzyskać dostęp do usługi Security Center za pomocą witryny Azure Portal, kliknij przycisk **Przeglądaj** i wpisz **Security Center** w polu **Filtruj**. Widoki, które uzyskuje użytkownik, są oparte na zastosowanych filtrach. Poniższy przykład przedstawia środowisko, w którym należy rozwiązać wiele problemów:

![pulpit nawigacyjny](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig6.png)

> [!NOTE]
> Usługa Security Center nie zakłóca zwykłych procedur operacyjnych, pasywnie monitoruje wdrożenia i zapewnia zalecenia na podstawie włączonych zasad zabezpieczeń.

Po podjęciu decyzji o pierwszym użyciu usługi Security Center dla bieżącego środowiska platformy Azure pamiętaj, aby przejrzeć wszystkie zalecenia. Można to zrobić na kafelku **Zalecenia** lub względem poszczególnych zasobów (**Obliczenia**, **Sieci**, **Magazyn i dane** oraz **Aplikacja**).

Po wykonaniu wszystkich zaleceń sekcja **Zapobieganie** powinna być zielona dla wszystkich przejrzanych zasobów. Na tym etapie monitorowanie ciągłe staje się łatwiejsze, ponieważ działania są podejmowane tylko w oparciu o zmiany kondycji zabezpieczeń zasobów oraz kafelki zaleceń.

Sekcja **Wykrywanie** jest bardziej reaktywna. Zawiera ona alerty dotyczące problemów, które występują teraz lub wystąpiły w przeszłości, wykrytych przez kontrolki usługi Security Center i systemy innych firm. W kafelku Alerty są wyświetlane wykresy słupkowe zawierające liczbę alertów wykrywania zagrożeń zidentyfikowanych w poszczególnych dniach oraz przedstawiające ich rozkład według różnych kategorii ważności (niskiej, średniej, wysokiej). Więcej informacji na temat alertów zabezpieczeń znajduje się w artykule [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> W celu wizualizacji danych usługi Security Center można też skorzystać z usługi Microsoft Power BI. Przeczytaj artykuł [Get insights from Azure Security Center data with Power BI](security-center-powerbi.md) (Uzyskiwanie szczegółowych informacji na podstawie danych z Centrum zabezpieczeń Azure za pomocą usługi Power BI).
> 
> 

<a id="monitoring-for-new-or-changed-resources" class="xliff"></a>

### Monitorowanie nowych lub zmodyfikowanych zasobów
Większość środowisk Azure jest dynamiczna. Systematycznie tworzone są nowe zasoby i usuwane stare, zmieniają się konfiguracje itd. Usługa Security Center pomaga zagwarantować widoczność stanu zabezpieczeń nowych zasobów.

Po dodaniu nowych zasobów (maszyn wirtualnych, baz danych SQL) do środowiska Azure usługa Security Center wykrywa je automatycznie i rozpoczyna monitorowanie ich zabezpieczeń. Obejmuje to także role procesu roboczego i role sieci Web usługi PaaS. Jeśli w [zasadach zabezpieczeń](security-center-policies.md) włączono funkcję zbierania danych, dla maszyn wirtualnych zostaną automatycznie włączone dodatkowe funkcje monitorowania.

![Kluczowe obszary](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-newUI.png)

1. Dla maszyn wirtualnych kliknij pozycję **Obliczenia** w obszarze **Zapobieganie**. Wszystkie problemy z włączaniem danych lub powiązane zalecenia zostaną wyświetlone na karcie **Omówienie** i w sekcji **Zalecenia dotyczące monitorowania**.
2. Wyświetl **zalecenia**, aby sprawdzić, jakie zagrożenia dla bezpieczeństwa, jeśli w ogóle wystąpiły, zostały zidentyfikowane dla nowego zasobu.
3. Często zdarza się, że podczas dodawania do środowiska nowych maszyn wirtualnych na początku zostaje zainstalowany tylko system operacyjny. Właściciel zasobu może potrzebować trochę czasu, aby wdrożyć inne aplikacje, które będą używane przez te maszyny wirtualne.  Najlepiej byłoby znać stan docelowy danego obciążenia. Czy będzie to serwer aplikacji? W zależności od tego, jaką rolę będzie pełnić nowe obciążenie, możesz włączyć odpowiednie **zasady zabezpieczeń**, co stanowi trzeci krok w tym przepływie pracy.
4. Ze względu na to, że do środowiska Azure dodawane są nowe zasoby, w kafelku **Alerty zabezpieczeń** mogą zostać wyświetlone nowe alerty. Zawsze sprawdzaj, czy kafelek zawiera nowe alerty, i podejmuj działania zgodnie z zaleceniami usługi Security Center.

Warto również regularnie monitorować stan istniejących zasobów, aby móc identyfikować zmiany konfiguracji, które spowodowały zagrożenia bezpieczeństwa, odejście od zalecanych planów bazowych oraz alerty zabezpieczeń. Rozpocznij na pulpicie nawigacyjnym usługi Security Center. Pulpit ma trzy główne obszary, które umożliwiają spójne przeglądanie.

![Operacje](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4-newUI.png)

1. Panel **Zapobieganie** zapewnia szybki dostęp do kluczowych zasobów. Opcja ta służy do monitorowania zasobów Obliczenia, Sieci, Magazyn i dane oraz Aplikacje.
2. Panel **Zalecenia** umożliwia przeglądanie zaleceń usługi Security Center. Podczas ciągłego monitorowania może się okazać, że zalecenia nie są wydawane codziennie, co jest zrozumiałe, biorąc pod uwagę, że wszystkie zalecenia zostały zastosowane podczas wstępnej konfiguracji usługi Security Center. Z tego powodu nowe informacje w tej sekcji mogą nie występować codziennie i trzeba uzyskiwać do nich dostęp w zależności od potrzeb.
3. Na panelu **Wykrywanie** zmiany mogą pojawiać się bardzo często lub bardzo rzadko. Zawsze czytaj alerty zabezpieczeń i podejmuj działania na podstawie zaleceń usługi Security Center.

<a id="incident-response" class="xliff"></a>

## Reagowanie na zdarzenia
Usługa Security Center wykrywa zagrożenia i powiadamia o nich, gdy tylko wystąpią. Firmy powinny śledzić pojawianie się nowych alertów zabezpieczeń i w razie potrzeby podejmować działania w celu zbadania ataku lub usunięcia jego skutków. Aby uzyskać więcej informacji na temat sposobu działania funkcji wykrywania zagrożeń usługi Security Center, przeczytaj [Funkcje wykrywania usługi Azure Security Center](security-center-detection-capabilities.md).

Ponieważ celem tego artykułu nie jest pomoc w tworzeniu Twojego planu reagowania na zdarzenia, jako podstawy dla etapów reagowania na zdarzenia użyjemy funkcji Microsoft Azure Security Response w cyklu życia chmury. Poszczególne etapy przedstawiono na poniższym diagramie:

![Podejrzane działania](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> W tworzeniu takiego planu może pomóc [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) (Przewodnik obsługi zdarzeń zabezpieczeń komputera) Narodowego Instytutu Norm i Technologii (NIST).
> 

Na poniższych etapach możesz używać alertów usługi Security Center:

* **Wykrywanie**: zidentyfikuj podejrzane działanie w co najmniej jednym zasobie. 
* **Ocenianie**: wykonaj wstępną ocenę, aby uzyskać więcej informacji na temat podejrzanego działania.
* **Diagnozowanie**: zastosuj czynności zaradcze, aby przeprowadzić procedurę techniczną rozwiązania problemu.

Każdy alert zabezpieczeń zawiera informacje, które mogą ułatwić lepsze zrozumienie natury ataku i zasugerować możliwe ograniczenie jego skutków. Niektóre alerty zapewniają również linki do większej ilości informacji lub do innych źródeł informacji w ramach platformy Azure. Dostarczonych informacji możesz użyć do dalszej analizy i rozpoczęcia rozwiązywania problemu. Możliwe jest również wyszukiwanie danych związanych z zabezpieczeniami przechowywanych w obszarze roboczym.

W poniższym przykładzie pokazano podejrzane działanie w protokole RDP:

![Podejrzane działania](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Jak widać, blok ten zawiera godzinę, o której nastąpił atak, nazwę hosta źródłowego, docelową maszynę wirtualną oraz poszczególne kroki zaleceń. W niektórych sytuacjach pola informacji źródłowych ataku mogą być puste. Więcej informacji na temat działania tego typu znajduje się w artykule [Missing Source Information in Azure Security Center Alerts](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/) (Brakujące informacje źródłowe w alertach Centrum zabezpieczeń Azure).

Na filmie [How to Leverage the Azure Security Center & Microsoft Operations Management Suite for an Incident Response](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) (Jak wykorzystać usługę Azure Security Center i pakiet Microsoft Operations Management Suite do reagowania na zdarzenia) możesz obejrzeć kilka prezentacji, które pomogą Ci zrozumieć, jak możesz wykorzystać usługę Security Center na każdym z tych etapów.

> [!NOTE]
> Aby uzyskać więcej informacji na temat korzystania z możliwości usługi Security Center w procesie reagowania na zdarzenia, zobacz [Korzystanie z usługi Azure Security Center w celu reagowania na zdarzenia](security-center-incident-response.md). 
> 
> 

<a id="see-also" class="xliff"></a>

## Zobacz też
W tym dokumencie omówiono, jak zaplanować wykorzystanie usługi Security Center. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure.
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.


