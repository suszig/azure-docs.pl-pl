---
title: "Przekształcanie i chronić interfejs API usługi Azure API Management | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak chronić interfejs API za pomocą zasad przydziałów i dławienia (ograniczania liczby wywołań)."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 772f3828d85c54e7b8bb44c857e555175b7444cc
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="transform-and-protect-your-api"></a>Przekształcanie i chronić interfejsu API 

Samouczek przedstawia sposób przekształcania interfejsu API, więc nie ujawnia informacji prywatnych wewnętrznej bazy danych. Na przykład można ukryć informacji o stos technologiczny, który działa na wewnętrznej bazy danych. Można również ukryć oryginalne adresy URL, które znajdują się w treści odpowiedzi HTTP interfejsu API i zamiast tego przekierowanie do bramy APIM.

Ten samouczek również pokazuje, jak łatwo jest dodać ochrony interfejs API zaplecza, konfigurując limit szybkości z usługą Azure API Management. Na przykład można ograniczyć liczbę wywołań interfejsu API jest wywoływana, więc jego jest nie nadmiernego obciążenia przez deweloperów. Aby uzyskać więcej informacji, zobacz [zasad interfejsu API zarządzania](api-management-policies.md)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Przekształć interfejs API do paska nagłówki odpowiedzi
> * Zamień na adresy URL bramy APIM oryginalne adresy URL w treści odpowiedzi interfejsu API
> * Ochrona interfejsu API przez dodanie zasady limit szybkości (przepustowości)
> * Testowanie — przekształcenia

![Zasady](./media/transform-api/api-management-management-console.png)

## <a name="prerequisites"></a>Wymagania wstępne

+ Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md).
+ Ponadto Ukończ samouczek następujących: [Import i opublikować swój pierwszy interfejs API](import-and-publish.md).
 
[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="transform-an-api-to-strip-response-headers"></a>Przekształć interfejs API do paska nagłówki odpowiedzi

W tej sekcji przedstawiono sposób ukrywanie nagłówków HTTP, które nie mają do wyświetlenia użytkownikom. W tym przykładzie usuwany następujące nagłówki odpowiedzi HTTP:

* **X obsługiwane przez**
* **X AspNet wersji**

### <a name="test-the-original-response"></a>Testowanie oryginalnego odpowiedzi

Aby wyświetlić oryginalne odpowiedzi:

1. Wybierz **interfejsu API** kartę.
2. Kliknij przycisk **API konferencji pokaz** z listy interfejsu API.
3. Wybierz **GetSpeakers** operacji.
4. Kliknij przycisk **testu** kartę w górnej części ekranu.
5. Naciśnij klawisz **wysyłania** u dołu ekranu. 

    Jak widać oryginalnego odpowiedzi wygląda następująco:

    ![Zasady](./media/transform-api/original-response.png)

### <a name="set-the-transformation-policy"></a>Ustawienie zasad przekształcania

1. Przejdź do swojego wystąpienia APIM.
2. Wybierz **interfejsu API** kartę.
3. Kliknij przycisk **API konferencji pokaz** z listy interfejsu API.
4. Wybierz **wszystkie operacje**.
5. W górnej części ekranu wybierz **projekt** kartę.
6. W **przetwarzania wychodzącego** oknie kliknij trójkąt (obok ołówka).
7. Wybierz **edytora kodu**.
    
     ![Edytuj zasady](./media/set-edit-policies/set-edit-policies01.png)
9. Umieść kursor w  **<outbound>**  elementu.
10. W prawym okienku w obszarze **zasad przekształcania**, kliknij przycisk **+ nagłówka HTTP zestaw** dwa razy (, aby wstawić dwa wstawki zasad).

    ![Zasady](./media/transform-api/transform-api.png)
11. Modyfikowanie użytkownika  **<outbound>**  kod, aby wyglądać następująco:

        <set-header name="X-Powered-By" exists-action="delete" />
        <set-header name="X-AspNet-Version" exists-action="delete" />
                
## <a name="replace-original-urls-in-the-body-of-the-api-response-with-apim-gateway-urls"></a>Zamień na adresy URL bramy APIM oryginalne adresy URL w treści odpowiedzi interfejsu API

W tej sekcji przedstawiono sposób Ukryj oryginalne adresy URL, które znajdują się w treści odpowiedzi HTTP interfejsu API i zamiast tego przekierowanie do bramy APIM.

### <a name="test-the-original-response"></a>Testowanie oryginalnego odpowiedzi

Aby wyświetlić oryginalne odpowiedzi:

1. Wybierz **interfejsu API** kartę.
2. Kliknij przycisk **API konferencji pokaz** z listy interfejsu API.
3. Wybierz **GetSpeakers** operacji.
4. Kliknij przycisk **testu** kartę w górnej części ekranu.
5. Naciśnij klawisz **wysyłania** u dołu ekranu. 

    Jak widać oryginalnego odpowiedzi wygląda następująco:

    ![Zasady](./media/transform-api/original-response2.png)

### <a name="set-the-transformation-policy"></a>Ustawienie zasad przekształcania

1. Przejdź do swojego wystąpienia APIM.
2. Wybierz **interfejsu API** kartę.
3. Kliknij przycisk **API konferencji pokaz** z listy interfejsu API.
4. Wybierz **wszystkie operacje**.
5. W górnej części ekranu wybierz **projekt** kartę.
6. W **przetwarzania wychodzącego** oknie kliknij trójkąt (obok ołówka).
7. Wybierz **edytora kodu**.
8. Umieść kursor w  **<outbound>**  elementu.
9. W prawym okienku w obszarze **zasad przekształcania**, kliknij przycisk **+ Znajdowanie i zamienianie ciągów w treści**.
10. Modyfikowanie użytkownika **< Znajdź i Zamień** kodu (w  **<outbound>**  element) zastąpić adres URL, który odpowiada APIM bramy. Na przykład:

        <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>

## <a name="protect-an-api-by-adding-rate-limit-policy-throttling"></a>Ochrona interfejsu API przez dodanie zasady limit szybkości (przepustowości)

W tej sekcji przedstawiono sposób dodawania ochrony interfejs API zaplecza przez Konfigurowanie limitów szybkości. Na przykład można ograniczyć liczbę wywołań interfejsu API jest wywoływana, więc jego jest nie nadmiernego obciążenia przez deweloperów. W tym przykładzie limit jest ustawiony na 3 wywołań na 15 sekund dla każdego identyfikatora subskrypcji. Po 15 sekund dewelopera można ponowić próbę wywołanie interfejsu API.

1. Przejdź do swojego wystąpienia APIM.
2. Wybierz **interfejsu API** kartę.
3. Kliknij przycisk **API konferencji pokaz** z listy interfejsu API.
4. Wybierz **wszystkie operacje**.
5. W górnej części ekranu wybierz **projekt** kartę.
6. W **przetwarzania przychodzącego** oknie kliknij trójkąt (obok ołówka).
7. Wybierz **edytora kodu**.
8. Umieść kursor w  **<inbound>**  elementu.
9. W prawym okienku w obszarze **zasady ograniczeń dostępu**, kliknij przycisk **+ Limit wywołania stawka za transakcje klucza**.
10. Modyfikowanie użytkownika **< szybkość limit przez klucz** kodu (w  **<inbound>**  element) poniższy kod:

        <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />

## <a name="test-the-transformations"></a>Testowanie — przekształcenia
        
W tym momencie z zasady kod wygląda następująco:

    <policies>
        <inbound>
            <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />
            <base />
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <set-header name="X-Powered-By" exists-action="delete" />
            <set-header name="X-AspNet-Version" exists-action="delete" />
            <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>

W tej sekcji reszty testy przekształcenia zasad, które można ustawić w tym artykule.

### <a name="test-the-stripped-response-headers"></a>Testowanie nagłówki odpowiedzi pozbawionego włókien

1. Przejdź do swojego wystąpienia APIM.
2. Wybierz **interfejsu API** kartę.
3. Kliknij przycisk **API konferencji pokaz** z listy interfejsu API.
4. Kliknij przycisk **GetSpeakers** operacji.
5. Wybierz **testu** kartę.
6. Naciśnij klawisz **wysyłania**.

    Jak widać, że nagłówki zostały usunięte:

    ![Zasady](./media/transform-api/final-response1.png)

### <a name="test-the-replaced-url"></a>Testowanie Zamieniono adres URL

1. Przejdź do swojego wystąpienia APIM.
2. Wybierz **interfejsu API** kartę.
3. Kliknij przycisk **API konferencji pokaz** z listy interfejsu API.
4. Kliknij przycisk **GetSpeakers** operacji.
5. Wybierz **testu** kartę.
6. Naciśnij klawisz **wysyłania**.

    Jak widać adres URL został zastąpiony.

    ![Zasady](./media/transform-api/final-response2.png)

### <a name="test-the-rate-limit-throttling"></a>Testowanie limit szybkości (przepustowości)

1. Przejdź do swojego wystąpienia APIM.
2. Wybierz **interfejsu API** kartę.
3. Kliknij przycisk **API konferencji pokaz** z listy interfejsu API.
4. Kliknij przycisk **GetSpeakers** operacji.
5. Wybierz **testu** kartę.
6. Naciśnij klawisz **wysyłania** trzy razy pod rząd.

    Po wysłaniu żądania 3 razy, możesz uzyskać **429 zbyt wiele żądań** odpowiedzi.
7. Poczekaj 15 sekund lub czynności, a następnie naciśnij klawisz **wysyłania** ponownie. Teraz należy pobrać **200 OK** odpowiedzi.

    ![Ograniczanie przepływności](./media/transform-api/test-throttling.png)

## <a name="video"></a>Połączenia wideo

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Przekształć interfejs API do paska nagłówki odpowiedzi
> * Zamień na adresy URL bramy APIM oryginalne adresy URL w treści odpowiedzi interfejsu API
> * Ochrona interfejsu API przez dodanie zasady limit szybkości (przepustowości)
> * Testowanie — przekształcenia

Przejdź do następnego samouczek:

> [!div class="nextstepaction"]
> [Monitorowanie interfejsu API](api-management-howto-use-azure-monitor.md)
