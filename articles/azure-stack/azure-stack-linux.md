---
title: "Dodawanie obrazów systemu Linux Azure stosu"
description: "Dowiedz się, jak dodać obrazy Linux Azure stosu."
services: azure-stack
documentationcenter: 
author: anjayajodha
manager: byronr
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/25/2017
ms.author: anajod
ms.openlocfilehash: a8763c01cba4e5a9eaa3b7842b627d6eb9661a95
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="add-linux-images-to-azure-stack"></a>Dodawanie obrazów systemu Linux Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit* 

Można wdrożyć maszyn wirtualnych systemu Linux na stosie Azure przez dodanie do obrazu opartych na systemie Linux w portalu Azure Marketplace stosu. Najprostszym sposobem, aby dodać obraz systemu Linux Azure stos jest za pośrednictwem zarządzania Marketplace.

## <a name="marketplace-management"></a>Zarządzanie Marketplace

Aby pobrać Linux obrazów z portalu Azure Marketplace, procedury przedstawione w artykule. Wybierz obrazy systemu Linux, które chcesz zapewnić użytkownikom na stosie Azure.

[Pobieranie elementów marketplace z platformy Azure do stosu Azure](azure-stack-download-azure-marketplace-item.md).

## <a name="download-an-image"></a>Pobranie obrazu

Może pobierać i wyodrębniać systemu Linux platformy Azure zgodnego stosu obrazów za pomocą następujących łączy:


   * [Bitnami](https://bitnami.com/azure-stack)
   * [CentOS](http://olstacks.cloudapp.net/latest/)
   * [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
   * [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
   * [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)

1. Wyodrębnij obrazu wirtualnego dysku twardego w razie potrzeby i [dodać obraz do witryny Marketplace](azure-stack-add-vm-image.md). Upewnij się, że `OSType` ustawiono parametr `Linux`.
2. Po dodaniu go do portalu Marketplace, zostanie utworzony element Marketplace, a użytkownicy będą mogli wdrażać maszyny wirtualnej systemu Linux.

## <a name="prepare-your-own-image"></a>Przygotowanie własnego obrazu

Można przygotować własny obraz systemu Linux przy użyciu jednej z poniższych instrukcji:
   
   * [Na podstawie centOS dystrybucji](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian systemu Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Oracle Linux](../virtual-machines/linux/oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Red Hat Enterprise Linux](../virtual-machines/linux/redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

1. Pobierz i zainstaluj [agenta systemu Linux Azure](https://github.com/Azure/WALinuxAgent/).
   
    Agenta systemu Linux platformy Azure w wersji 2.1.3 lub nowszy jest wymagany do obsługi administracyjnej maszyny Wirtualnej systemu Linux na stosie Azure. Wiele dystrybucji wymienione wcześniej już zawiera ta wersja agenta lub wyższej jako pakiet w ich repozytoriów (zwykle nazywane `WALinuxAgent` lub `walinuxagent`). Jednak jeśli wersja pakiet agenta platformy Azure jest mniejsza niż 2.1.3 (na przykład 2.0.18 lub małą), należy ręcznie zainstalować agenta. Zainstalowana wersja można ustalić na podstawie nazwy pakietu lub uruchamiając `/usr/sbin/waagent -version` na maszynie Wirtualnej.
   
    Postępuj zgodnie z instrukcjami poniżej, aby ręcznie — zainstaluj agenta systemu Linux platformy Azure
   
   a. Najpierw pobierz najnowszą wersję agenta systemu Linux platformy Azure z [GitHub](https://github.com/Azure/WALinuxAgent/releases), przykład:
     
            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.16.tar.gz
   b. Rozpakuj agenta programu Azure:
     
            # tar -vzxf v2.2.16.tar.gz
   c. Zainstaluj narzędzia setuptools języka python
     
        **Debian / Ubuntu**
     
            # sudo apt-get update
            # sudo apt-get install python-setuptools
     
        **Ubuntu 16.04+**
     
            # sudo apt-get install python3-setuptools
     
        **RHEL / CentOS / Oracle Linux**
     
            # sudo yum install python-setuptools
   d. Zainstaluj agenta platformy Azure:
     
            # cd WALinuxAgent-2.2.16
            # sudo python3 setup.py install --register-service
     
     Systemy z Python 2.x i Python 3.x zainstalowany side-by-side może być konieczne uruchomienie następującego polecenia:
     
         sudo python3 setup.py install --register-service
     Aby uzyskać więcej informacji, zobacz agenta systemu Linux Azure [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
2. [Dodawanie obrazu do witryny Marketplace](azure-stack-add-vm-image.md). Upewnij się, że `OSType` ustawiono parametr `Linux`.
3. Po dodaniu go do portalu Marketplace, zostanie utworzony element Marketplace, a użytkownicy będą mogli wdrażać maszyny wirtualnej systemu Linux.

## <a name="next-steps"></a>Następne kroki
[Przegląd oferty usług Azure stosu](azure-stack-offer-services-overview.md)

