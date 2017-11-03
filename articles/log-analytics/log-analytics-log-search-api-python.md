---
title: "Skrypt w języku Python do pobierania danych z usługi Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Interfejs API dziennika analizy dziennika wyszukiwania umożliwia dowolnego klienta interfejsu API REST do pobierania danych z obszaru roboczego analizy dzienników.  Ten artykuł zawiera przykładowy skrypt języka Python za pomocą interfejsu API Search dziennika."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2017
ms.author: bwren
ms.openlocfilehash: 56d7c6dc648a01e7b0efc167cb65c94bac5468ec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="retrieve-data-from-log-analytics-with-a-python-script"></a>Pobieranie danych z analizy dzienników przy użyciu skryptu języka Python
[Interfejs API dziennika analizy dziennika wyszukiwania](log-analytics-log-search-api.md) umożliwia dowolnego klienta interfejsu API REST do pobierania danych z obszaru roboczego analizy dzienników.  W tym artykule przedstawiono przykładowy skrypt języka Python, wykorzystujący interfejs API dziennika analizy dziennika wyszukiwania.  

## <a name="authentication"></a>Authentication
Ten skrypt używa nazwy głównej usługi w usłudze Azure Active Directory do uwierzytelniania do obszaru roboczego.  Nazwy główne usług Zezwalaj aplikacji klienckiej na żądanie usługi uwierzytelniania konta, nawet jeśli klient nie ma nazwy konta. Przed wykonaniem tego skryptu, należy utworzyć procesu przy użyciu nazwy głównej usługi [użycia portalu do tworzenia aplikacji i usług podmiot zabezpieczeń, który ma dostęp do zasobów usługi Azure Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md).  Należy podać identyfikator aplikacji, identyfikator dzierżawy i klucz uwierzytelniania do skryptu. 

> [!NOTE]
> Gdy użytkownik [utworzyć konto usługi Automatyzacja Azure](../automation/automation-create-standalone-account.md), tworzony jest nazwy głównej usługi, który nadaje się do tego skryptu za pomocą.  Jeśli masz już nazwy głównej usługi utworzonych przez usługi Automatyzacja Azure, powinno być możliwe do użycia zamiast tworzenia nowej, chociaż może być konieczne [utworzyć klucz uwierzytelniania](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) Jeśli go nie ma jeszcze.

## <a name="script"></a>Skrypt
``` python
import adal
import requests
import json
import datetime
from pprint import pprint

# Details of workspace.  Fill in details for your workspace.
resource_group = 'xxxxxxxx'
workspace = 'xxxxxxxx'

# Details of query.  Modify these to your requirements.
query = "Type=Event"
end_time = datetime.datetime.utcnow()
start_time = end_time - datetime.timedelta(hours=24)
num_results = 100  # If not provided, a default of 10 results will be used.

# IDs for authentication.  Fill in values for your service principal.
subscription_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'
tenant_id = 'xxxxxxxx-xxxx-xxxx-xxx-xxxxxxxxxxxx'
application_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx'
application_key = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'

# URLs for authentication
authentication_endpoint = 'https://login.microsoftonline.com/'
resource  = 'https://management.core.windows.net/'

# Get access token
context = adal.AuthenticationContext('https://login.microsoftonline.com/' + tenant_id)
token_response = context.acquire_token_with_client_credentials('https://management.core.windows.net/', application_id, application_key)
access_token = token_response.get('accessToken')

# Add token to header
headers = {
    "Authorization": 'Bearer ' + access_token,
    "Content-Type":'application/json'
}

# URLs for retrieving data
uri_base = 'https://management.azure.com'
uri_api = 'api-version=2015-11-01-preview'
uri_subscription = 'https://management.azure.com/subscriptions/' + subscription_id
uri_resourcegroup = uri_subscription + '/resourcegroups/'+ resource_group
uri_workspace = uri_resourcegroup + '/providers/Microsoft.OperationalInsights/workspaces/' + workspace
uri_search = uri_workspace + '/search'

# Build search parameters from query details
search_params = {
        "query": query,
        "top": num_results,
        "start": start_time.strftime('%Y-%m-%dT%H:%M:%S'),
        "end": end_time.strftime('%Y-%m-%dT%H:%M:%S')
        }

# Build URL and send post request
uri = uri_search + '?' + uri_api
response = requests.post(uri,json=search_params,headers=headers)

# Response of 200 if successful
if response.status_code == 200:

    # Parse the response to get the ID and status
    data = response.json()
    search_id = data["id"].split("/")
    id = search_id[len(search_id)-1]
    status = data["__metadata"]["Status"]

    # If status is pending, then keep checking until complete
    while status == "Pending":

        # Build URL to get search from ID and send request
        uri_search = uri_search + '/' + id
        uri = uri_search + '?' + uri_api
        response = requests.get(uri,headers=headers)

        # Parse the response to get the status
        data = response.json()
        status = data["__metadata"]["Status"]

else:

    # Request failed
    print (response.status_code)
    response.raise_for_status()

print ("Total records:" + str(data["__metadata"]["total"]))
print ("Returned top:" + str(data["__metadata"]["top"]))
pprint (data["value"])
```
## <a name="next-steps"></a>Następne kroki
- Dowiedz się więcej o [interfejs API dziennika analizy dziennika wyszukiwania](log-analytics-log-search-api.md).