---
title: "Zarządzaj wygasaniem zawartości sieci web w sieci dostarczania zawartości platformy Azure | Dokumentacja firmy Microsoft"
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
ms.date: 11/10/2017
ms.author: mazha
ms.openlocfilehash: dca6ca5f21f4a4f1701af57eb40d92094b6a4754
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Zarządzaj wygasaniem zawartości sieci web w sieci dostarczania zawartości platformy Azure
> [!div class="op_single_selector"]
> * [Zawartość sieci Web platformy Azure](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 

Pliki z dowolnego publicznie dostępnego źródła serwera sieci web mogą być buforowane w Azure Content Delivery Network (CDN), dopóki nie upłynie ich czas wygaśnięcia (TTL). Czas wygaśnięcia jest określany przez `Cache-Control` nagłówka odpowiedzi HTTP z serwera pochodzenia. W tym artykule opisano sposób ustawiania `Cache-Control` nagłówki dla funkcji aplikacji sieci Web Microsoft Azure App Service, usługi w chmurze Azure, aplikacji ASP.NET oraz witryny usług Internet Information Services (IIS), które są skonfigurowane podobnie. Można ustawić `Cache-Control` nagłówka przy użyciu plików konfiguracyjnych lub programowo. 

Ustawienia pamięci podręcznej z portalu Azure można też kontrolować przez ustawienie [CDN buforowanie reguły](cdn-caching-rules.md). Jeśli skonfigurować jeden lub więcej buforowanie reguły i ustawić ich zachowanie buforowania **zastąpienia** lub **obejścia pamięci podręcznej**, wprowadzone do pochodzenia ustawień buforowania omówione w tym artykule są ignorowane. Informacje ogólne koncepcje buforowania, zobacz [działa jak buforowanie](cdn-how-caching-works.md).

> [!TIP]
> Można ustawić nie TTL w pliku. W takim przypadku Azure CDN automatycznie stosuje domyślny czas wygaśnięcia wynosi siedem dni, jeśli nie zdefiniowano buforowania reguł w portalu Azure. To ustawienie domyślne TTL dotyczy tylko optymalizacji ogólne sieci web. Dla optymalizacji dużych plików domyślny czas wygaśnięcia wynosi jeden dzień, a dla multimediów strumieniowych optymalizacji, domyślny czas wygaśnięcia wynosi 1 rok.
> 
> Aby uzyskać więcej informacji na temat działania usługi Azure CDN do Przyspieszanie dostępu do plików i innych zasobów, zobacz [Omówienie usługi Azure Content Delivery Network](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-by-using-configuration-files"></a>Ustawienie nagłówki Cache-Control za pomocą plików konfiguracji
Dla zawartości statycznej, takich jak obrazy i arkusze stylów, możesz kontrolować częstotliwość aktualizacji przez zmodyfikowanie **applicationHost.config** lub **Web.config** pliki konfiguracji aplikacji sieci web. `<system.webServer>/<staticContent>/<clientCache>` Elementu w obu zestawach pliku `Cache-Control` nagłówek dla zawartości.

### <a name="using-applicationhostconfig-files"></a>Przy użyciu pliku ApplicationHost.config plików
**ApplicationHost.config** plik jest plikiem głównego systemu konfiguracji usług IIS. Ustawienia konfiguracji w **ApplicationHost.config** pliku wpływają na wszystkie aplikacje w lokacji, ale są zastępowane przez ustawienia dowolnej **Web.config** plików, które istnieją dla aplikacji sieci web.

### <a name="using-webconfig-files"></a>Przy użyciu plików Web.config
Z **Web.config** plików, można dostosować sposób zachowania aplikacji sieci web całego lub określonego katalogu w aplikacji sieci web. Zazwyczaj mają co najmniej jeden **Web.config** pliku w katalogu głównym aplikacji sieci web. Dla każdego **Web.config** plików w określonym folderze, ustawienia konfiguracji mają wpływ na wszystkie elementy w tym folderze i jego podfolderach chyba że są one zastąpione na poziomie podfolder przez inną **Web.config**pliku. Na przykład można ustawić `<clientCache>` element **Web.config** pliku w katalogu głównym aplikacji sieci web do pamięci podręcznej wszystkie zawartość statyczną w aplikacji sieci web dla trzech dni. Można również dodać **Web.config** plik w podfolderze o więcej zmiennych zawartości (na przykład `\frequent`) i ustawić jej `<clientCache>` elementu do pamięci podręcznej zawartości podfolder przez 6 godzin. Wynikiem jest tej zawartości w całej witryny sieci web będą buforowane na 3 dni, z wyjątkiem bez zawartości `\frequent` katalogu, który będzie zapisywane tylko sześciu godzin.  

W poniższym przykładzie XML przedstawiono sposób ustawiania `<clientCache>` w pliku konfiguracji, aby określić maksymalny wiek trzy dni:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Aby użyć **cacheControlMaxAge** atrybutu, należy ustawić wartość **cacheControlMode** atrybutu `UseMaxAge`. To ustawienie spowodował nagłówka HTTP i dyrektywy `Cache-Control: max-age=<nnn>`, mają zostać dodane do odpowiedzi. Format wartości timespan dla **cacheControlMaxAge** atrybutu `<days>.<hours>:<min>:<sec>`. Wartość jest konwertowany na sekund i jest używany jako wartość `Cache-Control` `max-age` dyrektywy. Aby uzyskać więcej informacji na temat `<clientCache>` elementu, zobacz [pamięci podręcznej klienta <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-programmatically"></a>Ustawienie programowo nagłówki Cache-Control
Dla aplikacji ASP.NET, sterowanie CDN buforowanie programowo, ustawiając **HttpResponse.Cache** właściwości interfejsu API programu .NET. Informacje o **HttpResponse.Cache** właściwości, zobacz [właściwości HttpResponse.Cache](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) i [HttpCachePolicy klasy](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Aby programowo zawartości pamięci podręcznej aplikacji w programie ASP.NET wykonaj następujące kroki:
   1. Sprawdź, czy zawartość jest oznaczony jako buforowalnej przez ustawienie `HttpCacheability` do `Public`. 
   2. Ustaw moduł weryfikacji pamięci podręcznej, wywołując jedną z następujących `HttpCachePolicy` metod:
      - Wywołanie `SetLastModified` można ustawić wartość sygnatury czasowej `Last-Modified` nagłówka.
      - Wywołanie `SetETag` można ustawić wartości dla `ETag` nagłówka.
   3. Opcjonalnie można określić czas wygaśnięcia pamięci podręcznej przez wywołanie metody `SetExpires` można ustawić wartości dla `Expires` nagłówka. W przeciwnym razie domyślny algorytm heurystyczny pamięci podręcznej, opisanych wcześniej w tym dokumencie mają zastosowanie.

Na przykład do pamięci podręcznej zawartości przez jedną godzinę, Dodaj następujący kod C#:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="testing-the-cache-control-header"></a>Testowanie nagłówek Cache-Control
Można łatwo sprawdzić ustawienia czas wygaśnięcia zawartości sieci web. Przy użyciu przeglądarki [narzędzia dla deweloperów](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test, który obejmuje zawartości sieci web `Cache-Control` nagłówka odpowiedzi. Można również użyć narzędzia takie jak **wget**, [Postman](https://www.getpostman.com/), lub [Fiddler](http://www.telerik.com/fiddler) Aby sprawdzić nagłówki odpowiedzi.

## <a name="next-steps"></a>Następne kroki
* [Przeczytaj szczegółowe informacje na temat **clientCache** — element](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Przeczytaj dokumentację **HttpResponse.Cache** właściwości](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Przeczytaj dokumentację **HttpCachePolicy — klasa**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx)  
* [Dowiedz się więcej o buforowanie pojęcia](cdn-how-caching-works.md)
