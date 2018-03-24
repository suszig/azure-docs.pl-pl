---
title: Przykładów dla sieci wirtualnej platformy Azure PowerShell | Dokumentacja firmy Microsoft
description: Przykładów dla platformy Azure środowiska PowerShell dla sieci wirtualnej.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: c9cb8da2a6a7512daa6721af90d5b21c6ba5e8e5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-powershell-samples-for-virtual-network"></a>Przykładów dla platformy Azure środowiska PowerShell dla sieci wirtualnej

Poniższa tabela zawiera linki do skryptów programu Azure Powershell:

| | |
|----|----|
| [Tworzenie sieci wirtualnej dla aplikacji wielowarstwowych](./scripts/virtual-network-powershell-sample-multi-tier-application.md) | Tworzy sieć wirtualną z podsieci frontonu i zaplecza. Ruch do podsieci frontonu jest ograniczone do protokołu HTTP, gdy ruch do podsieci wewnętrznej jest ograniczony do bazy danych SQL, port 1433. |
| [Dwie wirtualne sieci równorzędne](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md) | Tworzy i łączy dwie sieci wirtualne w tym samym regionie. |
| [Kierować ruchem przez urządzenie wirtualne sieci](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md) | Tworzy sieć wirtualną z podsieci frontonu i zaplecza i maszynę Wirtualną, która może kierować ruchem między dwiema podsieciami. |
| [Filtrowanie ruchu przychodzącego i wychodzącego ruchu sieciowego maszyny Wirtualnej](./scripts/virtual-network-powershell-sample-filter-network-traffic.md) | Tworzy sieć wirtualną z podsieci frontonu i zaplecza. Ruch sieciowy przychodzący do podsieci frontonu jest ograniczona do HTTP i HTTPS. Ruch wychodzący do Internetu z podsieci wewnętrznej jest niedozwolone. |
