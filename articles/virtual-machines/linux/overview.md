---
title: "Omówienie maszyn wirtualnych systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Opisuje usługi rozwiązań usługi obliczenia Azure, magazynu i sieci maszyn wirtualnych systemu Linux."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: rickstercdn
manager: timlt
editor: 
ms.assetid: 7965a80f-ea24-4cc2-bc43-60b574101902
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: overview
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: ae4b8f423489bf417f1086368db9b1043cd7f396
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="azure-and-linux"></a>Azure i Linux
Microsoft Azure to wciąż rozrastający się zbiór zintegrowanych publicznego usług bazy danych analizy, maszyny wirtualne, mobilnych, sieci, magazynu, w tym chmury i sieci web&mdash;nadaje się doskonale dla hostingu rozwiązań.  Microsoft Azure to skalowalna platforma obliczeniowa, która pozwala na płacenie tylko za używane usługi, z których można korzystać w dowolnej chwili — bez konieczności inwestowania w sprzęt lokalny.  Platforma Azure jest gotowa wraz z Tobą do skalowania rozwiązań tak, aby sprostać wymaganiom klientów.

Jeśli znasz różnych funkcji w portalu Amazon usług AWS, można sprawdzić Azure vs usług AWS [dokumentu mapowania definicji](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

## <a name="regions"></a>Regiony
Microsoft Azure zasoby są rozpowszechniane w różnych regionach geograficznych na całym świecie.  "region" reprezentuje wielu centrów danych w jednym obszarze geograficznym.  Mamy 34 regionów ogólnie dostępna na świecie z dodatkowe regiony 4 anonsowania. Ponieważ możemy w dalszym ciągu rozwiń naszych globalnych pokrycia - możemy Obsługa zaktualizowaną listę istniejących i nowo ogłoszenia regionów.

* [Regiony platformy Azure](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Dostępność
Firma Microsoft opublikowała branży wiodące pojedynczego wystąpienia maszyny wirtualnej z umową dotyczącą poziomu usług z wartością 99,9% pod warunkiem, że wdrożenie maszyny Wirtualnej z magazyn w warstwie premium dla wszystkich dysków.  W kolejności dla danego wdrożenia zakwalifikować się do naszej standardowe 99,95% maszyn wirtualnych Umowa dotycząca poziomu usług nadal należy wdrożyć co najmniej dwie maszyny wirtualne z systemami obciążenie wewnątrz zestawu dostępności. Zapewni to maszyny wirtualne są rozproszone na wielu domen błędów w naszych centrach danych, jak również wdrożyć na hostach z systemem windows konserwacji. Pełna treść [umowy SLA dotyczącej usługi Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wyjaśnia w całości kwestię gwarantowanej dostępności platformy Azure.

## <a name="managed-disks"></a>Managed Disks

Zarządzane dyski dojścia do usługi Azure Storage o tworzeniu konta i zarządzania w tle dla Ciebie i zapewnia, że nie trzeba martwić limity skalowalności konta magazynu. Po prostu określić rozmiar dysku i warstwę wydajności (standardowy lub Premium) i Azure tworzy i zarządza dysku. O używanym magazynie nie trzeba myśleć nawet przy dodawaniu dysków albo skalowaniu maszyny wirtualnej w górę i w dół. Jeśli tworzysz nowe maszyny wirtualne, [Użyj 2.0 interfejsu wiersza polecenia Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lub w portalu Azure do tworzenia maszyn wirtualnych z zarządzanych systemu operacyjnego i dysków z danymi. Jeśli masz maszyny wirtualne z dyskami niezarządzane, możesz [przekonwertować maszyny wirtualne do wykonania kopii z dyskami zarządzane](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Można też zarządzać własnymi obrazami niestandardowymi na jednym koncie magazynu na region platformy Azure i używać ich do tworzenia setek maszyn wirtualnych w tej samej subskrypcji. Aby uzyskać więcej informacji na temat dysków zarządzanych, zobacz [Omówienie usługi Managed Disks](../windows/managed-disks-overview.md).

## <a name="azure-virtual-machines--instances"></a>Maszyny wirtualne platformy Azure i wystąpień
Microsoft Azure obsługuje uruchamianie wielu popularnych dystrybucje systemu Linux podane i aktualizowany przez liczbę partnerów.  Znajdziesz dystrybucje, takich jak Red Hat Enterprise, CentOS, SUSE Linux Enterprise, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD i więcej w portalu Azure Marketplace. Aktywnie pracujemy z różnych społeczności Linux można dodać więcej odmian do [zatwierdzone Dystrybucjach systemu Linux na platformie Azure](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) listy.

Jeśli Twoje preferowanych distro Linux wyboru nie jest aktualnie w galerii, "przełączeniem własne Linux" maszyny Wirtualnej przez [tworzenie i przekazywanie wirtualnego dysku twardego systemu Linux na platformie Azure](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Maszyny wirtualne platformy Azure umożliwiają wdrażanie szeroką gamę rozwiązań opartych na przetwarzaniu w sposób elastyczne. Możesz wdrożyć niemal dowolnym obciążeniu i dowolnego języka na niemal każde system operacyjny: Windows, Linux, lub niestandardowy utworzony z jednego z naszych coraz dłuższej listy partnerów. Nadal nie widać, czego szukasz?  Nie martw się — można również przełączyć własnych obrazów z lokalnymi.

## <a name="vm-sizes"></a>Rozmiary maszyn wirtualnych
Podczas wdrażania maszyny Wirtualnej na platformie Azure mają być wybierz rozmiar maszyny Wirtualnej w ramach jednej z naszych serii rozmiarów, która jest odpowiednia do obciążenia. Rozmiar wpływa także przetwarzania zdolności zasilania, pamięci oraz magazyn maszyny wirtualnej. Są rozliczane, w oparciu o czas maszyna wirtualna jest uruchomiona i korzystanie z jej przydzielone zasoby. Pełna lista [rozmiary maszyn wirtualnych](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Poniżej przedstawiono podstawowe wskazówki dotyczące wybierania rozmiar maszyny Wirtualnej z jednego z naszych serii (A, D, DS, G i GS).
* A-series maszyny wirtualne są nasze wartość cenach klasy podstawowej maszyn wirtualnych dla lekkich obciążeń i scenariusze tworzenia/testowania. Są powszechnie dostępne we wszystkich regionach i mogą połączyć i używać wszystkie standardowe zasoby dostępne dla maszyn wirtualnych.
* A-series (A8 - A11) są specjalne obliczeń znacznym konfiguracje odpowiedni w przypadku wysokiej wydajności obliczeniowej klastra.
* Maszyny wirtualne serii D są zaprojektowane do uruchamiania aplikacji wymagających większej mocy obliczeniowej i wydajności dysków tymczasowych. Maszyny wirtualne serii D zapewniają szybsze procesory, większą ilość pamięci na rdzeń i dyski półprzewodnikowe (SSD) dla dysków tymczasowych.
* Dv2 serii jest najnowsza wersja naszych D-series, funkcje większe możliwości procesora CPU. Procesor CPU serii Dv2 jest o około 35% szybszy niż procesor CPU serii D. Jest on oparty na najnowszej generacji 2.4 v3® GHz Intel Xeon E5-2673 procesora (Haskell) i 2.0 technologii zwiększanie wyniku Turbo firmy Intel, można przejść do 3,2 GHz. Konfiguracje pamięci i dysków serii Dv2 są takie same jak w przypadku serii D.
* Maszyny wirtualne z serii G oferują największą ilość pamięci i są uruchamiane na hostach z procesorami z rodziny Intel Xeon E5 V3.

Uwaga: Seria DS i GS-series maszyny wirtualne mają dostęp do magazyn w warstwie Premium — nasze SSD kopii wysokiej wydajności i małych opóźnieniach magazynu dla intensywnych obciążeń we/wy. Usługa Premium Storage jest dostępna w określonych regionach. Aby uzyskać szczegółowe informacje, zobacz:

* [Magazyn w warstwie Premium: Magazyn o wysokiej wydajności dla obciążeń maszyny wirtualnej platformy Azure](../windows/premium-storage.md)

## <a name="automation"></a>Automatyzacja
Uzyskanie prawidłowego kultury DevOps, wszystkie infrastruktury musi być kodem.  Gdy wszystkie infrastruktury znajduje się w kodzie, które mogą być łatwo ponownie (Phoenix serwerów).  Azure współpracuje z głównych automatyzacji, narzędzi, takich jak Ansible, Chef, SaltStack i Puppet.  Platforma Azure ma własną narzędziami automatyzacji:

* [Szablony usługi Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure rozszerzenia VMAccess.](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure wprowadza obsługę [init chmury](http://cloud-init.io/) przez większość Dystrybucjach systemu Linux, który go obsługuje.  Obecnie maszyn wirtualnych systemu Ubuntu na Canonical zostały wdrożone za pomocą chmury inicjowaniem domyślnie włączone.  Czerwony Kapelusze RHEL, CentOS i Fedora obsługi chmury init, jednak obrazy platformy Azure obsługiwanego przez RedHat nie mają zainstalowany inicjowania chmury.  Aby używać init chmury rodzina RedHat systemu operacyjnego, należy utworzyć niestandardowego obrazu z inicjowaniem chmury zainstalowane.

* [Za pomocą init chmurze na maszynach wirtualnych systemu Linux platformy Azure](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quotas"></a>Przydziały
Każda subskrypcja platformy Azure ma domyślne limity przydziału w miejscu, które może mieć wpływ na wdrożenie dużej liczby maszyn wirtualnych dla projektu. Aktualny limit dla każdej subskrypcji wynosi 20 maszyn wirtualnych na region.  Limity przydziału może zostać wywołane szybko i łatwo zgłaszając bilet pomocy technicznej żąda limit zwiększyć.  Aby uzyskać więcej informacji na temat limitów przydziału:

* [Ograniczenia usługi subskrypcji platformy Azure](../../azure-subscription-service-limits.md)

## <a name="partners"></a>Partnerzy
Firma Microsoft ściśle współpracuje z partnerami w celu zapewnienia dostępnych obrazów są aktualizowane i zoptymalizowany pod kątem obsługi usługi Azure.  Aby uzyskać więcej informacji na temat naszych partnerów Sprawdź stronach witryny marketplace poniżej.

* Linux na platformie Azure - [dystrybucje zatwierdzone](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* SUSE - [witrynę Azure Marketplace — SUSE Linux Enterprise Server](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=%27SUSE%27)
* RedHat - [witrynę Azure Marketplace — RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)
* Canonical - [witrynę Azure Marketplace — LTS Ubuntu Server 16.04](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)
* Debian - [witrynę Azure Marketplace — 8 Debian "Joasia"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)
* FreeBSD - [witrynę Azure Marketplace — FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
* CoreOS - [witrynę Azure Marketplace — CoreOS (stały)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)
* RancherOS - [witrynę Azure Marketplace — RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)
* Bitnami - [Bitnami biblioteki dla platformy Azure](https://azure.bitnami.com/)
* Mesosphere - [witrynę Azure Marketplace — Mesosphere DC/OS na platformie Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)
* Docker - [witrynę Azure Marketplace — usługi kontenera platformy Azure z Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)
* Wpięć - [witrynę Azure Marketplace — CloudBees Wpięć platformy](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)

## <a name="getting-started-with-linux-on-azure"></a>Rozpoczynanie pracy z systemem Linux na platformie Azure
Aby rozpocząć korzystanie z usługi Azure potrzebne jest konto platformy Azure, Azure CLI zainstalowany i parę kluczy publicznych i prywatnych SSH.

### <a name="sign-up-for-an-account"></a>Tworzenie konta
Pierwszym krokiem przy użyciu chmury Azure jest zalogowania się do konta platformy Azure.  Przejdź do [możliwość tworzenia konta Azure](https://azure.microsoft.com/pricing/free-trial/) stronę, aby rozpocząć pracę.

### <a name="install-the-cli"></a>Instalowanie interfejsu wiersza polecenia
Przy użyciu nowego konta platformy Azure możesz rozpocząć pracę od razu przy użyciu portalu Azure, czyli panel administracyjny opartych na sieci web.  Aby zarządzać chmury Azure za pomocą wiersza polecenia, należy zainstalować `azure-cli`.  Zainstaluj [Azure CLI 2.0](/cli/azure/install-azure-cli) na stacji roboczej Mac lub Linux.

### <a name="create-an-ssh-key-pair"></a>Tworzenie pary kluczy SSH
Teraz masz konta platformy Azure, portalu sieci web platformy Azure i wiersza polecenia platformy Azure.  Następnym krokiem jest można utworzyć pary kluczy SSH, który służy do SSH w systemie Linux bez użycia hasła.  [Tworzenie kluczy SSH w systemie Linux i komputerów Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) umożliwienie hasła bez nazwy logowania i większe bezpieczeństwo.

### <a name="create-a-vm-using-the-cli"></a>Tworzenie maszyny wirtualnej przy użyciu interfejsu wiersza polecenia
Tworzenie maszyny Wirtualnej systemu Linux przy użyciu interfejsu wiersza polecenia jest szybkie wdrożenie maszyny Wirtualnej bez opuszczania terminal pracy.  Wszystkie obiekty, które można określić w portalu sieci web jest dostępna za pośrednictwem flagi wiersza polecenia lub przełącznika.  

* [Utwórz Maszynę wirtualną systemu Linux przy użyciu interfejsu wiersza polecenia](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="create-a-vm-in-the-portal"></a>Tworzenie maszyny Wirtualnej w portalu
Tworzenie maszyny Wirtualnej systemu Linux w portalu sieci web platformy Azure to sposób łatwo polecenie, a następnie kliknij za pomocą różnych opcji, aby uzyskać dostęp do wdrożenia.  Zamiast używać flagi wiersza polecenia i przełączniki, będą mogli wyświetlić układ nieuprzywilejowany web różnych opcji i ustawień.  Wszystkie dostępne za pośrednictwem interfejsu wiersza polecenia jest również dostępna w portalu.

* [Utwórz Maszynę wirtualną systemu Linux przy użyciu portalu](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="login-using-ssh-without-a-password"></a>Zaloguj się przy użyciu protokołu SSH bez hasła
Maszyna wirtualna jest teraz uruchomiona na platformie Azure i wszystko jest gotowe do logowania.  Za pomocą hasła do zalogowania się za pośrednictwem SSH jest niebezpieczne i czasochłonna.  Używanie kluczy SSH jest najbezpieczniejsza, a także najszybszym sposobem logowania.  Po utworzeniu można maszyny Wirtualnej systemu Linux za pomocą portalu lub interfejsu wiersza polecenia, dostępne są dwie opcje uwierzytelniania.  Jeśli hasło SSH, Azure umożliwia skonfigurowanie maszyny Wirtualnej, aby umożliwić logowania za pomocą hasła.  Jeśli zostanie wybrany klucz publiczny SSH, Azure umożliwia skonfigurowanie maszyny Wirtualnej, aby zezwolić tylko na nazwy logowania za pomocą kluczy SSH i wyłącza hasło logowania. Aby zabezpieczyć maszyny Wirtualnej systemu Linux, zezwalając tylko logowania do klucza SSH, opcja SSH publicznego klucza podczas tworzenia maszyny Wirtualnej w portalu lub interfejsu wiersza polecenia.

## <a name="related-azure-components"></a>Powiązane składniki platformy Azure
## <a name="storage"></a>Magazyn
* [Wprowadzenie do usługi Microsoft Azure Storage](../../storage/common/storage-introduction.md)
* [Dodaj dysk do maszyny Wirtualnej systemu Linux przy użyciu wiersza polecenia platformy azure](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Jak można dołączyć dysku danych do maszyny Wirtualnej systemu Linux, w portalu Azure](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Sieć
* [Omówienie sieci wirtualnej](../../virtual-network/virtual-networks-overview.md)
* [Adresy IP na platformie Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Otwieranie portów dla maszyny Wirtualnej systemu Linux na platformie Azure](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Utwórz w pełni kwalifikowaną nazwę domeny w portalu Azure](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="containers"></a>Kontenery
* [Maszyny wirtualne i kontenerów na platformie Azure](containers.md)
* [Wprowadzenie usługi kontenera platformy Azure](../../container-service/container-service-intro.md)
* [Wdrażanie klastra usługi Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md)

## <a name="next-steps"></a>Następne kroki
Masz teraz Omówienie systemu Linux na platformie Azure.  Następnym krokiem jest zajrzyj dostęp i Utwórz kilka maszyn wirtualnych!

* [Eksploruj nasze coraz dłuższej listy przykładowe skrypty do wykonywania typowych zadań za pomocą AzureCLI](cli-samples.md)
