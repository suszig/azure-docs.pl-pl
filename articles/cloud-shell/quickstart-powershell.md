---
title: "PowerShell w chmurze Azure powłoki (wersja zapoznawcza) — Szybki Start | Dokumentacja firmy Microsoft"
description: "Szybki Start dla środowiska PowerShell w chmurze powłoki"
services: Azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: damaerte
ms.openlocfilehash: fd1d340bc0408eaeb0b7b18235df109224eae5f5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="quickstart-for-powershell-in-azure-cloud-shell"></a>Szybki Start dla środowiska PowerShell w chmurze Azure powłoki

Ten dokument zawiera szczegóły dotyczące używania programu PowerShell w chmurze powłoki w [portalu Azure](https://aka.ms/PSCloudPreview).

> [!NOTE]
> A [Bash w powłoce chmury Azure](quickstart.md) szybkiego startu jest również dostępna.

## <a name="start-cloud-shell"></a>Uruchom powłokę chmury

1. Polecenie **powłoki chmury** przycisk z górnym pasku nawigacyjnym portalu Azure

  ![](media/quickstart-powershell/shell-icon.png)

2. Wybierz z listy rozwijanej środowiska PowerShell i będzie dysku platformy Azure`(Azure:)`

  ![](media/quickstart-powershell/environment-ps.png)

## <a name="run-powershell-commands"></a>Uruchom polecenia programu PowerShell

Uruchamianie regularne poleceń programu PowerShell w powłoce chmury, takich jak:

```Powershell
PS Azure:\> Get-Date
Monday, September 25, 2017 08:55:09 AM

PS Azure:\> Get-AzureRmVM -Status

ResourceGroupName       Name       Location                VmSize   OsType     ProvisioningState  PowerState
-----------------       ----       --------                ------   ------     -----------------  ----------
MyResourceGroup2        Demo        westus         Standard_DS1_v2  Windows    Succeeded           running
MyResourceGroup         MyVM1       eastus            Standard_DS1  Windows    Succeeded           running
MyResourceGroup         MyVM2       eastus   Standard_DS2_v2_Promo  Windows    Succeeded           deallocated
```

## <a name="navigate-azure-resources"></a>Przejdź do zasobów platformy Azure

 1. Lista subskrypcji

    ``` Powershell
    PS Azure:\> dir
    ```

 2. `cd`do preferowanego subskrypcji

    ``` Powershell
    PS Azure:\> cd MySubscriptionName
    PS Azure:\MySubscriptionName>
    ```

 3. Wyświetlanie wszystkich zasobów na platformie Azure w ramach bieżącej subskrypcji
 
    Typ `dir` na liście wiele widoków zasobów platformy Azure.
 
    ``` PowerShell
    PS Azure:\MySubscriptionName> dir

        Directory: azure:\MySubscriptionName

    Mode Name
    ---- ----
    +    AllResources
    +    ResourceGroups
    +    StorageAccounts
    +    VirtualMachines
    +    WebApps
     ```

### <a name="allresources-view"></a>Widok AllResources 
Typ `dir` w obszarze `AllResources` katalogu, aby wyświetlić zasobów platformy Azure.
    
    PS Azure:\MySubscriptionName> dir AllResources

### <a name="explore-resource-groups"></a>Eksploruj grup zasobów

 Można przejść do `ResourceGroups` katalogu i wewnątrz określonej grupy zasobów można znaleźć maszyny wirtualnej.

``` PowerShell
PS Azure:\MySubscriptionName> cd ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines

PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines> dir


    Directory: Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines


VMName    Location   ProvisioningState VMSize          OS            SKU             OSVersion AdminUserName  NetworkInterfaceName
------    --------   ----------------- ------          --            ---             --------- -------------  --------------------
TestVm1   westus     Succeeded         Standard_DS2_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo371
TestVm2   westus     Succeeded         Standard_DS1_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo271

```
> [!NOTE]
> Należy zauważyć, że podczas wpisywania po raz drugi `dir`, powłoka chmury jest możliwość wyświetlania elementów znacznie szybsze.
> Jest to spowodowane elementy podrzędne są buforowane w pamięci dla wygody użytkowników.
Jednak zawsze można użyć `dir -Force` można pobrać nowe dane.

### <a name="navigate-storage-resources"></a>Przejdź do zasobów magazynu
    
Wprowadzając w `StorageAccounts` folderze można łatwo przejść zasobów magazynu
    
``` PowerShell 
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> dir

    Directory: Azure:\MySubscriptionNameStorageAccounts\MyStorageAccountName\Files


Name          ConnectionString
----          ----------------
MyFileShare1  \\MyStorageAccountName.file.core.windows.net\MyFileShare1;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare2  \\MyStorageAccountName.file.core.windows.net\MyFileShare2;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare3  \\MyStorageAccountName.file.core.windows.net\MyFileShare3;AccountName=MyStorageAccountName AccountKey=<key>


```

Przy użyciu parametrów połączenia następujące polecenie służy do udziałów plików Azure.
        
``` PowerShell
net use <DesiredDriveLetter>: \\<MyStorageAccountName>.file.core.windows.net\<MyFileShareName> <AccountKey> /user:Azure\<MyStorageAccountName>


```

Aby uzyskać więcej informacji, zobacz [instalowanie udziału plików platformy Azure i uzyskać dostępu do udziału w systemie Windows][azmount].

Można także przechodzić katalogi w ramach udziału plików platformy Azure w następujący sposób:

            
``` PowerShell
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> cd .\MyFileShare1\
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files\MyFileShare1> dir

Mode  Name
----  ----
+     TestFolder
.     hello.ps1

    
```

### <a name="interact-with-virtual-machines"></a>Współdziałanie z maszynami wirtualnymi

Można znaleźć sieci maszyn wirtualnych w ramach bieżącej subskrypcji za pośrednictwem `VirtualMachines` katalogu.
    
``` PowerShell
PS Azure:\MySubscriptionName\VirtualMachines> dir

    Directory: Azure:\MySubscriptionName\VirtualMachines


Name       ResourceGroupName  Location  VmSize          OsType              NIC ProvisioningState  PowerState
----       -----------------  --------  ------          ------              --- -----------------  ----------
TestVm1    MyResourceGroup1   westus    Standard_DS2_v2 Windows       my2008r213         Succeeded     stopped
TestVm2    MyResourceGroup1   westus    Standard_DS1_v2 Windows          jpstest         Succeeded deallocated
TestVm10   MyResourceGroup2   eastus    Standard_DS1_v2 Windows           mytest         Succeeded     running


```

#### <a name="invoke-powershell-script-across-remote-vms"></a>Wywołanie skryptu programu PowerShell na maszynach wirtualnych zdalnego

 > [!WARNING]
 > Zapoznaj się z [Rozwiązywanie problemów z zdalne zarządzanie maszynami wirtualnymi Azure](troubleshooting.md#powershell-resolutions).

  Zakładając, że maszyny wirtualnej, MyVM1, można użyć `Invoke-AzureRmVMCommand` do wywołania blok skryptu programu PowerShell na komputerze zdalnym.

  ``` Powershell
  Invoke-AzureRmVMCommand -Name MyVM1 -ResourceGroupName MyResourceGroup -Scriptblock {Get-ComputerInfo} -EnableRemoting
  ```
  Można również przejść do katalogu virtualMachines najpierw i uruchomić `Invoke-AzureRmVMCommand` w następujący sposób.

  ``` Powershell
  PS Azure:\> cd MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines
  PS Azure:\MySubscriptionName\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Invoke-AzureRmVMCommand -Scriptblock{Get-ComputerInfo}
  ```
  Zostaną wyświetlone dane wyjściowe podobne do następującego:

  ``` Powershell
  PSComputerName                                          : 65.52.28.207
  RunspaceId                                              : 2c2b60da-f9b9-4f42-a282-93316cb06fe1
  WindowsBuildLabEx                                       : 14393.1066.amd64fre.rs1_release_sec.170327-1835
  WindowsCurrentVersion                                   : 6.3
  WindowsEditionId                                        : ServerDatacenter
  WindowsInstallationType                                 : Server
  WindowsInstallDateFromRegistry                          : 5/18/2017 11:26:08 PM
  WindowsProductId                                        : 00376-40000-00000-AA947
  WindowsProductName                                      : Windows Server 2016 Datacenter
  WindowsRegisteredOrganization                           :
   ...
  ```

#### <a name="interactively-log-on-to-a-remote-vm"></a>Interakcyjnego logowania się do zdalnego maszyny Wirtualnej

Można użyć `Enter-AzureRmVM` do interaktywnego zalogowania się do maszyny Wirtualnej z systemem w usłudze Azure.

  ``` Powershell
  Enter-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup -EnableRemoting
  ```

Można także przechodzić do `virtualMachines` katalogu pierwszej i uruchom `Enter-AzureRmVM` w następujący sposób

  ``` Powershell
 PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Enter-AzureRmVM
 ```

### <a name="discover-webapps"></a>Odnajdywanie WebApps

Wprowadzając w `WebApps` folderze można łatwo przejść zasobów magazynu

``` PowerShell
PS Azure:\MySubscriptionName> dir .\WebApps\

    Directory: Azure:\MySubscriptionName\WebApps


Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Stopped  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US



# You can use Azure cmdlets to Start/Stop your web apps for example,
PS Azure:\MySubscriptionName\WebApps> Start-AzureRmWebApp -Name mywebapp1 -ResourceGroupName MyResourceGroup1

Name           State    ResourceGroup        EnabledHostNames                   Location
----           -----    -------------        ----------------                   --------
mywebapp1      Running  MyResourceGroup1     {mywebapp1.azurewebsites.net ...   West US

# Refresh the current state with -force
PS Azure:\MySubscriptionName\WebApps> dir -force

    Directory: Azure:\MySubscriptionName\WebApps


Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Running  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US

```

## <a name="list-available-commands"></a>Listę dostępnych poleceń.

W obszarze `Azure` dysku, wpisz `Get-AzureRmCommand` można pobrać kontekstu określonych poleceń Azure.

Alternatywnie możesz za pomocą `Get-Command *azurerm* -Module AzureRM.*` Aby sprawdzić dostępne polecenia platformy Azure.

## <a name="install-custom-modules"></a>Instalowanie niestandardowych modułów

Można uruchomić `Install-Module` zainstalować moduły z [galerii programu PowerShell][gallery].

## <a name="get-help"></a>Get-Help

Typ `Get-Help` Aby uzyskać informacje dotyczące programu PowerShell w powłoce chmury Azure.

``` Powershell
PS Azure:\> Get-Help
```

Dla danego polecenia można nadal wykonywać Get-Help, a następnie polecenia cmdlet, na przykład

``` Powershell
PS Azure:\> Get-Help Get-AzureRmVM
```

## <a name="use-azure-file-storage-to-store-your-data"></a>Użyj Azure File Storage do przechowywania danych

Można utworzyć skrypt, powiedz `helloworld.ps1`i zapisz go do Twojego clouddrive go używać w wielu sesjach powłoki.

``` Powershell
cd C:\users\ContainerAdministrator\CloudDrive
PS C:\users\ContainerAdministrator\CloudDrive> vim .\helloworld.ps1
# Add the content, such as 'Hello World!'
PS C:\users\ContainerAdministrator\CloudDrive> .\helloworld.ps1
Hello World!
```

Następnym razem, kiedy używać środowiska PowerShell w chmurze powłoki, `helloworld.ps1` pliku będzie istnieć w obszarze `CloudDrive` folderu, który instaluje udziału plików platformy Azure.

## <a name="use-custom-profile"></a>Użyj profilu niestandardowego

Można dostosować środowiska PowerShell, tworząc PowerShell profile - `profile.ps1` lub `Microsoft.PowerShell_profile.ps1`. Zapisz go w obszarze `CloudDrive` , dzięki czemu mogą być ładowane w każdej sesji programu PowerShell podczas uruchamiania powłoki chmury.

Jak utworzyć profil, zapoznaj się [profile o][profile].

## <a name="use-git"></a>Przy użyciu narzędzia Git

Klonowanie repozytorium git w CloudShell, musisz utworzyć [osobisty token dostępu] [ githubtoken] i używać go jako nazwy użytkownika. Raz masz z tokenu, klonowanie repozytorium w następujący sposób:

 ``` PowerShell
  git clone https://<your-access-token>@github.com/username/repo.git

```
Ponieważ sesje w CloudShell nie zostaną utrwalone po wyrejestrowaniu lub limit czasu sesji, nie istnieje w pliku config Git podczas następnego logowania. Aby zachować konfigurację Git, musisz zapisać Twoje .gitconfig do Twojej `CloudDrive` i skopiuj go lub utworzyć łącza symbolicznego podczas `CloudShell` pobiera uruchomiona. Poniższy fragment kodu w Twojej profile.ps1 używać do tworzenia łącza symbolicznego do `CloudDrive`.

 ``` PowerShell
 
# .gitconfig path relative to this script
$script:gitconfigPath = Join-Path $PSScriptRoot .gitconfig

# Create a symlink to .gitconfig in user's $home
if(Test-Path $script:gitconfigPath){

    if(-not (Test-Path (Join-Path $Home .gitconfig ))){
         New-Item -ItemType SymbolicLink -Path $home -Name .gitconfig -Value $script:gitconfigPath
    }
}

```
## <a name="exit-the-shell"></a>Wyjdź z powłoki:

Typ `exit` zakończenie sesji.

[bashqs]:quickstart.md
[gallery]:https://www.powershellgallery.com/
[customex]:https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript
[profile]: https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_profiles
[azmount]: https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows
[githubtoken]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/