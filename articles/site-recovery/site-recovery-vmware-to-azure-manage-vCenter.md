---
title: " Zarządzanie programu VMware vCenter server w usłudze Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób dodawania i zarządzania nimi w usłudze Azure Site Recovery program VMware vCenter."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 091f0884417535427c52beee7bcdc5ed1dd83315
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-vmware-vcenter-server-in-azure-site-recovery"></a>Zarządzanie VMware vCenter Server w usłudze Azure Site Recovery
W tym artykule opisano różne operacje odzyskiwania lokacji, które można wykonywać w programie VMware vCenter.

## <a name="prerequisites"></a>Wymagania wstępne

**Obsługa programu VMware vCenter i VMware vSphere hosta ESX** | **Szczegóły** |
|--- | --- |
|**Serwery lokalne, VMware** | Co najmniej jeden VMware vSphere serwery, 6.0, 5.5, 5.1 z uruchomionym najnowsze aktualizacje. Serwery powinny znajdować się w tej samej sieci co serwer konfiguracji (lub oddzielnego serwera przetwarzania).<br/><br/> Firma Microsoft zaleca serwer vCenter do zarządzania hostach z systemem w wersji 6.0 lub 5.5 z najnowszymi aktualizacjami. Tylko funkcje, które są dostępne w wersji 5.5 są obsługiwane w przypadku wdrożenia w wersji 6.0.|

## <a name="prepare-an-account-for-automatic-discovery"></a>Przygotowanie konta automatycznego wykrywania
Odzyskiwanie lokacji musi mieć dostęp do programu VMware, serwera przetwarzania do automatycznego wykrywania maszyn wirtualnych i trybu failover i powrotu po awarii maszyn wirtualnych.

* **Migrowanie**: Jeśli chcesz przeprowadzić migrację maszyn wirtualnych VMware do platformy Azure, niezawodnie kiedykolwiek je ponownie, służy konto VMware z rolą tylko do odczytu. Takie roli można uruchomić trybu failover, ale nie można zamknąć chronionych maszyn źródłowych. Nie jest to konieczne do migracji.
* **Replikacja/odzyskania**: Jeśli chcesz wdrożyć pełnej replikacji (replikacja, trybu failover i powrotu po awarii) konto musi mieć możliwość uruchamiania operacji, takich jak tworzenie i usuwanie dysków, włączanie na maszynie wirtualnej.
* **Automatyczne odnajdowanie**: wymagane jest co najmniej konto tylko do odczytu.


|**Zadania** | **Wymagane konto/roli** | **Uprawnienia** | **Szczegóły**|
|--- | --- | --- | ---|
|**Serwer przetwarzania automatycznie odnajduje maszyn wirtualnych VMware** | Należy co najmniej użytkownika tylko do odczytu | Centrum danych obiektu –> propagowany do obiektu podrzędnego roli = tylko do odczytu | Użytkownik przypisane na poziomie centrum danych i ma dostęp do wszystkich obiektów w centrum danych.<br/><br/> Aby ograniczyć dostęp, Przypisz **dostępu** roli z **propagowany do podrzędnego** obiektu do obiektów podrzędnych (hostami vSphere, datastores maszyn wirtualnych i sieci).|
|**Tryb failover** | Należy co najmniej użytkownika tylko do odczytu | Centrum danych obiektu –> propagowany do obiektu podrzędnego roli = tylko do odczytu | Użytkownik przypisane na poziomie centrum danych i ma dostęp do wszystkich obiektów w centrum danych.<br/><br/> Aby ograniczyć dostęp, Przypisz **dostępu** roli z **propagowany do podrzędnego** obiektu do obiektów podrzędnych (hostami vSphere, datastores maszyn wirtualnych i sieci).<br/><br/> Przydatna do celów migracji, ale nie pełnej replikacji, pracy awaryjnej, powrotu po awarii.|
|**Tryb failover i powrotu po awarii** | Zalecamy, aby utworzyć rolę (AzureSiteRecoveryRole) z wymaganymi uprawnieniami, a następnie przypisać rolę użytkownikowi VMware lub grupy | Centrum danych obiektu –> propagowany do obiektu podrzędnego roli = AzureSiteRecoveryRole<br/><br/> Magazyn danych -> Przydziel przestrzeń na, Przeglądaj magazynu danych, operacje na plikach niskiego poziomu, usuń plik, zaktualizuj pliki maszyny wirtualnej<br/><br/> Sieć -> Przypisywanie sieci<br/><br/> Zasób -> Przypisywanie maszyny Wirtualnej do puli zasobów, migracji Zasilanie wyłączone maszyny Wirtualnej, migracja zasilanego na maszynie Wirtualnej<br/><br/> Zadania -> Utwórz zadanie, zadania aktualizacji<br/><br/> Maszyny wirtualne -> Konfiguracja<br/><br/> Maszyny wirtualne -> interakcja -> odpowiedzi na pytanie, połączenie z urządzeniem, skonfiguruj nośnik CD, skonfiguruj dyskietka, wyłącz zasilanie, włączania zasilania, zainstaluj narzędzia VMware<br/><br/> Maszyny wirtualne -> spisu -> Utwórz, rejestrowanie, wyrejestrowywanie<br/><br/> Maszyny wirtualne -> inicjowania obsługi administracyjnej -> Zezwalaj na pobieranie maszyny wirtualnej, a także zezwalanie przekazać pliki maszyny wirtualnej<br/><br/> Maszyny wirtualne -> migawki -> Usuń migawki | Użytkownik przypisane na poziomie centrum danych i ma dostęp do wszystkich obiektów w centrum danych.<br/><br/> Aby ograniczyć dostęp, Przypisz **dostępu** roli z **propagowany do podrzędnego** obiektu do obiektów podrzędnych (hostami vSphere, datastores maszyn wirtualnych i sieci).|

## <a name="create-an-account-to-connect-to-vmware-vcenter-server-vmware-vsphere-exsi-host"></a>Utwórz konto w celu połączenia się programu VMware vCenter Server / VMware vSphere EXSi hosta
1. Zaloguj się do konfiguracji serwera, a następnie uruchom cspsconfigtool.exe za pomocą skrótu umieszczony na pulpicie.
2. Kliknij przycisk **Dodaj konto** na **Zarządzaj kontem** kartę.

  ![Dodaj konto](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Podaj szczegóły konta, a następnie kliknij przycisk OK, aby dodać konto. Konto musi mieć uprawnienia na liście [przygotować konta do automatycznego odnajdowania](#prepare-an-account-for-automatic-discovery) sekcji.

  >[!NOTE]
  Trwa około 15 minut, aby uzyskać informacje o koncie, które można synchronizować się z usługą Site Recovery.


## <a name="associate-a-vmware-vcenter-vmware-vsphere-esx-host-add-vcenter"></a>Skojarz VMware vCenter / VMware vSphere ESX host (Dodaj vCenter)
* W portalu Azure, przejdź do *YourRecoveryServicesVault* > **infrastruktura usługi Site Recovery** > **serwery konfiguracji** > *ConfigurationServer*
* Na stronie Szczegóły serwera konfiguracji kliknij przycisk vCenter +.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials-used-to-connect-to-the-vcenter-server-vsphere-esxi-host"></a>Modyfikowanie poświadczenia używane do łączenia się z serwerem vCenter / hostem vSphere ESXi

1. Zaloguj się do konfiguracji serwera, a następnie uruchom cspsconfigtool.exe
2. Kliknij przycisk **Dodaj konto** na **Zarządzaj kontem** kartę.

  ![Dodaj konto](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Podaj nowe szczegóły konta i kliknij przycisk OK, aby dodać konto. Konto musi mieć uprawnienia na liście [przygotować konta do automatycznego odnajdowania](#prepare-an-account-for-automatic-discovery) sekcji.
4. W portalu Azure, przejdź do *YourRecoveryServicesVault* > **infrastruktura usługi Site Recovery** > **serwery konfiguracji** > *ConfigurationServer*
5. Na stronie Szczegóły serwera konfiguracji kliknij **odświeżania serwera** przycisku.
6. Po zakończeniu zadania serwer odświeżania, wybierz vCenter Server, aby otworzyć stronę podsumowania vCenter.
7. Wybierz nowo dodane konto w **konta hosta serwera/vSphere vCenter** a następnie kliknij przycisk **zapisać** przycisku.

  ![modyfikowanie konta](./media/site-recovery-vmware-to-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-in-azure-site-recovery"></a>Usuń vCenter w usłudze Azure Site Recovery
1. W portalu Azure, przejdź do *YourRecoveryServicesVault* > **infrastruktura usługi Site Recovery** > **serwery konfiguracji** > *ConfigurationServer*
2. Na stronie Szczegóły serwera konfiguracji wybierz vCenter Server, aby otworzyć stronę podsumowania vCenter.
3. Polecenie **usunąć** przycisk, aby usunąć vCenter

  ![Usuń konto](./media/site-recovery-vmware-to-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Jeśli trzeba zmodyfikować Vcenter adres IP/nazwa FQDN, Szczegóły portu należy usunąć serwera vCenter i dodaj go ponownie ponownie.
