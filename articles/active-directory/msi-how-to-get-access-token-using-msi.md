---
title: "Jak używać Azure VM zarządzane tożsamość usługi nabycia logowania i tokenu"
description: "Krok po kroku instrukcje dotyczące używania Instalatora MSI maszyny Wirtualnej Azure usługi główna dla logowania i uzyskania tokenu dostępu."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/17/2017
ms.author: bryanla
ms.openlocfilehash: 905e7b0d8a0c45c98a86882a8c8f387be0950f9f
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in-and-token-acquisition"></a>Jak używać Azure VM zarządzane usługi tożsamości (MSI) nabycia logowania i tokenu 
[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]Po włączeniu MSI w maszynie Wirtualnej platformy Azure, można użyć MSI dla logowania i na żądanie tokenu dostępu. W tym artykule przedstawiono różne sposoby użycia Instalatora MSI [nazwy głównej usługi](develop/active-directory-dev-glossary.md#service-principal-object) dla logowania i uzyskać [token dostępu tylko do aplikacji](develop/active-directory-dev-glossary.md#access-token) dostęp do innych zasobów, w tym:

- Silent/nienadzorowanej logowanie z programu PowerShell lub interfejsu wiersza polecenia platformy Azure
- Token nabycia dla różnych klientów, w tym .NET, programu PowerShell, Bash/ZWINIĘCIE, REST
- Zaloguj się przy użyciu zestawu Azure SDK, w tym .NET, Java, Node.js, Python, Ruby

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

Jeśli planujesz użyć programu PowerShell przykłady w tym artykule, należy zainstalować [Azure PowerShell w wersji 4.3.1](https://www.powershellgallery.com/packages/AzureRM) lub nowszej. Jeśli planujesz użyć przykłady interfejsu wiersza polecenia Azure, w tym artykule są trzy opcje:
- Użyj [powłoki chmury Azure](../cloud-shell/overview.md) z portalu Azure.
- Użyć osadzonego powłoki chmury Azure za pośrednictwem "Spróbuj on" przycisk, znajdujący się w prawym górnym rogu bloki kodu wiersza polecenia platformy Azure.
- [Zainstaluj najnowszą wersję interfejsu wiersza polecenia 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 lub nowszym) Jeśli wolisz korzystać z interfejsu wiersza polecenia w wierszu polecenia lokalnego. 


> [!IMPORTANT]
> - Wszystkie próbki kodu skryptu w tym artykule przyjęto założenie, klient jest uruchomiony na maszynie wirtualnej włączone MSI. Aby uzyskać więcej informacji na temat włączania MSI w maszynie Wirtualnej, zobacz [skonfigurować maszyny Wirtualnej zarządzane usługi tożsamości (MSI) przy użyciu portalu Azure](msi-qs-configure-portal-windows-vm.md), lub jednego z artykułów variant (przy użyciu programu PowerShell, interfejsu wiersza polecenia, szablonu lub zestawu SDK platformy Azure). 
> - Aby zapobiec błędom autoryzacji (403/AuthorizationFailed) w przykładach kodu skryptu tożsamości maszyny Wirtualnej muszą mieć "Czytnika" dostęp na zakres maszyny Wirtualnej, aby operacje usługi Azure Resource Manager na maszynie Wirtualnej. Zobacz [przypisywanie dostępu zarządzane tożsamości usługi (MSI) do zasobów przy użyciu portalu Azure](msi-howto-assign-access-portal.md) szczegółowe informacje.
> - Przed przystąpieniem do jednej z następujących sekcji, funkcja VM "Połącz" w portalu Azure na połączenie zdalne do maszyny Wirtualnej z włączoną MSI.

## <a name="how-to-sign-in-from-powershell-or-cli-using-msi"></a>Jak się zalogować z programu PowerShell lub interfejsu wiersza polecenia przy użyciu pliku MSI

Wcześniej uruchomienie skryptu w ramach własnej tożsamości przeznaczone:
- rejestrując go jako aplikacja kliencka poufne/sieci web w usłudze Azure AD
- Logowanie przy użyciu poświadczeń identyfikator/klucz tajny klienta aplikacji

MSI klient skryptu już musi zarejestrować w usłudze Azure AD ani poświadczenia. W poniższych przykładach zostanie przedstawiony sposób użycia MSI w maszynie Wirtualnej nazwy głównej usługi dla logowania.

### <a name="azure-powershell"></a>Azure PowerShell

Poniższy skrypt pokazuje, jak:

- uzyskać token dostępu MSI dla maszyny Wirtualnej platformy Azure
- Użyj tokenu dostępu do logowania do usługi Azure AD w odpowiedniej nazwy głównej usługi MSI
- Użyj nazwy głównej usługi MSI do wywoływania usługi Azure Resource Manager, aby uzyskać identyfikator nazwy głównej usługi

```powershell
# Get an access token from MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token

# Use the access token to sign in under the MSI service principal
Login-AzureRmAccount -AccessToken $access_token -AccountId “CLIENT”

# The MSI service principal is now signed in for this session.
# Next, a call to Azure Resource Manager is made to get the service principal ID for the VM's MSI. 
$spID = (Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>).identity.principalid
echo "The MSI service principal ID is $spID"
```
   
### <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

Poniższy skrypt pokazuje, jak:

- Zaloguj się do usługi Azure AD w obszarze nazwy głównej usługi MSI maszyny Wirtualnej
- Użyj nazwy głównej usługi MSI do wywoływania usługi Azure Resource Manager, aby uzyskać identyfikator nazwy głównej usługi

```azurecli-interactive
az login --msi
spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
echo The MSI service principal ID is $spID
```

## <a name="how-to-acquire-an-access-token-from-msi"></a>Jak uzyskać token dostępu z pliku MSI

Za pomocą Instalatora MSI, Twojej aplikacji/skrypt po stronie klienta nie musi zarejestrować w usłudze Azure AD, ani prezentować klienta identyfikator i klucz tajny, aby uzyskać token dostępu. Klienta można po prostu uzyskać lokalnego punktu końcowego MSI tokenu dostępu, bez przedstawiania poświadczenia aplikacji. Wystawiony token dostępu tylko do aplikacji dla jednostki usługi MSI, odpowiednie do użycia jako tokenu elementu nośnego w [service-to-service wymaga poświadczeń klienta wymagające](active-directory-protocols-oauth-service-to-service.md).

W poniższych przykładach zostanie przedstawiony sposób użycia tokenu nabycia MSI w maszynie Wirtualnej.

### <a name="net"></a>.NET

> [!IMPORTANT]
> Azure AD Authentication Library (ADAL) nie jest zintegrowany z pliku MSI. Aby uzyskać token dostępu za pomocą Instalatora MSI, należy wysłać żądanie lokalnego punktu końcowego MSI w maszynie Wirtualnej. Aby uzyskać więcej informacji, zobacz [MSI — często zadawane pytania i znane problemy](msi-known-issues.md#frequently-asked-questions-faqs).

```csharp
// Build request to acquire MSI token
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://localhost:50342/oauth2/token?resource=https://management.azure.com/");
request.Headers["Metadata"] = "true";
request.Method = "GET";

try
{
    // Call /token endpoint
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader, and extract access token
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    string accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

```

### <a name="azure-powershell-token"></a>Program Azure PowerShell

```powershell
# Get an access token from MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"
```

### <a name="bashcurl"></a>Bash/CURL

```bash
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

### <a name="rest"></a>REST

Przykładowe żądanie:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Element | Opis |
| ------- | ----------- |
| `GET` | Zlecenie HTTP wskazująca, że chcesz pobrać dane z punktu końcowego. W takim przypadku tokenu dostępu protokołu OAuth. | 
| `http://localhost:50342/oauth2/token` | MSI punktu końcowego, gdzie 50342 jest domyślnym portem i można go skonfigurować. |
| `resource` | Parametr ciągu zapytania, wskazującą aplikację identyfikator URI zasobu docelowego. Jest także wyświetlany w `aud` oświadczeń (odbiorcy) wystawionego tokenu. W tym przykładzie prosimy token dostępu usługi Azure Resource Manager, który ma identyfikator URI aplikacji z https://management.azure.com/ do. |
| `Metadata` | Pola nagłówka żądania HTTP, wymagane przez Instalatora MSI jako ograniczenie przed atakiem serwera po stronie żądania Międzywitrynowego (SSRF). Musi mieć ustawioną wartość "prawda", w małe litery.

Przykładowa odpowiedź:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Element | Opis |
| ------- | ----------- |
| `access_token` | Żądany dostęp token. Podczas wywoływania metody interfejsu API REST, token jest osadzony w `Authorization` pola nagłówka żądania jako token "bearer", umożliwiając interfejsu API do uwierzytelniania obiektu wywołującego. | 
| `refresh_token` | Nie są używane przez Instalatora MSI. |
| `expires_in` | Liczba sekund, przez które token dostępu w dalszym ciągu był prawidłowy, zanim wygaśnie od czasu wydania. Czas wystawiania można znaleźć w tokenie `iat` oświadczeń. |
| `expires_on` | Zakres czasu wygaśnięcia tokenu dostępu. Data jest reprezentowana jako liczba sekund od "1970-01-01T0:0:0Z UTC" (odpowiada tokenu `exp` oświadczenia). |
| `not_before` | Timespan, gdy token dostępu obowiązuje i mogą być akceptowane. Data jest reprezentowana jako liczba sekund od "1970-01-01T0:0:0Z UTC" (odpowiada tokenu `nbf` oświadczenia). |
| `resource` | Żądany zasób token dostępu został dla odpowiadający identyfikatorowi `resource` żądania parametr ciągu zapytania. |
| `token_type` | Typ tokenu, który jest "Bearer" tokenu dostępu, co oznacza, że zasób pozwala uzyskiwać dostęp do elementu nośnego tego tokenu. |

## <a name="how-to-use-msi-with-azure-sdk-libraries"></a>Jak używać MSI z bibliotekami zestawu Azure SDK

Azure obsługuje wiele platform programowania przez szereg [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads). Niektóre z nich zostały zaktualizowane do obsługi logowania za pomocą Instalatora MSI i podaj odpowiednie próbek, aby zademonstrować sposób użycia. Ta lista jest aktualizowana w miarę dodawania dodatkowego pomocy technicznej:

| SDK | Przykład |
| --- | ------ | 
| .NET | [Wdrażanie szablonu ARM z maszyny Wirtualnej systemu Windows przy użyciu tożsamości usługi zarządzania](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core | [Wywoływanie usług platformy Azure z maszyny Wirtualnej systemu Linux przy użyciu tożsamości usługi zarządzania](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js| [Zarządzanie zasobami za pomocą tożsamości zarządzanych usługi](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python | [Użyj Instalatora MSI w celu uwierzytelniania tylko z wewnątrz maszyny Wirtualnej](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby   | [Zarządzanie zasobami za pomocą maszyny Wirtualnej z włączoną MSI](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) | 

## <a name="additional-information"></a>Dodatkowe informacje

### <a name="token-expiration"></a>Wygaśnięcia tokenu

Podsystemu lokalnego MSI buforuje tokenów. W związku z tym można wywołać ją tyle razy, ile Ci się podoba i powoduje wywołanie w locie do usługi Azure AD tylko wtedy, gdy:
- Chybienia pamięci podręcznej występuje ze względu na nie tokenu w pamięci podręcznej
- token utracił ważność

Kod w pamięci podręcznej tokenu, powinny być przygotowane do obsługi scenariuszy, w którym zasobu wskazuje, czy token utracił ważność.

### <a name="resource-ids-for-azure-services"></a>Identyfikatory zasobów dla usług Azure

Zobacz [uwierzytelniania pomocy technicznej usługi Azure AD z usług Azure](msi-overview.md#azure-services-that-support-azure-ad-authentication) listę usług, które obsługują MSI, a ich odpowiednich identyfikatorów zasobów.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="sign-in-or-token-acquisition-fails"></a>Nabycia logowania lub tokenu nie powiedzie się.

Sprawdź, czy MSI została prawidłowo włączona. Wróć do maszyny Wirtualnej platformy Azure w ramach [portalu Azure](https://portal.azure.com) oraz:

- Szukaj na stronie "Konfiguracja" i upewnij się, MSI włączone = "Yes".
- Przyjrzyj się Strona "Rozszerzenia" i upewnij się, z rozszerzeniem MSI pomyślnie wdrożone.

Jeśli jest albo nieprawidłowa, może być konieczne ponownie Wdróż ponownie MSI na zasobie oraz rozwiązywania problemów dotyczących niepowodzenia wdrożenia.

### <a name="http-request-error-responses"></a>Odpowiedzi na błąd żądania HTTP

- *bad_request_102: nie określono nagłówka wymagane metadane*

  Albo `Metadata` pola nagłówka żądania brakuje żądania lub jest niepoprawnie sformatowana. Wartość musi być określona jako `true`, w małe litery. Zobacz sekcję "przykładowe żądanie" w [powyższej sekcji REST](#rest) przykład.

- *Nieznany: nie można pobrać tokenu z usługi Active directory. Szczegółowe informacje można znaleźć w dziennikach w \<ścieżka pliku\>*

  Sprawdź, czy żądanie HTTP GET identyfikatora URI jest prawidłowo sformatowane, szczególnie zasób, którego identyfikator URI określony w ciągu zapytania. Zobacz sekcję "przykładowe żądanie" w [powyższej sekcji REST](#rest) przykład lub [uwierzytelniania pomocy technicznej usługi Azure AD z usług Azure](msi-overview.md#azure-services-that-support-azure-ad-authentication) listę usług i ich odpowiednich identyfikatorów zasobów.

- *unknown_source: Nieznane źródło \<identyfikatora URI\>*

  Sprawdź, żądanie HTTP GET identyfikatora URI jest prawidłowo sformatowany. `scheme:host/resource-path` Części muszą być określone jako `http://localhost:50342/oauth2/token`. Zobacz sekcję "przykładowe żądanie" w [powyższej sekcji REST](#rest) przykład.

## <a name="related-content"></a>Zawartość pokrewna

- Omówienie MSI, zobacz [omówienie zarządzane tożsamość usługi](msi-overview.md).
- Aby włączyć MSI w maszynie Wirtualnej platformy Azure, zobacz [skonfigurować Azure VM zarządzane usługi tożsamości (MSI) przy użyciu programu PowerShell](msi-qs-configure-powershell-windows-vm.md).

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.

