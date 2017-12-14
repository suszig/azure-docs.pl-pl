---
title: " Korygowanie alerty Azure maszyny Wirtualnej z elementami Runbook automatyzacji | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono sposób integracji alerty maszyny wirtualnej platformy Azure z elementu runbook usługi Automatyzacja Azure i automatyczne rozwiązywanie problemów"
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 1f7baa7f-7283-4a4f-9385-3f5cd1062c7f
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: csand;magoedte
ms.openlocfilehash: ef18a2de8ce62de945c49b91dc74aca7d7f408b0
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a>Scenariusz automatyzacji Azure - skorygować alerty maszyny Wirtualnej Azure
Maszyny wirtualne Azure i automatyzacji Azure zostały wydane nowa funkcja umożliwia konfigurowanie alertów maszyn wirtualnych (VM) do uruchamiania elementów runbook automatyzacji. Ta nowa funkcja umożliwia automatycznie przeprowadzić korektę standardowe w odpowiedzi na alerty maszyny Wirtualnej, takie jak ponowne uruchomienie lub zatrzymanie maszyny Wirtualnej.

Wcześniej, podczas tworzenia reguły alertu maszyny Wirtualnej można było [Określ element webhook usługi automatyzacja](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) do elementu runbook, aby można było uruchomić element runbook, przy każdym wyzwoleniu alertu. Jednak to wymagane do pracy z tworzenia elementu runbook, tworzenia elementu webhook dla elementu runbook, kopiowanie i wklejanie elementu webhook podczas tworzenia reguły alertu. W tej nowej wersji proces jest znacznie łatwiejsze, ponieważ element runbook może bezpośrednio wybrać z listy, podczas tworzenia reguły alertu, można wybrać konto automatyzacji, który jest uruchomiony element runbook lub łatwo utworzyć konto.

W tym artykule zostanie przedstawiony zostanie sposób łatwo jest ustawić alert maszyny Wirtualnej platformy Azure i skonfigurować element runbook usługi Automatyzacja do uruchamiania w przypadku wyzwala alert. Przykładowe scenariusze obejmują ponowne uruchomienie maszyny Wirtualnej, gdy użycie pamięci przekroczy próg niektóre z powodu aplikacji na maszynie Wirtualnej o przeciek pamięci ani zatrzymywanie maszyny Wirtualnej, gdy czas użytkownika procesora CPU było poniżej 1% ostatniej godziny i nie jest używany. Prezentujemy również zasady sposób automatycznego tworzenia usługi podmiotu zabezpieczeń na Twoim koncie automatyzacji upraszcza korzystanie z elementów runbook w programie Azure korygowania alertu.

## <a name="create-an-alert-on-a-vm"></a>Utwórz alert na maszynie Wirtualnej
Wykonaj poniższe kroki, aby skonfigurować alert, aby uruchomić element runbook po jego próg zostały spełnione.

> [!NOTE]
> W tej wersji obsługiwany jest tylko maszyny wirtualne w wersji 2 i pomocy technicznej klasycznego maszyn wirtualnych zostaną dodane wkrótce.  
> 
> 

1. Zaloguj się do portalu Azure, a następnie kliknij przycisk **maszyn wirtualnych**.  
2. Wybierz jedną z maszyn wirtualnych.  
3. Na ekranie maszyny Wirtualnej w **monitorowanie** kliknij **reguły alertów**.
4. Na **reguły alertów** okienku, kliknij przycisk **Dodaj alert**.

Spowoduje to otwarcie **dodać regułę alertu** strony, w którym można skonfigurować warunki alertu i wyboru spośród jedno lub wszystkie z następujących opcji: Wyślij wiadomość e-mail do osoby, należy użyć elementu webhook do przesyłania dalej alertów do innego systemu i/lub uruchom Element runbook automatyzacji w odpowiedzi próbę rozwiązania problemu.

## <a name="configure-a-runbook"></a>Konfigurowanie elementu runbook
Aby skonfigurować element runbook uruchamiany, gdy próg alertu maszyny Wirtualnej jest spełniony, wybierz **elementu Runbook automatyzacji**. W **skonfigurować runbook** okienka, możesz wybrać na uruchomienie elementu runbook i konta automatyzacji, aby uruchomić element runbook.

![Skonfiguruj element runbook usługi Automatyzacja i Utwórz nowe konto automatyzacji](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> W tej wersji są dostępne trzy elementy runbook, które udostępnia usługę — ponowne uruchomienie maszyny Wirtualnej, Zatrzymaj maszyny Wirtualnej lub usunąć maszyny Wirtualnej (Usuń go).  Wybierz inne elementy runbook lub jeden z własnych elementów runbook możliwości będą dostępne w przyszłej wersji.
> 
> 

![Wybierz spośród elementów Runbook](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

Po wybraniu jednej z trzech dostępnych elementów runbook **konto automatyzacji** prawdopodobnie listy rozwijanej i można wybrać element runbook będzie uruchamiany jako konto automatyzacji. Elementy Runbook muszą działać w kontekście [konto automatyzacji](automation-security-overview.md) w Twojej subskrypcji platformy Azure. Można wybrać konto usługi Automatyzacja już utworzone, czy masz utworzone nowe konto automatyzacji.

Elementy runbook, które znajdują się uwierzytelnić się na platformie Azure przy użyciu nazwy głównej usługi. Jeśli chcesz uruchomić element runbook w jednym z istniejących kont automatyzacji, firma Microsoft automatycznie tworzy usługę podmiotu zabezpieczeń. Jeśli użytkownik chce utworzyć nowe konto usługi Automatyzacja, firma Microsoft automatycznie utworzy konto i nazwę główną usługi. W obu przypadkach dwa zasoby są również tworzone w ramach konta usługi Automatyzacja — zasób certyfikatu o nazwie **AzureRunAsCertificate** i zasób połączenia o nazwie **AzureRunAsConnection**. Elementy runbook, użyj **AzureRunAsConnection** do uwierzytelniania w usłudze Azure w celu wykonania akcji zarządzania dla maszyny Wirtualnej.

> [!NOTE]
> Nazwy głównej usługi jest tworzony w zakresie subskrypcji i przypisany do roli współautora. Ta rola jest wymagana w celu zarządzania maszynami wirtualnymi platformy Azure dla konta, aby mieć uprawnienia do uruchamiania elementów runbook automatyzacji.  Tworzenie konta Automaton i/lub nazwy głównej usługi jest zdarzenia jednorazowego. Po ich utworzeniu można użyć tego konta do uruchamiania elementów runbook dla innych alertów maszyny Wirtualnej platformy Azure.
> 
> 

Po kliknięciu **OK** alert został skonfigurowany, a jeśli wybrano opcję, aby utworzyć nowe konto automatyzacji jest tworzony wraz z nazwy głównej usługi.  Może to zająć kilka sekund.  

![Element Runbook jest skonfigurowany](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

Po zakończeniu konfiguracji zobaczysz nazwy elementu runbook w **dodać regułę alertu** strony.

![Element Runbook jest skonfigurowany](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

Kliknij przycisk **OK** w **dodać regułę alertu** strony.  Reguła alertu jest tworzony i aktywowane, jeśli maszyna wirtualna jest w stanie uruchomienia.

### <a name="enable-or-disable-a-runbook"></a>Włącz lub wyłącz elementu runbook
Jeśli element runbook skonfigurowane dla alertu, można ją wyłączyć, bez usuwania konfiguracji elementu runbook. Dzięki temu można zachować alertów uruchomiona i może testowania niektórych reguł alertów i później ponownie włączyć element runbook.

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a>Tworzenie elementu runbook, który współpracuje z Azure alertu
Po wybraniu elementu runbook regułę alertu Azure w ramach elementu runbook musi mieć logikę w nim zarządzać przekazywane do niego dane alertu.  Jeśli element runbook jest skonfigurowany w regule alertu, elementu webhook jest tworzona dla elementu runbook; tego elementu webhook jest następnie używany do uruchamiania elementu runbook zawsze wyzwala alert.  To rzeczywiste wywołanie do uruchamiania elementu runbook jest żądanie HTTP POST na adres URL elementu webhook. Treść żądania POST zawiera sformatowane JSON obiekt, który zawiera przydatne właściwości powiązane z danym alertem.  Jak widać poniżej, dane alertu zawiera szczegółowe informacje, takie jak identyfikator subskrypcji, grupy zasobów o nazwie resourceName i typu zasobu.

### <a name="example-of-alert-data"></a>Przykład danych alertu
```
{
    "WebhookName": "AzureAlertTest",
    "RequestBody": "{
    \"status\":\"Activated\",
    \"context\": {
        \"id\":\"/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/microsoft.insights/alertrules/AlertTest\",
        \"name\":\"AlertTest\",
        \"description\":\"\",
        \"condition\": {
            \"metricName\":\"CPU percentage guest OS\",
            \"metricUnit\":\"Percent\",
            \"metricValue\":\"4.26337916666667\",
            \"threshold\":\"1\",
            \"windowSize\":\"60\",
            \"timeAggregation\":\"Average\",
            \"operator\":\"GreaterThan\"},
        \"subscriptionId\":\<subscriptionID> \",
        \"resourceGroupName\":\"TestResourceGroup\",
        \"timestamp\":\"2016-04-24T23:19:50.1440170Z\",
        \"resourceName\":\"TestVM\",
        \"resourceType\":\"microsoft.compute/virtualmachines\",
        \"resourceRegion\":\"westus\",
        \"resourceId\":\"/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\",
        \"portalLink\":\"https://portal.azure.com/#resource/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\"
        },
    \"properties\":{}
    }",
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "<webhookURL>"
    }
}
```

Odebrania usługi elementu webhook usługi Automatyzacja HTTP POST wyodrębnia dane alertów i przekazuje je do elementu runbook w parametrze wejściowym WebhookData elementu runbook.  Poniżej przedstawiono przykładowy element runbook, który pokazuje, jak parametr WebhookData i wyodrębniania danych alertów i przy jego użyciu zarządzać zasobów platformy Azure, która wyzwoliła alert.

### <a name="example-runbook"></a>Przykładowy element runbook
```
#  This runbook restarts an ARM (V2) VM in response to an Azure VM alert.

[OutputType("PSAzureOperationResponse")]

param ( [object] $WebhookData )

if ($WebhookData)
{
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Assure that the alert status is 'Activated' (alert condition went from false to true)
    # and not 'Resolved' (alert condition went from true to false)
    if ($WebhookBody.status -eq "Activated")
    {
        # Get the info needed to identify the VM
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId

        # Assure that this is the expected resource type
        Write-Verbose "ResourceType: $ResourceType"
        if ($ResourceType -eq "microsoft.compute/virtualmachines")
        {
            # This is an ARM (V2) VM

            # Authenticate to Azure with service principal and certificate
            $ConnectionAssetName = "AzureRunAsConnection"
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null) {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Restart the VM
            Restart-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName
        } else {
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    } else {
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $WebhookBody.status)
    }
} else {
    Write-Error "This runbook is meant to be started from an Azure alert only."
}
```

## <a name="summary"></a>Podsumowanie
Podczas konfigurowania alertu na maszynie Wirtualnej platformy Azure, masz teraz łatwo skonfigurować element runbook usługi Automatyzacja, aby automatyczne wykonywanie akcji korygowania, gdy wyzwala alert. W tej wersji można z poziomu elementów runbook można ponownie uruchomić, zatrzymać lub usunąć Maszynę wirtualną w zależności od danego scenariusza alertu. Jest to tylko początek włączania scenariuszy, gdzie możesz kontrolować akcje, które są wykonywane automatycznie, gdy wyzwala alert (powiadomienia, rozwiązywania problemów, korygowania).

## <a name="next-steps"></a>Następne kroki
* Aby rozpocząć pracę z graficznymi elementami Runbook, zobacz artykuł [My first graphical runbook](automation-first-runbook-graphical.md) (Mój pierwszy graficzny element Runbook).
* Aby rozpocząć pracę z elementami Runbook przepływu pracy programu PowerShell, zobacz artykuł [My first PowerShell workflow runbook](automation-first-runbook-textual.md) (Mój pierwszy element Runbook przepływu pracy programu PowerShell).
* Aby dowiedzieć się więcej na temat typów elementów Runbook, ich zalet i ograniczeń, zobacz artykuł [Azure Automation runbook types](automation-runbook-types.md) (Typy elementów Runbook usługi Azure Automation).

