---
title: "Monitorowanie i rozwiązywanie problemów z ochroną maszyn wirtualnych i serwerów fizycznych | Dokumentacja firmy Microsoft"
description: "Usługa Azure Site Recovery koordynuje replikację, tryb failover i odzyskiwania maszyn wirtualnych znajdujących się na serwerach lokalnych do platformy Azure lub dodatkowego centrum danych. W tym artykule umożliwiają monitorowanie i rozwiązywanie problemów z ochroną witryny programu Virtual Machine Manager lub funkcji Hyper-V."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 0fc8e368-0c0e-4bb1-9d50-cffd5ad0853f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: 5bcb5dcb6afc3909e34dde31f845e014e7c539e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Monitorowanie i rozwiązywanie problemów z ochroną maszyn wirtualnych i serwerów fizycznych
Ten przewodnik monitorowania i rozwiązywania problemów pomaga śledzić kondycję replikacji i rozwiązywanie problemów z techniki dla usługi Azure Site Recovery.

## <a name="understand-the-components"></a>Opis składników
### <a name="vmware-virtual-machine-or-physical-server-site-deployment-for-replication-between-on-premises-and-azure"></a>Maszyny wirtualnej VMware lub serwerów fizycznych wdrożenia lokacji replikacji między lokalną i platformą Azure
Aby skonfigurować odzyskiwanie bazy danych między na lokalnej maszynie wirtualnej VMware lub serwerów fizycznych i usługi Azure, należy skonfigurować serwer konfiguracji głównego serwera docelowego, a proces składników serwera na maszynie wirtualnej lub serwerze. Po włączeniu ochrony dla serwera źródłowego usługi Azure Site Recovery instaluje funkcji Mobile Apps usługi Microsoft Azure App Service. Po awarii lokalnymi i po źródłowy serwer nie może za pośrednictwem na platformie Azure, klienci konieczne jest skonfigurowanie serwer przetwarzania na platformie Azure i głównego serwera docelowego na lokalnych do odbudowywania serwera źródłowego na lokalnych.

![Wdrożenie lokacji VMware/fizyczne replikacji między lokalną i platformą Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-sites"></a>Wdrożenie lokacji programu Virtual Machine Manager do replikacji między lokacjami lokalnymi
Aby skonfigurować bazę danych odzyskiwania między dwoma lokacjami lokalnymi, należy pobrać dostawcy usługi Azure Site Recovery i zainstalować go na serwerze programu Virtual Machine Manager. Dostawca musi mieć łączność Internet upewnij się, że wszystkie operacje, które są uruchamiane z portalu Azure Pobierz translacji na operacje lokalnymi.

![Wdrożenie lokacji programu Virtual Machine Manager do replikacji między lokacjami lokalnymi](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Wdrożenie lokacji programu Virtual Machine Manager replikacji między lokalizacje lokalną i platformą Azure
Podczas odzyskiwania bazy danych między lokalnymi lokalizacji i na platformie Azure, musisz Pobierz dostawcę usługi Azure Site Recovery i zainstalować ją na serwerze programu Virtual Machine Manager. Należy również zainstalować agenta usług odzyskiwania Azure, która musi zostać zainstalowany na każdym hoście funkcji Hyper-V. [Dowiedz się więcej](site-recovery-hyper-v-azure-architecture.md) Aby uzyskać więcej informacji.

![Wdrożenie lokacji programu Virtual Machine Manager replikacji między lokalizacje lokalną i platformą Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Wdrożenie lokacji funkcji Hyper-V dla replikacji między lokalnymi lokalizacji i na platformie Azure
Ten proces jest podobny do wdrażania programu Virtual Machine Manager. Jedyna różnica polega na uzyskać zainstalowany dostawca usługi Azure Site Recovery i Agent usług odzyskiwania Azure na sam host funkcji Hyper-V. [Dowiedz się więcej](site-recovery-hyper-v-azure-architecture.md). .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Monitorowanie operacji konfiguracji, ochrony i odzyskiwania
Każdej operacji w usłudze Azure Site Recovery jest sprawdzana i śledzić w obszarze **zadania** kartę. Dla każdego konfiguracji, ochrony lub odzyskiwania błąd, przejdź do **zadania** karcie i Znajdź błędy.

![Filtr nie powiodła się na karcie zadania](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Jeśli okaże się błędy w obszarze **zadania** , kliknij zadanie, a kliknij **szczegóły błędu** dla tego zadania.

![Przycisk Szczegóły błędu](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Szczegóły błędu pomoże Ci identyfikować możliwa przyczyna i zalecenia dotyczące problem.

![Powoduje wyświetlenie okna dialogowego szczegóły błędu dla określonego zadania](media/site-recovery-monitoring-and-troubleshooting/image5.png)

W poprzednim przykładzie jest prawdopodobnie powodować niepowodzenie konfiguracji ochrony inną operację, która jest w toku. Rozwiąż problem, na podstawie zalecenia, a następnie kliknij przycisk **RESART** zainicjować ponownie wykonać operację.

![Przycisk Uruchom ponownie na karcie zadania](media/site-recovery-monitoring-and-troubleshooting/image6.png)

**Ponowne URUCHOMIENIE** opcja nie jest dostępna dla wszystkich operacji. Jeśli nie ma operacji **ponowne URUCHOMIENIE** opcji, przejdź wstecz do obiektu i ponów próbę wykonania operacji. Możesz anulować wszystkie zadania, który jest w toku przy użyciu **ANULOWAĆ** przycisku.

![Przycisk Anuluj](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machines"></a>Monitorowanie kondycji replikacji dla maszyny wirtualnej
Azure portal umożliwia zdalne monitorowanie dostawców usługi Azure Site Recovery dla każdego chronionego jednostek. Kliknij przycisk **chronione elementy**, a następnie kliknij przycisk **chmur programu VMM** lub **grup ochrony**. **Chmur programu VMM** karta jest dostępna tylko dla wdrożeń, które są oparte na programu Virtual Machine Manager. W innych sytuacjach chronionych jednostek podlegają **grup ochrony** kartę.

![Opcje chmur programu VMM i grup ochrony](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Kliknij chronionej jednostki w odpowiedniej chmury lub grupy ochrony, aby wyświetlić wszystkie dostępne operacje są wyświetlane w okienku u dołu.

![Dostępne opcje dla wybranej jednostki chronionych](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Jak pokazano na poprzedni zrzut ekranu, kondycja maszyny wirtualnej jest **krytyczny**. Możesz kliknąć **szczegóły błędu** przycisk w dół, aby wyświetlić ten błąd. Na podstawie **możliwe przyczyny** i **zalecenie**, rozwiąż problem.

![Przycisk Szczegóły błędu](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Błędy i zalecenia w oknie dialogowym Szczegóły błędu](media/site-recovery-monitoring-and-troubleshooting/image11.png)

> [!NOTE]
> Jeśli wszystkie aktywnymi operacjami w toku lub nie powiodło się, przejdź do **zadania** wyświetlania, jak wspomniano wcześniej, aby wyświetlić błąd dla określonego zadania.
>
>

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Rozwiązywanie problemów funkcji Hyper-V lokalnej
Nawiązywanie połączenia w lokalnej konsoli Menedżera funkcji Hyper-V, wybierz maszynę wirtualną i sprawdzić kondycję replikacji.

![Opcję, aby wyświetlić kondycję replikacji w konsoli Menedżera funkcji Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image12.png)

W takim przypadku **kondycję replikacji** jest **krytyczny**. Kliknij prawym przyciskiem myszy maszynę wirtualną, a następnie kliknij przycisk **replikacji** > **Wyświetl kondycję replikacji** Aby wyświetlić szczegóły.

![Stan replikacji dla określonej maszyny wirtualnej](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Jeśli replikacja jest wstrzymana na maszynie wirtualnej, kliknij prawym przyciskiem myszy maszynę wirtualną, a następnie kliknij przycisk **replikacji** > **Wznów replikację**.

![Opcję, aby wznowić replikację w konsoli Menedżera funkcji Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Jeśli maszyna wirtualna jest migrowana nowy host funkcji Hyper-V, który znajduje się w klastrze lub autonomicznej maszyny, na hoście funkcji Hyper-V został skonfigurowany za pomocą usługi Azure Site Recovery nie mieć wpływ na replikację dla maszyny wirtualnej. Upewnij się, że nowy host funkcji Hyper-V, spełnia wszystkie wymagania wstępne i jest konfigurowana przy użyciu usługi Azure Site Recovery.

### <a name="event-log"></a>Dziennik zdarzeń
| Źródła zdarzeń | Szczegóły |
| --- |:--- |
| **Aplikacje i usługi Dzienniki/Microsoft/VirtualMachineManager/Server/Admin** (serwer programu Virtual Machine Manager) |Umożliwia rejestrowanie przydatne rozwiązywać wiele różnych problemów z programu Virtual Machine Manager. |
| **Aplikacje i usługi Dzienniki/MicrosoftAzureRecoveryServices/replikacji** (host funkcji Hyper-V) |Umożliwia rejestrowanie przydatne rozwiązywać problemy z wielu agenta usług odzyskiwania Microsoft Azure. <br/> ![Lokalizacja źródła zdarzeń replikacji dla hosta funkcji Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Aplikacje i usługi Microsoft/dzienniki/Azure lokacji odzyskiwania/dostawcy/Operational** (host funkcji Hyper-V) |Umożliwia rejestrowanie przydatne rozwiązywać wiele problemów z usługą Microsoft Azure Site Recovery. <br/> ![Lokalizacja źródła zdarzeń operacyjnych hosta funkcji Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Aplikacje i usługi Dzienniki/Microsoft/Windows/Hyper-V-VMMS/Admin** (host funkcji Hyper-V) |Umożliwia rejestrowanie przydatna do rozwiązywania problemów z zarządzaniem wiele maszyn wirtualnych funkcji Hyper-V. <br/> ![Lokalizacja źródła zdarzeń Menedżera maszyny wirtualnej dla hostów funkcji Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |

### <a name="hyper-v-replication-logging-options"></a>Opcje rejestrowania replikację funkcji Hyper-V
Wszystkie zdarzenia, które odnoszą się do replikacji funkcji Hyper-V są rejestrowane w funkcji Hyper-V-VMMS\\Admin dziennika znajduje się w obszarze Dzienniki aplikacji i usług\\Microsoft\\systemu Windows. Ponadto można włączyć dziennika analityczne dla Usługa zarządzania maszynami wirtualnymi funkcji Hyper-V. Aby włączyć ten dziennik, najpierw należy dzienniki analityczne i debugowania możliwych do wyświetlenia w Podglądzie zdarzeń. Otwórz Podgląd zdarzeń, a następnie kliknij przycisk **widoku** > **dzienniki Pokaż analityczne i debugowania**.

![Opcja dzienniki Pokaż analityczne i debugowania](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Analityczne dziennika jest widoczna w obszarze **funkcji Hyper-V-VMMS**.

![Analityczne dziennika zdarzeń podglądu drzewa](media/site-recovery-monitoring-and-troubleshooting/image15.png)

W **akcje** okienku, kliknij przycisk **Włącz dziennik**. Po włączeniu, zostanie wyświetlony w **monitora wydajności** jako **sesji śledzenia zdarzeń** znajduje się w obszarze **zestawy modułów zbierających dane.**

![Sesji śledzenia zdarzeń w drzewie monitora wydajności](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Aby wyświetlić informacje zbierane, najpierw Zatrzymaj sesję śledzenia przez wyłączenie dziennika. Zapisz dziennik i otwórz go ponownie w Podglądzie zdarzeń lub użyć innych narzędzi, aby przekonwertować go zgodnie z potrzebami.

## <a name="reach-out-for-microsoft-support"></a>Dotrzeć do firmy Microsoft Support
### <a name="log-collection"></a>Zbierania dzienników
Ochrona lokacji programu Virtual Machine Manager, można znaleźć w temacie [zbierania dzienników usługi Azure Site Recovery przy użyciu narzędzia pomocy technicznej platformy diagnostyki (SDP)](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) do zbierania dzienników wymagane.

W przypadku ochrony lokacji funkcji Hyper-V, Pobierz [narzędzie](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) i wykonaj go na hoście funkcji Hyper-V do zbierania dzienników.

Scenariusze serwera VMware/fizycznych, można znaleźć w temacie [zbierania dzienników usługi Azure Site Recovery VMware i ich ochrony lokacji fizycznej](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) do zbierania dzienników wymagane.

Narzędzie to zbiera dzienniki lokalnie w losowo wybranej nazwie podfolderze % LocalAppData%\ElevatedDiagnostics.

![Przykładowe kroki opisane z ochrony lokacji funkcji Hyper-V.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="open-a-support-ticket"></a>Otwórz bilet pomocy technicznej
Aby podnieść biletu pomocy technicznej dla usługi Azure Site Recovery, dotrzeć do obsługi platformy Azure przy użyciu adresu URL w <http://aka.ms/getazuresupport>.

## <a name="kb-articles"></a>Artykuły z bazy wiedzy
* [Jak zachować literę dysku dla chronionych maszyn wirtualnych, które zostały przełączone do trybu failover lub migracji do usługi Azure](http://support.microsoft.com/kb/3031135)
* [Jak zarządzać lokalnymi do użycia przepustowości sieci ochrony na platformie Azure](https://support.microsoft.com/kb/3056159)
* [Usługa Azure Site Recovery: "nie można odnaleźć zasobu klastra" Wystąpił błąd podczas próby włączenia ochrony dla maszyny wirtualnej](http://support.microsoft.com/kb/3010979)
* [Zrozumienie i rozwiązywanie problemów z funkcją Hyper-V replikacji przewodnik](http://social.technet.microsoft.com/wiki/contents/articles/21948.hyper-v-replica-troubleshooting-guide.aspx)

## <a name="common-azure-site-recovery-errors-and-their-resolutions"></a>Typowe błędy usługi Azure Site Recovery i ich rozwiązania
Poniżej przedstawiono typowe błędy oraz ich rozwiązania. Każdy z błędów jest udokumentowany na stronie osobnych stron typu wiki.

### <a name="general"></a>Ogólne
* <span style="color:green;">Nowy</span> [zadania kończą się niepowodzeniem z powodu błędu "operacja jest w toku". Błąd 505, 514, 532.](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
* <span style="color:green;">Nowy</span> [zadania kończą się niepowodzeniem z powodu błędu "Serwer nie jest połączony z Internetem". Błąd 25018.](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Konfiguracja
* [Nie można zarejestrować serwera programu Virtual Machine Manager z powodu błędu wewnętrznego. Odwołuje się do widoku zadania w portalu usługi Site Recovery, aby uzyskać więcej informacji na temat błędu usługi. Uruchom ponownie Instalatora, aby zarejestrować serwer.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
* [Nie można ustanowić połączenia magazynu Menedżera odzyskiwania funkcji Hyper-V. Sprawdź ustawienia serwera proxy lub spróbuj ponownie później.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfiguracja
* [Nie można utworzyć grupy ochrony: Wystąpił błąd podczas pobierania listy serwerów.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
* [Klaster hosta funkcji Hyper-V zawiera co najmniej jedną kartę sieciową statyczny lub żadna podłączona karta nie są skonfigurowane do korzystania z protokołu DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
* [Program Virtual Machine Manager nie ma uprawnień do wykonania akcji.](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
* [Nie można wybrać konto magazynu w ramach subskrypcji, podczas konfigurowania ochrony.](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Ochrona
* <span style="color:green;">Nowy</span> [Włącz ochronę niepowodzeniem z powodu błędu "Nie można skonfigurować ochrony dla maszyny wirtualnej". Błąd 60007, 40003.](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
* <span style="color:green;">Nowy</span> [Włącz ochronę niepowodzeniem z powodu błędu "Nie można włączyć ochrony dla maszyny wirtualnej." Błąd 70094.](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
* <span style="color:green;">Nowy</span> [migracji na żywo błąd 23848 — maszyna wirtualna ma zostać przeniesiona przy użyciu typu Live. Może to spowodować przerwanie stanu ochrony odzyskiwania maszyny wirtualnej.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx)
* [Nie można włączyć ochrony, ponieważ Agent nie jest zainstalowany na komputerze-hoście.](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
* [Nie można odnaleźć odpowiedniego hosta dla maszyny wirtualnej repliki — z powodu małej zasoby obliczeniowe.](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
* [Nie można odnaleźć odpowiedniego hosta dla maszyny wirtualnej repliki — ze względu na nie dołączone do sieci logicznej.](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
* [Nie można połączyć z komputerem hosta repliki — nie można nawiązać połączenia.](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)

### <a name="recovery"></a>Odzyskiwanie
* Program Virtual Machine Manager nie może ukończyć operacji hosta:
  * [Przełączyć do wybranego punktu odzyskiwania dla maszyny wirtualnej: ogólny błąd odmowy dostępu.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
  * [Funkcji Hyper-V nie można przełączyć do wybranego punktu odzyskiwania dla maszyny wirtualnej: operacja została przerwana.  Spróbuj nowszą punktu odzyskiwania. (0x80004004).](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
  * Połączenie z serwerem nie można ustanowić (0x00002EFD).
    * [Nie można włączyć replikację odwrotną dla maszyny wirtualnej funkcji Hyper-V.](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
    * [Nie można włączyć replikacji dla maszyny wirtualnej maszyny wirtualnej funkcji Hyper-V.](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
  * [Nie można zadeklarować w tryb failover maszyny wirtualnej.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
* [Plan odzyskiwania zawiera maszyny wirtualne, które nie jest gotowa na planowany tryb failover.](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
* [Maszyna wirtualna nie jest gotowa na planowany tryb failover.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* [Maszyna wirtualna nie jest uruchomiony i nie jest wyłączony.](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
* [Operacja poza pasmem wystąpiło na maszynie wirtualnej i zatwierdzania trybu failover nie powiodła się.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* Testowanie trybu failover
  * [Nie można zainicjować trybu failover, ponieważ trwa testowanie trybu failover.](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
* <span style="color:green;">NOWE</span> pracy awaryjnej upłynie limit czasu "PreFailoverWorkflow zadania WaitForScriptExecutionTaskTimeout" z powodu ustawienia konfiguracji dla grupy zabezpieczeń sieci skojarzonych z maszyny wirtualnej lub podsieci, do której należy komputer. Zapoznaj się ["task PreFailoverWorkflow WaitForScriptExecutionTaskTimeout"](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) szczegółowe informacje.

### <a name="configuration-server-process-server-master-target"></a>Serwer konfiguracji, serwer przetwarzania, główny serwer docelowy
* [Hosta ESXi, na którym PS/CS jest hostowany jako maszyny Wirtualnej kończy się niepowodzeniem z purpurowa ekran.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Rozwiązywanie problemów po pracy awaryjnej pulpitu zdalnego
* Wielu klientów ma muszą ponosić problemy, aby nawiązać połączenie nieudane przez maszynę wirtualną na platformie Azure. [Użyj rozwiązywania problemów dokumentu dla protokołu RDP na maszynie wirtualnej](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Dodawanie publicznego adresu IP na maszynie wirtualnej Menedżera zasobów
Jeśli **Connect** przycisk w portalu jest wygaszona i nie połączono z platformy Azure za pośrednictwem połączenia VPN Express Route lub lokacja-lokacja, musisz utworzyć i przypisać do publicznego adresu IP sieci maszyny wirtualnej przed użyciem pulpitu zdalnego / Udostępnionego powłoki. Następnie można dodać publicznego adresu IP dla interfejsu sieciowego maszyny wirtualnej.  

![Dodawanie publicznego adresu IP dla interfejsu sieci przejścia w tryb failover maszyny wirtualnej](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)
