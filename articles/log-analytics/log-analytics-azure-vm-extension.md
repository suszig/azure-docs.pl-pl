---
title: "Nawiązać maszyn wirtualnych platformy Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Dla systemu Windows i Linux maszyn wirtualnych działających na platformie Azure zalecanym sposobem dzienniki zebranych i metryki jest instalowanie rozszerzenia maszyny Wirtualnej Azure analizy dziennika. Aby zainstalować rozszerzenie Log Analytics maszyny wirtualnej na maszynach wirtualnych platformy Azure, można użyć portalu Azure lub programu PowerShell."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: ca39e586-a6af-42fe-862e-80978a58d9b1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: richrund
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cdae291b546fef4d7fdb8b067c8e4f4c9708d43f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="connect-azure-virtual-machines-to-log-analytics-with-a-log-analytics-agent"></a>Uzyskuj dostęp do maszyn wirtualnych platformy Azure do analizy dzienników agenta analizy dzienników

Na komputerach z systemami Windows i Linux zbierania dzienników i metryki zaleca przez zainstalowanie agenta analizy dzienników.

Najprostszym sposobem zainstalowania agenta analizy dzienników na maszynach wirtualnych Azure jest za pośrednictwem rozszerzenia maszyny Wirtualnej analizy dziennika.  Przy użyciu rozszerzenia upraszcza proces instalacji i automatycznie konfiguruje agenta do wysyłania danych do obszaru roboczego analizy dzienników, który określisz. Agent jest również uaktualniana automatycznie, sprawdzeniu, czy masz najnowsze funkcje i poprawki.

Dla maszyn wirtualnych systemu Windows, należy włączyć *programu Microsoft Monitoring Agent* rozszerzenie maszyny wirtualnej.
Dla maszyn wirtualnych systemu Linux, możesz włączyć *OMS agenta dla systemu Linux* rozszerzenie maszyny wirtualnej.

Dowiedz się więcej o [rozszerzenia maszyny wirtualnej platformy Azure](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) i [agenta systemu Linux](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Korzystając z agenta na podstawie kolekcji danych dziennika, należy skonfigurować [źródeł danych w analizy dzienników](log-analytics-data-sources.md) do określenia dzienników i metryki, które mają być zbierane.

> [!IMPORTANT]
> Jeśli można skonfigurować do indeksowania danych dziennika analizy dzienników przy użyciu [diagnostyki Azure](log-analytics-azure-storage.md)i skonfigurować agenta, aby zebrać dzienniki tego samego, a następnie dzienniki są zbierane dwa razy. Naliczane są opłaty za obu źródeł danych. Jeśli masz zainstalowany agent, zbieranie danych dziennika przy użyciu tylko agenta — nie jest skonfigurowana analizy dzienników do zbierania danych dziennika z diagnostyki Azure.
>
>

Istnieją trzy sposoby włączyć rozszerzenia maszyny wirtualnej analizy dzienników:

* Za pomocą portalu Azure
* Przy użyciu programu Azure PowerShell
* Za pomocą szablonu usługi Azure Resource Manager

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>Włączanie rozszerzenia maszyny Wirtualnej w portalu Azure
Można zainstalować agenta dla analizy dzienników i połączyć maszyny wirtualnej platformy Azure, którym jest on uruchamiany przy użyciu [portalu Azure](https://portal.azure.com).

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>Aby zainstalować agenta analizy dzienników i podłącz maszynę wirtualną do obszaru roboczego analizy dzienników
1. Zaloguj się do [Azure Portal](http://portal.azure.com).
2. Wybierz **Przeglądaj** po lewej stronie portalu, a następnie przejdź do **analizy dzienników (OMS)** i zaznacz je.
3. Na liście obszarów roboczych usługi Analiza dzienników wybierz jedną, która ma być używany z maszyny Wirtualnej platformy Azure.  
   ![Obszary robocze OMS](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. W obszarze **dziennika zarządzania analytics**, wybierz pozycję **maszyn wirtualnych**.  
   ![Maszyny wirtualne](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5. Na liście **maszyn wirtualnych**, wybierz maszynę wirtualną, na którym chcesz zainstalować agenta. **Stan połączenia OMS** dla maszyny Wirtualnej wskazuje, że jest **niepołączone**.  
   ![Maszyna wirtualna nie jest połączony](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6. Informacje dotyczące maszyny wirtualnej, wybierz **Connect**. Agent jest automatycznie instalowany i konfigurowany dla obszaru roboczego analizy dzienników. Ten proces trwa kilka minut, w tym czasie jest stan połączenia OMS *łączenie...*  
   ![Łączenie maszyny Wirtualnej](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7. Po zainstalowaniu i połącz agenta z **połączenia OMS** stan zostanie zaktualizowany w celu wyświetlenia **ten obszar roboczy**.  
   ![Połączone](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)

## <a name="enable-the-vm-extension-using-powershell"></a>Włączanie rozszerzenia maszyny Wirtualnej przy użyciu programu PowerShell
Po skonfigurowaniu maszyny wirtualnej przy użyciu programu PowerShell, należy podać **workspaceId** i **workspaceKey**. Nazwy właściwości w konfiguracji json są **z uwzględnieniem wielkości liter**.

Można znaleźć identyfikator i klucz na **ustawienia** strony portalu OMS lub przy użyciu programu PowerShell, jak pokazano w poprzednim przykładzie.

![Identyfikator i klucz podstawowy](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

Ma innego polecenia Azure klasycznych maszyn wirtualnych i maszyn wirtualnych Menedżera zasobów. Poniżej przedstawiono przykłady dla klasycznego i maszyn wirtualnych Menedżera zasobów.

Klasycznych maszyn wirtualnych skorzystaj z następującego przykładu środowiska PowerShell:

```PowerShell
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Dla maszyn wirtualnych Menedżera zasobów systemu Linux przy użyciu następujących interfejsu wiersza polecenia
```azurecli
az vm extension set --resource-group myRGMonitor --vm-name myMonitorVM --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --version 1.3 --protected-settings ‘{"workspaceKey": "<workspace-key>"}’ --settings ‘{"workspaceId": "<workspace-id>"}’ 
```

Dla maszyn wirtualnych Menedżera zasobów skorzystaj z następującego przykładu środowiska PowerShell:

```PowerShell
Login-AzureRMAccount
Select-AzureRMSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```


## <a name="deploy-the-vm-extension-using-a-template"></a>Wdrażanie rozszerzenia maszyny Wirtualnej przy użyciu szablonu
Za pomocą usługi Azure Resource Manager, można utworzyć szablon (w formacie JSON), który definiuje wdrażania i konfiguracji aplikacji. Jest on nazywany szablonem usługi Resource Manager i pozwala na deklaratywne definiowanie wdrożeń. Za pomocą szablonu, wielokrotnie można wdrożyć aplikację w całym cyklu życia aplikacji i mieć pewność, że zasoby są wdrażane w spójnym stanie.

W tym agentem analizy dzienników jako część szablonu usługi Resource Manager, można upewnij się, że każda maszyna wirtualna jest wstępnie skonfigurowana do raportu do obszaru roboczego analizy dzienników.

Aby uzyskać więcej informacji na temat szablonów usługi Resource Manager, zobacz [szablonów Authoring Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Poniżej przedstawiono przykład szablonu usługi Resource Manager, który służy do wdrażania maszyny wirtualnej z systemem Windows z rozszerzeniem Microsoft Monitoring Agent zainstalowany. Ten szablon jest szablon typowej maszyny wirtualnej, z następującymi dodatkami:

* Parametry workspaceId i workspaceName
* Sekcja rozszerzenia zasobu Microsoft.EnterpriseCloud.Monitoring
* Dane wyjściowe do odszukania workspaceId i workspaceSharedKey

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMS workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Szablonu można wdrożyć przy użyciu następującego polecenia programu PowerShell:

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-the-log-analytics-vm-extension"></a>Rozwiązywanie problemów z rozszerzenia maszyny Wirtualnej analizy dzienników
Zwykle komunikat o błędzie w sytuacji, gdy elementy nie działają z portalu Azure lub programu Azure powershell.

1. Zaloguj się do [Azure Portal](http://portal.azure.com).
2. Znajdź maszyny Wirtualnej, a następnie otwórz szczegóły maszyny Wirtualnej.
3. Kliknij przycisk **rozszerzenia** do sprawdzenia, czy rozszerzenie OMS jest włączone.

   ![Widok rozszerzenia maszyny Wirtualnej](./media/log-analytics-azure-vm-extension/oms-vmview-extensions.png)

4. Kliknij przycisk *MicrosoftMonitoringAgent*(system Windows) lub *OmsAgentForLinux*rozszerzenia i wyświetlenia jej szczegółów (Linux). 

   ![Szczegóły rozszerzenia maszyny Wirtualnej](./media/log-analytics-azure-vm-extension/oms-vmview-extensiondetails.png)

### <a name="troubleshooting-windows-virtual-machines"></a>Rozwiązywanie problemów z maszyn wirtualnych systemu Windows
Jeśli *programu Microsoft Monitoring Agent* nie instaluje rozszerzenia agenta maszyny Wirtualnej lub raporty, można wykonać następujące kroki, aby rozwiązać ten problem.

1. Sprawdź, czy agent maszyny Wirtualnej platformy Azure jest zainstalowany i działa prawidłowo przy użyciu kroków w [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * Można również przejrzeć plik dziennika agenta maszyny Wirtualnej`C:\WindowsAzure\logs\WaAppAgent.log`
   * Jeśli dziennika nie istnieje, nie jest zainstalowany agent maszyny Wirtualnej.
     * [Zainstaluj agenta maszyny Wirtualnej platformy Azure na klasycznych maszyn wirtualnych](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. Upewnij się, że zadanie pulsu rozszerzenie programu Microsoft Monitoring Agent działa, wykonując następujące czynności:
   * Zaloguj się do maszyny wirtualnej
   * Otwórz Harmonogram zadań i Znajdź `update_azureoperationalinsight_agent_heartbeat` zadań
   * Potwierdź zadania jest włączona i działa co minutę.
   * Sprawdź w pliku dziennika pulsu`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Przejrzyj pliki dziennika rozszerzenia maszyny Wirtualnej agenta monitorowania firmy Microsoft w`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Upewnij się, że maszyna wirtualna może uruchamiać skrypty programu PowerShell
5. Upewnij się, że nie zostały zmienione uprawnienia C:\Windows\temp
6. Wyświetl stan programu Microsoft Monitoring Agent, wpisując następujące polecenie w oknie programu PowerShell z podwyższonym poziomem uprawnień na maszynie wirtualnej`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Przejrzyj pliki dziennika instalacji programu Microsoft Monitoring Agent w`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z rozszerzeń systemu Windows](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="troubleshooting-linux-virtual-machines"></a>Rozwiązywanie problemów z maszyn wirtualnych systemu Linux
Jeśli *Agent pakietu OMS Linux* nie instaluje rozszerzenia agenta maszyny Wirtualnej lub raporty, można wykonać następujące kroki, aby rozwiązać ten problem.

1. Jeśli stan rozszerzenia *nieznany* Sprawdź, czy agent maszyny Wirtualnej platformy Azure jest zainstalowany i działa poprawnie, przeglądając plik dziennika agenta maszyny Wirtualnej`/var/log/waagent.log`
   * Jeśli dziennika nie istnieje, nie jest zainstalowany agent maszyny Wirtualnej.
   * [Zainstaluj agenta maszyny Wirtualnej platformy Azure na maszynach wirtualnych systemu Linux](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. Dla innych stanów złej kondycji, przejrzyj Agent pakietu OMS dla rozszerzenia maszyny Wirtualnej systemu Linux pliki dziennika `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` i`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Jeśli stan rozszerzenia jest w dobrej kondycji, ale nie można przekazać danych Przejrzyj Agent pakietu OMS dla plików dziennika systemu Linux`/var/opt/microsoft/omsagent/log/omsagent.log`

## <a name="next-steps"></a>Następne kroki
* Skonfiguruj [źródeł danych w analizy dzienników](log-analytics-data-sources.md) do określenia dzienników i metryk do zbierania.
* Aby zebrać dane z maszyn wirtualnych [rozwiązań dodać analizy dzienników z galerii rozwiązań](log-analytics-add-solutions.md).
* [Zbieranie danych za pomocą diagnostyki Azure](log-analytics-azure-storage.md) dla innych zasobów, które są uruchomione na platformie Azure.

Dla komputerów, które nie są na platformie Azure można zainstalować agenta analizy dzienników przy użyciu metod, które zostały opisane w następujących artykułach:

* [Łączenie komputerów z systemem Windows do analizy dzienników](log-analytics-windows-agents.md)
* [Łączenie komputerów z systemem Linux do analizy dzienników](log-analytics-linux-agents.md)
