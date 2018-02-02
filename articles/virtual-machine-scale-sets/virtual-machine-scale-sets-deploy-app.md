---
title: "Wdrażanie aplikacji do zestawu skalowania maszyny wirtualnej platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrażać aplikacji dla systemu Linux i Windows wystąpień maszyn wirtualnych w zestawie skalowania"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: iainfou
ms.openlocfilehash: 288bcdf6628f60d0b08fe151e630784d665db56f
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Wdrażanie aplikacji na zestawy skalowania maszyny wirtualnej
Do uruchamiania aplikacji na wystąpień maszyn wirtualnych (VM) w zestawie skalowania, najpierw należy zainstalować składniki aplikacji i wymaganych plików. W tym artykule przedstawiono sposoby budowania niestandardowego obrazu maszyny Wirtualnej dla wystąpień w skali ustawić lub automatycznego uruchamiania skryptów instalacji na istniejących wystąpień maszyny Wirtualnej. Możesz również sposób zarządzania aplikacji lub aktualizacji systemu operacyjnego przez zestaw skali.


## <a name="build-a-custom-vm-image"></a>Tworzenie niestandardowego obrazu maszyny Wirtualnej
Korzystając z jednego z obrazów platformy Azure można utworzyć wystąpienia w zestawie skali, żadne dodatkowe oprogramowanie jest zainstalowane lub skonfigurowane. Można zautomatyzować instalację tych składników, jednak dodający się czas wymagany do obsługi administracyjnej wystąpień maszyn wirtualnych do sieci zestawy skalowania. Wiele zmian konfiguracji w przypadku zastosowania do wystąpień maszyn wirtualnych, brak zarządzania nakłady pracy związane z tymi skrypty do konfiguracji i zadań.

Aby zmniejszyć Zarządzanie konfiguracją i czas do udostępnienia maszyny Wirtualnej, można utworzyć niestandardowy obraz maszyny Wirtualnej, który jest gotowy do uruchomienia aplikacji natychmiast po udostępnieniu wystąpienia w zestawie skalowania. Ogólny proces tworzenia niestandardowego obrazu maszyny Wirtualnej do skalowania wystąpień zestawu są następujące:

1. Do tworzenia niestandardowego obrazu maszyny Wirtualnej z skali zestaw wystąpień, możesz utworzyć i zaloguj się do maszyny Wirtualnej, a następnie zainstaluj i skonfiguruj aplikacji. Służy do definiowania i kompilacji pakujący [Linux](../virtual-machines/linux/build-image-with-packer.md) lub [Windows](../virtual-machines/windows/build-image-with-packer.md) obrazu maszyny Wirtualnej. Lub można ręcznie utworzyć i skonfigurować maszyny Wirtualnej:

    - Utwórz Maszynę wirtualną systemu Linux z [Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md), [programu Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md), lub [portal](../virtual-machines/linux/quick-create-portal.md).
    - Utwórz maszynę Wirtualną systemu Windows za pomocą [programu Azure PowerShell](../virtual-machines/windows/quick-create-powershell.md), [Azure CLI 2.0](../virtual-machines/windows/quick-create-cli.md), lub [portal](../virtual-machines/windows/quick-create-portal.md).
    - Zaloguj się do [Linux](../virtual-machines/linux/mac-create-ssh-keys.md#use-the-ssh-key-pair) lub [Windows](../virtual-machines/windows/connect-logon.md) maszyny Wirtualnej.
    - Zainstaluj i skonfiguruj aplikacje i narzędzia niezbędne. Jeśli potrzebujesz określonej wersji biblioteki lub środowisko uruchomieniowe, niestandardowy obraz maszyny Wirtualnej można zdefiniować w wersji i 

2. Przechwytywanie maszyny Wirtualnej z [Azure CLI 2.0](../virtual-machines/linux/capture-image.md) lub [programu Azure PowerShell](../virtual-machines/windows/capture-image.md). Spowoduje to utworzenie niestandardowego obrazu maszyny Wirtualnej, służący do następnie wdrażanie wystąpień w zestawie skalowania.

3. [Utwórz zestaw skali](virtual-machine-scale-sets-create.md) i określić niestandardowy obraz maszyny Wirtualnej utworzone w poprzednich krokach.


## <a name="already-provisioned"></a>Instalowanie aplikacji z rozszerzeniem niestandardowego skryptu
Niestandardowe rozszerzenie skryptu pobiera i uruchamia skrypty na maszynach wirtualnych Azure. To rozszerzenie jest przydatne w przypadku konfiguracji po wdrożeniu, instalowania oprogramowania lub każdego innego zadania związanego z konfiguracją lub zarządzaniem. Skrypty można pobrać z usługi Azure Storage lub GitHub bądź można je dostarczyć do witryny Azure Portal w czasie wykonywania rozszerzenia.

Rozszerzenie skryptu niestandardowego integruje się z szablonów usługi Azure Resource Manager i mogą być także uruchamiane przy użyciu wiersza polecenia platformy Azure, programu PowerShell, portalu Azure lub interfejsu API REST dla maszyny wirtualnej Azure. 

Aby uzyskać więcej informacji, zobacz [Omówienie niestandardowego rozszerzenia skryptu](../virtual-machines/windows/extensions-customscript.md).


### <a name="use-azure-powershell"></a>Korzystanie z programu Azure PowerShell
PowerShell korzysta z tablicy skrótów do przechowywania pliku do pobrania i polecenia do wykonania. Poniższy przykład:

- Powoduje, że wystąpień maszyn wirtualnych do pobrania z witryny GitHub - skryptu *https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1*
- Ustawia na uruchomienie skryptu install - rozszerzenia`powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1`
- Pobiera informacje o skali ustawiony za pomocą [Get AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- Dotyczy rozszerzenia wystąpień maszyn wirtualnych z [AzureRmVmss aktualizacji](/powershell/module/azurerm.compute/update-azurermvmss)

Niestandardowe rozszerzenie skryptu jest stosowany do *myScaleSet* wystąpień maszyn wirtualnych w grupie zasobów o nazwie *myResourceGroup*. Wprowadź własne nazwy w następujący sposób:

```powershell
# Define the script for your Custom Script Extension to run
$customConfig = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Custom Script Extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $customConfig

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

W przypadku zasad uaktualniania na zestawie skali *ręczne*, zaktualizować wystąpień maszyn wirtualnych z [AzureRmVmssInstance aktualizacji](/powershell/module/azurerm.compute/update-azurermvmssinstance). To polecenie cmdlet ma zastosowanie do konfiguracji zestaw skalowania zaktualizowane do wystąpień maszyn wirtualnych i instaluje aplikację.


### <a name="use-azure-cli-20"></a>Użyj interfejsu wiersza polecenia platformy Azure 2.0
Aby niestandardowe rozszerzenie skryptu za pomocą interfejsu wiersza polecenia Azure, należy utworzyć plik JSON, który definiuje, jakie pliki, aby uzyskać i poleceń do wykonania. Te definicje JSON mogą być ponownie używane we wszystkich wdrożeniach zestaw skalowania, aby zastosować spójne aplikacji instaluje.

W bieżącym powłoki, Utwórz plik o nazwie *customConfig.json* i wklej następującą konfigurację. Na przykład utwórz plik w usłudze Cloud Shell, a nie na maszynie lokalnej. Można użyć dowolnego edytora, którego chcesz. Wprowadź `sensible-editor cloudConfig.json`, aby utworzyć plik i wyświetlić listę dostępnych edytorów.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx.sh"],
  "commandToExecute": "./automate_nginx.sh"
}
```

Zastosuj konfigurację niestandardowe rozszerzenie skryptu do wystąpień maszyn wirtualnych w Twojej zestaw z skalowania [az vmss rozszerzenia zestawu](/cli/azure/vmss/extension#az_vmss_extension_set). Następujący przykład dotyczy *customConfig.json* konfigurację, aby *myScaleSet* wystąpień maszyn wirtualnych w grupie zasobów o nazwie *myResourceGroup*. Wprowadź własne nazwy w następujący sposób:

```azurecli
az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroup \
    --vmss-name myScaleSet \
    --settings @customConfig.json
```

W przypadku zasad uaktualniania na zestawie skali *ręczne*, zaktualizować wystąpień maszyn wirtualnych z [vmss az update wystąpienia](/cli/azure/vmss#update-instances). To polecenie cmdlet ma zastosowanie do konfiguracji zestaw skalowania zaktualizowane do wystąpień maszyn wirtualnych i instaluje aplikację.


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>Instalowanie aplikacji do systemu Windows maszyny Wirtualnej przy użyciu programu PowerShell DSC
[Konfiguracji żądanego stanu środowiska PowerShell (DSC)](https://msdn.microsoft.com/en-us/powershell/dsc/overview) to platforma zarządzania, aby zdefiniować konfigurację komputerów docelowych. Konfiguracji DSC zdefiniować, co należy zainstalować na maszynie oraz sposób konfigurowania hosta. Aparat lokalnego Configuration Manager (LCM) działa na każdym węźle docelowym, który przetwarza żądane akcje wciśnięcia konfiguracji.

Rozszerzenia DSC środowiska PowerShell umożliwia dostosowanie wystąpień maszyn wirtualnych w skali ustawić przy użyciu programu PowerShell. Poniższy przykład:

- Powoduje, że wystąpień maszyny Wirtualnej, aby pobrać pakiet DSC z serwisu GitHub - *https://github.com/iainfoulds/azure-samples/raw/master/dsc.zip*
- Ustawia na uruchomienie skryptu install - rozszerzenia`configure-http.ps1`
- Pobiera informacje o skali ustawiony za pomocą [Get AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- Dotyczy rozszerzenia wystąpień maszyn wirtualnych z [AzureRmVmss aktualizacji](/powershell/module/azurerm.compute/update-azurermvmss)

Rozszerzenia usługi Konfiguracja DSC jest stosowany do *myScaleSet* wystąpień maszyn wirtualnych w grupie zasobów o nazwie *myResourceGroup*. Wprowadź własne nazwy w następujący sposób:

```powershell
# Define the script for your Desired Configuration to download and run
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/iainfoulds/azure-samples/raw/master/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Desired State Configuration extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Publisher Microsoft.Powershell `
    -Type DSC `
    -TypeHandlerVersion 2.24 `
    -Name "DSC" `
    -Setting $dscConfig

# Update the scale set and apply the Desired State Configuration extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet"  `
    -VirtualMachineScaleSet $vmss
```

W przypadku zasad uaktualniania na zestawie skali *ręczne*, zaktualizować wystąpień maszyn wirtualnych z [AzureRmVmssInstance aktualizacji](/powershell/module/azurerm.compute/update-azurermvmssinstance). To polecenie cmdlet ma zastosowanie do konfiguracji zestaw skalowania zaktualizowane do wystąpień maszyn wirtualnych i instaluje aplikację.


## <a name="install-an-app-to-a-linux-vm-with-cloud-init"></a>Instalowanie aplikacji do maszyny Wirtualnej systemu Linux z inicjowaniem chmury
[Cloud-init](https://cloudinit.readthedocs.io/latest/) to powszechnie używana metoda dostosowywania maszyny wirtualnej z systemem Linux podczas jej pierwszego rozruchu. Za pomocą pakietu cloud-init można instalować pakiety i zapisywać pliki lub konfigurować użytkowników i zabezpieczenia. Pakiet cloud-init jest uruchamiany w trakcie początkowego rozruchu, więc do zastosowania konfiguracji nie są wymagane żadne dodatkowe kroki ani agenci.

Init chmury działa także w dystrybucji. Na przykład nie używaj **instalacji stanie get** lub **yum zainstalować** do zainstalowania pakietu. Zamiast tego można zdefiniować listę pakietów do zainstalowania. Init chmury automatycznie używa narzędzia do zarządzania natywnego pakietu dla distro, którą wybierzesz.

Aby uzyskać więcej informacji, w tym przykładzie *init.txt chmury* plików, zobacz [umożliwia dostosowywanie maszyn wirtualnych platformy Azure w chmurze init](../virtual-machines/linux/using-cloud-init.md).

Aby utworzyć zestaw skali i przy użyciu pliku init chmury, Dodaj `--custom-data` parametr [az vmss utworzyć](/cli/azure/vmss#az_vmss_create) polecenia i określ nazwę pliku init chmury. Poniższy przykład tworzy zestaw o nazwie skalowania *myScaleSet* w *myResourceGroup* i konfiguruje wystąpień maszyn wirtualnych przy użyciu pliku o nazwie *init.txt chmury*. Wprowadź własne nazwy w następujący sposób:

```azurecli
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```


## <a name="install-applications-as-a-set-scales-out"></a>Zainstaluj aplikacje zgodnie z zestawem skaluje się
Zestawy skalowania umożliwiają zwiększenie liczby wystąpień maszyn wirtualnych, których uruchamiana jest aplikacja. Ten skalowania w poziomie procesu mogą być uruchamiane ręcznie lub automatycznie w oparciu metryki, takie jak użycie procesora CPU lub pamięci.

Niestandardowe rozszerzenie skryptu jest stosowane do zestawu skalowania, aplikacja jest zainstalowana do każdego nowego wystąpienia maszyny Wirtualnej. Jeśli zestaw skalowania jest oparta na niestandardowego obrazu z wcześniej zainstalowaną aplikacją, każde nowe wystąpienie maszyny Wirtualnej jest wdrażana w stanie używać. 

Jeśli wystąpień maszyny Wirtualnej zestawu skali hostów kontenera, służy niestandardowe rozszerzenie skryptu do pobierania i uruchamiania konieczność kontener obrazów. Rozszerzenia niestandardowego skryptu można również zarejestrować nowe wystąpienie maszyny Wirtualnej z programem orchestrator, takich jak usługi kontenera platformy Azure.


## <a name="deploy-application-updates"></a>Wdrażać aktualizacje aplikacji
Po aktualizacji z kodu aplikacji, bibliotek lub pakietów, możesz wypchnąć najnowszy stan aplikacji do wystąpień maszyn wirtualnych w zestawie skalowania. Jeśli używasz niestandardowe rozszerzenie skryptu, aktualizacje aplikacji i nie są automatycznie wdrażane. Zmień konfigurację niestandardowego skryptu, takie jak polecenie do skryptu instalacji, który ma nazwę zaktualizowanej wersji. W poprzednim przykładzie, niestandardowe rozszerzenie skryptu używa skryptu o nazwie *automate_nginx.sh* w następujący sposób:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx.sh"],
  "commandToExecute": "./automate_nginx.sh"
}
```

Wszelkie aktualizacje wprowadzone do tej aplikacji nie są widoczne dla niestandardowe rozszerzenie skryptu, chyba że który zainstalować zmiany skryptu. Jednym z podejść jest uwzględnienie numer wersji, który zwalnia zwiększa z aplikacją. Rozszerzenia niestandardowego skryptu można odwoływać się *automate_nginx_v2.sh* w następujący sposób:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx_v2.sh"],
  "commandToExecute": "./automate_nginx_v2.sh"
}
```

Niestandardowe rozszerzenie skryptu jest uruchamiana teraz dla wystąpień maszyn wirtualnych w celu zastosowania najnowszych aktualizacji aplikacji.


### <a name="install-applications-with-os-updates"></a>Instalowanie aplikacji z aktualizacje systemu operacyjnego
Gdy dostępne są nowe wersje systemu operacyjnego, możesz użyć lub utworzyć obraz niestandardowy i [wdrażanie uaktualnień systemu operacyjnego](virtual-machine-scale-sets-upgrade-scale-set.md) skali ustawić. Każde wystąpienie maszyny Wirtualnej zostanie uaktualniony do najnowszej obrazu, który określisz. Niestandardowy obraz z aplikacją preinstalowanym, niestandardowe rozszerzenie skryptu lub DSC środowiska PowerShell umożliwia ma automatycznie dostępny aplikacji, jak przeprowadzić uaktualnienie. Może być konieczne planowanie konserwacji aplikacji wykonanie tego procesu, aby upewnić się, że nie ma żadnych wersji problemy ze zgodnością.

Jeśli używasz niestandardowego obrazu maszyny Wirtualnej z wcześniej zainstalowaną aplikację może integrować aktualizacje aplikacji z potokiem wdrożenia do tworzenia nowych obrazów i wdrażania uaktualnień systemu operacyjnego na zestawie skali. Takie podejście umożliwia potoku do odebrania najnowszej kompilacji aplikacji, Utwórz i Zweryfikuj obrazu maszyny Wirtualnej, a następnie uaktualnić wystąpień maszyny Wirtualnej w zestawie skalowania. Aby uruchomić potok wdrożenia, który tworzy i wdraża aktualizacje aplikacji w niestandardowych obrazów maszyn wirtualnych, można użyć [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [Spinnaker](https://www.spinnaker.io/), lub [Wpięć](https://jenkins.io/) .


## <a name="next-steps"></a>Kolejne kroki
Jak skompilować i wdrożyć aplikacje na Twoje zestawy skalowania, możesz przejrzeć [Omówienie projektowania zestaw skali](virtual-machine-scale-sets-design-overview.md). Aby uzyskać więcej informacji na temat zarządzania zestaw skalowania, zobacz [Użyj programu PowerShell do zarządzania zestawie skali](virtual-machine-scale-sets-windows-manage.md).
