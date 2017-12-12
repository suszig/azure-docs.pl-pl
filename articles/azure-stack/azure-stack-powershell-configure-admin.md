---
title: "Konfigurowanie środowiska PowerShell operator stosu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować środowisko PowerShell operator stosu Azure."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 37D9CAC9-538B-4504-B51B-7336158D8A6B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: mabrigg
ms.openlocfilehash: 96ce59d0390affaa57d05d6d08657f5c1a3c466a
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-the-azure-stack-operators-powershell-environment"></a>Konfigurowanie środowiska PowerShell Azure stosu — operator

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Jako operator stosu Azure można skonfigurować sieci Azure stosu Development Kit dla środowiska PowerShell. Po skonfigurowaniu, można za pomocą programu PowerShell zarządzania zasobami Azure stosu takich jak tworzenie oferty, planów, przydziały, Zarządzanie alertami itp. W tym temacie zakresie znajduje się za pomocą operatora chmury środowisk, jeśli chcesz skonfigurować środowiska PowerShell dla środowiska użytkownika odwoływać się tylko do [konfigurowania środowiska PowerShell użytkownika stosu Azure](user/azure-stack-powershell-configure-user.md) tematu. 

## <a name="prerequisites"></a>Wymagania wstępne

Uruchom następujące wymagania wstępne, albo z [zestaw deweloperski](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), lub z systemem Windows klienta zewnętrznych w przypadku [połączone za pośrednictwem sieci VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn): 

* Zainstaluj [modułów programu Azure PowerShell platformy Azure zgodnego stosu](azure-stack-powershell-install.md).  
* Pobierz [narzędzia niezbędne do pracy z stosu Azure](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Skonfiguruj środowisko operatora i zaloguj się do stosu Azure

Na podstawie typu wdrożenia (Azure AD lub AD FS), uruchom następujący skrypt, aby skonfigurować środowisko operatora stosu Azure przy użyciu programu PowerShell (Pamiętaj zastąpić AAD tenantName, GraphAudience punktu końcowego i wartości ArmEndpoint zgodnie z harmonogramem środowiska Konfiguracja):

### <a name="azure-active-directory-aad-based-deployments"></a>Azure Active Directory (AAD) na podstawie wdrożenia
       
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = “<Keyvault DNS suffix for your environment>”


  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint
    
  Set-AzureRmEnvironment `
    -Name "AzureStackAdmin" `
    -GraphAudience $GraphAudience 

  # Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
    -EnvironmentName "AzureStackAdmin"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
  ```

### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Wdrożenia na podstawie usługi Active Directory Federation Services (AD FS)
         
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = “<Keyvault DNS suffix for your environment>”


  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint


  # Get the Active Directory tenantId that is used to deploy Azure Stack     
  $TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackAdmin"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
  ```

## <a name="test-the-connectivity"></a>Testowanie łączności

Teraz, gdy mamy wszystko Konfigurowanie Użyjmy programu PowerShell do tworzenia zasobów w stosie Azure. Można na przykład utworzyć grupę zasobów dla aplikacji i Dodaj maszynę wirtualną. Aby utworzyć grupę zasobów o nazwie "MyResourceGroup", użyj następującego polecenia:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Następne kroki
* [Tworzenie szablonów dla stosu Azure](user/azure-stack-develop-templates.md)
* [Wdrażanie szablonów za pomocą programu PowerShell](user/azure-stack-deploy-template-powershell.md)
