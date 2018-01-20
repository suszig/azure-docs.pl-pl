---
title: "Znajdowanie i usuwanie odłączyć dysku systemu Azure zarządzane i niezarządzane | Dokumentacja firmy Microsoft"
description: "Jak znaleźć i usunąć odłączyć dysku systemu Azure zarządzane i niezarządzane (pliki VHD/stronicowych obiektów blob), za pomocą wiersza polecenia platformy Azure"
services: virtual-machines-linux
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: 9ada768cd4128b9dd6949b5a96c557496c6bb11c
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Znajdowanie i usuwanie odłączyć dysku systemu Azure zarządzane i niezarządzane
Po usunięciu maszyny wirtualnej na platformie Azure dysków dołączonych do niego nie są usuwane domyślnie. Zapobiega utracie danych z powodu usunięty przez pomyłkę maszyn wirtualnych, ale nadal niepotrzebnie opłacać odłączyć dysków. Użyj w tym artykule, aby znaleźć i usunąć wszystkie dyski odłączyć i zapisać koszt. 


## <a name="find-and-delete-unattached-managed-disks"></a>Znajdowanie i usuwanie odłączyć dysków zarządzanych 

Poniższy skrypt pokazuje, jak znaleźć odłączyć dysków zarządzanych za pomocą właściwości zarządzane.  W pętli wszystkich zarządzanych dysków w ramach subskrypcji i umożliwia sprawdzenie, czy *zarządzane* właściwość ma wartość null, aby znaleźć odłączyć dysków zarządzanych. *Zarządzane* właściwość przechowuje identyfikator zasobu maszyny wirtualnej, do której jest dołączona dysku zarządzanego. 

Zdecydowanie zalecamy do pierwszego uruchomienia skryptu przez ustawienie *deleteUnattachedDisks* zmiennej na 0, aby wyświetlić wszystkie odłączyć dysków. Po zapoznaniu się odłączyć dysków, uruchom skrypt przez ustawienie *deleteUnattachedDisks* 1, aby usunąć wszystkie odłączyć dysków.

 ```azurecli

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
deleteUnattachedDisks=0

unattachedDiskIds=$(az disk list --query '[?managedBy==`null`].[id]' -o tsv)
for id in ${unattachedDiskIds[@]}
do
    if (( $deleteUnattachedDisks == 1 ))
    then

        echo "Deleting unattached Managed Disk with Id: "$id
        az disk delete --ids $id --yes
        echo "Deleted unattached Managed Disk with Id: "$id

    else
        echo $id
    fi
done

```
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Znajdowanie i usuwanie odłączyć dysków niezarządzane 

Niezarządzane dyski są przechowywane jako pliki VHD [stronicowe](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) w [konta usługi Azure Storage](../../storage/common/storage-create-storage-account.md). Poniższy skrypt pokazuje, jak znaleźć za pomocą właściwości LeaseStatus odłączyć dysków niezarządzane (stronicowych obiektów blob). Wszystkie dyski niezarządzane we wszystkich magazyn kont w ramach subskrypcji i sprawdza, czy w pętli *LeaseStatus* odblokowaniu właściwości można znaleźć odłączyć dysków niezarządzane. *LeaseStatus* ustawiono właściwości do zablokowanego Jeśli niezarządzane dysk jest podłączony do maszyny wirtualnej. 

Zdecydowanie zalecamy do pierwszego uruchomienia skryptu przez ustawienie *deleteUnattachedVHDs* zmiennej na 0, aby wyświetlić wszystkie odłączyć dysków. Po zapoznaniu się odłączyć dysków, uruchom skrypt przez ustawienie *deleteUnattachedVHDs* 1, aby usunąć wszystkie odłączyć dysków.


 ```azurecli
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the details of the unattached VHDs
deleteUnattachedVHDs=1

storageAccountIds=$(az storage account list --query [].[id] -o tsv)

for id in ${storageAccountIds[@]}
do
    connectionString=$(az storage account show-connection-string --ids $id --query connectionString -o tsv)
    containers=$(az storage container list --connection-string $connectionString --query [].[name] -o tsv)

    for container in ${containers[@]}
    do 
        
        blobs=$(az storage blob list -c $container --connection-string $connectionString --query "[?properties.blobType=='PageBlob' && ends_with(name,'.vhd')].[name]" -o tsv)
        
        for blob in ${blobs[@]}
        do
            leaseStatus=$(az storage blob show -n $blob -c $container --connection-string $connectionString --query "properties.lease.status" -o tsv)
            
            if [ "$leaseStatus" == "unlocked" ]
            then 

                if (( $deleteUnattachedVHDs == 1 ))
                then 

                    echo "Deleting VHD: "$blob" in container: "$container" in storage account: "$id

                    az storage blob delete --delete-snapshots include  -n $blob -c $container --connection-string $connectionString

                    echo "Deleted VHD: "$blob" in container: "$container" in storage account: "$id
                else
                    echo "StorageAccountId: "$id" container: "$container" VHD: "$blob
                fi

            fi
        done
    done
done 

```

## <a name="next-steps"></a>Kolejne kroki

[Usunięcie konta magazynu](../../storage/common/storage-create-storage-account.md)



