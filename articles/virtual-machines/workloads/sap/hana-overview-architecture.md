---
title: "Omówienie i architektura SAP HANA na platformie Azure (wystąpienia duże) | Dokumentacja firmy Microsoft"
description: "Omówienie architektury wdrażanie SAP HANA na platformie Azure (wystąpienia duże)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/02/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e48e0e256306707ca7fde3636a4215b235fa2eb7
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Architektura na platformie Azure i SAP HANA (duże wystąpień) — omówienie

## <a name="what-is-sap-hana-on-azure-large-instances"></a>Co to jest SAP HANA na platformie Azure (wystąpienia duże)?

SAP HANA na platformie Azure (wystąpienia duże) to rozwiązanie unikatowa na platformie Azure. Oprócz maszynach wirtualnych platformy Azure na potrzeby wdrażania i uruchamiania SAP HANA Azure oferuje możliwości do uruchamiania i wdrożenia SAP HANA na serwerach bez systemu operacyjnego, które są przeznaczone do użytkownika jako klient. SAP HANA dla rozwiązania Azure (wystąpienia duże) oparty na sprzętu bez systemu operacyjnego nieudostępnione/serwera hosta, który jest przypisane do Ciebie jako klient. Sprzęt serwera jest osadzony w większych sygnatury, które zawiera compute/serwer, sieci i infrastruktury magazynu. Które jako połączenie jest certyfikowany TDI HANA. Oferta usługi SAP HANA na platformie Azure (wystąpienia duże) oferuje różnych wersji innego serwera lub rozmiary, począwszy od jednostek, które mają 72 procesorów i 768 GB pamięci RAM do jednostek mających 960 procesory i pamięć 20 TB.

Izolacji klienta w ramach infrastruktury sygnatury jest wykonywane w dzierżaw, które szczegółowo wygląda następująco:

- Sieci: Izolacja klientów w ramach infrastruktury stosu za pośrednictwem sieci wirtualnych dla poszczególnych dzierżawców odbiorcy przypisany. Dzierżawa jest przypisany do jednego odbiorcy. Klient może mieć wielu dzierżawców. Izolacja sieci dzierżawców uniemożliwia komunikację sieciową między dzierżaw w poziomie sygnatury infrastruktury. Nawet jeśli dzierżawcy należą do tego samego klienta.
- Składniki magazynu: izolacji za pomocą magazynu maszyn wirtualnych, które zostały przypisane do niej woluminów magazynu. Woluminy magazynu można przypisać do jednego magazynu tylko maszyny wirtualnej. Wyłącznie do jednego pojedynczej dzierżawy w stosie certyfikowanych infrastruktury SAP HANA TDI przypisano magazynu maszyny wirtualnej. W związku z tym woluminów magazynu przypisana do maszyny wirtualnej magazynu są dostępne w jednym określonym i pokrewnych dzierżawy tylko. I nie są widoczne od różnych dzierżawców wdrożone.
- Serwera lub hosta: jednostka serwera lub host nie jest udostępniana między klientów lub dzierżawców. Serwera lub hoście z klientem jest jednostką atomic obliczeniowe bez systemu operacyjnego, przypisane do jednej pojedynczej dzierżawy. **Nie** jest używane partycjonowanie sprzętowe lub partycjonowanie elastyczne można skutkiem, odbiorcy, udostępnianie hosta lub serwer z innego klienta. Do takiego serwera są zainstalowane woluminy magazynu, przypisanych do maszyny wirtualnej magazynu określonym dzierżawcy. Dzierżawca może zawierać jeden do wielu jednostek serwera różne jednostki magazynowe przypisana wyłącznie.
- W ramach SAP HANA w sygnaturze infrastruktury platformy Azure (wystąpienia duże) wiele różnych dzierżaw są wdrożone i odizolowane od siebie wzajemnie za pośrednictwem pojęcia dzierżawy w sieci, magazynu i poziomu obliczeniowej. 


Te jednostki bez systemu operacyjnego serwera są obsługiwane tylko uruchamianie SAP HANA. SAP warstwy aplikacji lub obciążeń pośredniczącym działa w maszynach wirtualnych platformy Azure firmy Microsoft. Sygnatury infrastruktury systemie SAP HANA jednostki Azure (wystąpienia duże) są podłączone do sieci Azure szkieletowymi, więc zapewniającą łączność małe opóźnienia między maszynami wirtualnymi Azure i SAP HANA w jednostkach Azure (wystąpienia duże).

Ten dokument jest jednym z wielu dokumentów, które obejmują SAP HANA na platformie Azure (wystąpienia duże). W tym dokumencie rozszerzana za pomocą podstawowej architektury, obowiązki, usług i ogólne za pośrednictwem możliwości rozwiązania. Dla większości obszarów, takich jak sieć i łączność cztery dokumenty są obejmującego szczegółowe informacje i przejść do szczegółów szczegółu. Dokumentację SAP HANA na platformie Azure (wystąpienia duże) nie obejmuje aspektów instalacji SAP NetWeaver lub wdrożenia SAP NetWeaver w maszynach wirtualnych platformy Azure. SAP NetWeaver na platformie Azure zostało opisane w oddzielnych dokumentów, w tym samym kontenerze dokumentacji platformy Azure. 


Różnych dokumentów wystąpienia dużych HANA wskazówek dotyczących opisano następujące kwestie:

- [Architektura na platformie Azure i SAP HANA (duże wystąpienia) — omówienie](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Infrastruktura SAP HANA (duże wystąpień) i łączność na platformie Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Jak zainstalować i skonfigurować SAP HANA (duże wystąpień) w systemie Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (duże wystąpień) wysokiej dostępności i odzyskiwania po awarii na platformie Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Rozwiązywanie problemów SAP HANA (duże wystąpień) i monitorowania na platformie Azure](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Wysoka dostępność skonfigurowane w systemie SUSE przy użyciu STONITH](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [Kopia zapasowa systemu operacyjnego i przywracania dla jednostki SKU II typu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

## <a name="definitions"></a>Definicje

Kilka wspólnych definicji są powszechnie używane w zakresie architektury i technicznej Deployment Guide. Zanotuj następujące pojęcia i ich znaczenie:

- **IaaS:** infrastruktura jako usługa
- **PaaS:** platforma jako usługa
- **SaaS:** oprogramowanie jako usługa
- **Składnik programu SAP:** poszczególnych aplikacji SAP, takie jak ECC, BW, Menedżer rozwiązania lub EP. Składniki programu SAP może bazować na tradycyjnych technologii ABAP lub Java lub aplikacji z systemem innym niż NetWeaver na podstawie takich jak obiektów biznesowych.
- **Środowisko SAP:** jeden lub więcej składników programu SAP logicznie pogrupowane do wykonywania funkcji biznesowych, takich jak projektowanie, QAS, szkolenia, odzyskiwania po awarii lub produkcji.
- **Poziomo SAP:** odwołuje się do całego zasoby SAP w orientacji poziomej Twojej IT. Poziomo SAP obejmuje wszystkie produkcyjnych i środowiskach nieprodukcyjnych.
- **Systemu SAP:** kombinację DBMS i warstwy aplikacji programistycznej SAP ERP, system testowy programu SAP BW SAP CRM produkcji systemu, itp. Azure wdrożenia nie obsługują dzielenia te dwie warstwy między lokalną i platformą Azure. Oznacza, że systemu SAP jest wdrożonych lokalnie lub jest wdrożony na platformie Azure. Jednak można wdrożyć różnych systemów pozioma SAP do platformy Azure lub lokalnie. Na przykład można wdrożyć SAP CRM systemów programowania i testowania na platformie Azure, podczas wdrażania SAP CRM produkcji systemu lokalnego. Dla SAP HANA na platformie Azure (wystąpienia duże) jest on przeznaczony hosta z warstwy aplikacji SAP systemów SAP w maszynach wirtualnych platformy Azure i powiązane wystąpieniem SAP HANA jednostkę sygnatura wystąpienia dużych HANA.
- **Sygnatura wystąpienia dużych:** stos infrastruktury sprzętu SAP HANA TDI certyfikowane i do uruchomienia wystąpień SAP HANA w obrębie platformy Azure w wersji dedykowanej.
- **SAP HANA na platformie Azure (duże wystąpienia):** oficjalną nazwą języka dla oferty na platformie Azure uruchomić HANA wystąpień w SAP HANA TDI certyfikowanego sprzętu, które zostało wdrożone w dużych wystąpienia sygnatury w różnych regionach platformy Azure. Powiązane termin **wystąpienia dużych HANA** jest skrót SAP HANA na platformie Azure (wystąpienia duże) i jest powszechnie używany ten przewodnik wdrożenia technicznego.
- **Między lokalizacjami:** opisano scenariusz wdrożonym maszyn wirtualnych z subskrypcją platformy Azure, lokacja lokacja, obejmujący wiele lokacji lub połączenia ExpressRoute między datacenter(s) lokalną i platformą Azure. Dokumentacja wspólnych Azure rodzaju wdrożenia również są opisane jako scenariusze między lokalizacjami. Przyczyna połączenia jest rozszerzenie domenami lokalnymi, Active Directory/OpenLDAP lokalnymi i DNS lokalnej na platformie Azure. Poziomo lokalnej jest rozszerzony do platformy Azure zasobów subskrypcji platformy Azure. Występuje to rozszerzenie, maszyn wirtualnych może być częścią domeny lokalnej. Użytkownicy domeny w domenie lokalnej mogą uzyskiwać dostęp do serwerów i usługi można uruchamiać na tych maszynach wirtualnych (takie jak usługi systemu DBMS). Możliwe jest komunikacji i rozpoznawania nazw między maszyn wirtualnych wdrożonych lokalnie i wdrożone maszyny wirtualne platformy Azure. Takie jest typowy scenariusz, w których większość SAP są wdrażane zasoby. Są one widoczne z [planowania i projektowania dla bramy sieci VPN](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) i [tworzenie sieci wirtualnej za pomocą połączenia lokacja-lokacja przy użyciu portalu Azure](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) więcej szczegółowych informacji.
- **Dzierżawy:** klienta wdrożony w dużych wystąpień HANA sygnatury pobiera izolowane do "dzierżawa". Dzierżawa jest izolowana w sieci, magazynu i warstwy obliczeniowej od pozostałych dzierżawców. Dlatego że jednostki magazynu i zasobów obliczeniowych, przypisane do różnych dzierżaw nie widać siebie lub komunikują się ze sobą na poziomie wystąpienia dużych HANA sygnatury. Klient może wybrać wdrożenia w różnych dzierżawców. Nawet wówczas nie występuje komunikacja między dzierżawcami na poziomie wystąpienia dużych HANA sygnatury.
- **Jednostka SKU kategoria:** HANA wystąpień dużych, dostępnych w następujących dwóch kategorii jednostki SKU.
    - **Typ I klasy:** S72, S72m S144, S144m, S192 i S192m
    - **Typ klasy II:** S384, S384m S384xm, S576, S768 i S960


Istnieje szereg dodatkowych zasobów, które zostały opublikowane na wdrożenie SAP obciążenia w chmurze publicznej Microsoft Azure. Zdecydowanie zaleca się, każdy planowania i wykonywania wdrożenia SAP HANA na platformie Azure jest doświadczony i korzystają z zasad IaaS platformy Azure i wdrożenia SAP obciążeń na platformie Azure IaaS. Następujące zasoby zawierają dodatkowe informacje i ma być utworzone odwołanie, aby kontynuować:


- [Za pomocą rozwiązania SAP na maszyny wirtualne Microsoft Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="certification"></a>Certyfikat

Oprócz certyfikacji NetWeaver SAP wymaga specjalnych certyfikacji dla SAP HANA do obsługi SAP HANA na niektórych infrastruktury, takich jak Azure IaaS.

Podstawowe SAP Uwaga na NetWeaver i certyfikacji SAP HANA stopnia jest [&#1928533; Uwaga SAP — aplikacje SAP na platformie Azure: typy obsługiwanych produktów i maszyny Wirtualnej Azure](https://launchpad.support.sap.com/#/notes/1928533).

To [SAP Uwaga #2316233 - SAP HANA w systemie Microsoft Azure (wystąpienia duże)](https://launchpad.support.sap.com/#/notes/2316233/E) również jest znacząca. Obejmuje ona rozwiązania opisane w tym przewodniku. Ponadto są obsługiwane do uruchamiania w typie GS5 maszyny Wirtualnej Azure SAP HANA. [Informacje w tym przypadku są publikowane w witrynie sieci Web programu SAP](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

SAP HANA na określone w &#2316233; Uwaga SAP rozwiązania Azure (wystąpienia duże) zapewnia klientom firmy Microsoft i SAP możliwość wdrażania duży pakiet Business SAP, SAP Business Warehouse (BW), S/4 HANA, BW/4HANA lub innych obciążeń SAP HANA na platformie Azure. Rozwiązanie opiera się na sygnatury certyfikowanym sprzętem dedykowanym SAP HANA ([SAP HANA dostosowane Datacenter integracja — TDI](https://scn.sap.com/docs/DOC-63140)). Uruchamianie jako SAP HANA TDI skonfigurowane rozwiązanie zapewnia bez obaw otrzymuje informacji o tym, że wszystkie aplikacje oparte na SAP HANA (w tym SAP Business Suite na SAP HANA, SAP Business magazynu (BW) SAP HANA, S4/HANA i BW4/HANA) będą działać w infrastrukturze sprzętu.

W porównaniu do uruchamiania SAP HANA w maszynach wirtualnych platformy Azure to rozwiązanie ma korzyści — zapewnia znacznie większych woluminów pamięci. Jeśli chcesz włączyć to rozwiązanie, są niektóre kluczowe aspekty, aby zrozumieć:

- Aplikacje warstwy i z systemem innym niż SAP aplikacji SAP są uruchamiane w maszynach wirtualnych platformy Azure (maszyny wirtualne) hostowanych w sygnatury zwykle Azure sprzętu.
- Infrastruktury lokalnej klienta, centrów danych i wdrożeń aplikacji są podłączone do platformy Microsoft Azure w chmurze za pomocą usługi Azure ExpressRoute (zalecane) lub wirtualnej sieci prywatnej (VPN). Active Directory (AD) i DNS są również rozszerzać na platformie Azure.
- Wystąpienie bazy danych SAP HANA HANA obciążenia działa na SAP HANA na platformie Azure (wystąpienia duże). Sygnatura wystąpienia dużych jest połączony w sieci Azure, więc oprogramowanie na maszynach wirtualnych Azure zakłócają działania w wystąpieniach dużych HANA wystąpienia HANA.
- Sprzętu SAP HANA na platformie Azure (wystąpienia duże) jest dedykowany sprzęt podać w przypadku infrastruktury jako usługi (IaaS) za pomocą systemu SUSE Linux Enterprise Server lub Red Hat Enterprise Linux, wstępnie zainstalowane. Jako usługi Azure Virtual Machines dalsze aktualizacje i obsługa systemu operacyjnego odpowiada użytkownik.
- Instalacja HANA lub dodatkowe składniki niezbędne do uruchomienia SAP HANA w jednostkach HANA dużych wystąpień odpowiada, jest odpowiednie wszystkie trwające operacje i administracji SAP HANA na platformie Azure.
- Oprócz rozwiązania opisane w tym miejscu można zainstalować inne składniki w Twojej subskrypcji platformy Azure, łączącego się SAP HANA na platformie Azure (wystąpienia duże).  Na przykład składniki, które umożliwiają komunikację z i/lub bezpośrednio do bazy danych SAP HANA (skok, serwery protokołu RDP SAP HANA Studio usług danych SAP dla scenariuszy analizy Biznesowej programu SAP lub rozwiązań w zakresie monitorowania sieci).
- Jak Azure wystąpień dużych HANA oferują obsługi funkcji wysokiej dostępności i odzyskiwania po awarii.

## <a name="architecture"></a>Architektura

Na ogólne SAP HANA dla rozwiązania Azure (wystąpienia duże) ma warstwy aplikacji SAP znajdującej się na maszynach wirtualnych Azure i warstwy bazy danych znajdującej się na sprzęcie TDI SAP skonfigurowane znajduje się w sygnaturze dużych wystąpienia, w tym samym regionie Azure, podłączoną do IaaS platformy Azure.

> [!NOTE]
> Należy wdrożyć SAP warstwy aplikacji w tym samym regionie Azure, jako warstwa SAP DBMS. Ta reguła jest dobrze udokumentowane w opublikowanych informacji o SAP obciążenia na platformie Azure. 

Ogólna architektura SAP HANA na platformie Azure (wystąpienia duże) zapewnia TDI SAP certyfikowane konfiguracja sprzętu (niezwirtualizowanych, systemu operacyjnego, wysokiej wydajności serwera bazy danych SAP HANA) oraz możliwości i elastyczność Azure do zasobów skalowania warstwy aplikacji SAP do własnych potrzeb.

![Omówienie architektury SAP HANA na platformie Azure (wystąpienia duże)](./media/hana-overview-architecture/image1-architecture.png)

Architektura wyświetlany jest podzielony na trzy części:

- **Prawo:** infrastruktury lokalnej z różnych aplikacji w centrach danych użytkownikom końcowym dostęp do aplikacji FIRMOWYCH (np. SAP). W idealnym przypadku to lokalnej infrastruktury następnie jest podłączony do platformy Azure przy użyciu platformy Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Centrum:** pokazuje IaaS platformy Azure i w takim przypadku użycie maszyn wirtualnych platformy Azure do hostowania SAP lub inne aplikacje, które używają SAP HANA jako DBMS system. Mniejsze wystąpień HANA zapewniające funkcji z pamięci maszyn wirtualnych platformy Azure są wdrażane w maszynach wirtualnych platformy Azure, wraz z ich warstwy aplikacji. Dowiedz się więcej o [maszyn wirtualnych](https://azure.microsoft.com/services/virtual-machines/).
<br />Azure Networking służy do grupowania systemów SAP wraz z innych aplikacji w sieciach wirtualnych platformy Azure (sieci wirtualne). Te sieci wirtualne nawiązać połączenia z systemów lokalnych, jak również dotyczące SAP HANA na platformie Azure (wystąpienia duże).
<br />Dla programu SAP NetWeaver aplikacji i baz danych, które są obsługiwane do uruchamiania na platformie Microsoft Azure, zobacz [&#1928533; SAP Obsługa Uwaga — aplikacje SAP na platformie Azure: typy obsługiwanych produktów i maszyny Wirtualnej Azure](https://launchpad.support.sap.com/#/notes/1928533). Dokumentacja na temat wdrażania rozwiązania SAP na platformie Azure należy przejrzeć:

  -  [Za pomocą programu SAP na maszynach wirtualnych systemu Windows (VM)](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Za pomocą rozwiązania SAP na maszyny wirtualne Microsoft Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Po lewej:** pokazuje SAP HANA TDI certyfikowanego sprzętu w sygnatura wystąpienia dużych Azure. Jednostki HANA dużych wystąpienia są połączone z sieciami wirtualnymi platformy Azure z subskrypcji przy użyciu tej samej technologii co łączność z lokalnymi na platformie Azure.

Sygnatura wystąpienia dużych Azure sam zawierają następujące składniki:

- **Obliczanie:** serwerów, które są oparte na Intel Xeon E7-8890v3 lub Intel Xeon E7-8890v4 procesorów, które zawierają niezbędne możliwości obliczeniowych i są SAP HANA certyfikowane.
- **Sieć:** A unified szybkich sieci szkieletowej, łączącego przetwarzania danych, magazynu i składników sieci LAN.
- **Magazyn:** infrastruktury magazynu, który jest dostępny za pośrednictwem ujednoliconego sieci szkieletowej. Określonej pojemności znajduje się w zależności od określonych SAP HANA konfiguracji platformy Azure (wystąpienia duże) wdrażany (większej pojemności magazynu jest dostępna w dodatkowych miesięczny koszt).

W wielodostępnej infrastrukturze sygnatura wystąpienia dużych klienci są wdrażane jako izolowanych dzierżaw. Na wdrożenie dzierżawcy musisz Nazwa subskrypcji platformy Azure w ramach Twojej rejestracji platformy Azure. Ta będzie subskrypcji platformy Azure, będzie będą naliczane opłaty przed wystąpienia dużych HANA. Te dzierżawy mają relację 1:1 z subskrypcją platformy Azure. Rozsądnego sieci jest możliwe do HANA dużych wystąpienia jednostki wdrożone w jednej dzierżawy w jednym regionie Azure z różnych sieci wirtualnych platformy Azure, która należy do innej subskrypcji platformy Azure. Chociaż te subskrypcje platformy Azure muszą należeć do tej samej rejestracji platformy Azure. 

Podobnie jak w przypadku maszyn wirtualnych platformy Azure, SAP HANA na platformie Azure (wystąpienia duże) jest oferowana w wielu regionach platformy Azure. Aby oferują możliwości odzyskiwania po awarii, można włączyć. Różne sygnatury dużych wystąpienie w ramach jednego regionu politycznej geograficznie są połączone ze sobą. Na przykład HANA dużych wystąpienia sygnatury nam zachód i nam wschodnie są połączone za pośrednictwem łącza dedykowanej sieci na potrzeby replikacji odzyskiwania po awarii. 

Tak samo, jak można wybrać różne typy maszyny Wirtualnej z maszyn wirtualnych platformy Azure, są dostępne różne jednostki SKU z HANA dużych wystąpień dostosowanych dla obciążenia różnych rodzajów SAP HANA. SAP dotyczy stosunek gniazda procesora dla różnych obciążeń oparte na generacje procesorów Intel pamięci. W poniższej tabeli przedstawiono typy jednostki SKU oferowane.

Począwszy od 2017 lipca SAP HANA na platformie Azure (wystąpienia duże) jest dostępny w kilka konfiguracji w programie Azure regionów z NAMI zachodnie i nam wschodnie, Australia Wschodnia, Australia Południowo-Wschodnia, Europa Zachodnia i Europa Północna:

| Rozwiązanie SAP | Procesor CPU | Memory (Pamięć) | Magazyn | Dostępność |
| --- | --- | --- | --- | --- |
| Zoptymalizowana pod kątem OLAP: programu SAP BW BW/4HANA<br /> lub SAP HANA dla ogólnych obciążeń OLAP | SAP HANA na Azure S72<br /> — 2 procesor Intel Xeon® x E7 8890 v3<br /> 36 rdzeni Procesora i 72 wątków CPU |  768 GB |  3 TB | Dostępna |
| --- | SAP HANA na Azure S144<br /> – 4 procesor Intel Xeon® x E7 8890 v3<br /> 72 rdzeni Procesora i 144 wątków CPU |  1,5 TB |  6 TB | Nieoferowane już |
| --- | SAP HANA na Azure S192<br /> – 4 procesor Intel Xeon® x E7 8890 v4<br /> 96 rdzeni Procesora i 192 wątków CPU |  2.0 TB |  8 TB | Dostępna |
| --- | SAP HANA na Azure S384<br /> – 8 procesor Intel Xeon® x E7 8890 v4<br /> 192 rdzeni Procesora i 384 wątków CPU |  4.0 TB |  16 TB | Gotowy do zlecenia |
| Zoptymalizowana pod kątem OLTP: SAP Business Suite<br /> SAP HANA lub S/4HANA (OLTP)<br /> ogólny OLTP | SAP HANA na Azure S72m<br /> — 2 procesor Intel Xeon® x E7 8890 v3<br /> 36 rdzeni Procesora i 72 wątków CPU |  1,5 TB |  6 TB | Dostępna |
|---| SAP HANA na Azure S144m<br /> – 4 procesor Intel Xeon® x E7 8890 v3<br /> 72 rdzeni Procesora i 144 wątków CPU |  3.0 TB |  12 TB | Nieoferowane już |
|---| SAP HANA na Azure S192m<br /> – 4 procesor Intel Xeon® x E7 8890 v4<br /> 96 rdzeni Procesora i 192 wątków CPU  |  4.0 TB |  16 TB | Dostępna |
|---| SAP HANA na Azure S384m<br /> – 8 procesor Intel Xeon® x E7 8890 v4<br /> 192 rdzeni Procesora i 384 wątków CPU |  6.0 TB |  18 TB | Gotowy do zlecenia |
|---| SAP HANA na Azure S384xm<br /> – 8 procesor Intel Xeon® x E7 8890 v4<br /> 192 rdzeni Procesora i 384 wątków CPU |  8.0 TB |  22 TB |  Gotowy do zlecenia |
|---| SAP HANA na Azure S576<br /> – 12 procesor Intel Xeon® x E7 8890 v4<br /> 288 rdzeni Procesora i 576 wątków CPU |  12.0 TB |  28 TB | Gotowy do zlecenia |
|---| SAP HANA na Azure S768<br /> – 16 procesor Intel Xeon® x E7 8890 v4<br /> 384 rdzeni Procesora i 768 wątków CPU |  16.0 TB |  36 TB | Gotowy do zlecenia |
|---| SAP HANA na Azure S960<br /> – 20 procesor Intel Xeon® x E7 8890 v4<br /> 480 rdzeni Procesora i 960 wątków CPU |  20.0 TB |  46 TB | Gotowy do zlecenia |

- Rdzenie procesora CPU = suma nie-Procesora rdzeni hiperwątkowych sumy procesorów jednostki serwera.
- Wątków procesora CPU = Suma wątków obliczeniowe udostępniane przez hiperwątkowych rdzeni procesora CPU sumy procesorów jednostki serwera. Wszystkie jednostki są domyślnie skonfigurowane, aby użyć funkcji Hyper-Threading.


Odwołuje się różne konfiguracje powyżej, które są dostępne lub "nie są oferowane już" [&#2316233; SAP Obsługa Uwaga — SAP HANA w systemie Microsoft Azure (wystąpienia duże)](https://launchpad.support.sap.com/#/notes/2316233/E). Konfiguracje, które są oznaczone jako "Gotowy do kolejności" znajdzie ich wejściem w nocie SAP wkrótce. Jednak te wystąpienia jednostki SKU może zostać określona już na sześciu różnych regionach platformy Azure, że dostępna jest usługa HANA dużych wystąpienia.

Konkretne konfiguracje wybrane są zależne od obciążenia, zasobów procesora CPU i pamięci żądany. Istnieje możliwość dla obciążenia OLTP jednostki magazynowe, które są optymalizowane na potrzeby obciążeń OLAP. 

Podstawa sprzętu wszystkie oferty są SAP HANA TDI certyfikowane. Jednak wyróżnia się między dwoma różnymi klasami sprzętu, która dzieli jednostki SKU do:

- S72, S72m S144, S144m, S192 i S192m, który nazywamy "Typ klasy I" z jednostki SKU.
- S384, S384m S384xm, S576, S768 i S960, który nazywamy "klasy typu II' SKU.

Należy pamiętać, że pełne sygnatury HANA dużych wystąpienia nie jest wyłącznie przydzielona dla jednego odbiorcy &#39; Użyj s. Ten fakt dotyczy stojakami zasoby obliczeniowe i magazynujące, które są połączone za pośrednictwem sieci szkieletowej również wdrożona na platformie Azure. Duże wystąpień HANA infrastruktury, takich jak Azure, wdraża różnych klientów &quot;dzierżaw&quot; które są odizolowane od siebie w następujących trzech poziomach:

- Sieci: Izolacja za pośrednictwem sieci wirtualnych w ramach sygnatura wystąpienia dużych HANA.
- Magazyn: Izolacji za pomocą magazynu maszyn wirtualnych, które mają woluminów magazynu przypisane i izolowanie woluminów magazynu między dzierżawcami.
- Obliczeniowe: Przypisanie dedykowanego serwera jednostki do pojedynczej dzierżawy. Nie trudne lub partycjonowanie elastyczne jednostek serwera. Bez udostępniania jednego serwera lub hosta jednostki między dzierżawcami. 

Tak wdrożenia HANA wystąpień dużych jednostek od różnych dzierżawców nie są widoczne dla siebie nawzajem. Nie można HANA dużych jednostek wystąpienia wdrożone w różnych dzierżawców komunikować się bezpośrednio ze sobą na poziomie wystąpienia dużych HANA sygnatury. Tylko HANA dużych wystąpienia jednostek w ramach jednego dzierżawcy mogą komunikować się ze sobą na poziomie wystąpienia dużych HANA sygnatury.
Wdrożone dzierżawcy w dużych wystąpienia sygnatury przypisano rozliczeń się jedną subskrypcją platformy Azure. Jednak sieci rozsądnego, są dostępne z sieciami wirtualnymi platformy Azure z innych subskrypcji platformy Azure w ramach tej samej rejestracji platformy Azure. W przypadku wdrożenia z innej subskrypcji platformy Azure, w tym samym regionie Azure, również możesz poprosić o rozdzielonych dzierżawy HANA dużych wystąpienia.

Brak istotnych różnic między systemem SAP HANA HANA dużych wystąpień i SAP HANA działające na maszynach wirtualnych Azure wdrożona na platformie Azure:

- Nie ma żadnych warstwa wirtualizacji dla SAP HANA na platformie Azure (wystąpienia duże). Otrzymasz wydajności używanego sprzętu bez systemu operacyjnego.
- W przeciwieństwie do usługi Azure SAP HANA na serwerze Azure (wystąpienia duże) jest dedykowany do określonego klienta. Nie było możliwości czy jednostki serwera lub hosta jest trudne lub elastyczne podzielona na partycje. W związku z tym jednostki HANA dużych wystąpienia jest używany jako przypisane jako całość do dzierżawy i z tym klientem. Ponowne uruchomienie lub zamknięcie serwera nie umożliwia automatycznego systemu operacyjnego i SAP HANA wdrażany na innym serwerze. (Dla typu klasy I jednostki SKU, jedynym wyjątkiem jest, jeśli serwer mogą wystąpić problemy i ponowne wdrożenie musi zostać wykonana na innym serwerze.)
- W przeciwieństwie do usługi Azure, których typy procesora hosta w przypadku wybrania najlepszych stosunku ceny do wydajności są typy procesora wybrany dla SAP HANA na platformie Azure (wystąpienia duże) najwyższy wykonywanie wierszu procesor Intel E7v3 i E7v4.


### <a name="running-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Uruchamianie wielu wystąpień SAP HANA na jedną jednostkę HANA dużych wystąpienia
Istnieje możliwość hostowania więcej niż jedno wystąpienie SAP HANA aktywne, w jednostkach HANA dużych wystąpienia. Aby w dalszym ciągu dostępne możliwości magazynu migawek i odzyskiwanie po awarii, taka konfiguracja wymaga woluminu z ustawić dla każdego wystąpienia. Od tej chwili jednostki wystąpienia dużych HANA może zostać podzielona w następujący sposób:

- S72, S72m, S144, S192: W przyrostach 256 GB 256 GB najmniejszą początkowa jednostki. Maksymalna pamięć jednostki łączenie wielokrotności jak 256 GB, 512 GB i tak dalej.
- S144m i S192m: W przyrostach 256 GB 512 GB najmniejsza jednostka. Maksymalna pamięć jednostki łączenie wielokrotności jak 512 GB, 768 GB i tak dalej.
- Typ klasy II: wielokrotnością 512 GB z najmniejszą początkowa jednostki 2 TB. Maksymalna pamięć jednostki łączenie wielokrotności jak 512 GB, 1 TB, 1,5 TB i tak dalej.

Przykłady uruchamianie wielu wystąpień SAP HANA może wyglądać jak:

| SKU | Rozmiar pamięci | Rozmiar magazynu | Rozmiary z wieloma bazami danych |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | Wystąpienie HANA 1 x 768 GB<br /> lub wystąpienie 1 x 512 GB + 1 x 256 GB wystąpienia<br /> lub wystąpień 3 x 256 GB | 
| S72m | 1,5 TB | 6 TB | 3x512GB HANA wystąpień<br />lub wystąpienie 1 x 512 GB + 1 x 1 TB wystąpienia<br />lub wystąpień 6 x 256 GB<br />lub wystąpienie 1x1.5 TB | 
| S192m | 4 TB | 16 TB | 8 x 512 GB wystąpień<br />lub wystąpień 4 x 1 TB<br />lub wystąpień 4 x 512 GB + 2 x 1 TB wystąpień<br />lub wystąpień 4 x 768 GB + 2 x 512 GB wystąpień<br />lub wystąpienie. 1 x 4 TB |
| S384xm | 8 TB | 22 TB | 4 x 2 TB wystąpień<br />lub wystąpień 2 x 4 TB<br />lub wystąpień 2 x 3 TB + 1 x 2 TB wystąpień<br />lub wystąpień 2x2.5 TB + 1 x 3 TB wystąpień<br />lub wystąpienie. 1 x 8 TB |


Otrzymasz pomysł. Na pewno istnieją także inne różnice. 

### <a name="using-sap-hana-data-tiering-and-extension-nodes"></a>Przy użyciu rozszerzenia i SAP HANA danych warstwy węzłów
SAP obsługuje model warstwy danych programu SAP BW różne wersje programu SAP NetWeaver i SAP BW/4HANA. Szczegóły dotyczące do obsługi danych modelu znajdują się w tym dokumencie i blog odwołuje się w tym dokumencie SAP: [SAP BW/4HANA SAP BW ON HANA z SAP HANA rozszerzenia węzły i](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
Wystąpieniami dużych HANA można użyć opcji-1 konfiguracji SAP HANA rozszerzenia węzłów zgodnie z opisem w tym dokumentach blogu — często zadawane pytania i SAP. Konfiguracje opcja 2 można można skonfigurować przy użyciu następujących HANA dużych wystąpienia jednostki SKU: S72m, S192 S192m, S384 i S384m.  
Wyszukiwanie w dokumentacji zaletą może nie być widoczna od razu. Ale wyszukiwania w wytycznych zmiany rozmiaru SAP, mogą przeglądać korzyści przy użyciu opcji-1 i 2 opcja węzłów rozszerzenia SAP HANA. W tym przykładzie:

- Wskazówki dotyczące rozmiaru SAP HANA zwykle wymaga double ilości ilość danych jako pamięci. Tak uruchamiając wystąpieniem SAP HANA gorących danych, wystarczy tylko 50% lub mniej pamięci wypełnione z danymi. W pozostałej części pamięci w idealnym przypadku odbywa się dla SAP HANA podczas pracy.
- To oznacza w jednostce HANA dużych S192 wystąpienia z 2 TB pamięci z bazy danych programu SAP BW, wystarczy tylko 1 TB jako ilość danych.
- Użycie dodatkowych SAP HANA węzeł rozszerzenia opcji-1, również S192 HANA dużych wystąpienia jednostki SKU, spowodowałoby to nadanie można zwiększyć pojemność 2 TB dla woluminów danych. W konfiguracji 2 opcja 4 nawet i dodatkowe TB, który ciepłych danych woluminu. W porównaniu do węzła gorących pojemności pamięci pełne węzeł rozszerzenia "ciepłych" może służyć do danych przechowywania dla opcji-1 i podwójnego pamięci może służyć do woluminów danych w konfiguracji węzła Rozszerzenia SAP HANA opcja 2.
- W związku z tym na końcu o pojemności 3 TB danych oraz stosunku hot przez ciepłych 1:2 dla opcji-1 i 5 TB danych i stosunek 1:4 w konfiguracji węzła Rozszerzenia opcja 2.

Jednak im większa ilość danych w porównaniu do pamięci, że ciepłych danych możesz są pytania o wyższa są prawdopodobieństwo jest przechowywana na dysku magazynu.


## <a name="operations-model-and-responsibilities"></a>Operacje modelu i jego obowiązki

Usługi z SAP HANA na platformie Azure (wystąpienia duże) jest wyrównywana z usług Azure IaaS. Możesz pobrać wystąpienia HANA dużych wystąpień z zainstalowanym systemem operacyjnym jest zoptymalizowany pod kątem SAP HANA. Podobnie jak w przypadku maszyn wirtualnych z IaaS platformy Azure, większość zadań wzmocnienia zabezpieczeń systemu operacyjnego, instalowania dodatkowego oprogramowania muszą HANA instalowania, obsługi systemu operacyjnego i HANA, a aktualizacja systemu operacyjnego i HANA odpowiada. Microsoft nie wymusza aktualizacje systemu operacyjnego lub HANA aktualizacje w przypadku.

![Obowiązki SAP HANA na platformie Azure (wystąpienia duże)](./media/hana-overview-architecture/image2-responsibilities.png)

Jak widać na powyższym diagramie, SAP HANA na platformie Azure (wystąpienia duże) jest wielodostępne infrastruktury jako oferty usługi. I w związku z tym podział odpowiedzialności na granicy infrastruktury systemu operacyjnego, w większości przypadków. Microsoft jest odpowiedzialny za wszystkie aspekty usługi poniżej wiersza system operacyjny i jest odpowiedzialny powyżej wiersza, w tym system operacyjny. Dlatego najbardziej aktualne lokalnymi metody mogą być których zastosowano zgodności, zabezpieczeń, zarządzanie aplikacjami, podstawy i systemu operacyjnego zarządzania można w dalszym ciągu używane. Systemy są wyświetlane tak, jakby znajdują się w sieci w odniesieniu do wszystkich.

Jednak ta usługa jest zoptymalizowana pod kątem SAP HANA, więc obszary, w których Tobą a firmą Microsoft musi współdziałać, aby korzystać z podstawowej funkcji infrastruktury dla uzyskania najlepszych wyników.

Poniższa lista zawiera więcej szczegółów na poszczególnych warstw i Twoje obowiązki:

**Sieć:** sieciach wewnętrznych dla sygnatury dużych wystąpienia systemem SAP HANA dostępu do magazynu, łączność między wystąpienia (dla skalowalnego w poziomie i inne funkcje), połączenie pozioma i łączności do platformy Azure gdzie SAP warstwy aplikacji jest hostowana w maszynach wirtualnych platformy Azure. Zawiera także połączenia sieci WAN między centrach danych platformy Azure dla replikacji celów odzyskiwania po awarii. Wszystkie sieci są podzielone na partycje przez dzierżawcę, oraz zastosować QOS.

**Magazyn:** zwirtualizowany magazynu dla wszystkich woluminów wymagane przez serwery SAP HANA, a także migawek na partycje. 

**Serwery:** dedykowanych serwerów fizycznych do uruchomienia baz danych HANA SAP, które zostały przypisane do dzierżawcy. Serwery o typie I klas jednostek magazynowych są pobieranej sprzętu. Z tych typów serwerów konfiguracji serwera jest zbierane i zarządzania profilami, które mogą zostać przeniesione z jednego fizycznego sprzętu do innego sprzętu fizycznego. Takie (ręczna) przenoszenie profilu przez operacje można porównać coś do naprawą usługi Azure. Serwery jednostki SKU klasy II typu nie są oferty takiej możliwości.

**SDDC:** oprogramowanie do zarządzania, który służy do zarządzania danymi w centrach oprogramowania zdefiniowane jednostki. Umożliwia firmy Microsoft do puli zasobów dla skali, dostępności i optymalnej wydajności.

**Systemu operacyjnego:** systemu operacyjnego, należy wybrać (SUSE Linux lub Red Hat Linux) uruchomionego na serwerze. Obrazy systemu operacyjnego, które są dostarczane są obrazy dostarczone przez poszczególne dostawcę Linux do firmy Microsoft na potrzeby uruchamiania SAP HANA. Należy mieć subskrypcję z dostawcą systemu Linux dla określonego obrazu zoptymalizowanych pod kątem SAP HANA. Twoje obowiązki obejmują rejestrowania obrazów za pomocą dostawcy systemu operacyjnego. Z punktu przekazanie przez firmę Microsoft możesz są również odpowiedzialne za wszelkie dodatkowe stosowanie poprawek systemu operacyjnego Linux. Tej poprawki również obejmuje dodatkowe pakiety, które mogą być wymagane do pomyślnej instalacji SAP HANA (zobacz firmy SAP HANA instalacji dokumentacji i uwagi SAP) i które nie zostały uwzględnione określonego dostawcy systemu Linux w ich SAP HANA zoptymalizowanych obrazów systemu operacyjnego. Odpowiedzialność klienta obejmuje również stosowanie poprawek systemu operacyjnego, które jest powiązane z usterki/optymalizacji systemu operacyjnego i sterowników dotyczące sprzętu określonego serwera. Lub zabezpieczeń ani funkcjonalności stosowania poprawek systemu operacyjnego. Odpowiedzialność klienta jest również monitorowania i planowania pojemności programu:

- Zużycie zasobów procesora CPU
- Zużycie pamięci
- Woluminy na dyskach związane z wolnego miejsca, IOPS i opóźnienia
- Ruch sieciowy woluminu między wystąpieniem dużych HANA i warstwy aplikacji SAP

Podstawowej infrastruktury wystąpień dużych HANA udostępnia funkcje tworzenia kopii zapasowych i przywracania woluminu systemu operacyjnego. Z tej funkcji jest również obowiązkiem użytkownika.

**Oprogramowanie pośredniczące:** głównie wystąpienia SAP HANA. Administracji, operacje i monitorowania są Twoje odpowiedzialności. Brak funkcji, pod warunkiem, który pozwala na użycie pamięci masowej migawki kopii zapasowej/przywracania i odzyskiwania po awarii. Te możliwości są udostępniane przez infrastrukturę. Jednak Twoje obowiązki również obejmować projektowanie wysokiej dostępności i odzyskiwania po awarii dzięki takim funkcjom, wykorzystaniu je i monitorowania, czy migawki magazynu zostały wykonane pomyślnie.

**Dane:** danych zarządzanych przez SAP HANA i innych danych, takich jak kopie zapasowe udziałów plików znajdujących się na woluminach lub pliku. Twoje obowiązki obejmują monitorowania wolnego miejsca na dysku i zarządzania zawartością w woluminach oraz ich monitorowania pomyślne wykonanie kopii zapasowych woluminów dysków i pamięci masowej migawki. Pomyślne wykonanie replikacji danych do lokacji odzyskiwania po awarii jest jednak odpowiedzialność firmy Microsoft.

**Aplikacje:** SAP wystąpienia aplikacji lub w przypadku aplikacji z systemem innym niż SAP, warstwy aplikacji tych aplikacji. Twoje obowiązki obejmują wdrożenia, administracji, operacje i monitorowania tych aplikacji dotyczące planowania wydajności użycia zasobów procesora CPU, zużycie pamięci, użycia magazynu Azure i zużycie przepustowości sieci w ramach sieci wirtualnych platformy Azure i z sieciami wirtualnymi platformy Azure do SAP HANA na platformie Azure (wystąpienia duże).

**WAN:** połączenia zostanie nawiązana lokalnych do wdrożeń platformy Azure dla obciążeń. Naszych klientów z wystąpieniami dużych HANA Użyj Azure ExpressRoute dla łączności. To połączenie nie jest częścią SAP HANA na rozwiązanie Azure (wystąpienia duże), dlatego jest odpowiedzialny za konfigurację tego połączenia.

**Archiwum:** łączyli się do archiwizacji kopii danych przy użyciu własnych metod na kontach magazynu. Archiwizowanie wymaga zarządzania, zgodności kosztów i operacji. Jest odpowiedzialny za Generowanie kopie archiwum i kopii zapasowych na platformie Azure i przechowywania ich w sposób zgodny.

Zobacz [umowy SLA dla SAP HANA na platformie Azure (wystąpienia duże)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Zmiana rozmiaru

Zmiany rozmiaru dla wystąpień dużych HANA nie różni się od rozmiaru dla HANA ogólnie. Dla istniejących i wdrażane systemy, chcesz przenieść z innych RDBMS HANA, SAP udostępnia wiele raportów, które Uruchom istniejących systemach SAP. Jeśli baza danych jest przenoszona do HANA, te raporty sprawdzenie, czy dane i obliczyć wymagania dotyczące pamięci dla wystąpienia HANA. Przeczytaj poniższe uwagi SAP, aby uzyskać więcej informacji na temat sposobu uruchamiania te raporty i jak uzyskać ich najnowsze poprawki/wersje:

- [Uwaga SAP #1793345 - zmiany rozmiaru pakietu SAP na HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [Uwaga &#1872170; - pakiet w raporcie zmiany rozmiaru HANA i S/4 HANA SAP](https://launchpad.support.sap.com/#/notes/1872170)
- [Uwaga SAP #2121330 — często zadawane pytania: Programu SAP BW na HANA zmiany rozmiaru raportu](https://launchpad.support.sap.com/#/notes/2121330)
- [Uwaga &#1736976; - raport rozmiaru dla BW na HANA SAP](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP Uwaga #2296290 — nowy raport rozmiaru dla BW na HANA](https://launchpad.support.sap.com/#/notes/2296290)

Implementacje zielony pola SAP szybkie Rozmiar symboli jest dostępna obliczyć wymagania dotyczące pamięci implementacji oprogramowania SAP na górze HANA.

Wymagania dotyczące pamięci dla HANA rosną wraz z rozwojem ilość danych, tak aby mieć świadomość teraz zużycie pamięci i można do prognozowania, co ma to być w przyszłości. Oparte na wymagania dotyczące pamięci, można wówczas zmapować Twoje żądanie do jednej jednostki SKU HANA dużych wystąpienia.

## <a name="requirements"></a>Wymagania

Ta lista składana wymagania dotyczące systemu SAP HANA na platformie Azure (wystąpienia większy).

**Microsoft Azure:**

- Subskrypcji platformy Azure, który może odnosić się do SAP HANA na platformie Azure (wystąpienia duże).
- Microsoft Premier umowę dotyczącą pomocy technicznej. Zobacz [&#2015553; SAP Obsługa Uwaga — SAP w systemie Microsoft Azure: wymagania wstępne dotyczące pomocy technicznej](https://launchpad.support.sap.com/#/notes/2015553) szczegółowe informacje związane z uruchamianiem SAP na platformie Azure. Używanie HANA dużych wystąpienia jednostki z 384 i większej liczby procesorów CPU, musisz umowy pomocy technicznej Premium, aby uwzględnić Azure szybkie odpowiedzi (ARR).
- Rozpoznawanie wystąpień dużych HANA jednostki SKU użytkownik musi po wykonaniu zmiany rozmiaru korzystają z programu SAP.

**Połączenie sieciowe:**

- Usługa Azure ExpressRoute między lokalnymi Azure: nawiązać połączenia z lokalnego centrum danych Azure, upewnij się, że kolejność co najmniej 1 GB/s połączenia od usługodawcy internetowego. 

**System operacyjny:**

- Licencji w systemie SUSE Linux Enterprise Server 12 SAP aplikacji.

> [!NOTE] 
> System operacyjny dostarczane przez firmę Microsoft nie jest zarejestrowany w systemie SUSE, ani nie jest połączony z wystąpieniem SMT.

- SUSE Linux subskrypcji zarządzania narzędzia (SMT) wdrożona na platformie Azure na maszynie Wirtualnej platformy Azure. Zapewnia możliwość SAP HANA na platformie Azure (wystąpienia duże) były rejestrowane i odpowiednio aktualizowane przez SUSE (ponieważ nie ma żadnych dostęp do Internetu w obrębie centrum danych wystąpienia dużych HANA). 
- Licencje Red Hat Enterprise Linux 6.7 lub 7.2 dla SAP HANA.

> [!NOTE]
> System operacyjny dostarczane przez firmę Microsoft nie jest zarejestrowany w Red Hat ani nie jest ona dołączona do wystąpienia Red Hat subskrypcji Menedżera.

- Red Hat subskrypcji Menedżera wdrożona na platformie Azure na maszynie Wirtualnej platformy Azure. Red Hat subskrypcji Menedżera pozwala uzyskać SAP HANA na platformie Azure (wystąpienia duże) były rejestrowane i odpowiednio aktualizowane przez Red Hat (ponieważ nie ma żadnych bezpośredni dostęp do Internetu z w ramach dzierżawy wdrożone w sygnaturze wystąpienia dużych Azure).
- SAP wymaga obsługi kontraktu u swojego dostawcy systemu Linux. To wymaganie jest zachowywana przy użyciu rozwiązania HANA wystąpień dużych lub fakt który Twojej uruchamiania systemu Linux na platformie Azure. W przeciwieństwie do niektórych obrazów galerii Linux Azure, opłaty za usługę nie wchodzi w oferta rozwiązania HANA dużych wystąpień. Jest on możesz klientów do spełnienia wymagań SAP dotyczących umów pomocy technicznej z dystrybutorem systemu Linux.   
   - W systemie SUSE Linux, wyszukiwanie wymagania umowy pomocy technicznej w [SAP Uwaga #1984787 - SUSE LINUX Enterprise Server 12: informacje o instalacji](https://launchpad.support.sap.com/#/notes/1984787) i [&#1056161; Uwaga SAP - SUSE priorytet obsługę aplikacji programu SAP](https://launchpad.support.sap.com/#/notes/1056161).
   - Red Hat Linux musisz mieć poziomy poprawną subskrypcję, które obsługują i usługi (aktualizacje systemów operacyjnych HANA dużych wystąpień. Red Hat zaleca się wprowadzenie "RHEL dla [rozwiązania SAP](https://access.redhat.com/solutions/3082481)" subskrypcji. 

Macierz obsługi różnych wersji SAP HANA z różnymi wersjami systemu Linux, można sprawdzić [SAP Uwaga #2235581](https://launchpad.support.sap.com/#/notes/2235581).


**Baza danych:**

- Licencje i składników instalacji oprogramowania SAP HANA (platforma lub enterprise edition).

**Aplikacje:**

- Licencje i składniki instalacji oprogramowania aplikacje SAP, połączenie SAP HANA i powiązane SAP umowy dotyczące pomocy technicznej.
- Licencje i składniki instalacji oprogramowania dla aplikacji z systemem innym niż SAP używana w celu SAP HANA w środowisku Azure (wystąpienia duże) i powiązanych umów pomocy technicznej.

**Umiejętności:**

- Środowisko i wiedzy na IaaS platformy Azure i jej składniki.
- Środowisko i wiedzy na temat wdrażania SAP obciążenia na platformie Azure.
- SAP HANA instalacji certyfikowane osobistych.
- SAP architektów umiejętności w kontekście projektu wysokiej dostępności i odzyskiwania po awarii wokół SAP HANA.

**SAP:**

- Oczekuje się, klienci SAP i obsługuje kontraktu o SAP
- Szczególnie w przypadku implementacji na klasę II typu jednostki SKU HANA dużych wystąpienia zaleca należy skontaktować się z programu SAP w wersjach programu SAP HANA i konfiguracje ostatecznego na dużych rozmiarze sprzętu skalowanie w pionie.


## <a name="storage"></a>Magazyn

Układ magazynu dla SAP HANA na platformie Azure (wystąpienia duże) jest konfigurowana przy SAP HANA na zarządzania usługą Azure za pośrednictwem SAP zalecane wytyczne udokumentowane w [SAP HANA przestrzeni dyskowej jest potrzebne](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) oficjalny dokument.

HANA dużych wystąpienia typu I klasy mają cztery razy wielkość pamięci jako wolumin magazynu. Dla typu klasy II HANA wystąpienia dużych jednostek magazynu nie ma być cztery razy więcej. Jednostki są dostarczane z woluminu, które są przeznaczone do przechowywania kopii zapasowej dziennika transakcji HANA. Znajdź więcej szczegółów w [jak zainstalować i skonfigurować SAP HANA (duże wystąpień) w systemie Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Zobacz poniższą tabelę pod względem Alokacja magazynu. W tabeli wymieniono około pojemności dla różnych woluminach wyposażone w różnych jednostkach HANA dużych wystąpienia.

| HANA dużych wystąpienia jednostki SKU | Hana/danych | Hana/dziennika | Hana/udostępnionych | Hana / / kopii zapasowej dziennika |
| --- | --- | --- | --- | --- |
| S72 | 1280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3328 GB | 768 GB |1280 GB | 768 GB |
| S192 | 4608 GB | 1024 GB | 1536 GB | 1024 GB |
| S192m | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384 | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384m | 12 000 GB | 2050 GB | 2050 GB | 2040 GB |
| S384xm | 16 000 GB | 2050 GB | 2050 GB | 2040 GB |
| S576 | 20 000 GB | 3100 GB | 2050 GB | 3100 GB |
| S768 | UŻYWANE 28 000 GB | 3100 GB | 2050 GB | 3100 GB |
| S960 | 36,000 GB | 4100 GB | 2050 GB | 4100 GB |


Rzeczywiste woluminów wdrożonej zależą nieco wdrożenia i narzędzie, które służy do wyświetlenia rozmiary woluminów.

Jeśli podzielić SKU wystąpienia z dużych HANA, kilka przykładów części dzielenia możliwe będzie wyglądała:

| Pamięć partycji w GB | Hana/danych | Hana/dziennika | Hana/udostępnionych | Hana / / kopii zapasowej dziennika |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1280 GB | 512 GB | 768 GB | 512 GB |
| 1024 | 1792 GB | 640 GB | 1024 GB | 640 GB |
| 1536 | 3328 GB | 768 GB | 1280 GB | 768 GB |


Rozmiary są liczbami nierównej woluminów, które może różnić się nieznacznie na podstawie wdrożenia i narzędzi służących do przyjrzeć się woluminów. Są także innych rozmiarów partycji thinkable, takich jak 2,5 TB. Rozmiary magazynu będzie obliczana formułą podobne jak używane dla partycji powyżej. Termin partycji nie wskazania, że system operacyjny, pamięć lub zasoby procesora CPU w żadnym sposób podzielona na partycje. Wskazuje on po prostu partycji magazynu dla różnych wystąpień HANA, które można wdrożyć w jednej jednostce HANA dużych wystąpienia. 

Jako klient może być wymagane dla miejsca do magazynowania, możesz mieć możliwość dodać magazyn do zakupu dodatkowego magazynu w jednostkach 1 TB. Ten dodatkowy magazyn może być dodany jako dodatkowe woluminu lub można rozszerzyć co najmniej jeden z istniejących woluminów. Nie jest możliwe zmniejszenie rozmiarów woluminów pierwotnie wdrożone, a przede wszystkim opisane w powyższej tabeli. Ponadto nie jest można zmienić nazwy woluminów lub instalacji nazwy. Woluminy magazynu, zgodnie z powyższym opisem są dołączone do jednostki HANA dużych wystąpienia jako woluminy NFS4.

Klientów można przy użyciu migawek magazynu na potrzeby odzyskiwania kopii zapasowej/przywracania i odzyskiwaniem po awarii. Więcej szczegółów wyszczególnione w [SAP HANA (duże wystąpień) wysokiej dostępności i odzyskiwania po awarii na platformie Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>Szyfrowanie nieużywanych danych
Magazyn używany dla wystąpień dużych HANA umożliwia przezroczystego szyfrowania danych, jak są przechowywane na dyskach. W czasie wdrażania HANA dużych wystąpienia jednostki istnieje możliwość tego typu jest włączone szyfrowanie. Można również zmienić na zaszyfrowanych woluminach po wdrożeniu już. Przenoszenie z niezaszyfrowane do woluminów zaszyfrowanych jest niewidoczny i nie wymaga przestojów. 

O typie I klas jednostek SKU, woluminu rozruchowego, jednostka LUN jest przechowywana na, są szyfrowane. W przypadku typu klasy II jednostki SKU z HANA dużych wystąpień należy zaszyfrować rozruchu jednostki LUN z metodami systemu operacyjnego. Aby uzyskać więcej informacji skontaktuj się z zespołu zarządzania usługami firmy Microsoft.


## <a name="networking"></a>Networking

Architektura sieci Azure jest kluczowym składnikiem pomyślne wdrożenie aplikacji SAP w wystąpieniach dużych HANA. Zazwyczaj SAP HANA we wdrożeniach Azure (wystąpienia duże) mają większy pozioma SAP z kilku różnych rozwiązań SAP o różnych rozmiarach baz danych, użycie zasobów procesora CPU i użycie pamięci. Prawdopodobnie nie wszystkie tych systemów SAP są oparte na SAP HANA, więc prawdopodobnie hybrydowych, który używa programu SAP orientacji poziomej:

- Wdrażane SAP systemów lokalnych. Z powodu ich rozmiary tych systemów aktualnie nie może być umieszczona w środowisku Azure. Klasycznym przykładem może być systemu SAP ERP produkcji w systemie Microsoft SQL Server (co baza danych), która wymaga więcej albo zasoby pamięci, które zapewniają maszynach wirtualnych platformy Azure.
- Wdrożona na podstawie SAP HANA SAP systemów lokalnych.
- Wdrożonych systemów SAP w maszynach wirtualnych platformy Azure. Te systemy można programowanie i testowanie piaskownicy, lub produkcji wystąpień na podstawie SAP NetWeaver aplikacji, które można pomyślnie wdrożyć na platformie Azure (na maszynach wirtualnych), na podstawie zapotrzebowania zasobów konsumenckich i pamięci. Te systemy również mogą być oparte na baz danych, takich jak SQL Server (zobacz [&#1928533; SAP Obsługa Uwaga — aplikacje SAP na platformie Azure: typy obsługiwanych produktów i maszyny Wirtualnej Azure](https://launchpad.support.sap.com/#/notes/1928533/E)) lub SAP HANA (zobacz [SAP HANA certyfikowane IaaS platformy](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)).
- Wdrożenia serwerów aplikacji SAP na platformie Azure (na maszynach wirtualnych), korzystających z SAP HANA na platformie Azure (wystąpienia duże) w sygnatury dużych wystąpienia platformy Azure.

Hybrydowego SAP w orientacji poziomej (co najmniej cztery różne scenariusze wdrażania) jest typowa, istnieje wiele przypadków klienta pełną pozioma SAP działające na platformie Azure. Jak maszyny wirtualne Microsoft Azure stają się coraz bardziej zaawansowanych, jest zwiększenie liczby klientów przenoszenia ich rozwiązania SAP na platformie Azure.

Azure networking w kontekście systemu SAP wdrożona na platformie Azure nie jest skomplikowane. Jest on oparty na następujące zasady:

- Sieci wirtualnych platformy Azure (sieci wirtualne) muszą być podłączone do obwodu Azure ExpressRoute, które łączy sieć lokalną.
- Nawiązywanie połączenia z lokalnym Azure zazwyczaj obwodu usługi expressroute powinien mieć przepustowości 1 GB/s lub wyższy. Ta minimalnej przepustowości umożliwia odpowiedniej przepustowości do przesyłania danych między lokalnymi i systemami uruchomionych na maszynach wirtualnych platformy Azure (a także połączenie systemów Azure z lokalnym użytkownicy końcowi).
- Wszystkie systemy SAP na platformie Azure należy skonfigurować w sieci wirtualnych platformy Azure do komunikowania się ze sobą.
- Usługi Active Directory i DNS obsługiwanego lokalnie zostały rozszerzone na platformie Azure za pośrednictwem usługi ExpressRoute z lokalnymi.


> [!NOTE] 
> Z rozliczeń punktu widzenia tylko jedna subskrypcja platformy Azure może odnosić się tylko do jednego pojedynczej dzierżawy w dużych wystąpienia sygnatury w określonym regionie Azure, a z drugiej strony pojedynczej dzierżawy sygnatury dużych wystąpienia można połączyć tylko z jedną subskrypcją platformy Azure. Ten fakt nie różni się do innych obiektów rozliczeniowy na platformie Azure

Wdrażanie SAP HANA na platformie Azure (duże wystąpień) w wielu różnych regionach platformy Azure, wynikiem oddzielne dzierżawy do wdrożenia w dużych wystąpienia sygnatury. Jednak można uruchomić jednocześnie w ramach tej samej subskrypcji platformy Azure tak długo, jak te wystąpienia są częścią tej samej pozioma SAP. 

> [!IMPORTANT] 
> Tylko wdrożenie zarządzania zasobami Azure jest obsługiwana z SAP HANA na platformie Azure (wystąpienia duże).

 

### <a name="additional-azure-vnet-information"></a>Dodatkowe informacje o sieci wirtualnej Azure

Aby połączyć sieć wirtualną platformy Azure usługi expressroute, można utworzyć bramy usługi Azure (zobacz [temat bram sieci wirtualnej dla usługi ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Brama usługi Azure może służyć z ExpressRoute do infrastruktury poza platformą Azure (lub Azure dużych sygnatura wystąpienia) lub nawiązywania połączenia między sieciami wirtualnymi platformy Azure (zobacz [skonfigurować połączenia do wirtualnymi dla Menedżera zasobów przy użyciu programu PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Azure bramy do maksymalnie czterech różnych połączeń ExpressRoute można łączyć tych połączeń pochodzą z różnych routerów krawędzi przedsiębiorstwa MS (MSEE).  Aby uzyskać więcej informacji, zobacz [SAP HANA (duże wystąpień) infrastruktury i łączności na platformie Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> Przepływność bramy Azure udostępnia jest inna oba przypadki użycia (zobacz [o bramy sieci VPN](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Maksymalna przepustowość, które możemy się z bramą sieci wirtualnej jest 10 GB/s, przy użyciu połączenia ExpressRoute. Należy pamiętać, że kopiowania plików między maszyny Wirtualnej platformy Azure, znajdującej się w sieci wirtualnej platformy Azure oraz systemu lokalnego (jako strumień o pojedynczej kopii) nie będzie pełnej przepływności różne jednostki SKU bramy. Wykorzystanie przepustowości pełną bramy sieci wirtualnej, musi użyć wielu strumieni lub skopiuj różne pliki w strumieniach równoległych pojedynczego pliku.


### <a name="networking-architecture-for-hana-large-instances"></a>Architektura sieci dla wystąpień dużych HANA
Architektura sieci dla wystąpień dużych HANA jak pokazano poniżej, można podzielić na cztery różne części:

- Sieci lokalnej i połączenia ExpressRoute na platformie Azure. Ta część jest domeny klientów i połączone z platformą Azure za pośrednictwem usługi ExpressRoute. Jest to część w prawej dolnej części grafiki poniżej.
- Jak krótko opisanych wyżej z sieciami wirtualnymi platformy Azure, która ponownie ma bram sieci platformy Azure. Jest to obszar, w których należy znaleźć odpowiednie projekty do wymagań aplikacji, zabezpieczeń i zgodności z przepisami. Za pomocą wystąpień dużych HANA jest inny punkt brany pod uwagę pod względem liczby bramy sieci wirtualnych i Azure jednostki SKU do wyboru. Jest to część w prawym górnym rogu grafiki.
- Łączność wystąpień dużych HANA za pomocą technologii ExpressRoute na platformie Azure. Ta część jest wdrożony i obsługiwane przez firmę Microsoft. Wszystko co należy zrobić, ponieważ klient jest zapewnienie niektórych IP zakresów adresów i po wdrożeniu zasobów w HANA wystąpień dużych połączenie ExpressRoute obwód Azure VNet(s) (zobacz [SAP HANA (duże wystąpień) infrastruktury i łączności na platformie Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 
- Sieć w dużych wystąpień HANA, który jest przeważnie przezroczysty dla Ciebie jako klient.

![Połączony SAP HANA Azure (wystąpienia duże) i lokalnej sieci wirtualnej platformy Azure](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Fakt używanie dużych HANA wystąpień nie powoduje zmiany wymagań można pobrać zasobów lokalnych połączone za pośrednictwem usługi Azure. Ponadto nie zmienia wymaganie o sieci z jednego lub wielu wirtualnych uruchamiania maszyn wirtualnych platformy Azure, który host warstwa aplikacji, która łączy się wystąpień HANA hostowanej w HANA dużych wystąpienia jednostki. 

Różnica do wdrożenia SAP w czystym Azure jest dostarczany z faktów który:

- Jednostki wystąpienia dużych HANA dzierżawy klienta są połączone za pośrednictwem innego obwodu usługi expressroute w VNet(s) Twojego Azure. Aby rozdzielić od siebie warunkach obciążenia, lokalnych łączy Azure ExpressRoute sieci wirtualnych i połączeń między sieciami wirtualnymi platformy Azure i wystąpień HANA dużych nie współużytkują tego samego routery.
- Profilu obciążenia między warstwy aplikacji SAP i wystąpieniem HANA jest inny rodzaj wiele żądań małe i serii, takich jak dane przesyła (zestawy wyników) z SAP HANA do warstwy aplikacji.
- Architektura SAP jest bardziej podatna na opóźnienia sieci niż typowy scenariusz, w którym dane pobiera wymieniane między lokalną i platformą Azure.
- Brama sieci wirtualnej ma co najmniej dwa połączenia ExpressRoute, a oba połączenia udziału maksymalna przepustowość dla danych przychodzących z bram sieci wirtualnej.

Doświadczony między maszynami wirtualnymi Azure i HANA dużych jednostek wystąpienia opóźnienia sieci może być większa niż typowe opóźnienia obustronne sieci maszyn wirtualnych do maszyny Wirtualnej. Zależne od regionu Azure, wartości mierzone może przekroczyć opóźnienia obustronne 0,7 ms sklasyfikowane jako poniżej średniej w [SAP Uwaga #1100926 — często zadawane pytania: wydajność sieci](https://launchpad.support.sap.com/#/notes/1100926/E). Niemniej jednak klienci wdrożone aplikacje SAP na podstawie SAP HANA produkcji bardzo pomyślnie na wystąpień dużych SAP HANA. Klienci, którzy wdrożone zgłosił ulepszenia dużą uruchamiając swoje aplikacje SAP na SAP HANA przy użyciu HANA dużych wystąpienia jednostki. Niemniej jednak należy przetestować procesy biznesowe dokładnie w dużych wystąpień HANA Azure.
 
Zapewnić opóźnienia deterministyczne sieci między maszynami wirtualnymi Azure i wystąpienia dużych HANA wybór jednostka SKU bramy sieci wirtualnej platformy Azure jest ważne. W odróżnieniu od wzorce ruchu między lokalnymi i maszyn wirtualnych platformy Azure wzorzec ruchu między maszynami wirtualnymi Azure i wystąpień dużych HANA można programować seria małych jednak dużych woluminów żądania i danych przekazywanych. Aby uzyskać takie seria również obsługiwane, zdecydowanie zaleca się użycie UltraPerformance jednostka SKU bramy. Dla klasy typu II jednostki SKU HANA dużych wystąpienia użycie bramy UltraPerformance SKU jako brama sieci wirtualnej platformy Azure jest obowiązkowe.  

> [!IMPORTANT] 
> Podana ogólną ruch sieciowy między aplikacją SAP i warstwy bazy danych, tylko wysokowydajną lub jednostki SKU bramy UltraPerformance dla sieci wirtualnych jest obsługiwana w celu nawiązania SAP HANA na platformie Azure (wystąpienia duże). Dla dużych HANA wystąpienia jednostki SKU II typu tylko UltraPerformance jednostka SKU bramy jest obsługiwany jako brama sieci wirtualnej platformy Azure.



### <a name="single-sap-system"></a>Pojedynczego systemu SAP

Infrastruktury lokalnej pokazanym powyżej jest połączony za pośrednictwem usługi na platformie Azure i obwodem usługi ExpressRoute łączy do firmy Microsoft Enterprise krawędzi routera (MSEE) (zobacz [opis techniczny ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Po ustanowione, tej trasy łączy do sieci szkieletowej Microsoft Azure, a wszystkie regiony platformy Azure są dostępne.

> [!NOTE] 
> Krajobrazy SAP działające na platformie Azure, można znaleźć najbliższy region platformy Azure w orientacji poziomej SAP MSEE. Sygnatury dużych wystąpienia platformy Azure są połączone za pośrednictwem dedykowanych urządzeń MSEE, aby zminimalizować opóźnienie sieci między maszynami wirtualnymi Azure w Azure IaaS oraz wystąpienia dużych sygnatury.

Brama sieci wirtualnej dla maszyn wirtualnych platformy Azure, hosting wystąpień aplikacji SAP, podłączoną do tego obwodu ExpressRoute i tej samej sieci wirtualnej jest połączona z oddzielnych Router MSEE przeznaczona do nawiązywania połączenia z dużych wystąpienia sygnatury.

To jest przykład prostego pojedynczego systemu SAP, gdzie SAP warstwy aplikacji jest hostowana na platformie Azure i bazy danych SAP HANA odbywa się na SAP HANA na platformie Azure (wystąpienia duże). Zakłada się, przepustowości bramy sieci wirtualnej 2 GB/s lub 10 GB/s przepustowości nie stanowi wąskiego gardła.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Wiele systemów SAP lub dużych systemów SAP

W przypadku wielu systemów SAP lub dużych systemach SAP wdrożenia nawiązywania SAP HANA na Azure (wystąpienia duże) &#39; s, można założyć przepływność bramy sieci wirtualnej mogą stać się wąskiego gardła. W takim przypadku należy podzielić warstwy aplikacji na wiele sieci wirtualnych platformy Azure. Ponadto może to być recommendable do tworzenia specjalnych sieci wirtualnych, które łączenia z wystąpieniami dużych HANA w przypadkach, takich jak:

- Wykonywanie kopii zapasowych bezpośrednio z wystąpień HANA w dużych wystąpień HANA do maszyny Wirtualnej na platformie Azure, który jest hostem udziałów NFS
- Kopiowanie dużych kopii zapasowych lub innych plików z HANA wystąpienia dużych jednostek zarządzanych na platformie Azure miejsca na dysku.

Używa oddzielnych sieci wirtualnych, że wpływ dużych plików pozwala uniknąć hosta maszyn wirtualnych, które zarządzają magazynu lub transfer danych z dużych wystąpień HANA na platformie Azure w bramie sieci wirtualnej pełniącą maszyny wirtualne z systemami SAP warstwy aplikacji. 

Aby uzyskać większą skalowalność architektury sieci:

- Korzystaj z wielu sieci wirtualnych platformy Azure pojedynczego, większy warstwy aplikacji SAP.
- Wdrażanie jednego oddzielne sieć wirtualną Azure dla każdego systemu SAP wdrożone, w porównaniu do łączenia tych systemów SAP w różnych podsieciach w tej samej sieci wirtualnej.

 Skalowalność architektury sieci SAP HANA na platformie Azure (wystąpienia duże):

![Wdrażanie programu SAP warstwy aplikacji przez wiele sieci wirtualnych platformy Azure](./media/hana-overview-architecture/image4-networking-architecture.png)

Wdrażanie programu SAP warstwy aplikacji lub składników, przez wiele sieci wirtualnych platformy Azure zgodnie z powyższym, wprowadzono koszty nieuniknione opóźnienia, który wystąpił podczas komunikacji między hostowanych w tych sieci wirtualnych platformy Azure. Domyślnie ruch sieciowy między maszynami wirtualnymi Azure znajduje się w różnych trasy sieci wirtualnych za pośrednictwem routerów MSEE w tej konfiguracji. Jednak od września 2016 marszruty mogą być optymalizowane. Sposobem optymalizacji i zmniejszyć opóźnienia w łączności między dwiema sieciami wirtualnymi jest komunikacji równorzędnej sieci wirtualnych platformy Azure w ramach tego samego regionu. Nawet jeśli te sieci wirtualne są w różnych subskrypcji. Przy użyciu sieci wirtualnej Azure komunikacji równorzędnej, komunikacja między maszynami wirtualnymi w dwóch różnych sieci wirtualnych platformy Azure umożliwia szkieletu sieci platformy Azure bezpośrednio komunikować się ze sobą. Dowodząc podobne opóźnienie tak, jakby byłoby maszyn wirtualnych w tej samej sieci wirtualnej. Natomiast ruch, adresowania zakresów adresów IP, które są połączone za pośrednictwem bramy sieci wirtualnej platformy Azure jest kierowany przez oddzielną bramę sieci wirtualnej w sieci wirtualnej. Komunikacja równorzędna w artykule można uzyskać szczegółowe informacje o sieci wirtualnej Azure [sieci wirtualnej komunikacji równorzędnej](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Routing na platformie Azure

Istnieją trzy ważne routingu zagadnienia dotyczące sieci dla SAP HANA na platformie Azure (wystąpienia duże):

1. SAP HANA na platformie Azure (wystąpienia duże) można uzyskać tylko za pośrednictwem maszynach wirtualnych platformy Azure oraz dedykowane połączenie ExpressRoute. nie bezpośrednio z lokalnego. Bezpośredni dostęp ze środowiska lokalnego do jednostki HANA dużych wystąpienia w dostarczonym przez firmę Microsoft, nie jest możliwe natychmiast z powodu ograniczenia routingu przechodnie bieżącej architektury sieci platformy Azure używana dla wystąpień dużych SAP HANA. Niektórzy klienci administracji i wszystkich aplikacji wymagających bezpośredniego dostępu, takie jak SAP rozwiązania Menedżera uruchomione w siedzibie firmy, nie może połączyć się bazy danych SAP HANA.

2. Jeśli masz jednostki wystąpienia dużych HANA wdrożone w dwóch różnych regionach platformy Azure na potrzeby odzyskiwania po awarii tego samego przejściowej routingu ograniczenia są stosowane. Lub innymi słowy, adresy IP HANA dużych wystąpienia jednostki w jednym regionie (np. nam zachód), nie będą kierowane do jednostki wystąpienia dużych HANA wdrożone dla Ciebie w innym regionie (np. nam wschodnie). To jest niezależny od użycia sieci platformy Azure komunikację równorzędną w regionach lub między łączenie obwody usługi ExpressRoute, który HANA dużych wystąpienia jednostki nawiązać połączenia z sieciami wirtualnymi platformy Azure. Jak pokazano nieco więcej w dół w niniejszej dokumentacji. To ograniczenie, dołączoną wdrożonej architektury, spowoduje odrzucenie natychmiastowe użycie HANA replikacji systemu jako funkcji odzyskiwania po awarii.

3. SAP HANA Azure (duże wystąpień) w jednostkach mieć przypisanego adresu IP z puli adresów IP serwera adresu zakresu można jako klienta przesłane (zobacz [SAP HANA (duże wystąpień) infrastruktury i łączności na platformie Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) szczegółowe informacje).  Ten adres IP jest dostępny za pośrednictwem subskrypcji platformy Azure i usługi ExpressRoute, łączący sieci wirtualnych Azure HANA na platformie Azure (wystąpienia duże). Adres IP przypisany poza puli IP tego serwera zakresu adresów przypisanej bezpośrednio do jednostki sprzętu i nie jest NAT'ed już to zostało w przypadku pierwszego wdrożenia tego rozwiązania. 

> [!NOTE] 
> Jeśli chcesz rozwiązać ograniczeń w przejściowych routingu, zgodnie z objaśnieniem w pierwszym powyższych elementów dwie listy, należy użyć dodatkowych składników dla routingu. Składniki, które pozwala pokonać ograniczenia mogą być: serwer proxy wstecznego na przesyłanie danych do i z. Na przykład F5 BIG-IP, NGINX Menedżera ruchu wdrożona na platformie Azure jako rozwiązanie routingu wirtualne zapory/ruchu.
> Przy użyciu [reguły IPTables](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) w maszyny Wirtualnej systemu Linux, aby włączyć routing między lokalizacje lokalnymi i HANA dużych wystąpienia jednostki lub wystąpienie dużych HANA jednostki w różnych regionach.
> Należy pamiętać, że wdrożenia i obsługę niestandardowych rozwiązań dotyczących urządzeń sieciowych innych firm lub IPTables nie jest obsługiwane przez firmę Microsoft. Obsługa musi dostarczanych przez dostawcą używany składnik lub integrator. 

### <a name="internet-connectivity-of-hana-large-instances"></a>Połączenie z Internetem HANA dużych wystąpień
Wystąpienia dużych HANA nie mieć bezpośrednie połączenie z Internetem. Jest to ograniczenie z możliwości, na przykład zarejestrować obrazu systemu operacyjnego bezpośrednio z dostawcą systemu operacyjnego. Dlatego należy współpracować z lokalnego serwera SLES SMT lub RHEL subskrypcji Menedżera

### <a name="data-encryption-between-azure-vms-and-hana-large-instances"></a>Szyfrowanie danych między maszynami wirtualnymi Azure i wystąpień dużych HANA
Danych przesyłanych między wystąpieniami dużych HANA i maszyn wirtualnych platformy Azure nie jest zaszyfrowany. Jednak wyłącznie w celu wymiany między po stronie HANA DBMS i aplikacji na podstawie JDBC/ODBC można włączyć szyfrowanie ruchu. Odwołanie [tej dokumentacji przez SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false)

### <a name="using-hana-large-instance-units-in-multiple-regions"></a>Przy użyciu HANA dużych wystąpienia jednostek w wielu regionach

Konieczne może być inne przyczyny do wdrożenia SAP HANA na platformie Azure (duże wystąpień) w wielu regionach platformy Azure, oprócz odzyskiwania po awarii. Może chcesz uzyskać dostęp do wystąpień dużych HANA z poszczególnych maszyn wirtualnych wdrożonych w różnych sieci wirtualnych w regionach. Ponieważ adresy IP przypisane do różnych jednostek HANA dużych wystąpień nie są przenoszone poza sieci wirtualnych platformy Azure, (które są połączone bezpośrednio za pośrednictwem ich bramy do wystąpień), jest niewielkie zmiany w projekcie sieci wirtualnej z wprowadzonych powyżej: bramy sieci wirtualnej platformy Azure może obsłużyć czterech różnych obwody usługi ExpressRoute z różnych MSEEs i każdej sieci wirtualnej, który jest połączony z jednym z sygnaturami dużych wystąpienia mogą zostać połączone w sygnaturze dużych wystąpienia w innym regionie Azure.

![Sieci wirtualne Azure podłączone do sygnatury wystąpienia dużych Azure w różnych regionach platformy Azure](./media/hana-overview-architecture/image8-multiple-regions.png)

Powyżej ilustracji przedstawiono sposób różnych sieci wirtualnych platformy Azure w obu regionach są podłączone do dwóch różnych obwody usługi ExpressRoute, które są używane do nawiązania połączenia SAP HANA na platformie Azure (wystąpienia duże), w obu regionach platformy Azure. Nowo wprowadzonych połączenia są prostokątne czerwonych linii. W przypadku tych połączeń z sieciami wirtualnymi platformy Azure maszyn wirtualnych uruchomionych w jednej z tych sieci wirtualnych są dostępne różne jednostki wystąpień dużych HANA wdrożone w dwóch regionach. Jak widać w powyższym grafiki, zakłada się, że masz dwa połączenia ExpressRoute z lokalnymi dwóch regionach platformy Azure; zalecana ze względów odzyskiwania po awarii.

> [!IMPORTANT] 
> Użycie wielu obwody usługi ExpressRoute dołączanie ścieżki AS i ustawienia lokalnych BGP preferencji powinien służyć do zapewnienia prawidłowego przekierowania ruchu.


