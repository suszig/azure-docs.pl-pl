---
title: "Konfiguracja żądanego stanu dla platformy Azure — omówienie | Dokumentacja firmy Microsoft"
description: "Omówienie dotyczące korzystania z programu Microsoft Azure rozszerzenia obsługi dla konfiguracji żądanego stanu programu PowerShell. W tym wymagania wstępne, architektura, polecenia cmdlet."
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/09/2017
ms.author: zachal
ms.openlocfilehash: deb360e36b68f7ddb13b00946c700d0c83890ca6
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2018
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Wprowadzenie do obsługi rozszerzenia konfiguracji żądanego stanu usługi Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Agent maszyny Wirtualnej i skojarzonych rozszerzeń są częścią usługi infrastruktury platformy Microsoft Azure. Rozszerzenia maszyny Wirtualnej są składniki oprogramowania, które zapewniają rozszerzenie funkcjonalności maszyny Wirtualnej i uproszczenia różne operacje zarządzania maszyny Wirtualnej. Na przykład rozszerzenia VMAccess można zresetować hasła administratora lub rozszerzenia niestandardowego skryptu może służyć do uruchomienia skryptu na maszynie Wirtualnej.

W tym artykule przedstawiono rozszerzenia konfiguracji żądanego stanu środowiska PowerShell (DSC) dla maszyn wirtualnych Azure w ramach zestawu SDK platformy Azure PowerShell. Nowe polecenia cmdlet służy do przekazywania i zastosowanie konfiguracji DSC środowiska PowerShell na maszynie Wirtualnej platformy Azure, włączyć rozszerzenia DSC środowiska PowerShell. DSC środowiska PowerShell rozszerzenia wywołań do usługi Konfiguracja DSC środowiska PowerShell wprowadza otrzymanej konfiguracji DSC na maszynie Wirtualnej. Ta funkcja jest również dostępny za pośrednictwem portalu Azure.

## <a name="prerequisites"></a>Wymagania wstępne
**Komputer lokalny** wchodzić w interakcje z rozszerzenia maszyny Wirtualnej Azure, musisz użyć portalu Azure lub zestaw SDK usługi Azure PowerShell. 

**Agent gościa** maszyny Wirtualnej platformy Azure, skonfigurowaną przy konfiguracji DSC musi być system operacyjny obsługujący Windows Management Framework (WMF) 4.0 albo 5.0. Pełną listę obsługiwanych wersji systemu operacyjnego można znaleźć w folderze [Historia wersji rozszerzenia DSC](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Definicje terminów i pojęć
W tym przewodniku zakłada się znajomość następujące kwestie:

* **Konfiguracja** — dokumentacja konfiguracji A DSC. 
* **Węzeł** -obiektu docelowego dla konfiguracji DSC. W tym dokumencie na zawsze "węzła" odwołuje się do maszyny Wirtualnej platformy Azure.
* **Dane konfiguracji** — plik psd1 zawierający dane środowiska konfiguracji

## <a name="architectural-overview"></a>Omówienie architektury
Rozszerzenia usługi Konfiguracja DSC Azure używa framework Agent maszyny Wirtualnej do dostarczania, wprowadza i raport o konfiguracji DSC uruchomionych na maszynach wirtualnych platformy Azure. Rozszerzenia DSC oczekuje, że plik zip zawierający co najmniej jeden dokument konfiguracji i zestaw parametrów udostępniane przez zestaw SDK usługi Azure PowerShell lub za pośrednictwem portalu Azure.

Jeśli rozszerzenie jest wywoływana po raz pierwszy, uruchamia proces instalacji. Ten proces powoduje zainstalowanie wersji programu Windows Management Framework (WMF) przy użyciu z następującą logiką:

1. Jeśli system operacyjny maszyny Wirtualnej Azure jest Windows Server 2016, nie podjęto żadnej akcji. Windows Server 2016 ma już najnowszą wersję programu PowerShell jest zainstalowane.
2. Jeśli `wmfVersion` właściwość zostanie określony, ta wersja platformy WMF jest zainstalowane, chyba że nie jest zgodny z systemem operacyjnym maszyny Wirtualnej.
3. Jeśli nie `wmfVersion` określona właściwość, jest zainstalowana najnowsza wersja dotyczy wersja programu WMF.

Instalacja programu WMF wymaga ponownego uruchomienia komputera. Po ponownym uruchomieniu, rozszerzenie pliki do pobrania określonego w pliku .zip `modulesUrl` właściwości. Jeśli ta lokalizacja znajduje się w magazynie obiektów blob platformy Azure, można określić tokenu sygnatury dostępu Współdzielonego w `sasToken` właściwości dostępu do tego pliku. Po pobraniu zip i rozpakowane, funkcja konfiguracji zdefiniowana w `configurationFunction` uruchamiany w celu wygenerowania pliku MOF. Następnie uruchamia rozszerzenia `Start-DscConfiguration -Force` w wygenerowanym pliku MOF. Rozszerzenie przechwytuje dane wyjściowe i zapisuje go Wycofaj do kanału stan Azure. Od tego momentu DSC LCM obsługuje monitorowanie i korekty normalnie. 

## <a name="powershell-cmdlets"></a>Polecenia cmdlet programu PowerShell
Polecenia cmdlet programu PowerShell można z usługi Azure Resource Manager lub w klasycznym modelu wdrażania pakietu, publikowanie i monitorować wdrożenia rozszerzenia usługi Konfiguracja DSC. Wdrożenie klasyczne moduły są następujące polecenia cmdlet na liście, ale "AzureRm", do korzystania z usługi Azure Resource Manager modelu można zastąpić "Azure". Na przykład `Publish-AzureVMDscConfiguration` używa klasycznym modelu wdrożenia, którym `Publish-AzureRmVMDscConfiguration` używa usługi Azure Resource Manager. 

`Publish-AzureVMDscConfiguration`przyjmuje w pliku konfiguracji, szuka zasoby zależne DSC i tworzy plik zip zawierający konfiguracji i zasoby DSC wymagane wprowadzenie konfiguracji. Można również utworzyć pakiet lokalnie za pomocą `-ConfigurationArchivePath` parametru. W przeciwnym razie publikuje plik zip do magazynu obiektów blob platformy Azure i zabezpiecza go z tokenem sygnatury dostępu Współdzielonego.

Plik zip utworzone przez to polecenie cmdlet ma skryptu konfiguracji .ps1 w katalogu głównym folderu archiwum. Zasoby mają folder modułu umieszczony w folderze archiwum. 

`Set-AzureVMDscExtension`injects ustawienia wymagane przez rozszerzenia DSC środowiska PowerShell do obiektu konfiguracji maszyny Wirtualnej. W klasycznym modelu wdrożenia, należy zastosować zmiany maszyny Wirtualnej na maszynie Wirtualnej platformy Azure z `Update-AzureVM`. 

`Get-AzureVMDscExtension`pobiera stan rozszerzenia usługi Konfiguracja DSC określonej maszyny wirtualnej. 

`Get-AzureVMDscExtensionStatus`pobiera stan konfiguracji DSC wdrożonych przez program obsługi rozszerzenia usługi Konfiguracja DSC. Akcję tę można wykonać na jednej maszyny Wirtualnej lub grupy maszyn wirtualnych.

`Remove-AzureVMDscExtension`Usuwa rozszerzenie obsługi z danej maszyny wirtualnej. To polecenie cmdlet jest **nie** Usuń konfigurację, odinstaluj WMF lub zmienić ustawienia zastosowane na maszynie wirtualnej. Powoduje usunięcie tylko obsługi rozszerzenia. 

**Podstawowe różnice w poleceniach cmdlet ASM i usługi Azure Resource Manager**

* Polecenia cmdlet Menedżera zasobów systemu Azure są synchroniczne. Polecenia cmdlet funkcji ASM są asynchroniczne.
* ResourceGroupName, VMName ArchiveStorageAccountName, wersji i lokalizacji są wszystkie wymagane parametry usługi Azure Resource Manager.
* ArchiveResourceGroupName jest nowy parametr opcjonalny dla usługi Azure Resource Manager. Można określić tego parametru, jeśli konta magazynu należy do innej grupie zasobów niż ten, gdy maszyna wirtualna została utworzona.
* ConfigurationArchive jest nazywany ArchiveBlobName usługi Azure Resource Manager
* Właściwość ContainerName jest nazywany ArchiveContainerName usługi Azure Resource Manager
* StorageEndpointSuffix jest nazywany ArchiveStorageEndpointSuffix usługi Azure Resource Manager
* Przełącznik AutoUpdate został dodany do usługi Azure Resource Manager umożliwia automatyczne aktualizowanie obsługi rozszerzenia do najnowszej wersji, jak i jest dostępny. Należy pamiętać, że ten parametr może potencjalnie spowodować ponowne uruchomienie na maszynie Wirtualnej po wydaniu nowej wersji programu WMF. 

## <a name="azure-portal-functionality"></a>Funkcje portalu Azure
Przejdź do maszyny Wirtualnej. W obszarze Ustawienia -> Ogólne kliknij polecenie "Rozszerzenia". Utworzono nowe okienko. Kliknij przycisk "Dodaj" i wybierz DSC środowiska PowerShell.

Portal wymaga danych wejściowych.
**Moduły konfiguracji lub skryptu**: to pole jest obowiązkowe. Wymaga pliku .ps1, zawierający skrypt konfiguracji lub plik zip ze skryptem konfiguracji .ps1 w katalogu głównym i wszystkimi zasobami zależnymi w folderach modułu w ramach zip. Mogą być tworzone z `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` zawarte w zestawie SDK Azure PowerShell polecenia cmdlet. Plik zip jest przekazywany do usługi magazynu obiektów blob użytkownika zabezpieczone przez tokenu sygnatury dostępu Współdzielonego. 

**Plik PSD1 danych konfiguracji**: to pole jest opcjonalne. Jeśli konfiguracja wymaga plik danych konfiguracji w psd1, to pole służy do zaznacz go i przekaż go do magazynu obiektów blob użytkownika, gdy jest zabezpieczony przez tokenu sygnatury dostępu Współdzielonego. 

**Kwalifikowanej nazwy konfiguracji**: ps1 pliki mogą mieć wiele funkcji konfiguracyjnych. Wprowadź nazwę skryptu ps1 konfiguracji, a następnie "\' i nazwą funkcji konfiguracji. Na przykład jeśli ps1 skrypt ma nazwę "configuration.ps1", a konfiguracja jest "IisInstall", możesz wprowadzić:`configuration.ps1\IisInstall`

**Argumenty konfiguracji**: Jeśli funkcja Konfiguracja przyjmuje argumenty, wprowadź je tutaj w formacie `argumentName1=value1,argumentName2=value2`. Należy zauważyć, że ten format jest innym formacie niż sposób konfiguracji argumenty są akceptowane za pomocą poleceń cmdlet programu PowerShell lub Menedżera zasobów szablonów. 

## <a name="getting-started"></a>Wprowadzenie
Rozszerzenia usługi Konfiguracja DSC Azure przyjmuje w dokumentach konfiguracji DSC i enacts ich na maszynach wirtualnych Azure. Prosty przykład konfiguracji jest zgodny. Zapisz go lokalnie jako "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Poniższe kroki umieść skrypt IisInstall.ps1 na określoną maszynę Wirtualną, wykonaj konfigurację i wysyłać raporty o stanie.
###<a name="classic-model"></a>Modelu klasycznego
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a>Model usługi Azure Resource Manager

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a>Rejestrowanie
Dzienniki są umieszczane w:

```
C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]
```

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać więcej informacji o konfiguracji DSC środowiska PowerShell [odwiedź Centrum dokumentacji programu PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Sprawdź [szablonu usługi Azure Resource Manager dla rozszerzenia DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Aby znaleźć dodatkowe funkcje, którymi można zarządzać w DSC środowiska PowerShell [przeglądać galerii programu PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) więcej zasobów usługi Konfiguracja DSC.

Aby uzyskać szczegółowe informacje na przekazywanie parametrów poufnych do konfiguracji, zobacz [Zarządzanie poświadczenia bezpiecznie z obsługą rozszerzenia DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

