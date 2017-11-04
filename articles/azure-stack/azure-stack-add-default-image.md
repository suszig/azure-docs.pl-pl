---
title: "Dodaj domyślny obraz maszyny Wirtualnej w portalu Azure Marketplace stosu | Dokumentacja firmy Microsoft"
description: "Dodaj maszyny Wirtualnej systemu Windows Server 2016 domyślnego obrazu w portalu Azure Marketplace stosu."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: sngun
ms.openlocfilehash: e5e7ab55800eccea98cbd71ae32bdc611ee3e961
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="add-the-windows-server-2016-vm-image-to-the-azure-stack-marketplace"></a>Dodaj obraz maszyny Wirtualnej systemu Windows Server 2016 w portalu Azure Marketplace stosu

Domyślnie nie obrazów maszyny wirtualnej są dostępne w portalu Azure Marketplace stosu. Operator stosu Azure należy dodać obraz do Marketplace dostępu dla użytkowników. Możesz dodać obraz systemu Windows Server 2016 w portalu Azure Marketplace stosu przy użyciu jednej z następujących metod:

* [Pobranie obrazu z portalu Azure Marketplace](#add-the-image-by-downloading-it-from-the-azure-marketplace). Użyj tej opcji, jeśli są używane w przypadku połączonych i wystąpienia stosu Azure zostały zarejestrowane przy użyciu platformy Azure.

* [Dodanie obrazu przy użyciu programu PowerShell](#add-the-image-by-using-powershell). Ta opcja została wdrożona stosu Azure w scenariuszu odłączonego ani w zastosowaniach z ograniczona łączność.

## <a name="add-the-image-by-downloading-it-from-the-azure-marketplace"></a>Dodanie obrazu, pobierając go z poziomu portalu Azure Marketplace

1. Wdrażanie stosu Azure, a następnie zaloguj się do zestawu Azure stosu Development Kit.

2. Wybierz **więcej usług** > **zarządzania Marketplace** > **Dodaj z platformy Azure**. 

3. Znajdowanie lub wyszukiwanie **systemu Windows Server Datacenter 2016 — Eval** obrazu, a następnie wybierz **Pobierz**.

   ![Pobierz obraz z platformy Azure](media/azure-stack-add-default-image/download-image.png)

Po zakończeniu pobierania obraz jest dostępny w obszarze **zarządzania Marketplace**. Obraz jest również dostępny w obszarze **maszyn wirtualnych**.

## <a name="add-the-image-by-using-powershell"></a>Dodanie obrazu przy użyciu programu PowerShell

### <a name="prerequisites"></a>Wymagania wstępne 

Uruchom następujące wymagania wstępne, albo z [zestaw deweloperski](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) lub z systemu zewnętrznego klienta, w przypadku [połączone za pośrednictwem sieci VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

1. Zainstaluj [modułów programu Azure PowerShell platformy Azure zgodnego stosu](azure-stack-powershell-install.md).  

2. Pobierz [narzędzia niezbędne do pracy z stosu Azure](azure-stack-powershell-download.md).  

3. Na stronie systemu Windows Server ocen [pobierania oceny systemu Windows Server 2016](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016). Po wyświetleniu monitu wybierz wersję ISO pobierania. Zanotuj ścieżkę do lokalizacji pobierania, który jest używany w dalszej części procedury opisane w tym artykule. Ten krok wymaga łączności z Internetem.  

### <a name="add-the-image-to-the-azure-stack-marketplace"></a>Dodawanie obrazu do portalu Azure Marketplace stosu
   
1. Zaimportuj moduły Azure stosu Connect i ComputeAdmin za pomocą następujących poleceń:

   ```powershell
   Set-ExecutionPolicy RemoteSigned

   # Import the Connect and ComputeAdmin modules.   
   Import-Module .\Connect\AzureStack.Connect.psm1
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   ```

2. Zaloguj się do środowiska Azure stosu. Uruchom jedno z poniższych skryptów, w zależności od tego, czy wdrożyć środowiska Azure stosu przy użyciu usługi Azure Active Directory (Azure AD) lub usługi Active Directory Federation Services (AD FS). (Zastąp usługi Azure AD `tenantName`, `GraphAudience` punktu końcowego, a `ArmEndpoint` wartości w celu uwzględnienia konfiguracji środowiska.)  

   * **Usługa Azure Active Directory**. Użyj następującego polecenia cmdlet:

    ```PowerShell
    # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
    $ArmEndpoint = "<Resource Manager endpoint for your environment>"

    # For Azure Stack Development Kit, this value is set to https://graph.windows.net/. To get this value for Azure Stack integrated systems, contact your service provider.
    $GraphAudience = "<GraphAuidence endpoint for your environment>"
    
    # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint $ArmEndpoint

    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience $GraphAudience

    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
      -EnvironmentName AzureStackAdmin

    Login-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID 
    ```

   * **Usługi federacyjne Active Directory**. Użyj następującego polecenia cmdlet:
    
    ```PowerShell
    # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
    $ArmEndpoint = "<Resource Manager endpoint for your environment>"

    # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
    $GraphAudience = "<GraphAuidence endpoint for your environment>"

    # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint $ArmEndpoint

    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience $GraphAudience `
      -EnableAdfsAuthentication:$true

   $TenantID = Get-AzsDirectoryTenantId `
     -ADFS `
     -EnvironmentName "AzureStackAdmin" 

    Login-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID 
    ```
   
3. Dodaj obraz systemu Windows Server 2016 w portalu Azure Marketplace stosu. (Zastąp *fully_qualified_path_to_ISO* ze ścieżką do ISO systemu Windows Server 2016, pobranego.)

    ```PowerShell
    $ISOPath = "<fully_qualified_path_to_ISO>"

    # Add a Windows Server 2016 Evaluation VM image.
    New-AzsServer2016VMImage `
      -ISOPath $ISOPath

    ```

Aby upewnić się, że obraz maszyny Wirtualnej systemu Windows Server 2016 ma najnowszej aktualizacji zbiorczej obejmują `IncludeLatestCU` parametru podczas uruchamiania `New-AzsServer2016VMImage` polecenia cmdlet. Informacje o parametrach dozwolonych dla `New-AzsServer2016VMImage` polecenia cmdlet, zobacz [parametry](#parameters). Trwa około godzinę do publikowania obrazu w portalu Azure Marketplace stosu. 

## <a name="parameters"></a>Parametry

|Parametry nowej AzsServer2016VMImage|Wymagana?|Opis|
|-----|-----|------|
|ISOPath|Tak|Pełna ścieżka do pobranego ISO 2016 serwera systemu Windows.|
|Net35|Nie|Instaluje środowisko uruchomieniowe .NET 3.5 na obrazie systemu Windows Server 2016. Domyślnie ta wartość jest równa **true**.|
|Wersja|Nie|Określa **Core**, **pełne**, lub **zarówno** obrazów systemu Windows Server 2016. Domyślnie ta wartość jest równa **pełne**.|
|VHDSizeInMB|Nie|Ustawia rozmiar obrazu wirtualnego dysku twardego do dodania do środowiska Azure stosu (w MB). Domyślnie ta wartość jest równa 40,960 MB.|
|CreateGalleryItem|Nie|Określa, czy można utworzyć elementu portalu Marketplace w obrazu systemu Windows Server 2016. Domyślnie ta wartość jest równa **true**.|
|location |Nie |Określa lokalizację, do którego powinien zostać opublikowany, obraz systemu Windows Server 2016.|
|IncludeLatestCU|Nie|Stosuje najnowszej aktualizacji zbiorczej dla systemu Windows Server 2016 nowego wirtualnego dysku twardego.|
|CUUri |Nie |Zestawy Windows Server 2016 aktualizacji zbiorczej można uruchamiać ją z określonym identyfikatorem URI. |
|CUPath |Nie |Zestawy Windows Server 2016 aktualizacji zbiorczej można uruchamiać ją z ścieżkę lokalną. Ta opcja jest pomocna, jeśli wdrożono wystąpienie Azure stosu w środowisku bez połączenia.|

## <a name="next-steps"></a>Następne kroki

[Inicjowanie obsługi administracyjnej maszyny wirtualnej](azure-stack-provision-vm.md)
