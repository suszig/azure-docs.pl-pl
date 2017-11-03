---
title: "Zarządzaj wygasaniem zawartości sieci web w usłudze Azure CDN | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zarządzać wygasaniem zawartości usług Azure Web Apps/Cloud Services, ASP.NET lub usługi IIS w usłudze Azure CDN."
services: cdn
documentationcenter: .NET
author: zhangmanling
manager: erikre
editor: 
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: c207d780857a61d4b1fc0f39e6185cae67abc955
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Zarządzaj wygasaniem zawartości usług Azure Web Apps/Cloud Services, ASP.NET lub usługi IIS w usłudze Azure CDN
> [!div class="op_single_selector"]
> * [Usługi aplikacji/w chmurze Azure sieci Web, ASP.NET lub usług IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Usługa blob magazynu Azure](cdn-manage-expiration-of-blob-content.md)
> 
> 

Mogą być buforowane pliki z dowolnego publicznie dostępnego źródła serwera sieci web w usłudze Azure CDN, dopóki nie upłynie jego czas wygaśnięcia (TTL).  Czas wygaśnięcia jest określany przez [ *Cache-Control* nagłówka](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) w odpowiedzi HTTP z serwera pochodzenia.  W tym artykule opisano sposób ustawiania `Cache-Control` nagłówki dla aplikacji sieci Web platformy Azure, usługi w chmurze Azure, aplikacji programu ASP.NET i witryny internetowe usługi informacyjne, które są skonfigurowane podobnie.

> [!TIP]
> Można ustawić nie TTL w pliku.  W takim przypadku Azure CDN automatycznie stosuje domyślny czas wygaśnięcia wynosi siedem dni.
> 
> Aby uzyskać więcej informacji na temat działania usługi Azure CDN do Przyspieszanie dostępu do plików i innych zasobów, zobacz [Omówienie usługi Azure CDN](cdn-overview.md).
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a>Nagłówki Cache-Control ustawienia w konfiguracji
Dla zawartości statycznej, takich jak obrazy i arkusze stylów, możesz kontrolować częstotliwość aktualizacji przez zmodyfikowanie **applicationHost.config** lub **web.config** plików dla aplikacji sieci web.  **System.webServer\staticContent\clientCache** ustawi elementu w pliku konfiguracyjnym `Cache-Control` nagłówek dla zawartości. Aby uzyskać **web.config**, ustawienia konfiguracji będzie miało wpływ na wszystkie elementy w folderze i wszystkie podfoldery, jeśli zastąpiona na poziomie podfolder.  Można na przykład domyślny czas wygaśnięcia główny zestaw ma wszystkie zawartości statycznej pamięci podręcznej dla 3 dni, ale ma podfolder, który ma więcej zmiennych zawartość z ustawieniem pamięci podręcznej 6 godzin.  Dla **applicationHost.config**, wszystkie aplikacje w witrynie zostaną zmienione, ale może zostać zastąpiona w **web.config** plików w aplikacjach.

Następujący kod XML przedstawiono przykład ustawienie **clientCache** określić maksymalny wiek 3 dni:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Określanie **UseMaxAge** dodaje `Cache-Control: max-age=<nnn>` nagłówka w odpowiedzi na podstawie wartości określone w **CacheControlMaxAge** atrybutu. Format elementu timespan **cacheControlMaxAge** atrybutu <days>.<hours>:<min>:<sec>. Aby uzyskać więcej informacji na temat **clientCache** węzła, zobacz [pamięci podręcznej klienta <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Ustawianie Cache-Control nagłówków w kodzie
Dla aplikacji ASP.NET można ustawić CDN buforowanie programowo, ustawiając **HttpResponse.Cache** właściwości. Aby uzyskać więcej informacji na temat **HttpResponse.Cache** właściwości, zobacz [właściwości HttpResponse.Cache](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) i [HttpCachePolicy klasy](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Jeśli chcesz programowo pamięci podręcznej zawartości aplikacji w programie ASP.NET, upewnij się, czy zawartość jest oznaczony jako buforowalnej ustawiając HttpCacheability *publicznego*. Ponadto upewnij się, że moduł weryfikacji pamięci podręcznej jest ustawiona. Moduł sprawdzania poprawności pamięci podręcznej może być ostatniej modyfikacji sygnatury czasowej ustawiony przez wywołanie SetLastModified lub wartość etag ustawiony przez wywołanie SetETag. Opcjonalnie można również określić czas wygaśnięcia pamięci podręcznej przez wywołanie metody SetExpires lub może polegać na domyślny algorytm heurystyczny pamięci podręcznej, opisem we wcześniejszej części tego dokumentu.  

Na przykład do pamięci podręcznej zawartości przez jedną godzinę, należy dodać:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Następne kroki
* [Przeczytaj szczegółowe informacje na temat **clientCache** — element](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Przeczytaj dokumentację **HttpResponse.Cache** właściwości](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Przeczytaj dokumentację **klasy HttpCachePolicy**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

