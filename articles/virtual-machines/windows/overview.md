---
title: "Omówienie maszyn wirtualnych z systemem Windows | Microsoft Docs"
description: "Dowiedz się więcej na temat tworzenia maszyn wirtualnych z systemem Windows na platformie Azure oraz zarządzania nimi."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: fbae9c8e-2341-4ed0-bb20-fd4debb2f9ca
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/01/2017
ms.author: davidmu
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 7ddd9244558479f1fc77c0a9f3d02d0d3b95ca9f
ms.contentlocale: pl-pl
ms.lasthandoff: 05/31/2017

---
<a id="overview-of-windows-virtual-machines-in-azure" class="xliff"></a>

# Omówienie maszyn wirtualnych z systemem Windows na platformie Azure
Usługa Azure Virtual Machines (VM) to jeden z wielu [skalowalnych zasobów obliczeniowych dostępnych na żądanie](../../app-service-web/choose-web-site-cloud-service-vm.md), które są oferowane na platformie Azure. W większości przypadków maszynę wirtualną należy wybrać wtedy, gdy potrzebna jest większa kontrola nad środowiskiem obliczeniowym niż ta, jaką oferują inne opcje. Ten artykuł zawiera informacje o kwestiach, jakie należy rozważyć przed przystąpieniem do tworzenia maszyny wirtualnej. Opisano w nim także tworzenie maszyny wirtualnej i zarządzanie nią.

Maszyna wirtualna Azure umożliwia swobodne korzystanie z wirtualizacji bez konieczności kupowania i utrzymywania fizycznego sprzętu potrzebnego do jej działania. Nadal niezbędna jest jednak konserwacja maszyny wirtualnej — konfigurowanie i instalowanie jej oprogramowania, a także instalowanie poprawek tego oprogramowania.

Z maszyn wirtualnych na platformie Azure można korzystać na różne sposoby. Przykłady to:

* **Tworzenie i testowanie oprogramowania** — maszyny wirtualne umożliwiają szybkie i łatwe utworzenie komputera o określonej konfiguracji wymaganej do tworzenia kodu oraz testowania aplikacji.
* **Aplikacje w chmurze** — jako że zapotrzebowanie na aplikację może zmieniać się w czasie, uzasadnione ekonomicznie może okazać się uruchomienie jej na maszynie wirtualnej na platformie Azure. Jeśli zajdzie potrzeba użycia dodatkowych maszyn wirtualnych, wystarczy je opłacić. Gdy dodatkowe maszyny staną się zbędne, można je wyłączyć.
* **Rozszerzone centrum danych** — maszyny wirtualne w sieci wirtualnej na platformie Azure można zostać z łatwością połączyć z siecią organizacji.

Liczbę maszyn wirtualnych używanych przez aplikację można dowolnie i bez ograniczeń zwiększać odpowiednio do potrzeb.

<a id="what-do-i-need-to-think-about-before-creating-a-vm" class="xliff"></a>

## Co należy wziąć pod uwagę przed utworzeniem maszyny wirtualnej?
Podczas tworzenia infrastruktury aplikacji na platformie Azure należy zawsze wziąć pod uwagę wiele różnych [zagadnień projektowych](infrastructure-virtual-machine-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Kwestie dotyczące maszyn wirtualnych, które należy rozważyć, to między innymi:

* nazwy zasobów aplikacji;
* lokalizacja, w której są przechowywane zasoby;
* rozmiar maszyny wirtualnej;
* maksymalna liczba maszyn wirtualnych, które można utworzyć;
* system operacyjny uruchomiony na maszynie wirtualnej;
* konfiguracja maszyny wirtualnej po jej uruchomieniu;
* powiązane zasoby niezbędne do działania maszyny wirtualnej.

<a id="naming" class="xliff"></a>

### Nazewnictwo
Maszyna wirtualna ma przypisaną do niej [nazwę](infrastructure-naming-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), a także nazwę komputera skonfigurowaną jako element systemu operacyjnego. Nazwa maszyny wirtualnej może zawierać maksymalnie 15 znaków.

Jeśli dysk systemu operacyjnego zostanie utworzony na platformie Azure, nazwa komputera jest taka sama jak nazwa maszyny wirtualnej. Nazwy mogą się różnić w przypadku [przesłania i użycia własnego obrazu](upload-generalized-managed.md), który zawiera skonfigurowany wcześniej system operacyjny, oraz wykorzystania go do utworzenia maszyny wirtualnej. Jeśli zostanie przesłany własny plik obrazu, zalecamy ustawienie takiej samej nazwy dla komputera w systemie operacyjnym i dla maszyny wirtualnej.

<a id="locations" class="xliff"></a>

### Lokalizacje
Wszystkie zasoby tworzone na platformie Azure są dystrybuowane w obrębie wielu [regionów geograficznych](https://azure.microsoft.com/regions/) na całym świecie. Zazwyczaj podczas tworzenia maszyny wirtualnej region określa się mianem **lokalizacji**. W przypadku maszyny wirtualnej lokalizacja określa miejsce, w którym są przechowywane wirtualne dyski twarde.

W poniższej tabeli przedstawiono wybrane metody uzyskania dostępu do listy dostępnych lokalizacji.

| Metoda | Opis |
| --- | --- |
| Azure Portal |Wybór lokalizacji z listy podczas tworzenia maszyny wirtualnej. |
| Azure PowerShell |Użycie polecenia [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation). |
| Interfejs API REST |Użycie operacji [wyświetlania listy lokalizacji](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_ListLocations). |

<a id="vm-size" class="xliff"></a>

### Rozmiar maszyny wirtualnej
[Rozmiar](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) maszyny wirtualnej zależy od obciążenia, które ma zostać uruchomione. Wybrany rozmiar ma więc wpływ na takie czynniki jak moc procesora, pamięć i przestrzeń dyskowa. W ramach platformy Azure dostępna jest szeroka gama rozmiarów umożliwiających wykorzystanie produktu do wielu różnych zastosowań.

Na platformie Azure obowiązuje [cena za godzinę](https://azure.microsoft.com/pricing/details/virtual-machines/windows/), która jest określana na podstawie rozmiaru maszyny wirtualnej i jej systemu operacyjnego. W przypadku rozpoczętych godzin opłaty są pobierane tylko za faktycznie wykorzystane minuty. Magazyn jest wyceniany oddzielnie; związane z nim opłaty są także pobierane osobno.

<a id="vm-limits" class="xliff"></a>

### Limity maszyn wirtualnych
Subskrypcje mają domyślne [limity przydziałów](../../azure-subscription-service-limits.md), które mogą mieć wpływ na wdrożenie wielu maszyn wirtualnych w projekcie. Aktualny limit dla każdej subskrypcji wynosi 20 maszyn wirtualnych na region. Limity można zwiększyć, wypełniając bilet pomocy technicznej.

<a id="operating-system-disks-and-images" class="xliff"></a>

### Dyski i obrazy z systemem operacyjnym
System operacyjny i dane maszyny wirtualnej są przechowywane na [wirtualnym dysku twardym (VHD)](../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Wirtualne dyski twarde są również używane do obsługi obrazów, spośród których można wybierać, chcąc zainstalować system operacyjny. 

Na platformie Azure jest dostępnych wiele [obrazów z portalu Marketplace](https://azure.microsoft.com/marketplace/virtual-machines/), których można używać z różnymi wersjami i typami systemów operacyjnych Windows Server. Obrazy z Marketplace są oznaczone nazwą wydawcy i oferty, jednostką SKU i wersją (zwykle najnowszą). 

W poniższej tabeli pokazano, jak można znaleźć informacje o obrazie.

| Metoda | Opis |
| --- | --- |
| Azure Portal |Wartości są podawane automatycznie po wybraniu obrazu, który ma zostać użyty. |
| Azure PowerShell |[Get-AzureRMVMImagePublisher](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimagepublisher) -Location "lokalizacja"<BR>[Get-AzureRMVMImageOffer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimageoffer) -Location "lokalizacja" -Publisher "nazwa_wydawcy"<BR>[Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) -Location "lokalizacja" -Publisher "nazwa_wydawcy" -Offer "nazwa_oferty" |
| Interfejsy API REST |[Wyświetl listę wydawców obrazów](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers)<BR>[Wyświetl listę ofert obrazów](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers)<BR>[Wyświetl listę jednostek SKU obrazów](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus) |

Istnieje możliwość [przesłania i użycia własnego obrazu](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account) — nazwa wydawcy, oferta i jednostka SKU nie są wtedy używane.

<a id="extensions" class="xliff"></a>

### Rozszerzenia
[Rozszerzenia](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) maszyn wirtualnych udostępniają dodatkowe funkcje, z których można korzystać po wdrożeniu i do automatyzacji zadań.

Typowe zadania można realizować przy użyciu różnych rozszerzeń:

* **Uruchamianie skryptów niestandardowych** — [rozszerzenie niestandardowego skryptu](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ułatwia konfigurowanie obciążeń na maszynie wirtualnej poprzez uruchomienie skryptu po aprowizacji maszyny wirtualnej.
* **Wdrażanie konfiguracji i zarządzanie nimi** — [rozszerzenie Konfiguracja żądanego stanu (Desired State Configuration, DSC) programu PowerShell](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ułatwia skonfigurowanie żądanego stanu na maszynie wirtualnej w celu zarządzania konfiguracjami i środowiskami.
* **Zbieranie danych diagnostycznych** — [rozszerzenie Diagnostyka Azure](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ułatwia skonfigurowanie maszyny wirtualnej w celu zbierania danych diagnostycznych używanych do monitorowania kondycji aplikacji.

<a id="related-resources" class="xliff"></a>

### Powiązane zasoby
Zasoby wymienione w tej tabeli są używane przez maszynę wirtualną i muszą istnieć lub zostać utworzone podczas jej tworzenia.

| Zasób | Wymagany | Opis |
| --- | --- | --- |
| [Grupa zasobów](../../azure-resource-manager/resource-group-overview.md) |Tak |Maszyna wirtualna musi być zawarta w grupie zasobów. |
| [Konto magazynu](../../storage/storage-create-storage-account.md) |Tak |Maszyna wirtualna wymaga konta magazynu do przechowywania wirtualnych dysków twardych. |
| [Sieć wirtualna](../../virtual-network/virtual-networks-overview.md) |Tak |Maszyna wirtualna musi należeć do sieci wirtualnej. |
| [Publiczny adres IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) |Nie |Maszyna wirtualna może mieć przypisany publiczny adres IP umożliwiający uzyskiwanie do niej dostępu zdalnego. |
| [Interfejs sieciowy](../../virtual-network/virtual-network-network-interface.md) |Tak |Maszyna wirtualna wymaga interfejsu sieciowego do komunikacji w sieci. |
| [Dyski danych](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |Nie |Maszyna wirtualna może zawierać dyski danych zwiększające jej pojemność. |

<a id="how-do-i-create-my-first-vm" class="xliff"></a>

## Jak utworzyć maszynę wirtualną?
Można skorzystać z jednej z kilku opcji utworzenia maszyny wirtualnej. Wybór zależy od używanego środowiska. 

Ta tabela zawiera informacje ułatwiające utworzenie maszyny wirtualnej.

| Metoda | Artykuł |
| --- | --- |
| Azure Portal |[Tworzenie maszyny wirtualnej z systemem Windows przy użyciu portalu](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Szablony |[Tworzenie maszyny wirtualnej z systemem Windows przy użyciu szablonu usługi Resource Manager](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Azure PowerShell |[Tworzenie maszyny wirtualnej z systemem Windows przy użyciu programu PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Zestawy SDK klienta |[Wdrażanie zasobów Azure przy użyciu języka C#](csharp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Interfejsy API REST |[Tworzenie lub aktualizowanie maszyny wirtualnej](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-create-or-update) |

Nikt tego nie lubi, ale czasem coś może pójść nie tak. Warto zapoznać się wtedy z artykułem [Troubleshoot Resource Manager deployment issues with creating a Windows virtual machine in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Rozwiązywanie problemów z wdrażaniem programu Resource Manager dotyczących tworzenia maszyny wirtualnej z systemem Windows na platformie Azure).

<a id="how-do-i-manage-the-vm-that-i-created" class="xliff"></a>

## Jak zarządzać maszyną wirtualną?
Maszynami wirtualnymi można zarządzać w portalu w przeglądarce internetowej, a także przy użyciu narzędzi wiersza polecenia z obsługą skryptów oraz bezpośrednio za pomocą interfejsów API. Najczęściej wykonywane zadania administracyjne to np. uzyskiwanie informacji na temat maszyny wirtualnej, logowanie się do niej, zarządzanie dostępnością oraz wykonywanie kopii zapasowych.

<a id="get-information-about-a-vm" class="xliff"></a>

### Uzyskiwanie informacji o maszynie wirtualnej
W tej tabeli opisano, w jaki sposób można uzyskiwać informacje o maszynie wirtualnej.

| Metoda | Opis |
| --- | --- |
| Azure Portal |W menu Centrum kliknij opcję **Maszyny wirtualne**, a następnie wybierz odpowiednią maszynę wirtualną z listy. Z poziomu bloku maszyny wirtualnej można uzyskać dostęp do podstawowych informacji, wartości ustawień i metryk monitorowania. |
| Azure PowerShell |Aby uzyskać informacje na temat używania programu PowerShell zarządzania maszynami wirtualnymi, zobacz [Create and manage Windows VMs with the Azure PowerShell module (Tworzenie maszyn wirtualnych z systemem Windows i zarządzanie nimi za pomocą programu Azure PowerShell)](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Interfejs API REST |Użyj operacji [Get VM information](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-get) (Pobierz informacje o maszynie wirtualnej), aby uzyskać informacje na temat maszyny wirtualnej. |
| Zestawy SDK klienta |Aby dowiedzieć się więcej o zarządzaniu maszynami wirtualnymi za pomocą języka C#, zobacz artykuł [Manage Azure Virtual Machines using Azure Resource Manager and C#](csharp-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Zarządzanie maszynami wirtualnymi na platformie Azure za pomocą usługi Azure Resource Manager i języka C#). |

<a id="log-on-to-the-vm" class="xliff"></a>

### Logowanie się do maszyny wirtualnej
Korzystając z przycisku Połącz w witrynie Azure, można [uruchomić sesję pulpitu zdalnego (RDP)](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Podczas próby użycia połączenia zdalnego mogą czasami występować problemy. W takiej sytuacji zapoznaj się z artykułem [Rozwiązywanie problemów z połączeniami Pulpitu zdalnego z maszynami wirtualnymi systemu Windows na platformie Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<a id="manage-availability" class="xliff"></a>

### Zarządzanie dostępnością
Ważne jest zrozumienie, jak można [zapewnić wysoką dostępność](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) aplikacji. Ta konfiguracja obejmuje utworzenie wielu maszyn wirtualnych w celu zagwarantowania, że przynajmniej jednak z nich na pewno działa.

Aby wdrożenie kwalifikowało się do naszej Umowy dotyczącej poziomu usług (SLA) uwzględniającej dostępność maszyn wirtualnych na poziomie 99,95%, wymagane jest wdrożenie co najmniej dwóch maszyn wirtualnych z uruchomionym obciążeniem w obrębie [zestawu dostępności](infrastructure-availability-sets-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ta konfiguracja daje gwarancję, że maszyny wirtualne są rozproszone w wielu domenach błędów i wdrożone na hostach z różnymi okresami konserwacji. Pełna treść [umowy SLA dotyczącej usługi Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wyjaśnia w całości kwestię gwarantowanej dostępności platformy Azure.

<a id="back-up-the-vm" class="xliff"></a>

### Tworzenie kopii zapasowych maszyn wirtualnych
[Magazyn usługi Recovery Services](../../backup/backup-introduction-to-azure-backup.md) służy do ochrony danych i zasobów w usługach Azure Backup i Azure Site Recovery. Magazynu usługi Recovery Services można użyć do [wdrażania kopii zapasowych maszyn wirtualnych wdrożonych przy użyciu usługi Resource Manager oraz zarządzania nimi przy użyciu programu PowerShell](../../backup/backup-azure-vms-automation.md). 

<a id="next-steps" class="xliff"></a>

## Następne kroki
* Użytkownicy korzystający z maszyn wirtualnych z systemem Linux powinni zapoznać się z tematem [Azure and Linux](../linux/overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Usługa Azure a system Linux).
* Więcej informacji na temat wytycznych dotyczących konfigurowania infrastruktury można znaleźć w artykule [Example Azure infrastructure walkthrough](infrastructure-example.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Przewodnik po konfigurowaniu przykładowej infrastruktury na platformie Azure).
* Zdecydowanie zalecamy korzystanie z [najlepszych rozwiązań dotyczących uruchamiania maszyny wirtualnej z systemem Windows na platformie Azure](guidance-compute-single-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


