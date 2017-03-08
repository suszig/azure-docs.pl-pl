---
title: "Wdrażanie aplikacji WordPress w witrynie Azure Portal w ciągu pięciu minut | Microsoft Docs"
description: "Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając aplikację WordPress. Wyniki będą widoczne od razu."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/13/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 7ef5802866bf96859d3f44cdb58cbb412b08a700
ms.lasthandoff: 02/28/2017


---
# <a name="deploy-a-wordpress-app-in-the-azure-portal-in-five-minutes"></a>Wdrażanie aplikacji WordPress w witrynie Azure Portal w ciągu pięciu minut

Ten samouczek pokazuje, jak wdrożyć pierwszą aplikację sieci Web [WordPress](https://wordpress.org/) w usłudze [Azure App Service](../app-service/app-service-value-prop-what-is.md) w ciągu kilku minut.

![Witryna WordPress](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a>Wymagania wstępne
Potrzebujesz konta platformy Microsoft Azure. Jeśli nie masz konta, możesz [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Usługę [App Service](https://azure.microsoft.com/try/app-service/) możesz wypróbować, nie mając konta platformy Azure. Utwórz aplikację startową i testuj ją nawet przez godzinę — bez kart kredytowych i bez zobowiązań.
> 
> 

## <a name="deploy-the-wordpress-app"></a>Wdrażanie aplikacji WordPress
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Otwórz link [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).

    Ten link to skrót umożliwiający natychmiastowe skonfigurowanie nowej aplikacji WordPress w witrynie Azure Portal.

3. W polu **Nazwa aplikacji** wpisz nazwę aplikacji sieci Web. Jeśli nazwa jest unikatowa w domenie `azurewebsites.net`, w polu zostanie wyświetlony zielony znacznik wyboru.
   
5. W polu **Grupa zasobów** kliknij pozycję **Utwórz nową** w celu utworzenia nowej [grupy zasobów](../azure-resource-manager/resource-group-overview.md), a następnie nadaj jej nazwę.

6. W polu **Dostawca bazy danych** wybierz pozycję **CleaDB**.

7. Kliknij pozycję **Plan/lokalizacja usługi App Service** > **Utwórz nowy**. Skonfiguruj [plan usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) zgodnie z poniższym opisem:

    - W polu **Plan usługi App Service** wpisz odpowiednią nazwę.
    - W polu **Lokalizacja** wybierz lokalizację na potrzeby hostowania planu.
    - Kliknij pozycję **Warstwa cenowa**, wybierz warstwę **Bezpłatna F1** lub inną warstwę, która Ci odpowiada, a następnie kliknij pozycję **Wybierz**.
    - Kliknij przycisk **OK**.

8. Kliknij pozycję **Baza danych** > **Utwórz nową**. Skonfiguruj bazę danych SQL w następujący sposób:

    - W polu **Nazwa bazy danych** wpisz nazwę bazy danych. 
    - W polu **Lokalizacja** wybierz tę samą lokalizację, którą wybrano dla planu usługi App Service.
    - Kliknij pozycję **Warstwa cenowa**, wybierz warstwę **Mercury** lub inną warstwę, która Ci odpowiada, a następnie kliknij pozycję **Wybierz**.
    - Kliknij pozycję **Postanowienia prawne**, a następnie pozycję **Zakup**.
    - Kliknij przycisk **OK**.

9. Kliknij przycisk **Utwórz**.

    Platforma Azure utworzy teraz aplikację WordPress zgodnie z konfiguracją. Powinno być widoczne powiadomienie **Wdrażanie rozpoczęte...**.

    ![Wdrażanie rozpoczęte — pierwsza aplikacja WordPress w usłudze Azure App Service](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a>Uruchamianie aplikacji sieci Web WordPress i zarządzanie nią

Po zakończeniu wdrażania aplikacji przez platformę Azure zostanie wyświetlone kolejne powiadomienie.

![Wdrażanie zakończyło się pomyślnie — pierwsza aplikacja WordPress w usłudze Azure App Service](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. Kliknij powiadomienie. Jeśli powiadomienie już zniknęło, zawsze można uzyskać do niego dostęp, klikając dzwonek powiadomień (![Dzwonek powiadomień — pierwsza aplikacja WordPress w usłudze Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Powinien zostać wyświetlony [blok](../azure-resource-manager/resource-group-portal.md#manage-resources) zarządzania aplikacją sieci Web (*blok*: strona portalu otwierana poziomo).

3. W górnej części strony Przegląd kliknij przycisk **Przeglądaj**.
   
    ![Przeglądanie — pierwsza aplikacja WordPress w usłudze Azure App Service](./media/app-service-web-get-started-php-portal/browse.png)

    Zostanie wyświetlona strona **powitalna** WordPress. Skonfiguruj instalację platformy WordPress i zacznij ją poznawać!

    ![Konfiguracja platformy WordPress — pierwsza aplikacja WordPress w usłudze Azure App Service](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a>Następne kroki
* [Tworzenie, konfigurowanie i wdrażanie aplikacji sieci Web Laravel na platformie Azure](app-service-web-php-get-started.md) — zdobądź podstawowe umiejętności potrzebne do uruchamiania dowolnych aplikacji sieci Web PHP na platformie Azure, na przykład:

    * tworzenie i konfigurowanie aplikacji na platformie Azure z poziomu programu PowerShell/Bash;
    * ustawianie wersji języka PHP;
    * używanie pliku startowego, który nie znajduje się w głównym katalogu aplikacji;
    * włączanie automatyzacji narzędzia Composer;
    * uzyskiwanie dostępu do zmiennych określonych dla środowiska;
    * rozwiązywanie typowych problemów.

* [Deploy your code to Azure App Service](web-sites-deploy.md) (Wdrażanie kodu w usłudze Azure App Service) — informacje o sposobie wdrażania z serwera FTP lub repozytoriów kontroli źródła.
* [Dodawanie funkcji do pierwszej aplikacji sieci Web](app-service-web-get-started-2.md) — przenieś swoją aplikację Azure na wyższy poziom. Uwierzytelniaj użytkowników. Skaluj ją zależnie od potrzeb. Skonfiguruj alerty dotyczące wydajności. Wszystkie te czynności możesz wykonać za pomocą kilku kliknięć.

