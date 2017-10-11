---
title: "Co robić w przypadku przerw w działaniu usługi Azure wpływających na sieciach wirtualnych platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, co należy zrobić w przypadku przerw w działaniu usługi Azure wpływających na sieciach wirtualnych platformy Azure."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: 4e125406d2e798138c45e3fbbf61a610afab69fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="virtual-network--business-continuity"></a>Sieć wirtualna — ciągłość prowadzenia działalności biznesowej
## <a name="overview"></a>Omówienie
Sieć wirtualną (VNet) jest logiczną reprezentacja sieci w chmurze. Umożliwia definiowanie własnych prywatnych przestrzeni adresów IP i podzielić sieć na podsieci. Sieci wirtualne służy jako granicę zaufania, aby obsługiwać zasoby obliczeniowe, takich jak maszyny wirtualne platformy Azure i usługi w chmurze (role sieć web/proces roboczy). Sieć wirtualną umożliwia bezpośrednie prywatnej komunikacji IP między zasobami znajdujące się w nim. Sieci wirtualnej również może być połączony z siecią lokalną za pomocą jednego z opcji hybrydowych, takich jak brama sieci VPN lub usługi ExpressRoute.

Sieci wirtualnej jest tworzony w zakresie regionu. Sieci wirtualne można tworzyć z tą samą przestrzenią adresów w dwóch różnych regionach (tj. nam wschodnie i zachód nam, ale nie można połączyć je bezpośrednio ze sobą). 

## <a name="business-continuity"></a>Ciągłość działania
Może istnieć kilka różnych sposobów, aplikacja może zostać zerwane. Danego regionu można całkowicie obcięty z powodu klęski żywiołowej lub częściowe po awarii z powodu błędu wielu urządzeń/usług. Wpływ na usługi sieci wirtualnej różni się w każdej z tych sytuacji.

**Pytanie: co chcesz zrobić w przypadku awarii całego regionu oznacza, że jeśli region jest całkowicie odcięcia z powodu klęski żywiołowej? Co się dzieje z sieci wirtualnych hostowanych w regionie?**

A: sieci wirtualne i zasoby w regionie, których dotyczy pozostają niedostępne podczas przerw w działaniu usługi.

![Diagram prostego sieci wirtualnej](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**Pytanie: jakie czy można ponownie utworzyć tej samej sieci wirtualnej w innym regionie?**

A: sieć wirtualna (VNet) jest stosunkowo lekkie zasobów. Mogą wywoływać interfejsy API Azure w celu utworzenia sieci wirtualnej z tą samą przestrzenią adresów w innym regionie. Aby ponownie utworzyć w tym samym środowisku, która znajdowała się w regionie, których dotyczy, konieczne będzie wprowadzenie wywołań interfejsu API do ponownego wdrożenia usługi w chmurze (role sieć web/proces roboczy) i maszyn wirtualnych, które były. Należy również aż bramy sieci VPN i łączyć się z siecią lokalną gdyby łączności lokalnie (np. hybrydowej).

Znajdują się instrukcje dotyczące tworzenia sieci wirtualnej [tutaj](virtual-networks-create-vnet-arm-pportal.md). 

**Pytanie: czy replika sieci wirtualnej w danym regionie zostać ponownie utworzone w innym regionie wcześniejsze?**

Odpowiedź: tak, można utworzyć dwie sieci wirtualnych za pomocą tego samego prywatnych przestrzeni adresów IP i zasobów w dwóch różnych regionach wcześniejsze. Jeśli klient został hosting internetowy usług w sieci wirtualnej, ich może ustawiono Konfigurowanie Menedżera ruchu geograficznie kierowanie ruchem do regionu, który jest aktywny. Jednak klient nie może połączyć dwie sieci wirtualne z tą samą przestrzenią adresów do ich sieci lokalnej, ponieważ spowodowałoby to problemy routingu. W czasie awarii i utratę sieci wirtualnej w jednym regionie klienta można połączyć sieć wirtualną w regionie dostępne z pasującymi przestrzeni adresowej dla sieci lokalnej.

Znajdują się instrukcje dotyczące tworzenia sieci wirtualnej [tutaj](virtual-networks-create-vnet-arm-pportal.md).

