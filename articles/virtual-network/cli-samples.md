---
title: Przykładów dla interfejsu wiersza polecenia platformy Azure dla sieci wirtualnej | Dokumentacja firmy Microsoft
description: Przykładów dla platformy Azure CLI dla sieci wirtualnej.
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
ms.openlocfilehash: 9459bad1060daa0c9f6c34272cecfbc67463be66
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="azure-cli-samples-for-virtual-network"></a>Przykładów dla interfejsu wiersza polecenia platformy Azure dla sieci wirtualnej

Poniższa tabela zawiera linki do bash skryptów za pomocą polecenia wiersza polecenia platformy Azure:

| | |
|----|----|
| [Tworzenie sieci wirtualnej dla aplikacji wielowarstwowych](./scripts/virtual-network-cli-sample-multi-tier-application.md) | Tworzy sieć wirtualną z podsieci frontonu i zaplecza. Ruch do podsieci frontonu jest ograniczona do HTTP i SSH, gdy ruch do podsieci wewnętrznej jest ograniczony do MySQL, port 3306. |
| [Dwie wirtualne sieci równorzędne](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | Tworzy i łączy dwie sieci wirtualne w tym samym regionie. |
| [Kierować ruchem przez urządzenie wirtualne sieci](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | Tworzy sieć wirtualną z podsieci frontonu i zaplecza i maszynę Wirtualną, która może kierować ruchem między dwiema podsieciami. |
| [Filtrowanie ruchu przychodzącego i wychodzącego ruchu sieciowego maszyny Wirtualnej](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | Tworzy sieć wirtualną z podsieci frontonu i zaplecza. Ruch sieciowy przychodzący do podsieci frontonu jest ograniczone do protokołu HTTP, HTTPS i SSH. Ruch wychodzący do Internetu z podsieci wewnętrznej jest niedozwolone. |