---
title: "Znajdowanie i usuwanie odłączyć dysku systemu Azure zarządzane i niezarządzane | Dokumentacja firmy Microsoft"
description: "Jak znaleźć i usunąć odłączyć dysku systemu Azure zarządzane i niezarządzane (pliki VHD/stronicowych obiektów blob), za pomocą programu Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: a846d3578d40b19762f185381c92bdf8e225b185
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Znajdowanie i usuwanie odłączyć dysku systemu Azure zarządzane i niezarządzane
Po usunięciu maszyny wirtualnej na platformie Azure dysków dołączonych do niego nie są usuwane domyślnie. Zapobiega utracie danych z powodu usunięty przez pomyłkę maszyn wirtualnych, ale nadal niepotrzebnie opłacać odłączyć dysków. Użyj w tym artykule, aby znaleźć i usunąć wszystkie dyski odłączyć i zapisać koszt. 


## <a name="find-and-delete-unattached-managed-disks"></a>Znajdowanie i usuwanie odłączyć dysków zarządzanych 

Poniższy skrypt pokazuje, jak znaleźć niedołączonej [dysków zarządzanych](managed-disks-overview.md) za pomocą *zarządzane* właściwości. Wszystkie zarządzane dyski w subskrypcji i kontroli w pętli *zarządzane* właściwość ma wartość null, aby znaleźć odłączyć dysków zarządzanych. *Zarządzane* właściwość przechowuje identyfikator zasobu maszyny wirtualnej, do której jest dołączona dysku zarządzanego.

Zdecydowanie zalecamy do pierwszego uruchomienia skryptu przez ustawienie *deleteUnattachedDisks* zmiennej na 0, aby wyświetlić wszystkie odłączyć dysków. Po zapoznaniu się odłączyć dysków, uruchom skrypt przez ustawienie *deleteUnattachedDisks* 1, aby usunąć wszystkie odłączyć dysków.

```azurepowershell-interactive

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
$deleteUnattachedDisks=0

$managedDisks = Get-AzureRmDisk

foreach ($md in $managedDisks) {
    
    # ManagedBy property stores the Id of the VM to which Managed Disk is attached to
    # If ManagedBy property is $null then it means that the Managed Disk is not attached to a VM
    if($md.ManagedBy -eq $null){

        if($deleteUnattachedDisks -eq 1){
            
            Write-Host "Deleting unattached Managed Disk with Id: $($md.Id)"

            $md | Remove-AzureRmDisk -Force

            Write-Host "Deleted unattached Managed Disk with Id: $($md.Id) "

        }else{

            $md.Id

        }
           
    }
     
 } 
```
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Znajdowanie i usuwanie odłączyć dysków niezarządzane 

Niezarządzane dyski znajdują się pliki VHD przechowywane jako [stronicowe obiekty BLOB] (/ rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) w [konta usługi Azure Storage](../../storage/common/storage-create-storage-account.md). Poniższy skrypt pokazuje, jak znaleźć odłączyć dysków niezarządzane (stronicowych obiektów blob) przy użyciu *LeaseStatus* właściwości. Wszystkie dyski niezarządzane we wszystkich magazyn kont w ramach subskrypcji i sprawdza, czy w pętli *LeaseStatus* odblokowaniu właściwości można znaleźć odłączyć dysków niezarządzane. *LeaseStatus* ustawiono właściwości do zablokowanego Jeśli niezarządzane dysk jest podłączony do maszyny wirtualnej.

Zdecydowanie zalecamy do pierwszego uruchomienia skryptu przez ustawienie *deleteUnattachedVHDs* zmiennej na 0, aby wyświetlić wszystkie odłączyć dysków. Po zapoznaniu się odłączyć dysków, uruchom skrypt przez ustawienie *deleteUnattachedVHDs* 1, aby usunąć wszystkie odłączyć dysków.


```azurepowershell-interactive
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=1

$storageAccounts = Get-AzureRmStorageAccount

foreach($storageAccount in $storageAccounts){

    $storageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.StorageAccountName)[0].Value

    $context = New-AzureStorageContext -StorageAccountName $storageAccount.StorageAccountName -StorageAccountKey $storageKey

    $containers = Get-AzureStorageContainer -Context $context

    foreach($container in $containers){

        $blobs = Get-AzureStorageBlob -Container $container.Name -Context $context

        #Fetch all the Page blobs with extension .vhd as only Page blobs can be attached as disk to Azure VMs
        $blobs | Where-Object {$_.BlobType -eq 'PageBlob' -and $_.Name.EndsWith('.vhd')} | ForEach-Object { 
        
            #If a Page blob is not attached as disk then LeaseStatus will be unlocked
            if($_.ICloudBlob.Properties.LeaseStatus -eq 'Unlocked'){
              
                  if($deleteUnattachedVHDs -eq 1){

                        Write-Host "Deleting unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"

                        $_ | Remove-AzureStorageBlob -Force

                        Write-Host "Deleted unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                  }
                  else{

                        $_.ICloudBlob.Uri.AbsoluteUri

                  }

            }
        
        }

    }

}

```

## <a name="next-steps"></a>Kolejne kroki

[Usunięcie konta magazynu](../../storage/common/storage-create-storage-account.md)




