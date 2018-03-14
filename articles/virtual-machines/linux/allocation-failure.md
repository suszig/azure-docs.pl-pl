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
ms.topic: article
ms.date: 11/03/2016
ms.author: cjiang
ms.openlocfilehash: 626968c463d76abe6becaa85813336567f108d0d
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/13/2018
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-linux-vms-in-azure"></a>Rozwiązywanie problemów z przydziałem podczas tworzenia, uruchom ponownie lub zmień rozmiar maszyn wirtualnych systemu Linux na platformie Azure
Podczas tworzenia maszyny Wirtualnej, ponownego uruchomienia zatrzymanej maszyny wirtualnej (cofnięciu przydziału) lub zmień rozmiar maszyny Wirtualnej Microsoft Azure przydziela zasoby obliczeniowe do subskrypcji. Czasami może pojawić błędy podczas wykonywania tych operacji — nawet zanim przejdziesz limity subskrypcji platformy Azure. W tym artykule opisano przyczyny niektórych typowych błędów alokacji i sugeruje możliwe korygowania. Informacje również mogą być przydatne podczas planowania wdrożenia usługi. Możesz również [Rozwiązywanie problemów z przydziałem podczas tworzenia, uruchom ponownie lub zmień rozmiar maszyn wirtualnych systemu Windows na platformie Azure](../windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

