---
title: "Omówienie wysokiej dostępności portów na platformie Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat równoważenia obciążenia wewnętrznego modułu równoważenia obciążenia porty wysokiej dostępności"
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
ms.openlocfilehash: e72fc0d4323f7a2d203fee66311c3fea10ad7a09
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="high-availability-ports-overview-preview"></a>Omówienie wysokiej dostępności portów (wersja zapoznawcza)

Azure załadować równoważenia standardowe wprowadza nowe możliwość ładowania przepływy TCP i UDP Saldo na wszystkich portach jednocześnie, używając wewnętrzny moduł równoważenia obciążenia. 

>[!NOTE]
> Funkcja wysokiej dostępności portów jest dostępna, przy użyciu Standard modułu równoważenia obciążenia i obecnie w wersji zapoznawczej. W okresie obowiązywania wersji zapoznawczej ta funkcja może nie oferować dostępności i niezawodności na tym samym poziomie, co funkcje w wersji ogólnodostępnej. Aby uzyskać więcej informacji, zobacz [Dodatkowe warunki użytkowania dotyczące wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Jest Załóż Podgląd standardowe usługi równoważenia obciążenia do korzystania z portów HA z zasobami standardowe usługi równoważenia obciążenia. Postępuj zgodnie z instrukcjami dotyczącymi tworzenia konta oprócz modułu równoważenia obciążenia [standardowe Podgląd](https://aka.ms/lbpreview#preview-sign-up) również.

Reguła HA portów jest wariant skonfigurowane na wewnętrzny załadować równoważenia standardowe reguły równoważenia obciążenia.  Scenariusze są uproszczone przez zapewnienie jednej reguły równoważeniem obciążenia na potrzeby równoważenia obciążenia, wszystkie protokoły TCP i UDP przepływów przychodzących na wszystkich portach wewnętrznych frontonu standardowe usługi równoważenia obciążenia. Decyzja równoważenia obciążenia jest wprowadzone na przepływ oparte na 5 elementowej źródłowy adres IP, portu źródłowego, docelowego adresu IP, docelowy Port i protokół.

HA porty umożliwia scenariuszy o kluczowym znaczeniu, takie jak wysoka dostępność i skalowania dla sieci wirtualnych urządzeń (NVA) wewnątrz sieci wirtualnych, a także inne scenariusze, w którym musi być duża liczba portów zrównoważonym obciążeniu. 

HA portów jest konfigurowane przez ustawienie frontonu i zaplecza porty **0** i protokołu do **wszystkich**.  Zasobu wewnętrznego modułu równoważenia obciążenia równoważy teraz wszystkie przepływy TCP i UDP, niezależnie od tego, numer portu.

## <a name="why-use-ha-ports"></a>Dlaczego warto używać portów wysokiej dostępności

### <a name="nva"></a>Urządzenie wirtualne sieci

Urządzenie wirtualne sieci (NVA) służy do zabezpieczania obciążenie platformy Azure z różnego rodzaju zagrożenia bezpieczeństwa. W przypadku używania NVA w tych scenariuszach muszą być wysokiej dostępności, niezawodnych i skalowalnych w poziomie na żądanie.

W danym scenariuszu można osiągnięcie tych celów, po prostu dodając NVA wystąpień do puli wewnętrznej bazy danych Azure wewnętrzny moduł równoważenia obciążenia i skonfigurowaniu reguły HA portów usługi równoważenia obciążenia.

HA portów zapewniają wiele korzyści dla scenariuszy NVA HA:
- szybko przełączyć dobrej kondycji wystąpień z każdym sondy kondycji wystąpienia
- większą wydajność, ale skalowalnego w poziomie do n aktywny wystąpień
- scenariusze n aktywny i aktywny / pasywny
- wyeliminowanie konieczności złożonych rozwiązań, takich jak węzły dozorcy monitorowania urządzeń

Poniższy przykład przedstawia informacje o wdrożeniu sieci wirtualnej-gwiazdy z tunelowania ruch do sieci wirtualnej koncentratora i za pośrednictwem analizę NVA przed opuszczeniem miejsca zaufanych wymuszonego klienci. NVAs znajdują się za obciążenia równoważenia wzorca wewnętrznego z konfiguracją HA portów.  Całego ruchu mogą być przetwarzane i odpowiednio do przodu. 

![ha porty przykład](./media/load-balancer-ha-ports-overview/nvaha.png)

Rysunek 1 —-gwiazdy sieci wirtualnej z NVAs wdrożone w trybie wysokiej dostępności

Jeśli korzystasz z wirtualnych urządzeń sieciowych, potwierdź u dostawcy odpowiednich jak najlepiej użyć HA portów i jakie scenariusze są obsługiwane.

### <a name="load-balancing-large-numbers-of-ports"></a>Duża liczba portów Równoważenie obciążenia

Umożliwia także HA portów dla scenariuszy aplikacji, które wymagają balanicng obciążenia dużej liczby portów. Te scenariusze można uprościć przy użyciu wewnętrznego [standardowe usługi równoważenia obciążenia](https://aka.ms/lbpreview) z HA portami gdzie jednej reguły równoważenia obciążenia zastępuje wiele obciążenia poszczególnych reguł, po jednej dla każdego portu równoważenia.

## <a name="region-availability"></a>Dostępność w danym regionie

HA porty są dostępne w [tego samego regionach w ramach standardowego modułu równoważenia obciążenia](https://aka.ms/lbpreview#region-availability).  

## <a name="preview-sign-up"></a>Podgląd rejestracji

Aby wziąć udział w wersji zapoznawczej funkcji porty wysokiej dostępności w standardowe usługi równoważenia obciążenia, Zarejestruj swoją subskrypcję do uzyskania dostępu za pomocą 2.0 interfejsu wiersza polecenia platformy Azure lub programu PowerShell.  Wykonaj poniższe trzy kroki:

>[!NOTE]
>Aby użyć tej funkcji, należy również rejestracji dla usługi równoważenia obciążenia [standardowe Podgląd](https://aka.ms/lbpreview#preview-sign-up) oprócz HA portów. Rejestracja podglądów HA portów lub standardowy moduł równoważenia obciążenia może potrwać do godziny.

### <a name="sign-up-using-azure-cli-20"></a>Zarejestruj się przy użyciu usługi Azure CLI 2.0

1. Zarejestruj tę funkcję z dostawcą
    ```cli
    az feature register --name AllowILBAllPortsRule --namespace Microsoft.Network
    ```
    
2. Poprzednich operacji może potrwać do 10 minut.  Można sprawdzić stan operacji za pomocą następującego polecenia:

    ```cli
    az feature show --name AllowILBAllPortsRule --namespace Microsoft.Network
    ```
    
    Przejdź do kroku 3, gdy stan rejestracji funkcji zwraca "Zarejestrowanej", jak pokazano poniżej:
   
    ```json
    {
       "id": "/subscriptions/foo/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowLBPreview",
       "name": "Microsoft.Network/AllowILBAllPortsRule",
       "properties": {
          "state": "Registered"
       },
       "type": "Microsoft.Features/providers/features"
    }
    ```
    
3. Wykonaj Podgląd tworzenia konta, rejestrując ponownie subskrypcji u dostawcy zasobów:

    ```cli
    az provider register --namespace Microsoft.Network
    ```
    
### <a name="sign-up-using-powershell"></a>Zaloguj przy użyciu programu PowerShell

1. Zarejestruj tę funkcję z dostawcą
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```
    
2. Poprzednich operacji może potrwać do 10 minut.  Można sprawdzić stan operacji za pomocą następującego polecenia:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```
    Przejdź do kroku 3, gdy stan rejestracji funkcji zwraca "Zarejestrowanej", jak pokazano poniżej:
   
    ```
    FeatureName          ProviderName      RegistrationState
    -----------          ------------      -----------------
    AllowILBAllPortsRule Microsoft.Network Registered
    ```
    
3. Wykonaj Podgląd tworzenia konta, rejestrując ponownie subskrypcji u dostawcy zasobów:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```


## <a name="limitations"></a>Ograniczenia

Poniżej przedstawiono obsługiwane konfiguracje lub wyjątki dla portów HA:

- Pojedynczy konfiguracji IP frontonu może mieć jedną regułę równoważenia obciążenia DSR z portami HA lub może mieć reguły modułu równoważenia obciążenia w jednej z systemem innym niż DSR z portami HA. Nie może mieć jednocześnie.
- Jednej konfiguracji IP interfejsu sieciowego może mieć tylko jeden z systemem innym niż — DSR reguła modułu równoważenia z portami HA obciążenia. Żadne inne reguły można skonfigurować dla tego polecenia ipconfig.
- Jednej konfiguracji IP interfejsu sieciowego może mieć jeden lub więcej reguł modułu równoważenia obciążenia DSR z HA portami, pod warunkiem wszystkie ich konfiguracji IP frontonu odpowiednich są unikatowe.
- Jeśli wszystkie równoważenia obciążenia reguł są porty HA DSR (tylko) lub, wszystkie reguły są nie - HA porty DSR & (z systemem innym niż DSR), wskazuje na tym samym może puli wewnętrznej bazy danych (co najmniej dwa) reguła moduł równoważenia obciążenia współistnieć. Dwa takich reguł równoważenia obciążenia nie mogą współistnieć w przypadku kombinację reguły HA portów i nie - HA portów.
- HA portów nie jest dostępna dla protokołu IPv6.
- Przepływ symetrycznego dla scenariuszy NVA jest obsługiwana z tylko jedną kartę Sieciową. Zobacz opis i diagram dla [wirtualnych urządzeń sieciowych](#nva). 



## <a name="next-steps"></a>Następne kroki

- [Skonfiguruj porty HA na obciążenia równoważenia wzorca wewnętrznego](load-balancer-configure-ha-ports.md)
- [Więcej informacji na temat podglądu standardowe usługi równoważenia obciążenia](https://aka.ms/lbpreview)

