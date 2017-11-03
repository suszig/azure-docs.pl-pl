---
title: "Połączenie maszyn wirtualnych systemu Windows w usłudze w chmurze | Dokumentacja firmy Microsoft"
description: "Połączenie maszyn wirtualnych systemu Windows utworzone z klasycznym modelu wdrażania usługi w chmurze Azure lub sieci wirtualnej."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c1cbc802-4352-4d2e-9e49-4ccbd955324b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: 0c7a21461e5bb111c4359df8e949d48382b591c1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Połączenie maszyn wirtualnych systemu Windows utworzone z klasycznym modelu wdrażania z wirtualnych sieci lub usługę w chmurze
> [!IMPORTANT]
> Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../../../resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager.

Maszyny wirtualne systemu Windows utworzone z klasycznym modelu wdrażania są zawsze umieszczane w usłudze w chmurze. Usługi w chmurze pełni funkcję kontenera i zapewnia unikatową publicznej nazwy DNS, publiczny adres IP i zestaw punktów końcowych na dostęp do maszyny wirtualnej za pośrednictwem Internetu. Usługi w chmurze może być w sieci wirtualnej, ale nie jest wymagane. Możesz również [połączenie maszyn wirtualnych systemu Linux za pomocą wirtualnej sieci lub w chmurze usługi](../../linux/classic/connect-vms.md).

Jeśli usługa w chmurze nie jest w sieci wirtualnej, jest nazywany *autonomiczny* usługi w chmurze. Maszyny wirtualne w usłudze w chmurze autonomiczny komunikować się z innych maszyn wirtualnych przy użyciu innych maszyn wirtualnych publicznej nazwy DNS, a ruch przechodzi przez Internet. Jeśli usługa w chmurze jest w sieci wirtualnej maszyn wirtualnych w tej usłudze w chmurze może komunikować się z innych maszyn wirtualnych w sieci wirtualnej bez wysyłania cały ruch w Internecie.

Jeśli w tej samej usłudze w chmurze autonomicznych maszyn wirtualnych, nadal używać zestawy dostępności i równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [maszyn wirtualnych równoważenia obciążenia](../../virtual-machines-windows-load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) i [Zarządzaj dostępnością maszyn wirtualnych](../../virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Nie można jednak zorganizować maszyn wirtualnych w podsieciach lub połączyć autonomiczna usługa chmury do sieci lokalnej. Oto przykład:

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Następne kroki
Po utworzeniu maszyny wirtualnej jest dobrym pomysłem jest [Dodaj dysk danych](attach-disk.md) więc usług i obciążeń lokalizację do przechowywania danych.
