---
title: "Przekaż token uwierzytelniania do usługi Azure Media Services | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wysyłać tokeny uwierzytelniania z klienta do usługi dostarczania klucza usługi Azure Media Services"
services: media-services
keywords: "token uwierzytelniania w usłudze Content protection DRM,"
documentationcenter: 
author: dbgeorge
manager: jasonsue
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: dwgeo
ms.openlocfilehash: 0e56726266898e5738dd797a8a019987d457170e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="how-clients-pass-tokens-to-azure-media-services-key-delivery-service"></a>Jak klienci przekazywania tokenów do usługi dostarczania klucza usługi Azure Media Services
Stale będziemy mieć pytania dotyczące procesu jak odtwarzacza można przekazać tokenu do naszych usług klucza dostawy, które zostanie Uzyskaj zweryfikowane i odtwarzacz uzyskuje klucza. Firma Microsoft obsługuje Simple Web Token (SWT) i tokenu Web JSON (JWT) tych dwóch formatów tokenu. Token uwierzytelniania można zastosować do dowolnego typu klucza — niezależnie od robią wspólnego szyfrowania lub AES envelope szyfrowanie w systemie.

Te są sposoby, które można przekazać tokenu do odtwarzacza, zależy od player i platform docelowych:
- Za pomocą nagłówek autoryzacji HTTP.
> [!NOTE]
> Należy pamiętać, że prefiksu "Bearer" oczekuje na specyfikacji protokołu OAuth 2.0. Brak odtwarzacza próbki z konfiguracją Token hostowanej w lokalizacji usługi Azure Media Player [strony pokaz](http://ampdemo.azureedge.net/). Wybierz AES (JWT Token) lub AES (SWT Token), można ustawić źródła wideo. Token jest przekazywany za pośrednictwem nagłówek autoryzacji.

- Przez dodanie parametru zapytania Url z "token = tokenvalue".  
> [!NOTE]
> Należy pamiętać, że prefiksu "Bearer". Ponieważ token jest wysyłany przy użyciu adresu URL, konieczne będzie ochrony ciąg tokena. Oto kod przykładowy C# o tym, jak to zrobić:

```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
```

- Za pomocą pola CustomData.
Dla PlayReady licencji nabycia tylko za pomocą pola CustomData pozyskiwania licencji PlayReady żądania. W takim przypadku token musi być w dokumencie xml opisane poniżej.

```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
```
Umieść token uwierzytelniania w <Token> elementu.

- Za pomocą listy HLS. Jeśli trzeba skonfigurować Token uwierzytelniania dla AES + HLS odtwarzania na iOS/Safari, nie istnieje sposób może bezpośrednio wysyłać w tokenie. Zobacz to [wpis w blogu](http://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/) na temat sposobu listy odtwarzania, aby włączyć ten scenariusz alternate.

## <a name="next-steps"></a>Następne kroki
Przejrzyj ścieżki szkoleniowe dotyczące usługi Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]