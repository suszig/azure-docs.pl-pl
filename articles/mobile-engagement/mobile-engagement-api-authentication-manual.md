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
ms.openlocfilehash: 9d6132e1a01be489b8e8e28a0219cf8a0b50b318
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Uwierzytelniania w usłudze Mobile Engagement REST API - instalacji ręcznej
To jest dokumentacji dodatku do [Uwierzytelnij z interfejsów API REST usługi Engagement Mobile](mobile-engagement-api-authentication.md). Upewnij się, że jej najpierw uzyskać kontekst odczytu. Opisuje alternatywny sposób celu jednorazowej konfiguracji dotyczące konfigurowania uwierzytelniania dla mobilnych interfejsów API REST usługi Engagement przy użyciu portalu Azure. 

> [!NOTE]
> Poniższe instrukcje są oparte na tym [przewodnik usługi Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md) i dostosowywać do co to jest wymagany do uwierzytelniania dla mobilnych interfejsów API usługi Engagement. Aby zapoznać się go, jeśli chcesz zrozumieć poniższe kroki szczegółowo. 
> 
> 

1. Zaloguj się do konta platformy Azure za pośrednictwem [klasyczny portal](https://manage.windowsazure.com/).
2. Wybierz **usługi Active Directory** w lewym okienku.
   
     ![Wybierz usługi Active Directory][1]
3. Wybierz **domyślne usługi Active Directory** w portalu Azure. 
   
     ![Wybierz katalog][2]
   
   > [!IMPORTANT]
   > Ta metoda działa tylko wtedy, gdy pracujesz w domyślnej usługi Active Directory dla konta i nie będzie działać, jeśli robią to w usłudze Active Directory utworzonego w ramach Twojego konta. 
   > 
   > 
4. Aby wyświetlić aplikacje w katalogu, kliknij **aplikacji**.
   
     ![Wyświetl aplikacje][3]
5. Polecenie **dodać**. 
   
     ![Dodawanie aplikacji][4]
6. Polecenie **Dodaj aplikację moją organizację**
   
     ![Nowa aplikacja][5]
7. Wprowadź nazwę aplikacji i wybierz typ aplikacji jako **interfejsu API sieci WEB i/lub aplikacji sieci WEB** i kliknij przycisk Dalej.
   
     ![Nazwa aplikacji][6]
8. Możesz podać wszelkie fikcyjny adresy URL dla **adres URL logowania** i **identyfikator URI aplikacji**. Nie są one używane do naszego scenariusza i same adresy URL nie są weryfikowane.  
   
     ![Właściwości aplikacji][7]
9. Na końcu tego będziesz mieć aplikację AAD o nazwie podane wcześniej podobnie do poniższego. Jest to Twoje **AD\_aplikacji\_nazwa** i zapisz go.  
   
     ![Nazwa aplikacji][8]
10. Kliknij nazwę aplikacji i wybierz polecenie **Konfiguruj**.
    
      ![Konfigurowanie aplikacji][9]
11. Zanotuj identyfikator klienta, która będzie służyć jako **klienta\_identyfikator** do interfejsu API wywołania. 
    
     ![Konfigurowanie aplikacji][10]
12. Przewiń w dół do **klucze** i Dodaj klucz o najlepiej czas trwania 2 lata (wygaśnięcia) i kliknij pozycję **zapisać**. 
    
     ![Konfigurowanie aplikacji][11]
13. Natychmiast skopiować wartość, która jest wyświetlana dla klucza jest wyświetlana tylko teraz, a nie są przechowywane, więc nie zostanie nigdy ponownie wyświetlone. W przypadku utraty następnie konieczne będzie wygenerowanie nowego klucza. Są to **CLIENT_SECRET** do interfejsu API wywołania. 
    
     ![Konfigurowanie aplikacji][12]
    
    > [!IMPORTANT]
    > Ten klucz wygaśnie z końcem okresu czasu trwania, które określone, upewnij się, że Odnów go w czasie w przeciwnym razie uwierzytelnianie interfejsu API nie będzie już działać. Można również usunąć i ponownie utwórz ten klucz, jeśli uważasz, że został złamany.
    > 
    > 
14. Polecenie **WYŚWIETL punkty końcowe** przycisk teraz który spowoduje to otwarcie **punkty końcowe aplikacji** okno dialogowe. 
    
    ![][13]
15. W oknie dialogowym punkty końcowe aplikacji skopiuj **KOŃCOWYM TOKENÓW OAUTH 2.0**. 
    
    ![][14]
16. Ten punkt końcowy będzie w następującym formacie, gdzie jest identyfikator GUID w adresie URL Twojego **TENANT_ID** tak zanotuj go: 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. Teraz możemy będzie kontynuowana, aby skonfigurować uprawnienia w tej aplikacji. W tym konieczne będzie otwarcie [portalu Azure](https://portal.azure.com). 
18. Polecenie **grup zasobów** i Znajdź **Mobile Engagement** grupy zasobów.  
    
    ![][15]
19. Kliknij przycisk **Mobile Engagement** zasobów grupy i przejdź do **ustawienia** bloku, w tym miejscu. 
    
    ![][16]
20. Polecenie **użytkowników** w bloku ustawienia i kliknij **Dodaj** Aby dodać użytkownika. 
    
    ![][17]
21. Polecenie **wybierz rolę**
    
    ![][18]
22. Polecenie **właściciela**
    
    ![][19]
23. Wyszukaj nazwę aplikacji **AD\_aplikacji\_nazwa** w polu wyszukiwania. Domyślnie w tym miejscu nie będą widzieć to. Po znalezieniu go, zaznacz go i kliknij na **wybierz** w dolnej części bloku. 
    
    ![][20]
24. Na **dostępu Dodaj** bloku, zostanie wyświetlona jako **użytkownika 1, 0 grupy**. Kliknij przycisk **OK** w tym bloku, aby potwierdzić zmianę. 
    
    ![][21]

Ukończono wymaganej konfiguracji usługi AAD i możesz są gotowe do wywoływania interfejsów API. 

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
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
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



