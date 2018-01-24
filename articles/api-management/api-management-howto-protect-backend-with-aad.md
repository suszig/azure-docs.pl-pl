---
title: "Ochrona interfejsu API sieci Web wewnętrznej bazy danych z usługi Azure Active Directory i zarządzanie interfejsami API | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zabezpieczyć interfejs API sieci Web wewnętrznej bazy danych z usługi Azure Active Directory i zarządzanie interfejsami API."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: 603a22059dcf07c68f4c6576ea1df97d810eacf3
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Jak zabezpieczyć interfejs API sieci Web wewnętrznej bazy danych z usługi Azure Active Directory i zarządzanie interfejsami API
Poniższe wideo przedstawia sposób tworzenia zaplecza interfejsu API sieci Web i chronić go przy użyciu protokołu OAuth 2.0 z usługą Azure Active Directory i zarządzanie interfejsami API.  Ten artykuł zawiera omówienie i dodatkowe informacje dotyczące czynności w wideo. To 24 minutę wideo pokazuje, jak do:

* Tworzenie zaplecza interfejsu API sieci Web i zabezpiecz ją przy użyciu usługi AAD — począwszy od 1:30
* Importowanie interfejsu API zarządzanie interfejsami API — począwszy od 7:10
* Konfigurowanie portalu dla deweloperów do wywołania interfejsu API — począwszy od 9:09
* Konfigurowanie aplikacji pulpitu do wywołania interfejsu API — począwszy od 18:08
* Skonfiguruj zasady sprawdzania poprawności tokenu JWT wstępnie autoryzować żądania — począwszy od 20:47

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

## <a name="create-an-azure-ad-directory"></a>Utwórz katalog usługi Azure AD
Aby zabezpieczyć interfejs API sieci Web kopii przy użyciu usługi Azure Active Directory, musisz najpierw mieć dzierżawę usługi AAD. W tym wideo dzierżawa o nazwie **APIMDemo** jest używany. Aby utworzyć dzierżawę usługi AAD, zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) i kliknij przycisk **nowy**->**usługi aplikacji**->**usługi Active Directory**->**katalogu**->**Utwórz niestandardowy**. 

![Usługa Azure Active Directory][api-management-create-aad-menu]

W tym przykładzie katalog o nazwie **APIMDemo** jest tworzony z domyślnej domeny o nazwie **DemoAPIM.onmicrosoft.com**. Ten katalog jest używany w całym wideo.

![Usługa Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Tworzenie usługi interfejsu API sieci Web zabezpieczonych przez usługi Azure Active Directory
W tym kroku zaplecza interfejsu API sieci Web jest tworzony przy użyciu programu Visual Studio 2013. W tym kroku wideo rozpoczyna się od 1:30. Do tworzenia projektu zaplecza interfejsu API sieci Web w programie Visual Studio, kliknij **pliku**->**nowy**->**projektu**i wybierz polecenie **aplikacji sieci Web ASP.NET** z **Web** listy szablonów. W tym wideo projektu o nazwie **APIMAADDemo**. Kliknij przycisk **OK**, aby utworzyć projekt. 

![Visual Studio][api-management-new-web-app]

Kliknij przycisk **interfejsu API sieci Web** z **wybierz listę szablonów** do tworzenia projektu interfejsu API sieci Web. Aby skonfigurować uwierzytelnianie katalogu Azure kliknij **Zmień uwierzytelnianie**.

![Nowy projekt][api-management-new-project]

Kliknij przycisk **konta organizacyjne**, a następnie określ **domeny** dzierżawy usługi AAD. W tym przykładzie domena jest **DemoAPIM.onmicrosoft.com**. Domeny katalogu można je uzyskać z **domen** katalogu.

![Domeny][api-management-aad-domains]

Skonfiguruj odpowiednie ustawienia w **Zmień uwierzytelnianie** okno dialogowe i kliknij przycisk **OK**.

![Zmienianie uwierzytelniania][api-management-change-authentication]

Po kliknięciu **OK** programu Visual Studio będzie próbował zarejestrować aplikację w usłudze Azure AD w katalogu i może pojawić się prośba do logowania przez program Visual Studio. Zaloguj się przy użyciu konta administracyjnego dla katalogu.

![Zaloguj się do programu Visual Studio][api-management-sign-in-vidual-studio]

Aby skonfigurować ten projekt jako interfejs API sieci Web Azure pole wyboru dla **Hostuj w chmurze** , a następnie kliknij przycisk **OK**.

![Nowy projekt][api-management-new-project-cloud]

Monit logowanie do platformy Azure, a następnie można skonfigurować aplikacji sieci Web.

![Konfigurowanie][api-management-configure-web-app]

W tym przykładzie nowy **planu usługi aplikacji** o nazwie **APIMAADDemo** jest określona.

Kliknij przycisk **OK** do skonfigurowania aplikacji sieci Web i utworzyć projekt.

## <a name="add-the-code-to-the-web-api-project"></a>Dodaj kod do projektu interfejsu API sieci Web
Następny krok w wideo dodaje kod do projektu interfejsu API sieci Web. Ten krok zostanie uruchomiony na 4:35.

Interfejs API sieci Web, w tym przykładzie implementuje Kalkulator podstawowe usługi przy użyciu modelu i kontrolera. Aby dodać model dla usługi, kliknij prawym przyciskiem myszy **modele** w **Eksploratora rozwiązań** i wybierz polecenie **Dodaj**, **klasy**. Nazwa klasy `CalcInput` i kliknij przycisk **Dodaj**.

Dodaj następujące `using` instrukcji na początku `CalcInput.cs` pliku.

```csharp
using Newtonsoft.Json;
```

Zastąp następujący kod wygenerowanej klasy.

```csharp
public class CalcInput
{
    [JsonProperty(PropertyName = "a")]
    public int a;

    [JsonProperty(PropertyName = "b")]
    public int b;
}
```

Kliknij prawym przyciskiem myszy **kontrolerów** w **Eksploratora rozwiązań** i wybierz polecenie **Dodaj**->**kontrolera**. Wybierz **kontrolera 2 interfejsu API sieci Web — pusty** i kliknij przycisk **Dodaj**. Typ **CalcController** kontrolera nazwę, a następnie kliknij przycisk **Dodaj**.

![Dodawanie kontrolera][api-management-add-controller]

Dodaj następujące `using` instrukcji na początku `CalcController.cs` pliku.

```csharp
using System.IO;
using System.Web;
using APIMAADDemo.Models;
```

Zastąp następujący kod klasy wygenerowanym kontrolerze. Implementuje ten kod `Add`, `Subtract`, `Multiply`, i `Divide` operacji podstawowe Kalkulator interfejsu API.

```csharp
[Authorize]
public class CalcController : ApiController
{
    [Route("api/add")]
    [HttpGet]
    public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/sub")]
    [HttpGet]
    public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/mul")]
    [HttpGet]
    public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/div")]
    [HttpGet]
    public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }
}
```

Naciśnij klawisz **F6** do tworzenia i Zweryfikuj rozwiązanie.

## <a name="publish-the-project-to-azure"></a>Publikowanie projektu na platformie Azure
W tym kroku programu Visual Studio projektu została opublikowana na platformie Azure. W tym kroku wideo rozpoczyna się od 5:45.

Aby opublikować projekt na platformie Azure, kliknij prawym przyciskiem myszy **APIMAADDemo** projekt w programie Visual Studio i wybierz pozycję **publikowania**. Zachowaj ustawienia domyślne **publikowanie w sieci Web** okno dialogowe i kliknij przycisk **publikowania**.

![Publikowanie w sieci Web][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Udzielanie uprawnień do wewnętrznej bazy danych aplikacji usługi Azure AD
Nową aplikację usługi wewnętrznej bazy danych jest tworzony w katalogu usługi Azure AD jako część procesu konfigurowania i publikowania projektu interfejsu API sieci Web. W tym kroku wideo, zaczynając od 6:13 przyznano uprawnienia do zaplecza interfejsu API sieci Web.

![Aplikacja][api-management-aad-backend-app]

Kliknij nazwę aplikacji do konfigurowania wymaganych uprawnień. Przejdź do **Konfiguruj** karcie i przewiń w dół do **uprawnień dotyczących innych aplikacji** sekcji. Kliknij przycisk **uprawnienia aplikacji** listy rozwijanej obok **Windows** **usługi Azure Active Directory**, pole wyboru dla **odczytuj dane katalogu**i kliknij przycisk **zapisać**.

![Dodaj uprawnienia][api-management-aad-add-permissions]

> [!NOTE]
> Jeśli **Windows** **usługi Azure Active Directory** jest niewymienione w obszarze uprawnienia do innych aplikacji, kliknij przycisk **dodać aplikację** i dodaj go do listy.
> 
> 

Zanotuj **identyfikator URI aplikacji** do użycia w kolejnym kroku po skonfigurowaniu aplikacji usługi Azure AD do portalu dla deweloperów usługi API Management.

![Identyfikator URI. Identyfikator aplikacji][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Importowanie interfejsu API sieci Web do zarządzania interfejsem API
Interfejsy API są skonfigurowane z portalu wydawcy interfejsu API, który jest dostępny za pośrednictwem portalu Azure. Aby uzyskać do niej dostęp, kliknij przycisk **portal wydawcy** na pasku narzędzi usługi Zarządzanie interfejsami API. Jeśli jeszcze nie utworzono wystąpienie usługi API Management, zobacz [Utwórz wystąpienie usługi Zarządzanie interfejsami API] [ Create an API Management service instance] w [pierwszy interfejs API zarządzania] [ Manage your first API] samouczka.

![Publisher portal][api-management-management-console]

Operacje mogą być [ręcznie dodawać do interfejsów API](api-management-howto-add-operations.md), lub mogą być importowane. W tym wideo operacje są importowane w formacie struktury Swagger, zaczynając od 6:40.

Utwórz plik o nazwie `calcapi.json` z następującą zawartość, a następnie zapisz go na komputerze. Upewnij się, że `host` atrybutu punktów z wewnętrzną bazą danych interfejsu API sieci Web. W tym przykładzie `"host": "apimaaddemo.azurewebsites.net"` jest używany.

```json
{
  "swagger": "2.0",
  "info": {
    "title": "Calculator",
    "description": "Arithmetics over HTTP!",
    "version": "1.0"
  },
  "host": "apimaaddemo.azurewebsites.net",
  "basePath": "/api",
  "schemes": [
    "http"
  ],
  "paths": {
    "/add?a={a}&b={b}": {
      "get": {
        "description": "Responds with a sum of two numbers.",
        "operationId": "Add two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>51</code>.",
            "required": true,
            "type": "string",
            "default": "51",
            "enum": [
              "51"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>49</code>.",
            "required": true,
            "type": "string",
            "default": "49",
            "enum": [
              "49"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/sub?a={a}&b={b}": {
      "get": {
        "description": "Responds with a difference between two numbers.",
        "operationId": "Subtract two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>50</code>.",
            "required": true,
            "type": "string",
            "default": "50",
            "enum": [
              "50"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/div?a={a}&b={b}": {
      "get": {
        "description": "Responds with a quotient of two numbers.",
        "operationId": "Divide two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/mul?a={a}&b={b}": {
      "get": {
        "description": "Responds with a product of two numbers.",
        "operationId": "Multiply two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>5</code>.",
            "required": true,
            "type": "string",
            "default": "5",
            "enum": [
              "5"
            ]
          }
        ],
        "responses": { }
      }
    }
  }
}
```

Aby zaimportować interfejs API kalkulatora, kliknij opcję **Interfejsy API** w menu **API Management** po lewej stronie, a następnie kliknij przycisk **Importuj interfejs API**.

![Przycisk do importowania interfejsu API][api-management-import-api]

Wykonaj poniższe kroki, aby skonfigurować Kalkulator interfejsu API.

1. Kliknij przycisk **z pliku**, przejdź do `calculator.json` można zapisać pliku, a następnie kliknij przycisk **Swagger** przycisk radiowy.
2. Typ **obliczenia** do **sufiks adresu URL interfejsu API sieci Web** pola tekstowego.
3. Kliknij pole **Produkty (opcjonalne)** i wybierz produkt **Starter**.
4. Kliknij przycisk **Zapisz**, aby zaimportować interfejs API.

![Dodawanie nowego interfejsu API][api-management-import-new-api]

Po zaimportowaniu interfejsu API w portalu wydawcy zostanie wyświetlona strona podsumowania dotycząca interfejsu API.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Wywołanie interfejsu API niepomyślnie z portalu dla deweloperów
W tym momencie interfejsu API został zaimportowany do interfejsu API zarządzania, ale nie jeszcze można wywołać pomyślnie z portalu dla deweloperów, ponieważ usługi wewnętrznej bazy danych jest chroniony za pomocą uwierzytelniania usługi Azure AD. To jest przedstawiona w wideo, zaczynając od 7:40, wykonując następujące kroki.

Kliknij przycisk **portalu dla deweloperów** z prawym górnym rogu strony portalu wydawcy.

![Portal dla deweloperów][api-management-developer-portal-menu]

Kliknij przycisk **interfejsów API** i kliknij przycisk **Kalkulator** interfejsu API.

![Portal dla deweloperów][api-management-dev-portal-apis]

Kliknij przycisk **wypróbuj**.

![Wypróbuj][api-management-dev-portal-try-it]

Kliknij przycisk **wysyłania** i zwróć uwagę na stan odpowiedzi **401 nieautoryzowane**.

![Send][api-management-dev-portal-send-401]

Nie ma autoryzacji żądania, ponieważ interfejs API zaplecza jest chroniony przez usługę Azure Active Directory. Przed pomyślnym wywołanie interfejsu API dewelopera portal musi być skonfigurowana do autoryzowania deweloperów korzystających z protokołu OAuth 2.0. Ten proces jest opisane w poniższych sekcjach.

## <a name="register-the-developer-portal-as-an-aad-application"></a>Zarejestruj portalu dla deweloperów jako aplikację AAD
Pierwszym etapem konfigurowania portalu dla deweloperów do autoryzacji deweloperów korzystających z protokołu OAuth 2.0 jest zarejestrowanie go jako aplikację AAD portalu dla deweloperów. Jest to wykazać, zaczynając od 8:27 wideo.

Przejdź do dzierżawy usługi Azure AD z pierwszym krokiem ten film, w tym przykładzie **APIMDemo** i przejdź do **aplikacji** kartę.

![Nowa aplikacja][api-management-aad-new-application-devportal]

Kliknij przycisk **Dodaj** przycisk, aby utworzyć nową aplikację usługi Azure Active Directory, a następnie wybierz pozycję **Dodaj aplikację moją organizację**.

![Nowa aplikacja][api-management-new-aad-application-menu]

Wybierz **sieci Web, aplikacji i/lub interfejs API sieci Web**, wprowadź nazwę i kliknij strzałkę dalej. W tym przykładzie **APIMDeveloperPortal** jest używany.

![Nowa aplikacja][api-management-aad-new-application-devportal-1]

Aby uzyskać **adres URL logowania** wprowadź adres URL usługi API Management i Dołącz `/signin`. W tym przykładzie `https://contoso5.portal.azure-api.net/signin` jest używany.

Aby uzyskać **adres URL identyfikatora aplikacji** wprowadź adres URL usługi API Management i Dołącz niektóre unikatowe znaki. Mogą to być wszystkie odpowiednie znaki w tym przykładzie `https://contoso5.portal.azure-api.net/dp` jest używany. Gdy żądane **właściwości aplikacji** są skonfigurowane, kliknij znacznik wyboru, aby utworzyć aplikację.

![Nowa aplikacja][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Konfigurowanie serwera autoryzacji usługi API Management OAuth 2.0
Następnym krokiem jest skonfigurowanie serwera autoryzacji OAuth 2.0 w usłudze API Management. Ten krok jest przedstawiona w wideo, zaczynając od 9:43.

Kliknij przycisk **zabezpieczeń** z interfejsu API zarządzania menu po lewej stronie, kliknij przycisk **OAuth 2.0**, a następnie kliknij przycisk **dodać autoryzacji** serwera.

![Dodawanie serwera autoryzacji][api-management-add-authorization-server]

Wprowadź nazwę i opcjonalny opis w **nazwa** i **opis** pola. Te pola są używane do identyfikowania serwera autoryzacji OAuth 2.0 w wystąpieniu usługi Zarządzanie interfejsami API. W tym przykładzie **pokaz serwera autoryzacji** jest używany. Później podczas określania serwera OAuth 2.0, który będzie używany do uwierzytelniania dla interfejsu API, możesz wybrać tej nazwy.

Aby uzyskać **adres URL strony rejestracji klienta** wprowadź wartość symbolu zastępczego, takich jak `http://localhost`.  **Adres URL strony rejestracji klienta** wskazuje strona, do której użytkownicy mogą używać, aby utworzyć i skonfigurować swoje własne konta dla dostawcy OAuth 2.0, które obsługują zarządzanie kontami użytkowników. W tym przykładzie użytkowników nie tworzyć i konfigurować własne konta, więc symbol zastępczy jest używany.

![Dodawanie serwera autoryzacji][api-management-add-authorization-server-1]

Następnie określ **adres URL punktu końcowego autoryzacji** i **adres URL punktu końcowego tokenu**.

![Serwer autoryzacji][api-management-add-authorization-server-1a]

Te wartości można pobrać z **punkty końcowe aplikacji** strony aplikacji AAD utworzona dla portalu dla deweloperów. Aby uzyskać dostęp do punktów końcowych, przejdź do **Konfigurowanie** aplikacji AAD i kliknij polecenie **wyświetlić punkty końcowe**.

![Aplikacja][api-management-aad-devportal-application]

![Wyświetl punkty końcowe][api-management-aad-view-endpoints]

Kopiuj **punktu końcowego autoryzacji OAuth 2.0** i wklej ją do **adres URL punktu końcowego autoryzacji** pola tekstowego.

![Dodawanie serwera autoryzacji][api-management-add-authorization-server-2]

Kopiuj **punkt końcowy tokenu OAuth 2.0** i wklej ją do **tokenu końcowego adresu URL** pola tekstowego.

![Dodawanie serwera autoryzacji][api-management-add-authorization-server-2a]

Oprócz wklejenie punktu końcowego tokenu, Dodaj parametr treści dodatkowe o nazwie **zasobów** i wykorzystanie wartości **identyfikator URI aplikacji** z usługi AAD aplikacji dla usługi wewnętrznej bazy danych, który został utworzony, gdy projekt programu Visual Studio został opublikowany.

![Identyfikator URI. Identyfikator aplikacji][api-management-aad-sso-uri]

Następnie określ poświadczenia klienta. Są to poświadczenia dla zasobu, który ma dostęp, w tym przypadku portalu dla deweloperów.

![Poświadczenia klienta][api-management-client-credentials]

Aby uzyskać **identyfikator klienta**, przejdź do **Konfiguruj** aplikacji AAD w portalu dla deweloperów i kopia **identyfikator klienta**.

Aby uzyskać **klucz tajny klienta** kliknij **wybierz czas trwania** listy rozwijanej w **klucze** sekcji i określ interwał. W tym przykładzie używany jest 1 rok.

![Identyfikator klienta][api-management-aad-client-id]

Kliknij przycisk **zapisać** do zapisania konfiguracji, a następnie wyświetlić klucz. 

> [!IMPORTANT]
> Zanotuj tego klucza. Po zamknięciu okna konfiguracji usługi Azure Active Directory, klucz nie może ponownie wyświetlone.
> 
> 

Kopiuj do Schowka klucz, przełączyć się do portalu wydawcy, Wklej klucz do **klucz tajny klienta** polem tekstowym i kliknij przycisk **zapisać**.

![Dodawanie serwera autoryzacji][api-management-add-authorization-server-3]

Bezpośrednio po poświadczeń klienta jest udzielania kodu autoryzacji. Ten kod autoryzacji i przełącznika z powrotem do aplikacji portalu dewelopera usługi Azure AD kopiowania skonfigurować stronę i Wklej udzielania autoryzacji do **adres URL odpowiedzi** , a następnie kliknij przycisk **zapisać** ponownie.

![Adres URL odpowiedzi][api-management-aad-reply-url]

Następnym krokiem jest skonfigurowanie uprawnień dla portalu dla deweloperów usługi AAD aplikacji. Kliknij przycisk **uprawnienia aplikacji** i pole wyboru dla **odczytuj dane katalogu**. Kliknij przycisk **zapisać** zapisać tę zmianę, a następnie kliknij przycisk **Dodaj aplikację**.

![Dodaj uprawnienia][api-management-add-devportal-permissions]

Kliknij ikonę wyszukiwania typu **APIM** na uruchamianie z polem, wybierz **APIMAADDemo**i kliknij znacznik wyboru, aby zapisać.

![Dodaj uprawnienia][api-management-aad-add-app-permissions]

Kliknij przycisk **delegowane uprawnienia** dla **APIMAADDemo** i pole wyboru dla **APIMAADDemo dostępu**i kliknij przycisk **zapisać**. Dzięki temu deweloper aplikacji portalu do uzyskania dostępu do usługi zaplecza.

![Dodaj uprawnienia][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Włączanie autoryzacji użytkownika OAuth 2.0 dla interfejsu API Kalkulator
Teraz, gdy serwer protokołu OAuth 2.0 jest skonfigurowany, możesz je określić ustawienia zabezpieczeń dla interfejsu API. Ten krok jest przedstawiona w wideo, zaczynając od 14:30.

Kliknij przycisk **interfejsów API** w menu po lewej stronie, a następnie kliknij przycisk **Kalkulator** Aby wyświetlić i skonfigurować jego ustawienia.

![Kalkulator interfejsu API][api-management-calc-api]

Przejdź do **zabezpieczeń** karcie wyboru **OAuth 2.0** pole wyboru, wybierz serwer autoryzacji żądaną z **serwera autoryzacji** listy rozwijanej i kliknij przycisk **zapisać**.

![Kalkulator interfejsu API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Pomyślne wywołanie interfejsu API programu Kalkulator z portalu dla deweloperów
Teraz, gdy autoryzacji OAuth 2.0 został skonfigurowany w interfejsie API, jego operacji może pomyślnie wywołana z Centrum deweloperów. Ten krok jest przedstawiona w wideo, zaczynając od 15:00.

Przejdź z powrotem do **Dodaj dwie liczb całkowitych** operacji usługi Kalkulator w portalu dla deweloperów i kliknij **wypróbuj**. Należy pamiętać, nowy element w **autoryzacji** sekcji odpowiadający właśnie dodano serwer autoryzacji.

![Kalkulator interfejsu API][api-management-calc-authorization-server]

Wybierz **kod autoryzacji** z autoryzacji listy rozwijanej liście, a następnie wprowadź poświadczenia konta, które będzie używane. Jeśli nastąpiło już zalogowanie się przy użyciu konta, które nie mogą być potrzebne.

![Kalkulator interfejsu API][api-management-devportal-authorization-code]

Kliknij przycisk **wysyłania** i zanotuj **stanu odpowiedzi** z **200 OK** i wyniki operacji w zawartości odpowiedzi.

![Kalkulator interfejsu API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Konfigurowanie aplikacji pulpitu do wywołania interfejsu API
Następna procedura wideo rozpoczyna się od 16:30 i konfiguruje prostą aplikację pulpitu do wywołania interfejsu API. Pierwszym krokiem jest zarejestrować aplikację w usłudze Azure AD i nadaj mu dostęp do katalogu i usługi wewnętrznej bazy danych. Na 18:25 istnieje pokaz pulpitu aplikacji podczas wywoływania operacji na Kalkulator interfejsu API.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Skonfiguruj zasady sprawdzania poprawności tokenu JWT wstępnie autoryzować żądania
Ostatnia procedura wideo rozpoczyna się od 20:48 i przedstawiono sposób użycia [JWT do zweryfikowania](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) zasad do wstępnie autoryzacji żądania, sprawdzając poprawność tokenów dostępu dla każdego żądania przychodzącego. Jeśli żądanie nie jest weryfikowany przez zasady sprawdzania poprawności tokenów JWT, żądanie jest blokowany przez interfejs API zarządzania i nie jest przekazywane do wewnętrznej bazy danych.

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
        </claim>
    </required-claims>
</validate-jwt>
```

Aby innego demonstracyjne konfigurowania i korzystania z tych zasad, zobacz [177 epizodu obejmują chmury: więcej funkcji interfejsu API zarządzania](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) i szybkie przewijanie do przodu do 13:50. Szybko przewiń do przodu do 15:00, aby wyświetlić zasady skonfigurowane w edytorze zasad, a następnie do 18:50 dla pokaz wywołanie operacji z portalu dla deweloperów zarówno z i bez tokenu autoryzacji wymagane.

## <a name="next-steps"></a>Kolejne kroki
* Zapoznaj się z kilku [wideo](https://azure.microsoft.com/documentation/videos/index/?services=api-management) o zarządzanie interfejsami API.
* Aby uzyskać inne metody zabezpieczania usługi wewnętrznej bazy danych, zobacz [uwierzytelnianie wzajemne certyfikatu](api-management-howto-mutual-certificates.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Create an API Management service instance]: get-started-create-service-instance.md
[Manage your first API]: import-and-publish.md
