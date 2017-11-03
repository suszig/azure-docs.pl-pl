---
title: "Zarządzanie regionu Azure stosu | Dokumentacja firmy Microsoft"
description: "Omówienie zarządzania regionu Azure stosu."
services: azure-stack
documentationcenter: 
author: efemmano
manager: dsavage
editor: 
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: efemmano
ms.openlocfilehash: d1310f0cb9a820366ab8712a782785e955a24134
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="region-management-in-azure-stack"></a>Zarządzanie regionu Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Stos Azure korzysta z koncepcji regionach, które są jednostek logicznych obejmuje zasoby sprzętowe wchodzące w skład infrastruktury Azure stosu. Wewnątrz obszaru zarządzania można znaleźć wszystkie zasoby, które są wymagane do pomyślnie działać cyklu życia infrastruktury Azure stosu.

Jedna zintegrowana wdrożenia systemu (nazywane *chmury Azure stosu*) stanowi pojedynczym regionie. Każdy zestaw deweloperski stosu Azure ma jeden region, o nazwie **lokalnego**. Jeśli wdrożyć drugiego systemu Azure stosu zintegrowane, lub skonfigurować inne wystąpienie development kit na oddzielnego sprzętu, chmury Azure stosu jest inny region.

## <a name="information-available-through-the-region-management-tile"></a>Informacje są dostępne za pośrednictwem kafelka zarządzania regionu
Stos Azure ma zestaw możliwości zarządzania regionu są dostępne w **zarządzania Region** kafelka. Ten Kafelek jest dostępny do operatorem Azure stosu dla domyślnego pulpitu nawigacyjnego w portalu administratora. Za pomocą tego kafelka można monitorować i aktualizować regionu Azure stosu i jego składników, które są specyficzne dla danego regionu.

 ![Na kafelku zarządzania regionu](media/azure-stack-manage-region/image1.png)

 Jeśli klikniesz przycisk region na kafelku zarządzania regionu, są dostępne następujące informacje:

  ![Opis okienka w bloku zarządzania regionu](media/azure-stack-manage-region/image2.png)

1. **Menu zasobów**. W tym miejscu można dostęp do infrastruktury określonych obszarów zarządzania, wyświetlanie i zarządzanie zasobami użytkownika, takich jak konta magazynu i sieci wirtualnych.

2. **Alerty**. Ten Kafelek zawiera listę całego systemu alerty, a także szczegółowe informacje na każdym z tych alertów.

3. **Aktualizacje**. W tym kafelku można wyświetlić bieżącej wersji infrastruktury Azure stosu.

4. **Dostawcy zasobów**. Dostawcy zasobów jest używana do zarządzania funkcjami dzierżawy oferowane przez składniki wymagane do uruchomienia stosu Azure. Każdy dostawca zasobów zawiera środowisko administracyjne. To środowisko może obejmować alerty dla określonego dostawcy, metryki i innych funkcji zarządzania specyficzne dla dostawcy zasobów.
 
5. **Role infrastruktury**. Infrastruktura role są składniki niezbędne do uruchomienia stosu Azure. Tylko role infrastruktury, które zgłosiły alerty są wyświetlane. Klikając rolę, można wyświetlić alertów skojarzonych z określonych ról i wystąpień ról, którym jest uruchomiona ta rola. Mimo że istnieje możliwość uruchomienia, uruchom ponownie, lub zamknij wystąpienie roli infrastruktury, wykonaj **nie** to zrobić w środowisku deweloperskim zestaw. Te opcje są przeznaczone tylko dla środowisku z wieloma węzłami, gdy istnieje więcej niż jedno wystąpienie roli dla każdej roli infrastruktury. Ponowne uruchomienie wystąpienia roli (szczególnie Xrp01 AzS) w zestawie powoduje niestabilność systemu.

## <a name="next-steps"></a>Następne kroki
[Monitorowanie kondycji i alertów w stosie Azure](azure-stack-monitor-health.md)

[Zarządzanie aktualizacjami w stosie Azure](azure-stack-updates.md)






