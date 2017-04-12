---
title: "Wdrażanie aplikacji sieci Web Umbraco w witrynie Azure Portal w ciągu pięciu minut | Microsoft Docs"
description: "Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając przykładową aplikację platformy ASP.NET. Wyniki będą widoczne od razu."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/10/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 6b1dede903083d1771733330a069b6ab533d9f00
ms.lasthandoff: 04/06/2017


---
# <a name="deploy-an-umbraco-web-app-in-the-azure-portal-in-five-minutes"></a>Wdrażanie aplikacji sieci Web Umbraco w witrynie Azure Portal w ciągu pięciu minut

Ten samouczek ułatwia wdrożenie aplikacji sieci Web [Umbraco](https://our.umbraco.org/) w usłudze [Azure App Service](../app-service/app-service-value-prop-what-is.md) w ciągu kilku minut.

![Aplikacja Umbraco](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a>Wymagania wstępne
Potrzebujesz konta platformy Microsoft Azure. Jeśli nie masz konta, możesz [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Usługę [App Service](https://azure.microsoft.com/try/app-service/) możesz wypróbować, nie mając konta platformy Azure. Utwórz aplikację startową i testuj ją nawet przez godzinę — bez kart kredytowych i bez zobowiązań.
> 
> 

## <a name="deploy-the-aspnet-app"></a>Wdrażanie aplikacji ASP.NET
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Otwórz link [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).

    Ten link to skrót umożliwiający natychmiastowe skonfigurowanie nowej aplikacji Umbraco w witrynie Azure Portal.

3. W polu **Nazwa aplikacji** wpisz nazwę aplikacji sieci Web. Jeśli nazwa jest unikatowa w domenie `azurewebsites.net`, w polu zostanie wyświetlony zielony znacznik wyboru.
   
5. W polu **Grupa zasobów** kliknij pozycję **Utwórz nową** w celu utworzenia nowej [grupy zasobów](../azure-resource-manager/resource-group-overview.md), a następnie nadaj jej nazwę.

7. Kliknij pozycję **Plan/lokalizacja usługi App Service** > **Utwórz nowy**. Skonfiguruj [plan usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) zgodnie z poniższym opisem:

    - W polu **Plan usługi App Service** wpisz odpowiednią nazwę.
    - W polu **Lokalizacja** wybierz lokalizację na potrzeby hostowania planu.
    - Kliknij pozycję **Warstwa cenowa**, wybierz warstwę **Bezpłatna F1** lub inną warstwę, która Ci odpowiada, a następnie kliknij pozycję **Wybierz**.
    - Kliknij przycisk **OK**.

    Konfiguracja aplikacji Umbraco CMS powinna teraz wyglądać podobnie jak na poniższym zrzucie ekranu:

    ![Konfiguracja w toku — pierwsza aplikacja Umbraco w usłudze Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. Kliknij opcję **Baza danych SQL** > **Utwórz nową bazę danych**. Skonfiguruj bazę danych SQL w następujący sposób:

    - W polu **Nazwa** wpisz nazwę, np. **mojaBazaDanych**.
    - Kliknij pozycję **Warstwa cenowa**, wybierz warstwę **Bezpłatna F** lub inną warstwę, która Ci odpowiada, a następnie kliknij pozycję **Wybierz**.
    - Kliknij pozycję **Serwer docelowy** > **Utwórz nowy serwer**. Skonfiguruj serwer bazy danych tak, jak pokazano:

        - W polu **Nazwa serwera** wpisz nazwę serwera. Jeśli nazwa jest unikatowa w domenie `.database.windows.net`, w polu zostanie wyświetlony zielony znacznik wyboru.
        - W polu **Identyfikator logowania administratora serwera** wpisz żądaną nazwę użytkownika administratora.
        - W polach **Hasło** i **Potwierdź hasło** wpisz odpowiednie hasło.
        - W polu Lokalizacja wybierz tę samą lokalizację, która jest używana dla aplikacji sieci Web.
        - Upewnij się, że opcja **Zezwalaj usługom platformy Azure na dostęp do serwera** jest wybrana.
        - Kliknij pozycję **Wybierz**.
    
    - Kliknij pozycję **Wybierz**.

13. Kliknij pozycję **Ustawienia aplikacji sieci Web**, określ nazwę użytkownika i hasło bazy danych, a następnie kliknij przycisk **OK**.

    Konfiguracja aplikacji Umbraco CMS powinna teraz wyglądać podobnie jak na poniższym zrzucie ekranu:

    ![Konfiguracja ukończona — pierwsza aplikacja Umbraco w usłudze Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. Kliknij przycisk **Utwórz**.
    
    Platforma Azure utworzy teraz aplikację Umbraco zgodnie z konfiguracją. Powinno być widoczne powiadomienie **Wdrażanie rozpoczęte...**.

    ![Wdrażanie zakończyło się pomyślnie — pierwsza aplikacja Umbraco w usłudze Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a>Uruchamianie aplikacji sieci Web Umbraco i zarządzanie nią

Po zakończeniu wdrażania aplikacji przez platformę Azure zostanie wyświetlone kolejne powiadomienie.

![Wdrażanie zakończyło się pomyślnie — pierwsza aplikacja Umbraco w usłudze Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. Kliknij powiadomienie. Jeśli powiadomienie już zniknęło, zawsze można uzyskać do niego dostęp, klikając dzwonek powiadomień (![Dzwonek powiadomień — pierwsza aplikacja Umbraco w usłudze Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Powinien zostać wyświetlony [blok](../azure-resource-manager/resource-group-portal.md#manage-resources) zarządzania aplikacją sieci Web (*blok*: strona portalu otwierana poziomo).

3. W górnej części strony Przegląd kliknij przycisk **Przeglądaj**.
   
    ![Przeglądanie — pierwsza aplikacja Umbraco w usłudze Azure App Service](./media/app-service-web-get-started-dotnet-portal/browse.png)

    Zostanie wyświetlona strona **powitalna** Umbraco. Skonfiguruj instalację aplikacji Umbraco i zacznij ją poznawać!

    ![Konfiguracja aplikacji Umbraco — pierwsza aplikacja Umbraco w usłudze Azure App Service](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a>Następne kroki
* [Wdrażanie aplikacji sieci Web programu ASP.NET w usłudze Azure App Service przy użyciu programu Visual Studio](app-service-web-get-started-dotnet.md) — dowiedz się, jak utworzyć nową aplikację sieci Web platformy Azure w programie Visual Studio przy użyciu jednego z dołączonych szablonów aplikacji.
* [Deploy your code to Azure App Service](web-sites-deploy.md) (Wdrażanie kodu w usłudze Azure App Service) — informacje o sposobie wdrażania z serwera FTP lub repozytoriów kontroli źródła.
* [Dodawanie funkcji do pierwszej aplikacji sieci Web](app-service-web-get-started-2.md) — przenieś swoją aplikację Azure na wyższy poziom. Uwierzytelniaj użytkowników. Skaluj ją zależnie od potrzeb. Skonfiguruj alerty dotyczące wydajności. Wszystkie te czynności możesz wykonać za pomocą kilku kliknięć.

