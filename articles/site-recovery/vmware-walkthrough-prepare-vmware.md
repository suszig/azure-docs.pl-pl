---
title: "Przygotowanie zasobów VMware lokalnej na platformie Azure za pomocą usługi Azure Site Recovery replikacji | Dokumentacja firmy Microsoft"
description: "Zawiera podsumowanie kroków potrzebnych do replikowania obciążeń uruchomionych na maszynach wirtualnych VMware do magazynu Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 6aba0e89-ad7c-467e-9db2-cfb3bfe4c7d6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 3e1c589030210c2eae1ad9c02811775d9d6365d4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="step-6-prepare-on-premises-vmware-replication-to-azure"></a>Krok 6: Przygotowanie lokalnej replikacji maszyn wirtualnych VMware do platformy Azure

Postępuj zgodnie z instrukcjami w tym artykule, aby przygotować lokalnych serwerów VMware do interakcji z usługą Azure Site Recovery i przygotowania instalacji usługi mobilności maszyny wirtualne VMWare. Musi być zainstalowany agent usługi mobilności na wszystkich maszynach wirtualnych lokalnych, które chcesz replikować do platformy Azure.

## <a name="prepare-for-automatic-discovery"></a>Przygotowanie do automatycznego wykrywania

Usługa Site Recovery automatycznie odnajduje maszyn wirtualnych na hostach ESXi vSphere (z lub bez serwera vCenter). Automatycznego wykrywania usługi Site Recovery wymaga konta dostępu do hostów i serwerów:

1. Aby użyć dedykowanego konta, należy utworzyć rolę (na poziomie vCenter z uprawnieniami opisane w poniższej tabeli. Nadaj mu nazwę, taką jak **Azure_Site_Recovery**.
2. Następnie utwórz użytkownika na serwerze hosta/vCenter vSphere i przypisać rolę użytkownikowi. To konto użytkownika można określić podczas wdrażania usługi Site Recovery.


### <a name="vmware-account-permissions"></a>VMware uprawnień konta

Odzyskiwanie lokacji musi mieć dostęp do programu VMware, serwer przetwarzania na automatyczne odnajdywanie maszyn wirtualnych i trybu failover i powrotu po awarii maszyn wirtualnych.

- **Migrowanie**: Jeśli chcesz przeprowadzić migrację maszyn wirtualnych VMware do platformy Azure, niezawodnie kiedykolwiek je ponownie, służy konto VMware z rolą tylko do odczytu. Takie roli można uruchomić trybu failover, ale nie można zamknąć chronionych maszyn źródłowych. Nie jest to konieczne do migracji.
- **Replikacja/odzyskania**: Jeśli chcesz wdrożyć pełnej replikacji (replikacja, trybu failover i powrotu po awarii) konto musi mieć możliwość uruchamiania operacji, takich jak tworzenie i usuwanie dysków, włączanie na maszynach wirtualnych itd.
- **Automatyczne odnajdowanie**: wymagane jest co najmniej konto tylko do odczytu.


**Zadanie podrzędne** | **Wymagane konto/roli** | **Uprawnienia** | **Szczegóły**
--- | --- | --- | ---
**Serwer przetwarzania automatycznie odnajduje maszyn wirtualnych VMware** | Należy co najmniej użytkownika tylko do odczytu | Centrum danych obiektu –> propagowany do obiektu podrzędnego roli = tylko do odczytu | Użytkownik przypisane na poziomie centrum danych i ma dostęp do wszystkich obiektów w centrum danych.<br/><br/> Aby ograniczyć dostęp, Przypisz **dostępu** roli z **propagowany do podrzędnego** obiektu do obiektów podrzędnych (hostami vSphere, datastores, maszyn wirtualnych i sieci).
**Tryb failover** | Należy co najmniej użytkownika tylko do odczytu | Centrum danych obiektu –> propagowany do obiektu podrzędnego roli = tylko do odczytu | Użytkownik przypisane na poziomie centrum danych i ma dostęp do wszystkich obiektów w centrum danych.<br/><br/> Aby ograniczyć dostęp, Przypisz **dostępu** roli z **propagowany do podrzędnego** obiektu do obiektów podrzędnych (hostami vSphere, datastores, maszyn wirtualnych i sieci).<br/><br/> Przydatna do celów migracji, ale nie pełnej replikacji, pracy awaryjnej, powrotu po awarii.
**Tryb failover i powrotu po awarii** | Zalecamy, aby utworzyć rolę (Azure_Site_Recovery) z wymaganymi uprawnieniami, a następnie przypisać rolę użytkownikowi VMware lub grupy | Centrum danych obiektu –> propagowany do obiektu podrzędnego roli = Azure_Site_Recovery<br/><br/> Magazyn danych -> Przydziel przestrzeń na, Przeglądaj magazynu danych, operacje na plikach niskiego poziomu, usuń plik, zaktualizuj pliki maszyny wirtualnej<br/><br/> Sieć -> Przypisywanie sieci<br/><br/> Zasób -> Przypisywanie maszyny Wirtualnej do puli zasobów, migracji Zasilanie wyłączone maszyny Wirtualnej, migracja zasilanego na maszynie Wirtualnej<br/><br/> Zadania -> Utwórz zadanie, zadania aktualizacji<br/><br/> Maszyny wirtualne -> Konfiguracja<br/><br/> Maszyny wirtualne -> interakcja -> odpowiedzi na pytanie, połączenie z urządzeniem, skonfiguruj nośnik CD, skonfiguruj dyskietka, wyłącz zasilanie, włączania zasilania, zainstaluj narzędzia VMware<br/><br/> Maszyny wirtualne -> spisu -> Utwórz, rejestrowanie, wyrejestrowywanie<br/><br/> Maszyny wirtualne -> inicjowania obsługi administracyjnej -> Zezwalaj na pobieranie maszyny wirtualnej, a także zezwalanie przekazać pliki maszyny wirtualnej<br/><br/> Maszyny wirtualne -> migawki -> Usuń migawki | Użytkownik przypisane na poziomie centrum danych i ma dostęp do wszystkich obiektów w centrum danych.<br/><br/> Aby ograniczyć dostęp, Przypisz **dostępu** roli z **propagowany do podrzędnego** obiektu do obiektów podrzędnych (hostami vSphere, datastores, maszyn wirtualnych i sieci).


## <a name="prepare-for-push-installation-of-the-mobility-service"></a>Przygotowanie do instalacji wypychanej usługi mobilności

Musi być zainstalowana usługa mobilności na wszystkich maszynach wirtualnych, które mają być replikowane. Istnieje wiele sposobów, aby zainstalować usługi, w tym instalacja ręczna, instalacja wypychana z serwera przetwarzania odzyskiwania lokacji i instalacji przy użyciu metod, takich jak System Center Configuration Manager.

Jeśli chcesz użyć instalację wypychaną, należy przygotować konta usługi Site Recovery umożliwia dostęp do maszyny Wirtualnej.

- Korzystając z domeny lub lokalnego konta
- Jeśli nie używasz konta domeny systemu Windows, należy wyłączyć kontroli dostępu użytkownika zdalnego na komputerze lokalnym. Aby to zrobić, w rejestrze, w obszarze **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, Dodaj wpis DWORD **LocalAccountTokenFilterPolicy**, o wartości 1.
- Jeśli chcesz dodać wpis rejestru dla systemu Windows z interfejsu wiersza polecenia, wpisz:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Dla systemu Linux konto powinno być głównym urzędem certyfikacji na serwer źródłowy z systemem Linux.



## <a name="next-steps"></a>Następne kroki

Przejdź do [kroku 7: Tworzenie magazynu](vmware-walkthrough-create-vault.md)
