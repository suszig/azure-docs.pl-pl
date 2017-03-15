---
title: "Stan sesji i usługa Azure Redis Cache w usłudze Azure App Service"
description: "Dowiedz się, jak używać usługi Azure Redis Cache do obsługi buforowania informacji o stanie sesji programu ASP.NET."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
manager: erikre
editor: none
ms.assetid: 4f98d289-2698-464d-85cd-99ec40fad1f2
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 06/27/2016
ms.author: riande
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: a682e51bfaed9056b170c3e9473180ca210557b9
ms.lasthandoff: 01/20/2017


---
# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Stan sesji i usługa Azure Redis Cache w usłudze Azure App Service
W tym temacie wyjaśniono, jak używać usługi Azure Redis Cache do buforowania informacji o stanie sesji.

Jeśli aplikacja sieci Web programu ASP.NET używa informacji o stanie sesji, należy skonfigurować zewnętrznego dostawcę stanu sesji (usługę Azure Redis Cache lub dostawcę stanu sesji programu SQL Server). Jeśli korzystasz z informacji o stanie sesji i nie używasz dostawcy zewnętrznego, obowiązuje ograniczenie do pojedynczego wystąpienia aplikacji sieci Web. Użycie usługi Azure Redis Cache jest najszybszym i najprostszym sposobem zapewnienia tej funkcji.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a id="createcache"></a>Tworzenie pamięci podręcznej
Wykonaj [te kroki](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-cache), aby utworzyć pamięć podręczną.

## <a id="configureproject"></a>Dodawanie pakietu RedisSessionStateProvider NuGet do aplikacji sieci Web
Zainstaluj pakiet NuGet `RedisSessionStateProvider`.  Użyj następującego polecenia do instalacji z poziomu konsoli menedżera pakietów (**Narzędzia** > **Menedżer pakietów NuGet** > **Konsola menedżera pakietów**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`

Aby zainstalować z lokalizacji **Narzędzia** > **Menedżer pakietów NuGet** > **Zarządzaj pakietami NugGet dla rozwiązania**, wyszukaj `RedisSessionStateProvider`.

Aby uzyskać więcej informacji, zapoznaj się ze [stroną pakietu NuGet RedisSessionStateProvider](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/) i sekcją [Configure the cache client](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#NuGet) (Konfigurowanie klienta pamięci podręcznej).

## <a id="configurewebconfig"></a>Modyfikowanie pliku Web.Config
Oprócz tworzenia odwołań do zestawów dla pamięci podręcznej pakiet NuGet dodaje wpisy zastępcze w pliku *web.config*. 

1. Otwórz plik *web.config* i znajdź element **sessionState**.
2. Wprowadź wartości dla `host`, `accessKey`, `port` (wymagany jest port SSL 6380) i ustaw opcję `SSL` na `true`. Te wartości można uzyskać z bloku witryny [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715) dotyczącego wystąpienia pamięci podręcznej. Aby uzyskać więcej informacji, zobacz sekcję [Connect to the cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache) (Nawiązywanie połączenia z pamięcią podręczną). Port bez protokołu SSL jest domyślnie wyłączony w przypadku nowych pamięci podręcznych. Aby uzyskać więcej informacji na temat włączania portu bez protokołu SSL, zobacz sekcję [Access Ports](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) (Porty dostępowe) w temacie [Configure a cache in Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx) (Konfigurowanie pamięci podręcznej w usłudze Azure Redis Cache). Następujący kod przedstawia zmiany w pliku *web.config*, a w szczególności zmiany elementów *port*, *host*, accessKey* i *ssl*.
   
          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;

## <a id="usesessionobject"></a>Użycie obiektu Session w kodzie
Ostatnim krokiem jest rozpoczęcie korzystania z obiektu Session w kodzie ASP.NET. Obiekty są dodawane do stanu sesji przy użyciu metody **Session.Add**. Ta metoda używa par kluczy i wartości do przechowywania elementów w pamięci podręcznej stanu sesji.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Poniższy kod pobiera tę wartość z informacji o stanie sesji.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue;    

Możesz również użyć usługi Pamięć podręczna Redis do buforowania obiektów w aplikacji sieci Web. Aby uzyskać więcej informacji, zobacz [MVC movie app with Azure Redis Cache in 15 minutes](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/) (Tworzenie aplikacji filmowej MVC przy użyciu usługi Azure Redis Cache w ciągu 15 minut).
Aby uzyskać więcej informacji na temat sposobu używania informacji o stanie sesji programu ASP.NET, zobacz [ASP.NET Session State Overview][ASP.NET Session State Overview] (Omówienie informacji o stanie sesji programu ASP.NET).

> [!NOTE]
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Wypróbuj usługę App Service](https://azure.microsoft.com/try/app-service/), w którym wyjaśniono, jak od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service. Bez kart kredytowych i bez zobowiązań.
> 
> 

## <a name="whats-changed"></a>Co zostało zmienione
* Przewodnik dotyczący przejścia od usługi Witryny sieci Web do usługi App Service można znaleźć w temacie [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714) (Usługa Azure App Service i jej wpływ na istniejące usługi platformy Azure).
  
  *Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)*

[installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
[ASP.NET Session State Overview]: http://msdn.microsoft.com/library/ms178581.aspx

[NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
[NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
[CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
[NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
[OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
[CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
[EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
[ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png


