---
title: "Tworzenie filtrów za pomocą zestawu .NET SDK usługi Azure Media Services"
description: "W tym temacie opisano sposób tworzenia filtrów dzięki klienta można użyć ich do określonych sekcji strumienia strumienia. Usługa Media Services tworzy dynamiczne manifestów do osiągnięcia tej selektywnego przesyłania strumieniowego."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2f6894ca-fb43-43c0-9151-ddbb2833cafd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: 6c43473b86c14679ace558de478bd95f41d476da
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="creating-filters-with-azure-media-services-net-sdk"></a>Tworzenie filtrów za pomocą zestawu .NET SDK usługi Azure Media Services
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

Począwszy od wersji 2.11, Media Services można zdefiniować filtry dla zasobów. Te filtry są reguły po stronie serwera, które umożliwi klientom wybierz można wykonywać następujące czynności: odtwarzanie tylko części klipu wideo (zamiast odtwarzanie całego), lub określ tylko podzestaw wersji audio i wideo, które urządzenia klienta może obsłużyć (zamiast tego wszystkie wersji skojarzonych z elementu zawartości). Ta filtrowania zasobów odbywa się za pośrednictwem **dynamiczne manifestu**, które są tworzone na żądanie klienta do strumienia wideo oparte na określonej filtry.

Aby uzyskać szczegółowe informacje dotyczące filtrów i dynamicznych manifestu, zobacz [dynamicznie manifesty omówienie](media-services-dynamic-manifest-overview.md).

W tym temacie przedstawiono sposób .NET SDK usługi Media Services umożliwia tworzenie, aktualizowanie i usuwanie filtrów. 

Należy zwrócić uwagę, jeśli zaktualizujesz filtru może potrwać maksymalnie 2 minuty dla punktu końcowego, aby odświeżyć zasady przesyłania strumieniowego. Jeśli zawartość zostało obsłużone przy użyciu tego filtru (i w pamięci podręcznej serwerów proxy i sieci CDN pamięci podręcznych), aktualizacja tego filtru może powodować błędy player. Jest zalecane, aby wyczyścić pamięć podręczną po zaktualizowaniu filtru. Jeśli ta opcja nie jest możliwe, należy rozważyć użycie inny filtr. 

## <a name="types-used-to-create-filters"></a>Typy używane do tworzenia filtrów
Następujące typy są używane podczas tworzenia filtrów: 

* **IStreamingFilter**.  Ten typ jest oparty na następujących interfejsu API REST [filtru](https://docs.microsoft.com/rest/api/media/operations/filter)
* **IStreamingAssetFilter**. Ten typ jest oparty na następujących interfejsu API REST [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* **PresentationTimeRange**. Ten typ jest oparty na następujących interfejsu API REST [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* **FilterTrackSelectStatement** i **IFilterTrackPropertyCondition**. Te typy są oparte na następujących interfejsów API REST [FilterTrackSelect i FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

## <a name="createupdatereaddelete-global-filters"></a>Filtry globalne tworzenia/aktualizacji/odczytu/usuwania
Poniższy kod przedstawia sposób .NET umożliwia tworzenie, aktualizowanie i do odczytu i usuwania filtrów zasobów.

    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();

    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();

    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);

    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);

    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();

    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


## <a name="createupdatereaddelete-asset-filters"></a>Filtry tworzenia/aktualizacji/odczytu/usuwania zasobów
Poniższy kod przedstawia sposób .NET umożliwia tworzenie, aktualizowanie i do odczytu i usuwania filtrów zasobów.

    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();


    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());

    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);

    filter.Update();

    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filterUpdated.Delete();




## <a name="build-streaming-urls-that-use-filters"></a>Tworzenie adresów URL, które należy używać filtrów przesyłania strumieniowego
Aby uzyskać informacje na temat sposobu publikowania i dostarczanie zasobów, zobacz [dostarczania zawartości do klientów omówienie](media-services-deliver-content-overview.md).

Poniższe przykłady przedstawiają sposób dodawania filtrów do przesyłania strumieniowego adresami URL.

**MPEG DASH** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live przesyłania strumieniowego V4 (HLS)**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live przesyłania strumieniowego V3 (HLS)**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Smooth Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zobacz też
[Omówienie dynamicznej manifestów](media-services-dynamic-manifest-overview.md)

