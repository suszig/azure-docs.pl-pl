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
ms.openlocfilehash: 6475a865b9d1b263bd7cc68c99acdb5f6959531e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="protecting-content-overview"></a>Ochrona zawartości — omówienie
Usługi Microsoft Azure Media Services umożliwiają zabezpieczenie multimediów od momentu wysłania danych z komputera w ramach procesów przechowywania, przetwarzania i dostarczania. Usługa Media Services umożliwia dostarczanie dynamicznie szyfrowanych za pomocą Advanced Encryption Standard (AES-128) lub jeden z trzech głównych systemów DRM zawartości na żywo i na żądanie: Microsoft PlayReady, Google Widevine i FairPlay firmy Apple. Usługa Media Services udostępnia usługę dostarczania kluczy AES i DRM (PlayReady, Widevine i FairPlay) licencji do autoryzowanych klientów. 

Poniższa ilustracja przedstawia przepływ pracy dotyczący ochrony zawartości dla usługi Azure Media Services. 

![Ochrona za pomocą PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

W tym artykule opisano pojęcia i terminologię istotne dla zrozumienia ochrony zawartości przy użyciu usługi AMS. Artykuł również zawiera łącza do artykułów, które zawierają opis do ochrony zawartości. 

## <a name="dynamic-encryption"></a>Szyfrowanie dynamiczne
Usługa Azure Media Services umożliwia dostarczanie zawartości dynamicznie szyfrowany za pomocą szyfrowania DRM lub wyczyść klucza AES: Microsoft PlayReady, Google Widevine i FairPlay firmy Apple. Obecnie można zaszyfrować następujących formatów przesyłania strumieniowego: HLS, MPEG DASH i Smooth Streaming. Szyfrowanie na pobierania progresywnego nie jest obsługiwane. Każda metoda szyfrowania obsługuje następujących protokołów:
- AES: MPEG-DASH, Smooth Streaming i HLS
- PlayReady: MPEG-DASH, Smooth Streaming i HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Aby zaszyfrować element zawartości, należy skojarzyć klucz szyfrowania zawartości z zawartości, a także skonfigurować zasady autoryzacji klucza. Kluczy zawartości można określić lub generowane automatycznie przez usługę Media Services.

Należy również skonfigurować zasady dostarczania elementu zawartości. Jeśli ma być przesyłana strumieniowo zasób zaszyfrowanych magazynu, upewnij się, że Określ sposób dostarczania go przez skonfigurowanie zasad dostarczania elementów zawartości.

Strumień zleconą przez odtwarzacz usługi Media Services używa określonego klucza do dynamicznego szyfrowania z zawartości przy użyciu klucza czyszczenia AES lub szyfrowania DRM. Aby odszyfrować strumienia, odtwarzacza zażąda klucza od usługi klucza dostawy AMS. Aby zdecydować, czy użytkownik jest autoryzowany do uzyskania klucza, usługa oblicza zasad autoryzacji, które podane dla klucza.

## <a name="aes-128-clear-key-vs-drm"></a>AES-128, klucz niezaszyfrowany vs DRM
Klienci często zastanawiasz się, czy należy używać szyfrowania AES lub DRM system. Główną różnicą między przy użyciu szyfrowania AES i systemy DRM jest aby z szyfrowania AES klucz zawartości jest przekazywane do klienta w niezaszyfrowanej postaci ("w Wyczyść"). W związku z tym klucz używany do szyfrowania zawartości można wyświetlić w śledzenia sieci na komputerze klienckim w postaci zwykłego tekstu. AES-128, klucz niezaszyfrowany jest odpowiedni dla przypadków użycia, gdy przeglądarka jest zaufany (np: szyfrowania firmowej wideo rozproszonych w obrębie firmy będą wyświetlane przez pracowników).

PlayReady, Widevine i FairPlay wszystkie zapewnia wyższy poziom szyfrowania w porównaniu do AES-128 wyczyść klucza szyfrowania. Klucz zawartości jest przekazywane w postaci zaszyfrowanej. Ponadto jest dojście w bezpiecznym środowisku na poziomie systemu operacyjnego gdzie jest znacznie trudniejsze złośliwy użytkownik na ataki. Przypadki użycia, gdy przeglądarka nie może być zaufany i wymagają najwyższy poziom zabezpieczeń zaleca się DRM.

## <a name="storage-encryption"></a>Szyfrowanie w usłudze Storage
Można użyć szyfrowania magazynu do zawartości lokalnie, przy użyciu standardu AES 256-bitowego szyfrowania i przekaż go do magazynu Azure gdzie jest przechowywana szyfrowane, gdy. Elementy zawartości chronione przy użyciu szyfrowania magazynu są automatycznie odszyfrowywane i umieszczane w systemie szyfrowania plików przed kodowaniem, a następnie opcjonalnie ponownie szyfrowane przed przesłaniem zwrotnym w formie nowego elementu zawartości wyjściowej. Pierwotnym zastosowaniem szyfrowania magazynu jest, gdy chcesz zabezpieczyć Twoje wysokiej jakości multimedialnych plików wejściowych z silne szyfrowanie przechowywanych na dysku.

Aby dostarczyć zasób zaszyfrowanych magazynu, należy skonfigurować zasady dostarczania elementu zawartości będzie wówczas traktował Media Services sposób dostarczania zawartości. Aby mogła być przesłana strumieniowo zawartości, serwer przesyłania strumieniowego odszyfrowuje i strumieni zawartości za pomocą zasad określonym dostarczania (na przykład AES, wspólnego szyfrowania lub bez szyfrowania).

## <a name="types-of-encryption"></a>Typy szyfrowania
PlayReady i Widevine wykorzystywać Common Encryption (tryb Ewidencyjne AES). FairPlay używa szyfrowania trybie AES CBC. Wyczyść klucza szyfrowania AES-128 używa szyfrowania koperty.

## <a name="licenses-and-keys-delivery-service"></a>Usługi dostarczania licencji i kluczy
Usługi Media Services udostępnia usługę dostarczania kluczy do dostarczania licencji DRM (PlayReady, Widevine, FairPlay) i kluczy AES do autoryzowanych klientów. Można użyć [portalu Azure](media-services-portal-protect-content.md), interfejsu API REST lub SDK usługi Media Services dla platformy .NET, aby skonfigurować zasady autoryzacji i uwierzytelnianie licencji i kluczy.

## <a name="control-content-access"></a>Kontrola dostępu do zawartości
Można kontrolować, kto ma dostęp do zawartości przez skonfigurowanie zasad autoryzacji klucza zawartości. Zasady autoryzacji klucza zawartości obsługuje ograniczenie otwarte lub tokenu.

### <a name="open-authorization"></a>Otwórz autoryzacji
Przy użyciu zasad autoryzacji Otwórz klucz zawartości jest wysyłane do dowolnego klienta (nie ograniczenia).

### <a name="token-authorization"></a>Token autoryzacji
Za pomocą zasad ograniczonej token autoryzacji klucza zawartości będzie tylko wysyłania do klienta, który stanowi prawidłowe JSON Web Token (JWT) lub Simple Web Token (SWT) w żądaniu klucz/licencji. Token ten musi być wystawiony przez Secure Token Service (STS). Można użyć usługi Microsoft Active Directory jako tokenu Zabezpieczającego lub wdrożyć niestandardowe STS. Usługa tokenu Zabezpieczającego musi być skonfigurowana do utworzenia tokenu podpisany z określonego klucza i problem oświadczenia określony w konfiguracji ograniczenia tokenu. Usługa Media Services klucza dostawy zwróci żądanej klucz/licencji do klienta, jeśli token jest prawidłowy i oświadczenia w tokenie pasują do klucza/licencji.

Podczas konfigurowania token ograniczony zasad, należy określić klucz podstawowy weryfikacji, wystawcy i parametry odbiorców. Klucz podstawowy weryfikacji zawiera klucz, który został podpisany token, z, wystawca jest bezpieczne usługi tokenu, który wystawia token. Odbiorców, nazywane również zakres, w tym artykule opisano celem tokenu lub zasobu tokenu zezwala na dostęp do. Usługa Media Services klucza dostawy weryfikuje, czy te wartości w tokenie pasują do wartości w szablonie.

## <a name="streaming-urls"></a>Adresy URL przesyłania strumieniowego
Jeśli zawartości została zaszyfrowana z więcej niż jeden DRM, należy użyć tag szyfrowania w adresie URL przesyłania strumieniowego: (format = "m3u8-aapl, szyfrowanie = 'xxx').

Następujące kwestie:
* Można określić maksymalnie jeden typ szyfrowania.
* Typ szyfrowania nie trzeba określonego w adresie URL, jeśli tylko jeden szyfrowania została zastosowana do zasobu.
* Typ szyfrowania jest uwzględniana wielkość liter.
* Można określić następujące typy szyfrowania:  
  * **cenc**: PlayReady lub Widevine (Common Encryption)
  * **cbcs-aapl**: dla FairPlay (szyfrowanie AES CBC)
  * **CBC**: dla szyfrowanie AES envelope (szyfrowanie koperty)

## <a name="next-steps"></a>Następne kroki
Następujące artykuły opisano kolejne kroki, aby zacząć korzystać z ochrony zawartości:
* [Ochrona przy użyciu szyfrowania magazynu](media-services-rest-storage-encryption.md)
* [Ochrona przy użyciu szyfrowania AES](media-services-protect-with-aes128.md)
* [Chroń za pomocą PlayReady i Widevine](media-services-protect-with-playready-widevine.md)
* [Chroń za pomocą FairPlay](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>Linki powiązane
[Wyjaśniono cennik dostarczania licencji PlayReady usługi multimediów Azure](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Jak można debugować AES strumienia zaszyfrowane w usłudze Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[Uwierzytelniania tokenu JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrowanie aplikacji usługi Azure Media Services OWIN MVC z usługą Azure Active Directory i ograniczenie klucza dostarczania zawartości na podstawie oświadczeń JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
