---
title: "Przykłady programu Azure PowerShell — sieci szkieletowej usług | Dokumentacja firmy Microsoft"
description: "Przykłady programu Azure PowerShell — sieci szkieletowej usług"
services: service-fabric
documentationcenter: service-fabric
author: rwike77
manager: timlt
editor: 
tags: 
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: service-fabric
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: service-fabric
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: ae132dbb650e08c3a25a9366563e70c6d56e089d
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="azure-powershell-samples"></a>Przykłady programu Azure PowerShell

Poniższa tabela zawiera linki do przykłady skryptów środowiska PowerShell utworzyć i zarządzać związanymi z klastrów, aplikacji i usług sieci szkieletowej usług.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **Tworzenie klastra** ||
| [Tworzenie klastra (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Tworzy klaster sieci szkieletowej usług Azure. |
| **Zarządzanie, klaster i węzły** ||
| [Dodaj certyfikat aplikacji](./scripts/service-fabric-powershell-add-application-certificate.md)| Dodaje certyfikat X.509 aplikacji na wszystkich węzłach w klastrze. |
|[Zmień zakres portów protokołu RDP w węźle klastra maszyny wirtualne](./scripts/service-fabric-powershell-change-rdp-port-range.md)|Zmienia zakres portów protokołu RDP w węźle klastra maszyny wirtualne w klastrze wdrożone.|
| [Zaktualizuj administrator użytkownika i hasło dla węzła klastra maszyny wirtualne](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | Nazwa użytkownika i hasło dla węzła klastra maszyny wirtualne aktualizacji. |
| **Zarządzanie aplikacjami** ||
| [Wdrażanie aplikacji](./scripts/service-fabric-powershell-deploy-application.md)| Wdrażanie aplikacji do klastra.|
| [Uaktualnianie aplikacji](./scripts/service-fabric-powershell-upgrade-application.md)| Uaktualnianie aplikacji |
| [Usuwanie aplikacji](./scripts/service-fabric-powershell-remove-application.md)| Usuń aplikację z klastra.|
| [Otwieranie portu w module równoważenia obciążenia](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | Otwórz port aplikacji w usłudze równoważenia obciążenia Azure. |
