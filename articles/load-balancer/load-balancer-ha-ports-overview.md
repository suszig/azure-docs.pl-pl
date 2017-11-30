---
title: "Omówienie portów wysokiej dostępności na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej na temat równoważenia obciążenia wewnętrznego modułu równoważenia obciążenia portów wysokiej dostępności."
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: kumud
ms.openlocfilehash: f72f4a3a81fc3a03c86805787caeeacbe6135c5e
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="high-availability-ports-overview"></a>Omówienie portów wysokiej dostępności

Azure załadować równoważenia standardowego pomaga załadować przepływy TCP i UDP Saldo na wszystkich portach jednocześnie, gdy jest używany do wewnętrznego modułu równoważenia obciążenia. 

>[!NOTE]
> Funkcja porty wysokiej dostępności (HA) jest dostępna w programie standardowe usługi równoważenia obciążenia i jest obecnie w przeglądzie. Podczas udostępniania wersji zapoznawczej funkcja może nie mieć taki sam poziom dostępności i niezawodności jako funkcje, które są w wersji ogólnodostępnej. Aby uzyskać więcej informacji, zobacz [Dodatkowe warunki użytkowania dotyczące wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Załóż Podgląd standardowe usługi równoważenia obciążenia do korzystania z portów wysokiej dostępności z zasobami standardowe usługi równoważenia obciążenia. Postępuj zgodnie z instrukcjami dotyczącymi tworzenia konta do modułu równoważenia obciążenia [standardowe Podgląd](https://aka.ms/lbpreview#preview-sign-up) również.

Reguła portów HA jest wariant skonfigurowano na wewnętrzny załadować równoważenia standardowe reguły równoważenia obciążenia. Korzystanie z usługi równoważenia obciążenia można uprościć przez zapewnienie jednej reguły równoważenia obciążenia wszystkie protokoły TCP i UDP przepływów przychodzących na wszystkich portach wewnętrzny załadować równoważenia Standard. Decyzja równoważenia obciążenia jest wykonywana na przepływ. Jest oparta na następujące połączenia 5 elementowej: źródłowy adres IP, portu źródłowego, docelowego adresu IP, docelowy Port i protokół.

Funkcja porty HA pomaga scenariuszy o kluczowym znaczeniu, takie jak wysokiej dostępności i skalowalności w przypadku urządzeń wirtualnych sieci (NVA) w sieciach wirtualnych. Może również pomóc, podczas dużej liczby portów muszą pochodzić ze zrównoważonym obciążeniem. 

Funkcję wysokiej dostępności portów jest konfigurowane podczas ustawioną portów frontonu i zaplecza **0**i protokół **wszystkich**. Zasobu wewnętrznego modułu równoważenia obciążenia równoważy następnie wszystkie przepływy TCP i UDP, niezależnie od tego, numer portu.

## <a name="why-use-ha-ports"></a>Dlaczego warto używać portów wysokiej dostępności?

### <a name="nva"></a>Urządzenie wirtualne sieci

NVAs służy do zabezpieczania obciążenie platformy Azure z różnego rodzaju zagrożenia bezpieczeństwa. W przypadku używania NVAs w tych scenariuszach muszą być niezawodnych i wysokiej dostępności, a musi skalowanie na żądanie.

Za osiągnięcie tych celów poprzez dodanie wystąpienia NVA do puli zaplecza Azure wewnętrznego modułu równoważenia obciążenia i skonfigurowaniu reguły modułu równoważenia obciążenia porty wysokiej dostępności.

HA portów zapewniają wiele korzyści dla scenariuszy NVA HA:
- Szybkie trybu failover do dobrej kondycji wystąpień, sondy kondycji poszczególnych wystąpień
- Wyższa wydajność skalowania w poziomie do  *n* -aktywnych wystąpień
- *N*-scenariuszy aktywnej i aktywny / pasywny
- Wyeliminowanie konieczności złożonych rozwiązań, takich jak Apache ZooKeeper węzły do monitorowania urządzeń

Na poniższym diagramie przedstawiono wdrożenia sieci wirtualnej-gwiazdy. Wymuś tunelu partnerzy ruch do sieci wirtualnej koncentratora i za pośrednictwem NVA, przed opuszczeniem zaufanych miejsca. NVAs znajdują się za wewnętrzny obciążenia modułu równoważenia standardowej konfiguracji portów wysokiej dostępności. Cały ruch może być przetwarzane i przekazywane w związku z tym.

![Diagram sieci wirtualnej-gwiazdy, o NVAs wdrożone w trybie wysokiej dostępności](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Jeśli używasz NVAs Potwierdź u dostawcy odpowiednich jak najlepiej użyć portów wysokiej dostępności i jakie scenariusze są obsługiwane.

### <a name="load-balancing-large-numbers-of-ports"></a>Duża liczba portów Równoważenie obciążenia

Umożliwia także porty wysokiej dostępności dla aplikacji, które wymagają dużej liczby portów równoważenia obciążenia. Te scenariusze można uprościć przy użyciu wewnętrznego [standardowe usługi równoważenia obciążenia](https://aka.ms/lbpreview) z portami wysokiej dostępności. Reguły równoważenia obciążenia pojedynczego zastępuje wiele obciążenia poszczególnych reguł, po jednej dla każdego portu równoważenia.

## <a name="region-availability"></a>Dostępność w danym regionie

Funkcja porty HA jest dostępna w [tego samego regionach w ramach standardowego modułu równoważenia obciążenia](https://aka.ms/lbpreview#region-availability).  

## <a name="preview-sign-up"></a>Podgląd rejestracji

Aby wziąć udział w wersji zapoznawczej funkcji porty wysokiej dostępności w standardowe usługi równoważenia obciążenia, należy zarejestrować subskrypcji dla usługi równoważenia obciążenia [standardowe Podgląd](https://aka.ms/lbpreview#preview-sign-up). Można zarejestrować za pomocą 2.0 interfejsu wiersza polecenia platformy Azure lub programu PowerShell.

>[!NOTE]
>Rejestracja może potrwać do godziny.

## <a name="limitations"></a>Ograniczenia

Poniżej przedstawiono obsługiwane konfiguracje lub wyjątki dla portów funkcji wysokiej dostępności:

- Z jednej konfiguracji IP frontonu może mieć pojedynczy DSR reguły modułu równoważenia obciążenia z portami wysokiej dostępności, lub może być reguły modułu równoważenia obciążenia w jednej z systemem innym niż DSR z portami wysokiej dostępności. Nie może mieć jednocześnie.
- Konfiguracja IP interfejsu sieciowego pojedynczego może mieć tylko jeden z systemem innym niż — DSR załadować reguły modułu równoważenia z portami wysokiej dostępności. Nie można skonfigurować inne zasady dla tego polecenia ipconfig.
- Konfiguracja IP interfejsu sieciowego pojedynczego mogą mieć jeden lub więcej DSR reguły modułu równoważenia z portami HA obciążenia, pod warunkiem wszystkich ich odpowiednich konfiguracji IP frontonu są unikatowe.
- Jeśli wszystkie reguły równoważenia obciążenia HA portów DSR (tylko), mogą współistnieć wskazuje do tej samej puli zaplecza (co najmniej dwa) reguła moduł równoważenia obciążenia. Jest taka sama wartość true, jeśli są wszystkie reguły z systemem innym niż — HA portów (DSR i z systemem innym niż DSR). W przypadku kombinację portów wysokiej dostępności i reguł portów bez wysokiej dostępności, jednak dwóch takich reguł równoważenia obciążenia nie mogą współistnieć.
- Funkcja porty wysokiej dostępności jest niedostępna dla protokołu IPv6.
- Przepływ symetrycznego dla scenariuszy NVA jest obsługiwana z tylko jedną kartę Sieciową. Zobacz opis i diagram dla [sieci wirtualnych urządzeń](#nva). 



## <a name="next-steps"></a>Następne kroki

- [Skonfiguruj porty wysokiej dostępności na obciążenia równoważenia wzorca wewnętrznego](load-balancer-configure-ha-ports.md)
- [Więcej informacji na temat podglądu standardowe usługi równoważenia obciążenia](https://aka.ms/lbpreview)

