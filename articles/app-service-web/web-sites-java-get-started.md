---
title: "Tworzenie aplikacji sieci Web Java w usłudze Azure App Service | Microsoft Docs"
description: "Ten samouczek pokazuje, jak wdrożyć aplikację sieci Web Java w usłudze Azure App Service."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d6e73cc3-8b71-4742-a197-3edeabc6a289
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: get-started-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 3451e6d13119bacc66e9ccd861862edea5a5b4fe


---
# <a name="create-a-java-web-app-in-azure-app-service"></a>Tworzenie aplikacji sieci Web Java w usłudze Azure App Service
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Ten samouczek pokazuje, jak utworzyć [Aplikacja sieci Web w usłudze Azure App Service] przy użyciu witryny [Azure Portal]. Portal Azure jest interfejsem sieci Web, którego można użyć do zarządzania zasobami platformy Azure.

> [!NOTE]
> Do wykonania kroków tego samouczka potrzebne jest konto platformy Microsoft Azure. Jeśli nie masz konta, możesz [aktywować korzyści dla subskrybentów programu Visual Studio] lub [utworzyć konto bezpłatnej wersji próbnej].
> 
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Wypróbuj usługę App Service]. W tej lokalizacji możesz od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service; bez karty kredytowej i bez zobowiązań.
> 
> 

## <a name="java-application-options"></a>Opcje aplikacji Java
Istnieje kilka metod konfigurowania aplikacji Java w aplikacji sieci Web usługa App Service. 

1. Utwórz aplikację, a następnie skonfiguruj **Ustawienia aplikacji**.
   
    Usługa App Service zapewnia kilka wersji kontenerów Tomcat i Jetty o domyślnych konfiguracjach. Jeśli aplikacja, którą będziesz hostować, będzie działać z jedną z wbudowanych wersji, ta metoda konfigurowania kontenera sieci Web jest najprostsza. Jest to doskonałe rozwiązanie, jeśli wszystko, co chcesz zrobić, to przesłać plik war do kontenera sieci Web. W przypadku tej metody utwórz aplikację w witrynie Azure Portal, a następnie przejdź do bloku **Ustawienia aplikacji** dla swojej aplikacji, aby wybrać wersję środowiska Java oraz żądany kontener sieci Web Java. Przy użyciu tej metody zarówno program Java, jak i kontener sieci Web są uruchamiane z poziomu plików programu. W przypadku innych metod kontener sieci Web i potencjalnie maszyna JVM są umieszczane w Twojej przestrzeni dyskowej. Jeśli używasz tego modelu, nie masz dostępu do edycji plików w tej części systemu plików. Oznacza to, że nie możesz wykonywać różnych czynności, np. konfigurować pliku *server.xml* ani umieszczać plików biblioteki w folderze */lib*. Aby uzyskać więcej informacji, zobacz sekcję [Tworzenie i konfigurowanie aplikacji sieci Web Java](#portal) poniżej w tym samouczku.
2. Użyj szablonu z portalu Azure Marketplace.
   
    Portal Azure Marketplace zawiera szablony, które automatycznie tworzą i konfigurują aplikacje sieci Web Java za pomocą kontenerów sieci Web Tomcat lub Jetty. Kontenery sieci Web tworzone przez szablony są konfigurowalne. Aby uzyskać więcej informacji, zobacz sekcję [Używanie szablonu Java z portalu Azure Marketplace](#marketplace) tego samouczka.
3. Utwórz aplikację, a następnie ręcznie skopiuj i edytuj pliki konfiguracji. 
   
    Możesz hostować niestandardową aplikację Java, która nie wdraża żadnego kontenera sieci Web dostarczanego przez usługę App Service. Na przykład:
   
   * Aplikacja Java wymaga wersji kontenera Tomcat lub Jetty, która nie jest bezpośrednio obsługiwana przez usługę App Service lub dostarczana poprzez galerię.
   * Aplikacja Java przyjmuje żądania HTTP i nie jest wdrażana jako plik WAR we wstępnie istniejącym kontenerze sieci Web.
   * Chcesz samodzielnie skonfigurować kontener sieci Web od podstaw. 
   * Chcesz użyć wersji programu Java, która nie jest obsługiwana w usłudze App Service, i chcesz przekazać ją samodzielnie.
     
     W takich przypadkach możesz utworzyć aplikację przy użyciu Portalu Azure, a następnie ręcznie zapewnić odpowiednie pliki wykonywalne. W takim przypadku pliki będą przeliczane względem przydziałów miejsca w magazynie dla Twojego planu usługi App Service. Aby uzyskać więcej informacji, zobacz [Przekazywanie niestandardowej aplikacji sieci Web Java na platformę Azure] .

## <a name="a-nameportala-create-and-configure-a-java-web-app"></a><a name="portal"></a> Tworzenie i konfigurowanie aplikacji sieci Web Java
W tej sekcji przedstawiono sposób tworzenia aplikacji sieci Web i konfigurowania jej pod kątem języka Java przy użyciu bloku **Ustawienia aplikacji** w portalu.

1. Zaloguj się do [Azure Portal].
2. Kliknij kolejno pozycje **Nowe > Sieci Web i mobilność > Aplikacja sieci Web**.
   
    ![Nowa aplikacja sieci Web][newwebapp]
3. Wprowadź nazwę aplikacji sieci Web w polu **Aplikacja sieci Web**.
   
    Ta nazwa musi być unikatowa w domenie azurewebsites.net, ponieważ adres URL aplikacji sieci Web będzie miał format {nazwa}.azurewebsites.net. Jeśli wprowadzona nazwa nie jest unikatowa, w polu tekstowym jest wyświetlany czerwony wykrzyknik.
4. Wybierz **grupę zasobów** lub utwórz nową.
   
    Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Resource Manager].
5. Wybierz pozycję **Plan usługi App Service/Lokalizacja** lub utwórz nowy plan.
   
    Aby uzyskać więcej informacji na temat planów usługi App Service, zobacz [Omówienie planów usługi Azure App Service].
6. Kliknij przycisk **Utwórz**.
   
    ![Tworzenie aplikacji sieci Web][newwebapp2]
7. Po utworzeniu aplikacji sieci Web kliknij kolejno pozycje **Aplikacje Web Apps > {Twoja aplikacja sieci Web}**.
   
    ![Wybieranie aplikacji sieci Web][selectwebapp]
8. W bloku **Aplikacja sieci Web** kliknij pozycję **Ustawienia**.
9. Kliknij pozycję **Ustawienia aplikacji**.
10. Wybierz żądaną **wersję Java**. 
11. Wybierz żądaną **wersję pomocniczą Java**. W przypadku wybrania opcji **Najnowsza** Twoja aplikacja będzie używać najnowszej wersji pomocniczej dostępnej w usłudze App Service dla danej wersji głównej Java. Element **Najnowsza** jest unikatowy dla aplikacji Java tworzonych z poziomu opcji **Ustawienia aplikacji**. Jeśli tworzysz aplikację Java z poziomu galerii, musisz zarządzać własnym kontenerem i zmianami maszyny JVM. 
12. Wybierz żądany **kontener sieci Web**. Jeśli wybierzesz kontener, którego nazwa rozpoczyna się od słowa **Newest** (Najnowsza), Twoja aplikacja będzie utrzymywana w najnowszej głównej wersji tego kontenera sieci Web, która jest dostępna w usłudze App Service. 
    
    ![Wersje kontenera sieci Web][versions]
13. Kliknij pozycję **Zapisz**.
    
    W ciągu kilku chwil aplikacja sieci Web zostanie oparta na języku Java i zostanie skonfigurowana do używania wybranego kontenera sieci Web.
14. Kliknij pozycję **Aplikacje sieci Web > {nazwa nowej aplikacji sieci Web}**.
15. Kliknij pozycję **Adres URL**, aby przejść do nowej lokacji.
    
    Ta strona sieci Web potwierdza, że utworzono aplikację sieci Web opartą na języku Java.

## <a name="a-namemarketplacea-use-a-java-template-from-the-azure-marketplace"></a><a name="marketplace"></a> Używanie szablonu Java z portalu Azure Marketplace
W tej sekcji przedstawiono sposób użycia portalu Azure Marketplace do utworzenia aplikacji sieci Web Java. Tego samego ogólnego przepływu pracy można użyć do utworzenia aplikacji mobilnej opartej na języku Java lub aplikacji interfejsu API. 

1. Zaloguj się w witrynie [Azure Portal]
2. Kliknij przycisk **Nowe > Marketplace**.
   
    ![Nowy element Marketplace][newmarketplace]
3. Kliknij pozycję **Sieci Web i mobilność**.
   
    Możliwe, że zajdzie konieczność przewinięcia w lewo do bloku **Marketplace**, w którym można wybrać pozycję **Sieci Web i mobilność**.
4. W polu wyszukiwania wprowadź nazwę serwera aplikacji Java, np. **Apache Tomcat** lub **Jetty**, a następnie naciśnij klawisz Enter.
5. W wynikach wyszukiwania kliknij pozycję serwera aplikacji Java.
   
    ![Mobilny serwer Jetty sieci Web][webmobilejetty]
6. W pierwszym bloku **Apache Tomcat** lub **Jetty** kliknij pozycję **Utwórz**.
   
    ![Blok portalu serwera Jetty][jettyblade]
7. W następnym bloku **Apache Tomcat** lub **Jetty** wprowadź nazwę aplikacji sieci Web w polu **Aplikacja sieci Web**.
   
    Ta nazwa musi być unikatowa w domenie azurewebsites.net, ponieważ adres URL aplikacji sieci Web będzie miał format {nazwa}.azurewebsites.net. Jeśli wprowadzona nazwa nie jest unikatowa, w polu tekstowym jest wyświetlany czerwony wykrzyknik.
8. Wybierz **grupę zasobów** lub utwórz nową.
   
    Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Resource Manager].
9. Wybierz pozycję **Plan usługi App Service/Lokalizacja** lub utwórz nowy plan.
   
    Aby uzyskać więcej informacji na temat planów usługi App Service, zobacz [Omówienie planów usługi Azure App Service].
10. Kliknij przycisk **Utwórz**.
    
    ![Tworzenie serwera Jetty przy użyciu portalu][jettyportalcreate2]
    
    Platforma Azure szybko (zazwyczaj trwa to nie dłużej niż minutę) kończy tworzenie nowej aplikacji sieci Web.
11. Kliknij pozycję **Aplikacje sieci Web > {nazwa nowej aplikacji sieci Web}**.
12. Kliknij pozycję **Adres URL**, aby przejść do nowej lokacji.
    
    ![Adres URL serwera Jetty][jettyurl]
    
    Serwer Tomcat jest dostarczany z domyślnym zestawem stron. Jeśli wybierzesz serwer Tomcat, zobaczysz stronę podobną do poniższego przykładu.
    
    ![Aplikacja sieci Web z serwerem Apache Tomcat][tomcat]
    
    Jeśli wybierzesz serwer Jetty, zobaczysz stronę podobną do poniższego przykładu. Serwer Jetty nie zawiera domyślnego zestawu stron, dlatego używa się tutaj tej samej metodologii JSP co w przypadku pustej witryny Java.
    
    ![Aplikacja sieci Web z serwerem Jetty][jetty]

Teraz, po utworzeniu aplikacji sieci Web z kontenerem aplikacji, zobacz sekcję [Następne kroki](#next-steps), aby uzyskać informacje o sposobach przekazywania aplikacji do aplikacji sieci Web.

## <a name="next-steps"></a>Następne kroki
Na tym etapie masz serwer aplikacji Java działający w aplikacji sieci Web w usłudze Azure App Service. Aby wdrożyć własny kod do aplikacji sieci Web, zobacz temat [Dodawanie aplikacji lub strony sieci Web do aplikacji sieci Web Java].

Aby uzyskać więcej informacji o tworzeniu aplikacji Java na platformie Azure, zobacz [Centrum deweloperów języka Java].

<!-- URL List -->

[Dodawanie aplikacji lub strony sieci Web do aplikacji sieci Web Java]: ./web-sites-java-add-app.md
[Omówienie planów usługi Azure App Service]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure Portal]: https://portal.azure.com/
[aktywować korzyści dla subskrybentów programu Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[utworzyć konto bezpłatnej wersji próbnej]: http://go.microsoft.com/fwlink/?LinkId=623901
[Wypróbuj usługę App Service]: https://azure.microsoft.com/try/app-service/
[Aplikacja sieci Web w usłudze Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Centrum deweloperów języka Java]: /develop/java/
[Omówienie usługi Resource Manager]: ../azure-resource-manager/resource-group-overview.md
[Przekazywanie niestandardowej aplikacji sieci Web Java na platformę Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png



<!--HONumber=Jan17_HO3-->


