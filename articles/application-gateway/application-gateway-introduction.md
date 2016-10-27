<properties
   pageTitle="Wprowadzenie do usługi Application Gateway | Microsoft Azure"
   description="Ta strona zawiera omówienie usługi Application Gateway dla równoważenia obciążenia warstwy 7, z uwzględnieniem rozmiarów bramy, równoważenia obciążenia HTTP, koligacji sesji na podstawie plików cookie i odciążania protokołu SSL."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="gwallace"/>


# <a name="application-gateway-overview"></a>Application Gateway — omówienie

## <a name="what-is-application-gateway"></a>Co to jest usługa Application Gateway

Usługa Microsoft Azure Application Gateway oferuje kontrolera dostarczania aplikacji (ADC, Application Delivery Controller) jako usługę, zapewniając wiele możliwości równoważenia obciążenia warstwy 7. Mówiąc prościej, działanie tej usługi polega na akceptowaniu ruchu i kierowaniu go do odpowiednich wystąpień zaplecza na podstawie zdefiniowanych za jej pomocą reguł.

Równoważenie obciążenia aplikacji umożliwia administratorom IT i deweloperom tworzenie reguł routingu dla ruchu sieciowego na podstawie protokołu HTTP.  Usługa Application Gateway jest wysoko dostępna i mierzalna. Umowę SLA oraz ceny można znaleźć na stronach [Umowa SLA](https://azure.microsoft.com/support/legal/sla/) i [Cennik](https://azure.microsoft.com/pricing/details/application-gateway/).

Usługa Application Gateway stosuje reguły routingu do ruchu HTTP, zapewniając równoważenie obciążenia warstwy 7 (HTTP). Utworzenie bramy aplikacji powoduje powiązanie punktu końcowego (adresu VIP) i używanie go jako publicznego adresu IP dla ruchu sieciowego danych przychodzących. Platforma Azure zapewnia równoważenie obciążenia warstwy 4 za pośrednictwem usługi Azure Load Balancer, która działa na poziomie transportu (TCP/UDP) i równoważy obciążenie całego przychodzącego ruchu sieciowego do usługi Application Gateway. Usługa Application Gateway kieruje ruch HTTP na podstawie konfiguracji, niezależnie od tego, czy jest to maszyna wirtualna, usługa w chmurze czy zewnętrzny adres IP.

## <a name="features"></a>Funkcje

Usługa Application Gateway obecnie obsługuje dostarczanie aplikacji warstwy 7 z następującymi funkcjami:

- **[Zapora aplikacji sieci Web (wersja zapoznawcza)](application-gateway-webapplicationfirewall-overview.md)** — zapora aplikacji sieci Web (WAF, web application firewall) w usłudze Azure Application Gateway chroni aplikacje sieci Web przed typowymi atakami internetowymi, takimi jak iniekcja SQL, ataki z użyciem skryptów wykorzystywanych w obrębie wielu witryn i przejęcia sesji.
- **Równoważenie obciążenia HTTP** — usługa Application Gateway zapewnia okrężne równoważenie obciążenia. Równoważenie obciążenia jest wykonywane na warstwie 7 i jest używane tylko na potrzeby ruchu HTTP(S).
- **Koligacja sesji na podstawie plików cookie** — ta funkcja jest przydatna, gdy chcesz zachować sesję użytkownika na tym samym serwerze zaplecza. Używając plików cookie zarządzanych przez bramę, usługa Application Gateway może kierować kolejny ruch z sesji użytkownika na ten sam serwer zaplecza w celu przetworzenia. Ta funkcja jest ważna w przypadkach, w których stan sesji jest zapisywany lokalnie na serwerze zaplecza dla sesji użytkownika.
- **[Odciążenie protokołu Secure Sockets Layer (SSL)](application-gateway-ssl-arm.md)** — ta funkcja zdejmuje z serwerów sieci Web kosztowne zadanie odszyfrowywania ruchu HTTPS. Dzięki zakończeniu połączenia SSL na usłudze Application Gateway i przesłaniu na serwer niezaszyfrowanego żądania, z serwera sieci Web jest zdejmowane zadanie odszyfrowywania.  Usługa Application Gateway ponownie szyfruje odpowiedź przed odesłaniem jej z powrotem do klienta. Ta funkcja jest przydatna w przypadkach, kiedy serwer zaplecza znajduje się w tej samej zabezpieczonej sieci wirtualnej, co usługa Application Gateway na platformie Azure.
- **[Kompleksowa usługa SSL](application-gateway-backend-ssl.md)** — usługa Application Gateway obsługuje kompleksowe szyfrowanie ruchu. Polega to na tym, że usługa Application Gateway przerywa połączenie SSL na bramie aplikacji. Następnie brama stosuje do ruchu reguły routingu, ponownie szyfruje pakiet i przekazuje pakiet do odpowiedniego serwera zaplecza na podstawie zdefiniowanych reguł routingu. Każda odpowiedź z serwera sieci Web przechodzi przez ten sam proces z powrotem do użytkownika końcowego.
- **[Routing zawartości oparty na adresach URL](application-gateway-url-route-overview.md)** — ta funkcja zapewnia możliwość używania różnych serwerów zaplecza dla różnego ruchu. Ruch dla folderu na serwerze sieci Web lub dla usługi CDN może być kierowany na inne zaplecze, zmniejszając niepotrzebne obciążenie na serwerach zaplecza, które nie obsługują określonej zawartości.
- **[Routing obejmujący wiele witryn](application-gateway-multi-site-overview.md)** — usługa Application Gateway umożliwia konsolidację maksymalnie 20 witryn sieci Web na jednej bramie aplikacji.
- **[Obsługa protokołu Websocket](application-gateway-websocket.md)** — kolejną atrakcyjną funkcją usługi Application Gateway jest natywna obsługa protokołu Websocket.
- **[Monitorowanie kondycji](application-gateway-probe-overview.md)** — usługa Application Gateway zapewnia domyślne monitorowanie kondycji zasobów zaplecza i niestandardowe sondy na potrzeby monitorowania bardziej specyficznych scenariuszy.

## <a name="benefits"></a>Korzyści

Usługa Application Gateway ma następujące zastosowania:

- Aplikacje, które wymagają żądań z tej samej sesji klienta/użytkownika, aby dotrzeć do tej samej maszyny wirtualnej zaplecza. Przykładami takich aplikacji są aplikacje koszyka zakupów i serwery poczty sieci Web.
- Aplikacje, które chcą uwolnić farmy serwerów sieci Web od nadmiaru przerywanych połączeń SSL.
- Aplikacje, takie jak sieć dostarczania zawartości, które wymagają, aby wiele żądań HTTP w ramach tego samego długotrwałego połączenia TCP było kierowanych do różnych serwerów zaplecza lub obsługiwanych w sposób równoważący obciążenie na tych serwerach.
- Aplikacje, które obsługują ruch w ramach protokołu Websocket.
- Ochrona aplikacji sieci Web przed typowymi atakami internetowymi, takimi jak iniekcja SQL, ataki z użyciem skryptów wykorzystywanych w obrębie wielu witryn i przejęcia sesji.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="gateway-sizes-and-instances"></a>Wystąpienia i rozmiary usługi Application Gateway

Usługa Application Gateway jest obecnie oferowana w trzech rozmiarach: małym (Small), średnim (Medium) i dużym (Large). Rozmiary małych wystąpień są przeznaczone na potrzeby programowania i scenariuszy testowania.

Obecnie istnieją dwie jednostki SKU dla usługi Application Gateway: WAF i Standard.

Można utworzyć maksymalnie 50 bram aplikacji na subskrypcję, a każda brama aplikacji może mieć maksymalnie 10 wystąpień. Każda brama aplikacji może składać się z 20 odbiorników HTTP. Ze względu na to, że równoważenie obciążenia usługi Application Gateway jest usługą zarządzaną przez platformę Azure, możliwa jest aprowizacja modułu równoważenia obciążenia warstwy 7 za modułem równoważenia obciążenia oprogramowania Azure.

W poniższej tabeli przedstawiono przepływność przy średniej wydajności dla każdego wystąpienia bramy aplikacji:

| Odpowiedź strony zaplecza | Small | Medium | Large|
|---|---|---|---|
| 6000 | 7,5 Mb/s | 13 Mb/s | 50 Mb/s |
|100 000 | 35 Mb/s | 100 Mb/s| 200 Mb/s |

>[AZURE.NOTE] Są to przybliżone wartości przepływności bramy aplikacji. Rzeczywista przepływność zależy od różnorodnych szczegółów środowiska, takich jak średni rozmiar strony, lokalizacja wystąpień zaplecza i czas przetwarzania potrzebny do obsługi strony. Aby uzyskać dokładne wartości wydajności, należy przeprowadzić własne testy. Te wartości są podane tylko jako wskazówki na potrzeby planowania pojemności.

## <a name="health-monitoring"></a>Monitorowanie kondycji

Usługa Azure Application Gateway automatycznie monitoruje kondycję wystąpień zaplecza za pośrednictwem podstawowych lub niestandardowych sond kondycji. Użycie sond kondycji zapewnia, że tylko hosty o dobrej kondycji będą odpowiadać na ruch sieciowy. Aby uzyskać więcej informacji, zobacz [Application Gateway health monitoring overview](application-gateway-probe-overview.md) (Monitorowanie kondycji usługi Application Gateway — omówienie).

## <a name="configuring-and-managing"></a>Konfigurowanie i zarządzanie

Punktem końcowym bramy aplikacji, jeśli został skonfigurowany, może być publiczny adres IP, prywatny adres IP lub obydwa te adresy. Usługa Application Gateway jest konfigurowana wewnątrz sieci wirtualnej we własnej podsieci. Podsieć utworzona lub używana na potrzeby bramy aplikacji nie może zawierać żadnych innych typów zasobów. Jedyne zasoby dozwolone w podsieci to inne bramy aplikacji. Aby zabezpieczyć zasoby zaplecza można umieścić serwery zaplecza w obrębie innej podsieci w tej samej sieci wirtualnej, co brama aplikacji. Ta dodatkowa podsieć nie jest wymagana dla aplikacji zaplecza. O ile tylko brama aplikacji może osiągnąć adres IP, to usługa Application Gateway jest w stanie zapewnić funkcje usługi ADC dla serwerów zaplecza.

Możesz utworzyć bramę aplikacji i zarządzać nią, używając interfejsów API REST, poleceń cmdlet programu PowerShell, interfejsu wiersza polecenia Azure lub witryny [Azure Portal](https://portal.azure.com/).

## <a name="next-steps"></a>Następne kroki

Po zapoznaniu się z informacjami na temat usługi Application Gateway możesz [utworzyć bramę aplikacji](application-gateway-create-gateway-portal.md) albo [utworzyć odciążenie protokołu SSL bramy aplikacji](application-gateway-ssl-arm.md), aby zrównoważyć obciążenie połączeń HTTPS.

Aby dowiedzieć się, jak utworzyć bramę aplikacji przy użyciu routingu zawartości opartego na adresach URL, zobacz [Create an application gateway using URL-based routing](application-gateway-create-url-route-arm-ps.md) (Tworzenie bramy aplikacji przy użyciu routingu opartego na adresach URL).




<!--HONumber=Oct16_HO3-->


