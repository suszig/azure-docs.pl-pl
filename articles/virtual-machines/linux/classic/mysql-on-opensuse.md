---
title: Zainstaluj MySQL na maszynie Wirtualnej OpenSUSE | Dokumentacja firmy Microsoft
description: "Dowiedz się zainstalować MySQL na maszynie OpenSUSE VMirtual systemu Linux na platformie Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/19/2016
ms.author: cynthn
ms.openlocfilehash: 01b798a25575b66f89057315ce80d6cc0cde53b5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Instalowanie bazy danych MySQL na maszynie wirtualnej z dystrybucją systemu OpenSUSE Linux na platformie Azure
[MySQL] [ MySQL] popularnych, open source baza danych SQL. W tym samouczku przedstawiono sposób tworzenia maszyny wirtualnej z systemem OpenSUSE Linux, a następnie zainstalować MySQL.

> [!IMPORTANT] 
> Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../../../resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager.

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Utwórz maszynę wirtualną z systemem OpenSUSE Linux
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Zainstaluj i uruchom MySQL na maszynie wirtualnej
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Następne kroki
Aby uzyskać szczegółowe informacje o MySQL, zobacz [dokumentacji MySQL][MySQLDocs].

[MySQLDocs]:http://dev.mysql.com/doc/index-topic.html
[MySQL]:http://www.mysql.com

