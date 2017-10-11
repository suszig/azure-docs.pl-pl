---
title: "Przekaż obiekt JSON do elementu runbook usługi Automatyzacja Azure | Dokumentacja firmy Microsoft"
description: "Jak do przekazania parametrów do elementu runbook jako obiekt JSON"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: "środowiska PowerShell, elementu runbook, json, usługi Automatyzacja azure"
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: eslesar
ms.openlocfilehash: eac0e95a46731b9d396ea0590e629d61ca6a7d70
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="pass-a-json-object-to-an-azure-automation-runbook"></a>Przekaż obiekt JSON do elementu runbook usługi Automatyzacja Azure

Może być przydatne do przechowywania danych, które mają zostać przekazane do elementu runbook w pliku JSON.
Na przykład może utworzyć pliku JSON, który zawiera wszystkie parametry, które mają zostać przekazane do elementu runbook.
Aby to zrobić, należy skonwertowane do ciągu JSON, a następnie wykonać konwersję ciąg obiekt programu PowerShell przed przekazaniem ich do elementu runbook z jego zawartość.

W tym przykładzie utworzymy skrypt programu PowerShell, który wywołuje [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) uruchomienia elementu runbook programu PowerShell, przekazywanie zawartości JSON do elementu runbook.
Element runbook programu PowerShell uruchamia maszyny Wirtualnej platformy Azure, pobieranie parametrów dla maszyny Wirtualnej z formatu JSON, która została przekazana.

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub <a href="/pricing/free-account/" target="_blank">[utworzyć bezpłatne konto](https://azure.microsoft.com/free/).
* [Konto usługi Automation](automation-sec-configure-azure-runas-account.md) do przechowywania elementu Runbook i uwierzytelniania w zasobach platformy Azure.  To konto musi mieć uprawnienia do uruchamiania i zatrzymywania maszyny wirtualnej.
* Maszyna wirtualna platformy Azure. Będziemy uruchamiać i zatrzymywać tę maszynę, dlatego należy użyć maszyny innej niż produkcyjna.
* Program Azure Powershell jest zainstalowane na komputerze lokalnym. Zobacz [Instalowanie i konfigurowanie programu Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) informacji o sposobie pobrania programu Azure PowerShell.

## <a name="create-the-json-file"></a>Utwórz plik JSON

Wpisz następującego testu w pliku tekstowym, a następnie zapisz go jako `test.json` pozwalającego na komputerze lokalnym.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-the-runbook"></a>Tworzenie elementu runbook

Utwórz nowy element runbook programu PowerShell o nazwie "Test-Json" automatyzacji Azure.
Aby dowiedzieć się, jak utworzyć nowy element runbook programu PowerShell, zobacz [Moje pierwszego elementu runbook programu PowerShell](automation-first-runbook-textual-powershell.md).

Aby zaakceptować dane JSON, elementu runbook musi pobrać obiektu jako parametr wejściowy.

Element runbook można użyć właściwości zdefiniowane w formacie JSON.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 Zapisz i Opublikuj ten element runbook do Twojego konta automatyzacji.

## <a name="call-the-runbook-from-powershell"></a>Wywołanie elementu runbook z programu PowerShell

Teraz można wywołać elementu runbook z komputera lokalnego przy użyciu programu Azure PowerShell.
Uruchom następujące polecenia programu PowerShell:

1. Logowanie do platformy Azure:
   ```powershell
   Login-AzureRmAccount
   ```
    Monit o podanie poświadczeń platformy Azure.
1. Pobierz zawartość pliku JSON i przekonwertować na ciąg:
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    `JsonPath`jest to ścieżka, w którym zapisano plik JSON.
1. Konwertowanie wartości ciągu `$json` na obiekt programu PowerShell:
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. Utworzyć obiektu hashtable parametrów dla `Start-AzureRmAutomstionRunbook`:
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   Zwróć uwagę, że ustawiasz wartość `Parameters` na obiekt programu PowerShell, który zawiera wartości z pliku JSON. 
1. Uruchomić element runbook
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

Element runbook używa wartości z pliku JSON do uruchamiania maszyny Wirtualnej.

## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się więcej na temat edytowania elementów runbook programu PowerShell i przepływ pracy programu PowerShell za pomocą edytora tekstową, zobacz [edycji tekstową elementy runbook automatyzacji Azure](automation-edit-textual-runbook.md) 
* Aby dowiedzieć się więcej o tworzeniu i importowania elementów runbook, zobacz [Tworzenie lub importowanie elementu runbook automatyzacji Azure](automation-creating-importing-runbook.md)


