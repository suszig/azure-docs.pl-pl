---
title: "Wprowadzenie do usługi Azure Application Gateway | Microsoft Docs"
description: "Ta strona zawiera omówienie usługi Application Gateway dla równoważenia obciążenia warstwy 7, z uwzględnieniem rozmiarów bramy, równoważenia obciążenia HTTP, koligacji sesji na podstawie plików cookie i odciążania protokołu SSL."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: b37a2473-4f0e-496b-95e7-c0594e96f83e
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: d23b400d8e6db66bc596731770a98e2833302543
ms.lasthandoff: 04/04/2017


---
# <a name="overview-of-application-gateway"></a>Omówienie usługi Application Gateway

Usługa Microsoft Azure Application Gateway to dedykowane urządzenie wirtualne oferujące kontroler dostarczania aplikacji (ADC, Application Delivery Controller) jako usługę, zapewniając różnorodne możliwości równoważenia obciążenia warstwy 7 dla Twojej aplikacji. Obejmuje ona wiele wystąpień procesu roboczego na potrzeby skalowalności i wysokiej dostępności. Umożliwia to klientom optymalizowanie wydajności farmy sieci Web dzięki przeniesieniu obciążenia intensywnego przerywania połączenia SSL z procesora CPU do usługi Application Gateway. Zapewnia także inne możliwości routingu warstwy 7, takie jak okrężna dystrybucja ruchu przychodzącego, koligacja sesji na podstawie plików cookie, routing oparty na ścieżkach URL i możliwość hostowania wielu witryn sieci Web za pojedynczą bramą Application Gateway. Usługę Application Gateway można skonfigurować jako bramę umożliwiającą dostęp do Internetu, bramę tylko wewnętrzną lub jako kombinację obu tych opcji. Usługa Application Gateway jest w pełni zarządzana przez platformę Azure, skalowalna i wysoko dostępna. Zapewnia ona bogaty zestaw funkcji diagnostyki i rejestrowania, aby uprościć zarządzanie. Utworzenie bramy aplikacji powoduje powiązanie punktu końcowego (publicznego adresu VIP lub wewnętrznego adresu IP modułu równoważenia obciążenia) i używanie go na potrzeby ruchu sieciowego danych przychodzących. Ten adres VIP lub adres IP wewnętrznego modułu równoważenia obciążenia jest dostarczany za pośrednictwem usługi Azure Load Balancer, która działa na poziomie transportu (TCP/UDP) i równoważy obciążenie całego przychodzącego ruchu sieciowego do wystąpień procesu roboczego usługi Application Gateway. Następnie usługa Application Gateway kieruje ruch HTTP/HTTPS na podstawie konfiguracji, niezależnie od tego, czy jest to maszyna wirtualna, usługa w chmurze czy wewnętrzny lub zewnętrzny adres IP.

Jest również udostępniana zapora aplikacji sieci Web (WAF) w ramach jednostki SKU zapory aplikacji sieci Web usługi Application Gateway, która chroni aplikacje sieci Web przed typowymi lukami w zabezpieczeniach sieci Web. Zapora aplikacji sieci Web zapewnia ochronę na podstawie reguł z [podstawowych zestawów reguł OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) w wersji 3.0 lub 2.2.9.

## <a name="differences-between-application-gateway-skus"></a>Różnice między jednostkami SKU usługi Application Gateway

Usługa Application Gateway jest oferowana w formie dwóch jednostek SKU: standardowej jednostki SKU i jednostki SKU zapory aplikacji sieci Web.

### <a name="standard"></a>Standardowa (Standard)

Standardowa jednostka SKU oferuje możliwość kończenia żądań SSL, ustawiania koligacji sesji na podstawie pliku cookie, dystrybucji obciążenia z działaniem okrężnym, routingu opartego na zawartości, a także możliwość hostowania wielu witryn sieci Web i stosowania rozszerzonych zabezpieczeń. Rozszerzenia zabezpieczeń oferowane przez usługę Application Gateway obejmują zarządzanie zasadami protokołu SSL, pełną obsługę w zakresie protokołu SSL i kończenie żądań SSL.

### <a name="web-application-firewall-waf"></a>Zapora aplikacji sieci Web

Jednostka SKU zapory aplikacji sieci Web oferuje wszystkie funkcje standardowej jednostki SKU oraz [zaporę aplikacji sieci Web](application-gateway-web-application-firewall-overview.md). Dzięki temu można korzystać z reguł wykrywania ataków zapewniających ochronę aplikacji sieci Web przed typowymi lukami w zabezpieczeniach sieci Web.

## <a name="features"></a>Funkcje

Usługa Application Gateway obecnie obsługuje dostarczanie aplikacji warstwy 7 z następującymi funkcjami:

* **[Zapora aplikacji sieci Web](application-gateway-webapplicationfirewall-overview.md)** — zapora aplikacji sieci Web (WAF, web application firewall) w usłudze Azure Application Gateway chroni aplikacje sieci Web przed typowymi atakami w sieci Web, takimi jak iniekcja SQL, ataki z użyciem skryptów w obrębie wielu witryn i przejęcia sesji.
* **Równoważenie obciążenia HTTP** — usługa Application Gateway zapewnia okrężne równoważenie obciążenia. Równoważenie obciążenia jest wykonywane na warstwie 7 i jest używane tylko na potrzeby ruchu HTTP(S).
* **Koligacja sesji na podstawie plików cookie** — ta funkcja jest przydatna, gdy chcesz zachować sesję użytkownika na tym samym serwerze zaplecza. Używając plików cookie zarządzanych przez bramę, usługa Application Gateway może kierować kolejny ruch z sesji użytkownika na ten sam serwer zaplecza w celu przetworzenia. Ta funkcja jest ważna w przypadkach, w których stan sesji jest zapisywany lokalnie na serwerze zaplecza dla sesji użytkownika.
* **[Odciążenie protokołu Secure Sockets Layer (SSL)](application-gateway-ssl-arm.md)** — ta funkcja zdejmuje z serwerów sieci Web kosztowne zadanie odszyfrowywania ruchu HTTPS. Dzięki zakończeniu połączenia SSL w usłudze Application Gateway i przesłaniu na serwer niezaszyfrowanego żądania z serwera sieci Web jest zdejmowane zadanie odszyfrowywania.  Usługa Application Gateway ponownie szyfruje odpowiedź przed odesłaniem jej z powrotem do klienta. Ta funkcja jest przydatna w przypadkach, kiedy serwer zaplecza znajduje się w tej samej zabezpieczonej sieci wirtualnej, co usługa Application Gateway na platformie Azure.
* **[Kompleksowa usługa SSL](application-gateway-backend-ssl.md)** — usługa Application Gateway obsługuje kompleksowe szyfrowanie ruchu. Polega to na tym, że usługa Application Gateway przerywa połączenie SSL na bramie aplikacji. Następnie brama stosuje do ruchu reguły routingu, ponownie szyfruje pakiet i przekazuje pakiet do odpowiedniego serwera zaplecza na podstawie zdefiniowanych reguł routingu. Każda odpowiedź z serwera sieci Web przechodzi przez ten sam proces z powrotem do użytkownika końcowego.
* **[Routing zawartości oparty na adresach URL](application-gateway-url-route-overview.md)** — ta funkcja zapewnia możliwość używania różnych serwerów zaplecza dla różnego ruchu. Ruch dla folderu na serwerze sieci Web lub dla usługi CDN może być kierowany na inne zaplecze, zmniejszając niepotrzebne obciążenie na serwerach zaplecza, które nie obsługują określonej zawartości.
* **[Routing obejmujący wiele witryn](application-gateway-multi-site-overview.md)** — usługa Application Gateway umożliwia konsolidację maksymalnie 20 witryn sieci Web na jednej bramie aplikacji.
* **[Obsługa protokołu Websocket](application-gateway-websocket.md)** — kolejną atrakcyjną funkcją usługi Application Gateway jest natywna obsługa protokołu Websocket.
* **[Monitorowanie kondycji](application-gateway-probe-overview.md)** — usługa Application Gateway zapewnia domyślne monitorowanie kondycji zasobów zaplecza i niestandardowe sondy na potrzeby monitorowania bardziej specyficznych scenariuszy.
* **[Zaawansowana diagnostyka](application-gateway-diagnostics.md)** — aplikacja Application Gateway oferuje kompletne dzienniki dostępu i diagnostyki. Dzienniki zapory są dostępne dla zasobów usługi Application Gateway z włączoną zaporą aplikacji sieci Web.

## <a name="benefits"></a>Korzyści

Usługa Application Gateway ma następujące zastosowania:

* Aplikacje, które wymagają żądań z tej samej sesji klienta/użytkownika, aby dotrzeć do tej samej maszyny wirtualnej zaplecza. Przykładami takich aplikacji są aplikacje koszyka zakupów i serwery poczty sieci Web.
* Eliminowanie nakładu pracy związanego z kończeniem żądań SSL dla farm serwerów sieci Web.
* Aplikacje, takie jak sieć dostarczania zawartości, które wymagają, aby wiele żądań HTTP w ramach tego samego długotrwałego połączenia TCP było kierowanych do różnych serwerów zaplecza lub obsługiwanych w sposób równoważący obciążenie na tych serwerach.
* Aplikacje, które obsługują ruch w ramach protokołu Websocket.
* Ochrona aplikacji sieci Web przed typowymi atakami internetowymi, takimi jak iniekcja SQL, ataki z użyciem skryptów wykorzystywanych w obrębie wielu witryn i przejęcia sesji.

Ze względu na to, że równoważenie obciążenia usługi Application Gateway jest usługą zarządzaną przez platformę Azure, możliwa jest aprowizacja modułu równoważenia obciążenia warstwy 7 za modułem równoważenia obciążenia oprogramowania Azure. Przy użyciu usługi Traffic Manager można ukończyć wykonywanie scenariusza, jak pokazano to na poniższym obrazie. W tym przypadku usługa Traffic Manager umożliwia przekierowywanie oraz udostępnianie ruchu wielu zasobom bramy aplikacji w różnych regionach, podczas gdy brama aplikacji zapewnia równoważenie obciążenia warstwy 7 między regionami. Przykład tego scenariusza można znaleźć w temacie [Using load balancing services in the Azure cloud](../traffic-manager/traffic-manager-load-balancing-azure.md) (Używanie usług równoważenia obciążenia w chmurze platformy Azure).

![scenariusz dla usług traffic manager i application gateway](./media/application-gateway-introduction/tm-lb-ag-scenario.png)

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="gateway-sizes-and-instances"></a>Wystąpienia i rozmiary usługi Application Gateway

Usługa Application Gateway jest obecnie oferowana w trzech rozmiarach: małym (**Small**), średnim (**Medium**) i dużym (**Large**). Rozmiary małych wystąpień są przeznaczone na potrzeby programowania i scenariuszy testowania.

Można utworzyć maksymalnie 50 bram aplikacji na subskrypcję, a każda brama aplikacji może mieć maksymalnie 10 wystąpień. Każda brama aplikacji może składać się z 20 odbiorników HTTP. Pełna lista limitów usługi Application Gateway znajduje się na stronie [ograniczeń usługi Application Gateway](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

W poniższej tabeli przedstawiono przepływność przy średniej wydajności dla każdego wystąpienia bramy aplikacji z włączonym obciążeniem SSL:

| Odpowiedź strony zaplecza | Small | Medium | Large |
| --- | --- | --- | --- |
| 6000 |7,5 Mb/s |13 Mb/s |50 Mb/s |
| 100 000 |35 Mb/s |100 Mb/s |200 Mb/s |

> [!NOTE]
> Są to przybliżone wartości przepływności bramy aplikacji. Rzeczywista przepływność zależy od różnorodnych szczegółów środowiska, takich jak średni rozmiar strony, lokalizacja wystąpień zaplecza i czas przetwarzania potrzebny do obsługi strony. Aby uzyskać dokładne wartości wydajności, należy przeprowadzić własne testy. Te wartości są podane tylko jako wskazówki na potrzeby planowania pojemności.

## <a name="health-monitoring"></a>Monitorowanie kondycji

Usługa Azure Application Gateway automatycznie monitoruje kondycję wystąpień zaplecza za pośrednictwem podstawowych lub niestandardowych sond kondycji. Użycie sond kondycji zapewnia, że tylko hosty o dobrej kondycji będą odpowiadać na ruch sieciowy. Aby uzyskać więcej informacji, zobacz [Application Gateway health monitoring overview](application-gateway-probe-overview.md) (Monitorowanie kondycji usługi Application Gateway — omówienie).

## <a name="configuring-and-managing"></a>Konfigurowanie i zarządzanie

Punktem końcowym bramy aplikacji, jeśli został skonfigurowany, może być publiczny adres IP, prywatny adres IP lub obydwa te adresy. Usługa Application Gateway jest konfigurowana wewnątrz sieci wirtualnej we własnej podsieci. Podsieć utworzona lub używana na potrzeby bramy aplikacji nie może zawierać żadnych innych typów zasobów. Jedyne zasoby dozwolone w podsieci to inne bramy aplikacji. Aby zabezpieczyć zasoby zaplecza, można umieścić serwery zaplecza w obrębie innej podsieci w ramach tej samej sieci wirtualnej, w której znajduje się brama aplikacji. Ta dodatkowa podsieć nie jest wymagana dla aplikacji zaplecza. O ile tylko brama aplikacji może osiągnąć adres IP, to usługa Application Gateway jest w stanie zapewnić funkcje usługi ADC dla serwerów zaplecza. 

Możesz utworzyć bramę aplikacji i zarządzać nią, używając interfejsów API REST, poleceń cmdlet programu PowerShell, interfejsu wiersza polecenia Azure lub witryny [Azure Portal](https://portal.azure.com/). Jeśli masz dodatkowe pytania dotyczące usługi Application Gateway, odwiedź stronę [często zadawanych pytań dotyczących usługi Application Gateway](application-gateway-faq.md), na której jest dostępna lista typowych często zadawanych pytań.

## <a name="pricing"></a>Cennik

Ceny zależą od opłaty godzinnej za wystąpienie bramy i opłaty za przetwarzanie danych. Opłata godzinna za bramę w przypadku jednostki SKU zapory aplikacji sieci Web różni się od opłat za standardową jednostkę SKU. Ceny można znaleźć na stronie [szczegółowego cennika usługi Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/). Opłaty za przetwarzanie danych pozostają bez zmian.

## <a name="next-steps"></a>Następne kroki

Po zapoznaniu się z informacjami na temat usługi Application Gateway możesz [utworzyć bramę aplikacji](application-gateway-create-gateway-portal.md) albo [utworzyć odciążenie protokołu SSL bramy aplikacji](application-gateway-ssl-arm.md), aby zrównoważyć obciążenie połączeń HTTPS.

Aby dowiedzieć się, jak utworzyć bramę aplikacji przy użyciu routingu zawartości opartego na adresach URL, zobacz [Create an application gateway using URL-based routing](application-gateway-create-url-route-arm-ps.md) (Tworzenie bramy aplikacji przy użyciu routingu opartego na adresach URL).

