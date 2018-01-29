---
title: "Obramowanie łączności sieciowej integracji zagadnienia dotyczące stosu Azure zintegrowanych systemów | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zaplanować łączności sieciowej obramowania centrum danych z wieloma węzłami Azure stosu."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: c1a5496f3ab9a625d7d97c3096ae89100b7c5592
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="border-connectivity"></a>Łączność obramowania 
Planowanie integracji sieci jest wstępnym ważne dla pomyślnego stosu Azure zintegrowanych systemów wdrożenie operacji i zarządzania. Planowanie łączności obramowania rozpoczyna się przez wybranie, czy należy korzystać z routingu dynamicznego z protokołu border gateway protocol (BGP). Wymaga to przypisywanie 16-bitowy numer systemu autonomicznego BGP (publicznej lub prywatnej) lub przy użyciu routingu statycznego, gdy domyślna trasa statyczna jest przypisany do urządzeń obramowania.

> [!IMPORTANT]
> Na początku przełączniki rack (TOR) wymagają warstwy 3 pasm z adresów IP Point-to-Point (/ 30 sieci) skonfigurowany na interfejsy fizyczne. Pasm warstwy 2 za pomocą przełączniki TOR obsługi operacji Azure stosu nie jest obsługiwane. 

## <a name="bgp-routing"></a>BGP routing
Przy użyciu protokołem routingu dynamicznego, takie jak Protokół BGP gwarantuje, że system jest zawsze pamiętać zmian w sieci i ułatwia administrację. 

Jak pokazano na poniższym diagramie, anonsujący prywatnego adresu IP miejsca na przełączniku TOR jest ograniczony przy użyciu listy prefiks. Wyświetla prefiks nie zezwala na prywatne podsieci IP i zastosowanie go jako mapy trasy dla połączenia między TOR i granicy.

Moduł równoważenia obciążenia oprogramowania (Programowego) uruchomionych wewnątrz rozwiązania Azure stosu równorzędnymi użytkownikami urządzeń TOR tak ją dynamicznie anonsowanie adresów VIP.

Do zapewnienia, że ruchu użytkowników natychmiast i w przezroczysty sposób odzyskiwania po awarii, VPC lub MLAG skonfigurowane między urządzeniami TOR zezwala na korzystanie z wielu obudowy łącze agregacji do hostów i HSRP lub VRRP, która zapewnia sieci nadmiarowości w sieciach IP.

![BGP routing](media/azure-stack-border-connectivity/bgp-routing.png)

## <a name="static-routing"></a>routing statyczny
Przy użyciu statycznych tras dodaje więcej stałym konfiguracji do obramowania i TOR urządzenia. Wymaga to szczegółowej analizy przed każdą zmianę. Problemy spowodowane błędem konfiguracji może zająć więcej czasu można wycofać, w zależności od zmian. Nie jest to zalecana metoda routingu, ale jest on obsługiwany.

Aby zintegrować stosu Azure do środowiska sieciowego przy użyciu tej metody, urządzenie obramowania musi być skonfigurowane przy użyciu statycznych tras wskazujący urządzenia TOR do ruch kierowany do sieci zewnętrznej publicznych adresów VIP.

Trasa domyślna statycznych wysyłania całego ruchu na urządzeniach obramowania musi być skonfigurowany urządzeń TOR. Jedynym wyjątkiem ruchu w do tej reguły jest prywatnej miejsca, które zostaną zablokowane, za pomocą listy kontroli dostępu stosowane na TOR do połączenia obramowania.

Wszystkie inne powinna być taka sama jak pierwsza metoda. BGP routingu dynamicznego będzie można nadal używane w stojaku, ponieważ jest on podstawowe narzędzie Programowego i inne składniki i nie może być wyłączone lub usunięte.

![routing statyczny](media/azure-stack-border-connectivity/static-routing.png)

## <a name="transparent-proxy"></a>Przezroczystego obiektu pośredniczącego
Jeśli centrum danych wymaga cały ruch do korzystania z serwera proxy, należy skonfigurować *przezroczystego obiektu pośredniczącego* przetworzyć cały ruch z stojak, aby postępować zgodnie z zasadami oddzielania ruchu między strefami w sieci.

> [!IMPORTANT]
> Rozwiązania Azure stosu nie obsługuje normalne internetowego serwera proxy.  

Przezroczystego obiektu pośredniczącego (znanej także jako przechwycenia, wbudowanego lub wymuszonego proxy) przechwytuje Normalna komunikacja w warstwie sieci bez konieczności żadnej konfiguracji specjalnych klienta. Klienci nie muszą mieć świadomość istnienia serwera proxy.

![Przezroczystego obiektu pośredniczącego](media/azure-stack-border-connectivity/transparent-proxy.png)

## <a name="next-steps"></a>Kolejne kroki
[Integracja z usługą DNS](azure-stack-integrate-dns.md)