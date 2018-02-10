---
title: "Konfiguracja żądanego stanu dla platformy Azure — omówienie | Dokumentacja firmy Microsoft"
description: "Omówienie dotyczące korzystania z programu Microsoft Azure rozszerzenia obsługi dla konfiguracji żądanego stanu programu PowerShell. W tym wymagania wstępne, architektura, polecenia cmdlet."
services: virtual-machines-windows
documentationcenter: 
author: mgreenegit
manager: timlt
editor: 
tags: azure-resource-manager
keywords: DSC
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 02/02/2018
ms.author: migreene
ms.openlocfilehash: ed8b5bc54baa3a5abfb596b202f0af58e1b6c74f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Wprowadzenie do obsługi rozszerzenia konfiguracji żądanego stanu usługi Azure

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Agent maszyny Wirtualnej i skojarzonych rozszerzeń są częścią usługi infrastruktury platformy Microsoft Azure.
Rozszerzenia maszyny Wirtualnej są składniki oprogramowania, które zapewniają rozszerzenie funkcjonalności maszyny Wirtualnej i uproszczenia różne operacje zarządzania maszyny Wirtualnej.

Przypadek użycia serwera podstawowego do rozszerzenia konfiguracji żądanego stanu maszyny Wirtualnej, aby zainicjować [usługi Konfiguracja DSC automatyzacji Azure](../../automation/automation-dsc-overview.md) zapewnia [korzyści](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig.md#pull-service) tym bieżące zarządzanie konfiguracji maszyny Wirtualnej i integracja z innych operacyjne narzędzi, takich jak monitorowanie Azure.

Istnieje również możliwość użycia rozszerzenia DSC niezależnie od usługi Konfiguracja DSC automatyzacji Azure, jednak jest to akcja pojedynczej, która występuje podczas wdrażania, a nie trwającą raportowania lub Zarządzanie konfiguracją innych niż lokalnie w Maszynie wirtualnej.

Ten artykuł zawiera informacje dotyczące scenariuszy, rozszerzenia DSC do dołączenia do usługi Automatyzacja Azure i sposobu użycia rozszerzenia DSC jako narzędzie do przypisywanie konfiguracji do maszyn wirtualnych przy użyciu zestawu SDK platformy Azure.

## <a name="prerequisites"></a>Wymagania wstępne

- **Komputer lokalny** — wchodzić w interakcje z rozszerzenia maszyny Wirtualnej Azure, musisz użyć portalu Azure lub zestaw SDK usługi Azure PowerShell.
- **Agent gościa** -Azure maszyny Wirtualnej skonfigurowaną przy konfiguracji DSC musi być systemu operacyjnego, który obsługuje Windows Management Framework (WMF) 4.0 lub nowszy. Pełną listę obsługiwanych wersji systemu operacyjnego można znaleźć na stronie [Historia wersji rozszerzenia DSC](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Definicje terminów i pojęć

W tym przewodniku zakłada się znajomość następujące kwestie:

- **Konfiguracja** — dokumentacja konfiguracji A DSC.
- **Węzeł** -obiektu docelowego dla konfiguracji DSC. W tym dokumencie na zawsze "węzła" odwołuje się do maszyny Wirtualnej platformy Azure.
- **Dane konfiguracji** — plik psd1 zawierający środowiska dane konfiguracji.

## <a name="architectural-overview"></a>Omówienie architektury

Rozszerzenia usługi Konfiguracja DSC Azure używa framework Agent maszyny Wirtualnej do dostarczania, wprowadza i raport o konfiguracji DSC uruchomionych na maszynach wirtualnych platformy Azure.
Rozszerzenia DSC akceptuje dokument konfiguracji i zestaw parametrów.
Jeśli plik nie zostanie podany, [domyślne skryptu konfiguracji](###default-configuration-script) jest osadzony z rozszerzeniem, która jest używana tylko w przypadku ustawienia metadanych w [lokalny program Configuration Manager](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig).

Jeśli rozszerzenie jest wywoływana po raz pierwszy, instalowana wersja programu Windows Management Framework (WMF) przy użyciu z następującą logiką:

1. Jeśli system operacyjny maszyny Wirtualnej Azure jest Windows Server 2016, nie podjęto żadnej akcji. Windows Server 2016 ma już najnowszą wersję programu PowerShell jest zainstalowane.
2. Jeśli `wmfVersion` właściwość zostanie określony, ta wersja platformy WMF jest zainstalowane, chyba że nie jest zgodny z systemem operacyjnym maszyny Wirtualnej.
3. Jeśli nie `wmfVersion` określona właściwość, jest zainstalowana najnowsza wersja dotyczy wersja programu WMF.

Instalacja programu WMF wymaga ponownego uruchomienia komputera.
Po ponownym uruchomieniu, rozszerzenie pliki do pobrania określonego w pliku .zip `modulesUrl` właściwości, jeśli zostanie podana.
Jeśli ta lokalizacja znajduje się w magazynie obiektów blob platformy Azure, można określić tokenu sygnatury dostępu Współdzielonego w `sasToken` właściwości dostępu do tego pliku.
Po pobraniu zip i rozpakowane, funkcja konfiguracji zdefiniowana w `configurationFunction` uruchamiany w celu wygenerowania pliku MOF.
Następnie uruchamia rozszerzenia `Start-DscConfiguration -Force` przy użyciu wygenerowanego pliku MOF.
Rozszerzenie przechwytuje dane wyjściowe i zapisuje go w kanale stan Azure.

### <a name="default-configuration-script"></a>Skrypt konfiguracji domyślnej

Rozszerzenia DSC Azure zawiera domyślną konfigurację skryptów przeznaczony do użycia podczas dołączania maszyny Wirtualnej do usługi Konfiguracja DSC automatyzacji Azure.
Parametry skryptu są zgodne z można skonfigurować właściwości [lokalny program Configuration Manager](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig).
Parametry skryptu są [udokumentowane](extensions-dsc-template.md##default-configuration-script) i pełne skrypt jest dostępny w [GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip?raw=true).

## <a name="dsc-extension-in-arm-templates"></a>Rozszerzenia usługi Konfiguracja DSC w szablonów ARM

Szablony wdrażania usługi Azure Resource Manager (ARM) jest oczekiwany sposób pracy z rozszerzenia DSC w przypadku większości scenariuszy.
Aby informacje i przykłady, w tym rozszerzenia usługi Konfiguracja DSC w szablonach wdrożenia ARM, zobacz stronę dokumentacji dedykowany [żądany stan konfiguracji rozszerzenia przy użyciu szablonów usługi Azure Resource Manager](extensions-dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>Polecenia cmdlet programu PowerShell rozszerzenia usługi Konfiguracja DSC

Polecenia cmdlet programu PowerShell do zarządzania rozszerzenia DSC są najlepiej nadaje się do interaktywnego Rozwiązywanie problemów i scenariusze zbierania informacji.
Polecenia cmdlet programu może służyć do pakietu, publikowanie i monitorować wdrożenia rozszerzenia usługi Konfiguracja DSC.
Należy pamiętać, że polecenia cmdlet dla rozszerzenia DSC jeszcze nie zostały zaktualizowane do pracy z [domyślny skrypt konfiguracji](###default-configuration-script).

`Publish-AzureRMVMDscConfiguration`przyjmuje w pliku konfiguracji, szuka zasoby zależne DSC i tworzy plik zip zawierający konfiguracji i zasoby DSC wymagane wprowadzenie konfiguracji.
Można również utworzyć pakiet lokalnie za pomocą `-ConfigurationArchivePath` parametru.
W przeciwnym razie publikuje plik zip do magazynu obiektów blob platformy Azure i zabezpiecza go z tokenem sygnatury dostępu Współdzielonego.

Plik zip utworzone przez to polecenie cmdlet ma skryptu konfiguracji .ps1 w katalogu głównym folderu archiwum.
Zasoby mają folder modułu umieszczony w folderze archiwum.

`Set-AzureRMVMDscExtension`injects ustawienia wymagane przez rozszerzenia DSC środowiska PowerShell do obiektu konfiguracji maszyny Wirtualnej.

`Get-AzureRMVMDscExtension`pobiera stan rozszerzenia usługi Konfiguracja DSC określonej maszyny wirtualnej.

`Get-AzureRMVMDscExtensionStatus`pobiera stan konfiguracji DSC wdrożonych przez program obsługi rozszerzenia usługi Konfiguracja DSC.
Akcję tę można wykonać na jednej maszyny Wirtualnej lub grupy maszyn wirtualnych.

`Remove-AzureRMVMDscExtension`Usuwa rozszerzenie obsługi z danej maszyny wirtualnej.
To polecenie cmdlet jest **nie** Usuń konfigurację, odinstaluj WMF lub zmienić ustawienia zastosowane na maszynie wirtualnej.
Powoduje usunięcie tylko obsługi rozszerzenia. 

Ważne informacje dotyczące polecenia cmdlet rozszerzenia DSC AzureRM:

- Polecenia cmdlet Menedżera zasobów systemu Azure są synchroniczne.
- ResourceGroupName, VMName ArchiveStorageAccountName, wersji i lokalizacji są wszystkie wymagane parametry.
- ArchiveResourceGroupName jest parametrem opcjonalnym. Można określić tego parametru, jeśli konta magazynu należy do innej grupie zasobów niż ten, gdy maszyna wirtualna została utworzona.
- Przełącznik AutoUpdate umożliwia automatyczne aktualizowanie obsługi rozszerzenia do najnowszej wersji, jak i jest dostępny. Należy pamiętać, że ten parametr może potencjalnie spowodować ponowne uruchomienie na maszynie Wirtualnej po wydaniu nowej wersji programu WMF.

### <a name="getting-started-with-cmdlets"></a>Wprowadzenie do poleceń cmdlet

Rozszerzenia usługi Konfiguracja DSC Azure jest w stanie bezpośrednio do skonfigurowania maszyny wirtualne Azure podczas wdrażania, mimo że to nie będą rejestrować się w węźle, aby usługi Automatyzacja Azure, węzeł zostanie za pomocą dokumentów konfiguracji DSC **nie*— można centralnie zarządzanego.

Prosty przykład konfiguracji jest zgodny.
Zapisz go lokalnie jako "IisInstall.ps1":

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

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.72 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"
```

## <a name="azure-portal-functionality"></a>Funkcje portalu Azure

Przejdź do maszyny Wirtualnej. W obszarze Ustawienia -> Ogólne kliknij polecenie "Rozszerzenia".
Utworzono nowe okienko.
Kliknij przycisk "Dodaj" i wybierz DSC środowiska PowerShell.

Portal wymaga danych wejściowych.
**Moduły konfiguracji lub skryptu**: to pole jest obowiązkowe (formularza nie został jeszcze zaktualizowany do [domyślny skrypt konfiguracji](###default-configuration-script).
Wymaga pliku .ps1, zawierający skrypt konfiguracji lub plik zip ze skryptem konfiguracji .ps1 w katalogu głównym i wszystkimi zasobami zależnymi w folderach modułu w ramach zip.
Mogą być tworzone z `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` zawarte w zestawie SDK Azure PowerShell polecenia cmdlet.
Plik zip jest przekazywany do usługi magazynu obiektów blob użytkownika zabezpieczone przez tokenu sygnatury dostępu Współdzielonego.

**Plik PSD1 danych konfiguracji**: to pole jest opcjonalne.
Jeśli konfiguracja wymaga plik danych konfiguracji w psd1, to pole służy do zaznacz go i przekaż go do magazynu obiektów blob użytkownika, gdy jest zabezpieczony przez tokenu sygnatury dostępu Współdzielonego.

**Kwalifikowanej nazwy konfiguracji**: ps1 pliki mogą mieć wiele funkcji konfiguracyjnych.
Wprowadź nazwę skryptu ps1 konfiguracji, a następnie "\' i nazwą funkcji konfiguracji.
Na przykład jeśli ps1 skrypt ma nazwę "configuration.ps1", a konfiguracja jest "IisInstall", możesz wprowadzić:`configuration.ps1\IisInstall`

**Argumenty konfiguracji**: Jeśli funkcja Konfiguracja przyjmuje argumenty, wprowadź je tutaj w formacie `argumentName1=value1,argumentName2=value2`.
Należy zauważyć, że ten format jest innym formacie niż sposób konfiguracji argumenty są akceptowane za pomocą poleceń cmdlet programu PowerShell lub Menedżera zasobów szablonów.

## <a name="logging"></a>Rejestrowanie
Dzienniki są umieszczane w:

```powerShell
C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Version Number]
```

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać więcej informacji o konfiguracji DSC środowiska PowerShell [odwiedź Centrum dokumentacji programu PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Sprawdź [szablonu usługi Azure Resource Manager dla rozszerzenia DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Aby znaleźć dodatkowe funkcje, którymi można zarządzać w DSC środowiska PowerShell [przeglądać galerii programu PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) więcej zasobów usługi Konfiguracja DSC.

Aby uzyskać szczegółowe informacje na przekazywanie parametrów poufnych do konfiguracji, zobacz [Zarządzanie poświadczenia bezpiecznie z obsługą rozszerzenia DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
