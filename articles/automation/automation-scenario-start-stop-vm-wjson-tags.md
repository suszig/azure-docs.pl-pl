---
title: "Używaj tagów formacie JSON można zaplanować stan maszyny Wirtualnej platformy Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono sposób używania ciągów JSON w tagach można zautomatyzować, harmonogram uruchamiania maszyny Wirtualnej i zamykania."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 6afed5d2-e939-4749-8b2c-9312b4c16fb2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: magoedte;paulomarquesc
ms.openlocfilehash: 9855921f4a3aa9cda8497b400d50a186d7162dc3
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-to-create-a-schedule-for-azure-vm-startup-and-shutdown"></a>Scenariusz automatyzacji Azure: przy użyciu tagów formacie JSON, aby utworzyć harmonogram uruchamiania maszyny Wirtualnej platformy Azure i zamykania
Klienci często chcą harmonogramu uruchamiania i wyłączania maszyny wirtualne lub zmniejszyć koszty subskrypcji obsługi wymagań technicznych i biznesowych.

Poniższy scenariusz umożliwia konfigurowanie automatycznego uruchamiania i wyłączania maszyn wirtualnych przy użyciu tagu o nazwie harmonogram na poziomie grupy zasobów lub maszyny wirtualnej na platformie Azure. Ten harmonogram można skonfigurować od niedzieli do soboty czas uruchamiania i czasu zamykania.

Mamy niektóre opcje poza pole. Należą do nich:

* [Zestawy skalowania maszyny wirtualnej](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) z ustawieniami automatycznego skalowania, które umożliwiają skalowanie przychodzący lub wychodzący.
* [DevTest Labs](../devtest-lab/devtest-lab-overview.md) usługi, która ma wbudowaną funkcję tworzenia harmonogramu uruchamiania i wyłączania operacji.

Jednak te opcje obsługują tylko określonych scenariuszy i nie można zastosować do infrastruktury jako — usługa (IaaS), maszynach wirtualnych.

Harmonogram zostanie zastosowany do grupy zasobów, również jest stosowana do wszystkich maszyn wirtualnych w tej grupie zasobów. Jeśli harmonogram jest również bezpośrednio zastosowane do maszyny Wirtualnej, harmonogram ostatniego ma pierwszeństwo przed w następującej kolejności:

1. Harmonogram stosowane do grupy zasobów
2. Harmonogram zastosowane do maszyny wirtualnej w grupie zasobów i grupy zasobów
3. Harmonogram zastosowane do maszyny wirtualnej

W tym scenariuszu zasadniczo przyjmuje ciągu JSON w określonym formacie i dodaje go jako wartość tagu o nazwie harmonogramu. Następnie element runbook zawiera listę wszystkich grup zasobów i maszyn wirtualnych i identyfikuje harmonogramy dla każdej maszyny Wirtualnej w scenariuszach wymienione wcześniej w oparciu. Następnie maszyn wirtualnych, które mają harmonogramy dołączony w pętli i ocenia, jakie działania należy podjąć. Na przykład określają one, które maszyny wirtualne muszą zatrzymana, zamknięta lub ignorowane.

Te elementy runbook uwierzytelniania za pomocą [konta Uruchom jako platformy Azure](automation-sec-configure-azure-runas-account.md).

## <a name="download-the-runbooks-for-the-scenario"></a>Pobierz elementy runbook dla tego scenariusza
Ten scenariusz obejmuje cztery elementy runbook przepływu pracy programu PowerShell, które można pobrać z [galerii TechNet](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) lub [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) repozytorium dla tego projektu.

| Element Runbook | Opis |
| --- | --- |
| ResourceSchedule testu |Sprawdza harmonogram każdej maszyny wirtualnej, a następnie wykonuje zamykania lub uruchamiania w zależności od harmonogramu. |
| Dodaj ResourceSchedule |Dodaje tag harmonogramu do maszyny Wirtualnej lub grupy zasobów. |
| ResourceSchedule aktualizacji |Modyfikuje istniejące tag harmonogramu przez zamianę nowy. |
| Usuń ResourceSchedule |Usuwa harmonogram tagu z maszyny Wirtualnej lub grupy zasobów. |

## <a name="install-and-configure-this-scenario"></a>Instalowanie i konfigurowanie tego scenariusza
### <a name="install-and-publish-the-runbooks"></a>Instalowanie i publikowanie elementów Runbook
Po pobraniu elementy runbook, można je zaimportować za pomocą procedury w [Tworzenie lub importowanie elementu runbook automatyzacji Azure](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).  Każdy element runbook należy opublikować po jego został pomyślnie zaimportowany na koncie automatyzacji.

### <a name="add-a-schedule-to-the-test-resourceschedule-runbook"></a>Dodaj harmonogram do elementu runbook ResourceSchedule testu
Wykonaj następujące kroki, aby włączyć harmonogram ResourceSchedule testu elementu runbook. Jest to element runbook, który sprawdza maszyn wirtualnych powinny być uruchomiona, zamknij lub pozostawione tak jak jest.

1. W portalu Azure, otwórz Twoje konto usługi Automatyzacja, a następnie kliknij **elementów Runbook** kafelka.
2. Na **ResourceSchedule testu** bloku, kliknij przycisk **harmonogramy** kafelka.
3. Na **harmonogramy** bloku, kliknij przycisk **Dodaj harmonogram**.
4. Na **harmonogramy** bloku, wybierz opcję **Połącz harmonogram z elementem runbook**. Następnie wybierz **Utwórz nowy harmonogram**.
5. Na **nowy harmonogram** bloku, wpisz nazwę tego harmonogramu, na przykład: *HourlyExecution*.
6. Dla harmonogramu **Start**, ustawić czas rozpoczęcia przyrost godzinę.
7. Wybierz **cyklu**, a następnie **powtarzanie co interwał**, wybierz pozycję **1 godzina**.
8. Sprawdź, czy **ustawienia okresu ważności** ustawiono **nr**, a następnie kliknij przycisk **Utwórz** można zapisać nowego harmonogramu.
9. Na **Runbook harmonogram** bloku opcji wybierz **parametry i ustawienia uruchamiania**. W ResourceSchedule testu **parametry** bloku, wprowadź nazwę subskrypcji w **Nazwa subskrypcji** pola.  Jest to tylko parametr, który jest wymagany dla elementu runbook.  Po zakończeniu kliknij przycisk **OK**.

Po jego ukończeniu harmonogram powinna wyglądać następująco:

![Skonfigurowany ResourceSchedule testu elementu runbook](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-the-json-string"></a>Format ciągu JSON
To rozwiązanie zasadniczo ma JSON ciąg w określonym formacie i dodaje go jako wartość tagu o nazwie harmonogramu. Następnie element runbook zawiera listę wszystkich grup zasobów i maszyn wirtualnych i identyfikuje harmonogramy dla każdej maszyny wirtualnej.

Element runbook pętli maszyn wirtualnych, które mają harmonogramy dołączona i sprawdza, jakie działania należy podjąć. Poniżej przedstawiono przykładowy sposób formatowania rozwiązania:

```json
{
    "TzId": "Eastern Standard Time",
    "0": {
        "S": "11",
        "E": "17"
    },
    "1": {
        "S": "9",
        "E": "19"
    },
    "2": {
        "S": "9",
        "E": "19"
    },
}
```

Oto niektóre szczegółowe informacje na temat tej struktury:

1. Format tej struktury JSON jest zoptymalizowany do obejścia ograniczenia 256 znaków wartości jeden tag na platformie Azure.
2. *TzId* reprezentuje strefę czasową dla maszyny wirtualnej. Ten identyfikator można uzyskać za pomocą klasy .NET informacje o strefie czasowej w sesji programu PowerShell —**[System.TimeZoneInfo]:: GetSystemTimeZones()**.

   ![GetSystemTimeZones w programie PowerShell](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * Dni robocze są reprezentowane z wartością liczbową 0 do 6. Wartość zero jest równa niedzielę.
   * Godzina rozpoczęcia jest reprezentowana z **S** atrybutu i jego wartość jest w formacie 24-godzinnym.
   * Czas zakończenia lub zamknięcie odpowiada z **E** atrybutu i jego wartość jest w formacie 24-godzinnym.

     Jeśli **S** i **E** atrybuty każdego mają wartość zero (0), maszyna wirtualna znajdzie się w jej obecnym stanie w chwili oceny.
3. Jeśli chcesz pominąć oceny dla określonego dnia, tygodnia, nie dodawaj sekcji za ten dzień tygodnia. W poniższym przykładzie jest oceniane tylko poniedziałek i dni tygodnia, są ignorowane:

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a>Tag grupy zasobów lub maszyny wirtualne
Aby zamknąć maszyn wirtualnych, należy tagu maszyn wirtualnych lub grup zasobów, w których są przechowywane. Maszyny wirtualne, które nie mają znacznika harmonogramu nie są uwzględniane. W związku z tym nie jest uruchomiona lub zamknięta.

Istnieją dwa sposoby tag grupy zasobów lub maszyn wirtualnych w tym rozwiązaniu. Należy go bezpośrednio z portalu. Lub może używać ResourceSchedule Dodaj ResourceSchedule aktualizacji i Usuń ResourceSchedule elementów runbook.

### <a name="tag-through-the-portal"></a>Tag za pośrednictwem portalu
Wykonaj następujące kroki, aby oznaczyć maszyny wirtualnej lub grupy zasobów w portalu:

1. Spłaszczanie ciągu JSON i sprawdź, czy nie ma żadnych spacji.  Ciągu JSON powinien wyglądać następująco:

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. Wybierz **Tag** ikona maszyny Wirtualnej lub zasób grupy do zastosowania tego harmonogramu.

   ![Opcja tag maszyny Wirtualnej](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. Tagi są definiowane po parę klucz/wartość. Typ **harmonogram** w **klucza** pola, a następnie wklej do ciągu JSON **wartość** pola. Kliknij pozycję **Zapisz**. Twoje nowy znacznik powinien zostać wyświetlony na liście tagów dla zasobu.

   ![Tag harmonogram maszyny Wirtualnej](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a>Znacznik z programu PowerShell
Wszystkie zaimportowane elementy runbook zawierają informacje pomocy na początku skryptu, który opisuje sposób wykonywania elementy runbook bezpośrednio z programu PowerShell. Dodaj ScheduleResource i ScheduleResource aktualizacji elementów runbook można wywołać z programu PowerShell. W tym przez przekazanie wymaganych parametrów, które umożliwiają tworzenie lub aktualizowanie tag harmonogramu dla maszyny Wirtualnej lub zasób grupy poza portalu.

Aby utworzyć, dodawanie i usuwanie tagów za pomocą programu PowerShell, należy najpierw [konfigurowania środowiska PowerShell dla usługi Azure](/powershell/azure/overview). Po zakończeniu instalacji można kontynuować następujące kroki.

### <a name="create-a-schedule-tag-with-powershell"></a>Utwórz harmonogram tag przy użyciu programu PowerShell
1. Otwórz sesję programu PowerShell. Następnie skorzystaj z następującego przykładu, do uwierzytelniania przy użyciu konta Uruchom jako, a aby określić subskrypcję:

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Zdefiniuj harmonogram tablicy skrótów. Poniżej przedstawiono przykładowy sposób wykonane:

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. Zdefiniuj parametry, które są wymagane przez element runbook. W poniższym przykładzie mamy przeznaczona dla maszyny Wirtualnej:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    Jeśli w przypadku znakowanie grupę zasobów, Usuń *VMName* parametru z skrótu $params tabeli w następujący sposób:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. Uruchom element runbook ResourceSchedule Dodaj z następującymi parametrami, aby utworzyć tag harmonogramu:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. Aby zaktualizować grupy zasobów lub tagu maszyny wirtualnej, należy wykonać **ResourceSchedule aktualizacji** elementu runbook z następującymi parametrami:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a>Usuń znacznik harmonogramu przy użyciu programu PowerShell
1. Otwórz sesję programu PowerShell i uruchom następujące polecenie do uwierzytelniania przy użyciu konta Uruchom jako i aby wybrać i określić subskrypcji:

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Zdefiniuj parametry, które są wymagane przez element runbook. W poniższym przykładzie mamy przeznaczona dla maszyny Wirtualnej:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    Jeśli usuwasz tag z grupy zasobów, Usuń *VMName* parametru z skrótu $params tabeli w następujący sposób:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. Wykonaj runbook ResourceSchedule Usuń, aby usunąć tag harmonogramu:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. Aby zaktualizować grupy zasobów lub tagu maszyny wirtualnej, wykonaj ResourceSchedule Usuń element runbook z następującymi parametrami:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> Zaleca się, że aktywnego monitorowania tych elementów runbook (i Stany maszyny wirtualnej), aby sprawdzić, czy maszyny wirtualne są zamknięte w dół i odpowiednio uruchomiona.
>

Aby wyświetlić szczegóły zadania ResourceSchedule testu elementu runbook w portalu Azure, wybierz **zadania** kafelka elementu runbook. W obszarze podsumowania zadania są wyświetlane parametry wejściowe i dane ze strumienia wyjściowego oraz ogólne informacje o zadaniu wraz z wygenerowanymi wyjątkami.

W obszarze **Podsumowanie zadania** są widoczne ostrzeżenia i komunikaty o błędach oraz dane ze strumienia wyjściowego. Wybierz kafelek **Dane wyjściowe**, aby wyświetlić szczegółowe wyniki wykonania elementu Runbook.

![Dane wyjściowe ResourceSchedule testu](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a>Następne kroki
* Aby rozpocząć pracę z elementami runbook przepływu pracy programu PowerShell, zobacz [Mój pierwszy element runbook przepływu pracy programu PowerShell](automation-first-runbook-textual.md).
* Aby dowiedzieć się więcej na temat typów elementów runbook i ich zalety i ograniczenia, zobacz [typy elementu runbook usługi Automatyzacja Azure](automation-runbook-types.md).
* Aby uzyskać więcej informacji o funkcji obsługi skryptów programu PowerShell, zobacz [Obsługa skryptów PowerShell natywnego automatyzacji Azure](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
* Aby dowiedzieć się więcej na temat rejestrowania elementu runbook i dane wyjściowe, zobacz [Runbook dane wyjściowe i komunikaty w automatyzacji Azure](automation-runbook-output-and-messages.md).
* Aby uzyskać więcej informacji na temat konta Uruchom jako platformy Azure i sposobu uwierzytelniania przy użyciu jego elementy runbook, zobacz [uwierzytelniania elementy runbook za pomocą konta Uruchom jako platformy Azure](automation-sec-configure-azure-runas-account.md).
