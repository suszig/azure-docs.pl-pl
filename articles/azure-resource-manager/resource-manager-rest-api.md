---
title: "Interfejsy API REST usługi Resource Manager | Dokumentacja firmy Microsoft"
description: "Przegląd przykładów uwierzytelniania i użycia interfejsów API REST usługi Resource Manager"
services: azure-resource-manager
documentationcenter: na
author: navalev
manager: timlt
editor: 
ms.assetid: e8d7a1d2-1e82-4212-8288-8697341408c5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: navale;tomfitz;
ms.openlocfilehash: 2f7ba23775545637de865f9ef63680ae22c62164
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="resource-manager-rest-apis"></a>Interfejsy API REST Menedżera zasobów
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Interfejs wiersza polecenia platformy Azure](xplat-cli-azure-resource-manager.md)
> * [Portal](resource-group-portal.md) 
> * [Interfejs API REST](resource-manager-rest-api.md)
> 
> 

Za każdego wywołania do usługi Azure Resource Manager za każdy wdrożonej szablon za każdego konta skonfigurowanego magazynu ma co najmniej jednego wywołania interfejsu API RESTful Menedżera zasobów Azure. Ten temat jest przeznaczony do tych interfejsów API i sposób ich wywołania bez przy użyciu dowolnego zestawu SDK w ogóle. Ta metoda jest przydatna, jeśli mają pełną kontrolę nad żądań na platformie Azure lub jeśli zestaw SDK dla preferowany język jest niedostępny lub nie obsługuje operacji.

W tym artykule nie przechodzi przez każdy interfejs API, który jest narażony na platformie Azure, ale raczej używa niektóre operacje jako przykłady sposobu połączenia ich. Po zrozumieniu podstaw możesz przeczytać [Azure Resource Manager REST API Reference](https://docs.microsoft.com/rest/api/resources/) można znaleźć szczegółowe informacje na temat korzystania z resztą interfejsów API.

## <a name="authentication"></a>Authentication
Uwierzytelnianie dla Menedżera zasobów jest obsługiwane przez usługi Azure Active Directory (AD). Aby połączyć się z jakiegokolwiek interfejsu API, należy najpierw uwierzytelniania za pomocą usługi Azure AD do tokenu uwierzytelniania, które można przekazać do każdego żądania odbierania. Jak możemy zawierająca opis czysty wywołania bezpośrednio do interfejsów API REST przyjęto założenie, że nie chcesz uwierzytelniać przy wyświetlaniu monitu o nazwę użytkownika i hasło. Możemy również założenie, że nie używasz dwa mechanizmy uwierzytelniania Multi-Factor. W związku z tym utworzymy tak zwany aplikacji usługi Azure AD i usługę podmiotu zabezpieczeń, które są używane podczas logowania się. Należy jednak pamiętać, że usługi Azure AD obsługuje kilka procedur uwierzytelniania i wszystkich z nich można pobrać tokenu uwierzytelniania wymagane dla kolejnych żądań interfejsu API.
Postępuj zgodnie z [Azure tworzenie aplikacji usługi AD i nazwę główną usługi](resource-group-create-service-principal-portal.md) instrukcje krok po kroku.

### <a name="generating-an-access-token"></a>Generuje Token dostępu
Uwierzytelnianie w usłudze Azure AD odbywa się przez wywołanie do usługi Azure AD, znajdujący się w login.microsoftonline.com. Na potrzeby uwierzytelniania, musisz mieć następujące informacje:

* Identyfikator dzierżawy usługi Azure AD (nazwa tej usługi Azure AD są przy użyciu logowania, często taki sam, jak firma, ale nie jest konieczna)
* Identyfikator aplikacji (podjętych podczas kroku tworzenia aplikacji usługi Azure AD)
* Hasło (wybranej podczas tworzenia aplikacji usługi Azure AD)

W poniższych żądania HTTP upewnij się, że Zamień poprawne wartości "Identyfikator dzierżawy Azure AD", "Identyfikator aplikacji" i "Password".

**Rodzajowe żądanie HTTP:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... zostanie (Jeśli uwierzytelnianie zakończy się powodzeniem) powoduje odpowiedź podobną do poniższej odpowiedzi:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(' Access_token ' w poprzedniej odpowiedzi zostały skrócone Aby zwiększyć czytelność)

**Generowanie tokenu dostępu, używając Bash:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Generowanie tokenu dostępu przy użyciu programu PowerShell:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Odpowiedź zawiera token dostępu, informacje o tym, jak długo token jest prawidłowy i informacje o jakie zasobów można użyć tokenu dla.
Token dostępu otrzymany w poprzedniego wywołania HTTP muszą być przekazywane w dla wszystkich żądań do interfejsu API Menedżera zasobów. Przekaż go jako wartość nagłówka o nazwie "Autoryzacja" o wartości "Bearer YOUR_ACCESS_TOKEN". Zwróć uwagę, odstęp między "Bearer" i tokenu dostępu.

Jak widać w powyższym wyników HTTP, token jest prawidłowy w danym okresie czasu, w którym powinien pamięci podręcznej i ponowne użycie tego samego tokenu. Nawet jeśli jest możliwe do uwierzytelniania usługi Azure AD dla każdego wywołania interfejsu API, może być bardzo mało wydajne.

## <a name="calling-resource-manager-rest-apis"></a>Menedżer zasobów wywołania interfejsów API REST
W tym temacie używa tylko kilka interfejsów API wyjaśnić podstawowe sposoby użycia operacji REST. Aby uzyskać informacje o wszystkich operacji, zobacz [interfejsów API REST usługi Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).

### <a name="list-all-subscriptions"></a>Wyświetl listę wszystkich subskrypcji
Jednym z najprostszym operacje, które może wykonywać jest lista dostępnych subskrypcji, do których masz dostęp. W poniższych zostanie wyświetlony, jak token dostępu jest przekazywany jako nagłówek:

(Zamiast YOUR_ACCESS_TOKEN Twojego rzeczywiste Token dostępu).

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... i w związku z tym uzyskać listę subskrypcji, które może uzyskać dostępu do tej nazwy głównej usługi

(Identyfikatory subskrypcji zostały skrócone dla czytelności)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Wyświetl listę wszystkich grup zasobów w określonej subskrypcji
Wszystkie zasoby dostępne z interfejsów API Menedżera zasobów są zagnieżdżone w grupie zasobów. Menedżer zasobów dla istniejącej grupy zasobów można badać w ramach subskrypcji, za pomocą następujących żądania HTTP GET. Zwróć uwagę, jak identyfikator subskrypcji jest przekazywany jako część adresu URL teraz.

(Zamiast YOUR_ACCESS_TOKEN i IDENTYFIKATOR_SUBSKRYPCJI dostępu rzeczywisty identyfikator token i subskrypcji)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Otrzymasz odpowiedź zależy od tego, czy zdefiniowano żadnych grup zasobów, a jeśli tak, jak wiele.

(Identyfikatory subskrypcji zostały skrócone dla czytelności)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Do tej pory firma Microsoft już tylko zostały zapytań interfejsów API Menedżera zasobów informacji. Nadszedł czas, możemy utworzyć niektórych zasobów i Zacznijmy od Najprostszym z nich wszystkie grupy zasobów. Następujące żądania HTTP tworzy grupę zasobów w regionu/lokalizacji, a następnie dodaje tag do niej.

(Zastąp YOUR_ACCESS_TOKEN, IDENTYFIKATOR_SUBSKRYPCJI, RESOURCE_GROUP_NAME z rzeczywistego tokenu dostępu, identyfikator subskrypcji i nazwę grupy zasobów, aby utworzyć)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

W przypadku powodzenia można uzyskać odpowiedzi, która jest podobna do poniższej odpowiedzi:

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Pomyślnie utworzona grupa zasobów na platformie Azure. Gratulacje!

### <a name="deploy-resources-to-a-resource-group-using-a-resource-manager-template"></a>Wdrażanie zasobów w grupie zasobów, przy użyciu szablonu usługi Resource Manager
Usługa Resource Manager można wdrożyć zasobów za pomocą szablonów. Szablon definiuje kilka zasobów oraz ich zależności. Dla tej sekcji przyjęto założenie, znasz szablony Menedżera zasobów i firma Microsoft tylko pokazano, jak wykonać wywołanie interfejsu API rozpocząć wdrażanie. Aby uzyskać więcej informacji na temat tworzenia szablonów, zobacz [szablonów Authoring Azure Resource Manager](resource-group-authoring-templates.md).

Wdrożenie szablonu nie różnią się znacznie sposób wywołania innych interfejsów API. Jeden istotnym elementem jest, że wdrażania szablonu może zająć sporo czasu. Po prostu zwraca wywołania interfejsu API i jest możesz jako Deweloper na zapytanie o stan wdrożenia, aby dowiedzieć się, gdy wdrożenie jest przeprowadzane. Aby uzyskać więcej informacji, zobacz [śledzić operacje asynchroniczne Azure](resource-manager-async-operations.md).

W tym przykładzie używamy publicznie ujawnionych szablon dostępny na [GitHub](https://github.com/Azure/azure-quickstart-templates). Szablon, który używamy wdraża Maszynę wirtualną systemu Linux do regionu zachodnie stany USA. Mimo że w tym przykładzie używa szablonu dostępne w publicznych repozytorium, takich jak GitHub, zamiast tego można przekazać pełny szablon jako część żądania. Należy pamiętać, że firma Microsoft wartości parametrów w żądaniu, które są używane w szablonie wdrożone.

(Zastąp IDENTYFIKATOR_SUBSKRYPCJI, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD i DNS_NAME_FOR_PUBLIC_IP wartości odpowiednich dla żądania)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Aby poprawić czytelność niniejszej dokumentacji została pominięta długich odpowiedzi JSON dla tego żądania. Odpowiedź zawiera informacje dotyczące wdrażania opartego na szablonie, utworzony.

## <a name="next-steps"></a>Następne kroki

- Informacje na temat obsługi operacji asynchronicznych REST, zobacz [śledzić operacje asynchroniczne Azure](resource-manager-async-operations.md).
