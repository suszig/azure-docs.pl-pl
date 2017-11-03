---
title: "Maszyny wirtualne systemu Linux platformy Azure i usługi Azure Storage | Dokumentacja firmy Microsoft"
description: "Zawiera opis usługi Azure Standard i Magazyn w warstwie Premium i zarówno zarządzane i niezarządzane dysków z maszyn wirtualnych systemu Linux."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: d364c69e-0bd1-4f80-9838-bbc0a95af48c
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 2/7/2017
ms.author: rasquill
ms.openlocfilehash: 934580f6fcfdbff6e61626ed685459478559717d
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="azure-and-linux-vm-storage"></a>Magazyn Azure i maszyny Wirtualnej systemu Linux
Usługa Azure Storage to rozwiązanie magazynu w chmurze dla nowoczesnych aplikacji, które polegają na trwałości, dostępności i skalowalności, aby spełniać potrzeby klientów.  Poza umożliwieniem deweloperom tworzenie aplikacji na dużą skalę do obsługi nowych scenariuszy, Azure Storage stanowi również podstawowy magazyn maszyn wirtualnych platformy Azure.

## <a name="managed-disks"></a>Managed Disks

Maszyny wirtualne platformy Azure są teraz dostępne za pomocą [Azure zarządzanych dysków](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), co umożliwia tworzenie maszyn wirtualnych bez tworzenia lub zarządzanie żadnego [konta usługi Azure Storage](../../storage/common/storage-introduction.md) samodzielnie. Możesz określić, czy Premium lub Standard storage i jak duże powinny być dysku, i Azure utworzy dysków maszyny Wirtualnej. Maszyny wirtualne z dyskami zarządzane mają wiele ważnych funkcji, w tym:

- Obsługa automatycznego skalowania. Azure tworzy dyski i zarządza powiązany magazyn do obsługi maksymalnie 10 000 dysków na subskrypcję.
- Zwiększenie niezawodności z zestawami dostępności. Azure zapewnia, że dyski maszyny Wirtualnej są automatycznie odizolowane od siebie nawzajem w obrębie zestawów dostępności.
- Kontrola dostępu zwiększona. Dyski zarządzanych ujawnia różnych operacji kontrolowane przez [based kontroli dostępu (RBAC)](../../active-directory/role-based-access-control-what-is.md).

Cennik dysków zarządzanych różni się od tego niezarządzane dysków. Informacje, zobacz [cennik i rozliczenia dla dysków zarządzanych](../windows/managed-disks-overview.md#pricing-and-billing).

Możesz przekonwertować istniejące maszyny wirtualne, które dysków niezarządzanych do zarządzanych dysków z [konwersji maszyny wirtualnej az](/cli/azure/vm#convert). Aby uzyskać więcej informacji, zobacz [jak przekonwertować Maszynę wirtualną systemu Linux z niezarządzanego dysków na dyskach zarządzanych Azure](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Nie można przekonwertować niezarządzane dysku na dysk zarządzany w przypadku niezarządzanych dysku na koncie magazynu, lub w dowolnym momencie zostały zaszyfrowane za pomocą [szyfrowanie usługi Magazyn Azure (SSE)](../../storage/common/storage-service-encryption.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Poniższe kroki szczegółowo sposób w celu konwersji niezarządzanego dysków, które są lub były na koncie magazynu zaszyfrowanych:

- Skopiuj wirtualny dysk twardy (VHD) z [rozpoczęcia kopiowania obiektu blob magazynu az](/cli/azure/storage/blob/copy#start) na konto magazynu, który nigdy nie został włączony dla szyfrowanie usługi Magazyn Azure.
- Utwórz maszynę Wirtualną, która używa dysków zarządzanych i określić tego pliku wirtualnego dysku twardego w trakcie tworzenia w programie [tworzenia maszyny wirtualnej az](/cli/azure/vm#create), lub
- Dołącz skopiowanego wirtualnego dysku twardego z [dołączyć dysku maszyny wirtualnej az](/cli/azure/vm/disk#attach) do uruchomionej maszyny Wirtualnej z dyskami zarządzanych.


## <a name="azure-storage-standard-and-premium"></a>Usługi Azure Storage: Standardowa i Premium
Maszyny wirtualne platformy Azure — czy za pomocą dysków zarządzanych lub niezarządzanych — mogą być wbudowane w system dyski magazynu w warstwie standardowa lub premium magazynu dysków. W przypadku użycia portalu do wyboru maszyny Wirtualnej musi Przełącz listy rozwijanej na **podstawy** ekranu, aby wyświetlić dyski zarówno warstwy standardowa i premium. Gdy przełączane do dysków SSD, tylko magazyn w warstwie premium włączone maszyny wirtualne będą wyświetlane, wszystkich kopii przez dysków SSD dyski.  Po ich dysk twardy, standardowy magazyn z włączonym maszyn wirtualnych przez Obracająca dysków są wyświetlane, wraz z premium magazynu maszyn wirtualnych obsługiwanych przez dysków SSD.

Podczas tworzenia maszyny Wirtualnej z `azure-cli` , możesz wybrać standard i premium w przypadku wybrania rozmiar maszyny Wirtualnej za pomocą `-z` lub `--vm-size` flagi interfejsu wiersza polecenia.

## <a name="creating-a-vm-with-a-managed-disk"></a>Tworzenie maszyny Wirtualnej z dyskiem zarządzanym

Poniższy przykład wymaga 2.0 interfejsu wiersza polecenia Azure, które można [zainstalować tutaj](/cli/azure/install-azure-cli).

Najpierw utwórz grupę zasobów do zarządzania zasobami z [Tworzenie grupy az](/cli/azure/group#create):

```azurecli
az group create --location westus --name myResourceGroup
```

Teraz utworzyć maszynę Wirtualną z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create). Określ unikatową `--public-ip-address-dns-name` argumentu, jako `mypublicdns` prawdopodobnie jest zajęty.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns
```

Poprzedni przykład tworzy Maszynę wirtualną z dysków zarządzanych w ramach konta magazynu w warstwie standardowa. Aby użyć konta magazynu w warstwie Premium, Dodaj `--storage-sku Premium_LRS` argumentu, jak w następującym przykładzie:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns \
    --storage-sku Premium_LRS
```

## <a name="standard-storage"></a>Standard Storage
Standardowy magazyn Azure to domyślny typ magazynu.  Standardowy magazyn jest ekonomiczne przy zachowaniu wydajności.  

## <a name="premium-storage"></a>Premium Storage
Usługa Azure Premium Storage oferuje obsługę wysokiej wydajności i małych opóźnieniach dysku dla maszyn wirtualnych uruchomionych/O wykonujących obciążeń. Dysków dla maszyny wirtualnej (VM) korzystające z magazynu Premium przechowywania danych na dyskach półprzewodnikowych (SSD). Dyski maszyn wirtualnych w aplikacji można migrować do usługi Azure Premium Storage wykorzystują szybkości i wydajności tych dysków.

Funkcje magazynu Premium:

* Dyski magazynu Premium: Usługa Azure Premium Storage obsługuje dyski maszyn wirtualnych, które można dołączyć do serii DS, DSv2 lub GS maszynach wirtualnych platformy Azure.
* Premium stronicowych obiektów Blob: Magazyn w warstwie Premium obsługuje Azure stronicowe obiekty BLOB, które są używane do przechowywania dysków trwałego na maszynach wirtualnych Azure (VM).
* Magazyn lokalnie nadmiarowy Premium: Konta Premium Storage tylko obsługuje lokalnie nadmiarowego magazynu (LRS) jako opcję replikacji i przechowuje trzy kopie danych w pojedynczym regionie.
* [Magazyn w warstwie Premium](../windows/premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Magazyn w warstwie Premium są obsługiwane maszyny wirtualne
Magazyn w warstwie Premium obsługuje serii DS, DSv2 serii, serie GS i maszynach wirtualnych Azure serii Fs (VM). Z magazynu Premium obsługiwanych maszyn wirtualnych służy dysków magazynu w wersjach Standard i Premium. Ale nie można używać dysków Premium Storage z serii maszyn wirtualnych, które nie są zgodne magazyn w warstwie Premium.

Poniżej przedstawiono dystrybucje systemu Linux możemy zweryfikować z magazyn w warstwie Premium.

| Dystrybucja | Wersja | Obsługiwane jądra |
| --- | --- | --- |
| Ubuntu |12.04 |3.2.0-75.110+ |
| Ubuntu |14.04 |3.13.0-44.73+ |
| Debian |7.x, 8.x |3.16.7-ckt4-1+ |
| SLES |SLES 12 |3.12.36-38.1+ |
| SLES |SLES 11 Z DODATKIEM SP4 |3.0.101-0.63.1+ |
| CoreOS |584.0.0+ |3.18.4+ |
| Centos |6.5, 6.6, 6.7, 7.0, 7.1 |3.10.0-229.1.2.el7+ |
| RHEL |6.8+, 7.2+ | |

## <a name="azure-file-storage"></a>Magazyn plików Azure
Magazyn plików Azure oferuje udziały plików w chmurze przy użyciu standardowego protokołu SMB. Przy użyciu plików Azure można migrować aplikacje dla przedsiębiorstw, które są oparte na serwerach plików w systemie Azure. Aplikacje działające na platformie Azure można łatwo zainstalować udziały plików z systemem Linux maszyn wirtualnych platformy Azure. I z najnowszej wersji pliku magazynu, można zainstalować udział plików z aplikacji lokalnej, która obsługuje protokół SMB 3.0.  Ponieważ udziały plików są udziałami SMB, możesz uzyskiwać do nich dostęp za pośrednictwem interfejsów API systemu plików standardowych.

Magazyn plików jest oparty na tej samej technologii co magazynu obiektów Blob, tabel i kolejek, więc magazyn plików oferuje dostępności, trwałości, skalowalność i nadmiarowość geograficzna, które są wbudowane w platformę Azure storage. Aby uzyskać więcej informacji o cele wydajności magazynu plików i limity Zobacz cele dotyczące wydajności i skalowalności magazynu Azure.

* [Jak używać usługi Azure File Storage z systemem Linux](../../storage/files/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Magazynu gorącego
Warstwa magazynu gorącego Azure jest zoptymalizowana pod kątem przechowywania danych często uzyskuje się dostęp.  Magazynu gorącego jest domyślny typ magazynu dla magazynów obiektów blob.

## <a name="cool-storage"></a>Magazynu chłodnego
Warstwa magazynu chłodnego Azure jest zoptymalizowany do przechowywania danych, których rzadko uzyskuje się dostęp i długotrwałe. Przykładowe przypadki użycia do magazynu chłodnego obejmują kopii zapasowych, zawartości multimedialnej danych naukowych, zgodności i archiwizowania danych. Ogólnie rzecz biorąc dane, które rzadko jest dostępny jest idealne kandydatem do magazynu chłodnego.

|  | Warstwa magazynu gorącego | Warstwa magazynu chłodnego |
|:--- |:---:|:---:|
| Dostępność |99,9% |99% |
| Dostępność (odczyty RA-GRS) |99,99% |99,9% |
| Opłaty za zużycie |Wyższe koszty magazynowania |Niższe koszty magazynowania |
| Niższe dostępu |Wyższy dostępu | |
| i koszty transakcji |i koszty transakcji | |

## <a name="redundancy"></a>Nadmiarowość
Dane na koncie magazynu Microsoft Azure jest zawsze replikowane w celu zapewnienia trwałości i wysokiej dostępności, spotkania SLA magazynu platformy Azure, nawet w wypadku przejściowych awarii sprzętu.

Podczas tworzenia konta magazynu należy wybrać jedną z następujących opcji replikacji:

* Magazyn lokalnie nadmiarowy (LRS)
* Magazyn strefowo nadmiarowy (ZRS)
* Magazyn geograficznie nadmiarowy (GRS)
* Magazyn geograficznie nadmiarowy dostępny do odczytu (RA-GRS)

### <a name="locally-redundant-storage"></a>Magazyn lokalnie nadmiarowy
Magazyn lokalnie nadmiarowy (LRS) replikuje dane w regionie, w którym utworzono konto magazynu. Aby zmaksymalizować trwałości, każde żądanie dotyczące danych na koncie magazynu są replikowane trzy razy. Te trzy repliki każdego znajdują się w oddzielnych błędów domen i domen uaktualnienia.  Żądanie pomyślnie zwraca tylko wtedy, gdy został zapisany do wszystkie trzech repliki.

### <a name="zone-redundant-storage"></a>Magazyn strefowo nadmiarowy
Magazyn strefowo nadmiarowy (ZRS) replikuje dane w dwóch do trzech lokalizacjach, w jednym lub dwóch regionach, zapewniając większą trwałość niż magazyn LRS. Jeśli Twoje konto magazynu ZRS włączone, dane są trwałe nawet w przypadku awarii w jednym z obiektów.

### <a name="geo-redundant-storage"></a>Magazyn geograficznie nadmiarowy
Magazyn geograficznie nadmiarowy (GRS) replikuje dane w regionie pomocniczym będący setki odległości od regionu podstawowego. Jeśli konto magazynu ma GRS włączone, dane są trwałe nawet w przypadku pełnej regionalnej awarii lub awarii, w którym regionie podstawowym nie jest możliwe do odzyskania.

### <a name="read-access-geo-redundant-storage"></a>Dostęp do odczytu magazynu geograficznie nadmiarowego
Dostęp do odczytu magazynu geograficznie nadmiarowego (RA-GRS) maksymalizację dostępności dla konta magazynu, zapewniając dostęp tylko do odczytu do danych w dodatkowej lokalizacji, oprócz replikacji dwóch regionach podał grs w warstwie. W przypadku, gdy dane będą niedostępne w regionie podstawowym, aplikacja może odczytywać dane z regionu pomocniczego.

Dla nowości w Zobacz nadmiarowość magazynu Azure:

* [Replikacja usługi Azure Storage](../../storage/common/storage-redundancy.md)

## <a name="scalability"></a>Skalowalność
Usługa Azure Storage jest skalowalna na ogromną skalę, dzięki czemu można przechowywać i przetwarzać setki terabajtów danych na potrzeby obsługi scenariuszy korzystających z danych big data wymaganych w przypadku aplikacji naukowych, finansowych, analitycznych i multimedialnych. Możesz także przechowywać małe ilości danych przeznaczone dla witryny sieci Web małej firmy. Gdy Twoje potrzeby zmniejszą się, zapłacisz tylko za przechowywane dane. Obecnie Magazyn Azure przechowuje dziesiątki bilionów unikatowych obiektów klienckich i obsługuje średnio miliony żądań na sekundę.

W przypadku kont magazynu w warstwie standardowa: konta standard storage ma maksymalna całkowita liczba żądań liczba IOPS 20 000. Łączna liczba IOPS na wszystkich dyskach maszyny wirtualnej w koncie magazynu w warstwie Standardowa nie powinna przekroczyć tego limitu.

Dla kont premium magazynu: Konto magazynu w warstwie premium ma maksymalną przepustowość łączna liczba 50 GB/s. Całkowita przepływność na wszystkich dyskach maszyny wirtualnej nie powinna przekroczyć tego limitu.

## <a name="availability"></a>Dostępność
Firma Microsoft gwarantuje, że przez co najmniej 99,99% czasu (99,9% w przypadku warstwy dostępu chłodnego) będzie pomyślnie przetwarzać żądania odczytu danych z kont usługi Storage geograficznie nadmiarowy do odczytu (RA-GRS), pod warunkiem, że nieudane próby odczytu danych z regionu podstawowego zostaną ponowione w regionie pomocniczym.

Firma Microsoft gwarantuje, że przez co najmniej 99,9% czasu (99% w przypadku warstwy dostępu chłodnego) będzie pomyślnie przetwarzać żądania odczytu danych z kont usług Magazyn lokalnie nadmiarowy (LRS), Magazyn strefowo nadmiarowy (ZRS) i Magazyn geograficznie nadmiarowy (GRS).

Firma Microsoft gwarantuje, że przez co najmniej 99,9% czasu (99% w przypadku warstwy dostępu chłodnego) będzie pomyślnie przetwarzać żądania zapisu danych na kontach usług Magazyn lokalnie nadmiarowy (LRS), Magazyn strefowo nadmiarowy (ZRS), Magazyn geograficznie nadmiarowy (GRS) i Magazyn geograficznie nadmiarowy do odczytu (RA-GRS).

* [SLA platformy Azure dla magazynu](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)

## <a name="regions"></a>Regiony
Azure jest ogólnie dostępna w regionach 30 na całym świecie i ogłosiła plany 4 dodatkowe regionów. Ekspansja geograficzna ma dla platformy Azure priorytetowe znaczenie, ponieważ umożliwia klientom osiąganie większych wydajności i wspiera ich wymagania oraz preferencje w zakresie lokalizacji danych.  Jest azures najnowsze regionie można uruchomić w Niemczech.

Niemcy Microsoft Cloud zapewnia opcję zróżnicowanych do usługi Microsoft Cloud już dostępne w Europie, tworzenie zwiększenie możliwości innowacji i wzrostu gospodarczego dla dużej podlegającymi ochronie partnerów i klientów w Niemczech, Unii Europejskiej (UE) i skojarzenia Europejskiego wolnego handlu (ESWH).

Dane klienta w tych nowych centrów danych w Magdeburgu i Frankfurt, odbywa się pod kontrolą zarządca danych, międzynarodowej T systemów, niezależnie od firmy niemieckim i zależne Telekom marki. Firmy Microsoft komercyjnych usług chmurowych w tych centrach danych stosować się do obsługi przepisy dotyczące danych, niemieckim i zapewniają klientom dodatkowe opcje z jak i gdzie dane są przetwarzane.

* [Mapa regionów platformy Azure](https://azure.microsoft.com/regions/)

## <a name="security"></a>Bezpieczeństwo
Magazyn Azure oferuje rozbudowany zestaw funkcji zabezpieczeń, które razem umożliwiają deweloperom tworzenie bezpiecznych aplikacji. Samo konto magazynu mogą być chronione przy użyciu kontroli dostępu opartej na rolach i Azure Active Directory. Dane mogą być chronione przesyłanych między aplikacją a Azure za pomocą szyfrowania po stronie klienta, HTTPS lub SMB 3.0. Dane można ustawić automatycznie były szyfrowane, gdy zapisany w magazynie Azure przy użyciu szyfrowania usługi magazynu (SSE). Systemu operacyjnego i dysków z danymi używanych przez maszyny wirtualne można ustawić będą szyfrowane przy użyciu szyfrowania dysków Azure. Delegowany dostęp do obiektów danych w usłudze Azure Storage można otrzymać za pomocą sygnatury dostępu współdzielonego.

### <a name="management-plane-security"></a>Zarządzanie płaszczyzny zabezpieczeń
Płaszczyzny zarządzania składa się z zasobami umożliwiają zarządzanie kontem magazynu. W tej sekcji będziesz omawianiu modelu wdrażania usługi Azure Resource Manager oraz sposób korzystania z kontroli dostępu opartej na rolach (RBAC) do kontrolowania dostępu do kont magazynu. Zostanie również są omawiane zarządzania kluczami konta magazynu i jak można ponownie wygenerować je.

### <a name="data-plane-security"></a>Zabezpieczenia warstwy danych
W tej sekcji wyjaśniono zezwalania na dostęp do danych rzeczywistych obiektów na koncie magazynu obiektów blob, plików, kolejek i tabel, np. przy użyciu sygnatury dostępu współdzielonego i przechowywane zasad dostępu. Omówimy SAS poziomu usług i poziomie konta sygnatury dostępu Współdzielonego. Firma Microsoft będzie również sprawdzić, jak ograniczyć dostęp do określonego adresu IP (lub zakres adresów IP), jak ograniczyć protokół używany do HTTPS i jak odwołać sygnaturę dostępu współdzielonego bez oczekiwania na jej wygaśnięcie.

## <a name="encryption-in-transit"></a>Szyfrowanie podczas przesyłania
W tej sekcji omówiono sposób do zabezpieczania danych podczas transferu do lub z usługi Azure Storage. Będzie omawianiu zalecane użycie protokołu HTTPS i szyfrowania używany przez protokół SMB 3.0 do udziały plików platformy Azure. Firma Microsoft będzie także Spójrz na szyfrowanie po stronie klienta, co umożliwia szyfrowanie danych, zanim zostanie przekazany do magazynu w aplikacji klienckiej oraz do odszyfrowania danych po przeniesieniu poza magazynu.

## <a name="encryption-at-rest"></a>Szyfrowanie magazynowanych
Zostanie omawianiu szyfrowanie usługi Magazyn (SSE) i jak można ją włączyć dla konta magazynu, co powoduje blokowe, stronicowe obiekty BLOB i uzupełnialnych obiektów blob są szyfrowane automatycznie, gdy zapisany w magazynie Azure. Ponadto przedstawiono sposób użycia szyfrowania dysków Azure i poznać różnice podstawowych i przypadków szyfrowania dysku i SSE i szyfrowania po stronie klienta. Krótko przedstawiono zgodności ze standardem FIPS dla Stanów Zjednoczonych Komputery dla instytucji rządowych.

* [Przewodnik po zabezpieczeniach magazynu Azure](../../storage/common/storage-security-guide.md)

## <a name="temporary-disk"></a>Tymczasowe dysku
Każda maszyna wirtualna zawiera dysku tymczasowym. Dysku tymczasowym zapewnia krótkoterminowy magazyn dla aplikacji i procesów i jest przeznaczona tylko przechowywania danych, takich jak pliki strony lub wymiany. Dane na dysku tymczasowym mogą zostać utracone podczas [zdarzenia konserwacji](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) lub gdy użytkownik [ponownego wdrażania maszyny Wirtualnej](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Podczas standardowego ponownego uruchomienia maszyny wirtualnej ma utrwalić danych na dysku tymczasowym.

W przypadku maszyn wirtualnych systemu Linux, dysk jest zwykle **/dev/sdb** jest sformatowany i zainstalowany **katalogu/mnt** przez agenta systemu Linux platformy Azure. Rozmiar dysku tymczasowym różni się zależnie od rozmiaru maszyny wirtualnej. Aby uzyskać więcej informacji, zobacz [rozmiary maszyn wirtualnych systemu Linux](sizes.md).

Aby uzyskać więcej informacji o używaniu dysku tymczasowym Azure, zobacz [opis dysku tymczasowym na maszynach wirtualnych Azure firmy Microsoft](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="cost-savings"></a>Oszczędności
* [Koszt magazynowania](https://azure.microsoft.com/pricing/details/storage/)
* [Kalkulator magazynu koszt](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Limity magazynu
* [Ograniczenia usługi magazynu](../../azure-subscription-service-limits.md#storage-limits)
