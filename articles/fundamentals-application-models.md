<properties  umbracoNaviHide="0" pageTitle="Application Model" metaKeywords="Azure, Azure, application model, Azure application model, development model, Azure development model, hosted service, Azure hosted service, web role, worker role" description="Learn about the Azure hosted service application model. Understand core concepts, design considerations, defining and configuring your application, and scaling." linkid="dev-net-fundamentals-application-model" urlDisplayName="Application Model" headerExpose="" footerExpose="" disqusComments="1" title="Application Model" authors="" />

# Modele wykonywania systemu Azure

System Azure udostępnia różne modele wykonywania na potrzeby uruchamiania aplikacji. Każdy model wykonywania udostępnia inny zestaw usług, a zatem Twój wybór zestawu usług powinien być ściśle oparty na tym, co próbujesz zrobić. W tym artykule opisano trzy zestawy usług, przedstawiono ich technologie i podano przykłady, w jakich przypadkach należy z nich korzystać.

## Spis treści

* [Maszyny wirtualne](#VMachine)
* [Witryny sieci Web](#WebSites)
* [Usługi w chmurze](#CloudServices)
* [Której usługi należy użyć? Dokonywanie wyboru](#WhatShouldIUse)

<h2><a  id="VMachine" ></a>Maszyny wirtualne</h2>


Usługa Maszyny wirtualne systemu Azure pozwala deweloperom, pracownikom działu informatycznego i innym osobom tworzyć maszyny wirtualne w chmurze i z nich korzystać. Ta technologia udostępnia model *Infrastruktura jako usługa (Infrastructure as a Service, IaaS)*, dzięki czemu można z niej korzystać na wiele sposobów. Na [ilustracji 1](#Fig1) przedstawiono jej podstawowe składniki.

<a name="Fig1"></a>![01_CreatingVMs](./media/fundamentals-application-models/ExecModels_01_CreatingVMs.png)

**Ilustracja 1. Usługa Maszyny wirtualne systemu Azure udostępnia model Infrastruktura jako usługa.**

Jak przedstawiono na ilustracji, maszyny wirtualne można tworzyć przy użyciu Portalu zarządzania systemu Azure lub interfejsu API zarządzania usługami systemu Azure opartego na protokole REST. Dostęp do Portalu zarządzania można uzyskać z poziomu każdej popularnej przeglądarki, w tym z poziomu przeglądarek Internet Explorer, Mozilla i Chrome. Dla interfejsu API protokołu REST firma Microsoft udostępnia narzędzia do obsługi skryptów klienta w przypadku systemów Windows, Linux i Macintosh. Z tego interfejsu można również swobodnie korzystać przy użyciu innego oprogramowania.

Niezależnie od tego, w jaki sposób uzyskasz dostęp do tej platformy, do utworzenia nowej maszyny wirtualnej jest wymagane wybranie wirtualnego dysku twardego (VHD) dla obrazu maszyny wirtualnej. Te wirtualne dyski twarde są przechowywane w obiektach BLOB systemu Azure.

Dwie opcje rozpoczęcia pracy

1.  Przekazanie własnego wirtualnego dysku twardego 2.  Użycie wirtualnych dysków twardych udostępnionych przez firmę
    Microsoft i jej partnerów w galerii usługi Maszyny wirtualne systemu
    Azure lub w witrynie open source [VMDepot][1] w sieci Web firmy
    Microsoft

Wirtualne dyski twarde dostępne w tej galerii oraz w witrynie sieci Web VMDepot obejmują czyste obrazy systemów operacyjnych Linux i systemów operacyjnych firmy Microsoft, a także obrazy zawierające już zainstalowane produkty firmy Microsoft oraz innych firm. Opcje te są cały czas wzbogacane. Ich przykłady to różne wersje, wydania i konfiguracje następujących produktów:

* Windows Server
* Serwery systemu Linux, na przykład Suse, Ubuntu i CentOS
* SQL Server
* BizTalk Server
* SharePoint Server

Oprócz wirtualnego dysku twardego musisz określić rozmiar nowej maszyny wirtualnej. Pełne statystyki dla poszczególnych rozmiarów podano w
[bibliotece systemu Azure][2].

* **Bardzo mały** ze wspólnym rdzeniem i 768 MB pamięci.
* **Mały** z 1 rdzeniem i 1,75 GB pamięci.
* **Średni** z 2 rdzeniami i 3,5 GB pamięci.
* **Duży** z 4 rdzeniami i 7 GB pamięci.
* **Bardzo duży** z 8 rdzeniami i 14 GB pamięci.
* **A6** z 4 rdzeniami i 28 GB pamięci.
* **A7** z 8 rdzeniami i 56 GB pamięci.

Na koniec musisz wybrać centrum danych systemu Azure, w którym Twoja nowa maszyna wirtualna ma zostać uruchomiona — w Stanach Zjednoczonych, Europie lub Azji.

Gdy maszyna wirtualna jest uruchomiona, płacisz za każdą godzinę jej działania i przestajesz płacić po usunięciu tej maszyny wirtualnej. Kwota do zapłaty nie zależy od tego, jak intensywnie Twoja maszyna wirtualna jest używana — kwota ta jest oparta wyłącznie na liczbie godzin działania maszyny wirtualnej. Koszt maszyny wirtualnej działającej przez jedną godzinę w stanie bezczynności jest taki sam jak koszt maszyny wirtualnej działającej przez jedną godzinę pod dużym obciążeniem.

Z każdą uruchomioną maszyną wirtualną jest skojarzony *dysk systemu operacyjnego*, który jest przechowywany w obiekcie BLOB. Gdy tworzysz maszynę wirtualną przy użyciu wirtualnego dysku twardego z galerii, ten wirtualny dysk twardy jest kopiowany na dysk systemu operacyjnego Twojej maszyny wirtualnej. Będą tutaj przechowywane wszelkie zmiany wprowadzone w systemie operacyjnym Twojej uruchomionej maszyny wirtualnej. Jeśli na przykład zainstalujesz aplikację, która wprowadza zmiany w rejestrze systemu Windows, zmiany te będą przechowywane na dysku systemu operacyjnego Twojej maszyny wirtualnej. Gdy następnym razem utworzysz maszynę wirtualną przy użyciu tego dysku systemu operacyjnego, maszyna wirtualna będzie nadal działała w tym samym stanie, w którym została przez Ciebie pozostawiona. W przypadku wirtualnych dysków twardych przechowywanych w galerii firma Microsoft w razie potrzeby stosuje aktualizacje, na przykład poprawki systemu operacyjnego. W przypadku wirtualnych dysków twardych przechowywanych na Twoich własnych dyskach systemu operacyjnego to Ty jednak ponosisz odpowiedzialność za stosowanie tych aktualizacji (chociaż usługa Windows Update jest domyślnie włączona).

Uruchomione maszyny wirtualne można również modyfikować, a następnie przechwytywać za pomocą narzędzia sysprep. Narzędzie sysprep usuwa oznaczenia, takie jak nazwa komputera, aby można było użyć obrazu wirtualnego dysku twardego w celu utworzenia dodatkowych maszyn wirtualnych przy użyciu tej samej konfiguracji ogólnej. Obrazy te są przechowywane w portalu zarządzania razem z przekazanymi obrazami, dzięki czemu można ich użyć jako punktu początkowego dodatkowych maszyn wirtualnych.

Usługa Maszyny wirtualne monitoruje ponadto sprzęt hostujący wszystkie utworzone maszyny wirtualne. W przypadku awarii serwera fizycznego z uruchomioną maszyną wirtualną platforma wykrywa tę awarię i uruchamia tę samą maszynę wirtualną na innym komputerze. Jeśli masz odpowiednie licencje, możesz skopiować zmodyfikowany wirtualny dysk twardy ze swojego dysku systemu operacyjnego, a następnie uruchomić go w innym miejscu, na przykład w swoim własnym lokalnym centrum danych lub u innego dostawcy środowiska chmury.

Oprócz dysku systemu operacyjnego Twoja maszyna wirtualna zawiera co najmniej jeden dysk z danymi. Chociaż każdy z nich wygląda jak litera dysku zainstalowana na Twojej maszynie wirtualnej, zawartość wszystkich dysków z danymi jest w rzeczywistości przechowywana w obiekcie BLOB. Wszystkie operacje zapisu na dysku z danymi są trwale zapisywane w odpowiednim obiekcie BLOB. Jak w przypadku wszystkich obiektów BLOB, system Azure replikuje je zarówno w obrębie pojedynczego centrum danych, jak i między centrami danych, aby zapewnić ochronę na wypadek awarii.

Uruchamianiem maszyn wirtualnych można zarządzać za pomocą Portalu zarządzania, programu PowerShell i innych narzędzi do obsługi skryptów lub bezpośrednio za pośrednictwem interfejsu API protokołu REST. (W rzeczywistości wszystko to, co można zrobić za pomocą Portalu zarządzania, można również zrobić programistycznie za pośrednictwem tego interfejsu API). Partnerzy firmy Microsoft, na przykład firmy RightScale i ScaleXtreme, również udostępniają usługi zarządzania oparte na interfejsie API protokołu REST.

Z usługi Maszyny wirtualne systemu Azure można korzystać na wiele sposobów. Oto podstawowe scenariusze uwzględniane przez firmę Microsoft:

* **Maszyny wirtualne na potrzeby tworzenia i testowania aplikacji.**
  Grupy projektowe zazwyczaj potrzebują maszyn wirtualnych o określonych
  konfiguracjach na potrzeby tworzenia aplikacji. Usługa Maszyny
  wirtualne systemu Azure udostępnia prosty i ekonomiczny sposób
  tworzenia tych maszyn wirtualnych, korzystania z nich, a następnie ich
  usuwania, gdy nie są już potrzebne.
* **Uruchamianie aplikacji w chmurze.** W przypadku niektórych aplikacji
  uruchamianie w chmurze publicznej jest uzasadnione ekonomicznie.
  Rozważmy na przykład aplikację, w przypadku której występują długie
  okresy zwiększonego zapotrzebowania. Zawsze można zakupić liczbę
  komputerów wystarczającą na własne centrum danych na potrzeby
  uruchamiania tej aplikacji, ale większość z tych komputerów
  prawdopodobnie przez znaczną część czasu nie byłaby wykorzystana.
  Uruchamiając tę aplikację w systemie Azure, płacisz za dodatkowe
  maszyny wirtualne tylko wtedy, gdy ich potrzebujesz, i zamykasz je,
  gdy okres zwiększonego zapotrzebowania na aplikację się zakończy.
  Załóżmy, że kierujesz rozwijającą się firmą, która potrzebuje zasobów
  obliczeniowych na żądanie i bez żadnych zobowiązań. Również w takim
  scenariuszu system Azure może być odpowiednim wyborem.
* **Rozszerzanie własnego centrum danych przy użyciu chmury
  publicznej.** Za pomocą usługi Sieć wirtualna systemu Azure Twoja
  organizacja może utworzyć sieć wirtualną (VNET), dzięki której grupa
  maszyn wirtualnych systemu Azure funkcjonuje tak, jakby była częścią
  Twojej własnej sieci lokalnej. Pozwala to uruchamiać w systemie Azure
  aplikacje, między innymi program SharePoint. Przy takim podejściu
  wdrożenia mogą okazać się prostsze i tańsze niż w przypadku
  uruchamiania aplikacji we własnym centrum danych.
* **Odzyskiwanie po awarii.** Nie musisz stale opłacać zapasowego
  centrum danych, którego rzadko używasz. Zamiast tego możesz korzystać
  z odzyskiwania po awarii opartego na modelu IaaS, w przypadku którego
  płacisz tylko za potrzebne zasoby obliczeniowe tylko wtedy, gdy ich
  naprawdę potrzebujesz. Jeśli na przykład Twoje główne centrum danych
  ulegnie awarii, możesz utworzyć maszyny wirtualne działające w
  systemie Azure, aby uruchomić niezbędne aplikacje, a następnie zamknąć
  je, gdy przestaną być potrzebne.

Te scenariusze nie są czysto hipotetycznymi sytuacjami, ale stanowią dobre przykłady na to, w jaki sposób możesz wykorzystać usługę Maszyny wirtualne systemu Azure.

### Grupowanie maszyn wirtualnych: Usługi w chmurze

Podczas tworzenia nowej maszyny wirtualnej przy użyciu usługi Maszyny wirtualne systemu Azure można podjąć decyzję o jej uruchomieniu jako samodzielnego wystąpienia lub uczynieniu jej częścią grupy maszyn wirtualnych działających razem w *usłudze w chmurze*. [Mimo podobnych nazw nie należy mylić tego pojęcia z usługą Usługi w chmurze, czyli nazwą technologii opartej na modelu Platforma jako usługa (PaaS) systemu Azure; są to dwa różne pojęcia]. Do każdej samodzielnej maszyny wirtualnej jest przypisany jej odrębny publiczny adres IP, natomiast dostęp do wszystkich maszyn wirtualnych w tej samej usłudze w chmurze odbywa się przy użyciu wspólnego publicznego adresu IP. Przedstawiono to na [ilustracji 2](#Fig2).

<a name="Fig2"></a>![02_CloudServices](./media/fundamentals-application-models/ExecModels_02_CloudServices.png)

**Ilustracja 2. Każda samodzielna maszyna wirtualna ma swój własny publiczny adres IP, natomiast dostęp do maszyn wirtualnych zgrupowanych w usłudze w chmurze odbywa się przy użyciu wspólnego publicznego adresu IP.**

Na przykład w przypadku tworzenia maszyny wirtualnej na potrzeby utworzenia i przetestowania prostej aplikacji lepszym wyborem prawdopodobnie będzie samodzielna maszyna wirtualna. W przypadku tworzenia aplikacji wielowarstwowej z frontonem sieci Web, bazą danych i być może nawet warstwą pośredniczącą lepiej jednak będzie połączyć kilka maszyn wirtualnych w usługę w chmurze, jak zaproponowano na ilustracji 2.

W przypadku grupowania maszyn wirtualnych w usługę w chmurze można również korzystać z innych opcji. System Azure udostępnia funkcję równoważenia obciążenia dla maszyn wirtualnych w tej samej usłudze w chmurze, która rozkłada żądania użytkowników między poszczególne maszyny wirtualne. Maszyny wirtualne połączone w ten sposób mogą również komunikować się bezpośrednio między sobą za pośrednictwem sieci lokalnej w obrębie centrum danych systemu Azure.

Maszyny wirtualne w tej samej usłudze w chmurze można również zgrupować w jeden bądź więcej *zestawów dostępności*. Aby zrozumieć, dlaczego jest to istotne, rozważmy aplikację sieci Web korzystającą z wielu maszyn wirtualnych frontonu. W przypadku wdrożenia wszystkich tych maszyn wirtualnych na tym samym komputerze fizycznym lub nawet na tym samym stojaku komputerów jedna awaria sprzętu może spowodować niedostępność wszystkich tych maszyn wirtualnych. W przypadku zgrupowania tych maszyn wirtualnych w zestaw dostępności system Azure wdroży je jednak w obrębie centrum danych w taki sposób, aby żadna z nich nie miała wspólnego pojedynczego punktu awarii z inną maszyną wirtualną.

### Scenariusz: uruchamianie aplikacji za pomocą programu SQL Server

Aby lepiej zrozumieć sposób działania usługi Maszyny wirtualne systemu Azure, warto rozważyć kilka scenariuszy nieco bardziej szczegółowo. Załóżmy na przykład, że chcesz utworzyć niezawodną i skalowalną aplikację sieci Web uruchamianą w systemie Azure. Jednym ze sposobów na to jest uruchamianie logiki aplikacji na jednej bądź większej liczbie maszyn wirtualnych systemu Azure, a następnie skorzystanie z programu SQL Server w celu zarządzania danymi. Przedstawiono to na [ilustracji 3](#Fig3).

<a name="Fig3"></a>![03_AppUsingSQLServer](./media/fundamentals-application-models/ExecModels_03_AppUsingSQLServer.png)

**Ilustracja 3. Aplikacja uruchomiona w usłudze Maszyny wirtualne systemu Azure może używać programu SQL Server do obsługi magazynu.**

W tym przykładzie oba rodzaje maszyn wirtualnych zostały utworzone na podstawie standardowych wirtualnych dysków twardych z galerii. Logika aplikacji jest uruchomiona w systemie Windows Server 2008 R2, a zatem deweloper tworzy trzy maszyny wirtualne na podstawie tego wirtualnego dysku twardego, a następnie instaluje swoją aplikację na każdej z tych maszyn wirtualnych. Ponieważ wszystkie te maszyny wirtualne znajdują się w tej samej usłudze w chmurze, deweloper może skonfigurować równoważenie obciążenia sprzętu, aby rozłożyć żądania między te maszyny wirtualne. Deweloper tworzy też dwie maszyny wirtualne na podstawie wirtualnego dysku twardego z galerii, który zawiera program SQL Server 2012. Po uruchomieniu tych maszyn wirtualnych deweloper konfiguruje program SQL Server w obu ich wystąpieniach, aby korzystać z dublowania baz danych z automatyczną pracą awaryjną. Nie jest to wymagane — aplikacja może korzystać tylko z jednego wystąpienia programu SQL Server — ale przyjęcie takiego podejścia zwiększa niezawodność.

### Scenariusz: uruchamianie farmy programu SharePoint

Załóżmy, że organizacja chce utworzyć farmę programu SharePoint, ale nie chce uruchamiać tej farmy w swoim własnym centrum danych. Być może lokalne centrum danych ma za mało zasobów lub jednostka biznesowa tworząca farmę nie chce korzystać ze swojej wewnętrznej grupy pracowników działu informatycznego. W takich przypadkach dobrym rozwiązaniem może być uruchomienie programu SharePoint w usłudze Maszyny wirtualne systemu Azure. Przedstawiono to na [ilustracji 4](#Fig4).

<a name="Fig4"></a>![04_SharePointFarm](./media/fundamentals-application-models/ExecModels_04_SharePointFarm.png)

**Ilustracja 4. Usługa Maszyny wirtualne systemu Azure pozwala uruchomić farmę programu SharePoint w chmurze.**

Farma programu SharePoint zawiera kilka składników. Każdy z nich jest uruchomiony na maszynie wirtualnej systemu Azure utworzonej na podstawie innego wirtualnego dysku twardego. Potrzebne są następujące składniki:

* Program Microsoft SharePoint. W galerii znajdują się wersje próbne
  obrazów lub organizacja udostępnia swój własny wirtualny dysk twardy.
* Program Microsoft SQL Server. Program SharePoint jest zależny od
  programu SQL Server, a zatem organizacja tworzy maszyny wirtualne z
  programem SQL Server 2012 na podstawie standardowego wirtualnego dysku
  twardego z galerii.
* Usługa Active Directory systemu Windows Server. Program SharePoint
  wymaga również usługi Active Directory, a zatem organizacja tworzy
  kontrolery domeny w chmurze przy użyciu obrazu systemu Windows Server
  z galerii. Nie jest to bezwzględnie wymagane — można również użyć
  lokalnych kontrolerów domeny — ale program SharePoint często prowadzi
  interakcję z usługą Active Directory, dlatego zaprezentowane tutaj
  podejście przyniesie w rezultacie lepszą wydajność.

Chociaż nie pokazano tego na ilustracji, ta farma programu SharePoint prawdopodobnie jest połączona z siecią lokalną przy użyciu usługi Sieć wirtualna systemu Azure. Dzięki temu maszyny wirtualne — oraz zawarte na nich aplikacje — wyglądają na lokalne dla osób korzystających z tej sieci.

Jak przedstawiono w tych przykładach, można skorzystać z usługi Maszyny wirtualne systemu Azure w celu utworzenia i uruchomienia nowych aplikacji w chmurze lub uruchomienia istniejących aplikacji albo można użyć tej usługi na inne sposoby. Niezależnie od wybranej opcji cel tej technologii pozostaje niezmienny: udostępnianie fundamentów o ogólnym przeznaczeniu na potrzeby przetwarzania danych w chmurze publicznej.

<h2><a  id="WebSites" ></a>Witryny sieci Web</h2>


Użytkownicy korzystają z technologii sieci Web na wiele różnych sposobów. W przypadku korporacji może być konieczne skonfigurowanie lub przeprowadzenie migracji witryny obecności w sieci Web, która umożliwia obsługę milionów wizyt tygodniowo i może zostać wdrożona w kilku centrach danych na całym świecie. Agencja zajmująca się projektowaniem witryn sieci Web może współpracować z zespołem deweloperów nad utworzeniem niestandardowej aplikacji sieci Web zapewniającej obsługę tysięcy użytkowników. W przypadku dewelopera w firmie może być konieczne skonfigurowanie aplikacji do śledzenia raportów z wydatków w chmurze, które są dostarczane przez uwierzytelnionych użytkowników z poziomu firmowej usługi Active Directory. Konsultant IT może użyć popularnej aplikacji typu open source w celu skonfigurowania systemu zarządzania zawartością dla małej firmy. Wszystkie te scenariusze można zrealizować za pomocą usługi Maszyny wirtualne systemu Azure. Do utworzenia czystych maszyn wirtualnych i zarządzania nimi są jednak wymagane pewne umiejętności oraz wysiłek. W przypadku, gdy musisz zaimplementować witrynę sieci Web lub aplikację sieci Web, istnieje prostsze (i tańsze) rozwiązanie: podejście znane jako model Platforma jako usługa (Platform as a Service, PaaS). Jak przedstawiono na ilustracji 5, system Azure udostępnia ten model w formie usługi Witryny sieci Web.

<a name="Fig5"></a>![05_Websites](./media/fundamentals-application-models/ExecModels_05_Websites.png)

**Ilustracja 5. Usługa Witryny sieci Web systemu Azure obsługuje statyczne witryny sieci Web, popularne aplikacje sieci Web oraz niestandardowe aplikacje sieci Web, które zostały utworzone przy użyciu wielu różnych technologii.**

Usługa Witryny sieci Web systemu Azure została opracowana na bazie usługi Usługi w chmurze systemu Azure w celu utworzenia rozwiązania typu Platforma jako usługa (PaaS) zoptymalizowanego pod kątem uruchamiania aplikacji sieci Web. Jak pokazano na ilustracji, usługa Witryny sieci Web jest uruchamiana na zestawie pojedynczych maszyn wirtualnych, który może zawierać wiele witryn sieci Web utworzonych przez różnych użytkowników, a także standardowe maszyny wirtualne należące do pojedynczych użytkowników. Maszyny wirtualne stanowią część puli zasobów zarządzanych przez usługę Witryny sieci Web systemu Azure, dzięki czemu oferują wysoką niezawodność i odporność na uszkodzenia. Rozpoczęcie pracy jest proste. Korzystając z usługi Witryny sieci Web systemu Azure, użytkownicy mają do wyboru wiele różnych aplikacji, platform i szablonów, za pomocą których mogą tworzyć witryny sieci Web w bardzo krótkim czasie. Następnie mogą korzystać ze swoich ulubionych narzędzi deweloperskich (programów WebMatrix i Visual Studio lub dowolnego innego edytora) oraz opcji kontroli źródła w celu skonfigurowania ciągłej integracji i zespołowego tworzenia aplikacji. Aplikacje oparte na bazach danych MySQL mogą korzystać z usługi MySQL udostępnianej dla systemu Azure przez firmę ClearDB będącą partnerem firmy Microsoft. Dzięki usłudze Witryny sieci Web deweloperzy mogą tworzyć duże, skalowalne aplikacje sieci Web. Technologia ta umożliwia tworzenie aplikacji przy użyciu języków programowania ASP.NET, PHP, Node.js i Python. Aplikacje mogą na przykład korzystać z umocowanych sesji, a istniejące aplikacje sieci Web można przenosić do tej platformy chmurowej bez wprowadzania jakichkolwiek zmian. Aplikacje opracowane na bazie usługi Witryny sieci Web mogą swobodnie korzystać z innych aspektów systemu Azure, na przykład usług Magistrala usług i Baza danych SQL oraz magazynu obiektów BLOB. Można również uruchomić wiele kopii aplikacji na różnych maszynach wirtualnych, korzystając z funkcji równoważenia obciążenia żądaniami między nimi oferowanej przez usługę Witryny sieci Web. Ponieważ nowe wystąpienia usługi Witryny sieci Web są tworzone na już istniejących maszynach wirtualnych, uruchamianie nowego wystąpienia aplikacji odbywa się bardzo szybko; przebiega ono znacznie szybciej niż oczekiwanie na utworzenie nowej maszyny wirtualnej. Jak pokazano na [ilustracji 5](#Fig5), można opublikować kod i inną zawartość sieci Web w usłudze Witryny sieci Web na kilka sposobów. Można skorzystać z protokołu FTP lub FTPS albo technologii WebDeploy firmy Microsoft. Usługa Witryny sieci Web umożliwia również publikowanie kodu z systemów kontroli źródła, takich jak Git, GitHub, CodePlex, BitBucket, Dropbox, Mercurial,
Team Foundation Server oraz oparty na chmurze Team Foundation Service.

<h2><a  id="CloudServices" ></a>Usługi w chmurze</h2>


Usługa Maszyny wirtualne systemu Azure udostępnia model IaaS, natomiast usługa Witryny sieci Web systemu Azure — hosting w sieci Web. Trzecia opcja przetwarzania danych — usługa Usługi w chmurze — udostępnia model *Platforma jako usługa (Platform as a Service, PaaS)*. Ta technologia została zaprojektowana do obsługi aplikacji, które są skalowalne, niezawodne i niedrogie w obsłudze. Celem jej jest również uwolnienie deweloperów od problemów związanych z zarządzaniem używaną platformą i umożliwienie im pełnego skupienia na opracowywanych aplikacjach. Przedstawiono to na [ilustracji 6](#Fig6).

<a name="Fig6"></a>![06_CloudServices2](./media/fundamentals-application-models/ExecModels_06_CloudServices2.png)

**Ilustracja 6. Usługa Usługi w chmurze systemu Azure udostępnia model Platforma jako usługa.**

Tak jak inne opcje przetwarzania danych oferowane przez system Azure, usługa Usługi w chmurze jest zależna od maszyn wirtualnych. Technologia ta udostępnia dwie nieco różne opcje w zakresie maszyn wirtualnych: wystąpienia *ról Sieć Web* uruchamiają wariant systemu Windows Server z usługami IIS, natomiast wystąpienia *ról Proces roboczy* uruchamiają ten sam wariant systemu Windows Server bez usług IIS. Aplikacja usługi Usługi w chmurze jest zależna od pewnej kombinacji tych dwóch opcji.

Na przykład prosta aplikacja może korzystać tylko z roli Sieć Web, natomiast aplikacja bardziej złożona może używać roli Sieć Web do obsługi żądań przychodzących od użytkowników, a następnie przekazywać wyniki pracy tych żądań do roli Proces roboczy w celu przetwarzania. (Na potrzeby tej komunikacji może być używana usługa Magistrala usług lub Kolejki systemu Azure).

Jak wynika z ilustracji, wszystkie maszyny wirtualne w pojedynczej aplikacji są uruchamiane w ramach tej samej usługi w chmurze, co opisano wcześniej w kontekście usługi Maszyny wirtualne systemu Azure. Z tego powodu użytkownicy uzyskują dostęp do aplikacji przy użyciu jednego publicznego adresu IP, a obciążenie związane z tymi żądaniami jest automatycznie równoważone między maszynami wirtualnymi aplikacji. Jak w przypadku usług w chmurze tworzonych za pomocą usługi Maszyny wirtualne systemu Azure, platforma wdroży maszyny wirtualne w aplikacji usługi Usługi w chmurze tak, aby uniknąć pojedynczego punktu awarii sprzętu.

Mimo że aplikacje są uruchomione na maszynach wirtualnych, istotne jest zrozumienie tego, że usługa Usługi w chmurze udostępnia model PaaS, a nie model IaaS. Można to rozpatrzyć w następujący sposób: W przypadku modelu IaaS, takiego jak usługa Maszyny wirtualne systemu Azure, najpierw tworzysz i konfigurujesz środowisko, w którym aplikacja zostanie uruchomiona, a następnie wdrażasz swoją aplikację w tym środowisku. Ponosisz odpowiedzialność za zarządzanie znaczną częścią tego środowiska, wykonując takie czynności, jak wdrażanie nowych poprawionych wersji systemu operacyjnego na każdej maszynie wirtualnej. Natomiast w przypadku modelu PaaS wszystko przebiega tak, jakby środowisko już istniało. Musisz tylko wdrożyć swoją aplikację. Zarządzaniem platformą, na której aplikacja jest uruchomiona, w tym wdrażaniem nowych wersji systemu operacyjnego, zajmują się dla Ciebie inne osoby.

W przypadku usługi Usługi w chmurze nie tworzysz maszyn wirtualnych. Zamiast tego udostępniasz plik konfiguracji instruujący system Azure w zakresie tego, ile sztuk każdej maszyny wirtualnej potrzebujesz, na przykład trzy wystąpienia roli Sieć Web i dwa wystąpienia roli Proces roboczy, a platforma tworzy je dla Ciebie. Nadal samodzielnie wybierasz odpowiednie rozmiary maszyn wirtualnych — dostępne opcje są identyczne jak w przypadku usługi Maszyny wirtualne systemu Azure — samodzielnie nie tworzysz jednak jawnie tych maszyn wirtualnych. Jeśli Twoja aplikacja musi obsługiwać większe obciążenie, możesz złożyć dyspozycję utworzenia większej liczby maszyn wirtualnych, a system Azure utworzy te wystąpienia. Gdy obciążenie ulegnie zmniejszeniu, możesz zamknąć te wystąpienia i przestać za nie płacić.

Aplikacja usługi Usługi w chmurze jest zwykle udostępniana użytkownikom w ramach procesu dwuetapowego. Najpierw deweloper przekazuje aplikację do obszaru tymczasowego platformy. Gdy deweloper jest gotowy do uaktywnienia aplikacji, korzysta z Portalu zarządzania systemu Azure, aby zażądać przełączenia jej do środowiska produkcyjnego. To przełączenie między środowiskiem tymczasowym i produkcyjnym można przeprowadzić bez żadnego przestoju, co pozwala uaktualnić uruchomioną aplikację do nowej wersji bez zakłócania pracy jej użytkownikom.

Usługa Usługi w chmurze udostępnia również funkcje monitorowania. Tak jak usługa Maszyny wirtualne systemu Azure, wykryje ona uszkodzony serwer fizyczny i ponownie uruchomi na nowym komputerze maszyny wirtualne, które były uruchomione na tym serwerze. Usługa Usługi w chmurze wykrywa też jednak uszkodzone maszyny wirtualne i aplikacje, a nie tylko awarie sprzętu. W przeciwieństwie do usługi Maszyny wirtualne ma ona agenta w każdej roli Sieć Web i każdej roli Proces roboczy, dzięki czemu może uruchamiać nowe maszyny wirtualne i wystąpienia aplikacji, gdy pojawiają się awarie.

Z modelem PaaS usługi Usługi w chmurze są również związane inne implikacje. Jedną z najważniejszych z tych implikacji jest to, że aplikacje opracowane na bazie tej technologii powinny być tak napisane, aby działały prawidłowo, gdy jakiekolwiek wystąpienie roli Sieć Web lub roli Proces roboczy ulegnie awarii. Aby osiągnąć ten cel, aplikacja usługi Usługi w chmurze nie powinna przechowywać stanu w systemie plików swoich własnych maszyn wirtualnych. W przeciwieństwie do maszyn wirtualnych utworzonych przy użyciu usługi Maszyny wirtualne systemu Azure zapisy wykonywane na maszynach wirtualnych usługi Usługi w chmurze są nietrwałe; nie istnieje nic takiego, jak dysk z danymi usługi Maszyny wirtualne. Zamiast tego aplikacja usługi Usługi w chmurze powinna jawnie zapisywać wszystkie informacje o stanie w usłudze Baza danych SQL, obiektach BLOB, tabelach lub jakimś innym magazynie zewnętrznym. Aplikacje opracowane w ten sposób można łatwiej skalować i są one bardziej odporne na błędy, czyli są zgodne z założeniami usługi Usługi w chmurze.

<h2><a  id="WhatShouldIUse" ></a>Której usługi należy użyć? Dokonywanie wyboru</h2>


Wszystkie trzy modele wykonywania udostępniane przez system Azure pozwalają tworzyć skalowalne i niezawodne aplikacje w chmurze. Biorąc pod uwagę to zasadnicze podobieństwo, której usługi należy użyć? Odpowiedź zależy od tego, co próbujesz zrobić.

Usługa Maszyny wirtualne zapewnia rozwiązanie o najbardziej ogólnym przeznaczeniu. Jeśli chcesz uzyskać najwyższy możliwy poziom kontroli lub potrzebujesz maszyn wirtualnych ogólnego przeznaczenia, na przykład na potrzeby tworzenia i testowania aplikacji, jest to najlepsza opcja. Usługa Maszyny wirtualne jest też najlepszym wyborem w przypadku uruchamiania standardowych aplikacji lokalnych w chmurze, jak to przedstawiono w opisanym wcześniej przykładzie programu SharePoint. Ponieważ maszyny wirtualne tworzone przy użyciu tej technologii mogą wyglądać tak samo jak Twoje lokalne maszyny wirtualne, jest to również najlepszy wybór w przypadku odzyskiwania po awarii. Wadą tego rozwiązania jest związana z zaawansowanymi możliwościami duża odpowiedzialność — model IaaS wymaga wykonywania pewnych prac administracyjnych.

Usługa Witryny sieci Web to odpowiednia opcja w sytuacji, gdy chcesz utworzyć prostą witrynę sieci Web. Warto ją wybrać szczególnie wtedy, gdy Twoja witryna zostanie oparta na istniejącej aplikacji, takiej jak Joomla, WordPress lub Drupal. Usługa Witryny sieci Web jest również dobrym wyborem w przypadku tworzenia aplikacji sieci Web o niskich wymaganiach w zakresie administracji, nawet takiej aplikacji, która musi być dość skalowalna, bądź przenoszenia istniejącej aplikacji sieci Web opartej na usługach IIS do chmury publicznej. Usługa ta umożliwia również szybkie wdrażanie aplikacji. Nowe wystąpienie Twojej aplikacji można uruchomić niemal natychmiast, natomiast wdrożenie nowej maszyny wirtualnej przy użyciu usługi Maszyny wirtualne lub Usługi w chmurze może zająć kilka minut.

Usługa Usługi w chmurze, która była początkowym modelem wykonywania udostępnionym przez system Azure, jest jawnie oparta na modelu PaaS. Chociaż granica między modelem PaaS a hostingiem w sieci Web jest rozmyta, usługa Usługi w chmurze różni się w istotnych aspektach od usługi Witryny sieci Web między innymi w następujący sposób:

* W przeciwieństwie do usługi Witryny sieci Web usługa Usługi w chmurze
  zapewnia dostęp administracyjny do maszyn wirtualnych Twojej
  aplikacji. Dzięki temu możesz instalować dowolne oprogramowanie
  potrzebne dla Twojej aplikacji — nie jest to możliwe w przypadku
  usługi Witryny sieci Web.
* Ponieważ usługa Usługi w chmurze oferuje zarówno role Sieć Web, jak i
  role Proces roboczy, jest ona lepszym wyborem niż usługa Witryny sieci
  Web dla aplikacji wielowarstwowych, które wymagają oddzielnych maszyn
  wirtualnych do obsługi swojej logiki biznesowej.
* Usługa Usługi w chmurze udostępnia oddzielne środowiska
  przemieszczania i produkcyjne, dzięki czemu aktualizowanie aplikacji
  przebiega płynniej niż w przypadku usługi Witryny sieci Web.
* W przeciwieństwie do usługi Witryny sieci Web możesz korzystać z
  technologii sieciowych, takich jak usługa Sieć wirtualna systemu Azure
  i usługa Połączenie systemu Azure, w celu dołączania komputerów
  lokalnych do aplikacji usługi Usługi w chmurze.
* Usługa Usługi w chmurze pozwala łączyć się bezpośrednio z maszynami
  wirtualnymi aplikacji przy użyciu pulpitu zdalnego, co nie jest
  możliwe w przypadku usługi Witryny sieci Web.

Ponieważ jest to model PaaS, usługa Usługi w chmurze oferuje również pewne korzyści w stosunku do usługi Maszyny wirtualne systemu Azure. Na przykład jest wykonywana dla Ciebie większa liczba zadań w zakresie zarządzania, w tym wdrażanie aktualizacji systemu operacyjnego, dzięki czemu koszty Twojej działalności zwykle są niższe niż w przypadku korzystania z modelu IaaS usługi Maszyny wirtualne Azure.

Wszystkie trzy modele wykonywania udostępniane przez system Azure mają swoje wady i zalety. Aby móc dokonać najlepszego wyboru, musisz zapoznać się z tymi usługami, musisz wiedzieć, co próbujesz uzyskać, a następnie musisz wybrać tę usługę, która najlepiej pasuje do określonych potrzeb.

Czasami żaden pojedynczy model wykonywania nie jest odpowiedni. W tego typu sytuacjach możesz śmiało łączyć dostępne opcje. Załóżmy na przykład, że tworzysz aplikację, w przypadku której potrzebujesz korzyści oferowanych przez role Sieć Web usługi Usługi w chmurze, ale chcesz też korzystać ze standardowego programu SQL Server ze względu na niezawodność lub wydajność. W tym przypadku najlepszą opcją jest połączenie modeli wykonywania, jak pokazano na [ilustracji 7](#Fig7).

<a name="Fig7"></a>![07_CombineTechnologies](./media/fundamentals-application-models/ExecModels_07_CombineTechnologies.png)

**Ilustracja 7. Jedna aplikacja może korzystać z kilku modeli wykonywania.**

Jak pokazano na ilustracji, maszyny wirtualne usługi Usługi w chmurze działają w oddzielnej usłudze w chmurze pochodzącej z maszyn wirtualnych usługi Maszyny wirtualne. Mimo to oba rodzaje maszyn wirtualnych mogą się dość skutecznie komunikować, przez co jest to czasami najlepsza opcja tworzenia aplikacji.

System Azure udostępnia różne modele wykonywania, ponieważ platformy chmurowe muszą obsługiwać wiele różnych scenariuszy. Każdy użytkownik, który chce efektywnie korzystać z tej platformy — jeśli jesteś w tym punkcie artykułu, prawdopodobnie dotyczy to również Ciebie — musi zapoznać się ze wszystkimi modelami wykonywania.



[1]: http://vmdepot.msopentech.com/
[2]: http://msdn.microsoft.com/en-us/library/windowsazure/dn197896.aspx