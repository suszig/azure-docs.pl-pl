---
title: "Przykłady interfejsu API raport aktywności logowania w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Jak rozpocząć pracę z usługi Azure Active Directory raportowania interfejsu API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/18/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d8fec08deb44ec1cbb58f45ff5a01d348d99945a
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Przykłady interfejsu API raport aktywności logowania w usłudze Azure Active Directory
Ten temat jest częścią zbiór tematów dotyczących usługi Azure Active Directory raportowania interfejsu API.  
Raportowanie na platformie Azure AD zapewnia interfejs API, który umożliwia dostęp do danych działań logowania za pomocą kodu lub narzędzia pokrewne.  
Zakres tego tematu jest dostarczają przykładowy kod **logowania działanie interfejsu API**.

Zobacz:

* [Dzienniki inspekcji](active-directory-reporting-azure-portal.md#activity-reports) Aby uzyskać więcej informacji o pojęciach
* [Wprowadzenie do usługi Azure Active Directory interfejsu API raportowania](active-directory-reporting-api-getting-started.md) Aby uzyskać więcej informacji na temat raportowania interfejsu API.


## <a name="prerequisites"></a>Wymagania wstępne
Zanim użyjesz przykłady w tym temacie, trzeba wykonać [wymagania wstępne dotyczące raportowania interfejsu API usługi Azure AD dostęp](active-directory-reporting-api-prerequisites.md).  

## <a name="powershell-script"></a>Skrypt programu PowerShell
    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.microsoftonline.com/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"

    $i=0

    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {

        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Wykonanie skryptu
Zwracany jest po Zakończ edycję skryptu, uruchom go i sprawdź, czy oczekiwane dane z inspekcji logowania raportu.

Skrypt zwraca dane wyjściowe z raportu logowania w formacie JSON. Tworzy również `SigninActivities.json` plik o tych samych danych wyjściowych. Możesz eksperymentować, modyfikując skryptu, aby zwrócić dane z innych raportów ujmij w komentarz formatów wyjściowych, które nie ma potrzeby.

## <a name="next-steps"></a>Następne kroki
* Czy chcesz dostosować przykłady w tym temacie? Zapoznaj się z [działania usługi Azure Active Directory logowania dokumentacja interfejsu API](active-directory-reporting-api-sign-in-activity-reference.md). 
* Jeśli chcesz zobaczyć pełny przegląd przy użyciu usługi Azure Active Directory interfejsem API raportowania, zobacz [wprowadzenie do korzystania z usługi Azure Active Directory interfejsem API raportowania](active-directory-reporting-api-getting-started.md).
* Jeśli chcesz dowiedzieć się więcej o usłudze Azure Active Directory, zobacz [Azure Active Directory Przewodnik po raportach](active-directory-reporting-guide.md).  

