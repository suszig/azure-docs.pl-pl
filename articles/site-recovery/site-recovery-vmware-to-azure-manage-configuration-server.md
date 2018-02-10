---
title: " Zarządzanie serwera konfiguracji na potrzeby odzyskiwania po awarii VMware z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób zarządzania istniejącego serwera konfiguracji na potrzeby odzyskiwania po awarii VMware do platformy Azure, z usługą Azure Site Recovery."
services: site-recovery
author: AnoopVasudavan
ms.service: site-recovery
ms.topic: article
ms.date: 02/04/2018
ms.author: anoopkv
ms.openlocfilehash: 9cdabfb4e24423d76e4f247f184ac4156c3b257b
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="manage-the-configuration-server"></a>Zarządzanie serwerem konfiguracji

Lokalny serwer konfiguracji należy skonfigurować korzystając z [usługi Azure Site Recovery](site-recovery-overview.md) usługi odzyskiwania po awarii maszyn wirtualnych VMware i serwerów fizycznych do platformy Azure. Serwer konfiguracji koordynuje komunikacji między lokalnymi VMware i na platformie Azure i zarządza replikacji danych. Ten artykuł zawiera podsumowanie typowych zadań zarządzania serwera konfiguracji po jego wdrożeniu.

## <a name="modify-vmware-settings"></a>Zmodyfikuj ustawienia VMware

Zmodyfikuj ustawienia dla serwera VMware, do którego nawiązuje połączenie z serwerem konfiguracji.

1. Zaloguj się na komputerze, na którym działa serwer konfiguracji.
2. Uruchom Menedżera konfiguracji usługi Azure Site Recovery z skrót na pulpicie. Możesz również otworzyć **https://configuration-server-name/IP:44315**.
3. Kliknij przycisk **Zarządzaj vCenter Server/vSPhere ESXi serwera**:
    - Aby skojarzyć innego serwera VMware z serwerem konfiguracji, kliknij przycisk **Dodaj vCenter Server/vSphere ESXi serwera**i określ szczegóły serwera.
    - Aby zaktualizować poświadczenia używane do łączenia się z serwerem VMware automatycznego wykrywania maszyn wirtualnych VMware, kliknij przycisk **Edytuj**. Określ nowe poświadczenia, a następnie kliknij przycisk **OK**.

        ![Modyfikowanie VMware](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-vmware-server.png)

## <a name="modify-credentials-for-mobility-service-installation"></a>Zmodyfikowania poświadczeń dla instalacji usługi mobilności

Zmodyfikuj poświadczenia używane do automatycznej instalacji usługi mobilności na maszynach wirtualnych VMware można włączyć replikacji.

1. Zaloguj się na komputerze, na którym działa serwer konfiguracji.
2. Uruchom Menedżera konfiguracji usługi Azure Site Recovery z skrót na pulpicie. Możesz również otworzyć **https://configuration-server-name/IP:44315**.
3. Kliknij przycisk **Zarządzanie poświadczeniami maszyny wirtualnej**i określ nowe poświadczenia. Następnie kliknij przycisk **OK** można zaktualizować ustawień.

    ![Zmodyfikowania poświadczeń usługi mobilności](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Zmodyfikuj ustawienia serwera proxy

Zmodyfikuj ustawienia serwera proxy używane przez komputer serwera konfiguracji dla dostępu do Internetu na platformie Azure. Jeśli masz maszyny serwera dodatkowych procesów, oprócz domyślnego serwera proces uruchomiony na komputerze z serwerem konfiguracji, zmodyfikuj ustawienia na obu komputerach.

1. Zaloguj się na komputerze, na którym działa serwer konfiguracji.
2. Uruchom Menedżera konfiguracji usługi Azure Site Recovery z skrót na pulpicie. Możesz również otworzyć **https://configuration-server-name/IP:44315**.
3. Kliknij przycisk **Zarządzanie łączności**i zaktualizuj wartości serwera proxy. Następnie kliknij przycisk **zapisać** można zaktualizować ustawień.

## <a name="add-a-network-adapter"></a>Dodaj kartę sieciową

Szablon OVF wdraża serwer konfiguracji maszyny Wirtualnej z jedną kartą sieciową. Możesz [dodać dodatkowe karty na maszynie wirtualnej)](how-to-deploy-configuration-server.md#add-an-additional-adapter), ale należy to zrobić, aby zarejestrować serwer konfiguracji w magazynie.

Jeśli trzeba dodać adapter po serwer konfiguracji został zarejestrowany w magazynie, musisz Dodaj kartę właściwości maszyny Wirtualnej, a następnie ponownie zarejestrować serwer w magazynie.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Zarejestruj ponownie serwer konfiguracji, w tym samym magazynie

Jeśli potrzebujesz można ponownie zarejestrować serwer konfiguracji, w tym samym magazynie. f maszyna serwera dodatkowych procesów, oprócz domyślnego serwera proces uruchomiony na komputerze z serwerem konfiguracji, Zarejestruj ponownie obydwie maszyny.

  1. W magazynie, otwórz **Zarządzaj** > **infrastruktura usługi Site Recovery** > **serwery konfiguracji**.
  2. W **serwerów**, kliknij przycisk **Pobierz klucz rejestracji**. Spowoduje to pobranie pliku poświadczeń magazynu.
  3. Zaloguj się na komputerze serwera konfiguracji.
  4. W **%ProgramData%\ASR\home\svagent\bin**, otwórz **cspsconfigtool.exe**.
  5. Na **rejestracji magazynu** karcie, kliknij przycisk Przeglądaj i znajduje się plik poświadczeń magazynu pobranego.
  6. Jeśli to konieczne, podaj szczegóły serwera proxy. Następnie kliknij przycisk **zarejestrować**.
  7. Otwórz okno poleceń programu PowerShell administratora i uruchom następujące polecenie:

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```
## <a name="upgrade-the-configuration-server"></a>Uaktualnij serwer konfiguracji

Możesz uruchomić pakiety zbiorcze aktualizacji, aby zaktualizować serwer konfiguracji. Aktualizacje mogą być stosowane dla maksymalnie N-4 wersji. Na przykład:

- Jeśli używasz 9.7, 9,8, 9.9 lub 9.10 — można uaktualnić bezpośrednio do 9.11.
- Jeśli używasz 9.6 lub starszym, i chcesz uaktualnić do 9.11, należy najpierw uaktualnić do wersji 9.7. przed 9.11.

Łącza do pakiety zbiorcze aktualizacji dla uaktualnienia wszystkich wersji serwera konfiguracji są dostępne w [strona typu wiki aktualizacji](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

Uaktualnij serwer w następujący sposób:

1. Pobierz plik Instalatora aktualizacji na serwerze konfiguracji.
2. Kliknij dwukrotnie, aby uruchomić Instalatora.
3. Instalator wykrywa bieżącą wersję uruchomionych na komputerze.
4. Kliknij przycisk **OK** do potwierdzenia, a następnie uruchom uaktualnianie. 


## <a name="delete-or-unregister-a-configuration-server"></a>Usuń lub Wyrejestruj serwer konfiguracji

1. Wyłącz [Wyłącz ochronę](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) dla wszystkich maszyn wirtualnych w ramach konfiguracji serwera.
2. [Usuń skojarzenie](site-recovery-setup-replication-settings-vmware.md#dissociate-a-configuration-server-from-a-replication-policy) i [usunąć](site-recovery-setup-replication-settings-vmware.md#delete-a-replication-policy) wszystkie zasady replikacji z serwera konfiguracji.
3. [Usuń](site-recovery-vmware-to-azure-manage-vCenter.md#delete-a-vcenter-in-azure-site-recovery) wszystkie hosty serwerów/vSphere Vcenter, które są skojarzone z serwerem konfiguracji.
4. W magazynie, otwórz **infrastruktura usługi Site Recovery** > **serwery konfiguracji**
5. Kliknij serwer konfiguracji, który ma zostać usunięty. Następnie na **szczegóły** kliknij przycisk **usunąć**.

    ![Usuwanie konfiguracji serwera](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>Usuwanie przy użyciu programu PowerShell

Opcjonalnie można usunąć serwera konfiguracji przy użyciu programu PowerShell:

1. [Zainstaluj](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) modułu Azure PowerShell
2. Zaloguj się do konta platformy Azure przy użyciu polecenia:
    
    `Login-AzureRmAccount`
3. Wybierz subskrypcję magazynu:

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Ustaw kontekst magazynu:
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Pobrać serwer konfiguracji:

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Usuń serwer konfiguracji:

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Można użyć **-Force** opcji w Usuń AzureRmSiteRecoveryFabric do wymuszenia usunięcia serwera konfiguracji.
 


## <a name="renew-ssl-certificates"></a>Odnawianie certyfikatów SSL

Serwer konfiguracji jest serwer sieci web wbudowanych, który organizuje działania usługi mobilności, proces serwerów i głównych serwerów docelowych dołączone do niego. Serwer sieci web używa certyfikatu SSL do uwierzytelniania klientów. Certyfikat wygaśnie po upływie trzech lat i mogą być odnawiane w dowolnym momencie.

### <a name="check-expiry"></a>Sprawdzanie wygaśnięcia

Dla wdrożenia serwera konfiguracji przed maj 2016 okresu ważności certyfikatu została ustawiona na jeden rok. Jeśli certyfikat jest wkrótce wygaśnie, są następujące:

- Jeśli data wygaśnięcia jest dwóch miesięcy lub mniej, Usługa rozpoczyna wysyłanie powiadomień, w portalu, a także za pośrednictwem poczty e-mail (Jeśli masz subskrypcję usługi Azure Site Recovery powiadomień).
- Transparent powiadomienie jest wyświetlane na stronie zasobów magazynu. Kliknij przycisk transparentu, aby uzyskać więcej informacji.
- Jeśli widzisz **Uaktualnij teraz** przycisku, oznacza to, czy niektóre składniki w danym środowisku, które nie zostały uaktualnione do 9.4.xxxx.x lub nowszej wersji. Należy uaktualnić składniki, aby odnowić certyfikat. Nie można odnowić w starszych wersjach.

### <a name="renew-the-certificate"></a>Odnów certyfikat

1. W magazynie, otwórz **infrastruktura usługi Site Recovery** > **serwera konfiguracji**i kliknij serwer konfiguracji.
2. Data wygaśnięcia jest wyświetlany w obszarze **kondycję konfiguracji serwera**
3. Kliknij przycisk **odnawiania certyfikatów**. 


## <a name="next-steps"></a>Kolejne kroki

Przejrzyj samouczki dotyczące konfigurowania odzyskiwania po awarii [maszyn wirtualnych VMware](tutorial-vmware-to-azure.md) i servers(tutorial-physical-to-azure.md) fizycznych do platformy Azure.
