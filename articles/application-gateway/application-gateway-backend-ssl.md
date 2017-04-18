---
title: "Włączanie zasad SSL i kompleksowej usługi SSL w usłudze Application Gateway | Microsoft Docs"
description: "Ta strona zawiera omówienie kompleksowej obsługi protokołu SSL w usłudze Application Gateway."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: amsriva
translationtype: Human Translation
ms.sourcegitcommit: 73ee330c276263a21931a7b9a16cc33f86c58a26
ms.openlocfilehash: 40368e31790a7ffa2d34a51a13e78d028cd0a1eb
ms.lasthandoff: 04/05/2017


---
# <a name="overview-of-end-to-end-ssl-and-ssl-policy-on-application-gateway"></a>Omówienie kompleksowej usługi SSL i zasad SSL w usłudze Application Gateway

Usługa Application Gateway obsługuje przerywanie połączenia SSL na bramie, po którym ruch na ogół płynie niezaszyfrowany do serwerów zaplecza. Ta funkcja umożliwia odciążenie serwerów sieci Web z nadmiaru kosztownych operacji szyfrowania i odszyfrowywania. Jednak dla niektórych klientów nieszyfrowana komunikacja z serwerami zaplecza jest opcją niemożliwą do zaakceptowania. Nieszyfrowana komunikacja może być spowodowana przez wymagania dotyczące zabezpieczeń lub zgodności albo aplikacja może akceptować jedynie bezpieczne połączenia. Na potrzeby takich aplikacji brama aplikacji obsługuje kompleksowe szyfrowanie SSL.

## <a name="overview"></a>Omówienie

Kompleksowa usługa SSL pozwala na bezpieczne przesyłanie zaszyfrowanych danych poufnych do zaplecza, umożliwiając jednocześnie korzystanie z funkcji równoważenia obciążenia warstwy 7, które oferuje usługa Application Gateway. Do tych funkcji należą koligacja sesji oparta na plikach cookie, routing oparty na adresach URL, obsługa routingu opartego na witrynach lub możliwość iniekcji nagłówków X-Forwarded-*.

Po skonfigurowaniu kompleksowego trybu komunikacji SSL usługa Application Gateway kończy sesje SSL na bramie i odszyfrowuje ruch użytkownika. Następnie stosuje skonfigurowane reguły, aby wybrać odpowiednie wystąpienie puli serwerów zaplecza w celu skierowania do nich ruchu. Następnie usługa Application Gateway inicjuje nowe połączenie SSL z serwerem zaplecza i ponownie szyfruje dane przy użyciu certyfikatu klucza publicznego serwera zaplecza przed przekazaniem żądania do zaplecza. Kompleksową usługę SSL można włączyć, konfigurując dla ustawienia protokołu BackendHTTPSetting wartość HTTPS, co jest następnie stosowane do puli zaplecza. Każdy serwer zaplecza w puli zaplecza z włączoną kompleksową usługą SSL należy skonfigurować przy użyciu certyfikatu, aby umożliwić bezpieczną komunikację.

![Scenariusz kompleksowej usługi SSL][1]

W tym przykładzie żądania używające protokołu TLS 1.2 są kierowane do serwerów zaplecza w puli Pula1 za pomocą kompleksowej usługi SSL.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Kompleksowa usługa SSL i lista dozwolonych certyfikatów

Usługa Application Gateway komunikuje się tylko ze znanymi wystąpieniami zaplecza, których certyfikaty znajdują się na liście dozwolonych certyfikatów tej usługi. Aby włączyć listę dozwolonych certyfikatów, należy przekazać klucz publiczny certyfikatów serwera zaplecza do usługi Application Gateway (nie certyfikat główny). W takim przypadku możliwe będą tylko połączenia do znanych zapleczy, które znajdują się na liście dozwolonych. Połączenia do pozostałych zapleczy zakończą się błędem bramy. Certyfikaty z podpisem własnym są przeznaczone tylko do celów testowych i nie są zalecane dla obciążeń w środowisku produkcyjnym. Takie certyfikaty także muszą zostać umieszczone na liście dozwolonych usługi Application Gateway, jak opisano w poprzednich krokach, zanim będzie można ich użyć.

## <a name="application-gateway-ssl-policy"></a>Zasady SSL usługi Application Gateway

Usługa Application Gateway obsługuje zasady negocjacji protokołu SSL konfigurowane przez użytkownika, które umożliwiają większą kontrolę klienta nad połączeniami SSL na bramie aplikacji.

1. Protokoły SSL 2.0 i 3.0 są domyślnie wyłączane dla wszystkich bram aplikacji. Tych zasad nie można w ogóle konfigurować.
2. Definicja zasad SSL umożliwia wyłączenie każdego z następujących trzech protokołów: **TLSv1\_0**, **TLSv1\_1** i **TLSv1\_2**.
3. Jeśli żadna zasada SSL nie zostanie zdefiniowana, wszystkie trzy protokoły (TLS 1\_0, TLS 1\_1, TLS 1_2) będą włączone.

## <a name="next-steps"></a>Następne kroki

Po zapoznaniu się z kompleksową usługą SSL i zasadami SSL zapoznaj się z informacjami dotyczącymi [włączania kompleksowej usługi SSL w bramie aplikacji](application-gateway-end-to-end-ssl-powershell.md), aby utworzyć bramę aplikacji korzystającą z kompleksowej usługi SSL.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png

