---
title: "Tworzenie aplikacji sieci Web WordPress w usłudze Azure App Service | Microsoft Docs"
description: "Dowiedz się, jak utworzyć nową aplikację sieci Web platformy Azure dla bloga WordPress przy użyciu witryny Azure Portal."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 193ae094-0d7c-4749-a09b-ff4b1240149e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: hero-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f48aab02e0229c440848dc8e4a3d26d0d43d96aa


---
# <a name="create-a-wordpress-web-app-in-azure-app-service"></a>Tworzenie aplikacji sieci Web WordPress w usłudze Azure App Service
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Ten samouczek pokazuje, jak wdrożyć witrynę bloga WordPress z poziomu portalu Azure Marketplace.

Po użyciu samouczka będziesz mieć witrynę bloga WordPress uruchomioną w chmurze.

![Witryna WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

Dowiesz się:

* Jak znajdować szablon aplikacji w portalu Azure Marketplace.
* Jak utworzyć aplikację sieci Web w usłudze Azure App Service przy użyciu szablonu.
* Jak konfigurować ustawienia usługi Azure App Service dla nowej aplikacji sieci Web i bazy danych.

Portal Azure Marketplace udostępnia szeroką gamę popularnych aplikacji sieci Web opracowanych przez firmę Microsoft, inne firmy oraz zespoły związane z inicjatywami dotyczącymi oprogramowania typu open source. Aplikacje sieci Web są tworzone w różnych popularnych platformach, takich jak [PHP](/develop/nodejs/) w tym przykładzie WordPress, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/) i [Python](/develop/python/). Jedynym oprogramowaniem potrzebnym do utworzenia aplikacji sieci Web z poziomu portalu Azure Marketplace jest przeglądarka, za pomocą której korzystasz z witryny [Azure Portal](https://portal.azure.com/). 

Witryna WordPress wdrażana w tym samouczku korzysta z bazy danych MySQL. Jeśli chcesz zamiast tego użyć usługi SQL Database, zobacz stronę rozwiązania [Project Nami](http://projectnami.org/). Rozwiązanie **Project Nami** jest również dostępne za pośrednictwem portalu Marketplace.

> [!NOTE]
> Do wykonania kroków tego samouczka potrzebne jest konto platformy Microsoft Azure. Jeśli nie masz konta, możesz [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) lub [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Wypróbuj usługę App Service](http://go.microsoft.com/fwlink/?LinkId=523751). W tej lokalizacji możesz od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service — bez karty kredytowej i bez zobowiązań.
> 
> 

## <a name="select-wordpress-and-configure-for-azure-app-service"></a>Wybieranie platformy WordPress i konfigurowanie dla usługi Azure App Service
1. Zaloguj się do [Portalu Azure](https://portal.azure.com/).
2. Kliknij przycisk **Nowy**.
   
    ![Kliknięcie przycisku Nowe][5]
3. Wyszukaj wyrażenie **WordPress**, a następnie kliknij pozycję **WordPress**. Jeśli chcesz użyć usługi SQL Database zamiast MySQL, wyszukaj wyrażenie **Project Nami**.
   
    ![Pozycja WordPress na liście][7]
4. Po przeczytaniu opisu aplikacji WordPress kliknij przycisk **Utwórz**.
   
    ![Przycisk Utwórz](./media/web-sites-php-web-site-gallery/create.png)
5. Wprowadź nazwę aplikacji sieci Web w polu **Aplikacja sieci Web**.
   
    Ta nazwa musi być unikatowa w domenie azurewebsites.net, ponieważ adres URL aplikacji sieci Web będzie miał format {nazwa}.azurewebsites.net. Jeśli wprowadzona nazwa nie jest unikatowa, w polu tekstowym jest wyświetlany czerwony wykrzyknik.
6. Jeśli masz więcej niż jedną subskrypcję, wybierz odpowiednią subskrypcję. 
7. Wybierz **grupę zasobów** lub utwórz nową.
   
    Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
8. Wybierz pozycję **Plan usługi App Service/Lokalizacja** lub utwórz nowy plan.
   
    Aby uzyskać więcej informacji na temat planów usługi App Service, zobacz [Omówienie planów usługi Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).    
9. Kliknij pozycję **Baza danych**, a następnie w bloku **Nowa baza danych MySQL** podaj wartości wymagane do skonfigurowania bazy danych MySQL.
   
    a. Wprowadź nową nazwę lub pozostaw nazwę domyślną.
   
    b. Pozostaw opcję **Typ bazy danych** ustawioną na wartość **Udostępniona**.
   
    c. Wybierz tę samą lokalizację, którą wybrano dla aplikacji sieci Web.
   
    d. Wybierz warstwę cenową. Warstwa Mercury (bezpłatnie z minimalnymi dopuszczalnymi połączeniami i miejscem na dysku) jest odpowiednia dla tego samouczka.
10. W bloku **Nowa baza danych MySQL** kliknij przycisk **OK**. 
11. W bloku **WordPress** zaakceptuj postanowienia prawne, a następnie kliknij przycisk **Utwórz**. 
    
     ![Konfigurowanie aplikacji sieci Web](./media/web-sites-php-web-site-gallery/configure.png)
    
     Zazwyczaj tworzenie aplikacji sieci Web przez usługę Azure App Service trwa nie dłużej niż minutę. Możesz obserwować postęp, klikając ikonę dzwonka w górnej części strony portalu.
    
     ![Wskaźnik postępu](./media/web-sites-php-web-site-gallery/progress.png)

## <a name="launch-and-manage-your-wordpress-web-app"></a>Uruchamianie aplikacji sieci Web WordPress i zarządzanie nią
1. Po ukończeniu tworzenia aplikacji sieci Web przejdź w witrynie Azure Portal do grupy zasobów, w której utworzono aplikację, aby wyświetlić aplikację sieci Web i bazę danych.
   
    Dodatkowym zasobem z ikoną żarówki jest usługa [Application Insights](/services/application-insights/), która umożliwia monitorowanie aplikacji sieci Web.
2. W bloku **Grupa zasobów** kliknij wiersz aplikacji sieci Web.
   
    ![Konfigurowanie aplikacji sieci Web](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. W bloku Aplikacja sieci Web kliknij przycisk **Przeglądaj**.
   
    ![Adres URL witryny][browse]
4. Na stronie **Welcome** (Powitanie) platformy WordPress wprowadź informacje konfiguracyjne wymagane przez platformę WordPress, a następnie kliknij pozycję **Install WordPress** (Zainstaluj platformę WordPress).
   
    ![Konfigurowanie usługi WordPress](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. Zaloguj się przy użyciu poświadczeń utworzonych na stronie **Welcome** (Powitanie).  
6. Zostanie otwarta strona pulpitu nawigacyjnego witryny.    
   
    ![Witryna WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## <a name="next-steps"></a>Następne kroki
Przedstawiono sposób tworzenia i wdrożenia aplikacji sieci Web PHP z poziomu galerii. Aby uzyskać więcej informacji na temat używania struktury PHP na platformie Azure, zobacz [Centrum deweloperów języka PHP](/develop/php/).

Aby uzyskać więcej informacji na temat sposobu pracy z aplikacjami Web Apps w usłudze App Service, zobacz linki po lewej stronie (szerokie okna przeglądarki) lub w górnej części strony (wąskie okna przeglądarki). 

## <a name="whats-changed"></a>Co zostało zmienione
* Przewodnik dotyczący przejścia od usługi Witryny sieci Web do usługi App Service można znaleźć w temacie [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714) (Usługa Azure App Service i jej wpływ na istniejące usługi platformy Azure).

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png



<!--HONumber=Dec16_HO1-->


