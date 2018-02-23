---
title: "Zestawy skalowania maszyny wirtualnej platformy Azure szyfrowania dysków | Dokumentacja firmy Microsoft"
description: "Informacje o szyfrowaniu dysków dołączonych w zestawy skalowania maszyny wirtualnej."
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: iainfou
ms.openlocfilehash: dddcece9f7566961b256369330661e5dbd5d4665
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set"></a>Szyfrowanie systemu operacyjnego i dysków dołączonych danych w zestawie skalowania maszyn wirtualnych
Azure [zestawy skalowania maszyny wirtualnej](/azure/virtual-machine-scale-sets/) obsługuje szyfrowanie dysków Azure (ADE).  Można włączyć szyfrowanie dysków Azure dla systemu Windows i Linux zestawach skali maszyn wirtualnych do ochrony i skali w zabezpieczaniu ustawia dane przechowywane przy użyciu szyfrowania standardowe technologie. Aby uzyskać więcej informacji przeczytaj maszyny wirtualne Azure dysku szyfrowanie dla systemu Windows i Linux.

> [!NOTE]
>  Szyfrowanie dysków Azure dla zestawy skalowania maszyny wirtualnej jest obecnie w publicznej wersji zapoznawczej, dostępne we wszystkich regionach publicznej Azure.

Szyfrowanie dysków Azure jest obsługiwane:
- Skala zestawów utworzonych za pomocą dysków zarządzanych i nieobsługiwane dla zestawów skalowania dysk lokalny (lub niezarządzanego).
- w przypadku woluminów systemu operacyjnego i danych w zestawach skali systemu Windows. Wyłącz szyfrowanie jest obsługiwane w woluminach systemu operacyjnego i danych dla zestawów skalowania systemu Windows.
- woluminy danych w zestawach skali systemu Linux. Szyfrowanie dysku systemu operacyjnego nie jest obsługiwane w bieżącej wersji zapoznawczej dla zestawów skalowania systemu Linux.

Zestaw maszyn wirtualnych odtworzenia z obrazu i uaktualniania operacji skalowania nie są obsługiwane w bieżącej wersji zapoznawczej. Szyfrowanie dysków Azure dla podglądu zestawy skalowania maszyny wirtualnej jest zalecane tylko w środowiskach testowych. W wersji zapoznawczej nie należy włączać szyfrowanie dysków w środowiskach produkcyjnych, w których konieczne może być uaktualnienia obrazu systemu operacyjnego w zestawie skalowania zaszyfrowane.

## <a name="prerequisites"></a>Wymagania wstępne
Zainstaluj najnowsze wersje [programu Azure Powershell](https://github.com/Azure/azure-powershell/releases), który zawiera polecenia szyfrowania.

Szyfrowania dysków Azure w celu Podgląd zestawów skali maszyny wirtualnej, należy zarejestrować się samodzielnie subskrypcji przy użyciu następujących poleceń programu PowerShell: 

```powershell
Login-AzureRmAccount
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Poczekaj około 10 minut, aż do stanu "Zarejestrowanej" jest zwracany za pomocą następującego polecenia: 

```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Tworzenie magazynu kluczy Azure włączone szyfrowanie dysków
Utwórz nowy magazyn kluczy w tej samej subskrypcji i regionu jako skali ustawiona i ustawić zasady dostępu "EnabledForDiskEncryption".

```powershell
$rgname="windatadiskencryptiontest"
$VaultName="encryptionvault321"

New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $rgName -Location southcentralus -EnabledForDiskEncryption
``` 

Lub włączyć istniejący magazyn kluczy w tej samej subskrypcji i regionu jako zestaw szyfrowanie dysków skalowania.

```powershell
$VaultName="encryptionvault321"
Set-AzureRmKeyVaultAccessPolicy -VaultName $VaultName -EnabledForDiskEncryption
```

## <a name="enable-encryption"></a>Włącz szyfrowanie
Następujące polecenia zaszyfrować dysk z danymi w skali uruchomionych ustawić za pomocą magazynu kluczy w tej samej grupie zasobów. Umożliwia także szablony do szyfrowania dysków w uruchomionej [zestaw skali Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) lub [zestaw skali Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
$DiskEncryptionKeyVaultUrl="https://encryptionvault321.vault.azure.net"
$KeyVaultResourceId="/subscriptions/0754ecc2-d80d-426a-902c-b83f4cfbdc95/resourceGroups/windatadiskencryptiontest/providers/Microsoft.KeyVault/vaults/encryptionvault321"

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName `
    -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId –VolumeType Data
```

## <a name="check-encryption-progress"></a>Sprawdzić postęp szyfrowania
Użyj następujących poleceń w celu określenia stanu szyfrowania zestawu skali.

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName -InstanceId "4"
```

## <a name="disable-encryption"></a>Wyłączenie szyfrowania
Wyłączenie szyfrowania w skali uruchomionej maszyny wirtualnej ustawić za pomocą następujących poleceń. Umożliwia także szablony umożliwia wyłączenie szyfrowania uruchomione [zestaw skali Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows) lub [zestaw skali Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux).

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName
```
