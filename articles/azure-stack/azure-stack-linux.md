---
title: "Dodawanie obrazów systemu Linux Azure stosu"
description: "Dowiedz się, jak dodać obrazy Linux Azure stosu."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 6e6f9ca3b314ee2f58d8007e7ddc93ddd213e361
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="add-linux-images-to-azure-stack"></a>Dodawanie obrazów systemu Linux Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Można wdrożyć maszyn wirtualnych systemu Linux (VM) na stosie Azure przez dodanie do obrazu opartych na systemie Linux w portalu Azure Marketplace stosu. Najprostszym sposobem, aby dodać obraz systemu Linux Azure stos jest za pośrednictwem zarządzania Marketplace. Te obrazy zostały przygotowane i sprawdzane pod kątem zgodności z stosu Azure.

## <a name="marketplace-management"></a>Zarządzanie Marketplace

Aby pobrać Linux obrazów z portalu Azure Marketplace, procedury przedstawione w artykule. Wybierz obrazy systemu Linux, które chcesz zapewnić użytkownikom na stosie Azure.

[Pobieranie elementów marketplace z platformy Azure do stosu Azure](azure-stack-download-azure-marketplace-item.md).

## <a name="prepare-your-own-image"></a>Przygotowanie własnego obrazu

Można przygotować własny obraz systemu Linux przy użyciu jednej z poniższych instrukcji:

   * [Na podstawie centOS dystrybucji](../virtual-machines/linux/create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Debian systemu Linux](../virtual-machines/linux/debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [SLES & openSUSE](../virtual-machines/linux/suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
   * [Ubuntu](../virtual-machines/linux/create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

1. Pobierz i zainstaluj [agenta systemu Linux Azure](https://github.com/Azure/WALinuxAgent/).

    Agenta systemu Linux platformy Azure w wersji 2.2.2 lub nowszy jest wymagany do udostępniania maszyny Wirtualnej systemu Linux na stosie Azure i niektóre wersje nie działają (2.2.12 i 2.2.13 przykłady). Wiele dystrybucji wymienione wcześniej już zawiera wersję agenta (zwykle nazywane `WALinuxAgent` lub `walinuxagent`). W przypadku utworzenia niestandardowego obrazu, należy zainstalować agenta ręcznie. Zainstalowana wersja można ustalić na podstawie nazwy pakietu lub uruchamiając `/usr/sbin/waagent -version` na maszynie Wirtualnej.

    Postępuj zgodnie z instrukcjami poniżej, aby ręcznie — zainstaluj agenta systemu Linux platformy Azure

   a. Najpierw pobierz najnowszą wersję agenta systemu Linux platformy Azure z [GitHub](https://github.com/Azure/WALinuxAgent/releases), przykład:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.21.tar.gz
   b. Rozpakuj agenta programu Azure:

            # tar -vzxf v2.2.21.tar.gz
   c. Zainstaluj narzędzia setuptools języka python

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools
   d. Zainstaluj agenta platformy Azure:

            # cd WALinuxAgent-2.2.21
            # sudo python3 setup.py install --register-service

     Systemy z Python 2.x i Python 3.x zainstalowany side-by-side może być konieczne uruchomienie następującego polecenia:

         sudo python3 setup.py install --register-service
     Aby uzyskać więcej informacji, zobacz agenta systemu Linux Azure [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
2. [Dodawanie obrazu do witryny Marketplace](azure-stack-add-vm-image.md). Upewnij się, że `OSType` ustawiono parametr `Linux`.
3. Po dodaniu go do portalu Marketplace, zostanie utworzony element Marketplace, a użytkownicy będą mogli wdrażać maszyny wirtualnej systemu Linux.

## <a name="next-steps"></a>Kolejne kroki
[Przegląd oferty usług Azure stosu](azure-stack-offer-services-overview.md)
