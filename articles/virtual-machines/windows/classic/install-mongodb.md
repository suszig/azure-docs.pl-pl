---
title: Instalowanie bazy danych MongoDB Windows maszyny Wirtualnej na platformie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak zainstalować bazy danych MongoDB na maszynie Wirtualnej platformy Azure utworzonych za pomocą klasycznego modelu wdrażania systemem Windows Server."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4095df41-bb69-4bbe-9c1c-70923b0d84ba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
ms.openlocfilehash: 6b5af18d02fd508a21cdc21b38b1c16e79f07ecb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a>Instalowanie bazy danych MongoDB Windows maszyny Wirtualnej na platformie Azure
> [!IMPORTANT]
> Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../../../resource-manager-deployment-model.md).  W tym artykule omówiono przy użyciu klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager. Aby zainstalować i skonfigurować bazy danych MongoDB przy użyciu modelu wdrażania usługi Resource Manager, zobacz [w tym artykule](../install-mongodb.md).

[Bazy danych MongoDB] [ MongoDB] jest popularnych open source, wysokiej wydajności bazę danych NoSQL. W tym artykule prowadzi użytkownika przez proces tworzenia maszyny wirtualnej (VM) systemu Windows Server przy użyciu [portalu Azure][AzurePortal]. Następnie utwórz i Dołącz dysku danych do maszyny Wirtualnej przed zainstalowaniem i skonfigurowaniem bazy danych MongoDB. Jeśli masz istniejącą maszynę Wirtualną na platformie Azure, którego chcesz użyć, można przejść bezpośrednio do [Instalowanie i Konfigurowanie bazy danych MongoDB](#install-and-run-mongodb-on-the-virtual-machine).

## <a name="create-a-virtual-machine-running-windows-server"></a>Tworzenie maszyny wirtualnej z systemem Windows Server
Wykonaj te instrukcje, aby utworzyć maszynę wirtualną.

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> Dodawanie punktu końcowego dla bazy danych MongoDB podczas tworzenia maszyny wirtualnej i skonfigurować w następujący sposób: Nazwa go jako **Mongo**, użyj **TCP** jako protokół i Ustaw porty publiczny i prywatny **27017**.
>
>

## <a name="attach-a-data-disk"></a>Dołączanie dysku danych
Zapewnianie magazynowania dla maszyny wirtualnej, dołączenie dysku danych, a następnie zainicjować, tak aby można go używać systemu Windows. Jeśli masz już dysk danych, można dołączyć istniejącego dysku, lub możesz dołączyć pusty dysk.

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

Aby uzyskać instrukcje dotyczące inicjowania dysku, zobacz "jak: Zainicjuj nowy dysk danych w systemie Windows Server" w [jak można dołączyć dysku danych do maszyny wirtualnej systemu Windows](attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Zainstaluj i uruchom bazy danych MongoDB na maszynie wirtualnej
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Podsumowanie
W tym samouczku przedstawiono sposób tworzenia maszyny wirtualnej z systemem Windows Server, zdalnie nawiązać z nim i dołączenie dysku danych.  Przedstawiono również sposób instalowania i konfigurowania bazy danych MongoDB na maszynie wirtualnej z systemem Windows. Na maszynie wirtualnej z systemem Windows można uzyskać dostęp bazy danych MongoDB, wykonując następujące tematy Zaawansowane w [dokumentacją usługi MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

