---
title: "Wdrażanie szablonów przy użyciu programu PowerShell w stosie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć maszynę wirtualną przy użyciu szablonu usługi Resource Manager i programu PowerShell."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: e4837be016b569dbd0b4bf8e071e6381b8daa85f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Wdrażanie szablonów w stosie Azure przy użyciu programu PowerShell

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Wdrażanie szablonów usługi Azure Resource Manager Development Kit stosu Azure za pomocą programu PowerShell.  Szablony Menedżera zasobów wdrażania i obsługi administracyjnej wszystkie zasoby aplikacji w jednej, skoordynowanej operacji.

## <a name="run-azurerm-powershell-cmdlets"></a>Uruchom polecenia cmdlet programu AzureRM PowerShell
W tym przykładzie, możesz uruchomić skrypt, aby wdrożyć maszynę wirtualną Azure stosu Development Kit przy użyciu szablonu usługi Resource Manager.  Przed kontynuowaniem upewnij się, masz [skonfigurowane programu PowerShell](azure-stack-powershell-configure-user.md)  

Wirtualny dysk twardy używany w tym przykładzie szablonie jest R2 Datacenter, Windows Server 2012 w —.

1. Przejdź do <http://aka.ms/AzureStackGitHub>, wyszukaj **101-prosty windows-vm** szablonu i zapisz go w następującej lokalizacji: c:\\szablony\\ azuredeploy-101-prosty windows-vm.json.
2. W programie PowerShell Uruchom następujący skrypt wdrożenia. Zastąp *username* i *hasło* o nazwę użytkownika i hasło. W kolejnych zastosowań, należy zwiększyć wartość *$myNum* parametr, aby zapobiec zastąpieniu wdrożenia.
   
   ```PowerShell
       # Set Deployment Variables
       $myNum = "001" #Modify this per deployment
       $RGName = "myRG$myNum"
       $myLocation = "local"
   
       # Create Resource Group for Template Deployment
       New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
       # Deploy Simple IaaS Template
       New-AzureRmResourceGroupDeployment `
           -Name myDeployment$myNum `
           -ResourceGroupName $RGName `
           -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
           -NewStorageAccountName mystorage$myNum `
           -DnsNameForPublicIP mydns$myNum `
           -AdminUsername <username> `
           -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
           -VmName myVM$myNum `
           -WindowsOSVersion 2012-R2-Datacenter
   ```
3. Otwieranie portalu, kliknij przycisk stosu Azure **Przeglądaj**, kliknij przycisk **maszyn wirtualnych**i poszukaj nowej maszyny wirtualnej (*myDeployment001*).


## <a name="next-steps"></a>Następne kroki
[Wdrażanie szablonów za pomocą programu Visual Studio](azure-stack-deploy-template-visual-studio.md)

