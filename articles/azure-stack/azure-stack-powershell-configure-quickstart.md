---
title: Instalowanie i konfigurowanie programu PowerShell dla Szybki Start Azure stosu | Dokumentacja firmy Microsoft
description: "Więcej informacji na temat instalowania i konfigurowania programu PowerShell dla usługi Azure stosu."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 6996DFC1-5E05-423A-968F-A9427C24317C
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: mabrigg
ms.openlocfilehash: ca61562607da274f0e0c7f504d1a24723210f2f7
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="get-up-and-running-with-powershell-in-azure-stack"></a>Rozpocząć pracę z programu PowerShell w programie Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Ta opcja szybkiego startu pomaga zainstalować i skonfigurować środowisko stosu Azure przy użyciu programu PowerShell. Skrypt, który firma Microsoft udostępnia w tym artykule jest zakresem **operator stosu Azure** tylko.

W tym artykule jest zmniejszoną wersję kroki, które zostały opisane w [Zainstaluj program PowerShell]( azure-stack-powershell-install.md), [Pobierz narzędzia]( azure-stack-powershell-download.md), i [konfigurowania środowiska PowerShell operator stosu Azure]( azure-stack-powershell-configure-admin.md) artykułów. Za pomocą skryptów w tym temacie, można skonfigurować programu PowerShell dla środowisk Azure stosu, które zostały wdrożone za pomocą usługi Azure Active Directory lub usługi Active Directory Federation Services (AD FS).  


## <a name="set-up-powershell-for-azure-active-directory-based-deployments"></a>Konfigurowanie środowiska PowerShell dla wdrożenia oparte na usłudze Azure Active Directory

Zaloguj się do zestawu Azure stosu Development Kit lub klient zewnętrznych z systemem Windows po nawiązaniu połączenia za pośrednictwem sieci VPN. Otwórz sesję programu PowerShell ISE z podwyższonym poziomem uprawnień, a następnie uruchom następujący skrypt. Upewnij się, że aktualizacja **TenantName**, **ArmEndpoint**, i **GraphAudience** zmienne zgodnie z potrzebami dla konfiguracji środowiska:

> [!IMPORTANT]
> Wersja modułu PowerShell AzureRM 1.2.11 zawiera listę fundamentalne zmiany. Aby uaktualnić 1.2.10 wersji, zobacz [Przewodnik po migracji](https://aka.ms/azspowershellmigration).

```powershell
# Specify Azure Active Directory tenant name.
$TenantName = "<mydirectory>.onmicrosoft.com"

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions, and then run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \

invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = "<Keyvault DNS suffix for your environment>"


# Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName $TenantName `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
```

## <a name="set-up-powershell-for-ad-fs-based-deployments"></a>Konfigurowanie środowiska PowerShell dla usługi AD FS na podstawie wdrożeń

Poniższy skrypt można użyć w przypadku stosu Azure w przypadku połączenia z Internetem. Jednak jeśli stosu Azure działają bez łączności z Internetem, użyj [odłączony sposób instalowania programu PowerShell](azure-stack-powershell-install.md#install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) i poleceń cmdlet programu PowerShell skonfiguruj pozostanie taka sama, jak pokazano w tym skrypcie. Zaloguj się do zestawu Azure stosu Development Kit lub klient zewnętrznych z systemem Windows po nawiązaniu połączenia za pośrednictwem sieci VPN. Otwórz sesję programu PowerShell ISE z podwyższonym poziomem uprawnień, a następnie uruchom następujący skrypt. Upewnij się, że aktualizacja **ArmEndpoint** i **GraphAudience** zmienne zgodnie z potrzebami dla konfiguracji środowiska:

```powershell

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions and run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
$ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = "<Keyvault DNS suffix for your environment>"

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

Teraz, gdy skonfigurowano programu PowerShell, można przetestować konfigurację przez utworzenie grupy zasobów:

```powershell
New-AzureRMResourceGroup -Name "ContosoVMRG" -Location Local
```

Po utworzeniu grupy zasobów, **stan inicjowania obsługi** właściwość jest ustawiona na **zakończyło się pomyślnie**.

## <a name="next-steps"></a>Kolejne kroki

* [Instalowanie i Konfigurowanie interfejsu wiersza polecenia](azure-stack-connect-cli.md)

* [Tworzenie szablonów](user/azure-stack-develop-templates.md)







