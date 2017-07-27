Rozwiązania w chmurze platformy Azure są oparte na maszynach wirtualnych (emulacja fizycznego sprzętu komputerowego), co umożliwia elastyczne tworzenie pakietów wdrożeń oprogramowania oraz lepszą konsolidację zasobów niż w przypadku sprzętu fizycznego. W przypadku kontenerów i ekosystemu platformy [Docker](https://www.docker.com) znacznie wzrosły możliwości projektowania i dostarczania oprogramowania rozproszonego oraz zarządzania nim. Kod aplikacji w kontenerze jest odizolowany od maszyny wirtualnej hosta i innych kontenerów w ramach tej samej maszyny wirtualnej. Izolacja zapewnia większą elastyczność projektowania i wdrażania.

Platforma Azure umożliwia skorzystanie z następujących zalet platformy Docker:

* [wiele](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) [różnych](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) sposobów tworzenia hostów platformy Docker w zależności od potrzeb
* usługa [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) służąca do tworzenia klastrów hostów kontenera przy użyciu koordynatorów, takich jak **marathon** i **swarm**.
* usługa [Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md) i [szablony grup zasobów](../articles/resource-group-authoring-templates.md) służące do uproszczenia wdrażania i aktualizowania złożonych aplikacji rozproszonych
* integracja z wieloma narzędziami do zarządzania konfiguracją, zarówno własnymi, jak i typu „open source”

Dodatkowo w związku z tym, że możliwe jest programowe tworzenie maszyn wirtualnych i kontenerów systemu Linux na platformie Azure, można także używać narzędzi *aranżacji* maszyny wirtualnej oraz kontenera do tworzenia grup maszyn wirtualnych i wdrażania aplikacji w ramach kontenerów systemu Linux oraz (od teraz) [kontenerów systemu Windows](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

W tym artykule nie tylko omówiono te pojęcia w sposób ogólny, ale także dołączono wiele linków do dodatkowych informacji, samouczków i produktów związanych z użyciem kontenera i klastra na platformie Azure. Jeśli te informacje są Ci już znane i potrzebujesz uzyskać tylko linki, znajdują się one w sekcji dotyczącej [narzędzi do pracy z kontenerami](#tools-for-working-with-azure-vms-and-containers).

## <a name="the-difference-between-virtual-machines-and-containers"></a>Różnica między maszynami wirtualnymi i kontenerami
Maszyny wirtualne działają w izolowanym środowisku wirtualizacji sprzętowej udostępnianym przez [funkcję hypervisor](http://en.wikipedia.org/wiki/Hypervisor). Na platformie Azure usługa [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/) obsługuje wszystko to za Ciebie: konieczne jest tylko utworzenie maszyn wirtualnych przez wybranie systemu operacyjnego i skonfigurowanie go lub przekazanie niestandardowego obrazu maszyny wirtualnej. Maszyny wirtualne to sprawdzona w praktyce technologia i istnieje wiele narzędzi służących do zarządzania działającymi w ich ramach systemami operacyjnymi i aplikacjami.  Aplikacje na maszynie wirtualnej są ukryte przed systemem operacyjnym hosta. Maszyna wirtualna dla korzystających z niej użytkowników lub aplikacji stanowi autonomiczny komputer fizyczny.

[Kontenery systemu Linux](http://en.wikipedia.org/wiki/LXC) oraz kontenery utworzone i obsługiwane za pomocą narzędzi platformy Docker nie używają funkcji hypervisor do zapewnienia izolacji. Za pomocą kontenerów ich host używa procesu i funkcji izolacji systemu plików jądra systemu Linux w celu udostępnienia kontenerowi oraz jego aplikacjom określonych funkcji jądra i swojego własnego izolowanego systemu plików. Kontener dla działającej wewnątrz niego aplikacji stanowi unikalne wystąpienie systemu operacyjnego. Dla zawartej aplikacji nie są widoczne znajdujące się poza jej kontenerem procesy ani inne zasoby.

W kontenerze platformy Docker jest używanych znacznie mniej zasobów niż w przypadku maszyny wirtualnej. Kontenery platformy Docker używają izolacji aplikacji i modelu wykonywania, który nie udostępnia jądra hosta platformy Docker. Kontener zajmuje znacznie mniejszą przestrzeń dyskową, ponieważ nie zawiera całego systemu operacyjnego. Czas uruchamiania jest znacznie krótszy, a ilość wymaganego miejsca na dysku znacznie mniejsza niż w przypadku maszyny wirtualnej.
Kontenery systemu Windows mają te same zalety co kontenery systemu Linux, ale dotyczą aplikacji działających w systemie Windows. Kontenery systemu Windows obsługują format obrazu i interfejs API platformy Docker, ale mogą być także zarządzane przy użyciu programu PowerShell. W przypadku kontenerów systemu Windows dostępne są dwa środowiska uruchomieniowe — kontenery systemu Windows Server i kontenery funkcji Hyper-V. Kontenery funkcji Hyper-V zapewniają dodatkową warstwę izolacji dzięki hostowaniu każdego kontenera na wysoce zoptymalizowanej maszynie wirtualnej. Aby dowiedzieć się więcej o kontenerach systemu Windows, zobacz [About Windows Containers](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview) (Informacje o kontenerach systemu Windows). Aby rozpocząć pracę z kontenerami systemu Windows na platformie Azure, dowiedz się, w jaki sposób [wdrażać klaster usługi Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).

## <a name="what-are-containers-good-for"></a>Dla jakich zastosowań dobre są kontenery?

Kontenery mogą poprawić:

* szybkość, z jaką kod aplikacji może zostać zaprojektowany i powszechnie udostępniony;
* szybkość i pewność, z jaką aplikacja może zostać przetestowana;
* szybkość i pewność, z jaką aplikacja może zostać wdrożona.

Kontenery działają na hoście kontenera, czyli w ramach systemu operacyjnego, co na platformie Azure oznacza maszynę wirtualną Azure Virtual Machines. Nawet jeśli od razu spodobała Ci się koncepcja kontenerów, nadal konieczna jest hostująca je infrastruktura maszyny wirtualnej — w tym przypadku korzystne jest to, że dla kontenerów nie ma znaczenia, na jakiej maszynie wirtualnej działają (mimo że na przykład istotne jest, czy przez kontener wymagane jest środowisko Linux czy Windows).


## <a name="what-are-containers-good-for"></a>Dla jakich zastosowań dobre są kontenery?
Jest bardzo wiele zastosowań kontenerów, ale tak jak w przypadku usług [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) i usługi [Azure Service Fabric](../articles/service-fabric/service-fabric-overview.md), ich najlepszym zastosowaniem jest tworzenie jednousługowych zorientowanych na mikrousługi aplikacji rozproszonych, których projekt składa się z wielu małych i konfigurowalnych części, a nie z większych i mocno powiązanych składników.

Jest to szczególnie zalecane w środowiskach chmur publicznych, takich jak platforma Azure, w których maszyny wirtualne są wynajmowane i uruchamiane zgodnie z potrzebami. Nie tylko pozwala to uzyskać izolację i dostęp do narzędzi służących do szybkiego wdrażania i aranżowania, ale także pozwala podejmować bardziej efektywne decyzje dotyczące infrastruktury aplikacji.

Na przykład użytkownik może posiadać aktualnie wdrożenie składające się z 9 dużych maszyn wirtualnych platformy Azure na potrzeby aplikacji rozproszonej o wysokiej dostępności. Jeśli składniki tej aplikacji mogą zostać wdrożone w kontenerach, możliwe będzie użycie tylko 4 maszyn wirtualnych i wdrożenie składników aplikacji w ramach 20 kontenerów w celu zapewnienia nadmiarowości oraz równoważenia obciążenia.

Jest to oczywiście tylko przykład, ale jeśli możliwe jest wykonanie tego w przypadku scenariusza użytkownika, to pozwoli to dostosować się do okresów zwiększonego użycia dzięki większej liczbie kontenerów, a nie większej liczbie maszyn wirtualnych platformy Azure, oraz umożliwi bardziej wydajne niż wcześniej użycie pozostałego całkowitego obciążenia procesora CPU.

Ponadto istnieje wiele scenariuszy, w przypadku których nie można skorzystać z podejścia wykorzystującego mikrousługi — w takiej sytuacji to użytkownik będzie najlepiej wiedział, kiedy mikrousługi i kontenery będą mu potrzebne.

### <a name="container-benefits-for-developers"></a>Korzyści dla deweloperów wynikające z korzystania z kontenerów
Ogólnie łatwo zauważyć, że technologia kontenerów jest krokiem do przodu, ale istnieją również bardziej konkretne korzyści. Spójrzmy na przykład dotyczący kontenerów platformy Docker. W tym temacie nie zostaną teraz przedstawione szczegółowe informacje dotyczące platformy Docker (w tym celu przeczytaj sekcję [Co to jest platforma Docker?](https://www.docker.com/whatisdocker/) lub zapoznaj się z informacjami w serwisie [wikipedia](http://wikipedia.org/wiki/Docker_%28software%29)), ale platforma Docker i jej ekosystem oferuje olbrzymie korzyści zarówno deweloperom, jak i informatykom.

Deweloperzy szybko docenią zalety korzystania z platformy Docker, ponieważ przede wszystkim ułatwia ona korzystanie z kontenerów systemu Linux i Windows:

* Możliwe jest używanie prostych przyrostowych poleceń służących do tworzenia stałego obrazu, który jest łatwy do wdrożenia; możliwe jest również zautomatyzowanie wdrażania tych obrazów za pomocą pliku dockerfile.
* Możliwe jest łatwe współużytkowanie tych obrazów przy użyciu prostych poleceń w stylu narzędzia [git](https://git-scm.com/) służących do ściągania i wypychania do [publicznych](https://registry.hub.docker.com/) lub [prywatnych rejestrów platformy Docker](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Możliwe jest działanie w kategoriach izolowanych składników aplikacji, a nie komputerów.
* Możliwe jest używanie wielu narzędzi, które współpracują z kontenerami platformy Docker i różnymi obrazami podstawowymi.

### <a name="container-benefits-for-operations-and-it-professionals"></a>Korzyści dla specjalistów ds. operacyjnych i informatyków wynikające z korzystania z kontenerów
Specjaliści ds. operacyjnych i informatycy mogą również czerpać korzyści z korzystania z kombinacji kontenerów i maszyn wirtualnych.

* Zawarte usługi są odizolowane od środowiska wykonywania hosta maszyny wirtualnej.
* Zawarty kod jest identyczny i możliwe jest tego sprawdzenie.
* Zawarte usługi można szybko uruchomić, zatrzymać oraz przenieść między środowiskiem programistycznym, testowym i produkcyjnym.

Wymienione wyżej funkcje — a jest ich więcej — są bardzo cenione w środowiskach biznesowych, w których celem profesjonalnych organizacji informatycznych jest dopasowywanie zasobów, w tym czystej mocy obliczeniowej, do zadań, aby nie tylko utrzymać się na rynku, ale także zwiększyć zadowolenie klientów i zasięg swojej działalności. Małe firmy, niezależni dostawcy oprogramowania i startupy mają dokładnie te same wymagania, ale mogą je formułować inaczej.

## <a name="what-are-virtual-machines-good-for"></a>Dla jakich zastosowań dobre są maszyny wirtualne?
Maszyny wirtualne zapewniają szkielet przetwarzania danych w chmurze i to nie ulega zmianie. Jeśli uruchomienie maszyn wirtualnych trwa dłużej, a same maszyny wirtualne zajmują większą przestrzeń dyskową i nie są mapowane bezpośrednio do architektury mikrousług, związane są z nimi bardzo istotne korzyści:

1. Domyślnie mają one bardziej niezawodne zabezpieczenia dla komputera hosta.
2. Obsługują wszystkie główne konfiguracje systemów operacyjnych i aplikacji.
3. Mają one stałe ekosystemy narzędzi na potrzeby poleceń i kontroli.
4. Zapewniają środowiska wykonywania dla kontenerów hostów.

Ostatni element jest ważny, ponieważ zawarta aplikacja nadal wymaga określonego systemu operacyjnego i typu procesora CPU w zależności od wywołań wykonywanych przez aplikację. Należy pamiętać, że kontenery są instalowane na maszynach wirtualnych, ponieważ zawierają one aplikacje, które mają zostać wdrożone. Kontenery nie zastępują natomiast maszyn wirtualnych i systemów operacyjnych.

## <a name="high-level-feature-comparison-of-vms-and-containers"></a>Ogólne porównanie funkcji maszyn wirtualnych i kontenerów
W poniższej tabeli w sposób bardzo ogólny opisano rodzaje różnic między funkcjami, które, bez potrzeby wykonywania dodatkowej pracy, istnieją między maszynami wirtualnymi a kontenerami systemu Linux. Należy pamiętać, że niektóre funkcje mogą być mniej lub bardziej pożądane w zależności od potrzeb aplikacji, więc jak w przypadku każdego oprogramowania wykonanie dodatkowej pracy zapewnia zwiększoną obsługę funkcji, szczególnie w zakresie zabezpieczeń.

| Funkcja | Maszyny wirtualne | Kontenery |
|:--- | --- | --- |
| „Domyślna” obsługa zabezpieczeń |w większym stopniu |w trochę mniejszym stopniu |
| Wymagane miejsce na dysku |Cały system operacyjny oraz aplikacje |Tylko zgodnie z wymaganiami aplikacji |
| Czas potrzebny do uruchomienia |Znacznie dłuższy: rozruch systemu operacyjnego oraz ładowanie aplikacji |Znacznie krótszy: tylko uruchamianie aplikacji (jądro jest już uruchomione) |
| Przenośność |Przenośne przy odpowiednim przygotowaniu |Przenośne w formacie obrazu; zwykle mniejsza |
| Automatyzacja obrazu |Różni się w zależności od systemu operacyjnego i aplikacji |[Rejestr platformy Docker](https://registry.hub.docker.com/); inne |

## <a name="creating-and-managing-groups-of-vms-and-containers"></a>Tworzenie grup maszyn wirtualnych oraz kontenerów i zarządzanie nimi
W tym momencie każdy architekt, deweloper lub specjalista ds. operacji IT może pomyśleć: „Mogę zautomatyzować to wszystko; jest to po prostu centrum danych jako usługa”.

Będzie to prawda — jest to możliwe. Istnieje wiele systemów, również tych używanych przez użytkowników, w których można zarządzać grupami maszyn wirtualnych platformy Azure i wstawiać kod niestandardowy za pomocą skryptów, często za pośrednictwem [rozszerzenia niestandardowego skryptu dla systemu Windows](https://msdn.microsoft.com/library/azure/dn781373.aspx) lub [rozszerzenia niestandardowego skryptu dla systemu Linux](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/). Można (i zapewne zostało to już zrobione) zautomatyzować wdrożenia platformy Azure za pomocą skryptów programu PowerShell lub interfejsu wiersza polecenia platformy Azure.

Te możliwości są często następnie migrowane do narzędzi, takich jak [Puppet](https://puppetlabs.com/) i [Chef](https://www.chef.io/) w celu zautomatyzowania tworzenia i konfigurowania maszyn wirtualnych na dużą skalę. Tutaj znajdują się linki dotyczące [używania tych narzędzi na platformie Azure](#tools-for-working-with-containers).

### <a name="azure-resource-group-templates"></a>Szablony grupy zasobów platformy Azure
Niedawno na platformie Azure został wydany interfejs API REST [usługi Azure Resource Management](../articles/resource-manager-deployment-model.md) oraz zaktualizowano program PowerShell i narzędzia 	interfejsu wiersza polecenia platformy Azure. [Szablony usługi Azure Resource Manager](../articles/resource-group-authoring-templates.md) umożliwiają wdrażanie, modyfikowanie lub ponowne wdrażanie całych topologii aplikacji przy użyciu interfejsu API usługi Azure Resource Manager oraz:

* witryny [Azure Portal za pomocą szablonów](https://github.com/Azure/azure-quickstart-templates) (wskazówka: użyj przycisku „Wdróż na platformie Azure”),
* [interfejsu wiersza polecenia platformy Azure](../articles/virtual-machines/linux/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json),
* [modułów programu Azure PowerShell](../articles/virtual-machines/linux/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="deployment-and-management-of-entire-groups-of-azure-vms-and-containers"></a>Wdrażanie całych grup maszyn wirtualnych platformy Azure i kontenerów oraz zarządzanie nimi
Istnieje kilka popularnych systemów, w ramach których można wdrożyć całe grupy maszyn wirtualnych i zainstalować platformę Docker (lub inne systemy hosta kontenera systemu Linux) jako grupę możliwą do zautomatyzowania. Bezpośrednie linki znajdują się w poniższej sekcji dotyczącej [kontenerów i narzędzi](#containers-and-vm-technologies). Istnieje kilka systemów, w których można przeprowadzić te operacje w mniejszym lub większym stopniu — na liście nie znajdują się wszystkie z nich. W zależności od potrzeb i posiadanych umiejętności mogą być one przydatne.

Na platformie Docker dostępny jest zestaw narzędzi do tworzenia maszyn wirtualnych ([docker-machine](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) oraz narzędzie do zarządzania klastrem kontenera platformy Docker w celu równoważenia obciążenia ([swarm](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Ponadto [rozszerzenie maszyny wirtualnej Azure Docker](https://github.com/Azure/azure-docker-extension/blob/master/README.md) domyślnie obsługuje polecenie [`docker-compose`](https://docs.docker.com/compose/), za pomocą którego można wdrożyć skonfigurowane kontenery aplikacji w ramach wielu kontenerów.

Ponadto można wypróbować [system Data Center Operating System (DCOS) firmy Mesosphere](http://docs.mesosphere.com/install/azurecluster/). System DCOS jest oparty na „rozproszonym jądrze systemu” [mesos](http://mesos.apache.org/) typu „open source”, które umożliwia traktowanie centrum danych jako adresowalnej usługi. System DCOS zawiera wbudowane pakiety dla kilku ważnych systemów, takie jak [Spark](http://spark.apache.org/) i [Kafka](http://kafka.apache.org/) (oraz inne), a także wbudowane usługi, takie jak [Marathon](https://mesosphere.github.io/marathon/) (system kontroli kontenera) i [Chronos](https://mesos.github.io/chronos/) (rozproszony program planujący). Rozwiązanie Mesos zostało utworzone między innymi na podstawie doświadczeń firm z branży sieci Web odpowiedzialnych za serwisy Twitter i AirBnb. Można również użyć narzędzia **swarm** jako aparatu aranżacji.

Oprócz tego [kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/) to system typu „open source” służący do zarządzania maszynami wirtualnymi i kontenerami utworzony na podstawie doświadczeń firmy Google. Możliwe jest nawet użycie systemu [kubernetes wraz z platformą Weave w celu zapewnienia obsługi sieci](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave).

[Deis](http://deis.io/overview/) to rozwiązanie „platforma jako usługa” (PaaS) typu „open source”, które ułatwia wdrażanie aplikacji na własnych serwerach i zarządzanie nimi. Rozwiązanie Deis działa w oparciu o platformę Docker i system CoreOS w celu zapewnienia lekkiego rozwiązania PaaS z przepływem pracy wzorowanym na platformie Heroku. Możesz łatwo [utworzyć 3-węzłową grupę maszyn wirtualnych platformy Azure i zainstalować rozwiązanie Deis](../articles/virtual-machines/linux/deis-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) na platformie Azure, a następnie [zainstalować aplikację Hello World Go](../articles/virtual-machines/linux/deis-cluster.md#deploy-and-scale-a-hello-world-application).

W systemie [CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html), czyli dystrybucji systemu Linux ze zoptymalizowaną zajmowaną przestrzenią dyskową, obsługą platformy Docker i własnym systemem kontenerów o nazwie [rkt](https://github.com/coreos/rkt), również istnieje narzędzie do zarządzania grupami kontenerów o nazwie [fleet](https://coreos.com/using-coreos/clustering/).

Ubuntu, inna bardzo popularna dystrybucja systemu Linux, również obsługuje platformę Docker, ale także obsługuje [klastry systemu Linux (typu LXC)](https://help.ubuntu.com/lts/serverguide/lxc.html).

## <a name="tools-for-working-with-azure-vms-and-containers"></a>Narzędzia do pracy z maszynami wirtualnymi platformy Azure i kontenerami
Podczas pracy z kontenerami i maszynami wirtualnymi platformy Azure używane są narzędzia. Ta sekcja zawiera listę tylko niektórych najbardziej przydatnych lub istotnych pojęć i narzędzi związanych z kontenerami i grupami oraz większych narzędzi do konfiguracji i aranżacji używanych wraz z nimi.

> [!NOTE]
> Ten obszar ulega bardzo szybkim zmianom i, mimo że dokładamy wszelkich starań, aby te linki były aktualne, może okazać się to niemożliwe do wykonania. Wyszukaj interesujące Cię tematy, aby być na bieżąco.
>
>

### <a name="containers-and-vm-technologies"></a>Technologie dotyczące kontenerów i maszyn wirtualnych
Niektóre technologie dotyczące kontenerów systemu Linux:

* [Docker](https://www.docker.com)
* [LXC](https://linuxcontainers.org/)
* [CoreOS i rkt](https://github.com/coreos/rkt)
* [Projekt Open Container](http://opencontainers.org/)
* [RancherOS](http://rancher.com/rancher-os/)

Linki dotyczące kontenerów systemu Windows:

* [Kontenery systemu Windows](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)

Linki dotyczące platformy Docker programu Visual Studio:

* [Visual Studio 2015 RC Tools for Docker — wersja zapoznawcza](https://visualstudiogallery.msdn.microsoft.com/6f638067-027d-4817-bcc7-aa94163338f0)

Narzędzia platformy Docker:

* [Demon platformy Docker](https://docs.docker.com/installation/#installation)
* Klienci platformy Docker
  * [Klient platformy Docker systemu Windows w ramach narzędzia Chocolatey](https://chocolatey.org/packages/docker)
  * [Instrukcje dotyczące instalacji platformy Docker](https://docs.docker.com/installation/#installation)

Platforma Docker na platformie Microsoft Azure:

* [Rozszerzenie maszyny wirtualnej platformy Docker dla systemu Linux na platformie Azure](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Podręcznik użytkownika rozszerzenia maszyny wirtualnej platformy Docker platformy Azure](https://github.com/Azure/azure-docker-extension/blob/master/README.md)
* [Korzystanie z rozszerzenia maszyny wirtualnej platformy Docker z poziomu interfejsu wiersza polecenia platformy Azure (Azure CLI)](../articles/virtual-machines/linux/classic/cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Korzystanie z rozszerzenia maszyny wirtualnej rozwiązania Docker z poziomu witryny Azure Portal](../articles/virtual-machines/linux/classic/portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Jak używać maszyny Docker na platformie Azure](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Jak używać platformy Docker wraz z narzędziem swarm na platformie Azure](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Wprowadzenie do platformy Docker i narzędzia Compose na platformie Azure](../articles/virtual-machines/linux/docker-compose-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Używanie szablonu grupy zasobów platformy Azure w celu szybkiego utworzenia hosta platformy Docker na platformie Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)
* [Wbudowana obsługa polecenia `compose`](https://github.com/Azure/azure-docker-extension#11-public-configuration-keys) dla zawartych aplikacji
* [Implementowanie prywatnego rejestru platformy Docker na platformie Azure](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Dystrybucje systemu Linux i przykłady związane z platformą Azure:

* [CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html)

Konfiguracja, zarządzanie klastrami i aranżacja kontenerów:

* [Narzędzie Fleet w systemie CoreOS](https://coreos.com/using-coreos/clustering/)
* Deis

  * [Tworzenie 3-węzłowej grupy maszyn wirtualnych platformy Azure, instalowanie rozwiązania Deis i uruchamianie aplikacji Hello World Go](../articles/virtual-machines/linux/deis-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Kubernetes

  * [Pełny przewodnik po zautomatyzowanym wdrażaniu klastra Kubernetes za pomocą systemu CoreOS i platformy Weave](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
  * [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Mesos](http://mesos.apache.org/)

  * [System Data Center Operating System (DCOS) firmy Mesosphere](http://beta-docs.mesosphere.com/install/azurecluster/)
* [Jenkins](https://jenkins-ci.org/) i [Hudson](http://hudson-ci.org/)

  * [Blog: Wtyczka podrzędna rozwiązania Jenkins dla platformy Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
  * [Repozytorium GitHub: Wtyczka magazynu rozwiązania Jenkins dla platformy Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)
  * [Inna firma: Wtyczka podrzędna rozwiązania Hudson dla platformy Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
  * [Inna firma: Wtyczka magazynu rozwiązania Hudson dla platformy Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Azure Automation](https://azure.microsoft.com/services/automation/)

  * [Film wideo: Używanie usługi Azure Automation z maszynami wirtualnymi systemu Linux](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* Rozszerzenie DSC programu PowerShell dla systemu Linux

  * [Blog: Jak utworzyć rozszerzenie DSC programu PowerShell dla systemu Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
  * [GitHub: Rozszerzenie DSC klienta platformy Docker](https://github.com/anweiss/DockerClientDSC)

## <a name="next-steps"></a>Następne kroki
Zapoznaj się z platformą [Docker](https://www.docker.com) i [kontenerami systemu Windows](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

<!--Anchors-->
[microservices]: http://martinfowler.com/articles/microservices.html
[microservice]: http://martinfowler.com/articles/microservices.html
<!--Image references-->
