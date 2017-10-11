---
title: Przywracanie z kopii zapasowej maszyny wirtualnej | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak przywracanie z punktu odzyskiwania maszyny wirtualnej platformy Azure"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "Przywracanie kopii zapasowej. jak przywrócić; punkt odzyskiwania;"
ms.assetid: fed877b3-b496-49fb-99e4-653be7c23e3a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: trinadhk; jimpark;
ms.openlocfilehash: fc52c909df5e91741ec1fa21fb911487be039fdc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="restore-virtual-machines-in-azure"></a>Przywracanie maszyn wirtualnych na platformie Azure
> [!div class="op_single_selector"]
> * [Przywracanie maszyn wirtualnych w portalu Azure](backup-azure-arm-restore-vms.md)
> * [Przywracanie maszyn wirtualnych w portalu klasycznym](backup-azure-restore-vms.md)
>
>

Przywrócenie maszyny wirtualnej do nowej maszyny Wirtualnej z kopii zapasowych znajdujących się w magazynie usługi Kopia zapasowa Azure następujące kroki.

> [!IMPORTANT]
> Magazyny kopii zapasowych możesz teraz uaktualnić do magazynów usługi Recovery Services. Więcej szczegółów znajduje się w artykule [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uaktualnianie magazynu kopii zapasowych do magazynu usługi Recovery Services). Firma Microsoft zachęca do przeprowadzenia uaktualnienia magazynów kopii zapasowych do magazynów usługi Recovery Services.<br/> **15 października 2017 r.**: nie będzie już można tworzyć magazynów kopii zapasowych przy użyciu programu PowerShell. <br/> **Od 1 listopada 2017 roku**:
>- Wszystkie pozostałe magazyny kopii zapasowych zostaną automatycznie uaktualnione do magazynów usługi Recovery Services.
>- Nie będzie możliwe uzyskanie dostępu do danych kopii zapasowych w portalu klasycznym. Zamiast tego należy użyć witryny Azure Portal, aby uzyskać dostęp do danych kopii zapasowych w magazynach usługi Recovery Services.
>

## <a name="restore-workflow"></a>Przywróć przepływu pracy
### <a name="step-1-choose-an-item-to-restore"></a>Krok 1: Wybierz element, aby przywrócić
1. Przejdź do **chronione elementy** i wybierz maszynę wirtualną, aby przywrócić do nowej maszyny Wirtualnej.

    ![Elementy chronione](./media/backup-azure-restore-vms/protected-items.png)

    **Punkt odzyskiwania** kolumny w **chronione elementy** strony informuje o liczbę punktów odzyskiwania dla maszyny wirtualnej. **Punkt odzyskiwania najnowszych** kolumny informuje czas wykonania ostatniej kopii zapasowej, z której można przywrócić maszyny wirtualnej.
2. Kliknij przycisk **przywrócić** otworzyć **przywraca element** kreatora.

    ![Przywraca element](./media/backup-azure-restore-vms/restore-item.png)

### <a name="step-2-pick-a-recovery-point"></a>Krok 2: Wybierz punkt odzyskiwania
1. W **wybierz punkt odzyskiwania** ekranu, można przywrócić z najnowszego punktu odzyskiwania lub z określonego punktu w czasie. Jest domyślną opcją wybrany, gdy zostanie otwarty Kreator *punkt odzyskiwania najnowszych*.

    ![Wybierz punkt odzyskiwania](./media/backup-azure-restore-vms/select-recovery-point.png)
2. Aby wybrać wcześniejszego punktu w czasie, wybierz **wybierz datę** opcji na liście rozwijanej i wybierz datę w formancie kalendarza, klikając **ikonę kalendarza**. W formancie wszystkie daty, które mają punkty odzyskiwania są wypełniane światła odcień szarości i można wybrać przez użytkownika.

    ![Wybierz datę](./media/backup-azure-restore-vms/select-date.png)

    Po kliknięciu Data w formancie kalendarza potrzebnych punktów odzyskiwania dostępnych na czy daty będą widoczne w poniższej tabeli punktów odzyskiwania. **Czasu** kolumna wskazuje czas, w którym migawki. **Typu** Wyświetla kolumny [spójności](https://azure.microsoft.com/documentation/articles/backup-azure-vms/#consistency-of-recovery-points) punktu odzyskiwania. Nagłówek tabeli pokazuje liczbę punktów odzyskiwania dostępnych w tym dniu w nawiasach.

    ![Punkty odzyskiwania](./media/backup-azure-restore-vms/recovery-points.png)
3. Wybierz punkt odzyskiwania z **punktów odzyskiwania** tabeli, a następnie kliknij strzałkę dalej, aby przejść do następnego ekranu.

### <a name="step-3-specify-a-destination-location"></a>Krok 3: Określ lokalizację docelową
1. W **wybierz Przywracanie wystąpienia** ekranu Określ szczegóły, gdzie próbę przywrócenia maszyny wirtualnej.

   * Określ nazwę maszyny wirtualnej: podany w usłudze w chmurze, Nazwa maszyny wirtualnej powinna być unikatowa. Nie obsługujemy nadmiernie zapisywania istniejącej maszyny Wirtualnej.
   * Wybierz usługę chmury dla maszyny Wirtualnej: jest to obowiązkowa w przypadku tworzenia maszyny Wirtualnej. Można użyć istniejącej usługi w chmurze lub utworzyć nową usługę w chmurze.

        Niezależnie od nazwy usługi w chmurze jest wybierany były unikatowe globalnie. Zazwyczaj nazwa usługi w chmurze pobiera skojarzony z adresem URL publicznych w formie [cloudservice]. cloudapp.net. Azure uniemożliwi tworzenie nowej usługi w chmurze, jeśli nazwa jest już używana. Jeśli wybierzesz opcję utworzenia nowej usługi w chmurze, będzie mógł skorzystać taką samą nazwę jak maszyny wirtualnej — w którym powinna być unikatowy ma zostać zastosowany do usługi w chmurze skojarzony przypadek pobrana Nazwa maszyny Wirtualnej.

        Wyświetli tylko usługi chmury i sieci wirtualnych, które nie są skojarzone z wszystkich grup koligacji w Szczegóły wystąpienia przywracania. [Dowiedz się więcej](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
2. Wybierz konto magazynu dla maszyny Wirtualnej: jest to konieczne do utworzenia maszyny Wirtualnej. Możesz wybrać z istniejących kont magazynu w tym samym regionie co magazyn usługi Kopia zapasowa Azure. Nie obsługujemy kont magazynu, które są strefy nadmiarowe lub typu magazynu Premium.

    Jeśli nie ma żadnych kont magazynu z obsługiwanych konfiguracji, Utwórz konto magazynu obsługiwanych konfiguracji przed rozpoczęciem operacji przywracania.

    ![Wybierz sieć wirtualną](./media/backup-azure-restore-vms/restore-sa.png)
3. Wybierz sieć wirtualną: sieci wirtualnej (VNET) dla maszyny wirtualnej powinna być zaznaczona w czasie tworzenia maszyny Wirtualnej. Przywracanie interfejsu użytkownika zawiera wszystkie sieci wirtualne w ramach tej subskrypcji, która może być używany. Nie jest to konieczne do wybrania sieci Wirtualnej dla maszyny Wirtualnej przywróconej — można połączyć się z przywróconej maszyny wirtualnej przez internet, nawet jeśli sieć wirtualna nie została zastosowana.

    Usługi w chmurze wybrano jest skojarzony z siecią wirtualną, nie można zmienić sieci wirtualnej.

    ![Wybierz sieć wirtualną](./media/backup-azure-restore-vms/restore-cs-vnet.png)
4. Wybierz podsieć: W przypadku, gdy sieć wirtualna ma podsieci, domyślnie pierwszej podsieci zostanie wybrany. Wybierz podsieć wybór z opcji listy rozwijanej. Aby uzyskać szczegóły podsieci, przejdź do rozszerzenia sieci w [strony głównej portalu](https://manage.windowsazure.com/), przejdź do **sieci wirtualnych** i wybierz sieć wirtualną i przejść do Konfiguruj, aby wyświetlić szczegóły podsieci.

    ![Wybierz podsieć](./media/backup-azure-restore-vms/select-subnet.png)
5. Kliknij przycisk **przesyłania** ikonę w kreatorze, aby przesłać szczegóły, a następnie utworzyć zadanie przywracania.

## <a name="track-the-restore-operation"></a>Śledź operacji przywracania
Po wejściowych wszystkich informacji do Kreatora przywracania i przesłać go kopia zapasowa Azure będzie próbować utworzyć zadanie, aby śledzić operacji przywracania.

![Tworzenie zadania przywracania](./media/backup-azure-restore-vms/create-restore-job.png)

Tworzenie zadań zakończy się pomyślnie, pojawi się powiadomienie wyskakujące wskazujące, utworzenia zadania. Więcej informacji można uzyskać, klikając **zadania** przycisku, który spowoduje przejście do **zadania** kartę.

![Utworzono zadanie przywracania](./media/backup-azure-restore-vms/restore-job-created.png)

Po zakończeniu operacji przywracania, zostanie ona oznaczona jako zakończona w **zadania** kartę.

![Zadanie ukończenia przywracania](./media/backup-azure-restore-vms/restore-job-complete.png)

Po przywróceniu maszyny wirtualnej należy ponownie zainstalować rozszerzenia istniejących oryginalna maszyna wirtualna i [zmodyfikować punkty końcowe](../virtual-machines/windows/classic/setup-endpoints.md) dla maszyny wirtualnej w portalu Azure.

## <a name="post-restore-steps"></a>Wykonywane po przywróceniu kroki
Korzystania z dystrybucji systemu Linux chmury inicjowania na podstawie takich jak Ubuntu, ze względów bezpieczeństwa hasło zostanie zablokowana po przywracania. Użyj rozszerzenia VMAccess przywróconej maszynę wirtualną i [resetowania hasła](../virtual-machines/linux/classic/reset-access.md). Zalecamy używanie kluczy SSH w tych dystrybucji w celu uniknięcia Resetowanie hasła post przywracania.

## <a name="backup-for-restored-vms"></a>Tworzenie kopii zapasowej dla przywróconej maszyny wirtualne
Jeśli maszyna wirtualna została przywrócona do tej samej usługi w chmurze o tej samej nazwie pierwotnie kopii zapasowej maszyny Wirtualnej, kopii zapasowej będzie kontynuowana przy przywracaniu post maszyny Wirtualnej. Jeśli masz przywrócone maszyny Wirtualnej do innej usługi w chmurze lub określić inną nazwę dla maszyny Wirtualnej przywróconej, to będzie traktowany jako nowej maszyny Wirtualnej i należy ustawienia kopii zapasowej przywróconej maszyny wirtualnej.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Przywracanie maszyny Wirtualnej podczas awarii centrum danych Azure
Kopia zapasowa Azure umożliwia przywracanie kopii zapasowej maszyn wirtualnych centrum danych sparowanego w przypadku danych podstawowych Centrum, gdy maszyny wirtualne są uruchomione po awarii środowiska i skonfigurować Magazyn kopii zapasowych się geograficznie nadmiarowego. W takich scenariuszach należy wybrać konto magazynu, który znajduje się w centrum danych sparowanego i resztę procesu przywracania pozostaje w tym samym. Kopia zapasowa Azure używa usługi obliczeniowe z geograficznie sparowanego do utworzenia przywróconej maszyny wirtualnej. Dowiedz się więcej o [odporności centrum danych Azure](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md)

## <a name="restoring-domain-controller-vms"></a>Przywracanie maszyn wirtualnych z kontrolera domeny
Kopia zapasowa maszyn wirtualnych kontrolera domeny (DC) jest obsługiwany scenariusz w usłudze Kopia zapasowa Azure. Jednak należy uważać podczas procesu przywracania. Proces przywracania poprawne zależy od struktura domeny. W przypadku najprostszym pojedynczy kontroler domeny znajduje się w jednej domenie. Najczęściej w przypadku obciążeń produkcyjnych, będziesz mieć pojedynczą domenę z wielu kontrolerów domeny, prawdopodobnie z kilka kontrolerów domeny lokalnie. Ponadto może być lesie z wieloma domenami.

Z perspektywy usługi Active Directory maszyny Wirtualnej Azure jest podobne do innych maszyn wirtualnych w nowoczesnych obsługiwanej funkcji hypervisor. Główna różnica z lokalnymi funkcji hypervisor jest dostępnej na platformie Azure jest konsoli maszyny Wirtualnej. Konsola jest wymagana dla niektórych scenariuszy, takich jak odzyskiwanie przy użyciu kopii zapasowej typu odzyskiwania systemu operacyjnego systemu od zera (BMR). Jednak przywrócenie maszyny Wirtualnej z magazynu kopii zapasowych nie zastępuje pełnego odzyskiwania systemu od ZERA. Active Directory przywracania trybu (DSRM) jest również dostępna, więc działało wszystkie scenariusze odzyskiwania usługi Active Directory. Aby uzyskać więcej informacji w tle, sprawdź, czy [kopia zapasowa i przywracanie zagadnienia dotyczące zwirtualizowanych kontrolerów domeny](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) i [planowanie odzyskiwanie lasu usługi Active Directory](https://technet.microsoft.com/en-us/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Pojedynczy kontroler domeny w jednej domenie
Można przywrócić maszyny Wirtualnej (na przykład innych maszyn wirtualnych) z platformy Azure portalu lub przy użyciu programu PowerShell.

### <a name="multiple-dcs-in-a-single-domain"></a>Wiele kontrolerów domeny w jednej domenie
Jeśli inne kontrolery domeny w tej samej domeny, można połączyć się za pośrednictwem sieci, takich jak żadnej maszyny Wirtualnej można przywrócić kontrolera domeny. Jeśli jest ostatniego pozostałego kontrolera domeny w domenie lub odzyskiwania w sieci izolowanej jest wykonywane, musi następować procedury odzyskiwania lasu.

### <a name="multiple-domains-in-one-forest"></a>Wiele domen w jednym lesie
Jeśli inne kontrolery domeny w tej samej domeny, można połączyć się za pośrednictwem sieci, takich jak żadnej maszyny Wirtualnej można przywrócić kontrolera domeny. Jednak w innych przypadkach zaleca się przywrócenie lasu.

<!--- WK: this following original supportability statement is incorrect, taking it out.
The challenge arises because DSRM mode is not present in Azure. So to restore such a VM, you cannot use the Azure portal. The only supported restore mechanism is disk-based restore using PowerShell.

> [!WARNING]
> For Domain Controller VMs in a multi-DC environment, do not use the Azure portal for restore! Only PowerShell based restore is supported
>
>

Read more about the [USN rollback problem](https://technet.microsoft.com/library/dd363553) and the strategies suggested to fix it.
--->

## <a name="restoring-vms-with-special-network-configurations"></a>Przywracanie maszyn wirtualnych z konfiguracjami sieci specjalne
Kopia zapasowa Azure obsługuje kopie zapasowe na następujących konfiguracji specjalnych sieci maszyn wirtualnych.

* Maszyn wirtualnych w usłudze równoważenia obciążenia (wewnętrznych i zewnętrznych)
* Maszyny wirtualne z wielu zastrzeżonych adresów IP
* Maszyny wirtualne z wieloma kartami sieciowymi

Te konfiguracje wprowadzić następujące zagadnienia dotyczące podczas przywracania ich.

> [!TIP]
> Użyj przepływu przywracania środowiska PowerShell, na podstawie odtworzyć konfiguracji sieci specjalne przywracania post maszyn wirtualnych.
>
>

### <a name="restoring-from-the-ui"></a>Przywracanie w interfejsie użytkownika:
Podczas przywracania z interfejsu użytkownika, **zawsze wybierz nową usługę w chmurze**. Należy pamiętać, że ponieważ portal przyjmuje tylko obowiązkowe parametry podczas przywracania przepływu, maszyn wirtualnych przywrócony przy użyciu interfejsu użytkownika zostaną utracone konfiguracji sieci specjalne, które posiadają. Innymi słowy, przywracanie maszyn wirtualnych będzie normalne maszyn wirtualnych bez konfiguracji usługi równoważenia obciążenia lub wielu kart Sieciowych lub wielu zastrzeżonego adresu IP.

### <a name="restoring-from-powershell"></a>Przywracanie z programu PowerShell:
PowerShell ma możliwość po prostu przywrócić dysków maszyny Wirtualnej z kopii zapasowej i nie utworzyć maszyny wirtualnej. Jest to przydatne podczas przywracania maszyn wirtualnych, które wymagają konfiguracji sieci specjalnych wymienionych powyżej.

Aby w pełni odtworzyć dysków przywracania post maszyny wirtualnej, wykonaj następujące kroki:

1. Przywróć dysków z magazynu kopii zapasowych za pomocą [kopii zapasowej programu Azure PowerShell](backup-azure-vms-classic-automation.md#restore-an-azure-vm)
2. Tworzenie konfiguracji maszyny Wirtualnej wymagana dla usługi równoważenia obciążenia / wielu kart interfejsu Sieciowego/wiele zastrzeżonego adresu IP za pomocą poleceń cmdlet programu PowerShell i użyj go w celu utworzenia maszyny Wirtualnej z Konfiguracja żądanego.

   * Tworzenie maszyny Wirtualnej w usłudze w chmurze z [wewnętrznego modułu równoważenia obciążenia](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Tworzenie maszyny Wirtualnej, aby nawiązać połączenie [internetowy modułu równoważenia obciążenia](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * Tworzenie maszyny Wirtualnej z [wiele kart sieciowych](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * Tworzenie maszyny Wirtualnej z [wielu zastrzeżone adresy IP](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Następne kroki
* [Rozwiązywanie problemów z błędami](backup-azure-vms-troubleshoot.md#restore)
* [Zarządzanie maszynami wirtualnymi](backup-azure-manage-vms.md)
