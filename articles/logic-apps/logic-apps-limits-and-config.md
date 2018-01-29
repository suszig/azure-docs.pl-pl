---
title: "Limity i konfiguracji — usługi Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Limity usług i wartości konfiguracji dla usługi Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 5e3147cbc9fce6737cfb9b2e93e8bf1662163f3c
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="logic-apps-limits-and-configuration"></a>Ograniczenia aplikacji logiki i konfiguracji

W tym temacie opisano limity bieżący oraz szczegóły dotyczące konfiguracji dla usługi Azure Logic Apps.

## <a name="limits"></a>Limity

### <a name="http-request-limits"></a>Limity żądań HTTP

Ograniczenia te dotyczą pojedyncze żądanie HTTP lub wywołanie łącznika.

#### <a name="timeout"></a>Limit czasu

| Name (Nazwa) | Limit | Uwagi | 
| ---- | ----- | ----- | 
| Limit czasu żądania | 120 sekund | [Wzorca asynchronicznego](../logic-apps/logic-apps-create-api-app.md) lub [do pętli](logic-apps-loops-and-scopes.md) można wyrównania w razie potrzeby |
|||| 

#### <a name="message-size"></a>Rozmiar komunikatu

| Name (Nazwa) | Limit | Uwagi | 
| ---- | ----- | ----- | 
| Rozmiar komunikatu | 100 MB | Niektóre łączniki i interfejsów API może nie obsługiwać 100 MB. | 
| Limit obliczania wyrażeń | 131 072 znaków | `@concat()`, `@base64()`, `string` nie może być dłuższa niż to ograniczenie. | 
|||| 

#### <a name="retry-policy"></a>Zasady ponawiania

| Name (Nazwa) | Limit | Uwagi | 
| ---- | ----- | ----- | 
| Liczba ponownych prób | 90 | Wartość domyślna to 4. Można skonfigurować [ponów parametr zasad](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Spróbuj ponownie Maksymalne opóźnienie | 1 dzień | Można skonfigurować [ponów parametr zasad](../logic-apps/logic-apps-workflow-actions-triggers.md). | 
| Min. opóźnienie ponowienia próby | 5 s | Można skonfigurować [ponów parametr zasad](../logic-apps/logic-apps-workflow-actions-triggers.md). |
|||| 

### <a name="run-duration-and-retention"></a>Czas trwania testu i przechowywania

Te limity mają zastosowanie do aplikacji logiki pojedynczego uruchomienia.

| Name (Nazwa) | Limit | 
| ---- | ----- | 
| Czas trwania testu | 90 dni | 
| Magazyn przechowywania | Godzina rozpoczęcia 90 dni od uruchomienia | 
| Interwał cyklu min | 1 sekunda </br>Dla aplikacji logiki z planu usługi App Service: 15 sekund | 
| Maksymalny interwał cyklu | 500 dni | 
||| 

Przekroczenie limitu czas trwania testu lub przechowywania magazynu z przepływu normalnego przetwarzania [skontaktuj się z nami](mailto://logicappsemail@microsoft.com) , dzięki czemu możemy pomóc z wymaganiami.

### <a name="looping-and-debatching-limits"></a>Powtarzanie i debatching limity

Te limity mają zastosowanie do aplikacji logiki pojedynczego uruchomienia.

| Name (Nazwa) | Limit | Uwagi | 
| ---- | ----- | ----- | 
| Elementy ForEach | 100,000 | Można użyć [zapytania akcji](../connectors/connectors-native-query.md) do filtrowania większych tablice zgodnie z potrzebami. | 
| Do iteracji | 5,000 | | 
| Elementy SplitOn | 100,000 | | 
| Równoległość ForEach | 50 | Wartość domyślna to 20. <p>Określony poziom równoległości w pętli ForEach ustawia `runtimeConfiguration` właściwości w `foreach` akcji. <p>Aby uruchomić sekwencyjnie pętli ForEach, ustaw `operationOptions` właściwości na wartość "Sequential" w `foreach` akcji. | 
|||| 

### <a name="throughput-limits"></a>Limity przepustowości

Te limity mają zastosowanie do aplikacji logiki pojedynczego wystąpienia.

| Name (Nazwa) | Limit | Uwagi | 
| ----- | ----- | ----- | 
| Wykonania akcji na 5 minut | 100,000 |<p>Maksymalnie 300 000 można zwiększyć limit, uruchamiając aplikację logiki `High Througput` tryb i to zrobić przez ustawienie `operationOptions` właściwości w`runtimeConfiguration` zasobu przepływu pracy, aby `OptimizedForHighThroughput`. <p>Należy pamiętać, że w trybie wysokiej przepływności jest w wersji zapoznawczej. Także obciążenia mogą być rozproszone na wielu aplikacji, zgodnie z potrzebami. | 
| Akcje równoczesnych połączeń wychodzących | ~2,500 | Można zmniejszyć liczbę jednoczesnych żądań lub skrócić czas trwania, zgodnie z potrzebami. | 
| Punkt końcowy środowiska wykonawczego: równoczesnych połączeń przychodzących |~1,000 | Można zmniejszyć liczbę jednoczesnych żądań lub skrócić czas trwania, zgodnie z potrzebami. | 
| Środowisko uruchomieniowe punkt końcowy: wywołania na 5 minut do odczytu  | 60,000 | Można rozpowszechniać obciążenia pracą wielu aplikacjom zgodnie z potrzebami. | 
| Środowisko uruchomieniowe punkt końcowy: wywołania wywołań na 5 minut| 45,000 |Można rozpowszechniać obciążenia pracą wielu aplikacjom zgodnie z potrzebami. | 
|||| 

Przekroczenie tych ograniczeń normalnego przetwarzania lub obciążenia wykonywania testów, które może przekroczyć te limity [skontaktuj się z nami](mailto://logicappsemail@microsoft.com) , dzięki czemu możemy pomóc z wymaganiami.

### <a name="logic-app-definition-limits"></a>Limity definicji aplikacji logiki

Te limity mają zastosowanie do definicji aplikacji logiki pojedynczego.

| Name (Nazwa) | Limit | Uwagi | 
| ---- | ----- | ----- | 
| Akcje dla przepływu pracy | 500 | Aby rozszerzyć ten limit, można dodać zagnieżdżone przepływy pracy, zgodnie z potrzebami. |
| Dozwolona liczba poziomów zagnieżdżenia akcji | 8 | Aby rozszerzyć ten limit, można dodać zagnieżdżone przepływy pracy, zgodnie z potrzebami. | 
| Przepływy pracy na region na subskrypcję | 1000 | | 
| Wyzwalaczy na przepływu pracy | 10 | | 
| Limit przypadków zakresu przełącznika | 25 | | 
| Liczba zmiennych dla przepływu pracy | 250 | | 
| Maksymalna liczba znaków w wyrażeniu | 8192 | | 
| Maksymalna liczba `trackedProperties` w znakach rozmiarze | 16,000 | 
| `action`/`trigger`limit nazwy | 80 | | 
| `description`limit długości | 256 | | 
| `parameters`limit | 50 | | 
| `outputs`limit | 10 | | 
|||| 

<a name="custom-connector-limits"></a>

### <a name="custom-connector-limits"></a>Limity łącznik niestandardowy

Ograniczenia te dotyczą łączników niestandardowych, które można utworzyć na podstawie interfejsów API sieci web.

| Name (Nazwa) | Limit | 
| ---- | ----- | 
| Liczba łączników niestandardowych, które można utworzyć | 1000 na subskrypcję platformy Azure | 
| Liczba żądań na minutę dla każdego połączenia utworzonego przez łącznik niestandardowy | 500 żądań dla każdego połączenia utworzonego przez łącznik |
||| 

### <a name="integration-account-limits"></a>Limity konta integracji

Te limity mają zastosowanie do artefaktów, które można dodać do konta integracji.

| Name (Nazwa) | Limit | Uwagi | 
| ---- | ----- | ----- | 
| Schemat | 8 MB | Można użyć [identyfikator URI obiektu blob](../logic-apps/logic-apps-enterprise-integration-schemas.md) przekazywania plików większych niż 2 MB. | 
| Mapy (plik XSLT) | 2 MB | | 
| Środowisko uruchomieniowe punkt końcowy: wywołania na 5 minut do odczytu | 60,000 | Można rozpowszechniać obciążenia między wieloma kontami, zgodnie z potrzebami. | 
| Środowisko uruchomieniowe punkt końcowy: wywołania wywołań na 5 minut | 45,000 | Można rozpowszechniać obciążenia między wieloma kontami, zgodnie z potrzebami. | 
| Punkt końcowy środowiska wykonawczego: śledzenie wywołań na 5 minut | 45,000 | Można rozpowszechniać obciążenia między wieloma kontami, zgodnie z potrzebami. | 
| Punkt końcowy środowiska wykonawczego: blokowanie równoczesnych wywołań | ~1,000 | Można zmniejszyć liczbę jednoczesnych żądań lub skrócić czas trwania, zgodnie z potrzebami. | 
|||| 

Ograniczenia te dotyczą liczbę artefaktów, które można dodać do konta integracji.

#### <a name="free-pricing-tier"></a>Bezpłatnej warstwy cenowej

| Name (Nazwa) | Limit | Uwagi | 
| ---- | ----- | ----- | 
| Umowy | 10 | | 
| Inne typy artefaktów | 25 |Typy artefaktu zawierają partnerów, schematów, certyfikaty i mapy. Każdy typ może zawierać maksymalnie maksymalną liczbę artefaktów. | 
|||| 

#### <a name="standard-pricing-tier"></a>Warstwa cenowa standardowa

| Name (Nazwa) | Limit | Uwagi | 
| ---- | ----- | ----- | 
| Dowolnego typu artefaktu | 500 | Typy artefaktu zawierają umowy, partnerów, schematów, certyfikaty i mapy. Każdy typ może zawierać maksymalnie maksymalną liczbę artefaktów. | 
|||| 

### <a name="b2b-protocols-as2-x12-edifact-message-size"></a>Rozmiar wiadomości protokoły B2B (AS2, X12, EDIFACT)

Ograniczenia te dotyczą protokoły B2B.

| Name (Nazwa) | Limit | Uwagi | 
| ---- | ----- | ----- | 
| AS2 | 50 MB | Ma zastosowanie do zdekodowania i kodowania | 
| X12 | 50 MB | Ma zastosowanie do zdekodowania i kodowania | 
| EDIFACT | 50 MB | Ma zastosowanie do zdekodowania i kodowania | 
|||| 

<a name="configuration"></a>

## <a name="configuration-ip-addresses"></a>Konfiguracji: Adresy IP

### <a name="logic-apps-service"></a>Usługa aplikacji logiki

Wywołania, przez które aplikacji logiki bezpośrednio, oznacza to, za pomocą [HTTP](../connectors/connectors-native-http.md) lub [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) lub inne żądania HTTP pochodzą z adresów IP na tej liście.

|Region aplikacji logiki|Wychodzącego|
|-----------------|-----------|
|Australia Wschodnia|13.75.149.4, 104.210.91.55, 104.210.90.241|
|Australia Południowo-Wschodnia|13.73.114.207, 13.77.3.139, 13.70.159.205|
|Brazylia Południowa|191.235.82.221, 191.235.91.7, 191.234.182.26|
|Kanada Środkowa|52.233.29.92, 52.228.39.241, 52.228.39.244|
|Kanada Wschodnia|52.232.128.155, 52.229.120.45, 52.229.126.25|
|Indie Środkowe|52.172.154.168, 52.172.186.159, 52.172.185.79|
|Środkowe stany USA|13.67.236.125, 104.208.25.27, 40.122.170.198|
|Azja Wschodnia|13.75.94.173, 40.83.127.19, 52.175.33.254|
|Wschodnie stany USA|13.92.98.111, 40.121.91.41, 40.114.82.191|
|Wschodnie stany USA 2|40.84.30.147, 104.208.155.200, 104.208.158.174|
|Japonia Wschodnia|13.71.158.3, 13.73.4.207, 13.71.158.120|
|Japonia Zachodnia|40.74.140.4, 104.214.137.243, 138.91.26.45|
|Środkowo-północne stany USA|168.62.248.37, 157.55.210.61, 157.55.212.238|
|Europa Północna|40.113.12.95, 52.178.165.215, 52.178.166.21|
|Środkowo-południowe stany USA|104.210.144.48, 13.65.82.17, 13.66.52.232|
|Azja Południowo-Wschodnia|13.76.133.155, 52.163.228.93, 52.163.230.166|
|Indie Południowe|52.172.50.24, 52.172.55.231, 52.172.52.0|
|Środkowo-zachodnie stany USA|52.161.27.190, 52.161.18.218, 52.161.9.108|
|Europa Zachodnia|40.68.222.65, 40.68.209.23, 13.95.147.65|
|Indie Zachodnie|104.211.164.80, 104.211.162.205, 104.211.164.136|
|Zachodnie stany USA|52.160.92.112, 40.118.244.241, 40.118.241.243|
|Zachodnie stany USA 2|13.66.210.167, 52.183.30.169, 52.183.29.132|
|Południowe Zjednoczone Królestwo|51.140.74.14, 51.140.73.85, 51.140.78.44|
|Zachodnie Zjednoczone Królestwo|51.141.54.185, 51.141.45.238, 51.141.47.136|
| | |

### <a name="connectors"></a>Łączniki

Wywołania który [łączniki](../connectors/apis-list.md) upewnij się, pochodzi z adresów IP na tej liście.

|Region aplikacji logiki|Wychodzącego|
|-----------------|-----------|
|Australia Wschodnia|40.126.251.213|
|Australia Południowo-Wschodnia|40.127.80.34|
|Brazylia Południowa|191.232.38.129|
|Kanada Środkowa|52.233.31.197, 52.228.42.205, 52.228.33.76, 52.228.34.13|
|Kanada Wschodnia|52.229.123.98, 52.229.120.178, 52.229.126.202, 52.229.120.52|
|Indie Środkowe|104.211.98.164|
|Środkowe stany USA|40.122.49.51|
|Azja Wschodnia|23.99.116.181|
|Wschodnie stany USA|191.237.41.52|
|Wschodnie stany USA 2|104.208.233.100|
|Japonia Wschodnia|40.115.186.96|
|Japonia Zachodnia|40.74.130.77|
|Środkowo-północne stany USA|65.52.218.230|
|Europa Północna|104.45.93.9|
|Środkowo-południowe stany USA|104.214.70.191|
|Azja Południowo-Wschodnia|13.76.231.68|
|Indie Południowe|104.211.227.225|
|Europa Zachodnia|40.115.50.13|
|Indie Zachodnie|104.211.161.203|
|Zachodnie stany USA|104.40.51.248|
|Południowe Zjednoczone Królestwo|51.140.80.51|
|Zachodnie Zjednoczone Królestwo|51.141.47.105|
| | | 

## <a name="next-steps"></a>Kolejne kroki  

* [Tworzenie pierwszej aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* [Typowe przykłady i scenariusze](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Wideo: Automatyzować procesy biznesowe z usługą Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
* [Wideo: Integrować systemy z usługą Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
