---
title: "Odpowiadanie na alerty element runbook usługi Automatyzacja Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można wyzwolić elementu runbook do uruchamiania w momencie pojawienia się alertów platformy Azure."
services: automation
keywords: 
author: georgewallace
ms.author: gwallace
ms.date: 01/11/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 486a3c80c9d6c19ca0a7ba7d4e3cdcde927053b9
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="respond-to-azure-alerts-with-an-automation-runbook"></a>Odpowiadanie na alerty element runbook usługi Automatyzacja Azure

[Azure monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md?toc=%2fazure%2fautomation%2ftoc.json) zapewnia podstawowym poziomie metryki i dzienniki większość usług w systemie Microsoft Azure. Elementy runbook automatyzacji Azure można wywołać za pomocą [grupy akcji](../monitoring-and-diagnostics/monitoring-action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) lub z klasycznym alertów do automatyzowania zadań na podstawie alertów. W tym artykule przedstawiono sposób konfigurowania i uruchamiania elementu runbook na podstawie tych alertów.

## <a name="alert-types"></a>Typy alertów

Elementy Runbook są obsługiwane akcje dla wszystkich trzech typów alertów. Gdy alert wywołuje element runbook, to rzeczywiste wywołanie jest żądanie HTTP POST do elementu webhook. Treść żądania POST zawiera sformatowane JSON obiekt, który zawiera przydatne właściwości powiązane z danym alertem. Poniższa tabela zawiera linki do schematu ładunku dla każdego typu alertu:

|Alerty  |Opis|Schemat ładunku  |
|---------|---------|---------|
|[Klasycznym metryki alertu](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Otrzymasz powiadomienie, gdy wszystkie metryki platformy poziomu spełnia określony warunek (na przykład procesora CPU Wynoszące % na maszynie Wirtualnej jest większa niż 90 ostatnich 5 minut).| [Schemat ładunku](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Alert dziennika aktywności](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Otrzymasz powiadomienie, gdy wszystkie nowe zdarzenie w dzienniku aktywności platformy Azure jest zgodna z określonych warunków (na przykład podczas operacji "Usuwanie maszyny Wirtualnej" występuje w myProductionResourceGroup lub po wyświetleniu nowego zdarzenia kondycji usługi z "Active" jako stan).| [Schemat ładunku](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)        |
|[W czasie rzeczywistym metryki alert o zbliżającym się](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Otrzymasz powiadomienie szybciej niż metryki alerty, gdy co najmniej jedną metrykę platformy poziomu spełnia określone warunki (na przykład jest większa niż 90 procent użycia procesora CPU na maszynie Wirtualnej i sieci w jest większa niż 500 MB przez ostatnich 5 minut).| [Schemat ładunku](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Ponieważ dane dostarczone przez każdy alert jest inny, każdy alert musi być obsługiwane inaczej. W następnej sekcji zostanie Dowiedz się, jak utworzyć element runbook do obsługi tych różnych typów alertów.

## <a name="create-a-runbook-to-handle-alerts"></a>Tworzenie elementu runbook do obsługi alertów

Aby korzystać z automatyzacji z alertami, należy elementu runbook, który zawiera logikę do zarządzania alert ładunek JSON, który zostanie przekazany do elementu runbook. Następujący przykładowy element runbook musi zostać wywołany z poziomu alertu platformy Azure. Zgodnie z opisem w poprzedniej sekcji, każdego rodzaju typu alertu ma inny schemat. Skrypt akceptuje danych elementu webhook w `WebhookData` runbook danych wejściowych parametru z poziomu alertu, a wartość ładunek JSON, aby ustalić, jakiego typu alertu użyto. Poniższy przykład będzie używany na alert z maszyny Wirtualnej. Pobiera dane dotyczące maszyny Wirtualnej z ładunku i używa tych informacji, aby zatrzymać maszynę Wirtualną. Połączenie musi zostać skonfigurowane w automatyzacji konto, gdy element runbook został uruchomiony.

Element runbook używa **AzureRunAsConnection** [konto Uruchom jako](automation-create-runas-account.md) do uwierzytelniania w usłudze Azure w celu wykonania akcji zarządzania dla maszyny Wirtualnej.

Może się zmienić następujący skrypt programu PowerShell do użytku z wielu różnych zasobów.

Następujący przykładowy i utworzyć element runbook o nazwie **Stop AzureVmInResponsetoVMAlert** próbki programu PowerShell.

1. Otwórz konto usługi Automation.
1. Kliknij pozycję **Elementy runbook** w obszarze **AUTOMATYZACJA PROCESÓW**. Zostanie wyświetlona lista elementów runbook.
1. Kliknij przycisk **Dodaj element runbook** znajdujący się w górnej części listy. Na **strony Dodaj Runbook**, wybierz pozycję **szybkie tworzenie**.
1. Wprowadź "Stop-AzureVmInResponsetoVMAlert" dla elementu runbook **nazwa**i wybierz **PowerShell** dla **typ elementu Runbook**. Kliknij przycisk **Utwórz**.
1. W poniższym przykładzie programu PowerShell, skopiuj do okienko edycji. Kliknij przycisk **publikowania** Aby zapisać i opublikować elementu runbook.

```powershell-interactive
<#
.SYNOPSIS
  This runbook will stop a resource management VM in response to an Azure alert trigger.

.DESCRIPTION
  This runbook will stop an resource management VM in response to an Azure alert trigger.
  Input is alert data with information needed to identify which VM to stop.
  
  DEPENDENCIES
  - The runbook must be called from an Azure alert via a webhook.
  
  REQUIRED AUTOMATION ASSETS
  - An Automation connection asset called "AzureRunAsConnection" that is of type AzureRunAsConnection.
  - An Automation certificate asset called "AzureRunAsCertificate".

.PARAMETER WebhookData
   Optional (user does not need to enter anything, but the service will always pass an object)
   This is the data that is sent in the webhook that is triggered from the alert.

.NOTES
   AUTHOR: Azure Automation Team
   LASTEDIT: 2017-11-22
#>

[OutputType("PSAzureOperationResponse")]

param
(
    [Parameter (Mandatory=$false)]
    [object] $WebhookData
)

$ErrorActionPreference = "stop"

if ($WebhookData)
{
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Get the info needed to identify the VM (depends on the payload schema)
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {
        # This is the near-real-time Metric Alert schema
        $AlertContext = [object] ($WebhookBody.data).context
        $ResourceName = $AlertContext.resourceName
        $status = ($WebhookBody.data).status
    }
    elseif ($schemaId -eq "Microsoft.Insights/activityLogs") {
        # This is the Activity Log Alert schema
        $AlertContext = [object] (($WebhookBody.data).context).activityLog
        $ResourceName = (($AlertContext.resourceId).Split("/"))[-1]
        $status = ($WebhookBody.data).status
    }
    elseif ($schemaId -eq $null) {
        # This is the original Metric Alert schema
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $status = $WebhookBody.status
    }
    else {
        # Schema not supported
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    Write-Verbose "status: $status" -Verbose
    if ($status -eq "Activated")
    {
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId
        Write-Verbose "resourceType: $ResourceType" -Verbose
        Write-Verbose "resourceName: $ResourceName" -Verbose
        Write-Verbose "resourceGroupName: $ResourceGroupName" -Verbose
        Write-Verbose "subscriptionId: $SubId" -Verbose

        # Do work only if this is a resource management VM
        if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
        {
            # This is the VM
            Write-Verbose "This is a resource management VM." -Verbose

            # Authenticate to Azure with service principal and certificate and set subscription
            Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
            $ConnectionAssetName = "AzureRunAsConnection"
            Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null)
            {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Write-Verbose "Authenticating to Azure with service principal." -Verbose
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Write-Verbose "Setting subscription to work against: $SubId" -Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Stop the VM
            Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
            Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
            # [OutputType(PSAzureOperationResponse")]
        }
        else {
            # ResourceType not supported
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    }
    else {
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
    }
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="create-an-action-group"></a>Utwórz grupę

Grupy akcji to zbiór akcji wykonywanych na podstawie wylogowuje alertu. Element Runbook jest tylko jeden z wielu akcji, które są dostępne w przypadku grup akcji.

1. W portalu, wybierz **Monitor**.

1. Wybierz **grupy akcji** w obszarze **ustawienia**.

1. Wybierz **Dodaj grupę akcji**, a następnie wypełnij pola.

1. Wprowadź nazwę w polu Nazwa grupy akcji, a następnie wprowadź nazwę w polu krótkiej nazwy. Krótka nazwa jest używana zamiast akcji Pełna nazwa grupy, podczas powiadomienia są wysyłane przy użyciu tej grupy.

1. Autofills pole subskrypcji w ramach bieżącej subskrypcji. Ta subskrypcja jest jeden, w którym jest zapisany grupy akcji.
Wybierz grupę zasobów, w którym jest zapisany grupy akcji.

Na przykład utworzyć dwie akcje, akcję elementu runbook i akcji powiadamiania.

### <a name="runbook-action"></a>Działania elementu Runbook

Poniższe kroki utworzyć akcję elementu runbook w ramach grupy działań.

1. W obszarze **akcje**, nadaj nazwę akcji.

1. Wybierz **elementu Runbook automatyzacji** dla **typ akcji**.

1. W obszarze **szczegóły** wybrać, **edytowanie szczegółów**

1. Na **skonfigurować elementu Runbook** wybierz pozycję **użytkownika** w obszarze **źródło elementu Runbook**.

1. Wybierz użytkownika **subskrypcji** i **konto automatyzacji**, a na koniec wybierz element runbook został utworzony w poprzednim kroku o nazwie "Stop-AzureVmInResponsetoVMAlert".

1. Po zakończeniu kliknij **OK**.

### <a name="notification-action"></a>Akcja powiadomienia

Poniższe kroki należy utworzyć akcję powiadomień w obrębie grupy działań.

1. W obszarze **akcje**, nadaj nazwę akcji.

1. Wybierz **E-mail** dla **typ akcji**.

1. W obszarze **szczegóły** wybrać, **edytowanie szczegółów**

1. Na **E-mail** wprowadź adres e-mail, który będzie powiadamiany, a następnie kliknij przycisk **OK**. Dodawanie adresu e-mail, a także elementu runbook jako akcja jest przydatne, ponieważ użytkownik jest powiadamiany o uruchomieniu elementu runbook. Grupy akcji powinna wyglądać poniższej ilustracji:

   ![Dodaj stronę grupy akcji](./media/automation-create-alert-triggered-runbook/add-action-group.png)

1. Kliknij przycisk **OK** można utworzyć grupy działań.

Z utworzoną grupę działań, możesz utworzyć [alertów dotyczących działań w dzienniku](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) lub [alertów w czasie rzeczywistym w pobliżu](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal) i użyj utworzoną grupę akcji.

## <a name="classic-alert"></a>Alert klasycznego

Klasycznym alerty są oparte na metryki i nie użycie grupy akcji, ale Akcje elementu runbook na ich podstawie. Aby utworzyć klasycznego alertu, użyj następujących kroków:

1. Wybierz **+ Dodaj alert metryki**.

1. Nazwa metryki alertu "myVMCPUAlert" i podaj krótki opis alertu.

1. Warunek metryki alertu jako "Większe niż", jako "10" należy ustawić próg i ustawić okres jako "w ciągu ostatnich 5 minut".

1. W obszarze **reakcję**, wybierz pozycję **uruchomienia elementu runbook z następującego alertu**

1. Na **skonfigurować elementu Runbook** wybierz pozycję **użytkownika** dla **źródło elementu Runbook**. Wybierz konto automatyzacja **Stop AzureVmInResponsetoVMAlert** elementu runbook. Kliknij przycisk **OK**, a następnie kliknij przycisk **OK** ponownie, aby zapisać reguły alertów.

## <a name="next-steps"></a>Kolejne kroki

* Aby uzyskać więcej informacji dotyczących uruchamiania elementu runbook usługi Automatyzacja z elementów webhook, zobacz [uruchomienia elementu runbook z elementu webhook](automation-webhooks.md)
* Aby uzyskać więcej informacji na różne sposoby uruchamiania elementu runbook, zobacz [uruchamianie elementu Runbook](automation-starting-a-runbook.md).
* Aby dowiedzieć się, jak utworzyć alert dziennika aktywności, zobacz [utworzyć alerty dziennika aktywności](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)
* Aby sprawdzić, jak utworzyć near alertów w czasie rzeczywistym, odwiedź stronę [utworzyć regułę alertu za pomocą portalu Azure](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal).