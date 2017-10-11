---
title: Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM do platformy Azure | Microsoft Docs
description: "W tym artykule opisano sposób replikowania maszyn wirtualnych funkcji Hyper-V na hostach funkcji Hyper-V w chmurach programu System Center VMM do platformy Azure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 9d526a1f-0d8e-46ec-83eb-7ea762271db5
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: ac0931a71a2814723380256fc5326fc431c82f2c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM do platformy Azure
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](site-recovery-vmm-to-azure.md)
> * [Program PowerShell — model usługi Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Portal klasyczny](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell — model klasyczny](site-recovery-deploy-with-powershell.md)
>
>

Usługa Azure Site Recovery przyczynia się do strategii związanej z ciągłością biznesową i odzyskiwaniem po awarii (BCDR, business continuity and disaster recovery) poprzez organizowanie replikacji, pracy w trybie failover i odzyskiwania maszyn wirtualnych oraz serwerów fizycznych. Maszyny można replikować do platformy Azure lub lokalnego pomocniczego centrum danych. Aby zapoznać się z szybkim omówieniem, przeczytaj temat [Co to jest usługa Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Omówienie
W tym artykule opisano sposób wdrażania usługi Site Recovery w celu replikowania maszyn wirtualnych funkcji Hyper-V na hostach funkcji Hyper-V w chmurach programu VMM do platformy Azure.

W tym artykule omówiono wymagania wstępne dla scenariusza i przedstawiono sposób konfigurowania magazynu usługi Azure Site Recovery, zapewniania instalacji dostawcy usługi Azure Site Recovery na źródłowym serwerze programu VMM, rejestrowania serwera w magazynie, dodawania konta magazynu Azure, instalowania agenta Usług odzyskiwania Azure na serwerach hostów funkcji Hyper-V, konfigurowania ustawień ochrony chmur programu VMM, które będą stosowane do wszystkich chronionych maszyn wirtualnych, a następnie zapewniania ochrony tych maszyn wirtualnych. Aby zakończyć, przetestuj tryb failover w celu sprawdzenia, czy wszystko działa zgodnie z oczekiwaniami.

Zamieść wszelkie komentarze lub pytania pod tym artykułem lub na [forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Architektura
![Architektura](./media/site-recovery-vmm-to-azure-classic/topology.png)

* Dostawca usługi Azure Site Recovery jest instalowany na serwerze programu VMM podczas wdrażania usługi Site Recovery, a serwer programu VMM jest rejestrowany w magazynie usługi Site Recovery. Dostawca komunikuje się z usługą Site Recovery, aby replikować aranżację.
* Agent Usług odzyskiwania Azure jest instalowany na serwerach hostów funkcji Hyper-V podczas wdrażania usługi Site Recovery. Obsługuje on replikację danych do magazynu Azure.

## <a name="azure-prerequisites"></a>Wymagania wstępne platformy Azure
Oto, czego będziesz potrzebować na platformie Azure.

| **Wymagania wstępne** | **Szczegóły** |
| --- | --- |
| **Konto platformy Azure** |Będziesz potrzebować konta platformy [Microsoft Azure](https://azure.microsoft.com/). Możesz rozpocząć od [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Dowiedz się więcej](https://azure.microsoft.com/pricing/details/site-recovery/) o cenach usługi Site Recovery. |
| **Magazyn platformy Azure** |Będziesz potrzebować konta magazynu Azure do przechowywania replikowanych danych. Replikowane dane są przechowywane w usłudze Azure Storage, a w przypadku przejścia w tryb failover uruchamiane są maszyny wirtualne platformy Azure. <br/><br/>Potrzebujesz [standardowego konta magazynu geograficznie nadmiarowego](../storage/common/storage-redundancy.md#geo-redundant-storage). Konto musi znajdować się w tym samym regionie co usługa Site Recovery i musi być skojarzone z tą samą subskrypcją. Replikacja do konta magazynu w warstwie Premium obecnie nie jest obsługiwana i nie powinna być używana.<br/><br/>[Przeczytaj o](../storage/common/storage-introduction.md) usłudze Azure Storage. |
| **Sieć platformy Azure** |Potrzebujesz sieci wirtualnej platformy Azure, z którą maszyny wirtualne Azure będą nawiązywać połączenie w przypadku przejścia w tryb failover. Sieć wirtualna platformy Azure musi znajdować się w tym samym regionie co magazyn usługi Site Recovery. |

## <a name="on-premises-prerequisites"></a>Lokalne wymagania wstępne
Oto, co należy zapewnić lokalnie.

| **Wymagania wstępne** | **Szczegóły** |
| --- | --- |
| **VMM** |Będziesz potrzebować co najmniej jednego serwera programu VMM wdrożonego jako fizyczny lub wirtualny serwer autonomiczny lub jako klaster wirtualny. <br/><br/>Na serwerze programu VMM powinien być uruchomiony program System Center 2012 R2 z najnowszymi aktualizacjami zbiorczymi.<br/><br/>Będziesz potrzebować co najmniej jednej chmury skonfigurowanej na serwerze programu VMM.<br/><br/>Chmura źródłowa, którą chcesz chronić, musi zawierać co najmniej jedną grupę hostów programu VMM.<br/><br/>Aby dowiedzieć się więcej na temat konfigurowania chmur programu VMM, zobacz [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) (Przewodnik: tworzenie chmur prywatnych przy użyciu programu System Center 2012 SP1 VMM) na blogu Keitha Mayera. |
| **Funkcja Hyper-V** |Będziesz potrzebować co najmniej jednego serwera hosta lub klastra funkcji Hyper-V w chmurze programu VMM. Serwer hosta powinien mieć co najmniej jedną maszynę wirtualną. <br/><br/>Serwer funkcji Hyper-V musi być uruchomiony w **systemie Windows Server 2012 R2** z rolą Hyper-V lub w **systemie Microsoft Hyper-V Server 2012 R2** i musi mieć zainstalowane najnowsze aktualizacje.<br/><br/>Serwer funkcji Hyper-V zawierający maszyny wirtualne, które chcesz chronić, musi znajdować się w chmurze programu VMM.<br/><br/>Jeśli używasz funkcji Hyper-V w klastrze, broker klastra nie jest tworzony automatycznie, jeśli masz klaster oparty na statycznym adresie IP. Konieczne będzie ręczne skonfigurowanie brokera klastra. Aby [dowiedzieć się więcej](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters), zobacz wpis na blogu Aidana Finna. |
| **Chronione maszyny** | Maszyny wirtualne, które chcesz chronić, powinny być zgodne z [wymaganiami platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). |

## <a name="network-mapping-prerequisites"></a>Wymagania wstępne związane z mapowaniem sieci
Gdy chronisz maszyny wirtualne na platformie Azure, mapowanie sieci działa między źródłowymi sieciami maszyny wirtualnej na źródłowym serwerze programu VMM a docelowymi sieciami platformy Azure. Dzięki temu dostępne są następujące korzyści:

* Wszystkie maszyny, które przechodzą w tryb failover w tej samej sieci, mogą się ze sobą łączyć niezależnie od tego, jakiego planu odzyskiwania używają.
* Jeśli brama sieci została skonfigurowana w docelowej sieci platformy Azure, maszyny wirtualne mogą łączyć się z innymi lokalnymi maszynami wirtualnymi.
* Jeśli nie skonfigurujesz mapowania sieci, tylko maszyny wirtualne, które przechodzą w tryb failover w tym samym planie odzyskiwania, będą mogły się ze sobą łączyć po przejściu w tryb failover na platformie Azure.

Jeśli chcesz wdrożyć mapowanie sieci, musisz upewnić się, że spełnione są następujące warunki:

* Maszyny wirtualne, które chcesz chronić na źródłowym serwerze programu VMM, powinny być połączone z siecią maszyn wirtualnych. Ta sieć powinna być połączona z siecią logiczną skojarzoną z chmurą.
* Sieć platformy Azure, z którą replikowane maszyny wirtualne mogą łączyć się po przejściu w tryb failover na platformie Azure. Tę sieć wybierzesz podczas przejścia w tryb failover. Sieć powinna znajdować się w tym samym regionie co subskrypcja usługi Azure Site Recovery.


Przygotuj sieci w programie VMM:

   * [Skonfiguruj sieci logiczne](https://technet.microsoft.com/library/jj721568.aspx).
   * [Skonfiguruj sieci maszyn wirtualnych](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Krok 1. Tworzenie magazynu usługi Site Recovery
1. Zaloguj się do [portalu zarządzania](https://portal.azure.com) z serwera programu VMM, który chcesz zarejestrować.
2. Kliknij pozycje **Data Services** > **Usługi odzyskiwania** > **Magazyn usługi Site Recovery**.
3. Kliknij pozycje **Utwórz nowe** > **Szybkie tworzenie**.
4. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn.
5. W polu **Region** wybierz region geograficzny dla magazynu. Aby sprawdzić obsługiwane regiony, zobacz sekcję dotyczącą dostępności geograficznej w temacie [Szczegóły cennika usługi Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Kliknij pozycję **Utwórz magazyn**.

    ![Nowy magazyn](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Sprawdź informacje na pasku stanu, aby potwierdzić pomyślne utworzenie magazynu. Magazyn będzie wyświetlany jako **Aktywny** na stronie głównej Usług odzyskiwania.

## <a name="step-2-generate-a-vault-registration-key"></a>Krok 2. Generowanie klucza rejestracji magazynu
Wygeneruj klucz rejestracji magazynu. Po pobraniu dostawcy usługi Azure Site Recovery i zainstalowaniu go na serwerze programu VMM użyjesz tego klucza do zarejestrowania serwera programu VMM w magazynie.

1. Na stronie **Usługi odzyskiwania** kliknij magazyn, aby otworzyć stronę Szybki start. Stronę Szybki start można również otworzyć w dowolnym momencie przy użyciu ikony.

    ![Ikona Szybki start](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)
2. Na liście rozwijanej wybierz pozycję **Między lokalną lokacją programu VMM a platformą Microsoft Azure**.
3. W oknie **Przygotowanie serwerów programu VMM** kliknij plik **Wygeneruj klucz rejestracji**. Plik klucza jest generowany automatycznie i ważny przez 5 dni po jego wygenerowaniu. Jeśli nie uzyskujesz dostępu do portalu Azure z serwera programu VMM, musisz skopiować ten plik na serwer.

    ![Klucz rejestracji](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Krok 3. Instalowanie dostawcy usługi Azure Site Recovery
1. W oknie **Szybki Start** > **Przygotowanie serwerów programu VMM** kliknij pozycję **Pobierz dostawcę usługi Microsoft Azure Site Recovery do instalacji na serwerach programu VMM**, aby uzyskać najnowszą wersję pliku instalacyjnego dostawcy.
2. Uruchom ten plik na źródłowym serwerze programu VMM.

   > [!NOTE]
   > Jeśli program VMM jest wdrożony w klastrze i instalujesz dostawcę po raz pierwszy, zainstaluj go w aktywnym węźle i ukończ instalację, aby zarejestrować serwer programu VMM w magazynie. Następnie zainstaluj dostawcę w pozostałych węzłach. Pamiętaj, że jeśli uaktualniasz dostawcę, musisz uaktualnić go we wszystkich węzłach, ponieważ powinna być w nich uruchomiona ta sama wersja dostawcy.
   >
   >
3. Instalator sprawdza wymagania wstępne i żąda uprawnienia do zatrzymania usługi programu VMM w celu rozpoczęcia instalacji dostawcy. Usługa programu VMM zostanie automatycznie ponownie uruchomiona po zakończeniu instalacji. Jeśli instalujesz w klastrze programu VMM, zostanie wyświetlony monit o zatrzymanie roli klastra.
4. W usłudze **Microsoft Update** można włączyć aktualizacje. Po włączeniu tego ustawienia aktualizacje dostawcy będą instalowane zgodnie z zasadami usługi Microsoft Update.

    ![Usługa Microsoft Updates](./media/site-recovery-vmm-to-azure-classic/updates.png)
5. Dostawca jest instalowany w lokalizacji **<SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin**. Kliknij pozycję **Zainstaluj**.

   ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)
6. Po zainstalowaniu dostawcy kliknij pozycję **Zarejestruj**, aby zarejestrować serwer w magazynie.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)
7. Zweryfikuj, że w polu **Nazwa magazynu** jest wpisana nazwa magazynu, w którym serwer zostanie zarejestrowany. Kliknij przycisk *Dalej*.

    ![Rejestracja serwera](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)
8. W obszarze **Połączenie internetowe** określ, jak dostawca uruchomiony na serwerze programu VMM łączy się z Internetem. Wybierz pozycję **Połącz z istniejącymi ustawieniami serwera proxy**, aby użyć domyślnych ustawień połączenia internetowego skonfigurowanych na serwerze.

    ![Ustawienia internetowe](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

   * Jeśli chcesz użyć niestandardowego serwera proxy, należy skonfigurować go przed zainstalowaniem dostawcy. Podczas konfigurowania niestandardowych ustawień serwera proxy wykonywany jest test w celu sprawdzenia połączenia serwera proxy.
   * Jeśli używasz niestandardowego serwera proxy lub domyślny serwer proxy wymaga uwierzytelniania, musisz wprowadzić szczegóły serwera proxy, łącznie z adresem i portem.
   * Następujące adresy URL powinny być dostępne z serwera programu VMM i hostów funkcji Hyper-V
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Zezwól na adresy IP, opisane w temacie [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (Zakresy adresów IP centrum danych Azure), i protokół HTTPS (port 443). Należy również umieścić na liście dozwolonych zakresy adresów IP regionu Azure, który będzie używany, i regionu Zachodnie stany USA.
   * W przypadku użycia niestandardowego serwera proxy konto Uruchom jako programu VMM (DRAProxyAccount) zostanie automatycznie utworzone przy użyciu określonych poświadczeń serwera proxy. Skonfiguruj serwer proxy tak, aby to konto mogło być pomyślnie uwierzytelnione. Ustawienia konta Uruchom jako programu VMM można zmodyfikować w konsoli programu VMM. Aby to zrobić, otwórz obszar roboczy **Ustawienia**, rozwiń pozycję **Zabezpieczenia**, kliknij pozycję **Konta Uruchom jako**, a następnie zmodyfikuj hasło dla konta DRAProxyAccount. Aby to ustawienie zostało zastosowane, należy ponownie uruchomić usługę programu VMM.
9. W oknie **Klucz rejestracji** wybierz klucz, aby potwierdzić, że klucz został pobrany z usługi Azure Site Recovery i skopiowany na serwer programu VMM.
10. Ustawienie szyfrowania jest używane tylko w przypadku replikacji maszyn wirtualnych funkcji Hyper-V w chmurach VMM do platformy Azure. Ustawienie nie jest używane w przypadku replikowania do lokacji dodatkowej.
11. W polu **Nazwa serwera** wprowadź przyjazną nazwę identyfikującą serwer VMM w magazynie. W konfiguracji klastra określ nazwę roli klastra VMM.
12. W oknie **Synchronizacja metadanych chmury** określ, czy chcesz synchronizować metadane dla wszystkich chmur na serwerze programu VMM z magazynem. To działanie ma miejsce tylko raz na każdym serwerze. Jeśli nie chcesz synchronizować wszystkich chmur, możesz nie zaznaczać tego ustawienia i synchronizować poszczególne chmury indywidualnie we właściwościach chmury w konsoli programu VMM.
13. Kliknij przycisk **Dalej**, aby ukończyć proces. Po rejestracji metadane z serwera programu VMM są pobierane przez usługę Azure Site Recovery. Serwer jest wyświetlany na karcie **Serwery programu VMM** na stronie **Serwery** w magazynie.

    ![Ostatnia strona](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Po rejestracji metadane z serwera programu VMM są pobierane przez usługę Azure Site Recovery. Serwer jest wyświetlany na karcie **Serwery programu VMM** na stronie **Serwery** w magazynie.

### <a name="command-line-installation"></a>Instalacja przy użyciu wiersza polecenia
Dostawcę usługi Azure Site Recovery można również zainstalować przy użyciu następującego polecenia w wierszu polecenia. Tej metody można użyć do zainstalowania dostawcy w rdzeniu serwera dla systemu Windows Server 2012 R2.

1. Pobierz plik instalacyjny dostawcy i klucz rejestracji do folderu. Na przykład C:\ASR.
2. Zatrzymaj usługę System Center Virtual Machine Manager.
3. W wierszu polecenia o podwyższonym poziomie uprawnień wyodrębnij instalatora dostawcy przy użyciu następujących poleceń:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Zainstaluj dostawcę w następujący sposób:

        C:\ASR> setupdr.exe /i
5. Zarejestruj dostawcę w następujący sposób:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

gdzie parametry są następujące:

* **/Credentials**: obowiązkowy parametr określający lokalizację pliku klucza rejestracji.  
* **/FriendlyName**: obowiązkowy parametr dla nazwy serwera hosta funkcji Hyper-V, która będzie wyświetlana w portalu usługi Azure Site Recovery.
* **/EncryptionEnabled**: opcjonalny parametr, który pozwala określić, czy maszyny wirtualne powinny być szyfrowane na platformie Azure (szyfrowanie danych w stanie spoczynku). Nazwa pliku powinna mieć rozszerzenie **pfx**.
* **/proxyAddress**: opcjonalny parametr określający adres serwera proxy.
* **/proxyport**: opcjonalny parametr określający port serwera proxy.
* **/proxyUsername**: opcjonalny parametr określający nazwę użytkownika serwera proxy.
* **/proxyPassword**: opcjonalny parametr określający hasło serwera proxy.  

## <a name="step-4-create-an-azure-storage-account"></a>Krok 4. Tworzenie konta magazynu platformy Azure
1. Jeśli nie masz konta usługi Azure Storage, kliknij pozycję **Dodaj konto usługi Azure Storage**, aby utworzyć konto.
2. Utwórz konto z włączoną funkcją replikacji geograficznej. Musi ono znajdować się w tym samym regionie co usługa Azure Site Recovery i musi być skojarzone z tą samą subskrypcją.

    ![Konto magazynu](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [!NOTE]
> [Migracja kont magazynu](../azure-resource-manager/resource-group-move-resources.md) w grupach zasobów w ramach tej samej subskrypcji lub w różnych subskrypcjach nie jest obsługiwana dla kont magazynu używanych do wdrażania usługi Site Recovery.
>
>

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Krok 5. Instalowanie agenta Usług odzyskiwania Azure
Zainstaluj agenta Usług odzyskiwania Azure na każdym serwerze hosta funkcji Hyper-V w chmurze programu VMM.

1. Kliknij pozycje **Szybki Start** > **Pobierz agenta Usług odzyskiwania Azure do instalacji na hostach**, aby otrzymać najnowszą wersję pliku instalacyjnego agenta.

    ![Instalowanie agenta Usług odzyskiwania](./media/site-recovery-vmm-to-azure-classic/install-agent.png)
2. Uruchom plik instalacyjny na każdym serwerze hosta funkcji Hyper-V.
3. Na stronie **Sprawdzanie wymagań wstępnych** kliknij przycisk **Dalej**. Wszystkie brakujące wymagania wstępne zostaną zainstalowane automatycznie.

    ![Wymagania wstępne dotyczące agenta Usług odzyskiwania](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)
4. Na stronie **Ustawienia instalacji** określ, gdzie chcesz zainstalować agenta, i wybierz lokalizację pamięci podręcznej, w której będą instalowane metadane kopii zapasowych. Następnie kliknij pozycję **Zainstaluj**.
5. Po ukończeniu instalacji kliknij przycisk **Zamknij**, aby zakończyć kreatora.

    ![Rejestracja agenta MARS](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Instalacja przy użyciu wiersza polecenia
Agenta Usług odzyskiwania Microsoft Azure można również zainstalować z poziomu wiersza polecenia przy użyciu następującego polecenia:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Krok 6. Konfigurowanie ustawień ochrony chmury
Po zarejestrowaniu serwera VMM można skonfigurować ustawienia ochrony chmury. Opcja **Synchronizuj dane chmury z magazynem** została włączona podczas instalowania dostawcy, dlatego wszystkie chmury na serwerze programu VMM są wyświetlane na karcie <b>Chronione elementy</b> w magazynie.

![Opublikowana chmura](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Na stronie Szybki start kliknij pozycję **Skonfiguruj ochronę dla chmur programu VMM**.
2. Na karcie **Chronione elementy** kliknij chmurę, którą chcesz skonfigurować, i przejdź do karty **Konfiguracja**.
3. W polu **Lokalizacja docelowa** wybierz pozycję **Azure**.
4. W polu **Konto magazynu** wybierz konto usługi Azure Storage używanego do replikacji.
5. Dla opcji **Szyfruj przechowywane dane** ustaw wartość **Wyłączone**. To ustawienie określa, że dane powinny być replikowane w postaci zaszyfrowanej między lokacją lokalną a platformą Azure.
6. W polu **Częstotliwość kopiowania** pozostaw ustawienie domyślne. Ta wartość określa, jak często dane mają być synchronizowane między lokalizacjami źródłowymi a docelowymi.
7. W polu **Zachowaj punkty odzyskiwania dla** pozostaw ustawienie domyślne. Przy wartości domyślnej równej zero tylko najnowszy punkt odzyskiwania dla podstawowej maszyny wirtualnej jest przechowywany na serwerze hosta repliki.
8. W polu **Częstotliwość migawek spójnych z aplikacjami** pozostaw ustawienie domyślne. Ta wartość określa, jak często mają być tworzone migawki. Migawki używają usługi kopiowania woluminów w tle (VSS), aby zapewnić stan spójności aplikacji podczas wykonywania migawki.  Jeśli ustawisz wartość, upewnij się, że jest ona mniejsza od liczby skonfigurowanych dodatkowych punktów odzyskiwania.
9. W polu **Czas rozpoczęcia replikacji** określ, kiedy powinna rozpocząć się replikacja początkowa danych z platformą Azure. Używana będzie strefa czasowa na serwerze hosta funkcji Hyper-V. Zalecamy zaplanowanie replikacji początkowej poza godzinami największego obciążenia.

    ![Ustawienia replikacji chmury](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Po zapisaniu ustawień zostanie utworzone zadanie, które można monitorować na karcie **Zadania**. Wszystkie serwery hostów funkcji Hyper-V w źródłowej chmurze programu VMM zostaną skonfigurowane do replikacji.

Zapisane ustawienia chmury można modyfikować na karcie **Konfigurowanie**. Aby zmodyfikować lokalizację docelową lub docelowe konto magazynu, musisz usunąć konfigurację chmury, a następnie ponownie skonfigurować chmurę. Jeśli zmienisz konto magazynu, ta zmiana zostanie zastosowana tylko do maszyn wirtualnych skonfigurowanych do ochrony po zmodyfikowaniu konta magazynu. Istniejące maszyny wirtualne nie są migrowane do nowego konta magazynu.

## <a name="step-7-configure-network-mapping"></a>Krok 7. Konfigurowanie mapowania sieci
Przed rozpoczęciem mapowania sieci należy się upewnić, że maszyny wirtualne na źródłowym serwerze programu VMM są połączone z siecią maszyny wirtualnej. Ponadto należy utworzyć co najmniej jedną sieć wirtualną platformy Azure. Wiele sieci maszyn wirtualnych można mapować do pojedynczej sieci platformy Azure.

1. Na stronie Szybki start kliknij pozycję **Mapuj sieci**.
2. Na karcie **Sieci** w polu **Lokalizacja źródłowa** wybierz źródłowy serwer programu VMM. W polu **Lokalizacja docelowa** wybierz pozycję Azure.
3. W polu **Sieci źródłowe** jest wyświetlana lista sieci maszyny wirtualnej skojarzonych z serwerem programu VMM. W polu **Sieci docelowe** są wyświetlane sieci platformy Azure skojarzone z subskrypcją.
4. Wybierz źródłową sieć maszyny wirtualnej i kliknij przycisk **Mapuj**.
5. Na stronie **Wybieranie sieci docelowej** wybierz docelową sieć platformy Azure, której chcesz użyć.
6. Kliknij znacznik wyboru, aby ukończyć proces mapowania.

    ![Ustawienia replikacji chmury](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Po zapisaniu ustawień zadanie zacznie śledzić postęp mapowania i może być monitorowane na karcie Zadania. Istniejące maszyny wirtualne repliki odpowiadające źródłowej sieci maszyny wirtualnej zostaną połączone z docelowymi sieciami platformy Azure. Nowe maszyny wirtualne, połączone ze źródłową siecią maszyny wirtualnej, zostaną połączone z zamapowaną siecią platformy Azure po replikacji. Jeśli zmodyfikujesz istniejące mapowanie, uwzględniając nową sieć, maszyn wirtualne repliki zostaną podłączone z wykorzystaniem nowych ustawień.

Jeśli sieć docelowa ma wiele podsieci i jedna z tych podsieci ma taką samą nazwę jak podsieć, w której znajduje się źródłowa maszyna wirtualna, replika maszyny wirtualnej zostanie podłączona do tej docelowej podsieci po przejściu w tryb failover. Jeśli nie istnieje docelowa podsieć o takiej samej nazwie, maszyna wirtualna zostanie podłączona do pierwszej podsieci w sieci.

> [!NOTE]
> [Migracja sieci](../azure-resource-manager/resource-group-move-resources.md) w grupach zasobów w ramach tej samej subskrypcji lub w różnych subskrypcjach nie jest obsługiwana dla sieci używanych do wdrażania usługi Site Recovery.
>
>

## <a name="step-8-enable-protection-for-virtual-machines"></a>Krok 8. Włączanie ochrony dla maszyn wirtualnych
Po poprawnym skonfigurowaniu serwerów, chmur i sieci można włączyć ochronę dla maszyn wirtualnych w chmurze. Pamiętaj o następujących kwestiach:

* Maszyny wirtualne muszą spełniać [wymagania związane z platformą Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
* Aby włączyć ochronę, należy ustawić właściwości systemu operacyjnego i dysku systemu operacyjnego dla maszyny wirtualnej. Podczas tworzenia maszyny wirtualnej w programie VMM przy użyciu szablonu maszyny wirtualnej można ustawić właściwości. Można również ustawić te właściwości dla istniejących maszyn wirtualnych na kartach **Ogólne** i **Konfiguracja sprzętu** właściwości maszyny wirtualnej. Jeśli nie ustawisz tych właściwości w programie VMM, możesz skonfigurować je w portalu Azure Site Recovery.

    ![Tworzenie maszyny wirtualnej](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Modyfikowanie właściwości maszyny wirtualnej](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)

1. Aby włączyć ochronę, na karcie **Virtual Machines** w chmurze, w której znajduje się maszyna wirtualna, kliknij pozycje **Włącz ochronę** > **Dodaj maszyny wirtualne**.
2. Z listy maszyn wirtualnych w chmurze wybierz maszynę, którą chcesz chronić.

    ![Włączanie ochrony maszyny wirtualnej](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    Możesz śledzić postęp akcji **Włącz ochronę** na karcie **Zadania**, łącznie z replikacją początkową. Po uruchomieniu zadania **Finalizowanie ochrony** maszyna wirtualna jest gotowa do przejścia w tryb failover. Po włączeniu ochrony i zreplikowaniu maszyn wirtualnych będą one widoczne na platformie Azure.

    ![Zadanie ochrony maszyny wirtualnej](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

1. Zweryfikuj właściwości maszyny wirtualnej i zmodyfikuj je zgodnie z wymaganiami.

    ![Weryfikowanie maszyn wirtualnych](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)
2. Na karcie **Konfigurowanie** właściwości maszyny wirtualnej można zmodyfikować poniższe właściwości sieci.

* **Liczba kart sieciowych na docelowej maszynie wirtualnej** — Liczba kart sieciowych jest zależna od rozmiaru określonego dla docelowej maszyny wirtualnej. Sprawdź [specyfikacje rozmiaru maszyny wirtualnej](../virtual-machines/linux/sizes.md), aby ustalić liczbę obsługiwanych kart sieciowych zależnie od rozmiaru maszyny wirtualnej. W razie zmodyfikowania rozmiaru maszyny wirtualnej i zapisania ustawień liczba kart sieciowych ulegnie zmianie po następnym otwarciu strony **Konfigurowanie**. Liczba kart sieciowych docelowych maszyn wirtualnych jest minimalną liczbą kart sieciowych na źródłowej maszynie wirtualnej i maksymalną liczbą kart sieciowych obsługiwanych przy wybranym rozmiarze maszyny wirtualnej zgodnie z następującą zależnością: 

  * Jeśli liczba kart sieciowych w maszynie źródłowej jest mniejsza lub równa liczbie kart sieciowych dozwolonych dla rozmiaru maszyny docelowej, maszyna docelowa będzie mieć taką samą liczbę kart sieciowych jak maszyna źródłowa.
  * Jeśli liczba kart sieciowych dla źródłowej maszyny wirtualnej przekracza liczbę dozwolonych kart sieciowych dla rozmiaru maszyny docelowej, zostanie użyta maksymalna liczba kart dla rozmiaru maszyny docelowej.
  * Na przykład, jeśli maszyna źródłowa ma dwie karty sieciowe, a rozmiar maszyny docelowej obsługuje cztery karty, maszyna docelowa będzie mieć dwie karty sieciowe. Jeśli maszyna źródłowa ma dwie karty sieciowe, ale rozmiar maszyny docelowej obsługuje tylko jedną kartę, maszyna docelowa będzie mieć tylko jedną kartę sieciową.     
* **Sieć docelowej maszyny wirtualnej** — Sieć, z którą łączy się maszyna wirtualna, jest wybierana zgodnie z mapowaniem sieci dla źródłowej maszyny wirtualnej. Jeśli źródłowa maszyna wirtualna ma kilka kart sieciowych, a źródłowe maszyny wirtualne są zamapowane do innych sieci w lokalizacji docelowej, należy wybrać jedną z sieci docelowych.
* **Podsieć każdej karty sieciowej** — Dla każdej karty sieciowej można wybrać podsieć, z którą będzie łączyć się maszyna wirtualna po przejściu w tryb failover.
* **Docelowy adres IP** — Jeśli karta sieciowa źródłowej maszyny wirtualnej jest skonfigurowana do użycia statycznego adresu IP, można zapewnić adres IP dla docelowej maszyny wirtualnej. Korzystając z tej funkcji, można zachować adres IP źródłowej maszyny wirtualnej po przejściu w tryb failover. Jeśli nie zapewniono adresu IP, dowolny dostępny adres IP jest przypisywany do karty sieciowej podczas przejścia w tryb failover. Jeśli docelowy adres IP zostanie określony, ale jest już używany przez inną maszynę wirtualną uruchomioną na platformie Azure, przejście w tryb failover zakończy się niepowodzeniem.  

    ![Modyfikowanie właściwości sieci](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

> [!NOTE]
> Maszyny wirtualne systemu Linux ze statycznym adresem IP nie są obsługiwane.
>
>

## <a name="test-the-deployment"></a>Testowanie wdrożenia
Aby przetestować wdrożenie, możesz przeprowadzić próbę przejścia w tryb failover dla pojedynczej maszyny wirtualnej lub utworzyć plan odzyskiwania uwzględniający wiele maszyn wirtualnych i przeprowadzić próbę przejścia w tryb failover dla tego planu.  

Próba przejścia w tryb failover symuluje mechanizm pracy awaryjnej i odzyskiwania w sieci izolowanej. Należy pamiętać, że:

* Jeśli chcesz nawiązać połączenie z maszyną wirtualną na platformie Azure przy użyciu Pulpitu zdalnego po przejściu w tryb failover, włącz funkcję Podłączanie pulpitu zdalnego na maszynie wirtualnej przed rozpoczęciem próby przejścia w tryb failover.
* Po przejściu w tryb failover publiczny adres IP będzie używany do nawiązania połączenia z maszyną wirtualną na platformie Azure przy użyciu funkcji Podłączanie pulpitu zdalnego. Jeśli chcesz to zrobić, upewnij się, że nie ma żadnych zasad domeny, które uniemożliwiają połączenie z maszyną wirtualną przy użyciu adresu publicznego.

> [!NOTE]
> Aby uzyskać najlepszą wydajność przechodzenia w tryb failover na platformie Azure, zainstaluj agenta platformy Azure na maszynie wirtualnej. Przyspiesza to rozruch i pomaga w rozwiązywaniu problemów. Pobierz [agenta systemu Linux](https://github.com/Azure/WALinuxAgent) lub [agenta systemu Windows](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

### <a name="create-a-recovery-plan"></a>Tworzenie planu odzyskiwania
1. Na karcie **Plany odzyskiwania** dodaj nowy plan. Określ nazwę **VMM** w polu **Typ źródła** i źródłowy serwer programu VMM w polu **Źródło**. Lokalizacją docelową będzie platforma Azure.

    ![Tworzenie planu odzyskiwania](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)
2. Na stronie **Wybieranie maszyn wirtualnych** wybierz maszyny wirtualne, które zostaną dodane do planu odzyskiwania. Te maszyny wirtualne są dodawane do planu odzyskiwania grupy domyślnej — Grupa 1. Przetestowano maksymalnie 100 maszyn wirtualnych w pojedynczym planie odzyskiwania.

* Jeśli chcesz zweryfikować właściwości maszyny wirtualnej przed dodaniem jej do planu, kliknij maszynę wirtualną na stronie właściwości chmury, w której ta maszyna się znajduje. Możesz również skonfigurować właściwości maszyny wirtualnej w konsoli programu VMM.
* Dla wszystkich wyświetlanych maszyn wirtualnych włączono ochronę. Na liście uwzględnione są zarówno maszyny wirtualne, dla których włączono ochronę i ukończono replikację początkową, jak i maszyny z włączoną ochroną, które oczekują na replikację początkową. Tylko maszyny wirtualne z ukończoną replikacją początkową mogą przechodzić w tryb failover w ramach planu odzyskiwania.

    ![Tworzenie planu odzyskiwania](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Utworzony plan odzyskiwania jest wyświetlany na karcie **Plany odzyskiwania**. Możesz również dodać [elementy runbook automatyzacji Azure](site-recovery-runbook-automation.md) do planu odzyskiwania, aby automatyzować czynności wykonywane w trybie failover.

### <a name="run-a-test-failover"></a>Wykonywanie próby przejścia w tryb failover
Istnieją dwa sposoby wykonania próby trybu failover do platformy Azure.

* **Testowanie trybu failover bez sieci platformy Azure** — Ten test trybu failover pozwala sprawdzić, czy maszyna wirtualna funkcjonuje poprawnie na platformie Azure. Maszyna wirtualna nie zostanie połączona z żadną siecią platformy Azure po przejściu w tryb failover.
* **Testowanie trybu failover z siecią platformy Azure** — Ten test trybu failover pozwala sprawdzić, czy całe środowisko replikacji funkcjonuje zgodnie z oczekiwaniami, a po przejściu w tryb failover maszyny wirtualne są łączone z określoną docelową siecią platformy Azure. W przypadku obsługi podsieci podczas testowania trybu failover podsieć dla testowanej maszyny wirtualnej będzie wybierana na podstawie podsieci maszyny wirtualnej repliki. Ta metoda różni się od zwykłej replikacji, w przypadku której podsieć maszyny wirtualnej repliki jest oparta na podsieci źródłowej maszyny wirtualnej.

Jeśli chcesz testować przejście w tryb failover na platformie Azure dla maszyny wirtualnej z włączonymi zabezpieczeniami bez określania docelowej sieci platformy Azure, nie musisz wykonywać żadnych czynności przygotowawczych. Aby przetestować tryb failover z docelową siecią platformy Azure, musisz utworzyć nową sieć platformy Azure, która jest odizolowana od sieci platformy Azure środowiska produkcyjnego (jest to domyślne zachowanie podczas tworzenia nowej sieci na platformie Azure). Aby uzyskać więcej informacji, zobacz, jak [testować tryb failover](site-recovery-failover.md).

Musisz również skonfigurować infrastrukturę dla zreplikowanej maszyny wirtualnej, aby mogła działać zgodnie z oczekiwaniami. Na przykład maszyna wirtualna z kontrolerem domeny i systemem DNS może być replikowana do platformy Azure przy użyciu usługi Azure Site Recovery i może być utworzona w sieci testowej przy użyciu testowego trybu failover. Aby uzyskać więcej informacji, zobacz sekcję [Zagadnienia dotyczące testowania trybu failover dla usługi Active Directory](site-recovery-active-directory.md#test-failover-considerations).

Aby testować tryb failover, wykonaj następujące czynności:

1. Na karcie **Plany odzyskiwania** wybierz plan i kliknij pozycję **Test pracy w trybie failover**.
2. Na stronie **Potwierdzanie testu pracy w trybie failover** wybierz opcję **Brak** lub określoną sieć platformy Azure.  Pamiętaj, że jeśli wybierzesz opcję Brak, test pracy w trybie failover sprawdzi, czy maszyna wirtualna została zreplikowana poprawnie na platformie Azure, ale nie sprawdzi konfiguracji sieci replikacji.

    ![Brak sieci](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)
3. Jeśli szyfrowanie danych jest włączone dla chmury, w polu **Klucz szyfrowania** wybierz certyfikat wystawiony podczas instalacji dostawcy na serwerze programu VMM, gdy została włączona opcja szyfrowania danych dla chmury.
4. Na karcie **Zadania** można śledzić postęp trybu failover. W portalu Azure powinna być również widoczna testowa replika maszyny wirtualnej. Jeśli skonfigurowano dostęp do maszyn wirtualnych z sieci lokalnej, możesz zainicjować połączenie Pulpitu zdalnego z maszyną wirtualną.
5. Gdy tryb failover osiągnie fazę **Ukończ testowanie**, kliknij przycisk **Zakończ test**, aby ukończyć test. Możesz przejść do szczegółów na karcie **Zadanie**, aby śledzić postęp i stan trybu failover oraz wykonać dowolne potrzebne akcje.
6. Po przejściu w tryb failover w witrynie Azure Portal będzie widoczna testowa replika maszyny wirtualnej. Jeśli skonfigurowano dostęp do maszyn wirtualnych z sieci lokalnej, możesz zainicjować połączenie Pulpitu zdalnego z maszyną wirtualną. Wykonaj następujące czynności:

   1. Sprawdź, czy maszyna wirtualna jest uruchamiana pomyślnie.
   2. Jeśli chcesz nawiązać połączenie z maszyną wirtualną na platformie Azure przy użyciu Pulpitu zdalnego po przejściu w tryb failover, włącz funkcję Podłączanie pulpitu zdalnego na maszynie wirtualnej przed rozpoczęciem próby przejścia w tryb failover. Musisz również dodać punkt końcowy Pulpitu zdalnego (RDP) na maszynie wirtualnej. W tym celu możesz użyć [elementów runbook automatyzacji Azure](site-recovery-runbook-automation.md).
   3. Jeśli po przejściu w tryb failover używasz publicznego adresu IP do nawiązywania połączenia z maszyną wirtualną na platformie Azure przy użyciu Pulpitu zdalnego, upewnij się, że nie ma żadnych zasad domeny, które uniemożliwiają połączenie z maszyną wirtualną przy użyciu adresu publicznego.
7. Po zakończeniu testowania wykonaj następujące czynności:

   * Kliknij pozycję **Ukończono testowe przełączanie w tryb failover**. Wyczyść środowisko testowe, aby automatycznie wyłączyć i usunąć testową maszynę wirtualną.
   * Kliknij pozycję **Uwagi**, aby zarejestrować i zapisać wszelkie obserwacje związane z testowym trybem failover.


## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat [konfigurowania planów odzyskiwania](site-recovery-create-recovery-plans.md) i [trybu failover](site-recovery-failover.md).
