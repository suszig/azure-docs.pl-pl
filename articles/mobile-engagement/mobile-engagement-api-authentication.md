---
title: "Uwierzytelniania za pomocą interfejsów API REST usługi Mobile Engagement"
description: "Opisuje sposób uwierzytelniania za pomocą interfejsów API REST usługi Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: b05181d9252c0a804648e01b4058019278ae5abe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Uwierzytelniania za pomocą interfejsów API REST usługi Mobile Engagement
## <a name="overview"></a>Omówienie
Ten dokument zawiera opis sposobu uzyskać prawidłowy Oauth AAD token uwierzytelniania za pomocą interfejsów API REST Mobile Engagement. 

Zakłada się, że masz ważnej subskrypcji platformy Azure i utworzono aplikację usługi Mobile Engagement, przy użyciu jednej z naszych [samouczki Developer](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Authentication
Microsoft Azure Active Directory na podstawie tokenu jest używany do uwierzytelniania OAuth. 

Aby żądania uwierzytelnienia interfejsu API nagłówek autoryzacji muszą być dodane do każdego żądania, który jest następujący:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Azure Active Directory tokeny wygaśnie w ciągu godziny.
> 
> 

Istnieje kilka sposobów w celu pobrania tokenu. Ponieważ interfejsy API są zwykle nazywane z usługi w chmurze, ma być używany klucz interfejsu API. Klucz interfejsu API w terminologii Azure jest nazywany hasło główną usługi. W poniższej procedurze opisano jeden ze sposobów konfigurowania go ręcznie.

### <a name="one-time-setup-using-script"></a>Jednorazowej konfiguracji (przy użyciu skryptu)
Należy stosować zestaw instrukcjami poniżej, aby przeprowadzić instalację za pomocą skryptu programu PowerShell, co ma minimalny czas instalacji, ale używa najbardziej dopuszczalnej wartości domyślne. Opcjonalnie można również wykonać instrukcje w [instalacji ręcznej](mobile-engagement-api-authentication-manual.md) Aby to zrobić bezpośrednio portalu Azure i są bardziej precyzyjną konfiguracji. 

1. Pobierz najnowszą wersję programu Azure PowerShell z [tutaj](http://aka.ms/webpi-azps). Aby uzyskać więcej informacji na instrukcje pobierania widać to [łącze](/powershell/azure/overview).  
2. Po zainstalowaniu programu Azure PowerShell, użyj następujących poleceń, aby upewnić się, że masz **modułu Azure** zainstalowane:
   
    a. Upewnij się, że moduł Azure PowerShell jest dostępny na liście dostępnych modułów. 
   
        Get-Module –ListAvailable 
   
    ![Dostępne moduły Azure][1]
   
    b. Jeśli nie zostanie znaleziony moduł Azure PowerShell w powyższej listy, a następnie należy uruchomić następujące:
   
        Import-Module Azure 
3. Logowanie do usługi Azure Resource Manager z programu PowerShell, uruchom następujące polecenie i podając nazwę użytkownika i hasło dla konta platformy Azure: 
   
        Login-AzureRmAccount
4. Jeśli masz wiele subskrypcji należy uruchomić następujące czynności:
   
    a. Pobranie listy wszystkich subskrypcji, a następnie skopiuj element SubscriptionId subskrypcję, której chcesz użyć. Upewnij się, że ta subskrypcja jest taka sama jak mającej Mobile Engagement App, w którym będą wchodzić w interakcje z użyciem interfejsów API. 
   
        Get-AzureRmSubscription
   
    b. Uruchom następujące polecenie podając identyfikator subskrypcji do konfigurowania subskrypcji do użycia.
   
        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Skopiuj tekst [AzureRmServicePrincipalOwner.ps1 nowy](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) skryptu na komputerze lokalnym i zapisz go jako polecenia cmdlet programu PowerShell (np. `APIAuth.ps1`) i wykonaj go `.\APIAuth.ps1`. 
6. Skrypt zostanie wyświetlony monit o podanie danych wejściowych dla **principalName**. Podaj odpowiednią nazwę chcesz używać do tworzenia aplikacji usługi Active Directory (np. APIAuth). 
7. Po ukończeniu działania skryptu, zostaną wyświetlone następujące cztery wartości, które będą potrzebne do uwierzytelnienia programowo z usługą Active Directory tak upewnij się, że skopiuj je. 
   
    **TenantId**, **SubscriptionId**, **ApplicationId**, i **klucz tajny**.
   
    Użyje TenantId jako `{TENANT_ID}`, identyfikator aplikacji jako `{CLIENT_ID}` i tajne jako `{CLIENT_SECRET}`.
   
   > [!NOTE]
   > Domyślne zasady zabezpieczeń blokują uruchomionych skryptów PowerShell. Jeśli tak, tymczasowo należy skonfigurować zasady wykonywania, aby pozwolić na wykonanie skryptu za pomocą następującego polecenia:
   > 
   > Set-ExecutionPolicy RemoteSigned
   > 
   > 
8. Oto jak będzie wyglądać zestaw poleceń cmdlet środowiska PS. 
   
    ![][3]
9. Sprawdź w portalu zarządzania Azure utworzenia nowej aplikacji usługi AD o nazwie dostarczone do skryptu o nazwie **principalName** w obszarze **Pokaż aplikacje Moja firma jest właścicielem**.
   
    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Kroki, aby pobrać prawidłowego tokenu
1. Wywołanie interfejsu API z następującymi parametrami i upewnij się zastąpić DZIERŻAWCY\_identyfikator, klient\_identyfikator i klienta\_klucz TAJNY:
   
   * **Adres URL żądania** jako *https://login.microsoftonline.com/ {dzierżawy\_identyfikator} / oauth2/token.*
   * **Nagłówek HTTP Content-Type** jako *application/x--www-form-urlencoded*
   * **Treść żądania HTTP** jako *przyznać\_typu = klient\_poświadczenia & client_id = {klienta\_identyfikator} & client_secret = {klienta\_klucz TAJNY} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*
     
     Poniżej zamieszczono przykładowe żądanie:
     
       POST / {TENANT_ID} / oauth2/token hostów protokołu HTTP/1.1: login.microsoftonline.com Content-Type: application/x--www-form-urlencoded typ grant_type = client_credentials & client_id = {CLIENT_ID} & client_secret = {CLIENT_SECRET} & staw nazw = https % 3A % 2f%2Fmanagement.Core.Windows.NET%2f
     
     Oto przykład odpowiedzi:
     
       HTTP/1.1 200 OK Content-Type: application/json; charset = utf-8 Content-Length: 1234
     
       {"token_type": "Bearer", "expires_in": "3599", "expires_on": "1445395811", "not_before": "144 5391911 zasobu","": "https://management.core.windows.net/", "' access_token '": {' access_token '}}
     
     W tym przykładzie uwzględnione podczas kodowania adresu URL parametrów POST `resource` wartość jest rzeczywiście `https://management.core.windows.net/`. Należy zachować ostrożność, jak również adres URL kodowania `{CLIENT_SECRET}` jako może zawierać znaków specjalnych.
     
     > [!NOTE]
     > Do testowania, można użyć narzędzia klienta HTTP, takiego jak [Fiddler](http://www.telerik.com/fiddler) lub [rozszerzenia Chrome Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 
     > 
     > 
2. W każdym wywołaniu interfejsu API zawierają teraz nagłówek żądania autoryzacji:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    Jeśli otrzymasz zwrócił kod stanu 401, sprawdź treść odpowiedzi go mogą informować o token utracił ważność. W takim przypadku należy uzyskać nowy token.

## <a name="using-the-apis"></a>Korzystanie z interfejsów API
Teraz, gdy masz prawidłowy token, można przystąpić do wykonywania wywołań interfejsu API.

1. W poszczególnych żądań interfejsu API należy przekazać token prawidłowych, niewygasłych uzyskaną w poprzedniej sekcji.
2. Należy podłączyć niektórych parametrów w żądaniu identyfikatora URI, który identyfikuje aplikację. Żądanie identyfikatora URI wygląda następująco
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    Aby pobrać parametrów, kliknij nazwę aplikacji i kliknij pulpit nawigacyjny i zostanie wyświetlona strona podobnie do następującej z 3 parametry.
   
   * **1** `{subscription-id}`
   * **2** `{app-collection}`
   * **3** `{app-resource-name}`
   * **4** nazwy Twojej grupy zasobów ma być **MobileEngagement** chyba że zostaną utworzone nowe. 
     
     ![Parametry identyfikatora URI interfejsu API usługi Engagement Mobile][2]

> [!NOTE]
> <br/>
> 
> 1. Ignoruj adres głównego interfejsu API, jak to dla poprzedniego interfejsów API.<br/>
> 2. Jeśli utworzono aplikację przy użyciu klasycznego portalu Azure należy użyć nazwy zasobu aplikacji, która jest inna niż nazwa aplikacji. Jeśli utworzono aplikację w portalu Azure należy używać cała nazwa aplikacji (brak rozróżnienia między nazwa zasobów aplikacji i nazwy aplikacji dla aplikacji utworzonych w nowego portalu).  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



