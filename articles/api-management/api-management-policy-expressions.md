---
title: "Wyrażenie zasad interfejsu API zarządzania platformy Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat wyrażeń zasad w usłudze Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 33bcc51466fa0918bf4484c58fac813d07ae14da
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="api-management-policy-expressions"></a>Wyrażenie zasad interfejsu API zarządzania
Zasady składni wyrażeń jest C# w wersji 6.0. Każde wyrażenie ma dostęp do udostępnionego niejawnie [kontekstu](api-management-policy-expressions.md#ContextVariables) zmienną i dozwolonych [podzestawu](api-management-policy-expressions.md#CLRTypes) typów .NET Framework.  
  
> [!NOTE]
>  Aby uzyskać więcej informacji na temat wyrażenia zasad, zobacz [wyrażenie zasad](https://azure.microsoft.com/documentation/videos/policy-expressions-in-azure-api-management/) wideo.  
>   
>  Aby pokazów konfigurowania zasad przy użyciu wyrażenia zasad, zobacz [177 epizodu obejmują chmury: więcej funkcji zarządzania interfejsu API z Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Ten film zawiera następujące pokazów wyrażenie zasad.  
>   
>  -   10:30 — w temacie jak stosowanie zasad na poziomie interfejsu API, aby podać informacje o kontekście przy użyciu usługi wewnętrznej bazy danych [ustawić parametr ciągu zapytania](api-management-transformation-policies.md#SetQueryStringParameter) i [nagłówka HTTP ustawić](api-management-transformation-policies.md#SetHTTPheader) zasad. Na 12:10 istnieje pokaz wywołanie operacji w portalu dla deweloperów, w którym można zobaczyć te zasady w miejscu pracy.  
> -   13:50 — zobacz temat jak korzystać [JWT do zweryfikowania](api-management-access-restriction-policies.md#ValidateJWT) zasad do wstępnie autoryzacji dostępu do operacji na podstawie tokenu oświadczeń. Szybko przewiń do przodu do 15:00, aby wyświetlić zasady skonfigurowane w edytorze zasad, a następnie do 18:50 dla pokaz wywołanie operacji z portalu dla deweloperów zarówno z i bez tokenu autoryzacji wymagane.  
> -   Sposób użycia Zobacz 21:00 - [inspektora interfejsu API](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) śledzenia, aby zobaczyć, jak są analizowane zasady i wyniki oceny.  
> -   25:25 — jak używać zasad wyrażenia z [pobrać z pamięci podręcznej](api-management-caching-policies.md#GetFromCache) i [magazynu pamięci podręcznej](api-management-caching-policies.md#StoreToCache) zasad, aby skonfigurować zarządzanie interfejsami API odpowiedzi buforowanie czasu trwania odpowiadającego buforowanie odpowiedzi wewnętrznej bazy danych usługi określone przez usługę kopii `Cache-Control` dyrektywy.  
> -   34:30 — Zobacz, jak wykonać filtrowanie zawartości przez usunięcie elementów danych z odpowiedzi otrzymanych z usługi wewnętrznej bazy danych przy użyciu [sterowania przepływem](api-management-advanced-policies.md#choose) i [ustawić treść](api-management-transformation-policies.md#SetBody) zasad. Rozpocznij od 31:50 wyświetlić przegląd [ciemny niebo prognozy interfejsu API](https://developer.forecast.io/) używane na potrzeby tego pokazu.  
> -   Aby pobrać deklaracji zasad, używane w tym wideo, zobacz [interfejsu api zarządzania — przykłady/zasadami](https://github.com/Azure/api-management-samples/tree/master/policies) repozytorium github.  
  
  
##  <a name="Syntax"></a>Składnia  
 Pojedyncza instrukcja wyrażenia są ujęte w `@(expression)`, gdzie `expression` jest poprawnie sformułowanym instrukcji wyrażenia języka C#.  
  
 Wyrażenia instrukcji wielu są ujęte w `@{expression}`. Wszystkie ścieżki kodu w wielu instrukcji wyrażenia musi być zakończona odpowiadającą `return` instrukcji.  
  
##  <a name="PolicyExpressionsExamples"></a>Przykłady  
  
```  
@(true)  
  
@((1+1).ToString())  
  
@("Hi There".Length)  
  
@(Regex.Match(context.Response.Headers.GetValueOrDefault("Cache-Control",""), @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value)  
  
@(context.Variables.ContainsKey("maxAge") ? int.Parse((string)context.Variables["maxAge"]) : 3600)  
  
@{   
  string value;   
  if (context.Request.Headers.TryGetValue("Authorization", out value))   
  {   
    return Encoding.UTF8.GetString(Convert.FromBase64String(value));  
  }   
  else   
  {   
    return null;  
  }  
}  
```  
  
##  <a name="PolicyExpressionsUsage"></a>Użycie  
 Wyrażenia mogą być używane jako wartości atrybutu lub tekst w żadnym z interfejsu API zarządzania [zasady](api-management-policies.md), chyba że informacje o zasadach Określa, w przeciwnym razie wartość.  
  
> [!IMPORTANT]
>  Należy pamiętać, że użycie wyrażenia zasad, tylko ograniczone weryfikacji wyrażenia zasad po zdefiniowaniu zasad. Ponieważ wyrażenia są wykonywane w czasie wykonywania w potoku ruchu przychodzącego lub wychodzącego przez bramę, wszelkie wyjątki czasu wykonywania wygenerowanych przez wyrażenie zasad spowoduje błąd w czasie wykonywania w wywołaniu interfejsu API.  
  
##  <a name="CLRTypes"></a>Typy .NET framework dozwolone w wyrażeniach zasad  
 Poniższa tabela zawiera listę typów .NET Framework i ich elementy członkowskie, które są dozwolone w wyrażeniach zasad.  
  
|Typ CLR|Obsługiwane metody|  
|--------------|-----------------------|  
|Newtonsoft.Json.Linq.Extensions|Obsługiwane są wszystkie metody|  
|Newtonsoft.Json.Linq.JArray|Obsługiwane są wszystkie metody|  
|Newtonsoft.Json.Linq.JConstructor|Obsługiwane są wszystkie metody|  
|Newtonsoft.Json.Linq.JContainer|Obsługiwane są wszystkie metody|  
|Newtonsoft.Json.Linq.JObject|Obsługiwane są wszystkie metody|  
|Newtonsoft.Json.Linq.JProperty|Obsługiwane są wszystkie metody|  
|Newtonsoft.Json.Linq.JRaw|Obsługiwane są wszystkie metody|  
|Newtonsoft.Json.Linq.JToken|Obsługiwane są wszystkie metody|  
|Newtonsoft.Json.Linq.JTokenType|Obsługiwane są wszystkie metody|  
|Newtonsoft.Json.Linq.JValue|Obsługiwane są wszystkie metody|  
|System.Collections.Generic.IReadOnlyCollection < T\>|Wszystkie|  
|System.Collections.Generic.IReadOnlyDictionary < TKey, TValue >|Wszystkie|  
|System.Collections.Generic.ISet < TKey, TValue >|Wszystkie|  
|System.Collections.Generic.KeyValuePair < TKey, TValue >|Klucz-wartość|  
|System.Collections.Generic.List < TKey, TValue >|Wszystkie|  
|System.Collections.Generic.Queue < TKey, TValue >|Wszystkie|  
|System.Collections.Generic.Stack < TKey, TValue >|Wszystkie|  
|System.Convert|Wszystkie|  
|System.DateTime|Wszystkie|  
|System.DateTimeKind|Czas UTC|  
|System.DateTimeOffset|Wszystkie|  
|System.Decimal|Wszystkie|  
|System.Double|Wszystkie|  
|System.Guid|Wszystkie|  
|System.IEnumerable < T\>|Wszystkie|  
|System.IEnumerator < T\>|Wszystkie|  
|System.Int16|Wszystkie|  
|System.Int32|Wszystkie|  
|System.Int64|Wszystkie|  
|System.Linq.Enumerable < T\>|Obsługiwane są wszystkie metody|  
|System.Math|Wszystkie|  
|System.MidpointRounding|Wszystkie|  
|System.Nullable < T\>|Wszystkie|  
|System.Random|Wszystkie|  
|System.SByte|Wszystkie|  
|System.Security.Cryptography. HMACSHA384|Wszystkie|  
|System.Security.Cryptography. HMACSHA512|Wszystkie|  
|System.Security.Cryptography.HashAlgorithm|Wszystkie|  
|System.Security.Cryptography.HMAC|Wszystkie|  
|System.Security.Cryptography.HMACMD5|Wszystkie|  
|System.Security.Cryptography.HMACSHA1|Wszystkie|  
|System.Security.Cryptography.HMACSHA256|Wszystkie|  
|System.Security.Cryptography.KeyedHashAlgorithm|Wszystkie|  
|System.Security.Cryptography.MD5|Wszystkie|  
|System.Security.Cryptography.RNGCryptoServiceProvider|Wszystkie|  
|System.Security.Cryptography.SHA1|Wszystkie|  
|System.Security.Cryptography.SHA1Managed|Wszystkie|  
|System.Security.Cryptography.SHA256|Wszystkie|  
|System.Security.Cryptography.SHA256Managed|Wszystkie|  
|System.Security.Cryptography.SHA384|Wszystkie|  
|System.Security.Cryptography.SHA384Managed|Wszystkie|  
|System.Security.Cryptography.SHA512|Wszystkie|  
|System.Security.Cryptography.SHA512Managed|Wszystkie|  
|System.Single|Wszystkie|  
|System.String|Wszystkie|  
|System.StringSplitOptions|Wszystkie|  
|System.Text.Encoding|Wszystkie|  
|System.Text.RegularExpressions.Capture|Wartość indeksu, długość,|  
|System.Text.RegularExpressions.CaptureCollection|Liczba elementów|  
|System.Text.RegularExpressions.Group|Przechwytywanie, Powodzenie|  
|System.Text.RegularExpressions.GroupCollection|Liczba elementów|  
|System.Text.RegularExpressions.Match|Puste, grup, wynik|  
|Obiektu System.Text.RegularExpressions.Regex|element .ctor, IsMatch, zgodne, zgodne, Zamień|  
|System.Text.RegularExpressions.RegexOptions|Skompilowany IgnoreCase, IgnorePatternWhitespace, wielowierszowego, None, RightToLeft, Singleline|  
|Obiekt System.TimeSpan|Wszystkie|  
|System.Tuple|Wszystkie|  
|System.UInt16|Wszystkie|  
|System.UInt32|Wszystkie|  
|System.UInt64|Wszystkie|  
|System.Uri|Wszystkie|  
|System.Xml.Linq.Extensions|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XAttribute|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XCData|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XComment|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XContainer|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XDeclaration|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XDocument|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XDocumentType|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XElement|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XName|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XNamespace|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XNode|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XNodeDocumentOrderComparer|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XNodeEqualityComparer|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XObject|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XProcessingInstruction|Obsługiwane są wszystkie metody|  
|System.Xml.Linq.XText|Obsługiwane są wszystkie metody|  
|System.Xml.XmlNodeType|Wszystkie|  
  
##  <a name="ContextVariables"></a>Zmienna kontekstu  
 Zmiennej o nazwie `context` jest niejawnie dostępne we wszystkich zasadach [wyrażenia](api-management-policy-expressions.md#Syntax). Jej elementów członkowskich zawierają informacje dotyczące `\request`. Wszystkie `context` elementy członkowskie są tylko do odczytu.  
  
|Zmienna kontekstu|Dozwolone metody, właściwości i wartości parametrów|  
|----------------------|-------------------------------------------------------|  
|Kontekst|Interfejs API: IApi<br /><br /> Wdrożenie<br /><br /> LastError<br /><br /> Operacja<br /><br /> Product (Produkt)<br /><br /> Żądanie<br /><br /> Identyfikator żądania: Identyfikator Guid<br /><br /> Odpowiedź<br /><br /> Subskrypcja<br /><br /> Śledzenie: wartość logiczna<br /><br /> Użytkownik<br /><br /> Zmienne: IReadOnlyDictionary < string, object ><br /><br /> void Trace(message: string)|  
|kontekst. Interfejs API|Identyfikator: ciąg<br /><br /> Nazwa: ciąg<br /><br /> Ścieżka: ciąg<br /><br /> ServiceUrl: IUrl|  
|kontekst. Wdrożenia|Region: ciąg<br /><br /> ServiceName: ciąg|  
|kontekst. LastError|Źródło: ciąg<br /><br /> Przyczyna: ciąg<br /><br /> Komunikat: ciąg<br /><br /> Zakres: ciąg<br /><br /> Sekcja: ciąg<br /><br /> Ścieżka: ciąg<br /><br /> PolicyId: ciąg<br /><br /> Aby uzyskać więcej informacji o kontekście. LastError, zobacz [obsługi błędów](api-management-error-handling-policies.md).|  
|kontekst. Operacja|Identyfikator: ciąg<br /><br /> Metoda: ciąg<br /><br /> Nazwa: ciąg<br /><br /> UrlTemplate: ciąg|  
|kontekst. Produktu|Interfejsy API: IEnumerable < IApi\><br /><br /> ApprovalRequired: wartość logiczna<br /><br /> Grupy: IEnumerable < IGroup\><br /><br /> Identyfikator: ciąg<br /><br /> Nazwa: ciąg<br /><br /> Stan: wyliczenia ProductState {NotPublished, opublikowaną}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: wartość logiczna|  
|kontekst. Żądanie|Treść: IMessageBody<br /><br /> Certyfikat: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> Nagłówki: IReadOnlyDictionary < string, string [] ><br /><br /> Adres IP: ciąg<br /><br /> MatchedParameters: IReadOnlyDictionary < ciąg, ciąg ><br /><br /> Metoda: ciąg<br /><br /> OriginalUrl:IUrl<br /><br /> Adres URL: IUrl|  
|kontekst ciągu. Request.Headers.GetValueOrDefault (headerName: ciąg, wartość domyślna: ciąg)|headerName: ciąg<br /><br /> Wartość domyślna: ciąg<br /><br /> Zwraca przecinkami wartości nagłówka żądania lub `defaultValue` Jeżeli nie znaleziono nagłówka.|  
|kontekst. Odpowiedź|Treść: IMessageBody<br /><br /> Nagłówki: IReadOnlyDictionary < string, string [] ><br /><br /> StatusCode: int<br /><br /> StatusReason: ciąg|  
|kontekst ciągu. Response.Headers.GetValueOrDefault (headerName: ciąg, wartość domyślna: ciąg)|headerName: ciąg<br /><br /> Wartość domyślna: ciąg<br /><br /> Zwraca wartości nagłówka odpowiedzi przecinkami lub `defaultValue` Jeżeli nie znaleziono nagłówka.|  
|kontekst. Subskrypcji|CreatedTime: daty i godziny<br /><br /> EndDate: DateTime?<br /><br /> Identyfikator: ciąg<br /><br /> Klucz: ciąg<br /><br /> Nazwa: ciąg<br /><br /> PrimaryKey: ciąg<br /><br /> Klucz pomocniczy: ciąg<br /><br /> Datą rozpoczęcia: DateTime?|  
|kontekst. Użytkownika|Wiadomości e-mail: ciąg<br /><br /> Imię: ciąg<br /><br /> Grupy: IEnumerable < IGroup\><br /><br /> Identyfikator: ciąg<br /><br /> Tożsamości: IEnumerable < IUserIdentity\><br /><br /> Nazwisko: ciąg<br /><br /> Uwaga: ciąg<br /><br /> RegistrationDate: daty i godziny|  
|IApi|Identyfikator: ciąg<br /><br /> Nazwa: ciąg<br /><br /> Ścieżka: ciąg<br /><br /> Protokoły: IEnumerable < ciąg\><br /><br /> ServiceUrl: IUrl<br /><br /> SubscriptionKeyParameterNames: ISubscriptionKeyParameterNames|  
|IGroup|Identyfikator: ciąg<br /><br /> Nazwa: ciąg|  
|IMessageBody|Jako < T\>(preserveContent: bool = false): gdy T: ciągu JObject, JToken, JArray, XNode klasy XElement, XDocument<br /><br /> `context.Request.Body.As<T>` i `context.Response.Body.As<T>` metody są używane do odczytu treści wiadomości w określonym typie żądań i odpowiedzi `T`. Domyślnie metoda korzysta z oryginalnego strumień treści komunikatu i reneders niedostępne po zwraca go. Aby tego uniknąć, ponieważ działają na kopii w strumieniu treści metody, należy ustawić `preserveContent` parametr `true`. Przejdź [tutaj](api-management-transformation-policies.md#SetBody) na przykład.|  
|IUrl|Host: ciąg<br /><br /> Ścieżka: ciąg<br /><br /> Port: int<br /><br /> Zapytań: IReadOnlyDictionary < string, string [] ><br /><br /> Ciąg zapytania: ciąg<br /><br /> Schemat: ciąg|  
|IUserIdentity|Identyfikator: ciąg<br /><br /> Dostawca: ciąg|  
|ISubscriptionKeyParameterNames|Nagłówek: ciąg<br /><br /> Zapytania: ciąg|  
|ciąg IUrl.Query.GetValueOrDefault (queryParameterName: ciąg, wartość domyślna: ciąg)|queryParameterName: ciąg<br /><br /> Wartość domyślna: ciąg<br /><br /> Zwraca przecinkami wartości parametrów zapytania lub `defaultValue` Jeśli parametr nie zostanie znaleziony.|  
|Kontekst T. Variables.GetValueOrDefault < T\>(nazwa_zmiennej: ciąg, wartość domyślna: T)|nazwa_zmiennej: ciąg<br /><br /> Wartość domyślna: T<br /><br /> Zwraca wartość zmiennej rzutowany na typ `T` lub `defaultValue` Jeśli zmienna nie zostanie znaleziony.<br /><br /> Ta metoda zgłasza wyjątek, jeśli określony typ nie jest zgodny z rzeczywisty typ zwrócony zmiennej.|  
|BasicAuthCredentials AsBasic(input: this string)|wprowadzania: ciąg<br /><br /> Jeśli parametr wejściowy zawiera prawidłową wartość nagłówka żądania uwierzytelnienia podstawowego uwierzytelniania HTTP, metoda zwraca obiekt typu `BasicAuthCredentials`; w przeciwnym razie metoda zwraca wartość null.|  
|wartość logiczna TryParseBasic (dane wejściowe: ten ciąg, wynik: limit BasicAuthCredentials)|wprowadzania: ciąg<br /><br /> wynik: limit BasicAuthCredentials<br /><br /> Jeśli parametr wejściowy zawiera prawidłową wartość nagłówka żądania uwierzytelnienia podstawowego uwierzytelniania HTTP, metoda zwraca `true` i parametr wynik zawiera wartości typu `BasicAuthCredentials`; w przeciwnym razie metoda zwraca `false`.|  
|BasicAuthCredentials|Hasło: ciąg<br /><br /> Nazwa użytkownika: ciąg|  
|Jwt AsJwt(input: this string)|wprowadzania: ciąg<br /><br /> Jeśli parametr wejściowy zawiera prawidłową wartość tokenu JWT, metoda zwraca obiekt typu `Jwt`; w przeciwnym razie metoda zwraca `null`.|  
|bool TryParseJwt (dane wejściowe: ten ciąg, wynik: limit Jwt)|wprowadzania: ciąg<br /><br /> wynik: limit Jwt<br /><br /> Jeśli parametr wejściowy zawiera prawidłową wartość tokenu JWT, metoda zwraca `true` i parametr wynik zawiera wartości typu `Jwt`; w przeciwnym razie metoda zwraca `false`.|  
|Token Jwt|Algorytm: ciąg<br /><br /> Grupy odbiorców: IEnumerable < ciąg\><br /><br /> Oświadczenia: IReadOnlyDictionary < string, string [] ><br /><br /> ExpirationTime: DateTime?<br /><br /> Identyfikator: ciąg<br /><br /> Wystawca: ciąg<br /><br /> Nie wcześniej niż: DateTime?<br /><br /> Podmiot: ciąg<br /><br /> Typ: ciąg|  
|ciąg Jwt.Claims.GetValueOrDefault (claimName: ciąg, wartość domyślna: ciąg)|claimName: ciąg<br /><br /> Wartość domyślna: ciąg<br /><br /> Zwraca wartości oświadczeń przecinkami lub `defaultValue` Jeżeli nie znaleziono nagłówka.|

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji, Praca z zasad, zobacz [zasad w usłudze API Management](api-management-howto-policies.md).  
