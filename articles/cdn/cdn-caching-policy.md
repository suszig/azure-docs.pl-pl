---
title: "Zarządzanie Azure CDN buforowanie zasad w usłudze Azure Media Services | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie zarządzania usługi Azure CDN buforowanie zasad w usłudze Azure Media Services."
services: media-services,cdn
documentationcenter: .NET
author: juliako
manager: erikre
editor: 
ms.assetid: be33aecc-6dbe-43d7-a056-10ba911e0e94
ms.service: media-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2017
ms.author: juliako
ms.openlocfilehash: 0c479a58f4158bb1a72dc43432507160f65d2791
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a>Zarządzanie Azure CDN buforowanie zasad w usłudze Azure Media Services
Usługa Azure Media Services zapewnia HTTP adaptacyjnego przesyłania strumieniowego i pobierania progresywnego. Oparty na protokole HTTP przesyłania strumieniowego jest wysoce skalowalna z zalet buforowanie w serwera proxy i warstwy CDN, jak również buforowanie po stronie klienta. Punkty końcowe przesyłania strumieniowego zawiera ogólne możliwości przesyłania strumieniowego, a także konfiguracji nagłówków HTTP pamięci podręcznej. Punkty końcowe przesyłania strumieniowego ustawia HTTP Cache-Control: maksymalny wiek i Expires headers. Można uzyskać dodatkowe informacje dotyczące nagłówków HTTP pamięci podręcznej z [adresem W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

## <a name="default-caching-headers"></a>Domyślne nagłówki buforowanie
Domyślnie — punkty końcowe przesyłania strumieniowego zastosować 3 dni nagłówków pamięci podręcznej danych przesyłania strumieniowego na żądanie (multimedialna fragmenty/fragmentów) i manifest(playlist). Transmisja strumieniowa na żywo punktów końcowych przesyłania strumieniowego zastosować 3 dni nagłówków pamięci podręcznej danych (multimedialna fragmenty/fragmentów) i nagłówek żądania manifest(playlist) pamięci podręcznej 2 sekundy. Podczas program na żywo zmieni się na żądanie (archiwum na żywo) mają zastosowanie nagłówków pamięci podręcznej z przesyłania strumieniowego na żądanie.

## <a name="azure-cdn-integration"></a>Integracja z usługą Azure CDN
Azure Media Services udostępnia [zintegrowane CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) dla — punkty końcowe przesyłania strumieniowego. Nagłówki kontroli pamięci podręcznej stosuje się w taki sam sposób jak punktów końcowych przesyłania strumieniowego do sieci CDN włączone punktów końcowych przesyłania strumieniowego. Azure używa CDN punktu końcowego przesyłania strumieniowego skonfigurowane wartości pamięci podręcznej, aby zdefiniować okres istnienia obiektów wewnętrznie pamięci podręcznej i używa również tę wartość można ustawić dostarczanie nagłówków pamięci podręcznej. Włączenie przy użyciu CDN punktów końcowych przesyłania strumieniowego nie zaleca się ustawiania wartości małych pamięci podręcznej. Ustawienie wartości małych zmniejszenie wydajności i zmniejszenia korzyści CDN. Ustaw mniejszy niż 600 sekund dla CDN włączone punktów końcowych przesyłania strumieniowego nagłówków pamięci podręcznej jest niedozwolone.

> [!IMPORTANT]
>Usługa Azure Media Services zawiera pełną integrację z usługą Azure CDN. Za pomocą jednego kliknięcia można zintegrować wszystkie dostępne usługi Azure CDN dostawców (Akamai i Verizon) do punktu końcowego przesyłania strumieniowego w tym produktów w sieci CDN w warstwie standardowa i Premium. Aby uzyskać więcej informacji, zobacz [anonsu](https://azure.microsoft.com/blog/standardstreamingendpoint/).
> 
> Tylko opłatami za przesyłanie danych z przesyłania strumieniowego punktu końcowego CDN pobiera wyłączony, jeśli element CDN jest włączona za pośrednictwem interfejsów API punktu końcowego przesyłania strumieniowego lub przy użyciu sekcji punktu końcowego przesyłania strumieniowego portalu zarządzania Azure. Integracja ręczna lub bezpośrednio tworzenia punktu końcowego usługi CDN przy użyciu interfejsów API usługi CDN lub portalu sekcji nie spowoduje wyłączenie opłatami za przesyłanie danych.

## <a name="configuring-cache-headers-with-azure-media-services"></a>Konfigurowanie nagłówków pamięci podręcznej w usłudze Azure Media Services
Aby skonfigurować wartości nagłówka pamięci podręcznej, można użyć portalu zarządzania Azure lub interfejsów API usługi Azure Media Services.

1. Konfigurowanie nagłówków pamięci podręcznej za pomocą portalu zarządzania zapoznaj się [jak zarządzać punktami końcowymi przesyłania strumieniowego](../media-services/media-services-portal-manage-streaming-endpoints.md) sekcji konfiguracji punktu końcowego przesyłania strumieniowego.
2. W przypadku interfejsu API REST usługi Azure Media Services [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Zestaw .NET SDK usługi Azure Media Services [StreamingEndpointCacheControl właściwości](http://go.microsoft.com/fwlink/?LinkId=615302).

## <a name="cache-configuration-precedence-order"></a>Kolejność pierwszeństwa konfiguracji pamięci podręcznej
1. Azure Media Services skonfigurowany pamięci podręcznej wartość zastępuje wartości domyślne.
2. Jeśli nie istnieje konfiguracja ręczna, zastosowanie wartości domyślnych.
3. Przez pamięć podręczną 2 sekundy domyślne nagłówki ma zastosowanie do aktywnego przesyłania strumieniowego manifest(playlist) niezależnie od konfiguracji multimediów Azure lub usługi Azure Storage i zastępowanie tej wartości nie jest dostępna.

