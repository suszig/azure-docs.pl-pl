---
title: "Za pomocą usługi Zarządzanie interfejsami API do generowania żądań HTTP"
description: "Dowiedz się, jak użyć zasad żądań i odpowiedzi w usłudze API Management do wywołania usług zewnętrznych z interfejsu API"
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 4539c0fa-21ef-4b1c-a1d4-d89a38c242fa
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 6b7f1268ea4893307713931e7288f5d38c5ee080
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="using-external-services-from-the-azure-api-management-service"></a>Za pomocą usług zewnętrznych z usługi Azure API Management
Zasady dostępne w usłudze Azure API Management należy szeroką gamę przydatne pracy wyłącznie na podstawie żądania przychodzącego, odpowiedzi wychodzącej i podstawowe informacje o konfiguracji. Jednak możliwość interakcji z usługami zewnętrznych z interfejsu API zarządzania otwiera wiele możliwości więcej zasad.

Zaobserwowano wcześniej, jak firma Microsoft może interakcyjnie [usługi Azure Event Hub rejestrowania, monitorowania i analizy](api-management-log-to-eventhub-sample.md). W tym artykule przedstawiono usługi opartej na zasadach, które umożliwiają interakcję z wszelkich zewnętrznych HTTP. Te zasady mogą służyć do wyzwalania zdarzenia zdalnego lub do pobierania informacji, która będzie służyć do modyfikowania oryginalnego żądania i odpowiedzi w jakiś sposób.

## <a name="send-one-way-request"></a>Sposób żądania, Wyślij co w-
Prawdopodobnie styl fire i zapomnij żądania, które umożliwia zewnętrznej usługi użytkownik jest powiadamiany określonego rodzaju zdarzenia ważne jest najprostsza interakcja zewnętrznych. Możemy użyć zasad przepływu sterowania `choose` do wykrywania warunku, że Dbamy o, a następnie, jeśli spełniony jest warunek, firma Microsoft może wprowadzać zewnętrznych HTTP żądania przy użyciu dowolnego rodzaju [— jeden — sposób — żądanie wysłania](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) zasad. Może to być żądanie systemu obsługi wiadomości, takie jak Hipchat lub Slack lub poczty interfejsu API SendGrid lub MailChimp, lub do obsługi krytycznych zdarzeń coś tak jak PagerDuty. Wszystkie te systemy obsługi komunikatów mają prostych interfejsów API protokołu HTTP, które firma Microsoft może łatwo wywołać.

### <a name="alerting-with-slack"></a>Alerty z zapas czasu
W poniższym przykładzie pokazano sposób wysyłania wiadomości Slack pokoju rozmów, jeśli kod stanu odpowiedzi HTTP jest większa niż lub równa 500. Błąd zakresu 500 wskazuje na problem z naszych zaplecza interfejsu API klienta interfejsie API nie mogą się samoistnie rozwiązać. Zwykle wymagają określonego rodzaju interwencji z naszej strony.  

```xml
<choose>
    <when condition="@(context.Response.StatusCode >= 500)">
      <send-one-way-request mode="new">
        <set-url>https://hooks.slack.com/services/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</set-url>
        <set-method>POST</set-method>
        <set-body>@{
                return new JObject(
                        new JProperty("username","APIM Alert"),
                        new JProperty("icon_emoji", ":ghost:"),
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                context.Request.Method,
                                                context.Request.Url.Path + context.Request.Url.QueryString,
                                                context.Request.Url.Host,
                                                context.Response.StatusCode,
                                                context.Response.StatusReason,
                                                context.User.Email
                                                ))
                        ).ToString();
            }</set-body>
      </send-one-way-request>
    </when>
</choose>
```

Zapas czasu ma pojęcie punkty zaczepienia dla ruchu przychodzącego sieci web. Podczas konfigurowania punktu zaczepienia przychodzący sieci web, zapas czasu generuje specjalne adresu URL, który umożliwia przeprowadzenie prostego żądania POST i przekazania wiadomości do kanału Slack. Treść kodu JSON, który utworzymy jest oparty na formacie zdefiniowane przez zapas czasu.

![Haku Slack sieci Web](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>To jest fire i zapomnij wystarczająco dobre?
Brak niektórych wady i zalety używając stylu fire i zapomnij żądania. Jeśli dla jakiegoś powodu żądanie zakończy się niepowodzeniem, a następnie błędu nie będzie raportowana. W takiej sytuacji określonego złożoność o awarii dodatkowej raportowania systemu i koszt wydajności dodatkowe oczekiwania na odpowiedź nie jest uzasadnione. W scenariuszach, w których jest sprawdzenie odpowiedzi a następnie [żądanie wysłania](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) zasad jest lepszym rozwiązaniem.

## <a name="send-request"></a>Żądanie wysłania
`send-request` Umożliwia zasad za pomocą zewnętrznej usługi do wykonywania zadań przetwarzania złożone i zwraca dane do usługi API management usług, który może służyć do dalszego przetwarzania zasad.

### <a name="authorizing-reference-tokens"></a>Autoryzowanie tokenów odwołań
Główna funkcja interfejsu API zarządzania jest ochrona zasobów wewnętrznej bazy danych. Jeśli serwer autoryzacji używany przez interfejs API tworzy [tokenów JWT](http://jwt.io/) w ramach swojego przepływu OAuth2 jako [usługi Azure Active Directory](../active-directory/active-directory-aadconnect.md) tak, możesz użyć `validate-jwt` zasad w celu zweryfikowania jego ważności tokenu. Jednak niektóre serwery autoryzacji utworzyć są nazywane [odwołania tokenów](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) nie można zweryfikować bez wykonywania wywołań do serwera autoryzacji.

### <a name="standardized-introspection"></a>Introspection standardowych
W przeszłości nie było żadnych standardowych możliwości sprawdzenia tokenu odwołania z serwera autoryzacji. Jednak ostatnio proponowany standard [RFC 7662](https://tools.ietf.org/html/rfc7662) została opublikowana przez grupę roboczą IETF, który definiuje sposób serwer zasobów można sprawdzić poprawności tokenu.

### <a name="extracting-the-token"></a>Wyodrębnianie tokenu
Pierwszym krokiem jest wyodrębnić tokenu w nagłówku autoryzacji. Wartość nagłówka powinien być sformatowany z `Bearer` schematu autoryzacji, pojedyncze spacje, a następnie autoryzację token zgodnie [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Niestety istnieją przypadki, w których pominięto schematu autoryzacji. Aby to uwzględniać podczas analizowania, możemy podzielić wartość nagłówka w miejscu i wybierz ostatni ciąg z zwrócony tablicy ciągów. Zapewnia to obejście nagłówki autoryzacji nieprawidłowo sformatowany.

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-the-validation-request"></a>Zgłoszenia żądania weryfikacji
Gdy mamy token autoryzacji, firma Microsoft może wprowadzać żądania do zweryfikowania tokenu. RFC 7662 wywołuje introspection ten proces i wymaga, aby użytkownik `POST` formularza HTML do zasobu introspection. Formularza HTML przy użyciu klucza musi zawierać co najmniej parę klucz/wartość `token`. To żądanie do serwera autoryzacji również musi zostać uwierzytelniony do zapewnienia, że złośliwe klientów nie można go połowów włokiem dla prawidłowych tokenów.

```xml
<send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
  <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
  <set-method>POST</set-method>
  <set-header name="Authorization" exists-action="override">
    <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
  </set-header>
  <set-header name="Content-Type" exists-action="override">
    <value>application/x-www-form-urlencoded</value>
  </set-header>
  <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
</send-request>
```

### <a name="checking-the-response"></a>Sprawdzanie odpowiedzi
`response-variable-name` Atrybut jest używany do udostępnienia zwrócona odpowiedź. Nazwa zdefiniowana w tej właściwości może służyć jako klucza do `context.Variables` słownika dostępu do `IResponse` obiektu.

Z obiektu odpowiedzi można pobrać treści i RFC 7622 nam informuje, że odpowiedź musi być obiektem JSON i musi zawierać co najmniej właściwość o nazwie `active` będący wartością logiczną. Gdy `active` ma wartość true, a następnie token jest uznawany za ważny.

### <a name="reporting-failure"></a>Niepowodzenie raportowania
Używamy `<choose>` zasady, aby wykryć, czy token jest nieprawidłowy, a jeśli tak, zwracać odpowiedzi 401.

```xml
<choose>
  <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
    <return-response response-variable-name="existing response variable">
      <set-status code="401" reason="Unauthorized" />
      <set-header name="WWW-Authenticate" exists-action="override">
        <value>Bearer error="invalid_token"</value>
      </set-header>
    </return-response>
  </when>
</choose>
```

Zgodnie [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) którym opisano sposób `bearer` tokenów należy również zwróconych `WWW-Authenticate` nagłówek odpowiedzi 401. WWW-Authenticate ma na celu poinstruować klienta, jak utworzyć prawidłowo nieautoryzowane żądanie. Z powodu różnych metod niemożliwe w ramach protokołu OAuth2 jest trudne do komunikowania się wszystkich wymaganych informacji. Na szczęście są przetwarzane, aby ułatwić [klientów dowiedzieć się, jak poprawnie autoryzować żądania do serwera zasobów](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Końcowa rozwiązania
Zestawienie wszystkie elementy, uzyskujemy następujące zasady:

```xml
<inbound>
  <!-- Extract Token from Authorization header parameter -->
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

  <!-- Send request to Token Server to validate token (see RFC 7662) -->
  <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
    <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
    <set-method>POST</set-method>
    <set-header name="Authorization" exists-action="override">
      <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
    </set-header>
    <set-header name="Content-Type" exists-action="override">
      <value>application/x-www-form-urlencoded</value>
    </set-header>
    <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
  </send-request>

  <choose>
          <!-- Check active property in response -->
          <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
              <!-- Return 401 Unauthorized with http-problem payload -->
              <return-response response-variable-name="existing response variable">
                  <set-status code="401" reason="Unauthorized" />
                  <set-header name="WWW-Authenticate" exists-action="override">
                      <value>Bearer error="invalid_token"</value>
                  </set-header>
              </return-response>
          </when>
      </choose>
  <base />
</inbound>
```

To jest tylko jedna wiele przykładów jak `send-request` zasad służy do integrowania przydatne usług zewnętrznych przetwarzania żądań i odpowiedzi przepływających przez usługi Zarządzanie interfejsami API.

## <a name="response-composition"></a>Kompozycja odpowiedzi
`send-request` Zasad może być używany do podnoszenia żądania podstawowego z systemem zaplecza jako widzieliśmy w poprzednim przykładzie, albo może służyć jako zakończenie Zamień dla wywołania wewnętrznej bazy danych. Ta technika można łatwo utworzyć złożonego zasoby, które są agregowane z wielu różnych systemów.

### <a name="building-a-dashboard"></a>Tworzenie pulpitu nawigacyjnego
Czasami ma być możliwe na celu ujawnienie informacji, który istnieje w wielu systemach wewnętrznej bazy danych, na przykład, aby dysk na pulpicie nawigacyjnym. Kluczowych wskaźników wydajności pochodzić z wszystkich różnych zapleczy, ale nie chcesz zapewnić bezpośredni dostęp do nich, i byłoby nieuprzywilejowany w ramach pojedynczego żądania można pobrać wszystkich informacji. Prawdopodobnie niektóre informacje wewnętrznej bazy danych wymaga niektórych dzielenie i grupowanie i niewielką najpierw oczyszczania! Możliwość buforowania złożonego zasobu będzie można zmniejszyć obciążenie wewnętrznej bazy danych, ponieważ wiadomo, że użytkownicy mają rodzaj atakowaniu klawisz F5, aby zobaczyć, czy ich underperforming metryki mogą ulec zmianie.    

### <a name="faking-the-resource"></a>Faking zasobu
Pierwszy krok w celu tworzenia zasobów naszej pulpit nawigacyjny jest do skonfigurowania nowej operacji w portalu zarządzania interfejsu API wydawcy. Jest to operacja symbolu zastępczego, używany do konfigurowania naszymi zasadami kompozycji do naszej zasobu dynamicznego tworzenia.

![Operacja pulpitu nawigacyjnego](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Tworzenie żądania
Raz `dashboard` utworzono operacji można skonfigurować zasady dla tej operacji. 

![Operacja pulpitu nawigacyjnego](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Pierwszym krokiem jest Wyodrębnij wszystkie parametry zapytania z żądania przychodzącego, dzięki czemu możemy przekazują je do naszej wewnętrznej bazy danych. W tym przykładzie naszych pulpit nawigacyjny jest pokazywane są informacje oparte na pewien czas w związku z tym ma `fromDate` i `toDate` parametru. Możemy użyć `set-variable` zasady w celu wyodrębnienia informacji z adresu URL żądania.

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

Gdy mamy tych informacji firma Microsoft może wprowadzać żądania we wszystkich systemach wewnętrznej bazy danych. Każde żądanie tworzy nowy adres URL z parametrów i wywołuje jego odpowiedniego serwera i przechowuje odpowiedzi w zmiennej kontekstu.

```xml
<send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
  <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
  <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>
```

Te żądania będą wykonywane w kolejności, która nie jest idealnym rozwiązaniem. W kolejnych wersji możemy można wprowadzenia nowych zasad o nazwie `wait` umożliwi wszystkie te żądania do wykonywania równoległego.

### <a name="responding"></a>Odpowiada
Do utworzenia złożonego odpowiedzi, możemy użyć [odpowiedzi zwracany](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) zasad. `set-body` Elementu można użyć wyrażenia do utworzenia nowego `JObject` z reprezentacji składnika osadzony jako właściwości.

```xml
<return-response response-variable-name="existing response variable">
  <set-status code="200" reason="OK" />
  <set-header name="Content-Type" exists-action="override">
    <value>application/json</value>
  </set-header>
  <set-body>
    @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                  new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                  new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                  new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                  ).ToString())
  </set-body>
</return-response>
```

Zasady w pełnej wygląda następująco:

```xml
<policies>
    <inbound>

  <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
  <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
</policies>
```

W konfiguracji symbol zastępczy operacji można skonfigurować zasobów pulpitu nawigacyjnego można buforować przez co najmniej godzinę, ponieważ Rozumiemy charakter danych oznacza, że nawet jeśli jest nieaktualna, będzie on nadal dostępny wystarczająco skuteczne w celu przedstawienia cenne informacje, które użytkownicy godzinę.

## <a name="summary"></a>Podsumowanie
Usługi Zarządzanie interfejsami API Azure oferuje elastyczne zasady, które można wybiórczo zastosować do ruchu HTTP i umożliwia kompozycji usługi wewnętrznej bazy danych. Czy chcesz zwiększyć bramy interfejsu API z alerty funkcje, weryfikacji i sprawdzania poprawności możliwości lub utworzyć nowe zasoby złożonego na podstawie wielu usług zaplecza, `send-request` i związane z nią zasady Otwórz world możliwości.

## <a name="watch-a-video-overview-of-these-policies"></a>Obejrzyj film poglądowy dotyczący tych zasad
Aby uzyskać więcej informacji na temat [sposób żądania, Wyślij co w-](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [żądanie wysłania](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), i [odpowiedzi zwracany](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) zasady omówione w tym artykule, obserwuj poniższego klipu wideo.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 

