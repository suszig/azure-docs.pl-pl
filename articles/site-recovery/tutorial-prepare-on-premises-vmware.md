---
title: "Przygotowanie serwerów VMware lokalnych do odzyskiwania po awarii maszyn wirtualnych VMware do platformy Azure | Dokumentacja firmy Microsoft"
description: "Poznaj sposoby przygotowania lokalnych serwerów VMware do odzyskiwania po awarii na platformie Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 90a4582c-6436-4a54-a8f8-1fee806b8af7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 33ec5775a371a04074f07d589d35d1c05bd64d30
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>Przygotowywanie serwerów VMware lokalnych do odzyskiwania awaryjnego na platformie Azure

Ten samouczek przedstawia sposób przygotowania infrastruktury lokalnej programu VMware można replikować maszyny wirtualne VMware do platformy Azure. Niniejszy samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Przygotowanie konta na vCenter server lub vSphere ESXi hosta, aby zautomatyzować odnajdywania maszyny Wirtualnej
> * Przygotowanie konta do automatycznej instalacji usługi mobilności na maszynach wirtualnych VMware
> * Przejrzyj wymagania dotyczące serwera VMware
> * Przejrzyj wymagania dotyczące maszyny Wirtualnej VMware

W tym samouczku zostanie przedstawiony sposób wykonywania kopii zapasowej jednej maszyny Wirtualnej za pomocą usługi Azure Site Recovery. Jeśli planujesz chronić wiele maszyn wirtualnych VMware, należy pobrać [narzędzie wdrożenia Planistę](https://aka.ms/asr-deployment-planner) dla replikacji maszyn wirtualnych VMware. To narzędzie można zbierać informacje o zgodności maszyn wirtualnych, dysków dla maszyny Wirtualnej i tworzeniem danych dla każdego dysku. Narzędzie obejmuje również wymagania dotyczące przepustowości sieci i infrastruktury platformy Azure wymagane do pomyślnego replikacji i testowania trybu failover. [Dowiedz się więcej](site-recovery-deployment-planner.md) o uruchomienie narzędzia.

Jest to drugi samouczek z tej serii. Upewnij się, że masz [Konfigurowanie składników Azure](tutorial-prepare-azure.md) zgodnie z opisem w poprzedniej samouczka.

## <a name="prepare-an-account-for-automatic-discovery"></a>Przygotowanie konta automatycznego wykrywania

Odzyskiwanie lokacji musi mieć dostęp do serwerów VMware:

- Automatycznie odnajduje maszyn wirtualnych. Wymagane jest co najmniej konto tylko do odczytu.
- Organizowanie replikacji, trybu failover i powrotu po awarii. Potrzebujesz konta, które można uruchomić operacji, takich jak tworzenie i usuwanie dysków i włączanie na maszynach wirtualnych.

Utwórz konto w następujący sposób:

1. Aby użyć dedykowanego konta, należy utworzyć rolę na poziomie vCenter. Nadaj nazwę roli takich jak **Azure_Site_Recovery**.
2. Przypisz rolę uprawnienia podsumowane w poniższej tabeli.
3. Utwórz użytkownika na hoście serwera lub vSphere vCenter. Przypisać rolę użytkownikowi.

### <a name="vmware-account-permissions"></a>VMware uprawnień konta

**Zadanie podrzędne** | **Uprawnienia roli /** | **Szczegóły**
--- | --- | ---
**Odnajdywanie maszyny Wirtualnej** | Co najmniej użytkownika tylko do odczytu<br/><br/> Centrum danych obiektu –> propagowany do obiektu podrzędnego roli = tylko do odczytu | Użytkownik przypisane na poziomie centrum danych i ma dostęp do wszystkich obiektów w centrum danych.<br/><br/> Aby ograniczyć dostęp, Przypisz **dostępu** roli z **propagowany do podrzędnego** obiektu do obiektów podrzędnych (hostami vSphere, datastores, maszyn wirtualnych i sieci).
**Pełnej replikacji, trybu failover i powrotu po awarii** |  Tworzenie roli (Azure_Site_Recovery) z wymaganymi uprawnieniami, a następnie przypisać rolę użytkownikowi VMware lub grupy<br/><br/> Centrum danych obiektu –> propagowany do obiektu podrzędnego roli = Azure_Site_Recovery<br/><br/> Magazyn danych -> Przydziel przestrzeń na, Przeglądaj magazynu danych, operacje na plikach niskiego poziomu, usuń plik, zaktualizuj pliki maszyny wirtualnej<br/><br/> Sieć -> Przypisywanie sieci<br/><br/> Zasób -> Przypisywanie maszyny Wirtualnej do puli zasobów, migracji Zasilanie wyłączone maszyny Wirtualnej, migracja zasilanego na maszynie Wirtualnej<br/><br/> Zadania -> Utwórz zadanie, zadania aktualizacji<br/><br/> Maszyny wirtualne -> Konfiguracja<br/><br/> Maszyny wirtualne -> interakcja -> odpowiedzi na pytanie, połączenie z urządzeniem, skonfiguruj nośnik CD, skonfiguruj dyskietka, wyłącz zasilanie, włączania zasilania, zainstaluj narzędzia VMware<br/><br/> Maszyny wirtualne -> spisu -> Utwórz, rejestrowanie, wyrejestrowywanie<br/><br/> Maszyny wirtualne -> inicjowania obsługi administracyjnej -> Zezwalaj na pobieranie maszyny wirtualnej, a także zezwalanie przekazać pliki maszyny wirtualnej<br/><br/> Maszyny wirtualne -> migawki -> Usuń migawki | Użytkownik przypisane na poziomie centrum danych i ma dostęp do wszystkich obiektów w centrum danych.<br/><br/> Aby ograniczyć dostęp, Przypisz **dostępu** roli z **propagowany do podrzędnego** obiektu do obiektów podrzędnych (hostami vSphere, datastores, maszyn wirtualnych i sieci).

## <a name="prepare-an-account-for-mobility-service-installation"></a>Przygotowanie do instalacji usługi mobilności konta

Musi być zainstalowana usługa mobilności na maszynie Wirtualnej, którą chcesz replikować. Usługa Site Recovery automatycznie instaluje tej usługi, po włączeniu replikacji dla maszyny Wirtualnej. Automatyczną instalację należy przygotować konta, które uzyskują dostęp maszyny Wirtualnej odzyskiwania lokacji. To konto będzie można określić podczas odzyskiwania po awarii w konsoli platformy Azure.

1. Przygotowanie domeny lub lokalnego konta z uprawnieniami do zainstalowania na maszynie Wirtualnej.
2. Aby zainstalować na maszynach wirtualnych systemu Windows, jeśli nie używasz konta domeny, należy wyłączyć kontroli dostępu użytkownika zdalnego na komputerze lokalnym.
   - Z rejestru w obszarze **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, Dodaj wpis DWORD **LocalAccountTokenFilterPolicy**, o wartości 1.
3. Do zainstalowania na maszynach wirtualnych systemu Linux, należy przygotować konta głównego na serwer źródłowy z systemem Linux.


## <a name="check-vmware-server-requirements"></a>Wymagania serwera VMware

Upewnij się, że serwery VMware spełniać następujące wymagania.

**Składnik** | **Wymaganie**
--- | ---
**Serwer vCenter** | vCenter 6.5, 6.0 lub 5.5
**hostem vSphere** | vSphere 6.5, 6.0, 5.5

## <a name="check-vmware-vm-requirements"></a>Sprawdź wymagania maszyny Wirtualnej VMware

Upewnij się, że maszyna wirtualna spełnia wymagania Azure podsumowane w poniższej tabeli.

**Wymaganie maszyny Wirtualnej** | **Szczegóły**
--- | ---
**Rozmiar dysku systemu operacyjnego** | Do 2048 GB.
**Liczba dysków systemu operacyjnego** | 1
**Liczba dysków danych** | 64 lub mniej
**Rozmiar wirtualnego dysku twardego dysku danych** | Do 4095 GB
**Karty sieciowe** | Wiele kart sieciowych są obsługiwane.
**Udostępniony wirtualny dysk twardy** | Nieobsługiwane
**FC dysku** | Nieobsługiwane
**Format dysku twardego** | Plik VHD lub VHDX.<br/><br/> Chociaż VHDX nie jest obecnie obsługiwany na platformie Azure, Usługa Site Recovery automatycznie konwertuje VHDX do wirtualnego dysku twardego, gdy awaryjnie na platformie Azure. Gdy nie powiedzie się do maszyn wirtualnych lokalnie nadal używać formatu VHDX.
**Funkcja BitLocker** | Nie jest obsługiwane. Wyłącz przed włączeniem replikacji dla maszyny Wirtualnej.
**Nazwa maszyny Wirtualnej** | Od 1 do 63 znaków.<br/><br/> Ograniczone do litery, cyfry i łączniki. Nazwa maszyny Wirtualnej musi zaczynać i kończyć literą lub cyfrą.
**Typu maszyny Wirtualnej** | Generacja 1 - Linux lub Windows<br/><br/>Generacja 2 — tylko w systemie Windows

Maszyna wirtualna musi również działać obsługiwanego systemu operacyjnego. Zobacz [macierz obsługi usługi Site Recovery](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) pełną listę obsługiwanych wersji.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Przygotowanie do połączenia z maszynami wirtualnymi Azure po przejściu do trybu failover

Podczas scenariusza trybu failover możesz nawiązać replikowanych maszyn wirtualnych na platformie Azure z sieci lokalnej.

Aby połączyć się za pomocą protokołu RDP po pracy awaryjnej maszyn wirtualnych systemu Windows, wykonaj następujące czynności:

1. Aby uzyskać dostęp przez internet, należy włączyć protokół RDP na lokalnej maszynie Wirtualnej przed trybu failover. Upewnij się, że TCP i UDP reguły są dodawane do **publicznego** profilu oraz że RDP jest dozwolone w **zapory systemu Windows** > **dozwolone aplikacje** we wszystkich profilach.
2. Aby uzyskać dostęp za pośrednictwem połączenia VPN lokacja lokacja, należy włączyć RDP na maszynie lokalnej. RDP powinno być dozwolone w **zapory systemu Windows** -> **dozwolone aplikacje i funkcje** dla **domeny i prywatnej** sieci.
   Sprawdź, czy zasady sieci SAN systemu operacyjnego są ustawione na **OnlineAll**. [Dowiedz się więcej](https://support.microsoft.com/kb/3031135). Powinien istnieć nie oczekujące aktualizacje systemu Windows na maszynie Wirtualnej, gdy użytkownik zainicjuje tryb failover. Jeśli, nie będzie można logować się do maszyny wirtualnej do momentu ukończenia aktualizacji.
3. Na maszynie Wirtualnej Azure z systemem Windows po w tryb failover, sprawdź **diagnostyki rozruchu** Aby wyświetlić zrzut ekranu maszyny wirtualnej. Jeśli nie możesz połączyć, sprawdź, czy maszyna wirtualna działa i przejrzyj te [porady dotyczące rozwiązywania problemów](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Aby połączyć się przy użyciu protokołu SSH po pracy awaryjnej maszyn wirtualnych systemu Linux, wykonaj następujące czynności:

1. Na maszynie lokalnej przed trybu failover Sprawdź, czy Usługa Secure Shell jest ustawiony na automatyczne uruchomienie przy rozruchu systemu. Sprawdź, czy reguły zapory zezwalają na połączenie SSH.

2. Na maszynie Wirtualnej Azure po pracy awaryjnej zezwalać na połączenia przychodzące do portu SSH dla zasad grupy zabezpieczeń sieci w trybie Failover maszyny Wirtualnej i podsieci platformy Azure, do którego jest podłączony.
   [Dodaj publiczny adres IP](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) dla maszyny Wirtualnej. Możesz sprawdzić **diagnostyki rozruchu** Aby wyświetlić zrzut ekranu maszyny wirtualnej.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Konfigurowanie odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych VMware](tutorial-vmware-to-azure.md)
