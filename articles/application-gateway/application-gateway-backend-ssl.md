---
title: Włączanie zasad SSL i kompleksowej usługi SSL w usłudze Application Gateway | Microsoft Docs
description: Ta strona zawiera omówienie kompleksowej obsługi protokołu SSL w usłudze Application Gateway.
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva

ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2016
ms.author: amsriva

---
# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Włączanie zasad SSL i kompleksowej usługi SSL w usłudze Application Gateway
## <a name="overview"></a>Omówienie
Usługa Application Gateway obsługuje przerywanie połączenia SSL na bramie, po którym ruch na ogół płynie niezaszyfrowany do serwerów zaplecza. Umożliwia to odciążenie serwerów sieci Web z nadmiaru kosztownych operacji szyfrowania i odszyfrowywania. Jednak dla niektórych klientów nieszyfrowana komunikacja z serwerami zaplecza jest opcją niemożliwą do zaakceptowania. Może to być spowodowane wymaganiami dotyczącymi zabezpieczeń lub zgodności lub aplikacja może akceptować jedynie bezpieczne połączenia. Na potrzeby takich aplikacji usługa Application Gateway obsługuje teraz kompleksowe szyfrowanie SSL.

Kompleksowa usługa SSL pozwala na bezpieczne przesyłanie zaszyfrowanych danych poufnych na zaplecze, umożliwiając jednocześnie korzystanie z funkcji równoważenia obciążenia warstwy 7, które oferuje usługa Application Gateway, takich jak koligacja plików cookie, routing oparty na adresach URL, obsługa routingu opartego na witrynach lub możliwość iniekcji nagłówków X-Forwarded-*.

Po skonfigurowaniu kompleksowego trybu komunikacji SSL usługa Application Gateway kończy sesje SSL użytkownika na bramie i odszyfrowuje ruch użytkownika. Następnie stosuje skonfigurowane reguły, aby wybrać odpowiednie wystąpienie puli serwerów zaplecza w celu skierowania do nich ruchu. Następnie usługa Application Gateway inicjuje nowe połączenie SSL z serwerem zaplecza i ponownie szyfruje dane przy użyciu certyfikatu klucza publicznego serwera zaplecza przed przekazaniem żądania do zaplecza. Kompleksową usługę SSL można włączyć, konfigurując dla ustawienia protokołu BackendHTTPSetting wartość HTTP, co jest następnie stosowane do puli zaplecza. Każdy serwer zaplecza w puli zaplecza z włączoną kompleksową usługą SSL należy skonfigurować przy użyciu certyfikatu, aby umożliwić bezpieczną komunikację.

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

W tym przykładzie żądania dla adresu https://contoso.com mogą być kierowane do puli ContosoServerPool za pośrednictwem protokołu HTTP, a dla adresu https://fabrikam.com będą kierowane do puli FabrikamServerPool za pośrednictwem protokołu HTTPS przy użyciu kompleksowej usługi SSL.

## <a name="end-to-end-ssl-and-white-listing-of-certificates"></a>Kompleksowa usługa SSL i lista dozwolonych certyfikatów
Usługa Application Gateway komunikuje się tylko ze znanymi wystąpieniami zaplecza, których certyfikaty znajdują się na liście dozwolonych certyfikatów tej usługi. Aby włączyć listę dozwolonych certyfikatów, należy przekazać klucz publiczny certyfikatów serwera zaplecza do usługi Application Gateway. W takim przypadku możliwe będą tylko połączenia do znanych zapleczy, które znajdują się na liście dozwolonych, a pozostałe zakończą się błędem bramy. Certyfikaty z podpisem własnym są przeznaczone tylko do celów testowych i nie są zalecane dla obciążeń w środowisku produkcyjnym. Takie certyfikaty także muszą zostać umieszczone na liście dozwolonych usługi Application Gateway, jak opisano powyżej, zanim będzie można ich użyć.

## <a name="application-gateway-ssl-policy"></a>Zasady SSL usługi Application Gateway
Usługa Application Gateway obsługuje również zasady negocjacji protokołu SSL konfigurowane przez użytkownika, które umożliwiają bardziej szczegółową kontrolę klienta nad połączeniami SSL na bramie aplikacji.

1. Protokoły SSL 2.0 i 3.0 są przymusowo wyłączane dla wszystkich bram aplikacji. Nie są one w ogóle konfigurowane.
2. Definicja zasad SSL udostępnia opcję wyłączenia każdego z następujących trzech protokołów: TLS 1_0, TLS 1_1, TLS 1_2.
3. Jeśli żadna zasada SSL nie zostanie zdefiniowana, wszystkie trzy protokoły (TLS 1_0, TLS 1_1, TLS 1_2) będą włączone.

## <a name="next-steps"></a>Następne kroki
Po zapoznaniu się z kompleksową usługą SSL i zasadami SSL zapoznaj się z informacjami dotyczącymi [włączania kompleksowej usługi SSL na bramie aplikacji](application-gateway-end-to-end-ssl-powershell.md), aby utworzyć bramę aplikacji z funkcją wysyłania ruchu do serwera zaplecza w postaci zaszyfrowanej.

<!--HONumber=Oct16_HO3-->


