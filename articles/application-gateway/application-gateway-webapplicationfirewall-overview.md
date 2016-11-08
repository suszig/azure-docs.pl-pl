---
title: Zapora aplikacji sieci Web w usłudze Application Gateway | Microsoft Docs
description: Ta strona zawiera omówienie funkcji zapory aplikacji sieci Web w usłudze Application Gateway.
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
# <a name="application-gateway-web-application-firewall-(preview)"></a>Zapora aplikacji sieci Web w usłudze Application Gateway (wersja zapoznawcza)
Aplikacje sieci Web coraz częściej stają się obiektami złośliwych ataków wykorzystujących znane luki w zabezpieczeniach. Wśród nich często zdarzają się np. ataki polegające na iniekcji SQL i ataki z użyciem skryptów wykorzystywanych w wielu witrynach.
Zapobieganie takim atakom z poziomu kodu aplikacji może być trudne. Może też wymagać rygorystycznego przestrzegania harmonogramu konserwacji, poprawek i monitorowania na poziomie wielu warstw topologii aplikacji. Scentralizowana ochrona przed atakami w sieci Web sprawia, że zarządzanie zabezpieczeniami jest znacznie prostsze oraz zapewnia lepszą ochronę aplikacji przed intruzami. Zapora aplikacji sieci Web może reagować na zagrożenia bezpieczeństwa szybciej — poprzez wdrażanie poprawek zapobiegających wykorzystaniu znanych luk w zabezpieczeniach w centralnej lokalizacji zamiast w poszczególnych aplikacjach sieci Web.

![imageURLroute](./media/application-gateway-webapplicationfirewall-overview/WAF1.png)

Usługa Application Gateway działa jak kontroler dostarczania aplikacji i oferuje możliwość kończenia żądań SSL, ustawiania koligacji sesji na podstawie pliku cookie, dystrybucji obciążenia z działaniem okrężnym, routingu opartego na zawartości, a także możliwość hostowania wielu witryn sieci Web i stosowania rozszerzonych zabezpieczeń. Rozszerzenia zabezpieczeń oferowane przez usługę Application Gateway obejmują zarządzanie zasadami protokołu SSL i pełne wsparcie w zakresie protokołu SSL. Firma Microsoft wzmacnia zabezpieczenia oferowanych przez siebie usług poprzez wprowadzenie zapory aplikacji sieci Web bezpośrednio zintegrowanej z elementami oferty usług ADC. Dzięki temu możliwa jest łatwa konfiguracja w centralnej lokalizacji. Dodatkowo wprowadzenie tej nowej funkcji sprawia, że zarządzanie aplikacjami sieci Web i ich ochrona przed znanymi lukami są niezwykle proste.

Konfiguracja zapory aplikacji sieci Web w obrębie bramy aplikacji zapewnia następujące korzyści:

* Ochrona aplikacji sieci Web przed atakami wykorzystującymi luki w zabezpieczeniach oraz atakami niewymagającymi modyfikacji kodu zaplecza.
* Jednoczesna ochrona wielu aplikacji sieci Web za bramą aplikacji. Pojedyncza brama aplikacji umożliwia hosting maksymalnie 20 witryn sieci Web. Wszystkie witryny za bramą są chronione przed atakami z sieci Web.
* Monitorowanie aplikacji sieci Web pod kątem ataków przy użyciu raportu w czasie rzeczywistym generowanego przez dzienniki zapory aplikacji sieci Web bramy aplikacji.
* Niektóre środki kontrolne mające na celu zapewnienie zgodności wymagają, aby wszystkie punkty końcowe umożliwiające dostęp do Internetu były chronione z użyciem zapory aplikacji sieci Web. Korzystając z bramy aplikacji z zaporą aplikacji sieci Web, można spełnić te wymagania zgodności.

## <a name="overview"></a>Omówienie
Zapora aplikacji sieci Web w usłudze Application Gateway jest oferowana w ramach nowej jednostki SKU (WAF SKU) i ma wstępnie skonfigurowaną funkcję ModSecurity oraz zestaw reguł podstawowych OWASP. Dzięki temu zapewnia podstawową ochronę przed większością ataków związanych z 10 najczęściej wykorzystywanymi lukami w zabezpieczeniach wskazanymi przez społeczność OWASP.

* Ochrona przed atakami polegającymi na iniekcji SQL
* Ochrona przed atakami z użyciem skryptów wykorzystywanych w obrębie wielu witryn
* Częste ataki w ramach sieci Web polegające na iniekcji poleceń, przemycaniu żądań HTTP, rozdzielaniu odpowiedzi HTTP i zdalnym dołączaniu plików
* Ochrona przed naruszeniami protokołu HTTP
* Ochrona przed nieprawidłowościami protokołu HTTP, takimi jak brakujące powiązania agenta i użytkownika hosta oraz akceptowanie nagłówków
* Ochrona przed atakami typu „odmowa usługi” w ramach protokołu HTTP, w tym atakami polegającymi na przesyłaniu dużej liczby pakietów, oraz zapobieganie powolnym atakom typu „odmowa usługi”
* Zapobieganie atakom z użyciem robotów, przeszukiwarek i skanerów
* Wykrywanie typowych błędów konfiguracji aplikacji (np. Apache, usługi IIS itp.)

## <a name="waf-modes"></a>Tryby zapory aplikacji sieci Web
Zapora aplikacji sieci Web bramy aplikacji może zostać skonfigurowana pod kątem jej uruchamiania w następujących dwóch trybach:

* **Tryb wykrywania** — kiedy zapora jest skonfigurowana do działania w trybie wykrywania, monitoruje i zapisuje w dziennikach wszelkie ostrzeżenia o zagrożeniach. Należy upewnić się, że rejestrowanie danych diagnostycznych usługi Application Gateway jest włączone w obszarze Diagnostyka. Należy również upewnić się, że opcja dziennika zapory aplikacji sieci Web jest zaznaczona i włączona.
* **Tryb zapobiegania** — kiedy zapora jest skonfigurowana do działania w tym trybie, usługa Application Gateway aktywnie blokuje próby włamań i ataki wykryte na podstawie reguł. Osoba atakująca odbiera wyjątek 403 odnoszący się do nieautoryzowanego dostępu, a jej połączenie zostaje zakończone. W trybie zapobiegania tego rodzaju ataki są rejestrowane w dziennikach zapory aplikacji sieci Web w sposób ciągły.

## <a name="application-gateway-waf-reports"></a>Raporty zapory aplikacji sieci Web w usłudze Application Gateway
Zapora aplikacji sieci Web w usłudze Application Gateway dostarcza szczegółowe raporty w zakresie każdego wykrytego zagrożenia. Rejestrowanie jest zintegrowane z Dziennikami diagnostycznymi platformy Azure, a alerty są zapisywane w formacie json.

![imageURLroute](./media/application-gateway-webapplicationfirewall-overview/waf2.png)

    {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
        "operationName": "ApplicationGatewayFirewall",
        "time": "2016-09-20T00:40:04.9138513Z",
        "category": "ApplicationGatewayFirewallLog",
        "properties":     {
            "instanceId":"ApplicationGatewayRole_IN_0",
            "clientIp":"108.41.16.164",
            "clientPort":1815,
            "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
            "ruleId":"OWASP_973336",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "action":"Logged",
            "site":"Global",
            "message":"XSS Filter - Category 1: Script Tag Vector",
            "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
    }

## <a name="application-gateway-waf-sku-pricing"></a>Cena jednostki SKU zapory aplikacji sieci Web w usłudze Application Gateway
W okresie korzystania z wersji zapoznawczej nie są naliczane żadne dodatkowe opłaty za korzystanie z zapory aplikacji sieci Web w usłudze Application Gateway. Opłaty są w dalszym ciągu naliczane w oparciu o istniejącą podstawową jednostkę SKU. Firma Microsoft przekaże informacje o wysokości opłat za jednostkę SKU zapory aplikacji sieci Web z chwilą jej przekazania do ogólnego użytku. Klienci, którzy zdecydują się wdrożyć usługę Application Gateway z zastosowaniem zapory aplikacji sieci Web zaczną płacić za jednostkę SKU zapory dopiero po przekazaniu jednostki do ogólnego użytku.

## <a name="next-steps"></a>Następne kroki
Więcej informacji na temat możliwości oferowanych przez zaporę aplikacji sieci Web można znaleźć w artykule [How to configure Web Application Firewall on Application Gateway](application-gateway-web-application-firewall-portal.md) (Jak skonfigurować zaporę aplikacji sieci Web w usłudze Application Gateway).

<!--HONumber=Oct16_HO3-->


