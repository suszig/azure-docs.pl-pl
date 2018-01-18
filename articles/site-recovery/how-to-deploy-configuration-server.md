---
title: " Wdrażanie serwera konfiguracji na potrzeby odzyskiwania po awarii VMware z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób wdrażania serwera konfiguracji na potrzeby odzyskiwania po awarii VMware z usługą Azure Site Recovery"
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: anoopkv
ms.openlocfilehash: e257ede08ac46ad863b4883b10399058e6f59f1f
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="deploy-a-configuration-server"></a>Wdrożenie serwera konfiguracji

Lokalny serwer konfiguracji jest wdrażany, kiedy należy używać [usługi Azure Site Recovery](site-recovery-overview.md) usługi odzyskiwania po awarii maszyn wirtualnych VMware i serwerów fizycznych do platformy Azure. Serwer konfiguracji WThe koordynuje komunikacji między lokalnymi VMware i na platformie Azure i zarządza replikacji danych. W tym artykule przedstawiono kroki niezbędne do wdrożenia serwera konfiguracji.

## <a name="prerequisites"></a>Wymagania wstępne

Zaleca się wdrożenie serwera konfiguracji jako wysokiej dostępności maszyny Wirtualnej VMware. Replikacja serwerów fizycznych można skonfigurować serwera konfiguracji na komputerze fizycznym. Minimalne wymagania sprzętowe zostały podsumowane w poniższej tabeli.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]




## <a name="capacity-planning"></a>Planowanie pojemności

Zmiany rozmiaru wymagania dotyczące serwera konfiguracji są zależne od potencjalnych częstotliwości zmian danych. Użyj tej tabeli jako przewodnika.

| **CPU** | **Pamięci** | **Rozmiar pamięci podręcznej dysku** | **Częstotliwość zmian danych** | **Chronione maszyny** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * 4 rdzenie @ 2,5 GHz) |16 GB |300 GB |500 GB lub mniej |Replikowanie maszyn mniej niż 100. |
| 12 Vcpu (2 sockets * 6 rdzeni @ 2,5 GHz) |18 GB |600 GB |500 GB do 1 TB |Replikują między 100 150 maszyn. |
| 16 Vcpu (2 sockets * 8 rdzeni @ 2,5 GHz) |32 GB |1 TB |1 TB do 2 TB |Replikują między 150 – 200 maszyn. |


Jeśli replikujesz maszyny wirtualne VMware, przeczytaj więcej na temat [zagadnienia związane z planowaniem pojemności](/site-recovery-plan-capacity-vmware.md)i uruchom [narzędzi planowania wdrożenia](site-recovery-deployment-planner.md) replikacji VMWare.



## <a name="download-the-template"></a>Pobierz szablon

Usługa Site Recovery zapewnia do pobrania szablonu, aby skonfigurować serwer konfiguracji jako wysokiej dostępności maszyny Wirtualnej VMware. 

1. W magazynie, przejdź do **przygotowanie infrastruktury** > **źródła**.
2. W **Przygotuj źródło**, kliknij przycisk **+ serwer konfiguracji**.
3. W **Dodaj serwer**, sprawdź, czy **serwera konfiguracji na potrzeby VMware** pojawia się w **typ serwera**.
4. Pobierz szablon Open Virtualization Format (OVF) dla serwera konfiguracji.

  > [!TIP]
  Można pobrać najnowszej wersji szablonu serwera konfiguracji bezpośrednio z [Microsoft Download Center](https://aka.ms/asrconfigurationserver)


## <a name="import-the-template-in-vmware"></a>Importowanie szablonu w środowisku programu VMware


1. Logowania się do programu VMware vCenter server lub vSphere ESXi hosta, za pomocą VMWare vSphere Client.
2. Na **pliku** menu, wybierz opcję **wdrażanie szablonu OVF**, aby uruchomić Kreatora wdrażania pakietu OVF szablonu.  

     ![Szablon OVF](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. W **wybierz źródło**, określ lokalizację pobranego pakietu OVF.
4. W **Przejrzyj szczegóły**, kliknij przycisk **dalej**.
5. W **wybierz nazwę i folder**, i **konfiguracji wybierz**, zaakceptuj ustawienia domyślne.
6. W **Wybieranie magazynu**, wybrać najlepszą wydajność **grubości udostępniania wczesny mają być wyzerowane** w **formatu dysku wirtualnego wybierz**.
4. W pozostałej części stron kreatora Zaakceptuj ustawienia domyślne.
5. W **gotowa do wykonania**:
  - Aby skonfigurować maszynę Wirtualną przy użyciu ustawień domyślnych, wybierz **włączają po wdrożeniu** > **Zakończ**.
  - Jeśli chcesz dodać dodatkowy interfejs sieciowy, wyczyść **włączają po wdrożeniu**, a następnie wybierz **Zakończ**. Domyślnie szablon serwera konfiguracji jest wdrażany z jednej karty Sieciowej, ale można dodać dodatkowe karty sieciowe, po wdrożeniu.


## <a name="add-an-additional-adapter"></a>Dodaj dodatkowe karty

Jeśli chcesz dodać dodatkowe karty Sieciowej na serwerze konfiguracji, należy to zrobić, aby zarejestrować serwer w magazynie. Dodawanie dodatkowych kart sieciowych nie jest obsługiwana po rejestracji.

1. W vSphere spisu klienta, kliknij prawym przyciskiem myszy maszynę Wirtualną, a następnie wybierz **Edytuj ustawienia**.
2. W **sprzętu**, kliknij przycisk **Dodaj** > **karty Ethernet**. Następnie kliknij przycisk **Next** (Dalej).
3. Wybierz i typ karty i sieci. 
4. Aby Podłącz wirtualną kartę Sieciową, gdy maszyna wirtualna jest włączona, wybierz **na połączenie na mocy**. Kliknij przycisk **dalej** > **Zakończ**, a następnie kliknij przycisk **OK**.
 

## <a name="register-the-configuration-server"></a>Zarejestruj serwer konfiguracji 

1. Z konsoli klienta VMWare vSphere włączyć na maszynie Wirtualnej.
2. Maszyna wirtualna wykona rozruch do środowiska instalacji systemu Windows Server 2016. Zaakceptuj Umowę licencyjną i określ hasło administratora.
3. Po zakończeniu instalacji, zaloguj się do maszyny Wirtualnej jako administrator.
4. Podczas pierwszego logowania, uruchamia narzędzie konfiguracji Azure lokacji odzyskiwania.
5. Określ nazwę, która służy do rejestrowania serwera konfiguracji z usługą Site Recovery. Następnie kliknij przycisk **Next** (Dalej).
6. Narzędzie sprawdza, czy połączenie maszyny Wirtualnej Azure. Po nawiązaniu połączenia, kliknij przycisk **Zaloguj**, aby zalogować się do subskrypcji platformy Azure. Poświadczenia muszą mieć dostęp do magazynu, w którym chcesz zarejestrować serwer konfiguracji.
7. Narzędzie wykonuje pewne zadania konfiguracji, a następnie wykonuje ponowny rozruch.
8. Ponownie zaloguj się na tym komputerze. Automatycznie uruchomi Kreatora konfiguracji serwera zarządzania.

### <a name="configure-settings"></a>Konfigurowanie ustawień

1. W Kreatorze konfiguracji serwera zarządzania > **ustawienia łączności**, wybierz kartę Sieciową, która będzie odbierać ruch związany z replikacją. Następnie kliknij przycisk **Save** (Zapisz). Nie można zmienić to ustawienie jest skonfigurowane.
2. W **magazyn usług odzyskiwania wybierz**, wybierz subskrypcję platformy Azure i odpowiedniej grupy zasobów i magazynu.
3. W **zainstalować oprogramowanie innych firm**, Zaakceptuj agreeemtn licencji i kliknij przycisk **Pobierz i zainstaluj**, aby zainstalować serwer MySQL.
4. Kliknij przycisk **zainstalować VMware PowerLCI**. Upewnij się, że przed tym zamknięciu wszystkich okien przeglądarki. Następnie kliknij przycisk **Kontynuuj**
5. W **sprawdzania poprawności konfiguracji urządzenia**, będzie można zweryfikować wymagania wstępne, przed kontynuowaniem.
6. W **Konfiguruj vCenter Server/vSphere ESXi serwer**, określ nazwę FQDN lub adres IP serwera vCenter lub hostem vSphere, na maszynach wirtualnych, które chcesz replikować znajdują się. Określ port, na którym nasłuchuje serwer i przyjazną nazwę używanego do serwera VMware w magazynie.
7. Określ poświadczenia, które będą używane przez serwer konfiguracji do łączenia się z serwerem VMware. Usługa Site Recovery używa tych poświadczeń do automatycznego wykrywania maszyn wirtualnych VMware, które są dostępne dla replikacji. Kliknij przycisk **Dodaj**, a następnie kliknij przycisk **Kontynuuj**.
8. W **skonfigurować poświadczenia maszyny wirtualnej**, określ nazwę użytkownika i hasło, które będzie używane automatycznie zainstalować usługi mobilności na maszynach, po włączeniu replikacji. W przypadku komputerów z systemem Windows konto musi uprawnienia administratora lokalnego na maszynach, które mają być replikowane. Dla systemu Linux należy podać szczegóły dla konta głównego.
9. Kliknij przycisk **konfiguracji Finalize** do ukończenia rejestracji. 
10. Po zakończeniu rejestracji w portalu Azure, sprawdź, czy serwer konfiguracji i serwera VMware są wyświetlane **źródła** strony w magazynie. Następnie kliknij przycisk **OK** do konfigurowania ustawień obiektu docelowego.


## <a name="troubleshoot-deployment-issues"></a>Rozwiązywanie problemów dotyczących wdrożenia

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Kolejne kroki

Przejrzyj samouczki dotyczące konfigurowania odzyskiwania po awarii [maszyn wirtualnych VMware](tutorial-vmware-to-azure.md) i [serwerów fizycznych](tutorial-physical-to-azure.md) na platformie Azure.
