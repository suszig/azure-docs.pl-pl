---
title: "Odwołanie do modelu danych szablonu usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat jednostki i typu oświadczenia dla najczęściej używane w modelach danych szablonów portalu deweloperów w usłudze Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 72936a4d38f809934ddea74e5ae4a6029450a97c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-api-management-template-data-model-reference"></a>Odwołanie modelu danych Azure API Management szablonu
W tym temacie opisano jednostki i typu oświadczenia dla najczęściej używane w modelach danych szablonów portalu deweloperów w usłudze Azure API Management.  
  
 Aby uzyskać więcej informacji na temat pracy z szablonami, zobacz [dostosowywaniu portalu dla deweloperów interfejsu API zarządzania za pomocą szablonów](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
-   [Interfejs API](#API)  
-   [Podsumowanie interfejsu API](#APISummary)  
-   [Aplikacji](#Application)  
-   [Załącznika](#Attachment)  
-   [Przykładowy kod](#Sample)  
-   [Komentarz](#Comment)  
-   [Filtrowanie](#Filtering)  
-   [Nagłówek](#Header)  
-   [Żądania HTTP](#HTTPRequest)  
-   [Odpowiedź HTTP](#HTTPResponse)  
-   [Problem](#Issue)  
-   [Operacja](#Operation)  
-   [Operacja menu](#Menu)  
-   [Operacja elementu menu](#MenuItem)  
-   [Stronicowania](#Paging)  
-   [Parametr](#Parameter)  
-   [Produktu](#Product)  
-   [Dostawca](#Provider)  
-   [Reprezentacja wartości](#Representation)  
-   [Subskrypcja](#Subscription)  
-   [Podsumowanie subskrypcji](#SubscriptionSummary)  
-   [Informacje o koncie użytkownika](#UserAccountInfo)  
-   [Logowanie użytkownika](#UseSignIn)  
-   [Tworzenia konta użytkownika](#UserSignUp)  
  
##  <a name="API"></a>INTERFEJS API  
 `API` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|id|Ciąg|Identyfikator zasobu. Identyfikuje interfejsu API w ramach bieżącego wystąpienia usługi Zarządzanie interfejsami API. Wartość jest nieprawidłowa względny adres URL w formacie `apis/{id}` gdzie `{id}` jest identyfikatorem interfejsu API. Ta właściwość jest tylko do odczytu.|  
|name|Ciąg|Nazwa interfejsu API. Nie może być pusta. Maksymalna długość wynosi 100 znaków.|  
|description|Ciąg|Opis interfejsu API. Nie może być pusta. Może obejmować formatowanie tagów HTML. Maksymalna długość wynosi 1000 znaków.|  
|serviceUrl|Ciąg|Bezwzględny adres URL usługi wewnętrznej bazy danych, wdrażanie tego interfejsu API.|  
|Ścieżka|Ciąg|Względny adres URL, który unikatowo identyfikuje ten interfejs API i wszystkie jego ścieżki zasobu w wystąpieniu usługi Zarządzanie interfejsami API. Jest on dołączany do interfejsu API punktu końcowego podstawowy adres URL określony podczas tworzenia wystąpienia usługi do utworzenia publiczny adres URL dla tego interfejsu API.|  
|protokoły|Tablica numer|Opisuje, na które protokoły można wywołać operacji w tym interfejsie API. Dozwolone wartości to `1 - http` i `2 - https`, lub obie.|  
|authenticationSettings|[Ustawienia uwierzytelniania serwera autoryzacji](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Kolekcja ustawień uwierzytelniania zawarte w tym interfejsie API.|  
|subscriptionKeyParameterNames|Obiekt|Opcjonalna właściwość, który może służyć do określenia nazwy niestandardowych parametrów zapytania lub nagłówek zawierający klucz subskrypcji. Jeśli ta właściwość ma musi zawierać co najmniej jeden z dwóch następujących właściwości.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a>Podsumowanie interfejsu API  
 `API summary` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|id|Ciąg|Identyfikator zasobu. Identyfikuje interfejsu API w ramach bieżącego wystąpienia usługi Zarządzanie interfejsami API. Wartość jest nieprawidłowa względny adres URL w formacie `apis/{id}` gdzie `{id}` jest identyfikatorem interfejsu API. Ta właściwość jest tylko do odczytu.|  
|name|Ciąg|Nazwa interfejsu API. Nie może być pusta. Maksymalna długość wynosi 100 znaków.|  
|description|Ciąg|Opis interfejsu API. Nie może być pusta. Może obejmować formatowanie tagów HTML. Maksymalna długość wynosi 1000 znaków.|  
  
##  <a name="Application"></a>Aplikacji  
 `application` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Identyfikator|Ciąg|Unikatowy identyfikator aplikacji.|  
|Tytuł|Ciąg|Tytuł aplikacji.|  
|Opis|Ciąg|Opis aplikacji.|  
|Url|IDENTYFIKATOR URI|Identyfikator URI dla aplikacji.|  
|Wersja|Ciąg|Informacje o wersji dla aplikacji.|  
|Wymagania|Ciąg|Opis wymagań dotyczących aplikacji.|  
|Stan|Numer|Bieżący stan aplikacji.<br /><br /> -0 - zarejestrowany<br /><br /> -1 - przesłane<br /><br /> -2 - opublikowane<br /><br /> -3 - odrzucone<br /><br /> -4 - nieopublikowane|  
|RegistrationDate|Data i godzina|Data i godzina, aplikacja została zarejestrowana.|  
|CategoryId|Numer|Do kategorii aplikacji (Finance, rozrywka itp.)|  
|DeveloperId|Ciąg|Unikatowy identyfikator dewelopera przesłania aplikacji.|  
|Załączniki|Kolekcja [załącznika](#Attachment) jednostek.|Wszystkie załączniki dla aplikacji, takie jak zrzuty ekranu lub ikony.|  
|Ikona|[Załącznika](#Attachment)|Ikona dla aplikacji.|  
  
##  <a name="Attachment"></a>Załącznika  
 `attachment` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Unikatowy identyfikator|Ciąg|Unikatowy identyfikator dla załącznika.|  
|Url|Ciąg|Adres URL zasobu.|  
|Typ|Ciąg|Typ załącznika.|  
|Typ zawartości|Ciąg|Typ nośnika załącznika.|  
  
##  <a name="Sample"></a>Przykładowy kod  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Tytuł|Ciąg|Nazwa operacji.|  
|fragment kodu|Ciąg|Ta właściwość jest przestarzała i nie powinna być używana.|  
|Pędzel|Ciąg|Składnia kodu kolorowania szablon ma być używany podczas wyświetlania przykładowy kod. Dozwolone wartości to `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, i `csharp`.|  
|szablon|Ciąg|Nazwa tego kod przykładowy szablon.|  
|Treści|Ciąg|Symbolu zastępczego dla części próbki kodu fragmentu.|  
|— Metoda|Ciąg|Metoda HTTP operacji.|  
|Schemat|Ciąg|Protokół do użycia dla żądania operacji.|  
|Ścieżka|Ciąg|Ścieżka operacji.|  
|query|Ciąg|Przykład zdefiniowanych parametrów ciągu zapytania.|  
|Host|Ciąg|Adres URL bramy usługi Zarządzanie interfejsami API dla interfejsu API, który zawiera tę operację.|  
|Nagłówki|Kolekcja [nagłówka](#Header) jednostek.|Nagłówki dla tej operacji.|  
|parameters|Kolekcja [parametru](#Parameter) jednostek.|Parametry, które są zdefiniowane dla tej operacji.|  
  
##  <a name="Comment"></a>Komentarz  
 `API` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Identyfikator|Numer|Identyfikator komentarza.|  
|Commenttext —|Ciąg|Treść komentarza. Może obejmować HTML.|  
|DeveloperCompany|Ciąg|Nazwa firmy dewelopera.|  
|PostedOn|Data i godzina|Data i godzina komentarz został opublikowany.|  
  
##  <a name="Issue"></a>Problem  
 `issue` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Identyfikator|Ciąg|Unikatowy identyfikator problemu.|  
|ApiID|Ciąg|Identyfikator dla interfejsu API, dla którego ten problem został zgłoszony.|  
|Tytuł|Ciąg|Tytuł problemu.|  
|Opis|Ciąg|Opis problemu.|  
|SubscriptionDeveloperName|Ciąg|Imię projektanta, który zgłosił problem.|  
|IssueState|Ciąg|Bieżący stan problemu. Możliwe wartości to proponowany, Opened, zamknięte.|  
|ReportedOn|Data i godzina|Data i godzina problem został zgłoszony.|  
|Komentarze|Kolekcja [komentarz](#Comment) jednostek.|Komentarze dotyczące tego problemu.|  
|Załączniki|Kolekcja [załącznika](api-management-template-data-model-reference.md#Attachment) jednostek.|Załączniki do problemu.|  
|Usługi|Kolekcja [interfejsu API](#API) jednostek.|Interfejsy API subskrybowana przez użytkownika, który zgłosić problem.|  
  
##  <a name="Filtering"></a>Filtrowanie  
 `filtering` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|wzorzec|Ciąg|Bieżący termin wyszukiwania; lub `null` Jeśli brak szukanego terminu.|  
|Symbol zastępczy|Ciąg|Tekst wyświetlany w polu wyszukiwania, gdy jest brak szukanego terminu określony.|  
  
##  <a name="Header"></a>Nagłówek  
 W tej sekcji opisano `parameter` reprezentacji.  
  
|Właściwość|Opis|Typ|  
|--------------|-----------------|----------|  
|name|Ciąg|Nazwa parametru.|  
|description|Ciąg|Opis parametru.|  
|wartość|Ciąg|Wartość nagłówka.|  
|Właściwość TypeName|Ciąg|Typ danych wartości nagłówka.|  
|Opcje|Ciąg|Opcje.|  
|Wymagane|Wartość logiczna|Określa, czy nagłówek jest wymagana.|  
|Tylko do odczytu|Wartość logiczna|Określa, czy nagłówek jest tylko do odczytu.|  
  
##  <a name="HTTPRequest"></a>Żądania HTTP  
 W tej sekcji opisano `request` reprezentacji.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|description|Ciąg|Opis żądania operacji.|  
|Nagłówki|Tablica [nagłówka](#Header) jednostek.|Nagłówki żądań.|  
|parameters|Tablica [parametru](#Parameter)|Kolekcja parametrów żądania operacji.|  
|oświadczenia|Tablica [reprezentacja](#Representation)|Kolekcja reprezentacje żądania operacji.|  
  
##  <a name="HTTPResponse"></a>Odpowiedź HTTP  
 W tej sekcji opisano `response` reprezentacji.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|statusCode|Dodatnia liczba całkowita|Kod stanu odpowiedzi operacji.|  
|description|Ciąg|Opis odpowiedzi operacji.|  
|oświadczenia|Tablica [reprezentacja](#Representation)|Kolekcja reprezentacje odpowiedzi operacji.|  
  
##  <a name="Operation"></a>Operacja  
 `operation` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|id|Ciąg|Identyfikator zasobu. Identyfikuje operacji w ramach bieżącego wystąpienia usługi Zarządzanie interfejsami API. Wartość jest nieprawidłowa względny adres URL w formacie `apis/{aid}/operations/{id}` gdzie `{aid}` jest identyfikatorem interfejsu API i `{id}` jest identyfikator operacji. Ta właściwość jest tylko do odczytu.|  
|name|Ciąg|Nazwa operacji. Nie może być pusta. Maksymalna długość wynosi 100 znaków.|  
|description|Ciąg|Opis operacji. Nie może być pusta. Może obejmować formatowanie tagów HTML. Maksymalna długość wynosi 1000 znaków.|  
|Schemat|Ciąg|Opisuje, na które protokoły można wywołać operacji w tym interfejsie API. Dozwolone wartości to `http`, `https`, lub obie `http` i `https`.|  
|Obiekt uriTemplate|Ciąg|Szablon względny adres URL identyfikujący zasób docelowy dla tej operacji. Mogą zawierać parametrów. Przykład:`customers/{cid}/orders/{oid}/?date={date}`|  
|Host|Ciąg|Brama adres URL interfejsu API zarządzania obsługującego interfejsu API.|  
|HttpMethod|Ciąg|Metoda operacji HTTP.|  
|Żądanie|[Żądania HTTP](#HTTPRequest)|Obiekt zawierający informacje dotyczące żądania.|  
|odpowiedzi|Tablica [odpowiedzi HTTP](#HTTPResponse)|Tablica operacji [odpowiedzi HTTP](#HTTPResponse) jednostek.|  
  
##  <a name="Menu"></a>Operacja menu  
 `operation menu` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|apiId|Ciąg|Identyfikator bieżącego interfejsu API.|  
|CurrentOperationId|Ciąg|Identyfikator bieżącej operacji.|  
|Akcja|Ciąg|Typ menu.|  
|Elementy MenuItem|Kolekcja [element menu operacji](#MenuItem) jednostek.|Operacje dla bieżącego interfejsu API.|  
  
##  <a name="MenuItem"></a>Operacja elementu menu  
 `operation menu item` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Identyfikator|Ciąg|Identyfikator operacji.|  
|Tytuł|Ciąg|Opis operacji.|  
|HttpMethod|Ciąg|Metoda Http operacji.|  
  
##  <a name="Paging"></a>Stronicowania  
 `paging` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Strona|Numer|Numer bieżącej strony.|  
|Wartość PageSize|Numer|Maksymalna wyników do wyświetlenia na jednej stronie.|  
|TotalItemCount|Numer|Liczba elementów do wyświetlenia.|  
|ShowAll|Wartość logiczna|Określa, czy Pokaż wszystko powoduje na jednej stronie.|  
|PageCount|Numer|Liczba stron wyników.|  
  
##  <a name="Parameter"></a>Parametr  
 W tej sekcji opisano `parameter` reprezentacji.  
  
|Właściwość|Opis|Typ|  
|--------------|-----------------|----------|  
|name|Ciąg|Nazwa parametru.|  
|description|Ciąg|Opis parametru.|  
|wartość|Ciąg|Wartość parametru.|  
|Opcje|Tablica ciągów|Wartości zdefiniowane dla wartości parametrów zapytania.|  
|Wymagane|Wartość logiczna|Określa, czy parametr jest wymagany.|  
|rodzaj|Numer|Określa, czy ten parametr jest parametr path (1) lub parametr querystring (2).|  
|Właściwość TypeName|Ciąg|Typ parametru.|  
  
##  <a name="Product"></a>Produktu  
 `product` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Identyfikator|Ciąg|Identyfikator zasobu. Unikatowy identyfikator produktu w ramach bieżącego wystąpienia usługi Zarządzanie interfejsami API. Wartość jest nieprawidłowa względny adres URL w formacie `products/{pid}` gdzie `{pid}` to identyfikator produktu. Ta właściwość jest tylko do odczytu.|  
|Tytuł|Ciąg|Nazwa produktu. Nie może być pusta. Maksymalna długość wynosi 100 znaków.|  
|Opis|Ciąg|Opis produktu. Nie może być pusta. Może obejmować formatowanie tagów HTML. Maksymalna długość wynosi 1000 znaków.|  
|Warunki|Ciąg|Warunki użytkowania produktów. Deweloperzy próbuje subskrybować produktu zostanie przedstawiony i musi je zaakceptować przed zakończeniem procesu subskrypcji.|  
|ProductState|Numer|Określa, czy produkt jest opublikowana lub nie. Opublikowane produkty są wykrywalny przez deweloperów w portalu dla deweloperów. Produkty opublikowane nie są widoczne tylko dla administratorów.<br /><br /> Dopuszczalne wartości stanu produktu są:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|Wartość logiczna|Określa, czy użytkownik może mieć wiele subskrypcji do tego produktu w tym samym czasie.|  
|MultipleSubscriptionsCount|Numer|Liczba subskrypcji do tego produktu przez bieżącego użytkownika.|  
  
##  <a name="Provider"></a>Dostawcy  
 `provider` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Właściwości|Słownik ciągu|Właściwości dla tego dostawcy uwierzytelniania.|  
|Typ authenticationType|Ciąg|Typ dostawcy. (Azure Active Directory, Facebook logowania, konta Google, Microsoft Account, Twitter).|  
|Podpis|Ciąg|Nazwa wyświetlana dostawcy.|  
  
##  <a name="Representation"></a>Reprezentacja wartości  
 W tej sekcji opisano `representation`.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Typ zawartości|Ciąg|Określa zarejestrowane lub niestandardowy typ zawartości dla tego reprezentacja, np. `application/xml`.|  
|próbki|Ciąg|Przykład reprezentacji.|  
  
##  <a name="Subscription"></a>Subskrypcji  
 `subscription` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Identyfikator|Ciąg|Identyfikator zasobu. Unikatowy identyfikator subskrypcji w ramach bieżącego wystąpienia usługi Zarządzanie interfejsami API. Wartość jest nieprawidłowa względny adres URL w formacie `subscriptions/{sid}` gdzie `{sid}` jest identyfikator subskrypcji. Ta właściwość jest tylko do odczytu.|  
|Identyfikator produktu|Ciąg|Identyfikator zasobu produktu subskrybowanego produktu. Wartość jest nieprawidłowa względny adres URL w formacie `products/{pid}` gdzie `{pid}` to identyfikator produktu.|  
|ProductTitle|Ciąg|Nazwa produktu. Nie może być pusta. Maksymalna długość wynosi 100 znaków.|  
|ProductDescription|Ciąg|Opis produktu. Nie może być pusta. Może obejmować formatowanie tagów HTML. Maksymalna długość wynosi 1000 znaków.|  
|ProductDetailsUrl|Ciąg|Względny adres URL do szczegółów produktu.|  
|state|Ciąg|Stan subskrypcji. Dostępne są następujące stany:<br /><br /> - `0 - suspended`— subskrypcji jest zablokowany i subskrybenta nie można wywołać wszystkie interfejsy API produktu.<br /><br /> - `1 - active`— Subskrypcja jest aktywna.<br /><br /> - `2 - expired`— Subskrypcja osiągnęła daty jego wygaśnięcia i została zdezaktywowana.<br /><br /> - `3 - submitted`— Żądanie subskrypcji przez deweloperów, ale ma nie została jeszcze zatwierdzeniu lub odrzuceniu.<br /><br /> - `4 - rejected`— Żądanie subskrypcji zostało odrzucone przez administratora.<br /><br /> - `5 - cancelled`— Subskrypcja została anulowana przez dewelopera lub administratora.|  
|Nazwa wyświetlana|Ciąg|Nazwa wyświetlana subskrypcji.|  
|CreatedDate|Data i godzina|Data subskrypcji został utworzony, w formacie ISO 8601: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|Wartość logiczna|Określa, czy subskrypcję można anulować przez bieżącego użytkownika.|  
|IsAwaitingApproval|Wartość logiczna|Określa, czy subskrypcja oczekuje na zatwierdzenie.|  
|datą rozpoczęcia|Data i godzina|Data rozpoczęcia subskrypcji, w formacie ISO 8601: `2014-06-24T16:25:00Z`.|  
|ExpirationDate|Data i godzina|Data wygaśnięcia subskrypcji, w formacie ISO 8601: `2014-06-24T16:25:00Z`.|  
|NotificationDate|Data i godzina|Data powiadomienia dla subskrypcji, w formacie ISO 8601: `2014-06-24T16:25:00Z`.|  
|primaryKey|Ciąg|Klucz podstawowy subskrypcji. Maksymalna długość wynosi 256 znaków.|  
|Klucz pomocniczy|Ciąg|Klucz pomocniczy subskrypcji. Maksymalna długość wynosi 256 znaków.|  
|CanBeRenewed|Wartość logiczna|Określa, czy subskrypcja mogą być odnawiane przez bieżącego użytkownika.|  
|HasExpired|Wartość logiczna|Określa, czy subskrypcja wygasła.|  
|IsRejected|Wartość logiczna|Określa, czy żądanie subskrypcji zostało odrzucone.|  
|cancelUrl|Ciąg|Względny adres Url do anulowania subskrypcji.|  
|RenewUrl|Ciąg|Względny adres Url odnowienia subskrypcji.|  
  
##  <a name="SubscriptionSummary"></a>Podsumowanie subskrypcji  
 `subscription summary` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Identyfikator|Ciąg|Identyfikator zasobu. Unikatowy identyfikator subskrypcji w ramach bieżącego wystąpienia usługi Zarządzanie interfejsami API. Wartość jest nieprawidłowa względny adres URL w formacie `subscriptions/{sid}` gdzie `{sid}` jest identyfikator subskrypcji. Ta właściwość jest tylko do odczytu.|  
|Nazwa wyświetlana|Ciąg|Nazwa wyświetlana subskrypcji|  
  
##  <a name="UserAccountInfo"></a>Informacje o koncie użytkownika  
 `user account info` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Imię|Ciąg|Imię. Nie może być pusta. Maksymalna długość wynosi 100 znaków.|  
|Nazwisko|Ciąg|Nazwisko. Nie może być pusta. Maksymalna długość wynosi 100 znaków.|  
|Adres e-mail|Ciąg|Adres e-mail. Nie może być pusta i musi być unikatowa w ramach wystąpienia usługi. Maksymalna długość to 254 znaków.|  
|Hasło|Ciąg|Hasło do konta użytkownika.|  
|NameIdentifier|Ciąg|Identyfikator konta, taki sam jak adres e-mail użytkownika.|  
|ProviderName|Ciąg|Nazwa dostawcy uwierzytelniania.|  
|IsBasicAccount|Wartość logiczna|Wartość true, jeśli konto zostało zarejestrowane za pomocą poczty e-mail i hasło; wartość false, jeśli konto zostało zarejestrowane za pomocą dostawcy.|  
  
##  <a name="UseSignIn"></a>Logowanie użytkownika  
 `user sign in` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|Adres e-mail|Ciąg|Adres e-mail. Nie może być pusta i musi być unikatowa w ramach wystąpienia usługi. Maksymalna długość to 254 znaków.|  
|Hasło|Ciąg|Hasło do konta użytkownika.|  
|ReturnUrl|Ciąg|Adres URL strony, gdy użytkownik kliknął Zaloguj się.|  
|RememberMe|Wartość logiczna|Określa, czy zapisać informacji o bieżącym użytkowniku.|  
|RegistrationEnabled|Wartość logiczna|Określa, czy rejestracja jest włączona.|  
|DelegationEnabled|Wartość logiczna|Określa, czy delegowany logowania jest włączona.|  
|DelegationUrl|Ciąg|Delegowane znaku w adresie url, jeśli włączone.|  
|SsoSignUpUrl|Ciąg|Rejestracja jednokrotna adres URL dla użytkownika, jeśli jest obecny.|  
|AuxServiceUrl|Ciąg|Jeśli bieżący użytkownik jest administratorem, to łącze do wystąpienia usługi w klasycznym portalu Azure.|  
|Dostawcy|Kolekcja [dostawcy](#Provider) jednostek|Dostawców uwierzytelniania dla tego użytkownika.|  
|UserRegistrationTerms|Ciąg|Warunki, które użytkownik musi wyrazić zgodę na przed zarejestrowaniem się.|  
|UserRegistrationTermsEnabled|Wartość logiczna|Czy warunki są włączone.|  
  
##  <a name="UserSignUp"></a>Tworzenia konta użytkownika  
 `user sign up` Jednostka ma następujące właściwości.  
  
|Właściwość|Typ|Opis|  
|--------------|----------|-----------------|  
|PasswordConfirm|Wartość logiczna|Wartość używana przez [rejestracji](api-management-page-controls.md#sign-up)kontroli rejestracji.|  
|Hasło|Ciąg|Hasło do konta użytkownika.|  
|PasswordVerdictLevel|Numer|Wartość używana przez [rejestracji](api-management-page-controls.md#sign-up)kontroli rejestracji.|  
|UserRegistrationTerms|Ciąg|Warunki, które użytkownik musi wyrazić zgodę na przed zarejestrowaniem się.|  
|UserRegistrationTermsOptions|Numer|Wartość używana przez [rejestracji](api-management-page-controls.md#sign-up)kontroli rejestracji.|  
|ConsentAccepted|Wartość logiczna|Wartość używana przez [rejestracji](api-management-page-controls.md#sign-up)kontroli rejestracji.|  
|Adres e-mail|Ciąg|Adres e-mail. Nie może być pusta i musi być unikatowa w ramach wystąpienia usługi. Maksymalna długość to 254 znaków.|  
|Imię|Ciąg|Imię. Nie może być pusta. Maksymalna długość wynosi 100 znaków.|  
|Nazwisko|Ciąg|Nazwisko. Nie może być pusta. Maksymalna długość wynosi 100 znaków.|  
|Danych użytkownika|Ciąg|Wartość używana przez [rejestracji](api-management-page-controls.md#sign-up) formantu.|  
|NameIdentifier|Ciąg|Wartość używana przez [rejestracji](api-management-page-controls.md#sign-up)kontroli rejestracji.|  
|ProviderName|Ciąg|Nazwa dostawcy uwierzytelniania.|

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat pracy z szablonami, zobacz [dostosowywaniu portalu dla deweloperów interfejsu API zarządzania za pomocą szablonów](api-management-developer-portal-templates.md).
