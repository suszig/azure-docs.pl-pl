---
title: "Jak uzyskać wystąpienia procesor multimediów, za pomocą usługi REST | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie tworzenia nośnika składnika procesora do kodowania, przekonwertować format, szyfrowania lub odszyfrowywania zawartości nośnika dla usługi Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 4ad90ad979c5bd74fc55155098c88d5c13cb12e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-get-a-media-processor-instance"></a>Jak uzyskać wystąpienia procesor multimediów
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Omówienie
W usłudze Media Services, który procesor multimediów jest składnikiem, który obsługuje przetwarzania specyficznego dla zadania, takie jak kodowanie format konwersji, szyfrowania lub odszyfrowywania zawartości nośnika. Podczas tworzenia zadania kodowania, szyfrowania lub przekonwertować format zawartości multimedialnej zwykle utworzyć procesor multimediów.

## <a name="azure-media-processors"></a>Procesory multimediów Azure 

Poniższy temat zawiera listę procesory multimediów:

* [Kodowanie procesory multimediów](scenarios-and-availability.md#encoding-media-processors)
* [Procesory multimediów usługi analiza](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
>Podczas uzyskiwania dostępu do obiektów w usłudze Media Services, należy ustawić określonych pól nagłówka i wartości w Twoich żądań HTTP. Aby uzyskać więcej informacji, zobacz [ustawień dla rozwoju interfejsu API REST usługi Media](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Łączenie się z usługą Media Services

Aby uzyskać informacje na temat nawiązywania połączenia z interfejsu API usług AMS, zobacz [dostępu Azure Media Services API przy użyciu uwierzytelniania usługi Azure AD](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Po pomyślnym połączeniu się https://media.windows.net, otrzymasz 301 przekierowanie, określając inny identyfikator URI usługi multimediów. Upewnij się kolejne wywołania nowy identyfikator URI.

## <a name="get-a-media-processor"></a>Pobierz procesor multimediów

Następujące wywołanie REST pokazano, jak uzyskać za pomocą nazwy wystąpienia procesora nośnika (w tym przypadku **Media Encoder Standard**). 

Żądanie:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net

Odpowiedź:

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Następne kroki
Teraz, gdy wiesz, jak uzyskać wystąpienia procesora nośnika, przejdź do [jak kodowanie elementu zawartości](media-services-rest-get-started.md) tematu, w której opisano, jak na potrzeby Media Encoder Standard kodowanie elementu zawartości.

