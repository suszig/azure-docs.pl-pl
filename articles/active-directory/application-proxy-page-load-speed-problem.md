---
title: "Aplikacja serwera Proxy aplikacji trwa zbyt długo, aby załadować | Dokumentacja firmy Microsoft"
description: "Strona obciążenia Rozwiązywanie problemów z wydajnością z serwer Proxy aplikacji usługi Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: ce462c90746e6af0dc201686557121665b82b93d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Aplikacja serwera Proxy aplikacji trwa zbyt długo, aby załadować

Ten artykuł ułatwia zrozumienie, dlaczego aplikacja serwera Proxy aplikacji usługi Azure AD może zająć dużo czasu ładowania i co można zrobić, aby rozwiązać ten problem.

## <a name="overview"></a>Omówienie
Jeśli pracy aplikacji, ale Zobacz długi czas oczekiwania, w topologii sieci, które warto uwzględnić w celu zwiększenia szybkości może istnieć kilka ulepszeń pomocniczych. W wersji ewaluacyjnej topologii, zobacz [dokumentu zagadnienia dotyczące sieci](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).

Jeśli te zagadnienia nie pomogły, Niestety nie ma obecnie mamy dodatkowe zalecenia dotyczące dostrajania wydajności. Jak usługa serwera Proxy aplikacji zostanie rozszerzony więcej centrów danych, które mogą być zbliżonej do Ciebie, można rozpocząć bezpośrednio Zobacz ulepszone opóźnienia. Aby zapoznać się z pełną listą centrach danych platformy Azure, zobacz [stronę testową opóźnienia](http://www.azurespeed.com/Azure/Latency). 

Centra danych z serwera Proxy aplikacji usługi można znaleźć z [narzędzia Test porty łącznika](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Opinię na lokalizacje centrum danych serwer Proxy aplikacji 
Może to być centrach danych platformy Azure, które jeszcze nie zawierają serwera Proxy aplikacji, ale może doprowadzić do poprawy opóźnienia doskonały dla Ciebie. Centrum danych lokalizacji < aadapfeedback@microsoft.com > , możemy użyć swoją opinię do zaplanowania, jak firma Microsoft Rozwiń.

Firma Microsoft pracuje nad pewne dodatkowe możliwości, które zwiększyć opóźnienie dzierżawcami, które są aktualnie długie opóźnienia i pamiętaj udostępnić dokumentacji, gdy są one dostępne.

## <a name="next-steps"></a>Następne kroki
[Praca z istniejącym lokalnych serwerów proxy](application-proxy-working-with-proxy-servers.md)
