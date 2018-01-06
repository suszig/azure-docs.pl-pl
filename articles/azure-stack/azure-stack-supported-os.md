---
title: "Obsługiwane systemy operacyjne gościa dla stosu Azure | Dokumentacja firmy Microsoft"
description: "Te systemy operacyjne gościa może służyć na stosie Azure."
services: azure-stack
documentationcenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: JeffGoldner
ms.openlocfilehash: 0a31da6cbc2c245b959825a4e715d0dc7511ba99
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Systemy operacyjne gościa obsługiwane na stosie Azure

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

## <a name="windows"></a>Windows
Stos Azure obsługuje następujące systemy operacyjne Windows gościa. Obrazów w witrynie Marketplace są dostępne do pobrania stos Azure. Obrazy klienta systemu Windows nie są dostępne w witrynie Marketplace.

Podczas wdrażania stosu Azure zapewnia, że odpowiedniej wersji agenta gościa jest dodane do obrazu.

| System operacyjny | Opis | Wydawca | Typ systemu operacyjnego | Portal Marketplace |
| --- | --- | --- | --- | --- | --- |
| Windows Server 2008 R2 SP1 | 64-bitowa | Microsoft | Windows | Centrum danych |
| Windows Server 2012 | 64-bitowa | Microsoft | Windows | Centrum danych |
| Windows Server 2012 R2 | 64-bitowa | Microsoft | Windows | Centrum danych |
| Windows Server 2016 | 64-bitowa | Microsoft | Windows | Centrum danych, podstawowe centrum danych centrum danych z kontenerami |
| Windows 7 | 64-bitowy, Pro i Enterprise | Microsoft | Windows | Nie |
| Windows 8.1 | 64-bitowy, Pro i Enterprise | Microsoft | Windows | Nie |
| Windows 10 | 64-bitowy, Pro i Enterprise | Microsoft | Windows | Nie |

## <a name="linux"></a>Linux

Dystrybucje systemu Linux wymienione w tym miejscu zawierają niezbędne systemu Windows Azure Linux Agent (WALA). 

> [!NOTE]
> Obrazów zbudowanych z WALA w wersji wcześniejszej niż 2.2.3 *nie* obsługiwane i są w stanie wdrożenia.

| Dystrybucja | Opis | Wydawca | Portal Marketplace |
| --- | --- | --- | --- | --- | --- |
| Kontener systemu Linux |  64-bitowa | CoreOS | Stable |
| Na podstawie centOS 6,9 | 64-bitowa | Nieautoryzowany Wave | Yes |
| Oparty na systemie CentOS 7.3 | 64-bitowa | Nieautoryzowany Wave | Yes |
| 7.4 na podstawie centOS | 64-bitowa | Nieautoryzowany Wave | Yes |
| Debian 8 "Joasia." | 64-bitowa | credativ |  Yes |
| Debian 9 "Stretch" | 64-bitowa | credativ | Yes |
| Oracle Linux | 64-bitowa | Oracle | Nie |
| Red Hat Enterprise Linux 7.x | 64-bitowa | Red Hat | Nie |
| SLES 11SP4 | 64-bitowa | SUSE | Yes |
| SLES 12SP3 | 64-bitowa | SUSE | Yes |
| Ubuntu 14.04-LTS | 64-bitowa | Canonical | Yes |
| Ubuntu 16.04-LTS | 64-bitowa | Canonical | Yes |

Inne dystrybucje systemu Linux mogą być obsługiwane w przyszłości.




