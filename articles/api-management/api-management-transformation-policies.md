---
title: "Zasad przekształcania usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat zasad przekształcania dostępne do użycia w usłudze Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 7406a8ce-5f9c-4fae-9b0f-e574befb2ee9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: c2bed904b82c569b28a6e00d0cc9b49107c148dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-transformation-policies"></a>Zarządzanie interfejsami API zasad przekształcania
W tym temacie znajdują się informacje na następujące zasady usługi API Management. Aby uzyskać informacje dotyczące dodawania i konfigurowania zasad, zobacz [zasad w usłudze API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="TransformationPolicies"></a>Zasad przekształcania  
  
-   [Konwertuj JSON do pliku XML](api-management-transformation-policies.md#ConvertJSONtoXML) — konwertuje żądania lub odpowiedzi body z formatu JSON do pliku XML.  
  
-   [Konwertuj do formatu JSON XML](api-management-transformation-policies.md#ConvertXMLtoJSON) — konwertuje żądania lub odpowiedzi body z pliku XML do formatu JSON.  
  
-   [Znajdowanie i zamienianie ciągów w treści](api-management-transformation-policies.md#Findandreplacestringinbody) — znajduje podciąg żądania lub odpowiedzi i zastępuje go inny podciąg.  
  
-   [Maski adresów URL w zawartości](api-management-transformation-policies.md#MaskURLSContent) -ponownie zapisuje (maski) łącza w odpowiedzi body tak, aby wskazywać równoważne połączenie za pośrednictwem bramy.  
  
-   [Ustaw usługę zaplecza](api-management-transformation-policies.md#SetBackendService) — zmienia usługi wewnętrznej bazy danych dla żądania przychodzącego.  
  
-   [Ustaw treści](api-management-transformation-policies.md#SetBody) -ustawia treść komunikatu dla żądań przychodzących i wychodzących.  
  
-   [Set — nagłówek HTTP](api-management-transformation-policies.md#SetHTTPheader) — przypisuje wartość do istniejących odpowiedzi i/lub nagłówek żądania lub dodaje nowy nagłówek odpowiedzi i/lub żądania.  
  
-   [Wartość parametru ciągu zapytania](api-management-transformation-policies.md#SetQueryStringParameter) — dodaje, zastępuje wartość lub usuwa parametru ciągu zapytania żądania.  
  
-   [Ponowne zapisywanie adresów URL](api-management-transformation-policies.md#RewriteURL) — konwertuje adresie URL żądania w postaci publicznego do formularza oczekiwane przez usługę sieci web.  
  
-   [Przekształcanie XML za pomocą XSLT](api-management-transformation-policies.md#XSLTransform) — ma zastosowanie transformacji XSL do formatu XML w treści żądania lub odpowiedzi.  
  
##  <a name="ConvertJSONtoXML"></a>Konwertuj JSON do pliku XML  
 `json-to-xml` Zasad konwertuje treści żądania lub odpowiedzi z formatu JSON do pliku XML.  
  
### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a>Przykład  
  
```xml  
<policies>  
    <inbound>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
        <json-to-xml apply="always" consider-accept-header="false" />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|JSON do pliku xml|Element główny.|Tak|  
  
### <a name="attributes"></a>Atrybuty  
  
|Nazwa|Opis|Wymagane|Domyślne|  
|----------|-----------------|--------------|-------------|  
|Zastosuj|Atrybut musi zostać ustawiony na jedną z następujących wartości.<br /><br /> -zawsze — Zawsze stosuj konwersji.<br />convert zawartości typu json - tylko wtedy, gdy nagłówek odpowiedzi Content-Type wskazuje obecność JSON.|Tak|Nie dotyczy|  
|należy wziąć pod uwagę zaakceptować — nagłówek|Atrybut musi zostać ustawiony na jedną z następujących wartości.<br /><br /> — wartość true — zastosowania konwersji w żądaniu nagłówka Accept żądania JSON.<br />— wartość false — Zawsze stosuj konwersji.|Nie|Wartość true|  
  
### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** przychodzące, wychodzące, na błąd  
  
-   **Zakresy zasad:** globalnych produktu interfejsu API, operacji  
  
##  <a name="ConvertXMLtoJSON"></a>Konwertuj do formatu JSON XML  
 `xml-to-json` Zasad konwertuje treści żądania lub odpowiedzi z pliku XML do formatu JSON. Ta zasada może służyć do modernizacji interfejsów API oparty na usługach sieci web XML — tylko do wewnętrznej bazy danych.  
  
### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a>Przykład  
  
```xml  
<policies>  
    <inbound>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
        <xml-to-json kind="direct" apply="always" consider-accept-header="false" />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|XML do formatu json|Element główny.|Tak|  
  
### <a name="attributes"></a>Atrybuty  
  
|Nazwa|Opis|Wymagane|Domyślne|  
|----------|-----------------|--------------|-------------|  
|rodzaj|Atrybut musi zostać ustawiony na jedną z następujących wartości.<br /><br /> javascript — przyjazne - przekonwertowanego JSON ma formę przyjazną dla deweloperów języka JavaScript.<br />-bezpośredni — przekonwertowanego JSON odzwierciedla struktury oryginalnego dokumentu XML.|Tak|Nie dotyczy|  
|Zastosuj|Atrybut musi zostać ustawiony na jedną z następujących wartości.<br /><br /> -zawsze - przekonwertować zawsze.<br />convert zawartości — typ xml - tylko wtedy, gdy nagłówek odpowiedzi Content-Type wskazuje obecność XML.|Tak|Nie dotyczy|  
|należy wziąć pod uwagę zaakceptować — nagłówek|Atrybut musi zostać ustawiony na jedną z następujących wartości.<br /><br /> Jeśli żądanie XML w żądaniu nagłówek Accept - wartość true — mają zastosowanie konwersji.<br />— wartość false — Zawsze stosuj konwersji.|Nie|Wartość true|  
  
### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** przychodzące, wychodzące, na błąd  
  
-   **Zakresy zasad:** globalnych produktu interfejsu API, operacji  
  
##  <a name="Findandreplacestringinbody"></a>Znajdowanie i zamienianie ciągów w treści  
 `find-and-replace` Zasad znajduje podciąg żądania lub odpowiedzi i zastępuje go inny podciąg.  
  
### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<find-and-replace from="what to replace" to="replacement" />  
```  
  
### <a name="example"></a>Przykład  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|Znajdź i Zamień|Element główny.|Tak|  
  
### <a name="attributes"></a>Atrybuty  
  
|Nazwa|Opis|Wymagane|Domyślne|  
|----------|-----------------|--------------|-------------|  
|Z|Ciąg do wyszukania.|Tak|Nie dotyczy|  
|na|Ciąg zastępczy. Określ długość zastępczy ciąg o zerowej do usunięcia ciąg wyszukiwania.|Tak|Nie dotyczy|  
  
### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** przychodzący, wychodzący wewnętrznej bazy danych, na błąd  
  
-   **Zakresy zasad:** globalnych produktu interfejsu API, operacji  
  
##  <a name="MaskURLSContent"></a>Adresy URL maski w zawartości  
 `redirect-content-urls` Zasad ponownie zapisuje łącza (maski) w treści odpowiedzi tak, aby wskazywać równoważne połączenie za pośrednictwem bramy. Użyj w sekcji wychodzącego ponownie zapisać łącza treści odpowiedzi będą wskazywać bramy. Użyj części ruchu przychodzącego dla odwrotny efekt.  
  
> [!NOTE]
>  Ta zasada nie zmienia żadnych wartości nagłówka takich `Location` nagłówków. Aby zmienić wartości nagłówka, użyj [nagłówka set](api-management-transformation-policies.md#SetHTTPheader) zasad.  
  
### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a>Przykład  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|adresy URL zawartości przekierowań|Element główny.|Tak|  
  
### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** przychodzące, wychodzące  
  
-   **Zakresy zasad:** globalnych produktu interfejsu API, operacji  
  
##  <a name="SetBackendService"></a>Ustawianie usługi wewnętrznej bazy danych  
 Użyj `set-backend-service` zasad, aby przekierować żądanie przychodzące do wewnętrznej bazy danych innej niż określona w ustawieniach interfejsu API dla tej operacji. Ta zasada zmiany wewnętrznej bazy danych usługi podstawowy adres URL żądania przychodzącego, jeśli określony w zasadach.  
  
### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<set-backend-service base-url="base URL of the backend service" />  
```  
  
### <a name="example"></a>Przykład  
  
```xml  
<policies>  
    <inbound>  
        <choose>  
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2013-05")">  
                <set-backend-service base-url="http://contoso.com/api/8.2/" />  
            </when>  
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2014-03")">  
                <set-backend-service base-url="http://contoso.com/api/9.1/" />  
            </when>  
        </choose>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
W tym przykładzie Ustaw zasady usługi wewnętrznej bazy danych przekierowuje żądania na podstawie wartości wersji przekazany ciąg zapytania do usługi zaplecza inny niż określony w interfejsie API.
  
Początkowo wewnętrznej bazy danych usługi podstawowy adres URL jest pochodną ustawień interfejsu API. Dlatego adres URL żądania `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` staje się `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` gdzie `http://contoso.com/api/10.4/` jest adres URL usługi wewnętrznej bazy danych, które zostały określone w ustawieniach interfejsu API.  
  
Gdy [< Wybierz\> ](api-management-advanced-policies.md#choose) deklaracji zasad są stosowane wewnętrznej bazy danych usługi podstawowy adres URL może zmienić ponownie albo `http://contoso.com/api/8.2` lub `http://contoso.com/api/9.1`, w zależności od wartości parametru zapytania żądania wersji. Na przykład, jeśli wartość jest `"2013-15"` żądania końcowego adresu URL staje się `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.  
  
Jeśli dodatkowo przekształcenie żądania jest odpowiednie, inne [zasad przekształcania](api-management-transformation-policies.md#TransformationPolicies) mogą być używane. Na przykład, aby teraz, żądanie jest przesyłane do wersji określonej w wewnętrznej bazie danych, należy usunąć parametr zapytania wersji [ustawić parametr ciągu zapytania](api-management-transformation-policies.md#SetQueryStringParameter) zasad może służyć do Usuń atrybut nadmiarowe obecnie wersji.  
  
### <a name="example"></a>Przykład  
  
```xml  
<policies>  
    <inbound>  
        <set-backend-service backend-id="my-sf-service" sf-partition-key="@(context.Request.Url.Query.GetValueOrDefault("userId","")" sf-replica-type="primary" /> 
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
W tym przykładzie zasady kieruje żądanie do usługi sieci szkieletowej wewnętrznej bazie danych, przy użyciu identyfikatora userId ciąg zapytania jako klucza partycji i repliką podstawową partycji.  

### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|zestaw wewnętrznej bazy danych usługi|Element główny.|Tak|  
  
### <a name="attributes"></a>Atrybuty  
  
|Nazwa|Opis|Wymagane|Domyślne|  
|----------|-----------------|--------------|-------------|  
|adres url Base|Nowego zaplecza podstawowy adres URL usługi.|Nie|Nie dotyczy|  
|Identyfikator wewnętrznej bazy danych|Identyfikator wewnętrznej bazy danych, aby kierować do.|Nie|Nie dotyczy|  
|rz partycji klucza|Dotyczy tylko podczas wewnętrznej bazy danych to usługa sieci szkieletowej usług i jest określona za pomocą "wewnętrznej bazy danych id". Używany do rozpoznawania określonej partycji z usługi rozpoznawania nazw.|Nie|Nie dotyczy|  
|rz repliki typu|Dotyczy tylko podczas wewnętrznej bazy danych to usługa sieci szkieletowej usług i jest określona za pomocą "wewnętrznej bazy danych id". Kontroluje, czy żądanie należy przejść do podstawowej lub pomocniczej replice partycji. |Nie|Nie dotyczy|    
|rz resolve warunków|Dotyczy tylko w przypadku wewnętrznej bazy danych usługi Service Fabric. Warunek identyfikowania wywołania do zaplecza usługi sieć szkieletowa usług musi być powtarzana z nowego rozwiązania.|Nie|Nie dotyczy|    
|rz usługi wystąpienie nazwa-|Dotyczy tylko w przypadku wewnętrznej bazy danych usługi Service Fabric. Pozwala zmienić wystąpień usługi w czasie wykonywania. |Nie|Nie dotyczy|    

### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** wewnętrznej bazy danych dla ruchu przychodzącego  
  
-   **Zakresy zasad:** globalnych produktu interfejsu API, operacji  
  
##  <a name="SetBody"></a>Zestaw treści  
 Użyj `set-body` zasady można ustawić treść komunikatu dla żądań przychodzących i wychodzących. Aby uzyskać dostęp do treści wiadomości, można użyć `context.Request.Body` właściwości lub `context.Response.Body`, w zależności od tego, czy zasady znajduje się w sekcji ruchu przychodzącego lub wychodzącego.  
  
> [!IMPORTANT]
>  Należy pamiętać, że domyślnie, gdy uzyskujesz dostęp do wiadomości body przy użyciu `context.Request.Body` lub `context.Response.Body`, treści oryginalnej wiadomości zostaną utracone i musi być ustawione przez zwrócenie treści Wstecz w wyrażeniu. Aby zachować zawartość treści, ustaw `preserveContent` parametr `true` podczas uzyskiwania dostępu do wiadomości. Jeśli `preserveContent` ustawiono `true` i różnych treści jest zwracany przez wyrażenie, treść zwracane jest używany.  
>   
>  Należy pamiętać przedstawione poniżej zagadnienia, korzystając z `set-body` zasad.  
>   
>  -   Jeśli używasz `set-body` zasad, aby zwrócić nowe lub zaktualizowane treści, nie trzeba ustawić `preserveContent` do `true` ponieważ są jawnie dostarczenie nowej zawartości w treści.  
> -   Zachowywanie treść odpowiedzi w potoku dla ruchu przychodzącego sensu ponieważ nie istnieje jeszcze żadnej odpowiedzi.  
> -   Zachowywanie zawartości żądania w potoku wychodzącego nie sensu, ponieważ żądania została już wysłana do wewnętrznej bazy danych w tym momencie.  
> -   Jeśli zasada ta jest używana, gdy brak treści wiadomości, na przykład w przychodzącej GET, jest zwracany wyjątek.  
  
 Aby uzyskać więcej informacji, zobacz `context.Request.Body`, `context.Response.Body`i `IMessage` sekcje w [zmiennej kontekstu](api-management-policy-expressions.md#ContextVariables) tabeli.  
  
### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a>Przykłady  
  
#### <a name="literal-text-example"></a>Przykład literału tekstu  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-the-body-as-a-string-note-that-we-are-preserving-the-original-request-body-so-that-we-can-access-it-later-in-the-pipeline"></a>Przykład: uzyskiwanie dostępu do treść jako ciąg. Należy pamiętać, że są firma Microsoft zachowuje oryginalne treści żądania tak, aby firma Microsoft dostęp do niego później w potoku.
  
```xml  
<set-body>  
@{   
    string inBody = context.Request.Body.As<string>(preserveContent: true);   
    if (inBody[0] =='c') {   
        inBody[0] = 'm';   
    }   
    return inBody;   
}  
</set-body>  
```  
  
#### <a name="example-accessing-the-body-as-a-jobject-note-that-since-we-are-not-reserving-the-original-request-body-accesing-it-later-in-the-pipeline-will-result-in-an-exception"></a>Przykład dostęp do treści jako JObject. Należy pamiętać, że od czasu firma Microsoft nie są rezerwowania oryginalnego treści żądania, spowoduje ona później w potoku Wystąpił wyjątek w celu dostępu.  
  
```xml  
<set-body>   
@{   
    JObject inBody = context.Request.Body.As<JObject>();   
    if (inBody.attribute == <tag>) {   
        inBody[0] = 'm';   
    }   
    return inBody.ToString();   
}   
</set-body>  
  
```  
  
#### <a name="filter-response-based-on-product"></a>Filtrowanie oparte na produktu odpowiedzi  
 W tym przykładzie pokazano, jak wykonać filtrowanie zawartości przez usunięcie elementów danych z odpowiedzi otrzymał od usługi wewnętrznej bazy danych, korzystając z `Starter` produktu. Aby demonstracyjne konfigurowania i korzystania z tych zasad, zobacz [177 epizodu obejmują chmury: więcej funkcji zarządzania interfejsu API z Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) i szybkie przewijanie do przodu do 34:30. Rozpocznij od 31:50 wyświetlić przegląd [ciemny niebo prognozy interfejsu API](https://developer.forecast.io/) używane na potrzeby tego pokazu.  
  
```xml  
<!-- Copy this snippet into the outbound section to remove a number of data elements from the response received from the backend service based on the name of the api product -->  
<choose>  
  <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter"))">  
    <set-body>@{  
        var response = context.Response.Body.As<JObject>();  
        foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {  
          response.Property (key).Remove ();  
        }  
        return response.ToString();  
      }  
    </set-body>  
  </when>  
</choose>  
```  

### <a name="using-liquid-templates-with-set-body"></a>Za pomocą szablonów płynne o treści zestawu 
`set-body` Zasad może być skonfigurowana do używania [płynne](https://shopify.github.io/liquid/basics/introduction/) tworzenia szablonów języka aby transfom treści żądania lub odpowiedzi. Może to być bardzo efektywnym sposobem, aby całkowicie zmienić format wiadomości.

> [!IMPORTANT]
> Implementacja płynnych używane w `set-body` zasada jest skonfigurowana w trybie"C#". Jest to szczególnie ważne podczas wykonywania czynności takich jak filtrowania. Na przykład przy użyciu filtru daty wymaga użycia Pascal wielkości liter i C# Data formatowania, np.:
>
> {{body.foo.startDateTime| Data: "yyyyMMddTHH:mm:ddZ"}}

> [!IMPORTANT]
> Aby można było poprawnie powiązać treści XML przy użyciu szablonu płynne, użyj `set-header` zasad, aby ustawić typ zawartości do albo aplikacja/xml, tekstu/xml (lub dowolnego typu kończąc + xml), na treść JSON, musi to być application/json, tekst/json (lub dowolnego typu kończąc + json).

#### <a name="convert-json-to-soap-using-a-liquid-template"></a>Konwertuj JSON przy użyciu szablonu płynne SOAP
```xml
<set-body template="liquid">
    <soap:Envelope xmlns="http://tempuri.org/" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
        <soap:Body>
            <GetOpenOrders>
                <cust>{{body.getOpenOrders.cust}}</cust>
            </GetOpenOrders>
        </soap:Body>
    </soap:Envelope>
</set-body>
```

#### <a name="tranform-json-using-a-liquid-template"></a>Tranform ciągu JSON przy użyciu szablonu płynne
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|zestaw treści|Element główny. Zawiera treść lub wyrażeń, które zwraca treści.|Tak|  

### <a name="properties"></a>Właściwości  
  
|Nazwa|Opis|Wymagane|Domyślne|  
|----------|-----------------|--------------|-------------|  
|szablon|Pozwala zmienić tryb tworzenia szablonów zasad treści zestaw uruchamianego w. Obecnie jest to jedyna obsługiwana wartość:<br /><br />-płynne — Ustaw zasady treści będzie korzystać z aparatu płynne tworzenia szablonów |Nie|płynnych|  

Aby uzyskać dostęp do informacji na temat żądania i odpowiedzi, płynne szablonu można powiązać z obiekt kontekstu z następującymi właściwościami: <br />
<pre>context.
    Request.
        Url
        Method
        OriginalMethod
        OriginalUrl
        IpAddress
        MatchedParameters
        HasBody
        ClientCertificates
        Headers

    Response.
        StatusCode
        Method
        Headers
Url.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString

OriginalUrl.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString
</pre>



### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** przychodzący, wychodzący wewnętrznej bazy danych  
  
-   **Zakresy zasad:** globalnych produktu interfejsu API, operacji  
  
##  <a name="SetHTTPheader"></a>Set — nagłówek HTTP  
 `set-header` Zasad przypisuje wartość do istniejących odpowiedzi i/lub nagłówek żądania lub dodaje nowy nagłówek odpowiedzi i/lub żądania.  
  
 Wstawia listę nagłówków HTTP do wiadomości HTTP. Po umieszczeniu w potoku dla ruchu przychodzącego, ta zasada ustawia nagłówki HTTP dla żądania przekazywany do usługi docelowej. Po umieszczeniu w potoku wychodzącego, ta zasada ustawia nagłówki HTTP są wysyłane do klienta bramy odpowiedzi.  
  
### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<set-header name="header name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple headers with the same name add additional value elements-->  
</set-header>  
```  
  
### <a name="examples"></a>Przykłady  
  
#### <a name="example"></a>Przykład  
  
```xml  
<set-header name="some header name" exists-action="override">  
    <value>20</value>   
</set-header>  
```  
  
#### <a name="forward-context-information-to-the-backend-service"></a>Informacje o kontekście do przodu do usługi zaplecza  
 W tym przykładzie przedstawiono sposób zastosowania zasad na poziomie interfejsu API, aby podać informacje o kontekście usługi wewnętrznej bazy danych. Aby demonstracyjne konfigurowania i korzystania z tych zasad, zobacz [177 epizodu obejmują chmury: więcej funkcji zarządzania interfejsu API z Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) i szybkie przewijanie do przodu do 10:30. Na 12:10 istnieje pokaz wywołanie operacji w portalu dla deweloperów, w którym można zobaczyć zasad w miejscu pracy.  
  
```xml  
<!-- Copy this snippet into the inbound element to forward some context information, user id and the region the gateway is hosted in, to the backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 Aby uzyskać więcej informacji, zobacz [wyrażenie zasad](api-management-policy-expressions.md) i [zmiennej kontekstu](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|set — nagłówek|Element główny.|Tak|  
|wartość|Określa wartość nagłówka do ustawienia. Do wiele nagłówków o takiej samej nazwie Dodaj dodatkowe `value` elementy.|Tak|  
  
### <a name="properties"></a>Właściwości  
  
|Nazwa|Opis|Wymagane|Domyślne|  
|----------|-----------------|--------------|-------------|  
|istnieje akcja|Określa, jakie działania w sytuacji, gdy nagłówek został już określony. Ten atrybut musi mieć jedną z następujących wartości.<br /><br /> -override - zastępuje wartość istniejący nagłówek.<br />-skip — nie zastępuje istniejącą wartość nagłówka.<br />-dołączania - dołącza wartość do istniejącej wartości nagłówka.<br />-Usunięcie — usuwa nagłówek z żądania.<br /><br /> Jeśli wartość `override` powoduje rejestrowanie wiele wpisów o takiej samej nazwie w nagłówku ustawiany zgodnie ze wszystkich zapisów (które zostaną wyświetlone wiele razy); zostanie ustawiona tylko wartości wyświetlane w wynikach.|Nie|zastąpienie|  
|name|Określa nazwę nagłówka do ustawienia.|Tak|Nie dotyczy|  
  
### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** przychodzący, wychodzący wewnętrznej bazy danych, na błąd  
  
-   **Zakresy zasad:** globalnych produktu interfejsu API, operacji  
  
##  <a name="SetQueryStringParameter"></a>Parametr ciągu zapytania dla zestawu  
 `set-query-parameter` Zasad dodaje wartość zastępuje, lub usuwa żądania parametru ciągu zapytania. Może służyć do przekazywania kwerendy parametry oczekiwany przez usługi wewnętrznej bazy danych, które są opcjonalne lub nigdy nie są obecne w żądaniu.  
  
### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<set-query-parameter name="param name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple parameters with the same name add additional value elements-->  
</set-query-parameter>  
```  
  
### <a name="examples"></a>Przykłady  
  
#### <a name="example"></a>Przykład  
  
```xml  
  
<set-query-parameter>  
  <parameter name="api-key" exists-action="skip">  
    <value>12345678901</value>  
  </parameter>  
  <!-- for multiple parameters with the same name add additional value elements -->  
</set-query-parameter>  
  
```  
  
#### <a name="forward-context-information-to-the-backend-service"></a>Informacje o kontekście do przodu do usługi zaplecza  
 W tym przykładzie przedstawiono sposób zastosowania zasad na poziomie interfejsu API, aby podać informacje o kontekście usługi wewnętrznej bazy danych. Aby demonstracyjne konfigurowania i korzystania z tych zasad, zobacz [177 epizodu obejmują chmury: więcej funkcji zarządzania interfejsu API z Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) i szybkie przewijanie do przodu do 10:30. Na 12:10 istnieje pokaz wywołanie operacji w portalu dla deweloperów, w którym można zobaczyć zasad w miejscu pracy.  
  
```xml  
<!-- Copy this snippet into the inbound element to forward a piece of context, product name in this example, to the backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 Aby uzyskać więcej informacji, zobacz [wyrażenie zasad](api-management-policy-expressions.md) i [zmiennej kontekstu](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|Parametr zestawu — zapytania|Element główny.|Tak|  
|wartość|Określa wartość parametru zapytania do ustawienia. Dla wielu parametrów zapytania o takiej samej nazwie dodanie dodatkowych `value` elementów.|Tak|  
  
### <a name="properties"></a>Właściwości  
  
|Nazwa|Opis|Wymagane|Domyślne|  
|----------|-----------------|--------------|-------------|  
|istnieje akcja|Określa, jakie działania w sytuacji, gdy parametr zapytania został już określony. Ten atrybut musi mieć jedną z następujących wartości.<br /><br /> -override - zastępuje wartość parametru istniejących.<br />-skip — nie zastępuje istniejącą wartość parametru zapytania.<br />-dołączania - dołącza wartość do istniejącej wartości parametru zapytania.<br />-Usunięcie — usuwa parametr zapytania z żądania.<br /><br /> Jeśli wartość `override` powoduje rejestrowanie wiele wpisów z tą samą nazwą parametru zapytania ustawiany zgodnie ze wszystkich zapisów (które zostaną wyświetlone wiele razy); zostanie ustawiona tylko wartości wyświetlane w wynikach.|Nie|zastąpienie|  
|name|Określa nazwę parametru zapytania do ustawienia.|Tak|Nie dotyczy|  
  
### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** wewnętrznej bazy danych dla ruchu przychodzącego  
  
-   **Zakresy zasad:** globalnych produktu interfejsu API, operacji  
  
##  <a name="RewriteURL"></a>Ponowne zapisywanie adresów URL  
 `rewrite-uri` Zasad konwertuje adresie URL żądania postaci publicznego formularza oczekiwane przez usługę sieci web, jak pokazano w poniższym przykładzie.  
  
-   Publiczny adres URL-`http://api.example.com/storenumber/ordernumber`  
  
-   Adres URL żądania —`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`  
  
 Ta zasada służy podczas transformacji człowieka i/lub przeglądarki przyjaznego adresu URL do formatu URL oczekiwanego przez usługę sieci web. Ta zasada musi tylko ma być stosowany podczas udostępnianie innego formatu adresu URL, takie jak czystą adresów URL, adresy URL RESTful, adresów URL przyjaznych dla użytkownika lub adresów URL przyjaznych dla aparatów wyszukiwania, które są całkowicie strukturalnych adresów URL, które nie zawierają ciągu zapytania i zamiast tego zawierają tylko ścieżki zasobu (po schemat i Urząd). Często jest to estetycznych, użyteczność lub aparat wyszukiwania (SEO) optymalizacji.  
  
> [!NOTE]
>  Można dodawać tylko przy użyciu zasad parametrów ciągu zapytania. Nie można dodać szablon dodatkowe parametry ścieżki w adresie URL ponownego zapisywania.  

### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />  
```  
  
### <a name="example"></a>Przykład  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/v2/US/hardware/{storenumber}&{ordernumber}?City=city&State=state" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/put" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
<!-- Resulting URL will be /put?c=d -->
```  
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/put" copy-unmatched-params="false" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
<!-- Resulting URL will be /put -->
```

### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|Identyfikator uri ponownego napisania|Element główny.|Tak|  
  
### <a name="attributes"></a>Atrybuty  
  
|Atrybut|Opis|Wymagane|Domyślne|  
|---------------|-----------------|--------------|-------------|  
|szablon|Adres URL usługi sieci web rzeczywiste wszystkie parametry ciągu zapytania. Używając wyrażenia wartości całkowitej musi być wyrażeniem.|Tak|Nie dotyczy|  
|Kopiuj niedopasowane — parametry|Określa, czy parametry zapytania w żądaniu przychodzącym nie znajduje się w oryginalnym szablonie adres URL są dodawane do adresu URL zdefiniowanej w szablonie Napisz ponownie|Nie|Wartość true|  
  
### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** dla ruchu przychodzącego  
  
-   **Zakresy zasad:** operacja produktu, interfejsu API,  
  
##  <a name="XSLTransform"></a>Przekształcanie XML za pomocą XSLT  
 `Transform XML using an XSLT` Zasady dotyczą transformacji XSL XML w treści żądania lub odpowiedzi.  
  
### <a name="policy-statement"></a>Deklaracja zasad  
  
```xml  
<xsl-transform>  
    <parameter name="User-Agent">@(context.Request.Headers.GetValueOrDefault("User-Agent","non-specified"))</parameter>  
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
        <xsl:output method="xml" indent="yes" />  
        <xsl:param name="User-Agent" />  
        <xsl:template match="* | @* | node()">  
            <xsl:copy>  
                <xsl:if test="self::* and not(parent::*)">  
                    <xsl:attribute name="User-Agent">  
                        <xsl:value-of select="$User-Agent" />  
                    </xsl:attribute>  
                </xsl:if>  
                <xsl:apply-templates select="* | @* | node()" />  
            </xsl:copy>  
        </xsl:template>  
    </xsl:stylesheet>  
  </xsl-transform>  
```  
  
### <a name="example"></a>Przykład  
  
```xml  
<policies>  
  <inbound>  
      <base />  
  </inbound>  
  <outbound>  
      <base />  
      <xsl-transform>  
        <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
            <xsl:output omit-xml-declaration="yes" method="xml" indent="yes" />  
            <!-- Copy all nodes directly-->  
            <xsl:template match="node()| @*|*">  
                <xsl:copy>  
                    <xsl:apply-templates select="@* | node()|*" />  
                </xsl:copy>  
            </xsl:template>  
        </xsl:stylesheet>  
    </xsl-transform>  
  </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elementy  
  
|Nazwa|Opis|Wymagane|  
|----------|-----------------|--------------|  
|Transformacja XSL|Element główny.|Tak|  
|Parametr|Używane do definiowania zmiennych w przekształceniu|Nie|  
|XSL: stylesheet|Elemencie głównym arkusza stylów. Wszystkie elementy i atrybuty zdefiniowane w ramach zgodne ze standardem [specyfikacji XSLT](http://www.w3.org/TR/xslt)|Tak|  
  
### <a name="usage"></a>Sposób użycia  
 Te zasady służą następujące zasady [sekcje](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) i [zakresy](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Sekcje zasad:** przychodzące, wychodzące  
  
-   **Zakresy zasad:** globalnych produktu interfejsu API, operacji  
  
## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji, Praca z zasad, zobacz [zasad w usłudze API Management](api-management-howto-policies.md).  
