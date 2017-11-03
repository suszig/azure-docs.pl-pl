---
title: "Ochrona zawartości przy użyciu usługi Azure Media Services | Dokumentacja firmy Microsoft"
description: "W tym artykule nadaj Omówienie ochrony zawartości z usługi Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 64be4ea104bd11b8e191e2c8d4170a2de88acb47
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="protecting-content-overview"></a>Ochrona zawartości — omówienie
Usługi Microsoft Azure Media Services umożliwiają zabezpieczenie multimediów od momentu wysłania danych z komputera w ramach procesów przechowywania, przetwarzania i dostarczania. Usługa Media Services umożliwia dostarczanie dynamicznie szyfrowanych za pomocą Standard AES (Advanced Encryption) (przy użyciu kluczy szyfrowania 128-bitowego) lub jedną z głównych DRMs zawartości na żywo i na żądanie: Microsoft PlayReady, Google Widevine i FairPlay firmy Apple. Usługa Media Services udostępnia usługę dostarczania kluczy AES i DRM (PlayReady, Widevine i FairPlay) licencji do autoryzowanych klientów. 

Poniższa ilustracja pokazuje przepływy pracy ochrony zawartości, które obsługuje AMS. 

![Ochrona za pomocą PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[!NOTE]
>Po utworzeniu konta usługi AMS zostanie do niego dodany **domyślny** punkt końcowy przesyłania strumieniowego mający stan **Zatrzymany**. Aby rozpocząć przesyłanie strumieniowe zawartości oraz korzystać z dynamicznego tworzenia pakietów i szyfrowania dynamicznego, punkt końcowy przesyłania strumieniowego, z którego chcesz strumieniowo przesyłać zawartość, musi mieć stan **Uruchomiony**. 

W tym temacie opisano [pojęcia i terminologię](media-services-content-protection-overview.md) istotne dla zrozumienia ochrony zawartości przy użyciu usługi AMS. Temat zawiera także [łącza](media-services-content-protection-overview.md#common-scenarios) do tematów, które przedstawiają sposób osiągnięcia zadań ochrony zawartości. 

## <a name="dynamic-encryption"></a>Szyfrowania dynamicznego
Microsoft Azure Media Services umożliwia dostarczanie zawartości dynamicznie szyfrowany za pomocą szyfrowania DRM lub wyczyść klucza AES: Microsoft PlayReady, Google Widevine i FairPlay firmy Apple.

Obecnie można zaszyfrować następujących formatów przesyłania strumieniowego: HLS, MPEG DASH i Smooth Streaming. Nie można zaszyfrować pobierania progresywnego.

Dla usługi Media Services zaszyfrować element zawartości, należy skojarzyć klucza szyfrowania (CommonEncryption lub EnvelopeEncryption) z zawartości, a także skonfigurować zasady autoryzacji klucza.

Należy również skonfigurować zasady dostarczania elementu zawartości. Jeśli chcesz strumienia zasobów systemu z zaszyfrowanego magazynu, pamiętaj określić sposób dostarcza go przez skonfigurowanie zasad dostarczania elementów zawartości.

Strumień zleconą przez odtwarzacz usługi Media Services używa określonego klucza do dynamicznego szyfrowania z zawartości przy użyciu klucza czyszczenia AES lub szyfrowania DRM. Aby odszyfrować strumienia, odtwarzacza zażąda klucz z usługi dostarczania klucza. Aby zdecydować, czy użytkownik jest autoryzowany do uzyskania klucza, usługa oblicza zasad autoryzacji, które podane dla klucza.


## <a name="storage-encryption"></a>Szyfrowanie magazynu
Użyj szyfrowania magazynu do zawartości lokalnie za pomocą AES 256-bitowego szyfrowania, a następnie przekaż go do magazynu Azure gdzie jest przechowywana szyfrowane, gdy. Elementy zawartości chronione przy użyciu szyfrowania magazynu są automatycznie odszyfrowywane i umieszczane w systemie szyfrowania plików przed kodowaniem, a następnie opcjonalnie ponownie szyfrowane przed przesłaniem zwrotnym w formie nowego elementu zawartości wyjściowej. Pierwotnym zastosowaniem szyfrowania magazynu jest, gdy chcesz zabezpieczyć z wysokiej jakości multimedialnych plików wejściowych pomocą silnego szyfrowania przechowywanych na dysku.

Aby dostarczyć zasób zaszyfrowanych magazynu, należy skonfigurować zasady dostarczania elementu zawartości będzie wówczas traktował Media Services sposób dostarczania zawartości. Przed zawartości mogą być przesyłane strumieniowo, serwer przesyłania strumieniowego usuwa szyfrowanie magazynu i strumieni zawartości za pomocą zasad określonym dostarczania (na przykład AES, wspólnego szyfrowania lub bez szyfrowania).

## <a name="common-encryption-cenc"></a>Szyfrowania Common encryption (CENC)
Typowe szyfrowania jest używane do szyfrowania zawartości przy użyciu PlayReady lub / i Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Przy użyciu szyfrowania cbcs-aapl
Cbcs-aapl jest używane do szyfrowania zawartości przy użyciu FairPlay.

## <a name="envelope-encryption"></a>Szyfrowanie koperty
Użyj tej opcji, jeśli chcesz chronić zawartość z AES-128, klucz niezaszyfrowany. Jeśli chcesz bardziej bezpieczna, wybierz jedną z DRMs wymienione w tym temacie. 

## <a name="licenses-and-keys-delivery-service"></a>Usługi dostarczania licencji i kluczy
Usługa Media Services udostępnia usługę dostarczania licencji DRM (PlayReady i Widevine, FairPlay) i AES wyczyść klucze do autoryzowanych klientów. Można użyć [portalu Azure](media-services-portal-protect-content.md), interfejsu API REST lub SDK usługi Media Services dla platformy .NET, aby skonfigurować zasady autoryzacji i uwierzytelnianie licencji i kluczy.

## <a name="token-restriction"></a>Ograniczenia tokenu
Zasady autoryzacji klucza zawartości mogą mieć jedno lub więcej ograniczeń: ograniczenie otwarte lub ograniczenie tokenu. Zasadzie ograniczenia tokenu musi towarzyszyć token wystawiony przez usługę STS (Secure Token Service). Usługa Media Services obsługuje tokenów w formacie proste tokenów sieci Web (SWT) i format tokenu Web JSON (JWT). Usługi Media Services nie zapewnia bezpieczny tokenu usługi. Można utworzyć niestandardowy STS lub korzystać z usługi Microsoft Azure ACS do wydawania tokenów. Usługa tokenu Zabezpieczającego musi być skonfigurowana do utworzenia tokenu podpisany z określonego klucza i problem oświadczenia określony w konfiguracji ograniczenia tokenu. Usługa Media Services klucza dostawy zwróci żądanego (licencji lub klucza) do klienta, jeśli token jest prawidłowy i oświadczeń z tokenu dopasowania tych skonfigurowana dla klucza (lub licencji).

Podczas konfigurowania token ograniczony zasad, należy określić klucz podstawowy weryfikacji, wystawcy i parametry odbiorców. Klucz podstawowy weryfikacji zawiera klucz, który został podpisany token, z, wystawca jest bezpieczne usługi tokenu, który wystawia token. Odbiorców (nazywane również zakres) opisano celem tokenu lub zasobu tokenu zezwala na dostęp do. Usługa Media Services klucza dostawy weryfikuje, czy te wartości w tokenie pasują do wartości w szablonie.

## <a name="streaming-urls"></a>Adresy URL przesyłania strumieniowego
Jeśli zawartości została zaszyfrowana z więcej niż jeden DRM, należy użyć tag szyfrowania w adresie URL przesyłania strumieniowego: (format = "m3u8-aapl, szyfrowanie = 'xxx').

Następujące kwestie:

* Można określić tylko zero lub jeden typ szyfrowania.
* Typ szyfrowania nie ma określonego w adresie url, jeśli tylko jeden szyfrowania została zastosowana do zasobu.
* Typ szyfrowania jest uwzględniana wielkość liter.
* Można określić następujące typy szyfrowania:  
  * **cenc**: szyfrowania Common encryption (Playready lub Widevine)
  * **cbcs-aapl**: Fairplay
  * **CBC**: szyfrowanie AES envelope.

## <a name="common-scenarios"></a>Typowe scenariusze
Poniższe tematy przedstawiają sposób chronić zawartość w magazynie, dostarczanie dynamicznie szyfrowanych multimediów strumieniowych, użyj usługi dostarczania klucz/licencjonowania AMS

* [Chroń za pomocą AES](media-services-protect-with-aes128.md) 
* [Chroń za pomocą PlayReady i Widevine](media-services-protect-with-drm.md)
* [Strumienia z HLS chronione zawartości z FairPlay firmy Apple i/lub PlayReady](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Dodatkowe scenariusze
* [Integrowanie usługi Azure licencji PlayReady z własnych serwer przesyłania strumieniowego/modułu szyfrującego](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
* [Przy użyciu castLabs na potrzeby dostarczania licencji DRM do usługi Azure Media Services](media-services-castlabs-integration.md)

>[!NOTE]
>Scenariusz, w której jest używany DRM server(technology) zewnętrzne i strumień AMS nie jest obecnie obsługiwane.


## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Linki pokrewne
[Anonsowanie PlayReady jako usługę i dynamicznego szyfrowania AES w usłudze Azure Media Services](http://mingfeiy.com/playready)

[Wyjaśniono cennik dostarczania licencji PlayReady usługi multimediów Azure](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Jak można debugować AES strumienia zaszyfrowane w usłudze Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[Authenitcation tokenu JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integracja aplikacji na podstawie Azure Media Services OWIN MVC z usługi Azure Active Directory i ograniczenie klucza dostarczania zawartości na podstawie oświadczeń JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Użyj Azure ACS do wydawania tokenów](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
