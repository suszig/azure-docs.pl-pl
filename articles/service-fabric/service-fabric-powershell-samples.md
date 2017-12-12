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
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: service-fabric
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 17b10011af915a06e7c65f8d321a1e452956c28a
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="azure-powershell-samples"></a>Przykłady programu Azure PowerShell

Poniższa tabela zawiera linki do przykłady skryptów środowiska PowerShell utworzyć i zarządzać związanymi z klastrów, aplikacji i usług sieci szkieletowej usług.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **Tworzenie klastra** ||
| [Tworzenie klastra (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Tworzy klaster sieci szkieletowej usług Azure. |
| **Zarządzanie klastrem, węzły i infrastruktury** ||
| [Dodaj certyfikat aplikacji](./scripts/service-fabric-powershell-add-application-certificate.md)| Dodaje certyfikat X.509 aplikacji na wszystkich węzłach w klastrze. |
| [Zaktualizuj zakres portów protokołu RDP w klastrze maszyny wirtualne](./scripts/service-fabric-powershell-change-rdp-port-range.md)|Zmienia zakres portów protokołu RDP w węźle klastra maszyny wirtualne w klastrze wdrożone.|
| [Zaktualizuj administrator użytkownika i hasło dla węzła klastra maszyny wirtualne](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | Nazwa użytkownika i hasło dla węzła klastra maszyny wirtualne aktualizacji. |
| [Otwieranie portu w module równoważenia obciążenia](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | Otwórz port aplikacji w usłudze równoważenia obciążenia Azure zezwalająca na ruch przychodzący na określonym porcie. |
| [Tworzenie reguły grupy zabezpieczeń sieci dla ruchu przychodzącego](./scripts/service-fabric-powershell-add-nsg-rule.md) | Tworzenie reguły grupy zabezpieczeń sieci dla ruchu przychodzącego zezwalająca na ruch przychodzący do klastra na określonym porcie. |
| **Zarządzanie aplikacjami** ||
| [Wdrażanie aplikacji](./scripts/service-fabric-powershell-deploy-application.md)| Wdrażanie aplikacji do klastra.|
| [Uaktualnianie aplikacji](./scripts/service-fabric-powershell-upgrade-application.md)| Uaktualnianie aplikacji.|
| [Usuwanie aplikacji](./scripts/service-fabric-powershell-remove-application.md)| Usuń aplikację z klastra.|
