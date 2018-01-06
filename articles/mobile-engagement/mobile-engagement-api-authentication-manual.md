---
title: "Uwierzytelniania w usłudze Mobile Engagement REST API - instalacji ręcznej"
description: "Opisuje, jak ręcznie skonfigurować uwierzytelnianie dla interfejsów API REST usługi Engagement Mobile"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3b678acbae225c28223a2ee76e5be2462a529362
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Uwierzytelniania w usłudze Mobile Engagement REST API - instalacji ręcznej
Niniejsza dokumentacja jest dokumentacją dodatku, aby [Uwierzytelnij z interfejsów API REST usługi Engagement Mobile](mobile-engagement-api-authentication.md). Upewnij się, że jej najpierw uzyskać kontekst odczytu.
Opisuje alternatywny sposób celu jednorazowej konfiguracji dotyczące konfigurowania uwierzytelniania dla mobilnych interfejsów API REST usługi Engagement przy użyciu portalu Azure.

> [!NOTE]
> Poniższe instrukcje są oparte na tym [przewodnik usługi Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md) i dostosowywać do co to jest wymagany do uwierzytelniania dla mobilnych interfejsów API usługi Engagement. Aby zapoznać się go, jeśli chcesz zrozumieć poniższe kroki szczegółowo.

1. Zaloguj się do konta platformy Azure za pośrednictwem [portalu Azure](https://portal.azure.com/).
2. Wybierz **usługi Active Directory** w lewym okienku.

     ![Wybierz usługi Active Directory][1]

3. Aby wyświetlić aplikacje w katalogu, kliknij **rejestracji aplikacji**.

     ![Wyświetl aplikacje][3]

4. Polecenie **nowej rejestracji aplikacji**.

     ![Dodawanie aplikacji][4]

5. Wprowadź nazwę aplikacji i pozostawić typu aplikacji jako **aplikacji/interfejs API sieci Web** i kliknij przycisk Dalej. Możesz podać wszelkie fikcyjny adresy URL dla **adres URL logowania**: nie są używane w tym scenariuszu i same adresy URL nie są weryfikowane.
6. Po zakończeniu masz aplikacji usługi Azure AD o nazwie podane. Jest Twoje **AD\_aplikacji\_nazwa**, należy zwrócić uwagę.

     ![nazwa aplikacji][8]

7. Kliknij nazwę aplikacji.
8. Znajdź **identyfikator aplikacji**, zapisz go, jest identyfikator klienta, który ma być używany jako **klienta\_identyfikator** do interfejsu API wywołania.

     ![Konfigurowanie aplikacji][10]

9. Znajdź **klucze** sekcji po prawej stronie.

     ![Konfigurowanie aplikacji][11]

10. Utwórz nowy klucz i natychmiast, skopiuj go i zapisać go do użycia. Nigdy nie znajdzie się ona ponownie.

     ![Konfigurowanie aplikacji][12]

    > [!IMPORTANT]
    > Ten klucz wygaśnie z końcem okresu czasu trwania, które określone, upewnij się, że Odnów go w czasie w przeciwnym razie uwierzytelnianie interfejsu API nie będzie już działać. Można również usunąć i ponownie utwórz ten klucz, jeśli uważasz, że został złamany.
    >
    >
11. Polecenie **punkty końcowe** przycisk u góry strony i skopiuj **KOŃCOWYM TOKENÓW OAUTH 2.0**.

    ![][14]

16. Ten punkt końcowy będzie w następującym formacie, gdzie jest identyfikator GUID w adresie URL Twojego **TENANT_ID** tak zanotuj go:`https://login.microsoftonline.com/<GUID>/oauth2/token`
17. Teraz możemy będzie kontynuowana, aby skonfigurować uprawnienia w tej aplikacji. W tym konieczne będzie otwarcie [portalu Azure](https://portal.azure.com). 
18. Polecenie **grup zasobów** i Znajdź **Mobile Engagement** grupy zasobów.

    ![][15]

19. Kliknij przycisk **Mobile Engagement** zasobów grupy i przejdź do **ustawienia** sekcji tutaj.

    ![][16]

20. Polecenie **użytkowników** w ustawieniach sekcji, a następnie kliknij polecenie **Dodaj** Aby dodać użytkownika.

    ![][17]

21. Polecenie **wybierz rolę**.

    ![][18]

22. Polecenie **właściciela**.

    ![][19]

23. Wyszukaj nazwę aplikacji **AD\_aplikacji\_nazwa** w polu wyszukiwania. Domyślnie w tym miejscu nie będą widzieć to. Po znalezieniu go, zaznacz go i kliknij na **wybierz** w dolnej części sekcji.

    ![][20]

24. Na **dostępu Dodaj** sekcji, zostanie wyświetlona jako **użytkownika 1, 0 grupy**. Kliknij przycisk **OK** w tej sekcji, aby potwierdzić zmianę.

    ![][21]

Ukończono wymaganych konfiguracji usługi Azure AD i są wszystko gotowe do wywoływania interfejsów API.

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client-secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



