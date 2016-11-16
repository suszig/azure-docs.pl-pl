---
title: Tworzenie aplikacji sieci Web z poziomu portalu Azure Marketplace | Microsoft Docs
description: "Dowiedz się, jak utworzyć nową aplikację sieci Web WordPress z poziomu portalu Azure Marketplace przy użyciu Portalu Azure."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: 972a296d-f927-470b-8534-0f2cb9eac223
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/20/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 73fafeff227061feae1fbc0a1c50e5056116e3ea


---
# <a name="create-a-web-app-from-the-azure-marketplace"></a>Tworzenie aplikacji sieci Web z poziomu portalu Azure Marketplace
<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Portal Azure Marketplace udostępnia szeroką gamę popularnych aplikacji sieci Web opracowanych przez firmę Microsoft, inne firmy oraz zespoły związane z inicjatywami dotyczącymi oprogramowania typu open source. Na przykład WordPress, Umbraco CMS, Drupal itd. Aplikacje sieci Web są tworzone przy użyciu różnych popularnych środowisk, takich jak [PHP] w tym przykładzie WordPress, [.NET], [Node.js], [Java] i [Python]. Jedynym oprogramowaniem potrzebnym do utworzenia aplikacji sieci Web z poziomu portalu Azure Marketplace jest przeglądarka, za pomocą której korzystasz z [Azure Portal].

Niniejszy samouczek zawiera informacje na temat wykonywania następujących czynności:

* Znajdowanie i tworzenie aplikacji sieci Web w usłudze Azure App Service przy użyciu szablonu portalu Azure Marketplace.
* Konfigurowanie ustawień usługi Azure App Service dla nowej aplikacji sieci Web.
* Uruchamianie aplikacji sieci Web i zarządzanie nią.

W tym samouczku przedstawiono, jak wdrożyć witrynę blogu WordPress z poziomu portalu Azure Marketplace. Po wykonaniu czynności opisanych w tym samouczku będziesz mieć witrynę WordPress działającą w chmurze.

![Przykład pulpitu nawigacyjnego aplikacji sieci Web WordPress][WordPressDashboard1]

Witryna WordPress, wdrażana w tym samouczku, korzysta z bazy danych MySQL. Jeśli zamiast tego chcesz użyć usługi SQL Database, zobacz program [Project Nami], który jest również dostępny za pośrednictwem portalu Azure Marketplace.

> [!NOTE]
> Do wykonania kroków tego samouczka potrzebne jest konto platformy Microsoft Azure. Jeśli nie masz konta, możesz [aktywować korzyści dla subskrybentów programu Visual Studio][aktywuj] lub [utworzyć konto bezpłatnej wersji próbnej][bezpłatna wersja próbna].
> 
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do strony [Wypróbuj usługę App Service]. W tym miejscu możesz od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service — bez karty kredytowej i bez zobowiązań.
> 
> 

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Znajdowanie i tworzenie aplikacji sieci Web w usłudze Azure App Service
1. Zaloguj się do [Azure Portal].
2. Kliknij przycisk **Nowy**.
   
    ![Tworzenie nowego zasobu platformy Azure][MarketplaceStart]
3. Wyszukaj wyrażenie **WordPress**, a następnie kliknij pozycję **WordPress**. Jeśli chcesz użyć usługi SQL Database zamiast MySQL, wyszukaj ciąg **Project Nami**.
   
    ![Wyszukiwanie platformy WordPress w portalu Marketplace][MarketplaceSearch]
4. Po przeczytaniu opisu aplikacji WordPress kliknij przycisk **Utwórz**.
   
    ![Tworzenie aplikacji sieci Web WordPress][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Konfigurowanie ustawień usługi Azure App Service dla nowej aplikacji sieci Web
1. Po utworzeniu nowej aplikacji sieci Web zostanie wyświetlony blok ustawień platformy WordPress, w którym należy wykonać następujące kroki:
   
    ![Konfigurowanie ustawień aplikacji sieci Web WordPress][ConfigStart]
2. Wprowadź nazwę aplikacji sieci Web w polu **Aplikacja sieci Web**.
   
    Ta nazwa musi być unikatowa w domenie azurewebsites.net, ponieważ adres URL aplikacji sieci Web będzie miał format *{nazwa}*.azurewebsites.net. Jeśli wprowadzona nazwa nie jest unikatowa, w polu tekstowym jest wyświetlany czerwony wykrzyknik.
   
    ![Konfigurowanie nazwy aplikacji sieci Web WordPress][ConfigAppName]
3. Jeśli masz więcej niż jedną subskrypcję, wybierz odpowiednią subskrypcję.
   
    ![Konfigurowanie subskrypcji dla aplikacji sieci Web][ConfigSubscription]
4. Wybierz **grupę zasobów** lub utwórz nową.
   
    Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Azure Resource Manager][ResourceGroups].
   
    ![Konfigurowanie grupy zasobów dla aplikacji sieci Web][ConfigResourceGroup]
5. Wybierz pozycję **Plan usługi App Service/Lokalizacja** lub utwórz nowy plan.
   
    Aby uzyskać więcej informacji na temat planów usługi App Service, zobacz [Omówienie planów usługi Azure App Service][AzureAppServicePlans].
   
    ![Konfigurowanie planu usługi dla aplikacji sieci Web][ConfigServicePlan]
6. Kliknij pozycję **Baza danych**, a następnie w bloku **Nowa baza danych MySQL** podaj wartości wymagane do skonfigurowania bazy danych MySQL.
   
    a. Wprowadź nową nazwę lub pozostaw nazwę domyślną.
   
    b. Pozostaw opcję **Typ bazy danych** ustawioną na wartość **Udostępniona**.
   
    c. Wybierz tę samą lokalizację, którą wybrano dla aplikacji sieci Web.
   
    d. Wybierz warstwę cenową. Warstwa **Mercury** (bezpłatnie z minimalną liczbą połączeń i ilością miejsca na dysku) jest wystarczająca dla tego samouczka.
   
    e. W bloku **Nowa baza danych MySQL** zaakceptuj postanowienia prawne, a następnie kliknij przycisk **OK**.
   
    ![Konfigurowanie ustawień bazy danych dla aplikacji sieci Web][ConfigDatabase]
7. W bloku **WordPress** zaakceptuj postanowienia prawne, a następnie kliknij przycisk **Utwórz**.
   
    ![Kończenie wprowadzania ustawień aplikacji sieci Web i klikanie przycisku OK][ConfigFinished]
   
    Zazwyczaj tworzenie aplikacji sieci Web przez usługę Azure App Service trwa nie dłużej niż minutę. Możesz obserwować postęp, klikając ikonę dzwonka w górnej części strony portalu.
   
    ![Wskaźnik postępu][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Uruchamianie aplikacji sieci Web WordPress i zarządzanie nią
1. Po ukończeniu tworzenia aplikacji sieci Web przejdź w witrynie Azure Portal do grupy zasobów, w której utworzono aplikację, aby wyświetlić aplikację sieci Web i bazę danych.
   
    Dodatkowym zasobem z ikoną żarówki jest usługa [Application Insights][ApplicationInsights], która umożliwia monitorowanie aplikacji sieci Web.
2. W bloku **Grupa zasobów** kliknij wiersz aplikacji sieci Web.
   
    ![Wybieranie aplikacji sieci Web WordPress][WordPressSelect]
3. W bloku Aplikacja sieci Web kliknij przycisk **Przeglądaj**.
   
    ![Przeglądanie aplikacji sieci Web WordPress][WordPressBrowse]
4. Jeśli zostanie wyświetlony monit o wybranie języka bloga WordPress, wybierz odpowiedni język, a następnie kliknij przycisk **Kontynuuj**.
   
    ![Konfigurowanie języka aplikacji sieci Web WordPress][WordPressLanguage]
5. Na stronie **Welcome** (Powitanie) platformy WordPress wprowadź informacje konfiguracyjne wymagane przez platformę WordPress, a następnie kliknij pozycję **Install WordPress** (Zainstaluj platformę WordPress).
   
    ![Konfigurowanie ustawień aplikacji sieci Web WordPress][WordPressConfigure]
6. Zaloguj się przy użyciu poświadczeń utworzonych na stronie **Welcome** (Powitanie).  
7. Zostanie otwarta strona pulpitu nawigacyjnego witryny, na której będą wyświetlone podane informacje.    
   
    ![Wyświetlanie pulpitu nawigacyjnego WordPress][WordPressDashboard2]

## <a name="next-steps"></a>Następne kroki
W tym samouczku przedstawiono informacje dotyczące sposobu tworzenia i wdrażania przykładowej aplikacji sieci Web z poziomu portalu Azure Marketplace.

Aby uzyskać więcej informacji na temat sposobu pracy z aplikacjami Web Apps w usłudze App Service, zobacz linki po lewej stronie (szerokie okna przeglądarki) lub w górnej części strony (wąskie okna przeglądarki).

Aby uzyskać więcej informacji na temat tworzenia aplikacji sieci Web WordPress na platformie Azure, zobacz [Developing WordPress on Azure App Service][WordPressOnAzure] (Tworzenie aplikacji WordPress w usłudze Azure App Service).

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[aktywuj]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[bezpłatna wersja próbna]: https://azure.microsoft.com/pricing/free-trial/
[Wypróbuj usługę App Service]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../azure-resource-manager/resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure Portal]: https://portal.azure.com/
[Project Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png



<!--HONumber=Nov16_HO2-->


