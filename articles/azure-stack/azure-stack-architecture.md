---
title: Architektura Microsoft Azure stosu Development Kit | Dokumentacja firmy Microsoft
description: "Wyświetlić architektura Microsoft Azure stosu Development Kit."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: b754ff5b5a82ac284eb59ff9b9d30a581f3d3af5
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Architektura Microsoft Azure stosu Development Kit

*Dotyczy: Azure stosu Development Kit*

Zestaw deweloperski stosu Azure to wdrożenie jednowęzłowej stosu Azure. Wszystkie składniki są instalowane na maszynach wirtualnych uruchomionych na maszynie jednego hosta. 

## <a name="logical-architecture-diagram"></a>Diagram architektury logiczne
Na poniższym diagramie przedstawiono architektura logiczna zestaw deweloperski stosu Azure i jego składniki.

![](media/azure-stack-architecture/image1.png)

## <a name="virtual-machine-roles"></a>Role maszyny wirtualnej
Zestaw deweloperski stosu Azure oferuje usługi przy użyciu następujących maszyn wirtualnych na hoście:

| Name (Nazwa) | Opis |
| ----- | ----- |
| **AzS-ACS01** | Usługi magazynu Azure stosu.|
| **AzS-ADFS01** | Usługi federacyjne Active Directory (AD FS).  |
| **AzS-BGPNAT01** | Krawędzi router i zapewnia możliwości translatora adresów Sieciowych i sieci VPN Azure stosu. |
| **AzS-CA01** | Urząd usług Azure stosu roli usług certyfikatów.|
| **AzS-DC01** | Active Directory, DNS i DHCP usługi Microsoft Azure stosu.|
| **AzS-ERCS01** | Konsola awaryjnego odzyskiwania maszyny Wirtualnej. |
| **AzS-GWY01** | Brama brzegowa usług takich jak połączenia sieci VPN lokacja lokacja dla sieci dzierżawcy.|
| **AzS-NC01** | Kontroler sieci, który zarządza usługi sieciowe Azure stosu.  |
| **AzS-SLB01** | Równoważenia obciążenia multiplekser usług Azure stosu dla zarówno dzierżawców, jak i usługi infrastruktury platformy Azure stosu.  |
| **AzS-SQL01** | Dane wewnętrzne przechowywania dla ról infrastruktury Azure stosu.  |
| **AzS-WAS01** | Portal administracyjny stosu platformy Azure i usługi Azure Resource Manager.|
| **AzS-WASP01**| Portalu użytkownika (dzierżawcy) stosu Azure i usługi Azure Resource Manager.|
| **AzS-XRP01** | Kontroler zarządzania infrastruktury dla programu Microsoft Azure stosu, w tym dostawców zasobów obliczeniowych, sieci i magazynu.|


## <a name="next-steps"></a>Kolejne kroki
[Wdrażanie usługi Azure stosu](azure-stack-deploy.md)

[Scenariusze pierwszej próby](azure-stack-first-scenarios.md)

