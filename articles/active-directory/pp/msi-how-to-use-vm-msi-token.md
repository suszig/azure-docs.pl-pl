---
title: "Jak używać tożsamości usługi zarządzania przypisane przez użytkownika do uzyskania tokenu dostępu na maszynie Wirtualnej."
description: "Krok po kroku instrukcje i przykłady korzystania przypisany użytkownik pliku MSI w maszynie Wirtualnej platformy Azure można uzyskać OAuth dostęp do tokenu."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 68454d3f3880df82ca895d1c5f140ebdb6030e77
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="acquire-an-access-token-for-a-vm-user-assigned-managed-service-identity-msi"></a>Uzyskać token dostępu dla maszyny Wirtualnej, zarządzane tożsamości usługi (MSI) przypisany użytkownik

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]
Ten artykuł zawiera różne przykłady kodu i skrypt nabycia token, a także wskazówki dotyczące ważnych tematów, takich jak obsługa wygaśnięcia tokenu i błędów HTTP.

## <a name="prerequisites"></a>Wymagania wstępne
[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]
Jeśli planujesz używać programu Azure PowerShell przykłady w tym artykule, należy zainstalować najnowszą wersję [programu Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM).

> [!IMPORTANT]
> - Wszystkie próbki kodu skryptu w tym artykule przyjęto założenie, klient jest uruchomiony na maszynie wirtualnej włączone MSI. Funkcja maszyny Wirtualnej "Połącz" w portalu Azure, aby zdalnie nawiązać połączenia z maszyną Wirtualną. Aby uzyskać więcej informacji na temat włączania MSI w maszynie Wirtualnej, zobacz [skonfigurować przypisany użytkownik zarządzane usługi tożsamości (MSI) dla maszyny Wirtualnej, przy użyciu interfejsu wiersza polecenia Azure](msi-qs-configure-cli-windows-vm.md), lub jednego z artykułów variant (przy użyciu portalu, programu PowerShell, szablonu lub zestawu SDK platformy Azure). 

## <a name="overview"></a>Przegląd

Aplikacja kliencka mogą żądać MSI [token dostępu tylko do aplikacji](~/articles/active-directory/develop/active-directory-dev-glossary.md#access-token) do uzyskiwania dostępu do zasobu. Token jest [oparte na nazwy głównej usługi MSI](msi-overview.md#how-does-it-work). Tak jest niepotrzebna dla klienta w celu zarejestrowania się w celu uzyskania tokenu dostępu w ramach własnej nazwy głównej usługi. Token jest odpowiednie do użycia jako tokenu elementu nośnego w [service-to-service wymaga poświadczeń klienta wymagające](~/articles/active-directory/active-directory-protocols-oauth-service-to-service.md).

|  |  |
| -------------- | -------------------- |
| [Uzyskaj token za pośrednictwem protokołu HTTP](#get-a-token-using-http) | Szczegóły protokół dla punktu końcowego tokena MSI |
| [Uzyskaj token przy użyciu programu CURL](#get-a-token-using-curl) | Przykład użycia punkt końcowy MSI REST w kliencie Bash/CURL |
| [Obsługa wygaśnięcia tokenu](#handling-token-expiration) | Wskazówki dotyczące obsługi tokenów dostępu wygasły |
| [Obsługa błędów](#error-handling) | Wskazówki dotyczące obsługi błędów HTTP zwrócony z punktu końcowego tokena MSI |
| [Identyfikatory zasobów dla usług Azure](#resource-ids-for-azure-services) | Skąd uzyskać identyfikatorów zasobów dla obsługiwanych usług platformy Azure |

## <a name="get-a-token-using-http"></a>Uzyskaj token za pośrednictwem protokołu HTTP 

Podstawowe interfejsu uzyskania tokenu dostępu jest oparta na REST, udostępnienie jej do klienta aplikacji uruchomionych na maszynie Wirtualnej, która może wykonywać wywołania REST protokołu HTTP. Jest to podobne do modelu programowania usługi Azure AD, z wyjątkiem klient używa punktu końcowego na maszynie wirtualnej (vs Azure AD punktu końcowego).

Przykładowe żądanie przy użyciu punktu końcowego usługi metadanych wystąpienia (IMDS):

```
GET http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F&client_id=712eac09-e943-418c-9be6-9fd5c91078bl HTTP/1.1 Metadata: true
```

Przykładowe żądanie przy użyciu punktu końcowego rozszerzenia maszyny Wirtualnej MSI (nadchodzących amortyzacja):

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F&client_id=712eac09-e943-418c-9be6-9fd5c91078bl HTTP/1.1 Metadata: true
```

| Element | Opis |
| ------- | ----------- |
| `GET` | Zlecenie HTTP wskazująca, że chcesz pobrać dane z punktu końcowego. W takim przypadku tokenu dostępu protokołu OAuth. | 
| `http://169.254.169.254/metadata/identity/oauth2/token` | Punkt końcowy MSI dla wystąpienia usługi metadanych. |
| `http://localhost:50342/oauth2/token` | Punkt końcowy MSI dla rozszerzenia maszyny Wirtualnej, w którym 50342 jest domyślnym portem i można go skonfigurować. |
| `api-version`  | Parametr ciągu zapytania, wskazujący wersję interfejsu API dla punktu końcowego IMDS.  |
| `resource` | Parametr ciągu zapytania, wskazującą aplikację identyfikator URI zasobu docelowego. Jest także wyświetlany w `aud` oświadczeń (odbiorcy) wystawionego tokenu. W tym przykładzie żądania tokenu dostępu usługi Azure Resource Manager, do którego ma identyfikator URI aplikacji z https://management.azure.com/. |
| `client_id` |  *Opcjonalne* parametr ciągu, wskazując Identyfikatora klienta (znanej także jako identyfikator aplikacji) zapytania z nazwy głównej usługi reprezentujący pliku MSI przypisane przez użytkownika. Jeśli używane są przypisywane przez system MSI, ten parametr nie jest wymagane. Ta wartość jest zwracana w `clientId` właściwości podczas tworzenia pliku msi przypisane przez użytkownika. W tym przykładzie żądania tokenu dla Identyfikatora klienta "712eac09-e943-418c-9be6-9fd5c91078bl". |
| `Metadata` | Pola nagłówka żądania HTTP, wymagane przez Instalatora MSI jako ograniczenie przed atakiem serwera po stronie żądania Międzywitrynowego (SSRF). Musi mieć ustawioną wartość "prawda", w małe litery.

Przykładowa odpowiedź:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
  "client_id":"712eac09-e943-418c-9be6-9fd5c91078bl"
}
```

| Element | Opis |
| ------- | ----------- |
| `access_token` | Żądany dostęp token. Podczas wywoływania metody zabezpieczonych interfejsu API REST, token jest osadzony w `Authorization` pola nagłówka żądania jako token "bearer", umożliwiając interfejsu API do uwierzytelniania obiektu wywołującego. | 
| `expires_in` | Liczba sekund, przez które token dostępu w dalszym ciągu był prawidłowy, zanim wygaśnie od czasu wydania. Czas wystawiania można znaleźć w tokenie `iat` oświadczeń. |
| `expires_on` | Zakres czasu wygaśnięcia tokenu dostępu. Data jest reprezentowana jako liczba sekund od "1970-01-01T0:0:0Z UTC" (odpowiada tokenu `exp` oświadczenia). |
| `not_before` | Timespan, gdy token dostępu obowiązuje i mogą być akceptowane. Data jest reprezentowana jako liczba sekund od "1970-01-01T0:0:0Z UTC" (odpowiada tokenu `nbf` oświadczenia). |
| `resource` | Żądany zasób token dostępu został dla odpowiadający identyfikatorowi `resource` żądania parametr ciągu zapytania. |
| `token_type` | Typ tokenu, który jest "Bearer" tokenu dostępu, co oznacza, że zasób pozwala uzyskiwać dostęp do elementu nośnego tego tokenu. |
| `client_id` | Identyfikator klienta (znanej także jako identyfikator aplikacji) nazwy głównej usługi reprezentujący MSI przypisane przez użytkownika, dla którego zażądano tokenu. |

## <a name="get-a-token-using-curl"></a>Uzyskaj token przy użyciu programu CURL

Pamiętaj zastąpić identyfikator klienta (znanej także jako identyfikator aplikacji) główny dla usługi użytkownika przypisany użytkownik MSI <MSI CLIENT ID> wartość `client_id` parametru. Ta wartość jest zwracana w `clientId` właściwości podczas tworzenia pliku msi przypisane przez użytkownika.
  
Przykładowe żądanie przy użyciu punktu końcowego usługi metadanych wystąpienia (IMDS):

   ```bash
   response=$(curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<MSI CLIENT ID>")
   access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   echo The MSI access token is $access_token
   ```
   
Przykładowe żądanie przy użyciu punktu końcowego rozszerzenia maszyny Wirtualnej MSI (nadchodzących amortyzacja):

   ```bash
   response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/&client_id=<MSI CLIENT ID>" -H Metadata:true -s)
   access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   echo The MSI access token is $access_token
   ```

   Odpowiedzi na przykład:

   ```bash
   user@vmLinux:~$ response=$(curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=9d484c98-b99d-420e-939c-z585174b63bl")
   user@vmLinux:~$ access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   user@vmLinux:~$ echo The MSI access token is $access_token
   The MSI access token is eyJ0eXAiOiJKV1QiLCJhbGciO...
   ```

## <a name="handling-token-expiration"></a>Obsługa wygaśnięcia tokenu

Podsystem MSI buforuje tokenów. W związku z tym można wywołać ją tyle razy, ile Ci się podoba i powoduje wywołanie w locie do usługi Azure AD tylko wtedy, gdy:
- Chybienia pamięci podręcznej występuje ze względu na nie tokenu w pamięci podręcznej
- token utracił ważność

Kod w pamięci podręcznej tokenu, powinny być przygotowane do obsługi scenariuszy, w którym zasobu wskazuje, czy token utracił ważność.

## <a name="error-handling"></a>Obsługa błędów 

Punkt końcowy MSI sygnały błędów za pomocą kodu stanu pola nagłówka komunikatu odpowiedzi HTTP, jak 4xx lub 5xx błędy:

| Kod stanu | Przyczyny błędu | Sposób obsługi |
| ----------- | ------------ | ------------- |
| Błąd 4xx w żądaniu. | Co najmniej jeden z parametrów żądania jest niepoprawne. | Nie próbuj ponownie.  Sprawdź, czy szczegóły błędu, aby uzyskać więcej informacji.  błędy 4xx są błędy czasu projektowania.|
| 5xx Błąd przejściowy z usługi. | Podsystem MSI lub Azure Active Directory zwróciła błąd przejściowy. | Jest bezpieczne ponowić próbę po odczekaniu co najmniej 1 sekundę.  Jeśli można ponowić próbę zbyt szybko lub zbyt często, usługi Azure AD mogą zwracać błąd limitu szybkości (429).|

Jeśli wystąpi błąd, odpowiednich treści odpowiedzi HTTP zawiera JSON z szczegóły błędu:

| Element | Opis |
| ------- | ----------- |
| error   | Identyfikator błędu. |
| error_description | Pełen opis błędu. **Opisy błędów można zmienić w dowolnym momencie. Nie zapisuj kod, który gałęzie na podstawie wartości w opisie błędu.**|

### <a name="http-response-reference"></a>Odwołanie odpowiedzi HTTP

W tej sekcji omówiono odpowiedzi może zawierać błąd. A "200 OK" stan to pomyślnej odpowiedzi, a token dostępu jest zawarty w treści odpowiedzi JSON, w elemencie ' access_token '.

| Kod stanu | Błąd | Opis błędu | Rozwiązanie |
| ----------- | ----- | ----------------- | -------- |
| 400 Niewłaściwe żądanie | invalid_resource | AADSTS50001: Aplikacja o nazwie  *\<URI\>*  nie znaleziono dzierżawy o nazwie  *\<identyfikator DZIERŻAWCY\>*. Może to nastąpić, jeśli aplikacja nie została zainstalowana przez administratora dzierżawy lub zgodę na każdy użytkownik w dzierżawie. Żądanie uwierzytelniania prawdopodobnie została wysłana do niewłaściwej dzierżawy. \ | (Tylko w systemie Linux) |
| 400 Niewłaściwe żądanie | bad_request_102 | Nie określono nagłówka wymagane metadane | Albo `Metadata` pola nagłówka żądania brakuje żądania lub jest niepoprawnie sformatowana. Wartość musi być określona jako `true`, w małe litery. Zobacz sekcję "przykładowe żądanie" w [pobrania tokenu przy użyciu protokołu HTTP](#get-a-token-using-http) sekcji, na przykład.|
| 401 nieautoryzowane | unknown_source | Nieznane źródło  *\<identyfikatora URI\>* | Sprawdź, żądanie HTTP GET identyfikatora URI jest prawidłowo sformatowany. `scheme:host/resource-path` Części muszą być określone jako `http://169.254.169.254/metadata/identity/oath2/token` lub `http://localhost:50342/oauth2/token`. Zobacz sekcję "przykładowe żądanie" w [pobrania tokenu przy użyciu protokołu HTTP](#get-a-token-using-http) sekcji, na przykład.|
|           | invalid_request | Żądania brakuje wymaganego parametru, obejmuje niepoprawna wartość parametru, zawiera więcej niż raz parametr lub w przeciwnym razie jest nieprawidłowo sformułowany. |  |
|           | unauthorized_client | Klient nie ma uprawnień do żądania tokenu dostępu przy użyciu tej metody. | Przyczyną żądanie, które nie zostały Użyj lokalnego sprzężenia zwrotnego, aby wywołać rozszerzenie, lub na maszynie Wirtualnej, która nie ma poprawnie skonfigurowany Instalatora MSI. Zobacz [skonfigurować maszyny Wirtualnej zarządzane usługi tożsamości (MSI) przy użyciu portalu Azure](msi-qs-configure-portal-windows-vm.md) Jeśli potrzebujesz pomocy w konfiguracji maszyny Wirtualnej. |
|           | access_denied | Właściciel zasobu lub autoryzacji serwer odrzucił żądanie. |  |
|           | unsupported_response_type | Serwer autoryzacji nie obsługuje uzyskiwania tokenu dostępu przy użyciu tej metody. |  |
|           | invalid_scope | Żądany zakres jest nieprawidłowy, nieznany lub źle skonstruowany. |  |
| 500 Wewnętrzny błąd serwera | nieznane | Nie można pobrać tokenu z usługi Active directory. Szczegółowe informacje można znaleźć w dziennikach w  *\<ścieżka pliku\>* | Sprawdź, czy włączono MSI w maszynie Wirtualnej. Zobacz [skonfigurować maszyny Wirtualnej zarządzane usługi tożsamości (MSI) przy użyciu portalu Azure](msi-qs-configure-portal-windows-vm.md) Jeśli potrzebujesz pomocy w konfiguracji maszyny Wirtualnej.<br><br>Sprawdź także, czy żądanie HTTP GET identyfikatora URI jest prawidłowo sformatowane, szczególnie zasób, którego identyfikator URI określony w ciągu zapytania. Zobacz sekcję "przykładowe żądanie" w [pobrania tokenu przy użyciu protokołu HTTP](#get-a-token-using-http) sekcji, na przykład lub [uwierzytelniania pomocy technicznej usługi Azure AD z usług Azure](msi-overview.md#azure-services-that-support-azure-ad-authentication) listę usług i ich odpowiednich identyfikatorów zasobów.

## <a name="resource-ids-for-azure-services"></a>Identyfikatory zasobów dla usług Azure

Zobacz [uwierzytelniania pomocy technicznej usługi Azure AD z usług Azure](msi-overview.md#azure-services-that-support-azure-ad-authentication) listę zasobów, które obsługują usługi Azure AD i zostały przetestowane msi, a ich odpowiednich identyfikatorów zasobów.


## <a name="next-steps"></a>Kolejne kroki

- Aby włączyć MSI w maszynie Wirtualnej platformy Azure, zobacz [skonfigurować przypisany użytkownik zarządzane usługi tożsamości (MSI) dla maszyny Wirtualnej, przy użyciu interfejsu wiersza polecenia Azure](msi-qs-configure-cli-windows-vm.md).

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.








