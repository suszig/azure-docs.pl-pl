---
title: "Wprowadzenie do usługi Azure Active Directory Identity Protection oraz Microsoft Graph | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do zapytań programu Microsoft Graph listę zdarzeń o podwyższonym ryzyku i skojarzonych informacji z usługi Azure Active Directory."
services: active-directory
keywords: "ochronę tożsamości usługi Azure active directory, zdarzenie ryzyka, luki w zabezpieczeniach, zasady zabezpieczeń, Microsoft Graph"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 568cad4e394dfedddce1bfce66ddf627947d7568
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Wprowadzenie do usługi Azure Active Directory Identity Protection oraz Microsoft Graph
Program Microsoft Graph jest Microsoft unified punkt końcowy interfejsu API i stroną główną [Azure Active Directory Identity Protection](active-directory-identityprotection.md) interfejsów API. Pierwszy interfejsu API, **identityRiskEvents**, pozwala na zapytanie Microsoft Graph listę [ryzyka zdarzenia](active-directory-identityprotection-risk-events-types.md) i skojarzonych informacji. W tym artykule pobiera pracę podczas badania tego interfejsu API. Szczegółowe wprowadzenie, pełną dokumentację i dostęp do Eksploratora wykresu, zobacz [witryny Microsoft Graph](https://graph.microsoft.io/).

> [!IMPORTANT]
> Firma Microsoft zaleca zarządzanie usługą Azure AD przy użyciu [centrum administracyjnego usługi Azure AD](https://aad.portal.azure.com) w witrynie Azure Portal zamiast korzystania z klasycznej witryny Azure Portal przywołanej w niniejszym artykule.

Istnieją trzy kroki w celu uzyskiwania dostępu do danych tożsamości ochrony za pomocą programu Microsoft Graph:

1. Dodawanie aplikacji przy użyciu klucza tajnego klienta. 
2. Użyć ten klucz tajny i kilka innych rodzajów informacji do uwierzytelnienia do programu Microsoft Graph, sposób ich otrzymywania tokenu uwierzytelniania. 
3. Użyj tego tokenu, aby żądania do punktu końcowego interfejsu API i wrócić tożsamości ochrony danych.

Przed rozpoczęciem pracy należy:

* Uprawnienia administratora, aby utworzyć aplikację w usłudze Azure AD
* Nazwa domeny Twojej dzierżawy (np. contoso.onmicrosoft.com)

## <a name="add-an-application-with-a-client-secret"></a>Dodawanie aplikacji z klucz tajny klienta
1. [Zaloguj się](https://manage.windowsazure.com) do portalu klasycznego Azure jako administrator. 
2. W lewym okienku nawigacji, wybierz polecenie **usługi Active Directory**. 
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. Z **katalogu** listy, wybierz katalog, dla którego chcesz włączyć integracji katalogów.
4. W menu u góry kliknij **aplikacji**.
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. Kliknij przycisk **Dodaj** w dolnej części strony.
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. Na **co chcesz zrobić** okna dialogowego, kliknij przycisk **Dodaj aplikację moją organizację**.
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. Na **Powiedz nam o aplikacji** okna dialogowego, wykonaj następujące czynności:
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    a. W **nazwa** tekstowym, wpisz nazwę dla aplikacji (np.: Aplikacja interfejsu API zdarzenia ryzyka AADIP).
   
    b. Jako **typu**, wybierz pozycję **aplikacji sieci Web i / lub interfejs API sieci Web**.
   
    c. Kliknij przycisk **Dalej**.
8. Na **właściwości aplikacji** okna dialogowego, wykonaj następujące czynności:
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    a. W **adres URL logowania** pole tekstowe, typ `http://localhost`.
   
    b. W **identyfikator URI aplikacji** pole tekstowe, typ `http://localhost`.
   
    c. Kliknij przycisk **Complete** (Zakończ).

Możesz teraz skonfigurować aplikacji.

![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-to-use-the-api"></a>Zezwolić aplikacji za pomocą interfejsu API
1. Na stronie aplikacji, w menu u góry, kliknij przycisk **Konfiguruj**. 
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. W **uprawnień dotyczących innych aplikacji** kliknij **Dodaj aplikację**.
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. Na **uprawnień dotyczących innych aplikacji** okna dialogowego, wykonaj następujące czynności:
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    a. Wybierz **Microsoft Graph**.
   
    b. Kliknij przycisk **Complete** (Zakończ).
4. Kliknij przycisk **uprawnienia aplikacji: 0**, a następnie wybierz **odczytać wszystkie informacje dotyczące zdarzenia ryzyka tożsamości**.
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. Kliknij przycisk **Zapisz** w dolnej części strony.
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a>Uzyskiwanie klucza dostępu
1. Na stronie aplikacji w **klucze** wybierz 1 rok jako czas trwania.
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. Kliknij przycisk **Zapisz** w dolnej części strony.
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. w sekcji klucze skopiować wartości z nowo utworzony klucz, a następnie wklej go w bezpiecznym miejscu.
   
    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > Jeśli ten klucz zostanie utracone, konieczne będzie wróć do tej sekcji i Utwórz nowy klucz. Zachowaj ten klucz tajny: każdy użytkownik, który można uzyskać dostęp do danych.
   > 
   > 
4. W **właściwości** sekcji, skopiuj **identyfikator klienta**, a następnie wklej go w bezpiecznym miejscu. 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Uwierzytelnianie Microsoft Graph i wykonywać zapytania interfejsu API zdarzenia ryzyka tożsamości
W tym momencie powinny mieć:

* Identyfikator klienta skopiowane powyżej
* Klucz skopiowane powyżej
* Nazwa domeny Twojej dzierżawy

W celu uwierzytelnienia wysłanie żądania post do `https://login.microsoft.com` z następującymi parametrami w treści:

* Typ grant_type: "**client_credentials**"
* Zasób: "**https://graph.microsoft.com**"
* client_id:<your client ID>
* client_secret:<your key>

> [!NOTE]
> Należy podać wartości **client_id** i **client_secret** parametru.
> 
> 

Jeśli to się powiedzie, to zwraca token uwierzytelniania.  
Aby wywołać interfejs API, należy utworzyć nagłówek z następującym parametrem:

    `Authorization`=”<token_type> <access_token>"


Podczas uwierzytelniania, można znaleźć typu token i token dostępu w tokenie zwrócony.

Wyślij ten nagłówek jako żądania do następującego adresu URL interfejsu API:`https://graph.microsoft.com/beta/identityRiskEvents`

Odpowiedź, w przypadku powodzenia jest zbierania zdarzeń o podwyższonym ryzyku tożsamości i skojarzone dane w formacie OData JSON, który może być analizowana i obsługiwane zgodnie z własnymi potrzebami.

Oto przykładowy kod do uwierzytelniania i wywołanie interfejsu API przy użyciu programu Powershell.  
Wystarczy dodać Identyfikatora klienta, a dla klucza dzierżawy domeny.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Następne kroki
Gratulacje, wystarczy wprowadzić Twoje pierwsze wywołanie do programu Microsoft Graph!  
Teraz można zbadać zdarzenia o podwyższonym ryzyku tożsamości i korzystanie z danych, jednak użytkownik mieści się w temacie.

Aby dowiedzieć się więcej na temat programu Microsoft Graph i jak tworzyć aplikacje przy użyciu interfejsu API programu Graph, zapoznaj się [dokumentacji](https://graph.microsoft.io/docs) i znacznie więcej informacji na temat [witryny Microsoft Graph](https://graph.microsoft.io/). Ponadto upewnij się, że zakładki [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) strony, która zawiera listę wszystkich interfejsów API ochrony tożsamości, które są dostępne na wykresie. Jak możemy dodać nowe sposoby pracy z ochrony tożsamości za pomocą interfejsu API, zobaczysz je na tej stronie.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Ochronę tożsamości usługi Azure Active Directory](active-directory-identityprotection.md)
* [Rodzaje zdarzeń o podwyższonym ryzyku wykrywanych przez Azure Active Directory Identity Protection](active-directory-identityprotection-risk-events-types.md)
* [Microsoft Graph](https://graph.microsoft.io/)
* [Omówienie programu Microsoft Graph](https://graph.microsoft.io/docs)
* [Katalogu głównego usługi programu Azure AD Identity Protection](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

