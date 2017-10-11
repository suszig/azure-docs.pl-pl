---
title: "Zarządzanie i monitorowanie kopii zapasowych maszyn wirtualnych platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zarządzanie nimi oraz monitorowanie kopii zapasowych maszyny wirtualnej platformy Azure"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 4372944e-d33a-4f6a-bd5f-d04af2842713
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: trinadhk;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d876bb1759600fa29a26730bfa8b4ec19db1e442
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-the-classic-portal"></a>Zarządzanie typowe zadania kopia zapasowa Azure i alerty wyzwalacza w klasycznym portalu
> [!div class="op_single_selector"]
> * [Zarządzanie kopiami zapasowymi maszyny Wirtualnej Azure](backup-azure-manage-vms.md)
> * [Zarządzanie kopiami zapasowymi klasyczne maszyny Wirtualnej](backup-azure-manage-vms-classic.md)
>
>

Ten artykuł zawiera informacje o wspólnego zarządzania i monitorowania zadań związanych z klasycznego modelu maszyny wirtualne chronione na platformie Azure.  

> [!NOTE]
> Platforma Azure ma dwa modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [Resource Manager i model klasyczny](../azure-resource-manager/resource-manager-deployment-model.md). Zobacz [przygotowania środowiska do tworzenia kopii zapasowych maszyn wirtualnych platformy Azure](backup-azure-vms-prepare.md) szczegółowe informacje na temat pracy z Classic deployment model maszyn wirtualnych.
>
> [!IMPORTANT]
>Począwszy od marca 2017 r. nie można już tworzyć magazynów kopii zapasowych za pomocą klasycznego portalu.
>
> Magazyny kopii zapasowych możesz teraz uaktualnić do magazynów usługi Recovery Services. Więcej szczegółów znajduje się w artykule [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uaktualnianie magazynu kopii zapasowych do magazynu usługi Recovery Services). Firma Microsoft zachęca do przeprowadzenia uaktualnienia magazynów kopii zapasowych do magazynów usługi Recovery Services.<br/> Po 15 października 2017 r. nie będzie można tworzyć magazynów kopii zapasowych za pomocą programu PowerShell. **Do 1 listopada 2017 r.**:
>- Wszystkie pozostałe magazyny kopii zapasowych zostaną automatycznie uaktualnione do magazynów usługi Recovery Services.
>- Nie będzie możliwe uzyskanie dostępu do danych kopii zapasowych w portalu klasycznym. Zamiast tego należy użyć witryny Azure Portal, aby uzyskać dostęp do danych kopii zapasowych w magazynach usługi Recovery Services.

## <a name="manage-protected-virtual-machines"></a>Zarządzanie chronione maszyny wirtualne
Aby zarządzać chronionych maszyn wirtualnych:

1. Do wyświetlania i zarządzania kliknij polecenie ustawienia kopii zapasowych dla maszyny wirtualnej **chronione elementy** kartę.
2. Kliknij nazwę chronionego elementu, aby wyświetlić **szczegóły kopii zapasowej** kartę, która pokazuje informacje dotyczące ostatniej kopii zapasowej.

    ![Kopia zapasowa maszyny wirtualnej](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. Do wyświetlania i zarządzania ustawień zasad tworzenia kopii zapasowej dla maszyny wirtualnej, kliknij **zasady** kartę.

    ![Zasady maszyny wirtualnej](./media/backup-azure-manage-vms/manage-policy-settings.png)

    **Zasady tworzenia kopii zapasowej** karta zawiera istniejące zasady. Można zmodyfikować zgodnie z potrzebami. Jeśli musisz utworzyć nowy kliknij zasady **Utwórz** na **zasady** strony. Należy zauważyć, że jeśli chcesz usunąć zasadę go nie powinien skojarzone z nią żadne maszyny wirtualne.

    ![Zasady maszyny wirtualnej](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. Możesz też uzyskać więcej informacji na temat akcji lub stan maszyny wirtualnej **zadania** strony. Kliknij zadanie na liście, aby uzyskać więcej informacji, lub filtrowania zadań dla określonej maszyny wirtualnej.

    ![Zadania](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a>Na żądanie kopii zapasowej maszyny wirtualnej
Skonfigurowane dla ochrony na żądanie można wykonać kopii zapasowej maszyny wirtualnej. Jeśli trwa oczekiwanie na początkową kopię zapasową maszyny wirtualnej kopii zapasowej na żądanie utworzy pełną kopię maszyny wirtualnej w magazynie kopii zapasowej Azure. Jeśli pierwsza kopia zapasowa została wykonana, na żądanie kopii zapasowej będą tylko do wysyłania zmiany z poprzedniej kopii zapasowej kopia zapasowa Azure tj. Magazyn go mają zawsze wartości rosnące.

> [!NOTE]
> Zakres przechowywania kopii zapasowej na żądanie ma ustawioną wartość przechowywania określona dla przechowywania codziennych w zasadach tworzenia kopii zapasowej odpowiadającej Maszynie wirtualnej.  
>
>

Aby wykonać kopię zapasową maszyny wirtualnej na żądanie:

1. Przejdź do **chronione elementy** i wybrać opcję **maszyny wirtualnej Azure** jako **typu** (jeśli jeszcze nie został wybrany) i wybierz polecenie **wybierz** przycisku.

    ![Typu maszyny Wirtualnej](./media/backup-azure-manage-vms/vm-type.png)
2. Wybierz maszynę wirtualną, na którym chcesz wykonać kopii zapasowej na żądanie i kliknij przycisk **Utwórz kopię zapasową teraz** u dołu strony.

    ![Wykonaj kopię zapasową teraz](./media/backup-azure-manage-vms/backup-now.png)

    Spowoduje to utworzenie zadania tworzenia kopii zapasowej na wybranej maszynie wirtualnej. Zakres przechowywania punktu odzyskiwania został utworzony za pomocą tego zadania będzie taka sama jak określona w zasadach skojarzoną z maszyną wirtualną.

    ![Tworzenie zadania tworzenia kopii zapasowej](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > Aby wyświetlić zasady skojarzone z maszyną wirtualną, przejdź do szczegółów maszyny wirtualnej w **chronione elementy** strony i przejdź do karty zasad tworzenia kopii zapasowej.
   >
   >
3. Po utworzeniu zadania można kliknąć **zadania** na pasku wyskakującego powiadomienia, aby wyświetlić odpowiedniego zadania na stronie zadań.

    ![Utworzono zadanie kopii zapasowej](./media/backup-azure-manage-vms/created-job.png)
4. Po pomyślnym ukończeniu zadania punktu odzyskiwania zostanie utworzona służące do przywrócenia maszyny wirtualnej. To powoduje również zwiększenie wartości kolumny punktu odzyskiwania przez 1 w **chronione elementy** strony.

## <a name="stop-protecting-virtual-machines"></a>Zatrzymaj ochronę maszyn wirtualnych
Można wybrać zatrzymać wykonanie kolejnych kopii zapasowych maszyny wirtualnej z następujących opcji:

* Zachowaj dane kopii zapasowej skojarzonego z maszyną wirtualną w magazynie usługi Kopia zapasowa Azure
* Usuwanie danych kopii zapasowej skojarzonego z maszyną wirtualną

Jeśli wybrano opcję zachowania danych kopii zapasowej skojarzonego z maszyną wirtualną, można użyć danych kopii zapasowej do przywrócenia maszyny wirtualnej. O cenach szczegóły takich maszyn wirtualnych, kliknij przycisk [tutaj](https://azure.microsoft.com/pricing/details/backup/).

Aby zatrzymać ochronę dla maszyny wirtualnej:

1. Przejdź do **chronione elementy** i wybrać opcję **maszyny wirtualnej platformy Azure** jako typ filtru (jeśli jeszcze nie został wybrany) i kliknięcie **wybierz** przycisku.

    ![Typu maszyny Wirtualnej](./media/backup-azure-manage-vms/vm-type.png)
2. Wybierz maszynę wirtualną, a następnie kliknij pozycję **Zatrzymaj ochronę** w dolnej części strony.

    ![Zatrzymaj ochronę](./media/backup-azure-manage-vms/stop-protection.png)
3. Domyślnie program Azure Backup nie powoduje usunięcia danych kopii zapasowej skojarzonego z maszyną wirtualną.

    ![Upewnij się, Zatrzymaj ochronę](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    Jeśli chcesz usunąć dane kopii zapasowej, zaznacz pole wyboru.

    ![Pole wyboru](./media/backup-azure-manage-vms/checkbox.png)

    Wybierz przyczynę zatrzymywanie tworzenia kopii zapasowej. Jest to opcjonalne, podania przyczyny pomoże kopia zapasowa Azure do pracy w opinii i ustalić ich priorytety scenariusze klienta.
4. Polecenie **przesyłania** przycisk, aby przesłać **Zatrzymaj ochronę** zadania. Polecenie **zadania** Aby wyświetlić odpowiednie zadanie w **zadania** strony.

    ![Zatrzymaj ochronę](./media/backup-azure-manage-vms/stop-protect-success.png)

    Jeśli nie wybrano **Usuń skojarzone dane kopii zapasowej** opcję podczas **Zatrzymaj ochronę** kreatora, a następnie po zakończeniu zadania, zmiany stanu ochrony **ochrony zatrzymana**. Dane pozostaną w usłudze Kopia zapasowa Azure dopóki zostanie jawnie usunięty. Zawsze można usunąć dane, wybierając maszynę wirtualną w **chronione elementy** strony i klikając **usunąć**.

    ![Zatrzymania ochrony](./media/backup-azure-manage-vms/protection-stopped-status.png)

    Jeśli wybrano **Usuń skojarzone dane kopii zapasowej** opcji, maszyna wirtualna nie będzie częścią **chronione elementy** strony.

## <a name="re-protect-virtual-machine"></a>Ponownie włączyć ochronę maszyny wirtualnej
Jeśli nie wybrano **Skojarz dane kopii zapasowej usunięcia** opcji w **Zatrzymaj ochronę**, można ponownie włączyć ochronę maszyny wirtualnej, wykonując kroki podobne do tworzenia kopii zapasowej zarejestrowanych maszyn wirtualnych. Gdy chroniony, tej maszyny wirtualnej zostanie zatrzymane przed Zatrzymaj ochronę danych kopii zapasowej i punkty odzyskiwania utworzone po ponownie ochronę.

Po ponownego włączenia ochrony stanu ochrony maszyny wirtualnej zostanie zmieniony na **chronione** jeżeli istnieją punkty odzyskiwania, używając programu starszego niż **Zatrzymaj ochronę**.

  ![Maszyny Wirtualnej przełączonej](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> Jeśli ponownie ochronę maszyny wirtualnej, można określić różnych zasad niż zasady, z którą maszyny wirtualnej został początkowo chronić.
>
>

## <a name="unregister-virtual-machines"></a>Wyrejestruj maszyny wirtualne
Jeśli chcesz usunąć maszynę wirtualną z magazynu kopii zapasowych:

1. Polecenie **UNREGISTER** u dołu strony.

    ![Wyłącz ochronę](./media/backup-azure-manage-vms/unregister-button.png)

    Powiadomienie wyskakujące będą wyświetlane w dolnej części ekranu żądania potwierdzenia. Kliknij przycisk **tak** aby kontynuować.

    ![Wyłącz ochronę](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a>Usuwanie danych kopii zapasowej
Można usunąć danych kopii zapasowej skojarzonego z maszyną wirtualną, albo:

* Podczas zadanie zatrzymania ochrony
* Po Zatrzymaj ochronę ukończenia zadania na maszynie wirtualnej

Usuwanie kopii zapasowej danych na maszynie wirtualnej, który znajduje się w *ochrony zatrzymana* stanu po pomyślnym zarejestrowaniu **zatrzymać kopii zapasowej** zadania:

1. Przejdź do **chronione elementy** i wybrać opcję **maszyny wirtualnej Azure** jako *typu* i kliknij przycisk **wybierz** przycisku.

    ![Typu maszyny Wirtualnej](./media/backup-azure-manage-vms/vm-type.png)
2. Wybierz maszynę wirtualną. Maszyna wirtualna znajduje się w **ochrony zatrzymana** stanu.

    ![Ochrona zatrzymana](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. Kliknij przycisk **usunąć** u dołu strony.

    ![Usuwanie kopii zapasowej](./media/backup-azure-manage-vms/delete-backup.png)
4. W **usunąć danych kopii zapasowej** kreatora wybierz przyczynę usuwanie danych kopii zapasowej (zdecydowanie zalecane) i kliknij przycisk **przesyłania**.

    ![Usuwanie danych kopii zapasowej](./media/backup-azure-manage-vms/delete-backup-data.png)
5. Spowoduje to utworzenie zadanie do usunięcia danych kopii zapasowej z wybraną maszynę wirtualną. Kliknij przycisk **zadania** wyświetlić odpowiedniego zadania na stronie zadań.

    ![Pomyślnie usunięto danych](./media/backup-azure-manage-vms/delete-data-success.png)

    Po zakończeniu zadania wpis odpowiadający maszyny wirtualnej zostanie usunięte z **chronione elementy** strony.

## <a name="dashboard"></a>Pulpit nawigacyjny
Na **pulpitu nawigacyjnego** stronie można przejrzeć informacje o maszynach wirtualnych platformy Azure, Magazyn i zadań skojarzonych z nimi w ciągu ostatnich 24 godzin. Można wyświetlić stanu kopii zapasowej oraz wszelkie skojarzone błędy tworzenia kopii zapasowej.

![Pulpit nawigacyjny](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> Wartości na pulpicie nawigacyjnym są odświeżane co 24 godziny.
>
>

## <a name="auditing-operations"></a>Operacje inspekcji
Kopia zapasowa Azure udostępnia przegląd "dzienniki operacji" operacje tworzenia kopii zapasowej wyzwalane przez klienta, co ułatwia Zobacz dokładnie jakie operacje zarządzania były wykonywane w magazynie kopii zapasowych. Dzienniki operacji włączyć doskonałe których post i inspekcji obsługę operacji tworzenia kopii zapasowej.

Następujące operacje są rejestrowane w dziennikach operacji:

* Zarejestruj subskrypcję
* Unregister
* Konfigurowanie ochrony
* Kopia zapasowa (zarówno zaplanowanych oraz kopii zapasowej na żądanie za pomocą BackupNow)
* Przywracanie
* Zatrzymaj ochronę
* Usuwanie danych kopii zapasowej
* Dodawanie zasad
* Usuń zasady
* Zaktualizuj zasady
* Anulowanie zadania

Aby wyświetlić dzienniki operacji odpowiadający magazynu kopii zapasowych:

1. Przejdź do **usług zarządzania** w portalu Azure, a następnie kliknij przycisk **dzienniki operacji** kartę.

    ![Dzienniki operacji](./media/backup-azure-manage-vms/ops-logs.png)
2. Filtry, wybierz **kopii zapasowej** jako *typu* i określ nazwę magazynu kopii zapasowych w *nazwa usługi* i wybierz polecenie **przesyłania**.

    ![Filtr dzienniki operacji](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. W dziennikach Operacje wybierz żadnej operacji, a następnie kliknij przycisk **szczegóły** Aby wyświetlić szczegóły odpowiadającej operacji.

    ![Szczegóły pobierania dzienników operacji](./media/backup-azure-manage-vms/ops-logs-details.png)

    **Kreatora szczegóły** zawiera informacje na temat operacji wyzwoleniu zadania. identyfikator zasobu, na którym ta operacja zostanie wywołany i godzina rozpoczęcia operacji.

    ![Szczegóły operacji](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a>Powiadomienia o alertach
W portalu można uzyskać powiadomienia o alertach niestandardowych dla zadania. Jest to osiągane przez zdefiniowanie opartych na środowisku PowerShell reguł alertów na operacyjne dzienniki zdarzeń. Firma Microsoft zaleca używanie *środowiska PowerShell w wersji 1.3.0 lub nowszej*.

Aby zdefiniować niestandardowe powiadomienie powiadamiania w przypadku niepowodzenia tworzenia kopii zapasowej, przykładowe polecenie będzie wyglądać:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

**ResourceId**: można uzyskać tę z menu podręczne dzienniki operacji zgodnie z opisem w powyżej sekcji. ResourceUri w oknie podręcznym szczegóły operacji jest ResourceId mają być dostarczone dla tego polecenia cmdlet.

**OperationName**: są to w formacie "Microsoft.Backup/backupvault/<EventName>" w przypadku, gdy EventName jest jednym z rejestru, Unregister, ConfigureProtection, kopia zapasowa, Przywróć StopProtection, DeleteBackupData, CreateProtectionPolicy, DeleteProtectionPolicy, UpdateProtectionPolicy

**Stan**: obsługiwane wartości są Started zakończyło się pomyślnie, a nie powiodła się.

**Grupa zasobów**: Grupa zasobów zasobu, na którym zostanie wywołany operacji. Możesz go uzyskać z ResourceId wartość. Wartość między polami */resourceGroups/* i */providers/* w ResourceId wartość jest wartością grupa zasobów.

**Nazwa**: Nazwa reguły alertów.

**CustomEmail**: Określ adres e-mail niestandardowe, do którego chcesz wysłać powiadomień o alertach

**SendToServiceOwners**: Ta opcja wysyła powiadomień o alertach do wszystkich administratorów i współadministratorów subskrypcji. Mogą być używane w **AzureRmAlertRuleEmail nowy** polecenia cmdlet

### <a name="limitations-on-alerts"></a>Ograniczenia dotyczące alertów
Alerty na podstawie zdarzenia są poddawane następujące ograniczenia:

1. Alerty są wyzwalane na wszystkich maszynach wirtualnych w magazynie kopii zapasowych. Nie można dostosować, aby otrzymywać alerty dotyczące określonej grupy maszyn wirtualnych w magazynie kopii zapasowych.
2. Ta funkcja jest dostępna w wersji zapoznawczej. [Dowiedz się więcej](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Będą wysyłane alerty z "alerts-noreply@mail.windowsazure.com". Obecnie nie można zmodyfikować nadawcą wiadomości e-mail.

## <a name="next-steps"></a>Następne kroki
* [Przywracanie maszyny wirtualne platformy Azure](backup-azure-restore-vms.md)
