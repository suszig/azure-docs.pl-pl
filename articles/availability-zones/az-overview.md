---
title: Przegląd stref dostępności | Dokumentacja firmy Microsoft
description: Ten artykuł zawiera omówienie stref dostępności na platformie Azure.
services: ''
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2018
ms.author: markgal
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: b4db442a54b4360b75df40156ca0d4e4ee1eb0d1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="overview-of-availability-zones-in-azure-preview"></a>Przegląd stref dostępności na platformie Azure (wersja zapoznawcza)

Dostępność strefy ułatwiają ochronę przed błędami centrum danych na poziomie. Znajdują się w regionie Azure, a każda z nich ma własny niezależne od zasilania źródła, sieci i chłodzenia. W celu zapewnienia odporności, istnieje co najmniej trzech oddzielnych stref we wszystkich regionach włączone. Fizycznych i logicznych separacji dostępności stref w obrębie regionu chroni aplikacje i dane z poziomu strefy błędów. 

![Widok koncepcyjny jedną strefę przechodzi w dół w regionie](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Regionach, które obsługują stref dostępności

- Wschodnie stany USA 2
- Środkowe stany USA
- Europa Zachodnia
- Francja Środkowa
- Azja Południowo-Wschodnia

## <a name="services-that-support-availability-zones"></a>Usługi obsługujące stref dostępności

Usług Azure, które obsługują dostępność strefy są:

- Maszyny wirtualne z systemem Linux
- Maszyny wirtualne z systemem Windows
- Zestawy skali maszyn wirtualnych
- Managed Disks
- Moduł równoważenia obciążenia
- Publiczny adres IP
- Magazyn strefowo nadmiarowy
- SQL Database

## <a name="get-started-with-the-availability-zones-preview"></a>Rozpoczynanie pracy z podglądu dostępności stref

Podgląd stref dostępności jest dostępne w wschodnie stany USA 2, nam centralnej, Europa Zachodnia i Francja centralnej regionów dla określonych usług platformy Azure. 

1. [Zarejestruj dla stref dostępności Podgląd](http://aka.ms/azenroll). 
2. Zaloguj się do subskrypcji platformy Azure.
3. Wybierz region, który obsługuje stref dostępności.
4. Aby rozpocząć korzystanie z usługą dostępność strefy, użyj jednej z poniższych łączy. 
    - [Utwórz maszynę wirtualną](../virtual-machines/windows/create-portal-availability-zone.md)
    - [Tworzenie zestawu skalowania maszyn wirtualnych](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
    - [Dodaj dysk zarządzane przy użyciu programu PowerShell](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
    - [Moduł równoważenia obciążenia](../load-balancer/load-balancer-standard-overview.md)

## <a name="next-steps"></a>Kolejne kroki
- [Quickstart templates (Szablony szybkiego startu)](http://aka.ms/azqs)
