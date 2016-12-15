---
title: "Przewodnik planowania i obsługi usługi Security Center | Microsoft Docs"
description: "Ten dokument ułatwia planowanie przed wdrożeniem Centrum zabezpieczeń Azure oraz zawiera informacje o jego codziennych operacjach."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: f984e4a2-ac97-40bf-b281-2f7f473494c4
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 71fdf71d48959856697603c765e299f8a40b089e


---
# <a name="azure-security-center-planning-and-operations-guide"></a>Przewodnik planowania i obsługi usługi Azure Security Center
Ten przewodnik jest przeznaczony dla specjalistów IT, architektów IT, analityków zabezpieczeń informacji i administratorów chmury, których organizacje planują wdrożenie usługa Azure Security Center.

## <a name="planning-guide"></a>Przewodnik planowania
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
> 

## <a name="security-roles-and-access-controls"></a>Role zabezpieczeń i kontrola dostępu
W zależności od rozmiaru i struktury organizacji wiele osób oraz zespołów może korzystać z Centrum zabezpieczeń, aby wykonywać różne zadania związane z zabezpieczeniami. Poniższy diagram przedstawia przykład fikcyjnych osób oraz ich ról i obowiązków związanych z zabezpieczeniami:

![Role](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-ga.png)

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
* Rozwiązuje alerty lub współpracuje z właścicielem obciążenia chmury w celu zastosowania rozwiązania 

Usługa Security Center używa [kontroli dostępu opartej na rolach](../active-directory/role-based-access-control-configure.md) udostępniającej [wbudowane role](../active-directory/role-based-access-built-in-roles.md), które można przypisać do użytkowników, grup i usług na platformie Azure. Gdy użytkownik otwiera usługę Security Center, widzi tylko informacje dotyczące zasobów, do których ma dostęp. Oznacza to, że użytkownik ma przypisaną rolę właściciela, współautora lub czytelnika subskrypcji albo grupy zasobów, do której należy zasób. 

> [!NOTE]
> Użytkownik musi posiadać subskrypcję, być właścicielem grupy zasobów lub współautorem, aby mógł korzystać z usługi Security Center na platformie Azure.
> 
> 

W przypadku osób opisanych na poprzednim diagramie będzie potrzebna następująca kontrola dostępu oparta na rolach:

**Jan (właściciel obciążenia chmury)**

* Właściciel/Współautor grupy zasobów

**Daniel (zabezpieczenia informatyczne)**

* Właściciel/Współautor subskrypcji

**Magda (operacje zabezpieczeń)**

* Czytelnik subskrypcji przeglądający alerty
* Właściciel/Współautor subskrypcji, który musi odrzucać alerty

**Stanisław (analityk zabezpieczeń)**

* Czytelnik subskrypcji przeglądający alerty
* Właściciel/Współautor subskrypcji, który musi rozwiązywać lub odrzucać alerty
* Może być wymagany dostęp do magazynu

Niektóre inne istotne informacje, które należy wziąć pod uwagę:

* Tylko właściciele i współautorzy subskrypcji mogą edytować zasady zabezpieczeń
* Tylko właściciele i współautorzy subskrypcji i grupy zasobów mogą stosować zalecenia dotyczące zabezpieczeń zasobu

Podczas planowania kontroli dostępu przy użyciu kontroli dostępu opartej na rolach dla usługi Security Center musisz wiedzieć, kto w Twojej organizacji będzie korzystał z usługi. Ponadto musisz wiedzieć, jakiego rodzaju zadania będą wykonywały te osoby, a następnie odpowiednio skonfigurować kontrolę dostępu opartą na rolach.

> [!NOTE]
> Zaleca się przypisanie użytkownikom najbardziej ograniczonej roli wystarczającej do wykonywania zadań. Na przykład użytkownikom, którzy muszą tylko przeglądać informacje o stanie zabezpieczeń zasobów, ale nie muszą podejmować działań, np. stosować zaleceń ani edytować zasad, należy przypisać rolę czytelnika.
> 
> 

## <a name="security-policies-and-recommendations"></a>Zasady zabezpieczeń i zalecenia w tym zakresie
Zasady zabezpieczeń określają zestaw mechanizmów kontrolnych, które są zalecane dla zasobów w określonej subskrypcji lub grupie zasobów. W usłudze Security Center możesz zdefiniować zasady zgodnie z wymaganiami w zakresie zabezpieczeń firmy oraz typem aplikacji lub stopniem poufności danych.

Zasady włączone na poziomie subskrypcji są automatycznie propagowane wśród wszystkich grup zasobów w ramach subskrypcji, jak pokazano na poniższym diagramie:

![Zasady zabezpieczeń](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-ga.png)

Jak pokazano na poprzednim rysunku, zasady zabezpieczeń dla grup zasobów mogą być dziedziczone z poziomu subskrypcji.

W niektórych scenariuszach, gdzie w grupie zasobów mogą znajdować się zasoby wymagające innego zestawu zasad, można wyłączyć funkcję dziedziczenia i zastosować do danej grupy zasobów zasady niestandardowe.

Jeśli w określonych grupach zasobów potrzebne są zasady niestandardowe, należy wyłączyć funkcję dziedziczenia w grupie zasobów i zmienić zasady zabezpieczeń. Na przykład w przypadku występowania obciążeń, które nie wymagają zasad SQL Transparent Data Encryption, wyłącz zasady na poziomie subskrypcji i włącz je tylko w grupach zasobów, które wymagają funkcji SQL TDE.

Po rozpoczęciu tworzenia zasad niestandardowych dla różnych grup zasobów należy zaplanować wdrażanie zasad, pamiętając, że w przypadku konfliktu zasad (między subskrypcją i grupą zasobów) obowiązują zasady grupy zasobów.

> [!NOTE]
> Jeśli musisz sprawdzić, które zasady zostały zmienione, możesz użyć [Dzienników inspekcji Azure](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). Zmiany zasad są zawsze rejestrowane w Dziennikach inspekcji Azure.
> 
> 

### <a name="security-recommendations"></a>Zalecenia dotyczące zabezpieczeń
Przed skonfigurowaniem zasad zabezpieczeń przejrzyj poszczególne [zalecenia dotyczące zabezpieczeń](security-center-recommendations.md) i określ, czy te zasady są właściwe dla różnych subskrypcji i grup zasobów. Ważne jest również, aby zrozumieć, jakie działania są podejmowane w celu wypełnienia zaleceń dotyczących zabezpieczeń.

**Ochrona punktów końcowych**: Jeśli maszyna wirtualna nie ma włączonej funkcji ochrony punktów końcowych, usługa Security Center zaleca jej zainstalowanie. Jeśli używasz preferowanej funkcji ochrony punktów końcowych, która została już wdrożona lokalnie, musisz zdecydować, czy będziesz używać tej samej ochrony przed złośliwym oprogramowaniem na maszynach wirtualnych platformy Azure. Usługa Security Center zapewnia kilka opcji ochrony punktów końcowych.  Można użyć bezpłatnego oprogramowania firmy Microsoft chroniącego przed złośliwym kodem lub wybrać oprogramowanie z listy rozwiązań ochrony punktów końcowych powiązanych partnerów. Aby uzyskać więcej informacji na temat wdrażania oprogramowania chroniącego przed złośliwym kodem przy użyciu usługi Security Center, przeczytaj artykuł [Install Endpoint Protection in Azure Security Center](security-center-install-endpoint-protection.md) (Instalowanie ochrony punktów końcowych w usłudze Azure Security Center).

**Aktualizacje systemu**: Usługa Security Center znajduje maszyny wirtualne, na których brakuje aktualizacji zabezpieczeń lub aktualizacji krytycznych systemu operacyjnego dotyczących usługi IaaS i Cloud Services (PaaS). Zastanów się, kto odpowiada za stosowanie aktualizacji w razie potrzeby i jak są one stosowane. Wiele firm korzysta z usług WSUS, Windows Update lub innych narzędzi.

**Konfiguracje podstawowe**: Jeśli konfiguracje systemu operacyjnego maszyny wirtualnej nie są zgodne z zalecanymi planami bazowymi, jest wydawane zalecenie. Przejrzyj zestaw planów bazowych dostępny [tutaj](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) i zastanów się nad sposobem stosowania konfiguracji systemu operacyjnego.

**Szyfrowanie dysku**: Jeśli masz dyski maszyny wirtualnej, które nie są szyfrowane, usługa Security Center zaleca zastosowanie usługi Azure Disk Encryption. Ta funkcja korzysta z funkcji BitLocker dla systemu Windows oraz funkcji DM-Crypt dla systemu Linux, aby zapewnić szyfrowanie woluminów dysków systemu operacyjnego i danych. To zalecenie przekierowuje do [przewodnika krok po kroku](security-center-disk-encryption.md) zawierającego instrukcje dotyczące wykonania szyfrowania.

Należy pamiętać, że istnieje kilka scenariuszy szyfrowania wymagających wdrożenia. Dla każdego z tych scenariuszy należy zaplanować unikatowe wymagania:

* Szyfrowanie nowych maszyn wirtualnych Azure z wirtualnych dysków twardych, które zostały zaszyfrowane za pomocą własnych kluczy szyfrowania
* Szyfrowanie nowych maszyn wirtualnych Azure utworzonych za pomocą galerii Azure
* Szyfrowanie maszyn wirtualnych Azure, które zostały już uruchomione na platformie Azure

Wymagania dotyczące planowania będą inne dla każdego z tych scenariuszy. Szczegóły dotyczące poszczególnych scenariuszy znajdują się w [oficjalnym dokumencie dotyczącym szyfrowania dysków Azure](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

**Zapora aplikacji sieci Web**: Usługa Service Center zidentyfikuje maszyny wirtualne, na których zostały uruchomione aplikacje sieci Web, i zaleci zainstalowanie zapory aplikacji sieci Web. Oceń dostępne rozwiązania partnerów, aby ustalić, które jest najbardziej odpowiednie dla firmy, i jak będzie ono licencjonowane (partnerzy mogą obsługiwać modele Bring Your Own License lub Pay As You Go). Więcej informacji na temat wdrażania zapory aplikacji sieci Web na maszynach wirtualnych platformy Azure za pomocą usługi Security Center znajduje się w artykule [Add a web application firewall in Azure Security Center](security-center-add-web-application-firewall.md) (Dodawanie zapory aplikacji sieci Web w usłudze Azure Security Center).

**Zapora nowej generacji**: Umożliwia udostępnianie urządzeń wirtualnych wiodących producentów, między innymi Check Point, a zaraz potem Cisco i Fortinet. Powoduje rozszerzenie ochrony sieci poza sieciowe grupy zabezpieczeń, które są wbudowane w platformę Azure. Centrum zabezpieczeń wykryje wdrożenia, dla których zaleca się zaporę nowej generacji, i pozwoli na aprowizację urządzenia wirtualnego.

**Sieci wirtualne**: Usługa Security Center ocenia infrastrukturę i konfigurację usługi [Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/), aby sprawdzić, czy [sieciowe grupy zabezpieczeń](../virtual-network/virtual-networks-nsg.md) są stosowane i poprawnie skonfigurowane za pomocą reguł ruchu przychodzącego. Należy się zastanowić, jakie reguły ruchu należy zdefiniować, i przekazać te informacje osobom, które będą stosować powiązane zalecenia dotyczące zabezpieczeń.

Usługa Security Center zaleci podanie szczegółowych informacji dotyczących kontaktu zabezpieczeń dla Twojej subskrypcji platformy Azure. Te informacje będą używane przez firmę Microsoft do kontaktowania się z Tobą, gdy centrum Microsoft Security Response Center (MSRC) wykryje, że osoby nieupoważnione lub działające niezgodnie z prawem uzyskały dostęp do Twoich danych klienta. Aby uzyskać więcej informacji na temat sposobu włączania tego zalecenia, przeczytaj [Provide security contact details in Azure Security Center](security-center-provide-security-contact-details.md) (Wprowadzanie danych kontaktowych na potrzeby zabezpieczeń w usłudze Azure Security Center).

## <a name="data-collection-and-storage"></a>Zbieranie i przechowywanie danych
Stanowczo zaleca się włączenie funkcji zbierania danych dla każdej subskrypcji, ponieważ pozwala to zagwarantować, że monitorowanie zabezpieczeń jest dostępne dla wszystkich maszyn wirtualnych. Zbieranie danych odbywa się za pośrednictwem agenta monitorowania Azure (ASMAgentLauncher.exe) i rozszerzenia monitorowania zabezpieczeń Azure (ASMMonitoringAgent.exe).

Rozszerzenie monitorowania zabezpieczeń Azure skanuje pod kątem różnych konfiguracji związanych z zabezpieczeniami i zbiera dzienniki zabezpieczeń z maszyny wirtualnej. Te dane są wysyłane na określone przez użytkownika konto magazynu. Menedżer skanowania (ASMSoftwareScanner.exe) zostanie również zainstalowany na maszynie wirtualnej i będzie używany jako skaner poprawek.

Agent monitorowania zabezpieczeń Azure (ASM) zawiera następujący plan bazowy:

* W przeważającej części 3 MB miejsca pamięci, a w przypadku potencjalnych wartości szczytowych 10 MB, jeśli scenariusz jest uruchamiany co 12 godzin.  
* Niewielkie wykorzystanie procesora zarówno ze strony procesu trwałego, jak i skanerów. 
* Niewielkie wykorzystanie dysku.

Agent monitorowania zabezpieczeń Azure zawiera łańcuch procesów, które mogą łącznie zająć około 30 MB pamięci.  Każde wystąpienie agenta monitorowania może zająć maksymalnie do 3 GB dysku. Każde wystąpienie zajmuje 20% procesora, mimo że w praktyce jest to znacznie mniej. 

Po włączeniu funkcji zbierania danych w zasadach zabezpieczeń agent monitorowania i rozszerzenia są instalowane automatycznie na wszystkich istniejących i nowych obsługiwanych maszynach wirtualnych aprowizowanych na platformie Azure.  Proces agenta został tak zaprojektowany, aby był nieinwazyjny i miał minimalny wpływ na wydajność maszyny wirtualnej.

> [!NOTE]
> W celu rozwiązania problemów związanych z Agentem monitorowania zabezpieczeń Azure przeczytaj [Przewodnik rozwiązywania problemów z usługą Azure Security Center](security-center-troubleshooting-guide.md).
> 
> 

Jeśli w dowolnym momencie chcesz wyłączyć funkcję zbierania danych, możesz to zrobić w zasadach zabezpieczeń. Aby usunąć wdrożone wcześniej agenty monitorowania, wybierz opcję menu Usuń agenty.

> [!NOTE]
> Lista obsługiwanych maszyn wirtualnych znajduje się w artykule [Azure Security Center frequently asked questions (FAQ)](security-center-faq.md) (Centrum zabezpieczeń Azure — często zadawane pytania [FAQ]).
> 
> 

Dla każdego regionu, w którym zostały uruchomione maszyny wirtualne, wybierz konto magazynu, w którym będą przechowywane dane z tych maszyn. Jeśli nie wybierzesz konta magazynu dla każdego regionu, zostanie ono utworzone automatycznie. Można wybrać lokalizację magazynu na region lub przechowywać wszystkie informacje w centralnej lokalizacji. Chociaż zasady zabezpieczeń można ustawić na poziomie subskrypcji i grupy zasobów Azure, region dla konta magazynu można wybrać tylko na poziomie subskrypcji.

Jeśli używasz konta magazynu współdzielonego między różnymi zasobami Azure, pamiętaj, aby przeczytać artykuł [Azure Storage Scalability and Performance Targets](../storage/storage-scalability-targets.md) (Skalowalność i cele wydajności usługi Azure Storage), by uzyskać więcej informacji na temat limitów rozmiarów i ograniczeń. Subskrypcja ma również limity konta magazynu. Aby zapoznać się z tymi limitami, przejrzyj artykuł [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md) (Limity, limity przydziału i ograniczenia subskrypcji i usługi Azure).

Koszty związane z magazynem nie są uwzględnione w cenie usługi Security Center i będą naliczane osobno według zwykłych [stawek obowiązujących dla usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Z perspektywy planowania należy zdawać sobie sprawę, że usługa Security Center doda dane do usługi Azure Storage, co może zwiększyć koszty od 1 do 3 dolarów rocznie.

Zagadnienia dotyczące wydajności i skalowalności powinny być również planowane w oparciu o rozmiar środowiska Azure oraz zasoby korzystające z konta magazynu. Więcej informacji znajduje się w artykule [Microsoft Azure Storage Performance and Scalability Checklist](../storage/storage-performance-checklist.md) (Lista kontrolna wydajności i skalowalności usługi Microsoft Azure Storage).

> [!NOTE]
> Firma Microsoft zobowiązuje się chronić poufność i bezpieczeństwo tych danych. Firma Microsoft przestrzega surowych wymogów z zakresu zabezpieczeń i zgodności — od kodu po działanie usługi. Aby uzyskać więcej informacji na temat obsługi danych i poufności, należy przeczytać artykuł [Azure Security Center — bezpieczeństwo danych](security-center-data-security.md).
> 
> 

## <a name="ongoing-security-monitoring"></a>Bieżące monitorowanie zabezpieczeń
Po wstępnej konfiguracji i zastosowaniu zaleceń usługi Security Center następny krok polega na uwzględnieniu procesów operacyjnych usługi Security Center.

Aby uzyskać dostęp do usługi Security Center za pomocą witryny Azure Portal, kliknij przycisk **Przeglądaj** i wpisz **Security Center** w polu **Filtruj**. Widoki, które uzyskuje użytkownik, są oparte na zastosowanych filtrach.

Usługa Security Center nie zakłóca zwykłych procedur operacyjnych, pasywnie monitoruje wdrożenia i zapewnia zalecenia na podstawie włączonych zasad zabezpieczeń.

Pulpit nawigacyjny usługi Security Center jest podzielony na dwie główne części:

* Zapobieganie
* Wykrywanie

Po pierwszym włączeniu funkcji zbierania danych w usłudze Security Center dla bieżącego środowiska platformy Azure pamiętaj, aby przejrzeć wszystkie zalecenia. Możesz to zrobić w bloku **Zalecenia** lub względem poszczególnych zasobów (**Maszyny wirtualne**, **Sieci**, **SQL** i **Aplikacja**).

Po wykonaniu wszystkich zaleceń sekcja **Zapobieganie** powinna być zielona dla wszystkich przejrzanych zasobów. Na tym etapie monitorowanie ciągłe staje się łatwiejsze, ponieważ działania są podejmowane tylko w oparciu o zmiany kondycji zabezpieczeń zasobów oraz kafelki zaleceń.

Sekcja **Wykrywanie** jest bardziej reaktywna. Zawiera ona alerty dotyczące problemów, które występują teraz lub wystąpiły w przeszłości, wykrytych przez kontrolki usługi Security Center i systemy innych firm. W kafelku Alerty są wyświetlane wykresy słupkowe zawierające liczbę alertów wykrywania zagrożeń zidentyfikowanych w poszczególnych dniach oraz przedstawiające ich rozkład według różnych kategorii ważności (niskiej, średniej, wysokiej). Więcej informacji na temat alertów zabezpieczeń znajduje się w artykule [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> W celu wizualizacji danych usługi Security Center można też skorzystać z usługi Microsoft Power BI. Przeczytaj artykuł [Get insights from Azure Security Center data with Power BI](security-center-powerbi.md) (Uzyskiwanie szczegółowych informacji na podstawie danych z Centrum zabezpieczeń Azure za pomocą usługi Power BI).
> 
> 

### <a name="monitoring-for-new-or-changed-resources"></a>Monitorowanie nowych lub zmodyfikowanych zasobów
Większość środowisk Azure jest dynamiczna. Systematycznie tworzone są nowe zasoby i usuwane stare, zmieniają się konfiguracje itd. Usługa Security Center pomaga zagwarantować widoczność stanu zabezpieczeń nowych zasobów.

Po dodaniu nowych zasobów (maszyn wirtualnych, baz danych SQL) do środowiska Azure usługa Security Center wykrywa je automatycznie i rozpoczyna monitorowanie ich zabezpieczeń. Obejmuje to także role procesu roboczego i role sieci Web usługi PaaS. Jeśli w [zasadach zabezpieczeń](security-center-policies.md) włączono funkcję zbierania danych, dla maszyn wirtualnych zostaną automatycznie włączone dodatkowe funkcje monitorowania.

![Kluczowe obszary](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-ga.png)

1. W przypadku maszyn wirtualnych uzyskaj dostęp do kafelka **Kondycja zabezpieczeń zasobów** i kliknij opcję **Maszyny wirtualne**. Wszystkie problemy z włączaniem zbierania danych lub powiązanymi zaleceniami zostaną wyświetlone w sekcji **Zalecenia dotyczące monitorowania**.
2. Wyświetl **zalecenia**, aby sprawdzić, jakie zagrożenia dla bezpieczeństwa, jeśli w ogóle wystąpiły, zostały zidentyfikowane dla nowego zasobu.
3. Często zdarza się, że podczas dodawania do środowiska nowych maszyn wirtualnych na początku zostaje zainstalowany tylko system operacyjny. Właściciel zasobu może potrzebować trochę czasu, aby wdrożyć inne aplikacje, które będą używane przez te maszyny wirtualne.  Najlepiej byłoby znać stan docelowy danego obciążenia. Czy będzie to serwer aplikacji? W zależności od tego, jaką rolę będzie pełnić nowe obciążenie, możesz włączyć odpowiednie **zasady zabezpieczeń**, co stanowi trzeci krok w tym przepływie pracy.
4. Ze względu na to, że do środowiska Azure dodawane są nowe zasoby, w kafelku **Alerty zabezpieczeń** mogą zostać wyświetlone nowe alerty. Zawsze sprawdzaj, czy kafelek zawiera nowe alerty, i podejmuj działania zgodnie z zaleceniami usługi Security Center.

Warto również regularnie monitorować stan istniejących zasobów, aby móc identyfikować zmiany konfiguracji, które spowodowały zagrożenia bezpieczeństwa, odejście od zalecanych planów bazowych oraz alerty zabezpieczeń. Rozpocznij na pulpicie nawigacyjnym usługi Security Center. Pulpit ma trzy główne obszary, które umożliwiają spójne przeglądanie.

![Operacje](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4.png)

1. Panel **Kondycja zabezpieczeń zasobów** zapewnia szybki dostęp do kluczowych zasobów. Opcja ta służy do monitorowania maszyn wirtualnych, sieci, SQL i aplikacji.
2. Panel **Zalecenia** umożliwia przeglądanie zaleceń usługi Security Center. Podczas ciągłego monitorowania może się okazać, że zalecenia nie są wydawane codziennie, co jest zrozumiałe, biorąc pod uwagę, że wszystkie zalecenia zostały zastosowane podczas wstępnej konfiguracji usługi Security Center. Z tego powodu nowe informacje w tej sekcji mogą nie występować codziennie i trzeba uzyskiwać do nich dostęp w zależności od potrzeb.
3. W panelu **Wykrywanie** zmiany mogą pojawiać się bardzo często lub bardzo rzadko. Zawsze czytaj alerty zabezpieczeń i podejmuj działania na podstawie zaleceń usługi Security Center.

## <a name="incident-response"></a>Reagowanie na zdarzenia
Usługa Security Center wykrywa zagrożenia i powiadamia o nich, gdy tylko wystąpią. Firmy powinny śledzić pojawianie się nowych alertów zabezpieczeń i w razie potrzeby podejmować działania w celu zbadania ataku lub usunięcia jego skutków. Aby uzyskać więcej informacji na temat sposobu działania funkcji wykrywania zagrożeń usługi Security Center, przeczytaj [Funkcje wykrywania usługi Azure Security Center](security-center-detection-capabilities.md).

Ponieważ celem tego artykułu nie jest pomoc w tworzeniu Twojego planu reagowania na zdarzenia, jako podstawy dla etapów reagowania na zdarzenia użyjemy funkcji Microsoft Azure Security Response w cyklu życia chmury. Poszczególne etapy przedstawiono na poniższym diagramie:

![Podejrzane działania](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> W tworzeniu takiego planu może pomóc [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) (Przewodnik obsługi zdarzeń zabezpieczeń komputera) Narodowego Instytutu Norm i Technologii (NIST).
> 
> 

Na poniższych etapach możesz używać alertów usługi Security Center:

* **Wykrywanie**: zidentyfikuj podejrzane działanie w co najmniej jednym zasobie. 
* **Ocenianie**: wykonaj wstępną ocenę, aby uzyskać więcej informacji na temat podejrzanego działania.
* **Diagnozowanie**: zastosuj czynności zaradcze, aby przeprowadzić procedurę techniczną rozwiązania problemu.

Każdy alert zabezpieczeń zawiera informacje, które mogą ułatwić lepsze zrozumienie natury ataku i zasugerować możliwe ograniczenie jego skutków. Niektóre alerty zapewniają również linki do większej ilości informacji lub do innych źródeł informacji w ramach platformy Azure. Możesz użyć dostarczonych informacji do dalszej analizy ataku lub w celu rozpoczęcia ograniczenia jego skutków.

W poniższym przykładzie pokazano podejrzane działanie w protokole RDP:

![Podejrzane działania](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Jak widać, blok ten zawiera godzinę, o której nastąpił atak, nazwę hosta źródłowego, docelową maszynę wirtualną oraz poszczególne kroki zaleceń. W niektórych sytuacjach pola informacji źródłowych ataku mogą być puste. Więcej informacji na temat działania tego typu znajduje się w artykule [Missing Source Information in Azure Security Center Alerts](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/) (Brakujące informacje źródłowe w alertach Centrum zabezpieczeń Azure).

Na filmie [How to Leverage the Azure Security Center & Microsoft Operations Management Suite for an Incident Response](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) (Jak wykorzystać usługę Azure Security Center i pakiet Microsoft Operations Management Suite do reagowania na zdarzenia) możesz obejrzeć kilka prezentacji, które pomogą Ci zrozumieć, jak możesz wykorzystać usługę Security Center na każdym z tych etapów.

> [!NOTE]
> Aby uzyskać więcej informacji na temat korzystania z możliwości usługi Security Center w procesie reagowania na zdarzenia, zobacz [Korzystanie z usługi Azure Security Center w celu reagowania na zdarzenia](security-center-incident-response.md). 
> 
> 

## <a name="see-also"></a>Zobacz też
W tym dokumencie omówiono, jak zaplanować wykorzystanie usługi Security Center. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure.
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.




<!--HONumber=Dec16_HO1-->


