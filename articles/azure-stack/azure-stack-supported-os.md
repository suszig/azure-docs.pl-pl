---
title: "Obsługiwane systemy operacyjne gościa dla stosu Azure | Dokumentacja firmy Microsoft"
description: "Te systemy operacyjne gościa może służyć na stosie Azure."
services: azure-stack
documentationcenter: 
author: Brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: Brenduns
ms.reviewer: JeffGoldner
ms.openlocfilehash: c9f5bee38772623fb79fa081be8eaece981cc8ab
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
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
| Windows 10 *(patrz Uwaga 1)* | 64-bitowy, Pro i Enterprise | Microsoft | Windows | Nie |

***Uwaga 1:****do wdrażania systemów operacyjnych klienta systemu Windows 10 na stosie Azure, musi mieć [Windows licencjonowania na użytkownika](https://www.microsoft.com/Licensing/product-licensing/windows10.aspx) lub zakupu za pośrednictwem dostawcy usług hostingowych wielodostępnej kwalifikowana ([QMTH](https://www.microsoft.com/CloudandHosting/licensing_sca.aspx)).* 


## <a name="linux"></a>Linux

Dystrybucje systemu Linux wymienione w tym miejscu zawierają niezbędne systemu Windows Azure Linux Agent (WALA).

> [!NOTE]   
> Obrazów zbudowanych z WALA w wersji wcześniejszej niż 2.2.3 *nie* obsługiwane i są w stanie wdrożenia. Niektóre wersje agenta WALA wiadomo, że nieprawidłowe działanie na maszynach wirtualnych stosu Azure, w tym wersje 2.2.12 i 2.2.13.


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
