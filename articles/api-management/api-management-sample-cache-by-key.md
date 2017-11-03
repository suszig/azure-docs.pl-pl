---
title: "Buforowanie niestandardowych w usłudze Azure API Management"
description: "Dowiedz się, jak i pamięci podręcznej elementów przez klucz w usłudze Azure API Management"
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 772bc8dd-5cda-41c4-95bf-b9f6f052bc85
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 4a41e4e0be44e855ead253ad76fe5a3af52070ec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="custom-caching-in-azure-api-management"></a>Buforowanie niestandardowych w usłudze Azure API Management
Usługa Azure API Management ma wbudowaną obsługę [buforowanie odpowiedzi HTTP](api-management-howto-cache.md) przy użyciu adresu URL zasobu jako klucz. Klucz może być modyfikowany przez nagłówki żądania przy użyciu `vary-by` właściwości. Jest to przydatne w przypadku buforowanie całej odpowiedzi HTTP (alias oświadczenia), ale czasami jest przydatne do pamięci podręcznej tylko część reprezentacji. Nowy [pamięci podręcznej wyszukiwania wartości](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) i [-magazynu wartość w pamięci podręcznej](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) zasady umożliwiają przechowywanie i pobieranie dowolnych fragmentów danych z poziomu definicji zasad. Tę możliwość również dodaje wartość poprzednio wprowadzone [żądanie wysłania](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) zasad ponieważ teraz może buforować odpowiedzi z usług zewnętrznych.

## <a name="architecture"></a>Architektura
Zarządzanie interfejsami API usługi używa pamięci podręcznej danych dzierżawy udostępnionego tak, aby podczas skalowania do wielu jednostkach nadal uzyskają dostęp do tej samej dane z pamięci podręcznej. Podczas pracy z wdrożenia w przypadku istnieją niezależne pamięci podręcznych w poszczególnych regionach. Ze względu na to koniecznie traktuje jako magazynu danych, gdzie jest tylko źródło dla elementu informacji o pamięci podręcznej. Jeśli została i później chcę korzystać z wdrożenia w przypadku klientów z użytkowników, którzy podróżują mogą stracić dostęp do pamięci podręcznej danych.

## <a name="fragment-caching"></a>Buforowanie fragmentu
Brak niektórych przypadkach, gdy odpowiedzi zwracanych zawiera pewną część danych, jest kosztowna ustalić, który jeszcze pozostaje świeże sensownym czasie. Na przykład należy wziąć pod uwagę usługę utworzony przez linii lotniczych, który zawiera informacje dotyczące zastrzeżenia transmitowane, stan transmitowane itp. Jeśli użytkownik jest członkiem program punktów airlines, również zostałyby informacji dotyczących ich bieżący stan i zebraniu przebiegu. Te informacje dotyczące użytkowników mogą być przechowywane w innym systemie, ale może być pożądane, aby uwzględnić go w odpowiedzi dotyczące stanu transmitowane i zastrzeżenia zwrócone. Można to zrobić w procesie zwanym buforowanie fragmentu. Reprezentacja głównej mogą być zwracane z serwera pochodzenia przy użyciu określonego rodzaju token, aby wskazać, gdzie jest informacji dotyczących użytkownika ma zostać wstawiony. 

Należy wziąć pod uwagę następujące odpowiedzi JSON z zaplecza interfejsu API.

```json
{
  "airline" : "Air Canada",
  "flightno" : "871",
  "status" : "ontime",
  "gate" : "B40",
  "terminal" : "2A",
  "userprofile" : "$userprofile$"
}  
```

I dodatkowej zasobu pod adresem `/userprofile/{userid}` takie, jak,

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

Aby było możliwe określenie odpowiedniego użytkownika informacje do uwzględnienia, należy określić kto jest użytkownika końcowego. Ten mechanizm jest zależy od implementacji. Na przykład używam `Subject` oświadczeń z `JWT` tokenu. 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

To są przechowywane `enduserid` wartość w zmiennej kontekstu do późniejszego użycia. Następnym krokiem jest ustalenie, jeśli poprzednie żądanie już pobrane informacje o użytkowniku i zapisana w pamięci podręcznej. W tym używamy `cache-lookup-value` zasad.

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

Jeśli w pamięci podręcznej, która odpowiada wartości klucza, a następnie nr nie ma wpisu `userprofile` zmiennej kontekstu, która zostanie utworzona. Musimy sprawdzić, powodzenia za pomocą wyszukiwania `choose` sterowanie przepływem zasad.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
    </when>
</choose>
```

Jeśli `userprofile` zmiennej kontekstu, która nie istnieje, a następnie zamierzamy wykonanie żądania HTTP można go pobrać.

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points to the profile for the current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

Używamy `enduserid` do konstruowania adresu URL do zasobu profilu użytkownika. Gdy mamy odpowiedzi możemy ściągnięcia treści tekstu z odpowiedzi i zapisze go do zmiennej kontekstu.

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

Aby uniknąć nam konieczności żądania HTTP ponownie, gdy użytkownik przesyła innego żądania, firma Microsoft może przechowywać profil użytkownika w pamięci podręcznej.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

Wartości są przechowywane w pamięci podręcznej używa dokładnie tego samego klucza, który pierwotnie próby pobrania za pomocą. Czas trwania wybranego do przechowywania wartości powinny być oparte na temat często zmiany informacji i sposobu odporny na błędy użytkowników są do nieaktualne informacje. 

Należy koniecznie należy pamiętać, że pobieranie z pamięci podręcznej jest nadal poza procesem, żądanie sieciowe i potencjalnie można nadal dodawać dziesiątki w milisekundach czas oczekiwania na żądanie. Zalety trybu podczas ustalania, czy informacje o profilu użytkownika znacznie dłużej, niż ze względu na potrzeby bazy danych zapytania lub zagregowanych informacji z wielu zapleczy.

Ostatnim krokiem w procesie jest aktualizacja odpowiedź zwrócona z naszych informacje o profilu użytkownika.

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

Wybrano do znaki cudzysłowu należy uwzględnić jako część tokenu, dzięki czemu nawet wtedy, gdy Zastąp nie zachodzi, odpowiedź była nadal poprawne dane JSON. To przede wszystkim ułatwia debugowanie.

Te kroki są łączone ze sobą, wynik końcowy po zasad, która wygląda podobnie do następującego.

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in the cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request to get user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points to the profile for the current end-user -->
                    <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>

                <!-- Store response body in context variable -->
                <set-variable
                  name="userprofile"
                  value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                <!-- Store result in cache -->
                <cache-store-value
                  key="@("userprofile-" + context.Variables["enduserid"])"
                  value="@((string)context.Variables["userprofile"])"
                  duration="100000" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <!-- Update response body with user profile-->
        <find-and-replace
              from='"$userprofile$"'
              to="@((string)context.Variables["userprofile"])" />
        <base />
    </outbound>
</policies>
```

Takie podejście buforowania jest używany głównie w witrynach sieci web w przypadku gdy składa HTML po stronie serwera, dzięki czemu mogą być renderowane jako pojedynczej strony. Jednak również jest przydatny w interfejsów API, gdzie klienci nie klienta po stronie buforowanie HTTP lub jest wprowadzania że odpowiedzialność na kliencie.

Można również wykonać tego samego rodzaju fragmentu buforowanie na serwerach sieci web zaplecza przy użyciu Redis, buforowanie serwera, jednak przy użyciu usługi API Management do wykonywania tej pracy jest przydatne, gdy buforowane fragmenty pochodzą z różnych zapleczy niż podstawowy odpowiedzi.

## <a name="transparent-versioning"></a>Przechowywanie wersji przezroczyste
Jest typowym rozwiązaniem w przypadku wielu wersji inną implementację interfejsu API do obsługi w dowolnym momencie. Jest to prawdopodobnie do obsługi różnych środowiskach, takich jak deweloperów, testu, produkcji itp., lub może być do obsługi starszych wersji interfejsu API, aby zapewnić czas dla konsumentów interfejsu API przeprowadzić migrację do nowszych wersji. 

Jeden ze sposobów to obsługi zamiast deweloperom klienta zmiany adresów URL z `/v1/customers` do `/v2/customers` jest do przechowywania danych profilu konsumenta wersji interfejsu API obecnie chcą korzystać, a następnie wywołać URL odpowiedniej wewnętrznej bazy danych. W celu ustalenia zaplecza poprawny adres URL do wywołania dla określonego klienta, należy zbadać niektóre dane konfiguracji. Buforowanie danych konfiguracji, możemy zminimalizować spadek wydajności prowadzenia tego wyszukiwania.

Pierwszym krokiem jest identyfikator używany do konfigurowania żądanej wersji. W tym przykładzie wybrano skojarzyć tę wersję produktu klucza subskrypcji. 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

Następnie przejdziemy wyszukiwania pamięci podręcznej, aby zobaczyć, jeśli już pobrano wersja żądanego klienta.

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

Następnie możemy Sprawdź, czy nie znaleziono go w pamięci podręcznej.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

Jeśli firma Microsoft nie możemy przejść i pobrać go.

```xml
<send-request
    mode="new"
    response-variable-name="clientconfiguresponse"
    timeout="10"
    ignore-error="true">
            <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
            <set-method>GET</set-method>
</send-request>
```

Wyodrębnienie z odpowiedzi tekstu treści odpowiedzi.

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

Zapisz go w pamięci podręcznej do użycia w przyszłości.

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

I na koniec zaktualizuj adres URL zaplecza, aby wybrać wersję żądanej przez klienta usługi.

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

Zasady całkowicie ma następującą składnię.

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If we don’t find it in the cache, make a request for it and store it -->
    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">
            <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
            </send-request>
            <!-- Store response body in context variable -->
            <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
            <!-- Store result in cache -->
            <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
        </when>
    </choose>
    <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
</inbound>
```

Umożliwiając użytkownikom interfejsu API niewidocznie kontroli wersji wewnętrznej bazy danych jest uzyskiwany przez klientów bez konieczności aktualizacji i wdrożenie klientów jest atrakcyjny rozwiązaniem, które rozwiązuje wiele problemów przechowywanie wersji interfejsu API.

## <a name="tenant-isolation"></a>Izolacji dzierżawców
W przypadku większych i wielodostępne wdrożeń niektóre firmy utworzenie oddzielnych grup dzierżawcy na różnych wdrożeń sprzętu wewnętrznej bazy danych. Pozwala to zmniejszyć liczbę klientów, którzy mają wpływ problemem sprzętowym do wewnętrznej bazy danych. Umożliwia również nowe wersje oprogramowania na wprowadzanie etapami. W idealnym przypadku tej architektury wewnętrznej bazy danych powinny być przezroczyste konsumentom interfejsu API. Można to osiągnąć w sposób podobny do przechowywania wersji przezroczysty, ponieważ jest on oparty na tę samą metodę manipulowania URL wewnętrznej bazy danych przy użyciu stan konfiguracji na klucz interfejsu API.  

Zamiast zwracać preferowaną wersję interfejsu API dla każdego klucza subskrypcji, zwróci identyfikator, który dotyczy dzierżawcy grupy przypisanej sprzętu. Ten identyfikator może służyć do skonstruowania URL odpowiedniej wewnętrznej bazy danych.

## <a name="summary"></a>Podsumowanie
Swobody obsługi pamięci podręcznej zarządzania interfejsu API Azure do przechowywania każdego typu danych umożliwia wydajne dostępu do danych konfiguracji, które mogą mieć wpływ na sposób przetwarzania przychodzącego żądania. Można go również używane do przechowywania fragmenty danych, które można rozszerzyć odpowiedzi zwrócone przez interfejs API zaplecza.

## <a name="next-steps"></a>Następne kroki
Daj nam swoją opinię w wątku usługi Disqus dla tego tematu, jeśli istnieją inne scenariusze, w których te zasady mają włączona lub jeśli istnieją scenariusze chcesz osiągnąć, ale czy konieczne jest obecnie możliwe.

