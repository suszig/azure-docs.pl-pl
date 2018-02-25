---
title: Monitoruj alerty kopii zapasowej maszyn wirtualnych platformy Azure | Dokumentacja firmy Microsoft
description: "Monitorowanie zdarzeń i alertów z zadań tworzenia kopii zapasowej maszyny wirtualnej platformy Azure. Wyślij wiadomość e-mail na podstawie alertów."
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: fed32015-2db2-44f8-b204-d89f6fd1bea2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: markgal;trinadhk;giridham;
ms.openlocfilehash: 0f92d63d6eabe48bfac444b79bc10e090df239ca
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Monitorowanie alertów związanych z kopiami zapasowymi maszyny wirtualnej platformy Azure
Alerty są odpowiedzi z usługi, czy próg zdarzenia zostały spełnione lub przekroczenia. Uzyskiwanie informacji o tym, gdy rozpoczęcia problemów może być krytyczne zachowaniu niskich kosztów biznesowych. Alerty zwykle nie występują zgodnie z harmonogramem, a więc warto wiedzieć, jak najszybciej po wystąpieniu alerty. Na przykład gdy zadanie tworzenia kopii zapasowej lub przywracania nie powiodło się, alert występuje w ciągu pięciu minut błędu. Na pulpicie nawigacyjnym magazynu w kafelku alerty kopii zapasowej Wyświetla zdarzeń krytycznych i poziom ostrzeżeń. W ustawieniach alerty kopii zapasowej można wyświetlić wszystkie zdarzenia. Ale co zrobić, jeśli alarm występuje, gdy pracujesz na oddzielnych problem? Jeśli nie znasz w przypadku alertu, może to być pomocnicza niedogodności lub może naruszyć bezpieczeństwo danych. Aby upewnić się, że osobom potrafią alertu — Jeśli występuje on, należy skonfigurować usługę, aby wysłać powiadomienia o alertach pocztą e-mail. Aby uzyskać więcej informacji na temat konfigurowania powiadomień e-mail, zobacz [skonfigurować powiadomienia](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-the-alerts"></a>Jak znaleźć informacje o alertach?
Aby wyświetlić informacje o zdarzeniu, która wywołała alert, należy otworzyć blok alerty kopii zapasowej. Istnieją dwa sposoby, aby otworzyć blok alerty kopii zapasowej: z alerty kopii zapasowej kafelka na pulpicie nawigacyjnym magazynu lub za pomocą bloku alertów i zdarzeń.

Aby otworzyć blok alerty kopii zapasowej w kafelku alerty kopii zapasowej:

* Na **alerty kopii zapasowej** kafelka na pulpicie nawigacyjnym magazynu, kliknij przycisk **krytyczny** lub **ostrzeżenie** można wyświetlać zdarzenia operacyjne dla tego poziomu ważności.

    ![Kafelek alerty kopii zapasowej](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

Aby otworzyć blok alerty kopii zapasowej za pomocą bloku alerty i zdarzenia:

1. Na pulpicie nawigacyjnym magazynu, kliknij **wszystkie ustawienia**. ![Przycisk wszystkie ustawienia](./media/backup-azure-monitor-vms/all-settings-button.png)
2. Na **ustawienia** bloku, kliknij przycisk **alerty i zdarzenia**. ![Przycisk alertów i zdarzeń](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. Na **alerty i zdarzenia** bloku, kliknij przycisk **alerty kopii zapasowej**. ![Przycisk tworzenia kopii zapasowej alertów](./media/backup-azure-monitor-vms/backup-alerts.png)

    **Alerty kopii zapasowej** bloku otwiera i wyświetla filtrowane alerty.

    ![Kafelek alerty kopii zapasowej](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. Aby wyświetlić szczegółowe informacje na temat określonego alertu, z listy zdarzeń, kliknij alert, aby otworzyć jego **szczegóły** bloku.

    ![Szczegół zdarzenia](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    Aby dostosować wyświetlane na liście atrybutów, zobacz [wyświetlić atrybuty dodatkowe zdarzenia](backup-azure-monitor-vms.md#view-additional-event-attributes)

## <a name="configure-notifications"></a>Konfigurowanie powiadomień
 Można skonfigurować usługę do wysyłania powiadomień e-mail dla alertów, które wystąpiły w ciągu ostatnich godzin lub po wystąpieniu określonych typach zdarzeń.

Aby skonfigurować powiadomienia e-mail dla alertów

1. W menu alerty kopii zapasowej, kliknij przycisk **skonfigurować powiadomienia**

    ![Menu alerty kopii zapasowej](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    Zostanie otwarty blok powiadomienia Konfigurowanie.

    ![Konfigurowanie powiadomień bloku](./media/backup-azure-monitor-vms/configure-notifications.png)
2. W bloku powiadomienia Konfigurowanie powiadomień E-mail kliknij **na**.

    Adresatów i ważność okien dialogowych mieć gwiazdką, ponieważ te informacje są wymagane. Podaj co najmniej jeden adres e-mail, a następnie wybierz co najmniej jeden ważność.
3. W **adresatów (Poczta E-mail)** okna dialogowego, wpisz adresy e-mail, na który otrzymywać powiadomienia. Użyj formatu: username@domainname.com. Oddzielaj adresy e-mail średnikami (;).
4. W **powiadamiania** obszaru, wybierz **na Alert** można wysłać powiadomienia, gdy wystąpi określony alert, lub **co godzinę szyfrowanego** do wysłania podsumowanie dla ostatniej godziny.
5. W **ważność** okno dialogowe, wybierz co najmniej jeden poziom, które wyzwalacza powiadomienia e-mail.
6. Kliknij pozycję **Zapisz**.

   ### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Jakie typy alertów są dostępne dla kopii zapasowych maszyn wirtualnych IaaS platformy Azure?
   | Poziom alertu | Wysyłania alertów |
   | --- | --- |
   | Krytyczne | niepowodzenia tworzenia kopii zapasowej, niepowodzenia odzyskiwania |
   | Ostrzeżenie | w przypadku zadań tworzenia kopii zapasowej zakończyło się pomyślnie z ostrzeżeniami (np: Niektóre moduły zapisujące nie powiodło się podczas tworzenia migawki) |
   | Informacyjne | obecnie alerty informacyjne nie są dostępne dla kopii zapasowej maszyny Wirtualnej Azure |

### <a name="are-there-situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Czy występują sytuacje, w których wiadomość e-mail nie jest wysyłana, mimo że powiadomienia zostały skonfigurowane?
Istnieją sytuacje, w którym alert nie są wysyłane, nawet jeśli powiadomienia zostały prawidłowo skonfigurowane. W następujących sytuacjach wiadomości e-mail powiadomienia nie są wysyłane do uniknięcia szumu alertu:

* Jeśli skonfigurowano powiadomienia szyfrowanego co godzinę, a alert jest uruchamiany i rozpoznać w ciągu godziny.
* Zadanie zostało anulowane.
* Zadanie tworzenia kopii zapasowej zostanie wywołany, a następnie nie, a inne zadanie tworzenia kopii zapasowej jest w toku.
* Uruchamia zaplanowane zadanie tworzenia kopii zapasowej dla maszyny Wirtualnej z obsługą usługi Resource Manager, ale maszyna wirtualna już nie istnieje.

## <a name="customize-your-view-of-events"></a>Dostosowywanie widoku zdarzeń
**Dzienniki inspekcji** ustawienie zawiera zestaw wstępnie zdefiniowanych filtry i kolumny pokazywane są informacje zdarzeń operacyjnych. Można dostosować widok, tak że w przypadku **zdarzenia** zostanie otwarty blok, zawiera informacje mają.

1. W [pulpitem nawigacyjnym magazynu](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), Znajdź i kliknij przycisk **dzienników inspekcji** otworzyć **zdarzenia** bloku.

    ![Dzienniki inspekcji](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    **Zdarzenia** zostanie otwarty blok do zdarzenia operacyjne filtrowane tylko dla bieżącego magazynu.

    ![Filtr dzienniki inspekcji](./media/backup-azure-monitor-vms/audit-logs-filter.png)

    Blok z listą krytyczny, błąd, ostrzeżenie i zdarzenia informacyjne, które wystąpiły w ostatnim tygodniu. Przedział czasu jest domyślnie ustawiony w **filtru**. **Zdarzenia** blok zawiera także wykres słupkowy śledzenie po wystąpieniu zdarzenia. Jeśli nie chcesz wyświetlić na wykresie słupkowym w **zdarzenia** menu, kliknij przycisk **wykresu Ukryj** Aby przełączyć poza wykresem. Domyślny widok zdarzenia zawiera informacje o operacji, poziom, stan, zasobów i czasu. Aby dowiedzieć się, jak udostępnianie dodatkowe atrybuty zdarzeń, zobacz sekcję [rozszerzanie informacji o zdarzeniu](backup-azure-monitor-vms.md#view-additional-event-attributes).
2. Aby uzyskać dodatkowe informacje dotyczące zdarzenia operacyjne w **operacji** kolumny, kliknij przycisk zdarzeń operacyjnych, aby otworzyć jego blok. Blok zawiera szczegółowe informacje o zdarzeniach. Zdarzenia są pogrupowane według ich identyfikator korelacji i listę zdarzeń, które wystąpiły w przedziale czasu.

    ![Szczegóły operacji](./media/backup-azure-monitor-vms/audit-logs-details-window.png)
3. Aby wyświetlić szczegółowe informacje dotyczące konkretnego zdarzenia, z listy zdarzeń, kliknij zdarzenie, aby otworzyć jego **szczegóły** bloku.

    ![Szczegół zdarzenia](./media/backup-azure-monitor-vms/audit-logs-details-window-deep.png)

    Informacje o poziom zdarzenia są szczegółowych informacji. Jeśli preferowane jest wyświetlany tyle informacje o każdym zdarzeniu i chcesz dodać tyle szczegółów **zdarzenia** bloku, zobacz sekcję [rozszerzanie informacji o zdarzeniu](backup-azure-monitor-vms.md#view-additional-event-attributes).

## <a name="customize-the-event-filter"></a>Dostosowywanie filtr zdarzeń
Użyj **filtru** dostosowanie lub wybrać informacje wyświetlane w szczególności bloku. Aby filtrować dane zdarzenia:

1. W [pulpitem nawigacyjnym magazynu](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), Znajdź i kliknij przycisk **dzienników inspekcji** otworzyć **zdarzenia** bloku.

    ![Dzienniki inspekcji](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    **Zdarzenia** zostanie otwarty blok do zdarzenia operacyjne filtrowane tylko dla bieżącego magazynu.

    ![Filtr dzienniki inspekcji](./media/backup-azure-monitor-vms/audit-logs-filter.png)
2. Na **zdarzenia** menu, kliknij przycisk **filtru** do otwarcia tego bloku.

    ![otwarcie bloku filtru](./media/backup-azure-monitor-vms/audit-logs-filter-button.png)
3. Na **filtru** bloku, Dostosuj **poziom**, **przedział czasu**, i **wywołującego** filtrów. Inne filtry nie są dostępne, ponieważ była równa aktualnymi informacjami dla magazynu usług odzyskiwania.

    ![Szczegóły zapytania dzienniki inspekcji](./media/backup-azure-monitor-vms/filter-blade.png)

    Można określić **poziom** zdarzenia: krytyczny, błąd, ostrzeżenie lub komunikat informacyjny. Można wybrać dowolną kombinację poziomów zdarzeń, ale musisz mieć co najmniej jeden wybrany poziom. Przełącz poziom lub wyłączyć. **Przedział czasu** filtru pozwala określić czas dla przechwytywania zdarzeń. Jeśli używasz niestandardowego przedział czasu, można ustawić godziny rozpoczęcia i zakończenia.
4. Gdy wszystko będzie gotowe do zapytania przy użyciu filtru dzienniki operacji, kliknij przycisk **aktualizacji**. Wyniki są wyświetlane w **zdarzenia** bloku.

    ![Szczegóły operacji](./media/backup-azure-monitor-vms/edited-list-of-events.png)

### <a name="view-additional-event-attributes"></a>Widok zdarzeń dodatkowe atrybuty
Przy użyciu **kolumn** przycisku, można włączyć atrybuty dodatkowe zdarzenia są wyświetlane na liście na **zdarzenia** bloku. Domyślną listę zdarzeń wyświetla informacje dotyczące operacji, poziom, stan, zasobów i czasu. Aby włączyć dodatkowe atrybuty:

1. Na **zdarzenia** bloku, kliknij przycisk **kolumn**.

    ![Otwórz kolumn](./media/backup-azure-monitor-vms/audi-logs-column-button.png)

    **Wybierz kolumny** zostanie otwarty blok.

    ![Blok kolumn](./media/backup-azure-monitor-vms/columns-blade.png)
2. Aby wybrać atrybut, zaznacz pole wyboru. Pole wyboru atrybutu przełącza włączać i wyłączać.
3. Kliknij przycisk **zresetować** zresetować listę atrybutów w **zdarzenia** bloku. Po dodaniu lub usunięciu atrybutów z listy, użyj **zresetować** Aby wyświetlić nową listę atrybutów zdarzeń.
4. Kliknij przycisk **aktualizacji** , aby zaktualizować dane w przypadku atrybutów. Poniższa tabela zawiera informacje dotyczące każdego atrybutu.

| Nazwa kolumny | Opis |
| --- | --- |
| Operacja |Nazwa operacji |
| Poziom |Poziom operacji, możliwe wartości: informacyjny, ostrzeżenie, błąd lub krytyczny |
| Stan |Opis stanu operacji |
| Zasób |Adres URL identyfikujący zasób. znany również jako identyfikator zasobu |
| Time |Czas, od bieżącego czasu wystąpienia zdarzenia |
| Obiekt wywołujący |Kto lub co o nazwie lub wywołał zdarzenie; może to być system lub użytkownik |
| Sygnatura czasowa |Czas, gdy zdarzenie zostało wyzwolone |
| Grupa zasobów |Grupy zasobów |
| Typ zasobu |Wewnętrzny typ zasobu używany przez usługę Resource Manager |
| Identyfikator subskrypcji |Identyfikator subskrypcji skojarzone |
| Kategoria |Kategoria zdarzenia |
| Identyfikator korelacji |Identyfikator wspólnej dla zdarzenia powiązane |

## <a name="use-powershell-to-customize-alerts"></a>Dostosowywanie alertów za pomocą programu PowerShell
W portalu można uzyskać powiadomienia o alertach niestandardowych dla zadania. Aby uzyskać te zadania, należy zdefiniować opartych na środowisku PowerShell reguły alertów dla zdarzenia operacyjne dzienniki. Użyj *środowiska PowerShell w wersji 1.3.0 lub nowszym*.

Aby zdefiniować niestandardowe powiadomienie powiadamiania w przypadku niepowodzenia tworzenia kopii zapasowej, użyj polecenia, takich jak następujący skrypt:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.RecoveryServices/recoveryServicesVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/Microsoft.RecoveryServices/vaults/trinadhVault -Actions $actionEmail
```

**ResourceId** : ResourceId można uzyskać z dzienników inspekcji. Element ResourceId jest adres URL podany w kolumnie zasobów dzienników operacji.

**OperationName** : OperationName jest w formacie "Microsoft.RecoveryServices/recoveryServicesVault/*EventName*" gdzie *EventName* może być:<br/>

* Zarejestruj subskrypcję <br/>
* Unregister <br/>
* ConfigureProtection <br/>
* Backup <br/>
* Przywracanie <br/>
* StopProtection <br/>
* DeleteBackupData <br/>
* CreateProtectionPolicy <br/>
* DeleteProtectionPolicy <br/>
* UpdateProtectionPolicy <br/>

**Stan** : obsługiwane wartości to rozpoczęte, Powodzenie lub niepowodzenie.

**Grupa zasobów** : jest to grupa zasobów, do której należy dany zasób. Można dodać kolumny grupy zasobów, do wygenerowanych dzienników. Grupa zasobów jest jednym z dostępnych typów informacji o zdarzeniu.

**Nazwa** : Nazwa reguły alertów.

**CustomEmail** : Określ adres e-mail niestandardowe, do którego chcesz wysłać powiadomień o alertach

**SendToServiceOwners** : Ta opcja wysyła powiadomienia o alertach do wszystkich administratorów i współadministratorów subskrypcji. Mogą być używane w **AzureRmAlertRuleEmail nowy** polecenia cmdlet

### <a name="limitations-on-alerts"></a>Ograniczenia dotyczące alertów
Alerty oparte na zdarzeniu obowiązują następujące ograniczenia:

1. Alerty są wyzwalane na wszystkich maszynach wirtualnych w magazynie usług odzyskiwania. Nie można dostosować alert dotyczący podzbiór maszyn wirtualnych w magazynie usług odzyskiwania.
2. Ta funkcja jest dostępna w wersji zapoznawczej. [Dowiedz się więcej](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Alerty są wysyłane z "alerts-noreply@mail.windowsazure.com". Obecnie nie można zmodyfikować nadawcą wiadomości e-mail.

## <a name="next-steps"></a>Kolejne kroki
Dzienniki zdarzeń włączyć doskonałe których post i inspekcji obsługę operacji tworzenia kopii zapasowej. Rejestrowane są następujące operacje:

* Zarejestruj subskrypcję
* Unregister
* Konfigurowanie ochrony
* Kopia zapasowa (zarówno zaplanowanych oraz kopii zapasowej na żądanie)
* Przywracanie
* Zatrzymaj ochronę
* Usuwanie danych kopii zapasowej
* Dodawanie zasad
* Usuń zasady
* Aktualizowanie zasad
* Anuluj zadanie

Szerokie opis zdarzenia, operacje i dzienniki inspekcji dla usług Azure, zapoznaj się z artykułem [wyświetlania zdarzeń i dzienniki inspekcji](../monitoring-and-diagnostics/insights-debugging-with-events.md).

Informacje dotyczące ponownego tworzenia maszyny wirtualnej z punktu odzyskiwania, zapoznaj się z [przywracanie maszyn wirtualnych Azure](backup-azure-arm-restore-vms.md). Jeśli potrzebujesz informacji na temat ochrony maszyn wirtualnych, zobacz [Pierwsze spojrzenie: Utwórz kopię zapasową maszyn wirtualnych do magazynu usług odzyskiwania](backup-azure-vms-first-look-arm.md). Więcej informacji na temat zadań zarządzania dla kopii zapasowych maszyn wirtualnych w artykule [kopii zapasowych maszyn wirtualnych Azure zarządzanie](backup-azure-manage-vms.md).
