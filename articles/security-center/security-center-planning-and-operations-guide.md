<properties
   pageTitle="Przewodnik planowania i obsługi Centrum zabezpieczeń| Microsoft Azure"
   description="Ten dokument ułatwia planowanie przed wdrożeniem Centrum zabezpieczeń Azure oraz zawiera informacje o jego codziennych operacjach."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/25/2016"
   ms.author="yurid"/>
 
# Przewodnik planowania i obsługi Centrum zabezpieczeń Azure
Ten przewodnik jest przeznaczony dla specjalistów IT, architektów IT, analityków zabezpieczeń informacji i administratorów chmury, których organizacje planują wdrożenie Centrum zabezpieczeń Azure.

> [AZURE.NOTE] Podane tu informacje odnoszą się do wersji zapoznawczej Centrum zabezpieczeń Azure.

## Omówienie Centrum zabezpieczeń Azure
Centrum zabezpieczeń Azure ułatwia zapobieganie zagrożeniom, ich wykrywanie i reagowanie na nie, a przy tym zapewnia lepszy wgląd i większą kontrolę w zakresie bezpieczeństwa zasobów na platformie Azure. Umożliwia zintegrowane monitorowanie zabezpieczeń i zarządzanie zasadami dla wszystkich subskrypcji, pomaga wykrywać zagrożenia, które w przeciwnym razie mogłyby pozostać niezauważone, a także współpracuje z szerokim ekosystemem rozwiązań zabezpieczających. 

Artykuł [Azure Security Center frequently asked questions (FAQ)](security-center-faq.md) (Centrum zabezpieczeń Azure — często zadawane pytania) zawiera listę często zadawanych pytań, która również może być przydatna w fazie projektowania i planowania.

## Przewodnik planowania
Ten przewodnik obejmuje zestaw kroków i zadań, które można wykonać, aby zoptymalizować korzystanie z Centrum zabezpieczeń Azure w zależności od wymagań dotyczących zabezpieczeń oraz modelu zarządzania chmurą w organizacji. Aby w pełni wykorzystać zalety Centrum zabezpieczeń Azure, ważne jest, aby zrozumieć, w jak różny sposób będą używać usługi poszczególne osoby i zespoły w organizacji, gdyż umożliwi to spełnienie wymagań związanych z bezpieczeństwem programowania i obsługi, monitorowaniem, zarządzaniem i reagowaniem na zdarzenia. Kluczowe obszary, które należy wziąć pod uwagę podczas planowania użycia Centrum zabezpieczeń Azure:

- Role zabezpieczeń i kontrola dostępu
- Zasady zabezpieczeń i zalecenia w tym zakresie
- Zbieranie i przechowywanie danych
- Bieżące monitorowanie zabezpieczeń 
- Reagowanie na zdarzenia

Z następnej sekcji dowiesz się, jak planować dla każdego z tych obszarów i stosować te zalecenia w zależności od wymagań. 

## Role zabezpieczeń i kontrola dostępu 

W zależności od rozmiaru i struktury organizacji wiele osób oraz zespołów może korzystać z Centrum zabezpieczeń, aby wykonywać różne zadania związane z zabezpieczeniami. Poniżej przestawiono przykład zawierający fikcyjne osoby oraz ich role i obowiązki związane z zabezpieczeniami:

![Role](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01.png)

Centrum zabezpieczeń Azure umożliwia tym osobom wypełnianie różnych obowiązków. Na przykład:

**Jan (właściciel obciążenia chmury)**

- Przegląda i wypełnia zalecenia Centrum zabezpieczeń w portalu Azure 
- Może również używać systemu tworzenia biletów, aby śledzić zmiany (wypełnia zalecenia za pomocą interfejsu API)

**Rafał (CISO/CIO)**

- Przegląda raporty Centrum zabezpieczeń przy użyciu programu Excel lub usługi Power BI

**Daniel (Zabezpieczenia informatyczne)**

- Ustawia zasady zabezpieczeń i sprawdza kondycję zabezpieczeń w portalu Azure
- Analizuje dane i generuje raporty w usłudze Power BI 

**Szymon (Operacje zabezpieczeń)**

- Przegląda i klasyfikuje alerty Centrum zabezpieczeń w portalu Azure 
- Może używać istniejącego pulpitu nawigacyjnego (wypełnia alerty za pomocą interfejsu API)

**Stanisław (Analityk zabezpieczeń)**

- Przegląda alerty Centrum zabezpieczeń w portalu Azure 
- Może używać istniejącego pulpitu nawigacyjnego (wypełnia alerty za pomocą interfejsu API)
- Analizuje trendy alertów w usłudze Power BI 
- Przegląda dzienniki zdarzeń z magazynu

Centrum zabezpieczeń Azure używa [kontroli dostępu opartej na rolach (RBAC)](../active-directory/role-based-access-control-configure.md), która zapewnia [wbudowane role](../active-directory/role-based-access-built-in-roles.md), które można przypisać do użytkowników, grup i usług Azure. Po otwarciu Centrum zabezpieczeń Azure użytkownik widzi tylko informacje związane z zasobami, do których ma dostęp, tzn. przypisaną rolę właściciela, współautora lub czytelnika subskrypcji lub grupy zasobów, do której należy dany zasób. W przypadku osób wymienionych powyżej potrzebne są następujące funkcje RBAC:

**Jan (właściciel obciążenia chmury)**

- Właściciel/Współautor grupy zasobów

**Daniel (Zabezpieczenia informatyczne)**

- Właściciel/Współautor subskrypcji

**Szymon (Operacje zabezpieczeń)**

- Czytelnik subskrypcji przeglądający alerty
- Właściciel/Współautor subskrypcji, który musi odrzucać alerty

**Stanisław (Analityk zabezpieczeń)**

- Czytelnik subskrypcji przeglądający alerty
- Właściciel/Współautor subskrypcji, który musi rozwiązywać lub odrzucać alerty
- Może być wymagany dostęp do magazynu

Niektóre inne istotne informacje, które należy wziąć pod uwagę:

- Tylko właściciele i współautorzy subskrypcji mogą edytować zasady zabezpieczeń
- Tylko właściciele i współautorzy subskrypcji i grupy zasobów mogą stosować zalecenia dotyczące zabezpieczeń zasobu

Podczas planowania kontroli dostępu przy użyciu funkcji RBAC dla Centrum zabezpieczeń Azure należy wiedzieć, kto w organizacji będzie używać Centrum i jakie typy zadań będzie wykonywać. Następnie skonfiguruj odpowiednio funkcję RBAC. 

> [AZURE.NOTE] Zaleca się przypisanie użytkownikom najbardziej ograniczonej roli wystarczającej do wykonywania zadań. Na przykład użytkownikom, którzy muszą tylko przeglądać informacje o stanie zabezpieczeń zasobów, ale nie muszą podejmować działań, np. stosować zaleceń ani edytować zasad, należy przypisać rolę czytelnika.

## Zasady zabezpieczeń i zalecenia w tym zakresie
Zasady zabezpieczeń określają zestaw mechanizmów kontrolnych, które są zalecane dla zasobów w określonej subskrypcji lub grupie zasobów. W Centrum zabezpieczeń Azure można zdefiniować zasady zgodnie z wymaganiami w zakresie zabezpieczeń firmy i typem aplikacji oraz stopnień poufności danych. 

Zasady włączone na poziomie subskrypcji zostaną automatycznie rozpropagowane wśród wszystkich grup zasobów w ramach subskrypcji, jak pokazano na poniższym schemacie: 

![Zasady zabezpieczeń](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2.png) 

Jak pokazano na rysunku powyżej, zasady zabezpieczeń dla grup zasobów mogą być dziedziczone z poziomu subskrypcji. 

W niektórych scenariuszach, gdzie w grupie zasobów mogą znajdować się zasoby wymagające innego zestawu zasad, można wyłączyć funkcję dziedziczenia i zastosować do danej grupy zasobów zasady niestandardowe. 

Jeśli w określonych grupach zasobów potrzebne są zasady niestandardowe, należy wyłączyć funkcję dziedziczenia w grupie zasobów i zmienić zasady zabezpieczeń. Na przykład w przypadku występowania obciążeń, które nie wymagają zasad SQL Transparent Data Encryption, wyłącz zasady na poziomie subskrypcji i włącz je tylko w grupach zasobów, które wymagają funkcji SQL TDE.
 
Po rozpoczęciu tworzenia zasad niestandardowych dla różnych grup zasobów należy zaplanować wdrażanie zasad, pamiętając, że w przypadku konfliktu zasad (między subskrypcją i grupą zasobów) obowiązują zasady grupy zasobów. 

> [AZURE.NOTE] Jeśli musisz sprawdzić, które zasady zostały zmienione, możesz użyć [Dzienników inspekcji Azure](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). Zmiany zasad są zawsze rejestrowane w Dziennikach inspekcji Azure.

### Zalecenia dotyczące zabezpieczeń

Przed skonfigurowaniem zasad zabezpieczeń należy przejrzeć poszczególne [zalecenia dotyczące zabezpieczeń](security-center-recommendations.md) i określić, czy są one właściwe dla różnych subskrypcji i grup zasobów. Ważne jest również, aby zrozumieć, jakie działania zostaną podjęte w celu wypełnienia zaleceń dotyczących zabezpieczeń.

**Ochrona punktów końcowych**: jeśli maszyna wirtualna nie ma włączonej funkcji ochrony punktów końcowych, Centrum zabezpieczeń Azure zaleca jej zainstalowanie. Jeśli używasz preferowanej funkcji ochrony punktów końcowych, która została już wdrożona lokalnie, musisz zdecydować, czy będziesz używać tej samej ochrony przed złośliwym oprogramowaniem w maszynach wirtualnych platformy Azure. Centrum zabezpieczeń Azure zapewnia kilka opcji ochrony punktów końcowych.  Można użyć bezpłatnego oprogramowania firmy Microsoft chroniącego przed złośliwym kodem lub wybrać oprogramowanie z listy rozwiązań ochrony punktów końcowych powiązanych partnerów. Aby uzyskać więcej informacji na temat wdrażania oprogramowania chroniącego przed złośliwym kodem przy użyciu Centrum zabezpieczeń Azure, przeczytaj artykuł [Enable antimalware in Azure Security Center](security-center-enable-antimalware.md) (Włączanie oprogramowania chroniącego przed złośliwym kodem w Centrum zabezpieczeń Azure).

**Aktualizacje systemu**: centrum zabezpieczeń Azure znajdzie maszyny wirtualne, na których brakuje zabezpieczeń lub krytycznych aktualizacji systemu operacyjnego. Należy się zastanowić, kto będzie odpowiadać za wdrażanie aktualizacji w razie potrzeby i jak będą one wdrażane. Wiele firm korzysta z usług WSUS, Windows Update lub innych narzędzi.

**Konfiguracje podstawowe**: jeśli konfiguracje systemu operacyjnego maszyny wirtualnej nie są zgodne z zalecanymi planami bazowymi, zostanie wydane zalecenie. Należy przejrzeć zestaw planów bazowych dostępny [tutaj](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) i zastanowić się nad sposobem stosowania konfiguracji systemu operacyjnego. 

**Szyfrowanie dysków**: jeśli masz niezaszyfrowane dyski maszyny wirtualnej, Centrum zabezpieczeń Azure zaleca zastosowanie Szyfrowania dysków Azure, które korzysta z funkcji BitLocker dla systemu Windows i DM-Crypt dla systemu Linux w celu zapewnienia szyfrowania woluminów systemu operacyjnego i dysków z danymi. Z tego zalecenia nastąpi przekierowanie do [przewodnika krok po kroku](security-center-disk-encryption.md) zawierającego instrukcje dotyczące wykonania szyfrowania. 

Należy pamiętać, że istnieje kilka scenariuszy szyfrowania wymagających wdrożenia. Dla każdego z tych scenariuszy należy zaplanować unikatowe wymagania:

- Szyfrowanie nowych maszyn wirtualnych Azure z wirtualnych dysków twardych, które zostały zaszyfrowane za pomocą własnych kluczy szyfrowania
- Szyfrowanie nowych maszyn wirtualnych Azure utworzonych za pomocą galerii Azure
- Szyfrowanie maszyn wirtualnych Azure, które zostały już uruchomione na platformie Azure

Wymagania dotyczące planowania będą inne dla każdego z tych scenariuszy. Szczegóły dotyczące poszczególnych scenariuszy znajdują się w [oficjalnym dokumencie dotyczącym szyfrowania dysków Azure](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

**Zapora aplikacji sieci Web**: Centrum zabezpieczeń Azure zidentyfikuje maszyny wirtualne, na których zostały uruchomione aplikacje sieci Web, i zaleci zainstalowanie zapory aplikacji sieci Web (WAF). Oceń dostępne rozwiązania partnerów, aby ustalić, które jest najbardziej odpowiednie dla firmy, i jak będzie ono licencjonowane (partnerzy mogą obsługiwać modele Bring Your Own License lub Pay As You Go). Więcej informacji na temat wdrażania zapory aplikacji sieci Web na maszynach wirtualnych platformy Azure za pomocą Centrum zabezpieczeń Azure znajduje się w artykule [Add a web application firewall in Azure Security Center](security-center-add-web-application-firewall.md) (Dodawanie zapory aplikacji sieci Web w Centrum zabezpieczeń Azure).

**Sieci wirtualne**: Centrum zabezpieczeń Azure będzie oceniać infrastrukturę i konfigurację usługi [Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/), aby sprawdzić, czy [grupy zabezpieczeń sieciowych](../virtual-network/virtual-networks-nsg.md) są stosowane i poprawnie skonfigurowane za pomocą reguł ruchu przychodzącego. Należy się zastanowić, jakie reguły ruchu należy zdefiniować, i przekazać te informacje osobom, które będą stosować powiązane zalecenia dotyczące zabezpieczeń.

## Zbieranie i przechowywanie danych

Stanowczo zaleca się włączenie funkcji zbierania danych dla każdej subskrypcji, ponieważ pozwala to zagwarantować, że monitorowanie zabezpieczeń jest dostępne dla wszystkich maszyn wirtualnych. Zbieranie danych odbywa się za pośrednictwem agenta monitorowania Azure (ASMAgentLauncher.exe) i rozszerzenia monitorowania zabezpieczeń Azure (ASMMonitoringAgent.exe). 

Rozszerzenie monitorowania zabezpieczeń Azure skanuje pod kątem różnych konfiguracji związanych z zabezpieczeniami i zbiera dzienniki zabezpieczeń z maszyny wirtualnej. Te dane są wysyłane na określone przez użytkownika konto magazynu. Menedżer skanowania (ASMSoftwareScanner.exe) zostanie również zainstalowany na maszynie wirtualnej i będzie używany jako skaner poprawek.

Po włączeniu funkcji zbierania danych w zasadach zabezpieczeń agent monitorowania i rozszerzenia są instalowane automatycznie na wszystkich istniejących i nowych obsługiwanych maszynach wirtualnych aprowizowanych na platformie Azure.  Proces agenta jest nieinwazyjny i nie ma wpływu na wydajność maszyny wirtualnej.
 
Jeśli w dowolnym momencie chcesz wyłączyć funkcję zbierania danych, możesz to zrobić w zasadach zabezpieczeń. Aby usunąć wdrożone wcześniej agenty monitorowania, wybierz opcję menu Usuń agenty.

> [AZURE.NOTE] Lista obsługiwanych maszyn wirtualnych znajduje się w artykule [Azure Security Center frequently asked questions (FAQ)](security-center-faq.md) (Centrum zabezpieczeń Azure — często zadawane pytania [FAQ]).

Dla każdego regionu, w którym zostały uruchomione maszyny wirtualne, wybierz konto magazynu, w którym będą przechowywane dane z tych maszyn. Jeśli nie wybierzesz konta magazynu dla każdego regionu, zostanie ono utworzone automatycznie. Można wybrać lokalizację magazynu na region lub przechowywać wszystkie informacje w centralnej lokalizacji. Chociaż zasady zabezpieczeń można ustawić na poziomie subskrypcji i grupy zasobów Azure, region dla konta magazynu można wybrać tylko na poziomie subskrypcji.

Jeśli używasz konta magazynu współdzielonego między różnymi zasobami Azure, pamiętaj, aby przeczytać artykuł [Azure Storage Scalability and Performance Targets](../storage/storage-scalability-targets.md) (Skalowalność i cele wydajności magazynu Azure), by uzyskać więcej informacji na temat limitów rozmiarów i ograniczeń. Subskrypcja ma również limity konta magazynu. Aby zapoznać się z tymi limitami, przejrzyj artykuł [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits) (Limity, przydziały i ograniczenia subskrypcji i usługi Azure).

> [AZURE.NOTE] Koszty związane z magazynem nie są uwzględnione w cenie usługi Centrum zabezpieczeń Azure i będą naliczane osobno według zwykłych [stawek obowiązujących dla magazynu Azure](https://azure.microsoft.com/pricing/details/storage/). 

Zagadnienia dotyczące wydajności i skalowalności powinny być również planowane w oparciu o rozmiar środowiska Azure oraz zasoby korzystające z konta magazynu. Więcej informacji znajduje się w artykule [Microsoft Azure Storage Performance and Scalability Checklist](../storage/storage-performance-checklist.md) (Lista kontrolna wydajności i skalowalności magazynu Microsoft Azure).

## Bieżące monitorowanie zabezpieczeń

Po wstępnej konfiguracji i zastosowaniu zaleceń Centrum zabezpieczeń Azure następny krok polega na uwzględnieniu procesów operacyjnych Centrum zabezpieczeń Azure. 

Aby uzyskać dostęp do Centrum zabezpieczeń Azure za pomocą portalu Azure, kliknij przycisk **Przeglądaj** i wpisz **Centrum zabezpieczeń** w polu **Filtruj**. Widoki, które uzyskuje użytkownik, są oparte na zastosowanych filtrach.

Centrum zabezpieczeń Azure nie zakłóca zwykłych procedur operacyjnych, pasywnie monitoruje wdrożenia i zapewnia zalecenia na podstawie włączonych zasad zabezpieczeń.

Pulpit nawigacyjny Centrum zabezpieczeń Azure jest podzielony na dwie główne części: 

- Zapobieganie 
- Wykrywanie 

Po pierwszym włączeniu funkcji zbierania danych w Centrum zabezpieczeń Azure dla bieżącego środowiska platformy Azure pamiętaj, aby przejrzeć wszystkie zalecenia. Możesz to zrobić w bloku **Zalecenia** lub względem poszczególnych zasobów (**Maszyny wirtualne**, **Sieci**, **SQL** i **Aplikacja**). 

Po wykonaniu wszystkich zaleceń sekcja **Zapobieganie** powinna być zielona dla wszystkich przejrzanych zasobów. Na tym etapie monitorowanie ciągłe staje się łatwiejsze, ponieważ działania są podejmowane tylko w oparciu o zmiany kondycji zabezpieczeń zasobów oraz kafelki zaleceń.

Sekcja **Wykrywanie** jest bardziej reaktywna. Zawiera ona alerty dotyczące problemów, które występują teraz lub wystąpiły w przeszłości, wykrytych przez kontrolki Centrum zabezpieczeń Azure i systemy innych firm. W kafelku Alerty są wyświetlane wykresy słupkowe zawierające liczbę alertów wykrywania zagrożeń zidentyfikowanych w poszczególnych dniach oraz przedstawiające ich rozkład według różnych kategorii ważności (niskiej, średniej, wysokiej). Więcej informacji na temat alertów zabezpieczeń znajduje się w artykule [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md). 

> [AZURE.NOTE] W celu wizualizacji danych Centrum zabezpieczeń Azure można też skorzystać z usługi Microsoft Power BI. Przeczytaj artykuł [Get insights from Azure Security Center data with Power BI](security-center-powerbi.md) (Uzyskiwanie szczegółowych informacji na podstawie danych z Centrum zabezpieczeń Azure za pomocą usługi Power BI).

### Monitorowanie nowych lub zmodyfikowanych zasobów

Większość środowisk Azure jest dynamiczna. Systematycznie tworzone są nowe zasoby i usuwane stare, zmieniają się konfiguracje itd. Centrum zabezpieczeń Azure pomaga zagwarantować widoczność stanu zabezpieczeń nowych zasobów.

Po dodaniu nowych zasobów (maszyn wirtualnych, baz danych SQL) do środowiska Azure Centrum zabezpieczeń Azure wykrywa je automatycznie i rozpoczyna monitorowanie ich zabezpieczeń. Jeśli w zasadach zabezpieczeń włączono funkcję zbierania danych, dla maszyn wirtualnych zostaną automatycznie włączone dodatkowe funkcje monitorowania. 

![Kluczowe obszary](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3.png) 

1.  W przypadku maszyn wirtualnych uzyskaj dostęp do kafelka **Kondycja zabezpieczeń zasobów** i kliknij opcję **Maszyny wirtualne**. Wszystkie problemy z włączaniem zbierania danych lub powiązanymi zaleceniami zostaną wyświetlone w sekcji **Zalecenia dotyczące monitorowania**.
2.  Wyświetl **zalecenia**, aby sprawdzić, jakie zagrożenia dla bezpieczeństwa, jeśli w ogóle wystąpiły, zostały zidentyfikowane dla nowego zasobu.
3.  Często zdarza się, że podczas dodawania do środowiska nowych maszyn wirtualnych na początku zostaje zainstalowany tylko system operacyjny. Właściciel zasobu może potrzebować trochę czasu, aby wdrożyć inne aplikacje, które będą używane przez te maszyny wirtualne.  Najlepiej byłoby znać stan docelowy danego obciążenia. Czy będzie to serwer aplikacji? W zależności od tego, jaką rolę będzie pełnić nowe obciążenie, możesz włączyć odpowiednie **zasady zabezpieczeń**, co stanowi trzeci krok w tym przepływie pracy.
4.  Ze względu na to, że do środowiska Azure dodawane są nowe zasoby, w kafelku **Alerty zabezpieczeń** mogą zostać wyświetlone nowe alerty. Zawsze sprawdzaj, czy kafelek zawiera nowe alerty, i podejmuj działania zgodnie z zaleceniami Centrum zabezpieczeń Azure.

Warto również regularnie monitorować stan istniejących zasobów, aby móc identyfikować zmiany konfiguracji, które spowodowały zagrożenia bezpieczeństwa, odejście od zalecanych planów bazowych oraz alerty zabezpieczeń. Rozpocznij na pulpicie nawigacyjnym Centrum zabezpieczeń Azure. Pulpit ma trzy główne obszary, które umożliwiają spójne przeglądanie.

![Operacje](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4.png) 

1.  Panel **Kondycja zabezpieczeń zasobów** zapewnia szybki dostęp do kluczowych zasobów. Opcja ta służy do monitorowania maszyn wirtualnych, sieci, SQL i aplikacji. 
2.  Panel **Zalecenia** umożliwia przeglądanie zaleceń Centrum zabezpieczeń Azure. Podczas ciągłego monitorowania może się okazać, że zalecenia nie są wydawane codziennie, co jest zrozumiałe, biorąc pod uwagę, że wszystkie zalecenia zostały zastosowane podczas wstępnej konfiguracji Centrum zabezpieczeń Azure. Z tego powodu nowe informacje w tej sekcji mogą nie występować codziennie i trzeba uzyskiwać do nich dostęp w zależności od potrzeb.
3.  W panelu **Wykrywanie** zmiany mogą pojawiać się bardzo często lub bardzo rzadko. Zawsze czytaj alerty zabezpieczeń i podejmuj działania na podstawie zaleceń Centrum zabezpieczeń Azure. 

## Reagowanie na zdarzenia

Centrum zabezpieczeń Azure wykrywa zagrożenia i powiadamia o nich, gdy tylko wystąpią. Firmy powinny śledzić pojawianie się nowych alertów zabezpieczeń i w razie potrzeby podejmować działania w celu zbadania ataku lub usunięcia jego skutków.

> [AZURE.NOTE] Ten artykuł nie ma na celu ułatwienia tworzenia własnego planu reagowania na zdarzenia. W tworzeniu takiego planu może pomóc [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) (Przewodnik obsługi zdarzeń zabezpieczeń komputera) Narodowego Instytutu Norm i Technologii (NIST).

Każdy alert zabezpieczeń zawiera informacje, które mogą ułatwić lepsze zrozumienie natury ataku i zasugerować możliwe ograniczenie jego skutków. Niektóre alerty zapewniają również linki do większej ilości informacji lub do innych źródeł informacji w ramach platformy Azure. Można użyć dostarczonych informacji do dalszej analizy ataku lub w celu rozpoczęcia ograniczenia jego skutków.

W poniższym przykładzie pokazano podejrzane działania w protokole RDP:

![Podejrzane działania](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5.png)

Jak widać, blok ten zawiera godzinę, o której nastąpił atak, nazwę hosta źródłowego, docelową maszynę wirtualną oraz poszczególne kroki zaleceń. W niektórych sytuacjach pola informacji źródłowych ataku mogą być puste. Więcej informacji na temat działania tego typu znajduje się w artykule [Missing Source Information in Azure Security Center Alerts](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/) (Brakujące informacje źródłowe w alertach Centrum zabezpieczeń Azure).

| **Poziom**        | **Wykrywanie ataków**                     | **Sugerowane ograniczenie skutków**                                                                       |
|------------------|------------------------------------------|-----------------------------------------------------------------------------------------------|
| Sieć          | Wykrycie ataku siłowego w protokole RDP                | Zmniejsz obszar ataku, usuwając niepotrzebny otwarty punkt końcowy, skonfiguruj listy ACL, użyj silnego hasła. |
| Maszyny wirtualne | Proces SVCHOST uruchamiany z niewłaściwego katalogu | Przenieś maszynę wirtualną do innej (izolowanej) podsieci, przeskanuj ją i utwórz ponownie                        |
| Aplikacja      | Iniekcja kodu SQL w bazie danych Azure          | Utrwal konfiguracje bazy danych                                                                |



## Następne kroki
W tym dokumencie przedstawiono sposób konfigurowania zasad zabezpieczeń w Centrum zabezpieczeń. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure.
- [Centrum zabezpieczeń Azure — często zadawane pytania](security-center-faq.md) — odpowiedzi na najczęstsze pytania dotyczące korzystania z usługi.
- [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.



<!--HONumber=Jun16_HO2-->


