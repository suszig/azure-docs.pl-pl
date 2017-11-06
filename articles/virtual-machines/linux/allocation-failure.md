---
title: "Rozwiązywanie problemów z błędami alokacji maszyny Wirtualnej systemu Linux | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów z przydziałem podczas tworzenia, uruchom ponownie lub zmień rozmiar maszyny Wirtualnej systemu Linux na platformie Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resourece-manager,azure-service-management
ms.assetid: 1ef41144-6dd6-4a56-b180-9d8b3d05eae7
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2016
ms.author: cjiang
ms.openlocfilehash: 8cf421afe9b41af050152dc965d42e7809b5f2d9
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-linux-vms-in-azure"></a>Rozwiązywanie problemów z przydziałem podczas tworzenia, uruchom ponownie lub zmień rozmiar maszyn wirtualnych systemu Linux na platformie Azure
Podczas tworzenia maszyny Wirtualnej, ponownego uruchomienia zatrzymanej maszyny wirtualnej (cofnięciu przydziału) lub zmień rozmiar maszyny Wirtualnej Microsoft Azure przydziela zasoby obliczeniowe do subskrypcji. Czasami może pojawić błędy podczas wykonywania tych operacji — nawet zanim przejdziesz limity subskrypcji platformy Azure. W tym artykule opisano przyczyny niektórych typowych błędów alokacji i sugeruje możliwe korygowania. Informacje również mogą być przydatne podczas planowania wdrożenia usługi. Możesz również [Rozwiązywanie problemów z przydziałem podczas tworzenia, uruchom ponownie lub zmień rozmiar maszyn wirtualnych systemu Windows na platformie Azure](../windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

