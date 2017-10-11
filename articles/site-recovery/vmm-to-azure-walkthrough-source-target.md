---
title: "Konfigurowanie źródłowych i docelowych replikacji funkcji Hyper-V do platformy Azure (w programie System Center VMM) z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera podsumowanie kroków, aby skonfigurować ustawienia źródłowego i docelowego replikacji maszyn wirtualnych funkcji Hyper-V w chmurach VMM do magazynu Azure z usługą Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5edb6d87-25a5-40fe-b6f1-ddf7b55a6b31
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/25/2017
ms.author: raynew
ms.openlocfilehash: c72f839d0a1288dccb7deb3e44fc2b20d64818f0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="step-8-set-up-the-source-and-target-for-hyper-v-with-vmm-replication-to-azure"></a>Krok 8: Konfigurowanie źródłowych i docelowych dla replikacji funkcji Hyper-V (w programie VMM) na platformie Azure

Po [utworzenie magazynu](vmm-to-azure-walkthrough-create-vault.md) i określenie, co chcesz replikować, użyj w tym artykule do skonfigurowania ustawień źródłowym i docelowym, podczas replikowania lokalnych maszyn wirtualnych funkcji Hyper-V w chmurach programu System Center Virtual Machine Manager (VMM) na platformie Azure przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.

Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego

S1. Kliknij pozycję **Przygotowanie infrastruktury** > **Źródło**.

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. W obszarze **Przygotowywanie źródła** kliknij pozycję **+ VMM**, aby dodać serwer programu VMM.

    ![Konfiguracja źródła](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. W obszarze **Dodawanie serwera** sprawdź, czy **Serwer programu System Center VMM** pojawia się w sekcji **Typ serwera** i czy serwer VMM spełnia [warunki wstępne i wymagania dotyczące adresu URL](#prerequisites).
4. Pobierz plik instalacyjny dostawcy usługi Azure Site Recovery.
5. Pobierz klucz rejestracji. Będzie on potrzebny po uruchomieniu Instalatora. Klucz jest ważny przez pięć dni po jego wygenerowaniu.

    ![Konfiguracja źródła](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-the-provider-on-the-vmm-server"></a>Instalowanie dostawcy na serwerze VMM

1. Uruchom plik Instalatora dostawcy na serwerze VMM.
2. W usłudze **Microsoft Update** można włączyć aktualizacje, aby aktualizacje dostawcy były instalowane zgodnie z zasadami Microsoft Update.
3. W obszarze **Instalacja** zaakceptuj lub zmodyfikuj domyślną lokalizację instalacji dostawcy i kliknij przycisk **Zainstaluj**.

    ![Lokalizacja instalacji](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. Po zakończeniu instalacji kliknij przycisk **Zarejestruj**, aby zarejestrować serwer programu VMM w magazynie.
5. Na stronie **Ustawienia magazynu** kliknij przycisk **Przeglądaj**, aby wybrać plik klucza magazynu. Określ subskrypcję usługi Azure Site Recovery oraz nazwę magazynu.

    ![Rejestracja serwera](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. W obszarze **Połączenie internetowe** określ, jak dostawca uruchomiony na serwerze próbujemy VMM będzie się łączyć z usługą Site Recovery za pośrednictwem Internetu.

   * Jeśli chcesz, aby dostawca łączył się bezpośrednio, wybierz opcję **Połącz bezpośrednio z usługą Azure Site Recovery bez serwera proxy**.
   * Jeśli istniejący serwer proxy wymaga uwierzytelniania lub chcesz użyć niestandardowego serwera proxy, wybierz opcję **Połącz z usługą Azure Site Recovery przy użyciu serwera proxy**.
   * Jeśli używasz niestandardowego serwera proxy, określ adres, port i poświadczenia.
   * W przypadku użycia serwera proxy już zezwolono na użycie adresów URL opisanych w [wymaganiach wstępnych](#on-premises-prerequisites).
   * W przypadku użycia niestandardowego serwera proxy konto Uruchom jako programu VMM (DRAProxyAccount) zostanie automatycznie utworzone przy użyciu określonych poświadczeń serwera proxy. Skonfiguruj serwer proxy tak, aby to konto mogło być pomyślnie uwierzytelnione. Ustawienia konta Uruchom jako programu VMM można zmodyfikować w konsoli programu VMM. W obszarze **Ustawienia** rozwiń pozycję **Zabezpieczenia** > **Konta Uruchom jako**, a następnie zmodyfikuj hasło dla konta DRAProxyAccount. Aby to ustawienie zostało zastosowane, należy ponownie uruchomić usługę programu VMM.

     ![Internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. Zaakceptuj lub zmodyfikuj lokalizację certyfikatu SSL, który jest automatycznie generowany w związku z szyfrowaniem danych. Ten certyfikat jest wykorzystywany, jeśli włączysz szyfrowanie danych dla chmury chronionej przez platformę Azure w portalu usługi Azure Site Recovery. Przechowuj ten certyfikat w bezpiecznym miejscu. Po uruchomieniu trybu failover na platformie Azure certyfikat będzie potrzebny do odszyfrowania danych, jeśli włączono szyfrowanie.
8. W polu **Nazwa serwera** wprowadź przyjazną nazwę identyfikującą serwer VMM w magazynie. W konfiguracji klastra określ nazwę roli klastra VMM.
9. Włącz **synchronizację metadanych chmury**, jeśli chcesz synchronizować metadane dla wszystkich chmur na serwerze VMM w magazynie. To działanie ma miejsce tylko raz na każdym serwerze. Jeśli nie chcesz synchronizować wszystkich chmur, możesz nie zaznaczać tego ustawienia i synchronizować poszczególne chmury indywidualnie we właściwościach chmury w konsoli programu VMM. Kliknij przycisk **Zarejestruj**, aby zakończyć proces.

    ![Rejestracja serwera](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. Rozpoczyna się rejestracja. Po zakończeniu rejestracji serwer jest wyświetlany w obszarze **Infrastruktura usługi Site Recovery** > **Serwery VMM**.


## <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Instalacja agenta Usług odzyskiwania Azure na hostach funkcji Hyper-V

1. Po skonfigurowaniu dostawcy należy pobrać plik instalacyjny agenta usług Azure Recovery Services. Uruchom Instalator na każdym serwerze funkcji Hyper-V w chmurze VMM.

    ![Lokacje funkcji Hyper-V](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. W obszarze **Sprawdzanie wymagań wstępnych** kliknij przycisk **Dalej**. Wszystkie brakujące wymagania wstępne zostaną zainstalowane automatycznie.

    ![Wymagania wstępne dotyczące agenta Usług odzyskiwania](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. W obszarze **Ustawienia instalacji** zaakceptuj lub zmodyfikuj lokalizację instalacji i lokalizację pamięci podręcznej. Możesz skonfigurować pamięć podręczną na dysku, który ma co najmniej 5 GB dostępnego miejsca, ale zalecamy dysk pamięci podręcznej z co najmniej 600 GB wolnego miejsca. Następnie kliknij pozycję **Zainstaluj**.
4. Po zakończeniu instalacji kliknij przycisk **Zamknij**, aby zakończyć.

    ![Rejestracja agenta MARS](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a>Instalacja przy użyciu wiersza polecenia
Agenta Usług odzyskiwania Microsoft Azure można zainstalować z poziomu wiersza polecenia przy użyciu następującego polecenia:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Konfigurowanie internetowego dostępu serwera proxy do usługi Site Recovery na hostach funkcji Hyper-V

Agent Usług odzyskiwania uruchomiony na hostach funkcji Hyper-V wymaga internetowego dostępu do platformy Azure, aby umożliwić replikację maszyny wirtualnej. Jeśli uzyskujesz dostęp do Internetu za pośrednictwem serwera proxy, skonfiguruj go w następujący sposób:

1. Otwórz przystawkę MMC usługi Microsoft Azure Backup na hoście funkcji Hyper-V. Domyślnie skrót do usługi Microsoft Azure Backup jest dostępny na pulpicie lub w folderze C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. W przystawce kliknij pozycję **Zmień właściwości**.
3. Na karcie **Konfiguracja serwera proxy** podaj informacje dotyczące serwera proxy.

    ![Rejestracja agenta MARS](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. Upewnij się, że agent może osiągnąć adresy URL opisane w [wymaganiach wstępnych](#on-premises-prerequisites).

## <a name="set-up-the-target-environment"></a>Konfigurowanie środowiska docelowego
Określ konto magazynu Azure do wykorzystania podczas replikacji i sieć platformy Azure, z którą maszyny wirtualne Azure będą się łączyć po przejściu w tryb failover.

1. Kliknij pozycję **Przygotowywanie infrastruktury** > **Cel**, a następnie wybierz subskrypcję i grupę zasobów, w której chcesz utworzyć maszyny wirtualne w trybie failover. Wybierz model wdrażania, którego chcesz użyć na platformie Azure (model usługi Resource Manager lub model klasyczny) dla maszyn wirtualnych w trybie failover.

    ![Magazyn](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure.

    ![Magazyn](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. Jeśli nie utworzono konta magazynu, a chcesz je utworzyć przy użyciu usługi Resource Manager, kliknij pozycję **+ Konto magazynu**, aby zrobić to w tym miejscu.  W bloku **Utwórz konto magazynu** określ nazwę konta, jego typ, subskrypcję i lokalizację. Konto powinno znajdować się w tej samej lokalizacji co magazyn Usług odzyskiwania.

   ![Magazyn](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * Jeśli chcesz utworzyć konto magazynu przy użyciu modelu klasycznego, należy to zrobić w witrynie Azure Portal. [Dowiedz się więcej](../storage/common/storage-create-storage-account.md)
   * Jeśli używasz konta usługi Premium Storage dla replikowanych danych, skonfiguruj dodatkowe, standardowe konto magazynu do przechowywania dzienników replikacji, które przechwytują zachodzące zmiany w danych lokalnych.
5. Jeśli nie utworzono sieci platformy Azure, a chcesz ją utworzyć przy użyciu usługi Resource Manager, kliknij pozycję **+ Sieć**, aby zrobić to w tym miejscu. W bloku **Utwórz sieć wirtualną** określ nazwę sieciową, zakres adresów, szczegóły podsieci, subskrypcję i lokalizację. Sieć powinna znajdować się w tej samej lokalizacji co magazyn Usług odzyskiwania.

   ![Sieć](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   Jeśli chcesz utworzyć sieć przy użyciu modelu klasycznego, zrób to w witrynie Azure Portal. [Dowiedz się więcej](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).





## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 9: Konfigurowanie mapowania sieci](vmm-to-azure-walkthrough-network-mapping.md)
