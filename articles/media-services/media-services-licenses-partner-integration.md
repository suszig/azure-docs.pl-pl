---
title: "Przy użyciu partnerów do dostarczania licencji Widevine do usługi Azure Media Services | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak Azure Media Services (AMS) umożliwia dostarczanie dynamicznie szyfrowanych przez AMS z PlayReady i Widevine DRMs strumienia. Licencja PlayReady pochodzi z serwera licencji Media Services PlayReady i licencji Widevine jest dostarczany przez serwer licencji castLabs."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5bcad5a4-c0bb-4871-9cce-808a913c53e6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 6867e4f910970121df3858516c6bab3114c3c6f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a>Korzystanie z partnerów w celu dostarczania licencji Widevine do usługi Azure Media Services
## <a name="overview"></a>Omówienie
Microsoft Azure Media Services umożliwia dostarczanie MPEG-DASH chronione przy użyciu Widevine DRM, są szyfrowane zgodnie ze specyfikacją wspólnego szyfrowania (CENC).

Począwszy od platformy .NET SDK usługi Media Services wersja 3.5.2 Media Services umożliwia skonfigurowanie szablonu licencji Widevine i uzyskać licencji Widevine. Licencje Widevine są dostępne również u następujących partnerów usługi AMS: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) i [castLabs](http://castlabs.com/company/partners/azure/).

## <a name="castlabs"></a>castLabs
Można użyć [castLabs](http://castlabs.com/company/partners/azure/) dostarczać licencje Widevine. Aby uzyskać więcej informacji, zobacz [przy użyciu castLabs DRM dostarczać licencje do usługi Azure Media Services](media-services-castlabs-integration.md)

## <a name="axinom"></a>Axinom
Można użyć [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) dostarczać licencje Widevine. Aby uzyskać więcej informacji, zobacz [Axinom przy użyciu DRM dostarczać licencje do usługi Azure Media Services](media-services-axinom-integration.md)

## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zobacz też
[Używanie dynamicznego szyfrowania Common Encryption w usługach PlayReady i Widevine](media-services-protect-with-drm.md)

[Mingfei w blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

