---
title: "Projektowanie szablonów platformy Azure złożonych rozwiązań | Dokumentacja firmy Microsoft"
description: "Przedstawiono najlepsze rozwiązania dotyczące projektowania szablonów usługi Azure Resource Manager w złożonych scenariuszach"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ce1141d6-ece7-4976-acea-1db1f775409e
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: dcc31f7a8c85a8f7fbd554371a66fb1e348bca17
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="design-patterns-for-azure-resource-manager-templates-when-deploying-complex-solutions"></a>Wzorce projektowe dla szablonów usługi Azure Resource Manager w przypadku wdrażania złożonych rozwiązań
Przy użyciu elastyczne podejście oparte na szablonach usługi Azure Resource Manager, można wdrożyć złożonej topologii szybkie i spójne. Dostosowanie tych wdrożeń łatwe jak ofert core rozwijać lub w celu uwzględnienia wariantów dla scenariuszy izolowanej lub klientów.

Ten temat jest częścią większej oficjalny dokument. Aby uzyskać pełne papieru, Pobierz [światowej klasy Azure zasobów Menedżer szablonów zagadnienia i sprawdzonych rozwiązań](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

Szablony łączyć korzyści z podstawowej usługi Azure Resource Manager z zdolności adaptacyjnych i czytelność kodu JavaScript Object Notation (JSON). Za pomocą szablonów, można:

* Spójne wdrażanie topologii i ich obciążeń.
* Zarządzanie wszystkich zasobów w aplikacji ze sobą przy użyciu grup zasobów.
* Zastosowanie kontroli dostępu opartej na rolach (RBAC) w celu udzielenia dostępu odpowiednie do użytkowników, grup i usług.
* Użycie znakowanie skojarzenia uprościć zadania, takie jak pakiety zbiorcze rozliczeń.

Ten artykuł zawiera szczegółowe informacje na scenariusze użycia, architektura i wzorce implementacji zidentyfikowanego w trakcie naszych sesji projektowania i implementacji rzeczywistych szablonu z klientami Azure zespół doradczy klientów (AzureCAT). Daleko od uczelni, tych sposobów są sprawdzonych rozwiązań poinformowany przez tworzenie szablonów dla 12 technologii top OSS opartych na systemie Linux, w tym: Apache Kafka, Apache Spark, Cloudera, Couchbase, Hortonworks HDP, obsługiwane przez Apache Enterprise DataStax Cassandra, Elasticsearch Wpięć, bazy danych MongoDB, PostgreSQL, Redis i Nagios. 

W tym artykule współużytkuje te wskazówki ułatwiające projektowania szablonów usługi Azure Resource Manager klas world sprawdzone.  

W naszym Praca z klientami określiliśmy kilka środowisko konsumowania szablon Menedżera zasobów przedsiębiorstwa, s integratory systemu (SI) i udostępnionych woluminów klastra. Poniższe sekcje zawierają ogólne omówienie typowych scenariuszy i wzorce dla różnych typów klientów.

## <a name="enterprises-and-system-integrators"></a>Przedsiębiorstwach i integratorów systemów. platforma
W dużych organizacjach widzimy często konsumentów dwóch szablonów usługi Resource Manager: zespoły rozwoju oprogramowania wewnętrzny i firmowych IT. Znaleźliśmy który scenariusze mapy SIs do scenariuszy dla przedsiębiorstw, więc te same kwestie.

### <a name="internal-software-development-teams"></a>Zespoły rozwoju oprogramowania wewnętrznego
Jeśli zespół osiąga oprogramowania Twojej firmy, Szablony zapewniają prosty sposób na szybkie wdrożenie technologie do zastosowania w rozwiązaniach specyficzne dla firm. Szablony umożliwia również szybkie tworzenie środowisk szkolenia, umożliwiające członkom zespołu uzyskanie umiejętności niezbędne.

Za pomocą szablonów jako — jest lub rozszerzyć lub tworzą je, aby zmieścił się w Twoim potrzebom. Za pomocą znakowania w szablonach, można udostępnić rozliczeń Podsumowanie różnych widokach, takich jak zespołu, projekt, osoby i education.

Firmy często mają zespoły deweloperów oprogramowania do tworzenia szablonu spójne wdrażanie rozwiązania. Szablonu ułatwia ograniczenia, więc niektóre elementy w tym środowisku pozostają z ustaloną i nie można zastąpić. Na przykład bank może wymagać szablon zawiera RBAC, więc programisty nie może skorygować banku rozwiązania do przesyłania danych do konta magazynu osobistego.

### <a name="corporate-it"></a>Firmowej IT
Działy IT firmy zwykle korzystają z szablonów do dostarczania chmury możliwości pojemności i hostowanych w chmurze.

#### <a name="cloud-capacity"></a>Pojemność w chmurze
Jest to często stosowana metoda firmowej grup IT zapewnienie pojemności chmury dla zespołów "rozmiary obrazów na koszulki", które są standardowe oferty rozmiary, takich jak małych, średnich i dużych. Oferty obrazów na koszulki o rozmiarze można łączyć różnych typów zasobów ilości zapewniając poziom normalizacji, który umożliwia szablony i zarządzać nimi. Szablony dostarczanie pojemności w spójny sposób, który wymusza zasady firmowe i używa znakowanie, aby zapewnić obciążenia zwrotnego na korzystanie z organizacji.

Na przykład może być konieczne Podaj rozwoju, testów lub produkcji środowisk, w ramach których zespoły rozwoju oprogramowania można wdrożyć ich rozwiązania. W środowisku występuje topologii sieci wstępnie zdefiniowane i elementy, które można zmienić zespoły rozwoju oprogramowania, takie jak zasady dostępu do wglądu publicznego Internetu i pakietów. Ponadto klientowi mogą przysługiwać ról w danej organizacji dla tych środowisk różne uprawnienia dla środowiska.

#### <a name="cloud-hosted-capabilities"></a>Możliwości hostowanych w chmurze
Szablony służy do obsługi hostowanych w chmurze możliwości, takie jak pakiety oprogramowania lub złożone oferty, które są dostępne dla wewnętrznego rodzajach działalności gospodarczej. Przykładem złożone oferty może być analytics jako usługa — analytics, wizualizacji i innych technologii — dostarczone w konfiguracji zoptymalizowane, połączonych w topologii sieci wstępnie zdefiniowane.

Zagadnienia dotyczące zabezpieczeń i roli ustala pojemności chmury oferty, w którym są wbudowane dotyczy możliwości hostowanych w chmurze. Te możliwości są oferowane zmian lub jako zarządzanych usług. Dla ról drugie, ograniczonego dostępu są wymagane do włączenia dostępu do środowiska w celu zarządzania nim.

## <a name="cloud-service-vendors"></a>Dostawców usługi w chmurze
Po rozmowie z wielu woluminów CSV, określiliśmy, że wiele metod, które należy wykonać w celu wdrażania usług dla klientów i skojarzone wymagania.

### <a name="csv-hosted-offering"></a>Hostowana CSV oferty
Jeśli w ramach własnej subskrypcji platformy Azure Twojej oferty, typowe są dwa podejścia hostingu: wdrażanie różnych wdrożenia dla każdego klienta lub wdrożeniu jednostek skalowania, które stanowi podstawę współużytkowanej infrastruktury używane dla wszystkich klientów.

* **Różne wdrożenia dla każdego klienta.** Różne wdrożeń na klienta wymaga stałej topologie znanych konfiguracji. Te wdrożenia może mieć rozmiary inną maszynę wirtualną (VM), różną liczbę węzłów i różnych ilości powiązanym magazynie. Znakowanie wdrożeń służy do rozliczeń zbiorczy każdego klienta. RBAC może być włączone, aby umożliwić klientom dostęp do aspektów ich środowiska chmury.
* **Jednostki skalowania w udostępnionych środowiskach wielodostępnych.** Szablon może reprezentować jednostki skalowania dla środowiska z wieloma dzierżawcami. W takim przypadku infrastruktura jest używana do obsługi wszystkich klientów. Wdrożenia reprezentuje grupę zasobów dostarczających poziom wydajności hostowanej oferty, takie jak liczba użytkowników oraz liczbę transakcji. Te jednostki skali są zwiększyć lub zmniejszyć, jak długo żądanie.

### <a name="csv-offering-injected-into-customer-subscription"></a>Oferta CSV do subskrypcji klienta
Można wdrażać oprogramowanie do subskrypcji własnością klientów końcowych. Szablony umożliwia wdrażanie różnych wdrożeń do klienta konto platformy Azure.

Te wdrożenia użycie funkcji RBAC, aby można było zaktualizować i Zarządzaj wdrożeniem w ramach konta klienta.

### <a name="azure-marketplace"></a>Azure Marketplace
Reklamy i sprzedawać Twojej oferty za pośrednictwem portalu marketplace, takich jak Azure Marketplace można tworzyć szablony do dostarczania różne typy wdrożenia, które są uruchamiane z konta Azure klienta. Te wdrożenia distinct można przedstawić zwykle jako rozmiar koszulki (małe, średnie, duża), typ produktu/odbiorców (społeczności, developer i enterprise) lub typu funkcji (podstawowe o wysokiej dostępności).  W niektórych przypadkach te typy umożliwiają określenie niektóre atrybuty wdrożenia, takie jak typ maszyny Wirtualnej lub liczbę dysków.

## <a name="oss-projects"></a>Projekty OSS
W obrębie projektów typu open source szablony usługi Resource Manager Włącz społeczności wdrożyć rozwiązanie szybko przy użyciu sprawdzonych rozwiązań. Szablony można przechowywać w repozytorium GitHub, więc społeczności je skorygować w czasie. Użytkownicy wdrażania tych szablonów w ich własnej subskrypcji platformy Azure.

W poniższych sekcjach podano czynności, które należy wziąć pod uwagę przed rozpoczęciem projektowania rozwiązania.

## <a name="identifying-what-is-outside-and-inside-a-vm"></a>Identyfikowanie, co to jest poza i wewnątrz maszyny Wirtualnej
Podczas projektowania szablonu, warto przyjrzeć się wymagania w zakresie co to jest poza i wewnątrz maszyn wirtualnych (VM):

* Poza oznacza maszyn wirtualnych i innych zasobów wdrożenia, takie jak topologii sieci, znakowanie, odwołuje się do certyfikatów/klucze tajne i kontroli dostępu opartej na rolach. Te zasoby są częścią szablonu.
* Wewnątrz oznacza zainstalowanego oprogramowania i ogólny stan żądanej konfiguracji. Inne mechanizmy, takie jak rozszerzeń maszyny Wirtualnej lub skrypty, są używane w całości lub częściowo. Mechanizmy te mogą być zidentyfikowane i wykonywane przez szablon, ale nie są w nim.

Typowe przykłady działań, które należy wykonać "wewnątrz pola" —  

* Instalowanie lub usuwanie ról i funkcji serwera
* Instalowanie i konfigurowanie oprogramowania na poziomie węzła lub klastra
* Wdrażanie witryn sieci Web na serwerze sieci web
* Wdrażanie schematów bazy danych
* Zarządzanie rejestru lub inne ustawienia konfiguracji
* Zarządzanie plikami i katalogami
* Uruchamianie, zatrzymywanie i zarządzania procesami i usługami
* Zarządzanie grupami lokalnymi i kont użytkowników
* Zainstaluj pakiety i zarządzać nimi (.msi, .exe, yum itp.)
* Zarządzanie zmienne środowiskowe
* Uruchom skrypty natywnego (środowiska Windows PowerShell, bash, itp.)

### <a name="desired-state-configuration-dsc"></a>Konfiguracja żądanego stanu (DSC)
Planowanie wewnętrzny stan klasy poza wdrażania maszyn wirtualnych, chcesz upewnij się, że to wdrożenie nie "rozchodzenia" z konfiguracji, który został zdefiniowany i zaznaczone do kontroli źródła. To podejście zapewnia deweloperów lub Operatorzy nie wprowadzać zmian ad hoc środowisko, które nie są sprawdzane, przetestowane lub zarejestrowane w kontroli źródła. Ten formant jest ważne, ponieważ ręcznej zmiany nie są w kontroli źródła. Również nie są częścią wdrożenia standardowego i będzie miał wpływ na przyszłe zautomatyzowanych wdrożeń oprogramowania.

Poza pracownikom wewnętrzny konfiguracji żądanego stanu są też ważne z punktu widzenia zabezpieczeń. Przed hakerami próbujesz regularnie naruszyć i wykorzystać systemów oprogramowania. Gdy to się powiedzie, jest często stosowanym rozwiązaniem zainstalować pliki, a w przeciwnym razie zmiany stanu systemu ze złamanymi zabezpieczeniami. Za pomocą konfiguracji żądanego stanu, można zidentyfikować delty między żądaną i rzeczywistego stanu i przywrócenia znanej konfiguracji.

Brak rozszerzenia zasobu dla najbardziej popularnych mechanizmów DSC - PowerShell DSC, Chef i Puppet. Każdy z tych rozszerzeń można wdrożyć początkowy stan maszyny Wirtualnej i również upewnij się, że żądany stan jest przechowywany.

## <a name="common-template-scopes"></a>Typowe zakresy szablonu
W naszym środowisku możemy przedstawiono trzy zakresy szablony klucza rozwiązania wyłonić. Te trzy zakresy — wydajność, możliwości i end-to-end rozwiązania — są opisane w poniższych sekcjach.

### <a name="capacity-scope"></a>Zakres pojemności
Zakres pojemności oferuje zestaw zasobów w standardowej topologii, który jest wstępnie skonfigurowana zgodnie z przepisami i zasad. Najbardziej typowym przykładem jest wdrażana przez środowisko deweloperskie standardowe w scenariuszu informatycznego w firmie lub tożsamości usługi.

### <a name="capability-scope"></a>Zakres możliwości
Zakres możliwości koncentruje się na wdrażanie i Konfigurowanie topologii dla danej technologii. Typowe scenariusze, w tym technologii, takich jak SQL Server, Cassandra, Hadoop.

### <a name="end-to-end-solution-scope"></a>Zakres end-to-end rozwiązania
Zakres rozwiązania End-to-End docelowe poza jednym możliwości, a zamiast tego koncentruje się na dostarczaniu kompleksowe rozwiązania składającej się z wielu funkcji.  

Zakres zakres rozwiązania szablonu manifesty jako zestaw co najmniej jeden zakres możliwości szablonów zasobów specyficznych dla rozwiązania, logiki i żądanego stanu. Przykład szablonu o zakresie rozwiązania to szablon rozwiązania potoku pełne dane. Szablon może mieszać topologii określonego rozwiązania i stan z wielu szablonów zakres możliwości rozwiązania, takie jak Kafka, Storm i Hadoop.

## <a name="choosing-free-form-vs-known-configurations"></a>Wybranie dowolnego a znanych konfiguracji
Może początkowo uważasz, że szablon powinien zapewnić konsumentów największą elastyczność, ale wiele kwestii wpłynąć na wybór, czy używać dowolnych konfiguracje a znanych konfiguracji. W tej sekcji wymieniono wymagania dotyczące klienta klucza i kwestii technicznych, które w kształcie podejście udostępnione w tym dokumencie.

### <a name="free-form-configurations"></a>Konfiguracje dowolnych
Na pierwszy rzut oka konfiguracje dowolnych dźwiękowej idealne. Pozwala na wybór typu maszyny Wirtualnej i podaj dowolną liczbę węzłów i dołączone dyski dla tych węzłów — i zrobić jako parametry szablonu. Jednak ta metoda nie jest idealnym rozwiązaniem w niektórych scenariuszach.

W [rozmiary maszyn wirtualnych](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)różnych typach maszyn wirtualnych i dostępne rozmiary są identyfikowane i dyski każdej liczby trwałe (2, 4, 8, 16 i 32), który może zostać dołączony. Każdy dysk dołączony zapewnia 500 IOPS i wielu z tych dysków może puli dla mnożnik tej liczby IOPS. Na przykład 16 dysków może puli zapewnienie 8000 IOPS. Buforowanie wykonuje się za pomocą konfiguracji systemu operacyjnego, za pomocą funkcji miejsca do magazynowania systemu Windows firmy Microsoft lub macierzy tanich dysków (RAID) w systemie Linux.

Konfiguracja dowolnych umożliwia wybór kilka wystąpień maszyn wirtualnych, maszyny Wirtualnej w różnych typów i rozmiar dla tych wystąpień różne dyski dla typu maszyny Wirtualnej i co najmniej jednego skryptu skonfigurować zawartość maszyny Wirtualnej.

Bardzo często, że wdrożenie może mieć wiele typów węzłów, takich jak wzorzec i węzły danych, tego rodzaju elastyczności jest często podane dla każdego typu węzła.

Po rozpoczęciu wdrażania klastrów żadnego znaczenia rozpoczęciem pracy z tych scenariuszy złożonych. Jeśli zostały wdrażanie klastra usługi Hadoop, na przykład z 8 węzłów głównych i węzłów 200 danych oraz puli 4 dołączonych dysków w każdym węźle głównym i puli 16 dysków dołączonych na węzeł danych, trzeba 208 maszyn wirtualnych i 3,232 dysków, aby zarządzać.

Konto magazynu będzie ograniczenie przepustowości żądań powyżej jego zidentyfikowanych 20 000 ograniczyć transakcje na sekundę, dlatego należy przyjrzeć się partycjonowanie konta magazynu i umożliwia określenie odpowiedniej liczby kont magazynu w celu uwzględnienia tej topologii obliczeń. Podana wieloma obsługiwane przez metody dowolne kombinacje, obliczenia dynamiczne są wymagane do określenia odpowiedniego partycjonowania. Język szablonu usługi Azure Resource Manager nie zawiera obecnie funkcji matematycznych, należy wykonać te obliczenia w kodzie, generowania unikatowych, ustalony szablonu z odpowiednie informacje.

W organizacji IT i scenariusze SI ktoś Obsługa szablonów i obsługuje wdrożonej topologii dla organizacji, co najmniej jeden. To dodatkowe obciążenie — szablony dla każdego klienta i różnych konfiguracjach — równocześnie pożądane.

Te szablony można użyć do wdrożenia środowiska klienta subskrypcji platformy Azure, ale zespoły IT firmy i woluminy CSV zwykle wdrożyć je do ich własnych subskrypcji przy użyciu funkcji obciążeń zwrotnych do klientów. W tych scenariuszach celem jest wdrażanie pojemność dla wielu klientów w puli subskrypcji i zachowanie wdrożenia gęsto umieszczane w subskrypcji, aby zminimalizować ilość okablowania subskrypcji — to znaczy więcej subskrypcji do zarządzania. O rozmiarze naprawdę dynamiczne wdrożenia osiągnięcie tego typu gęstość wymaga zachować ostrożność podczas planowania i projektowania dodatkowe szkieletów pracy w imieniu organizacji.

Ponadto nie można utworzyć subskrypcji za pośrednictwem wywołania interfejsu API, ale należy to zrobić ręcznie za pośrednictwem portalu. Zwiększa liczbę subskrypcji, wszelkie wynikowy przetwarzania subskrypcji wymaga udziału człowieka — nie mogły zostać zautomatyzowane. O wiele zmienności rozmiary wdrożeń będzie musiał wstępnego zainicjowania obsługi liczbę subskrypcji ręcznie, aby upewnić się, że dostępnych subskrypcji.

Biorąc pod uwagę następujące czynniki Konfiguracja naprawdę dowolnych jest mniej atrakcyjne niż co najpierw blush.

### <a name="known-configurations--the-t-shirt-sizing-approach"></a>Znane konfiguracje — metoda ustalania rozmiaru obrazów na koszulki
Raczej nie oferują szablon, który zapewnia elastyczność całkowitej i niezliczonych zmian, wiemy z doświadczenia wspólnego wzorca jest zapewnienie możliwość wybrania znanych konfiguracji — w efekcie rozmiary obrazów na standardowe koszulki takich jak piaskownicy, małych, średnich i dużych. Inne przykłady rozmiary obrazów na koszulki są ofert produktów, takich jak community edition lub enterprise edition.  W innych przypadkach może być konfiguracje specyficznego dla obciążenia technologii — takie jak ograniczyć mapy lub nie sql.

Wiele organizacji IT organizacji, dostawców OSS i SIs udostępnianie ich ofert dzisiaj w ten sposób w lokalnej, zwirtualizowanych środowiskach (przedsiębiorstwa) lub oprogramowania jako usługa (SaaS) oferty (woluminy CSV i OSVs).

Takie podejście zapewnia dobre, znanej konfiguracji o różnych rozmiarach, które są wstępnie skonfigurowane dla klientów. Bez znanych konfiguracji klientów końcowych musi ustalić klastra zmiany rozmiaru na ich własnych uwzględnieniu ograniczenia zasobów platformy i wykonywanie obliczeń do identyfikowania wynikowy partycjonowanie konta magazynu i innych zasobów (ze względu na rozmiar klastra i zasoby ograniczenia). Znane konfiguracje umożliwiają klientom łatwe wybierz prawa Rozmiar koszulki — to znaczy danego wdrożenia. Oprócz tworzenia lepsze środowisko dla odbiorcy, łatwiej jest niewielka liczba znanych konfiguracji obsługuje i ułatwiają dostarczanie wyższego poziomu gęstości.

To podejście znanej konfiguracji koncentruje się na rozmiary obrazów na koszulki mogą także mieć różną liczbę węzłów w rozmiarze. Na przykład mały rozmiar koszulki może być między węzłami 3 do 10.  Rozmiar koszulki będzie przeznaczony obsłużyć maksymalnie 10 węzłów i umożliwiają konsumenta dowolnej postaci opcje do maksymalnego rozmiaru zidentyfikowane.  

Rozmiar koszulki, na podstawie typu obciążenie może być więcej dowolnej postaci charakteru pod względem liczby węzłów, które można wdrożyć, ale mają rozmiar węzła różne obciążenia i konfiguracji oprogramowania na węźle.

Rozmiary obrazów na koszulki oparte na ofert produktów, takich jak społeczność lub przedsiębiorstwa, może mieć typów różnych zasobów i maksymalną liczbę węzłów, które można wdrożyć, zwykle powiązane zagadnień związanych z licencjami lub dostępność funkcji w różnych ofert.

Można również zapewnić klientom z unikatowy wariantów za pomocą szablonów opartych na formacie JSON. Podczas pracy nad wartości odstających, możesz dołączyć do nich odpowiednie planowanie i zagadnienia dotyczące projektowania, pomocy technicznej i analiza kosztów.

Na podstawie scenariuszy użycia szablonu i wymagania określone w chwili rozpoczęcia tego dokumentu, określiliśmy, że wzorzec do rozkładu szablonu.

## <a name="capacity-and-capability-scoped-solution-templates"></a>Pojemność i szablony zakres możliwości rozwiązania
Dekompozycji zapewnia podejściu tworzenia szablonu, że obsługuje ponowne użycie, rozszerzalności, testowania i narzędzi. Ta sekcja zawiera szczegóły na jak podejście dekompozycji można zastosować do szablonów z zakresem możliwości.

W takie podejście szablonu głównego odbiera wartości parametrów od konsumenta szablonu, a następnie łączy kilka typów szablonów i skrypty downstream jak pokazano poniżej. Podać wartości i szablonów połączonych są używane parametry, zmienne statyczne i zmienne wygenerowany.

![Parametry szablonu](./media/best-practices-resource-manager-design-templates/template-parameters.png)

**Parametry są przekazywane do głównego szablonu następnie do połączonego szablonów**

Poniższe sekcje skupić się na typy szablonów i skrypty, które jest cenowego jednego szablonu. Sekcje stanowią podejścia do przekazywania informacji o stanie między szablonów. Każdy szablon i typy skryptu w obrazie opisano wraz z przykładami. Na przykład kontekstowe, zobacz "umieścić go razem: Przykładowe zastosowanie" dalszej części tego dokumentu.

### <a name="template-metadata"></a>Metadane szablonu
Metadane szablonu (plik metadata.json) zawiera opis szablonu w formacie JSON, który może zostać odczytany przez ludzi i systemów oprogramowania par klucz/wartość.

![Metadane szablonu](./media/best-practices-resource-manager-design-templates/template-metadata.png)

**Metadane szablonu jest opisany w pliku metadata.json**

Agenci oprogramowanie można pobrać pliku metadata.json i publikować informacje i łącza do szablonu w katalogu lub strony sieci web. Elementy obejmują *itemDisplayName*, *opis*, *podsumowania*, *githubUsername*, i *dateUpdated*.

Poniżej przedstawiono przykładowy plik w całości.

    {
        "itemDisplayName": "PostgreSQL 9.3 on Ubuntu VMs",
        "description": "This template creates a PostgreSQL streaming-replication between a master and one or more slave servers each with 2 striped data disks. The database servers are deployed into a private-only subnet with one publicly accessible jumpbox VM in a DMZ subnet with public IP.",
        "summary": "PostgreSQL stream-replication with multiple slave servers and a publicly accessible jumpbox VM",
        "githubUsername": "arsenvlad",
        "dateUpdated": "2015-04-24"
    }

### <a name="main-template"></a>Główny szablonu
Szablonie głównym odbiera parametrów z użytkownikiem, używa tych informacji, aby wypełnić zmienne obiekt złożony i wykonuje połączonego szablonów.

![Główny szablonu](./media/best-practices-resource-manager-design-templates/main-template.png)

**Szablonie głównym odbiera parametrów z użytkownikiem**

Jeden parametr, który został dostarczony jest typem znanej konfiguracji znanej także jako parametr rozmiaru obrazów na koszulki ze względu na jego wartości standardowych, takich jak małych, średnich i dużych. W praktyce można użyć tego parametru na wiele sposobów. Aby uzyskać więcej informacji zobacz temat "Znanej konfiguracji zasobów szablonu" w dalszej części tego dokumentu.

Niektóre zasoby są wdrażane niezależnie od tego, znanej konfiguracji określonej przez parametr użytkownika. Te zasoby są udostępniane za pomocą szablonu pojedynczego zasobu udostępnionego i są współużytkowane przez innych szablonów, a szablon zasób udostępniony zostanie wykonany w pierwszym.

Niektóre zasoby są wdrażane opcjonalnie niezależnie od określonej konfiguracji znane.

### <a name="shared-resources-template"></a>Udostępnione zasoby szablonu
Ten szablon udostępnia zasoby, które są wspólne dla wszystkich znanych konfiguracji. Zawiera sieć wirtualną, zestawów dostępności i inne zasoby, które są wymagane niezależnie od tego szablonu znanej konfiguracji, które zostało wdrożone.

![Zasobów szablonu](./media/best-practices-resource-manager-design-templates/template-resources.png)

**Udostępnione zasoby szablonu**

Nazwy zasobów, takie jak nazwa sieci wirtualnej, są oparte na szablonie głównym. Można określić je jako zmienną w obrębie tego szablonu lub ich odbierania jako parametr użytkownika, zgodnie z wymaganiami organizacji.

### <a name="optional-resources-template"></a>Opcjonalne zasoby szablonu
Szablon opcjonalnych zasobów zawiera zasoby programowe wdrażane na podstawie wartości parametr lub zmienna.

![Opcjonalne zasoby](./media/best-practices-resource-manager-design-templates/optional-resources.png)

**Opcjonalne zasoby szablonu**

Na przykład umożliwia szablonu zasobów opcjonalnie skonfigurować jumpbox, który umożliwia bezpośredni dostęp do wdrożonych środowisko z publicznej sieci Internet. Określa, czy powinno być włączone jumpbox użyje parametr lub zmienna i *concat* funkcji do kompilacji docelową nazwę dla szablonu, takie jak *jumpbox_enabled.json*. Łączenie szablonu czy należy zainstalować jumpbox wynikowy zmiennej.

Możesz połączyć szablonu opcjonalnych zasobów z wielu miejscach:

* W razie potrzeby do każdego wdrożenia połączyć oparte na parametr szablonu zasobów udostępnionych.
* W razie potrzeby zaznacz znanych konfiguracji — na przykład zainstalować tylko w dużych wdrożeniach — połączyć oparte na parametr lub zmienna zmiennych szablonu znanej konfiguracji.

Określa, czy dany zasób jest opcjonalny mogą nie kierować konsumenta szablonu tylko przez dostawcę szablonu. Na przykład może wymagać spełniają wymagania danego produktu lub dodatek produktu (wspólne dla woluminów CSV) lub do wymuszania zasad (w przypadku SIs i rozwiązaniami INFORMATYCZNYMI dla firm grupy). W takich przypadkach można użyć zmiennej określa, czy zasób powinny zostać wdrożone.

### <a name="known-configuration-resources-template"></a>Szablon zasobów znanej konfiguracji
W szablonie głównym parametru może być narażony na Zezwalaj szablon konsumenta Określ żądany znanej konfiguracji do wdrożenia. Często ta konfiguracja znane korzysta z podejścia rozmiar obrazów na koszulki z zestawem rozmiary stałym konfiguracji, takie jak piaskownicy, małe, średnie i duże.

![Zasoby znanej konfiguracji](./media/best-practices-resource-manager-design-templates/known-config.png)

**Szablon zasobów znanej konfiguracji**

Podejście rozmiar obrazów na koszulki to powszechnie używane, ale parametry może reprezentować dowolny zbiór znanych konfiguracji. Na przykład można określić zestaw środowisk dla aplikacji przedsiębiorstwa, takich jak projektowanie, testów i produktu. Można także użyć go do usługi w chmurze do reprezentowania różnych jednostek, wersje produktu lub konfiguracje produktu, takie jak społeczności deweloperów i Enterprise.

Podobnie jak w przypadku szablonu zasobów udostępnionych zmiennych są przekazywane do szablonu znanych konfiguracji z poziomu:

* Użytkownik końcowy — czyli parametry, które zostały wysłane do szablonu głównego.
* Organizacja — czyli zmienne w szablonie głównym reprezentujących wymagań wewnętrznych lub zasad.

### <a name="member-resources-template"></a>Szablon elementu członkowskiego zasobów
W znanej konfiguracji co najmniej jednego elementu członkowskiego typu węzła często są dołączone. Na przykład z platformą Hadoop masz węzłów głównych i węzły danych. Jeśli instalujesz bazy danych MongoDB ma węzły danych i kryterium. Jeśli wdrażasz DataStax masz węzły danych i maszyny Wirtualnej z OpsCenter zainstalowane.

![Elementy członkowskie zasobów](./media/best-practices-resource-manager-design-templates/member-resources.png)

**Szablon elementu członkowskiego zasobów**

Każdy typ węzłów może mieć różne rozmiary maszyn wirtualnych, liczba dołączonych dysków, skryptów do instalowania i konfigurowania węzłów, konfiguracji portów dla maszyn wirtualnych, liczbę wystąpień oraz innych szczegółów. Dlatego każdy typ węzła pobiera jej członka zasobu szablon, który zawiera szczegóły dotyczące wdrażania i konfigurowania infrastruktury, a także wykonywanie skryptów do wdrożenia i skonfigurowania oprogramowania w ramach maszyny Wirtualnej.

Dla maszyn wirtualnych zazwyczaj dwa typy skryptów są używane, powszechnie wielokrotnego użytku i niestandardowych skryptów.

### <a name="widely-reusable-scripts"></a>Skrypty powszechnie wielokrotnego użytku
Można używać skryptów powszechnie wielokrotnego użytku przez wiele typów szablonów. Skonfigurowanie jednego lepsze Przykłady te skrypty powszechnie wielokrotnego użytku RAID w systemie Linux w puli dysków i uzyskać większą liczbę IOPS. Niezależnie od tego, oprogramowanie instalowane na maszynie wirtualnej skrypt umożliwia ponowne użycie sprawdzonych rozwiązań dla typowych scenariuszy.

![Skryptów wielokrotnego użytku](./media/best-practices-resource-manager-design-templates/reusable-scripts.png)

**Szablony zasobów Członkowskich można wywołać skryptów powszechnie wielokrotnego użytku**

### <a name="custom-scripts"></a>Niestandardowych skryptów
Szablony zwykle wywołać co najmniej jeden skrypty, które Instalowanie i konfigurowanie oprogramowania w maszynach wirtualnych. Wspólnego wzorca jest widoczna w dużych topologiach wdrożonym wielu wystąpień co najmniej jeden typ elementu członkowskiego. Skrypt instalacji jest inicjowane dla każdej maszyny Wirtualnej, które mogą być wykonywane równolegle, następuje skrypt instalacji, który jest wywoływana po wdrożeniu wszystkich maszyn wirtualnych (lub wszystkich maszyn wirtualnych typu danego elementu członkowskiego).

![Niestandardowych skryptów](./media/best-practices-resource-manager-design-templates/custom-scripts.png)

**Szablony zasobów Członkowskich można wywołać skryptów do określonego celu, takie jak konfiguracja maszyny Wirtualnej**

## <a name="capability-scoped-solution-template-example---redis"></a>Przykład szablonu zakres możliwości rozwiązania — Redis
Aby pokazać, jak implementacja może działać, Przyjrzyjmy się praktycznym przykładem tworzenia szablonu, który ułatwia wdrożenie i konfigurację pamięci podręcznej Redis w standardowe rozmiary obrazów na koszulki.  

Dla wdrożenia ma zestaw zasobów udostępnionych (sieci wirtualne, konta magazynu, zestawów dostępności) i opcjonalnie zasobów (jumpbox). Istnieją znane konfiguracji z wieloma reprezentowane jako rozmiary obrazów na koszulki (małe, średnie, duża), ale każdego z jednego węzła typu. Istnieją również dwa skrypty specyficzne dla celu (instalacji, konfiguracji).

### <a name="creating-the-template-files"></a>Tworzenie plików szablonu
Należy utworzyć szablon głównego o nazwie azuredeploy.json.

Utwórz szablon zasobów o nazwie resources.json udostępnionych

Tworzenie szablonu zasobów opcjonalne włączyć wdrożenie jumpbox, o nazwie jumpbox_enabled.json

Redis używa tylko jednego węzła o typie, utworzyć szablon pojedynczego zasobu elementu członkowskiego o nazwie resources.json węzła.

Z pamięci podręcznej Redis, dla których chcesz zainstalować każdego indywidualnego węzła, a następnie ponowne skonfigurowanie klastra.  Masz skrypty w celu uwzględnienia instalacja i konfigurowanie, redis klastra install.sh i setup.sh-redis klastra.

### <a name="linking-the-templates"></a>Łączenie z szablonów
Przy użyciu szablonu łączenie łącza szablonu głównego się do szablonu zasobów udostępnionych, który określa sieci wirtualnej.

Logika jest dodawany w szablonie głównym umożliwiające konsumentów szablonu określić, czy jumpbox powinny zostać wdrożone. *Włączone* wartość *EnableJumpbox* parametr wskazuje, że odbiorca chce wdrożyć jumpbox. Gdy ta wartość zostanie podana, szablon łączy *_enabled* sufiks na nazwę szablonu podstawowego dla funkcji jumpbox.

Zastosowanie szablonu głównego *dużych* wartość parametru sufiks na nazwę szablonu podstawowego rozmiary obrazów na koszulki, a następnie używa tej wartości w łącze szablonu wychodzących do *technology_on_os_large.json*.

Topologia będzie wyglądać na tej ilustracji.

![Redis szablonu](./media/best-practices-resource-manager-design-templates/redis-template.png)

**Struktura szablonu dla szablonu Redis**

### <a name="configuring-state"></a>Konfigurowanie stanu
Dla węzłów w klastrze należy wykonać dwie czynności w przypadku konfigurowania stanu, zarówno reprezentowany przez skrypty określonego celu.  Redis instaluje "redis klastra install.sh" i "redis klastra setup.sh" Ustawia klaster.

### <a name="supporting-different-size-deployments"></a>Obsługa wdrożeń inny rozmiar
Wewnątrz zmiennych szablonu rozmiar obrazów na koszulki określa liczbę węzłów każdego typu wdrożenia dla określonego rozmiaru (*dużych*). Następnie wdraża tej liczby wystąpień maszyn wirtualnych przy użyciu pętli zasobów, zapewniając unikatowe nazwy do zasobów przez dodanie nazwy węzła z liczbą sekwencji liczbową z *copyIndex()*. Te kroki dla strefy zarówno gorącego i ciepłych maszyn wirtualnych, pojawia się zgodnie z definicją w szablonie nazwy obrazów na koszulki

## <a name="decomposition-and-end-to-end-solution-scoped-templates"></a>Szablony rozwiązania w zakresie dekompozycji i end-to-end
Szablon rozwiązania w zakresie rozwiązania end-to-end koncentruje się na dostarczanie rozwiązania end-to-end.  Ta metoda jest zwykle kompozycji wielu szablonów zakres możliwości dodatkowe zasoby, logiki i stanu.

Jak wyróżniono na poniższej ilustracji ten sam model używane dla szablonów możliwości zakres zostanie rozszerzony na użytek szablonów w zakresie rozwiązania End-to-End.

Udostępnione zasoby szablonu i opcjonalnie szablony zasobów pełnią taką samą funkcję jak pojemność i możliwości zakres podejścia szablonu, ale są w zakresie kompleksowe rozwiązania.

Kompleksowe rozwiązanie zakres szablonów również można zwykle mają rozmiary obrazów na koszulki, szablon znanych zasobów konfiguracji odzwierciedla, co jest wymagane dla danej konfiguracji znane rozwiązania.

Łącza znane konfiguracji szablonu zasobów do jednego lub więcej możliwości zakresu szablony rozwiązań, które mają zastosowanie do pełnego rozwiązania, a także szablony zasobów — członek, która jest wymagana dla kompleksowe rozwiązania.

Jak rozmiar koszulki rozwiązania może być inna niż poszczególnych szablon zakres możliwości, zmienne w szablonie znanych zasobów konfiguracji są używane do podaj odpowiednie wartości dla szablonów rozwiązania możliwości podrzędny zakres należy wdrożyć odpowiedni rozmiar koszulki.

![End-to-end](./media/best-practices-resource-manager-design-templates/end-to-end.png)

**Model używany dla możliwości zakresu rozwiązania, które można łatwo rozszerzyć szablony dla zakresów szablonu kompleksowe rozwiązania**

## <a name="preparing-templates-for-the-marketplace"></a>Przygotowywanie szablony do witryny Marketplace
Łatwo poprzedniego podejście bierze pod uwagę scenariusze miejscu przedsiębiorstwa, SIs i woluminów CSV do wdrażania szablonów, samodzielnie lub ich klienci mogą wdrożyć własne.

Żądana innego, że scenariusza wdrożenia szablonu za pomocą portalu marketplace.  Takie podejście dekompozycji działa w przypadku witryny marketplace, jak również z niewielkimi zmianami.

Jak wspomniano wcześniej, szablonów można można użyć do zaoferowania wdrożenia różne typy dla sprzedaży w witrynie marketplace. Typy wdrożeń różne może być rozmiary obrazów na koszulki (małe, średnie, duża), typ produktu/odbiorców (społeczności, developer i enterprise) lub typu funkcji (podstawowe o wysokiej dostępności).

Jak pokazano poniżej, kompleksowe rozwiązania lub szablonów zakres możliwości mogą zostać łatwo użyte listy różnych konfiguracji znane w witrynie marketplace.

Parametry szablonu głównego są najpierw zmodyfikowane w celu usunięcia przychodzących parametr o nazwie tshirtSize.

Podczas mapowania typów wdrożenia różne znane konfiguracji szablonu zasobów, muszą wspólnych zasobów i można znaleźć w szablonie zasobów udostępnionych i potencjalnie te w opcjonalne szablony zasobów konfiguracji.

Jeśli chcesz opublikować szablon do witryny marketplace, należy ustanowić różne kopie Main szablonu zastępującym wcześniej dostępne dla ruchu przychodzącego parametr tshirtSize ze zmienną osadzone w szablonie.

![Portal Marketplace](./media/best-practices-resource-manager-design-templates/marketplace.png)

**Dostosowania rozwiązania o zakresie szablonu witryny marketplace**

## <a name="next-steps"></a>Następne kroki
* Aby dowiedzieć się więcej na temat udostępniania stanu do i z szablonów, zobacz [udostępniania stanu w szablonach usługi Azure Resource Manager](best-practices-resource-manager-state.md).
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).

