<properties
   pageTitle="Rozwiązania usługi Batch i HPC w chmurze | Microsoft Azure"
   description="Wprowadzenie do scenariuszy i opcji rozwiązań usługi Batch i obliczeń o wysokiej wydajności (HPC i duże obliczenia) na platformie Azure"
   services="batch, virtual-machines, cloud-services"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="04/21/2016"
   ms.author="danlep"/>

# Rozwiązania usługi Batch i HPC w chmurze Azure

Platforma Azure oferuje wydajne, skalowalne rozwiązania w chmurze dla usługi Batch i obliczeń o wysokiej wydajności (HPC) — zwanych również *dużymi obliczeniami*. Informacje poniżej dotyczą obciążeń dużych obliczeń oraz usług Azure, w których są one obsługiwane. Można także przejść od razu do [scenariuszy rozwiązań](#scenarios) w dalszej części tego artykułu. Ten artykuł jest przeznaczony głównie dla osób podejmujących decyzje techniczne, menedżerów działu IT i niezależnych dostawców oprogramowania, ale z opisanymi tu rozwiązaniami mogą zapoznać się również inni specjaliści i deweloperzy IT.

Organizacje zajmują się zagadnieniami obliczeniowymi na dużą skalę, m.in. projektowaniem inżynierskim i analizą, renderowaniem obrazu, złożonym modelowaniem, symulacjami Monte Carlo oraz obliczeniami w zakresie ryzyka finansowego. Platforma Azure ułatwia organizacjom realizowanie tych zadań i podejmowanie decyzji, biorąc pod uwagę potrzebne zasoby, odpowiednią skalę i założony harmonogram. Za pomocą platformy Azure organizacje mogą:

* tworzyć rozwiązania hybrydowe przez rozszerzenie lokalnego klastra HPC w celu odciążenia obciążeń maksymalnych chmury;

* uruchamiać narzędzia klastra HPC i obciążenia w całości na platformie Azure;

* używać zarządzanych i skalowalnych usług Azure, takich jak [Batch](https://azure.microsoft.com/documentation/services/batch/), do uruchamiania obciążeń wymagających intensywnych obliczeń bez konieczności wdrażania infrastruktury obliczeniowej i zarządzania nią.

Mimo że ten artykuł nie obejmuje tych zagadnień, platforma Azure dostarcza deweloperom i partnerom pełny zestaw funkcji, rozwiązań architektury i narzędzi programistycznych do tworzenia niestandardowych przepływów pracy w zakresie dużych obliczeń na dużą skalę. Gotowy jest także coraz większy ekosystem rozwiązań partnerskich, który ułatwia zapewnienie wydajności obciążeń dotyczących dużych obliczeń w chmurze Azure.


## Aplikacje usługi Batch i HPC

W przeciwieństwie do aplikacji sieci Web i wielu aplikacji biznesowych — aplikacje usługi Batch i HPC mają zdefiniowany początek i koniec oraz mogą być uruchamiane zgodnie z harmonogramem lub na żądanie — czasami przez wiele godzin lub dłużej. Większość można podzielić na dwie główne kategorie: *wewnętrznie równoległe* (nazywane czasem „żenująco równoległymi”, ponieważ problemy, które są za ich pomocą rozwiązywane, występują równolegle na wielu komputerach lub na wielu procesorach) i *mocno powiązane*. Więcej informacji na temat tych typów aplikacji znajduje się w poniższej tabeli. Niektóre rozwiązania platformy Azure sprawdzają się lepiej dla danego typu.

>[AZURE.NOTE] W rozwiązaniach usługi Batch i HPC uruchomione wystąpienie aplikacji jest zwykle nazywane *zadaniem* i każde zadanie może zostać podzielone na *podzadania*. Klastrowane zasoby obliczeniowe aplikacji są często nazywane *węzłami obliczeniowymi*.

Typ | Właściwości | Przykłady
------------- | ----------- | ---------------
**Wewnętrznie równoległe**<br/><br/>![Wewnętrznie równoległe][parallel] |• Poszczególne komputery uruchamiają logikę aplikacji niezależnie<br/><br/> • Dodawanie komputerów pozwala aplikacji na skalowanie i skrócenie czasu obliczeniowego<br/><br/>• Aplikacja składa się z oddzielnych plików wykonywalnych lub jest podzielona na usługi wywoływane przez klienta (architekturę zorientowaną na usługi lub SOA, aplikację) |• Modelowanie ryzyka finansowego<br/><br/>• Renderowanie obrazu i przetwarzanie obrazu<br/><br/>• Kodowanie i transkodowanie nośników<br/><br/>• Symulacje Monte Carlo<br/><br/>• Testowanie oprogramowania
**Mocno powiązane**<br/><br/>![Mocno powiązane][coupled] |• Aplikacja wymaga, aby węzły obliczeniowe wchodziły w interakcję lub wymieniały się wynikami pośrednimi<br/><br/>• Węzły obliczeniowe mogą komunikować się za pomocą interfejsu MPI, wspólnego protokołu komunikacji do przetwarzania równoległego<br/><br/>• Aplikacja jest wrażliwa na opóźnienie sieci i przepustowość<br/><br/>• Wydajność aplikacji można zwiększyć za pomocą szybkich technologii sieciowych, takich jak InfiniBand i zdalny bezpośredni dostęp do pamięci (RDMA) |• Modelowanie zbiorników ropy naftowej i gazu<br/><br/>• Projektowanie inżynieryjne i analiza, np. obliczeniowa dynamika hydrauliczna<br/><br/>• Fizyczne symulacje, takie jak wypadki samochodowe i reakcje jądrowe<br/><br/>• Prognozowanie pogody

### Zagadnienia dotyczące uruchamiania aplikacji usługi Batch i HPC w chmurze

Wiele aplikacji przeznaczonych do uruchamiania w lokalnych klastrach HPC na platformie Azure lub w środowisku hybrydowym (obejmującym wiele lokalizacji) można łatwo migrować. Mogą jednak występować pewne ograniczenia lub problemy, m.in.:


* **Dostępność zasobów w chmurze** — w zależności od typu używanych zasobów obliczeniowych w chmurze może nie być możliwe zagwarantowanie, że komputer będzie cały czas dostępny podczas wykonywania zadania. Obsługa stanu i sprawdzanie postępu to typowe metody obsługi możliwych błędów przejściowych, które są ważniejsze podczas korzystania z zasobów w chmurze.


* **Dostęp do danych** — powszechnie dostępne metody dostępu do danych w ramach klastra sieci przedsiębiorstwa, np. NFS, mogą wymagać specjalnej konfiguracji w chmurze lub zastosowania innych rozwiązań dostępu do danych i wzorców dla chmury.

* **Przenoszenie danych** — aplikacje, które przetwarzają duże ilości danych, wymagają zastosowania strategii przenoszenia danych do magazynu w chmurze i zasobów obliczeniowych. Może być też konieczne użycie szybkiej sieci obejmującej różne lokalizacje (takiej jak [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/)). Należy również rozważyć ograniczenia prawne, regulacyjne lub dotyczące zasad w zakresie przechowywania lub uzyskiwania dostępu do danych.


* **Licencjonowanie** — skontaktuj się z dostawcą aplikacji komercyjnej w celu uzyskania informacji na temat licencjonowania lub innych ograniczeń związanych z uruchamianiem w chmurze. Nie wszyscy dostawcy oferują licencjonowanie oparte na płatności zgodnie z rzeczywistym użyciem. Dla danego rozwiązania może być konieczne zaplanowanie serwera licencyjnego w chmurze lub połączenie z lokalnym serwerem licencji.


### Duże obliczenia czy dane big data?

Podział na aplikacje dużych obliczeń i danych big data nie zawsze jest jednoznaczny i niektóre aplikacje mogą mieć cechy obu kategorii. Obie kategorie obejmują uruchamianie obliczeń na dużą skalę, zwykle w klastrach komputerów. Zastosowane metody i narzędzia pomocnicze mogą się jednak różnić.

• Funkcja **dużych obliczeń** obejmuje zwykle zastosowania wykorzystujące możliwości procesora CPU i pamięci (np. symulacje inżynieryjne, modelowanie ryzyka finansowego i renderowanie cyfrowe). Klastry obsługujące rozwiązanie dużych obliczeń mogą obejmować komputery z wyspecjalizowanymi procesorami wielordzeniowymi, co umożliwia wykonywanie obliczeń na danych pierwotnych, oraz wyspecjalizowany sprzęt szybkiej sieci, co pozwala na połączenie komputerów.

• Funkcja **danych big data** pozwala rozwiązać problemy związane z analizą danych, które polegają na tym, że nie można zarządzać dużą ilością danych przy użyciu jednego komputera lub w jednym systemie zarządzania bazą danych (np. dużymi ilościami dzienników sieci Web lub innymi danymi analiz biznesowych). Funkcja danych big data zwykle zależy bardziej od pojemności dysku i wydajności We/Wy niż od mocy procesora CPU i może używać specjalistycznych narzędzi takich jak Apache Hadoop do zarządzania klastrem i rozdzielania danych. (Informacje o usłudze Azure HDInsight oraz innych rozwiązaniach Azure Hadoop znajdują się w temacie [Hadoop](https://azure.microsoft.com/solutions/hadoop/)).

## Zarządzanie mocą obliczeniową i planowanie zadań

Uruchamianie aplikacji usługi Batch i HPC często wiąże się z zastosowaniem *menedżer klastra* i *harmonogramu zadań* w celu ułatwienia zarządzania klastrowanymi zasobami obliczeniowymi oraz przydzielenia ich do aplikacji, które uruchamiają zadania. Operacje te można wykonać przy użyciu osobnych narzędzi lub zintegrowanego narzędzia bądź usługi.

* **Menedżer klastra** — aprowizuje i zwalnia zasoby obliczeniowe (lub węzły obliczeniowe) oraz nimi zarządza. Menedżer klastra może zautomatyzować instalowanie obrazów systemu operacyjnego i aplikacji w węzłach obliczeniowych, skalować zasoby obliczeniowe zgodnie z żądaniami i monitorować wydajność węzłów.

* **Harmonogram zadań** — określa zasoby (na przykład procesory lub pamięć), których potrzebuje aplikacja, oraz warunki uruchamiania aplikacji. Harmonogram zadań obejmuje kolejki zadań i przydziela do nich zasoby w oparciu o przypisane priorytety lub inne właściwości.

Narzędzia do klastrowania i planowania zadań dla klastrów opartych na systemach Windows i Linux można łatwo migrować na platformę Azure. Na przykład [zestaw Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), bezpłatne rozwiązanie klastra obliczeniowego firmy Microsoft dla obciążeń HPC w systemach Windows i Linux, oferuje kilka opcji uruchamiania na platformie Azure. Można również utworzyć klastry systemu Linux do uruchamiania narzędzi typu open-source, takich jak Torque i SLURM lub uruchamiania narzędzi komercyjnych, takich jak [TIBCO DataSynapse GridServer](http://www.tibco.com/products/automation/application-development/grid-computing/gridserver) i [Univa Grid Engine](http://www.univa.com/products/grid-engine) na platformie Azure.

Jak opisano w poniższych sekcjach, można także korzystać z usług Azure do zarządzania zasobami obliczeniowymi i zadaniami harmonogramu bez tradycyjnych narzędzi do zarządzania klastrami (lub oprócz nich).


## Scenariusze

Poniżej przedstawiono trzy typowe scenariusze uruchamiania obciążeń funkcji dużych obliczeń na platformie Azure. Obejmują one użycie istniejących rozwiązań klastrów HPC i usług Azure (lub ich połączenia). Najważniejsze kwestie dotyczące wybierania poszczególnych scenariuszy zostały wymienione, ale nie opisano ich tutaj w sposób wyczerpujący. Więcej informacji na temat dostępnych usług Azure, których można użyć w rozwiązaniu, znajduje się w dalszej części tego artykułu.

  | Scenariusz | Powód wybrania?
------------- | ----------- | ---------------
**Rozszerzenie klastra HPC na platformę Azure**<br/><br/>[![Cluster burst][burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Więcej informacji:<br/>• [Burst to Azure with Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx) (Rozszerzanie na platformę Azure za pomocą zestawu Microsoft HPC Pack)<br/><br/>• [Set up a hybrid compute cluster with Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) (Konfigurowanie hybrydowego klastra obliczeniowego za pomocą zestawu Microsoft HPC Pack)<br/><br/>|• Połączenie [zestawu Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) lub innego klastra lokalnego z dodatkowymi zasobami Azure w ramach rozwiązania hybrydowego.<br/><br/>• Rozszerzenie obciążenia funkcji dużych obliczeń do uruchamiania w wystąpieniach maszyny wirtualnej typu platforma jako usługa (PaaS) (obecnie tylko w systemie Windows Server).<br/><br/>• Dostęp do lokalnego serwera licencji lub magazynu danych przy użyciu opcjonalnej sieci wirtualnej Azure.|• Masz istniejący klaster HPC i potrzebujesz dodatkowych zasobów. <br/><br/>• Nie chcesz kupować dodatkowej infrastruktury klastra HPC ani nią zarządzać.<br/><br/>• Występują przejściowe okresy maksymalnego zapotrzebowania lub specjalne projekty.
**Utworzenie klastra HPC w całości na platformie Azure**<br/><br/>[![Cluster in IaaS][iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Więcej informacji:<br/>• [HPC cluster solutions in Azure](./big-compute-resources.md) (Rozwiązania klastra HPC na platformie Azure)<br/><br/>|• Szybkie i spójne wdrażanie aplikacji i narzędzi klastra na standardowych i niestandardowych maszynach wirtualnych w modelu IaaS (infrastruktura jako usługa) w systemie Windows lub Linux.<br/><br/>• Uruchamianie różnych obciążeń dużych obliczeń za pomocą wybranego rozwiązania planowania zadań.<br/><br/>• Korzystanie z dodatkowych usług Azure, w tym sieci i magazynu, w celu utworzenia pełnego rozwiązania w chmurze. |• Nie chcesz kupować dodatkowej infrastruktury klastra HPC dla systemu Linux lub Windows ani nią zarządzać.<br/><br/>• Występują przejściowe okresy maksymalnego zapotrzebowania lub specjalne projekty.<br/><br/>• Potrzebujesz dodatkowego klastra na pewien czas, ale w celu jego wdrożenia nie chcesz inwestować w komputery ani przestrzeń.<br/><br/>• Chcesz odciążyć aplikację wymagającą intensywnych obliczeń, aby była uruchamiania jako usługa w całości w chmurze.
**Skalowanie w poziomie równoległej aplikacji do platformy Azure**<br/><br/>[![Azure Batch][batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Więcej informacji:<br/>• [Podstawy usługi Azure Batch](./batch-technical-overview.md)<br/><br/>• [Wprowadzenie do biblioteki usługi Azure Batch dla środowiska .NET](./batch-dotnet-get-started.md)|• Projektowanie za pomocą interfejsów API usługi [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) w celu skalowania w poziomie różnych obciążeń funkcji dużych obliczeń i uruchamiania ich w pulach maszyn wirtualnych z systemem Windows lub Linux.<br/><br/>• Wykorzystanie usługi Azure do zarządzania wdrażaniem i automatycznym skalowaniem maszyn wirtualnych, harmonogramem zadań, odzyskiwaniem po awarii, przenoszeniem danych, zarządzaniem zależnościami i wdrażaniem aplikacji — bez potrzeby użycia oddzielnego klastra HPC lub harmonogramu zadań.|• Nie chcesz zarządzać zasobami obliczeniowymi ani harmonogramem zadań. Zamiast tego chcesz skupić się na uruchamianiu aplikacji.<br/><br/>• Chcesz odciążyć aplikację wymagającą intensywnych obliczeń, aby była uruchamiania jako usługa w chmurze.<br/><br/>• Chcesz automatycznie skalować zasoby obliczeniowe, aby odpowiadały obciążeniu obliczeniowemu.


## Usługi Azure dla dużych obliczeń

W tej sekcji znajdziesz więcej informacji na temat obliczeń, danych, sieci i powiązanych usług, które można połączyć w ramach rozwiązań i przepływów pracy dużych obliczeń. Dokładne wskazówki dotyczące usług Azure znajdują się w [dokumentacji](https://azure.microsoft.com/documentation/) usług Azure. W [scenariuszach](#scenarios) we wcześniejszej części tego artykułu opisano tylko niektóre sposoby korzystania z tych usług.

>[AZURE.NOTE] Na platformie Azure są systematycznie wprowadzane nowe usługi, które mogą okazać się przydatne w danym scenariuszu. Jeśli masz pytania, skontaktuj się z [partnerem Azure](https://pinpoint.microsoft.com/en-US/search?keyword=azure) lub wyślij wiadomość e-mail na adres *bigcompute@microsoft.com*.

### Usługi obliczeniowe

Usługi obliczeniowe Azure stanowią podstawę rozwiązania do obsługi dużych obliczeń. Różne usługi obliczeniowe zapewniają korzyści w różnych scenariuszach. Na poziomie podstawowym usługi te oferują różne tryby uruchamiania aplikacji w wystąpieniach obliczeniowych opartych na maszynach wirtualnych udostępnianych przez platformę Azure przy użyciu technologii Windows Server Hyper-V. W tych wystąpieniach można uruchamiać różne standardowe i niestandardowe systemy operacyjne Linux i Windows oraz obsługiwane w nich narzędzia. Platforma Azure umożliwia wybór [rozmiarów wystąpień](../virtual-machines/virtual-machines-windows-sizes.md) z różnymi konfiguracjami rdzeni procesora CPU, pamięci, pojemności dysku i innych właściwościach. Zależnie od potrzeb wystąpienia można skalować do tysięcy rdzeni, a następnie skalować w dół, jeśli potrzebnych będzie mniej zasobów.

>[AZURE.NOTE] Użyj wystąpień A8-A11, aby poprawić wydajność niektórych obciążeń HPC, m.in. równoległych zastosowań MPI, które wymagają małego opóźnienia, oraz sieci aplikacji o wysokiej wydajności. Zobacz temat [About the A8, A9, A10, and A11 Compute Intensive Instances](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) (Informacje o intensywnych wystąpieniach obliczeniowych A8, A9, A10 i A11).  

Usługa | Opis
------------- | -----------
**[Usługi w chmurze](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Mogą uruchamiać aplikacje funkcji dużych obliczeń w wystąpieniach roli procesu roboczego, które są maszynami wirtualnymi z uruchomioną wersją systemu Windows Server i są zarządzane w całości na platformie Azure.<br/><br/>• Włączają skalowalne, niezawodne aplikacje o niskim obciążeniu administracyjnym, działające w ramach modelu typu platforma jako usługa (PaaS).<br/><br/>• Mogą wymagać dodatkowych narzędzi lub programowania do integracji z lokalnymi rozwiązaniami klastra HPC.
**[Maszyny wirtualne](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Zapewniają infrastrukturę obliczeniową typu infrastruktura jako usługa (IaaS) przy użyciu technologii Microsoft Hyper-V.<br/><br/>• Umożliwiają elastyczne udostępnianie trwałych komputerów w chmurze ze standardowych obrazów systemu Windows Server lub Linux, lub obrazów i dysków z danymi dostarczanych przez użytkownika lub z portalu [Azure Marketplace](https://azure.microsoft.com/marketplace/) oraz zarządzanie nimi.<br/><br/>• Można je wdrażać i zarządzać nimi jako [Zestawami skalowania maszyny wirtualnej](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/), aby tworzyć usługi na dużą skalę z identycznych maszyn wirtualnych, korzystając przy tym ze skalowania automatycznego w celu automatycznego zwiększania lub zmniejszania wydajności.<br/><br/>• Uruchamiają lokalne narzędzia i aplikacje klastrów obliczeniowych w całości w chmurze.<br/><br/>
**[Batch](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Uruchamia obciążenia równoległe i partie zadań na dużą skalę w pełni zarządzanej usłudze.<br/><br/>• Służy do planowania i skalowania automatycznego zadań zarządzanej puli maszyn wirtualnych.<br/><br/>• Umożliwia deweloperom tworzenie i uruchamianie aplikacji typu aplikacja jako usługa lub włączenie istniejących aplikacji w chmurze.<br/>

### Usługi Storage

Rozwiązanie dużych obliczeń działa zazwyczaj na zestawie danych wejściowych i generuje dane w celu uzyskania wyników. Niektóre usługi Azure Storage używane w rozwiązaniach dużych obliczeń:

* [Obiekt blob, tabela i Queue Storage](https://azure.microsoft.com/documentation/services/storage/) — do zarządzania dużymi ilościami danych niestrukturalnych, danych NoSQL oraz komunikatów dla przepływów pracy i komunikacji. Można na przykład użyć magazynu obiektów Blob w procesach aplikacji obejmujących duże zestawy danych technicznych lub obrazy wejściowe czy pliki nośników. Kolejek można użyć do komunikacji asynchronicznej w ramach rozwiązania. Zobacz temat [Wprowadzenie do usługi Microsoft Azure Storage](../storage/storage-introduction.md).

* [Azure File Storage](https://azure.microsoft.com/services/storage/files/) — udostępnia wspólne pliki i dane na platformie Azure przy użyciu standardowego protokołu SMB, który jest niezbędny do niektórych rozwiązań klastra HPC.

* [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) — zapewnia rozproszony system plików Apache Hadoop na dużą skalę dla chmury, co jest szczególnie przydatne w przypadku analizy wsadowej, w czasie rzeczywistym i interaktywnej.

### Usługi danych i analizy

Niektóre scenariusze funkcji dużych obliczeń obejmują przepływy danych na dużą skalę lub generowanie danych wymagających dalszego przetwarzania lub analizy. Dlatego też platforma Azure udostępnia szereg usług obejmujących dane i analizę. Są to m.in. :

* [Fabryka danych](https://azure.microsoft.com/documentation/services/data-factory/) — tworzy przepływy pracy oparte na danych (potoki), które dołączają, agregują i przekształcają dane z lokalnych, opartych na chmurze oraz internetowych magazynów danych.

* [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/) — zapewnia kluczowe funkcje systemu zarządzania relacyjnymi bazami danych na serwerze Microsoft SQL Server.

* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) — wdraża i udostępnia klastry Apache Hadoop oparte na systemie Windows Server lub Linux w chmurze, aby zarządzać danymi big data, analizować je i raportować.

* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) — ułatwia tworzenie, testowanie i obsługę rozwiązań z zakresu analiz predykcyjnych oraz zarządzanie nimi w ramach w pełni zarządzanej usługi.

### Usługi dodatkowe

Rozwiązanie funkcji dużych obliczeń może wymagać innych usług Azure do połączenia z zasobami lokalnie lub w innych środowiskach. Przykłady:

* [Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) — tworzy logicznie wyizolowaną sekcję na platformie Azure, aby połączyć zasoby Azure z lokalnym centrum danych lub jednym komputerem klienckim przy użyciu protokołu IPSec. Umożliwia aplikacjom z funkcją dużych obliczeń uzyskanie dostępu do danych lokalnych, usług Active Directory i serwerów licencji.

* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) — tworzy prywatne połączenie między centrami danych firmy Microsoft oraz infrastrukturą, która występuje lokalnie lub w środowisku współlokacji, zapewniając większe bezpieczeństwo, większą niezawodność i szybkość oraz mniejsze opóźnienia niż typowe połączenia przez Internet.

* [Service Bus](https://azure.microsoft.com/documentation/services/service-bus/) — zapewnia kilka metod komunikacji lub wymiany danych przez aplikacje, niezależnie od tego, czy znajdują się na platformie Azure, na innej platformie w chmurze czy w centrum danych.

## Następne kroki

* Wskazówki techniczne dotyczące tworzenia rozwiązania znajdują się w temacie [Technical Resources for Batch and HPC](big-compute-resources.md) (Zasoby techniczne dotyczące usługi Batch i HPC).

* Omów opcje Azure (takie jak Cycle Computing czy UberCloud) z partnerami.

* Przeczytaj informacje o rozwiązaniach funkcji dużych obliczeń Azure oferowanych przez firmy [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/) oraz [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088).

* Najnowsze informacje można znaleźć na [blogu zespołu Microsoft HPC i usługi Batch](http://blogs.technet.com/b/windowshpc/) oraz [blogu platformy Azure](https://azure.microsoft.com/blog/tag/hpc/).

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png



<!--HONumber=jun16_HO2-->


