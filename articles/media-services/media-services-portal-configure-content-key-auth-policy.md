---
title: "Skonfiguruj zasady autoryzacji klucza zawartości przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania zasad autoryzacji klucza zawartości."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 5a35c7255a1c30a693862589c14f6a22a1900790
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-content-key-authorization-policy"></a>Skonfiguruj zasady autoryzacji klucza zawartości
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Omówienie
Microsoft Azure Media Services umożliwia dostarczanie strumieni MPEG-DASH, Smooth Streaming i HTTP-Live-Streaming (HLS) chronionych z Standard AES (Advanced Encryption) (przy użyciu kluczy szyfrowania 128-bitowego) lub [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS umożliwia także dostarczanie zaszyfrowanych za pomocą Widevine DRM strumienie DASH. Obie usługi, PlayReady i Widevine, szyfrują dane zgodnie ze specyfikacją Common Encryption (ISO/IEC CENC 23001-7).

Media Services udostępnia również **usługi dostarczania licencji/klucz** z których klienci może uzyskać kluczy AES lub licencje PlayReady/Widevine, aby odtworzyć zaszyfrowaną zawartość.

W tym temacie pokazano, jak skonfigurować zasady autoryzacji klucza zawartości przy użyciu portalu Azure. Klucz później może służyć do dynamicznego szyfrowania zawartości. Należy pamiętać, że obecnie można zaszyfrować następujące przesyłania strumieniowego formaty: HLS, MPEG DASH i Smooth Streaming. Nie można zaszyfrować pobierania progresywnego.

Gdy odtwarzacza zażąda strumienia, który ma ustawioną wartość był dynamicznie szyfrowany, Media Services używa klucza skonfigurowanych do dynamicznego szyfrowania zawartości przy użyciu szyfrowania AES lub DRM. Aby odszyfrować strumienia, odtwarzacza zażąda klucz z usługi dostarczania klucza. Aby zdecydować, czy użytkownik jest autoryzowany do uzyskania klucza, usługa oblicza zasad autoryzacji, które podane dla klucza.

Jeśli planujesz mieć wiele kluczy zawartości lub aby określić **usługi dostarczania licencji/klucz** adresu URL innego niż usługi Media Services klucza dostawy, za pomocą zestawu .NET SDK usługi Media lub interfejsów API REST.

[Skonfiguruj zasady autoryzacji klucza zawartości przy użyciu zestawu .NET SDK usługi multimediów](media-services-dotnet-configure-content-key-auth-policy.md)

[Skonfiguruj zasady autoryzacji klucza zawartości przy użyciu interfejsu API REST usługi multimediów](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Zagadnienia do rozważenia:
* Po utworzeniu konta usługi AMS **domyślne** punktu końcowego przesyłania strumieniowego jest dodany do Twojego konta w **zatrzymane** stanu. Aby rozpocząć przesyłanie strumieniowe zawartości i korzystać z dynamicznego tworzenia pakietów i dynamicznego szyfrowania, punkt końcowy przesyłania strumieniowego musi być w **systemem** stanu. 
* Zawartości musi zawierać zestaw o adaptacyjnej szybkości bitowej MP4s lub pliki Smooth Streaming adaptacyjną szybkością transmisji bitów. Aby uzyskać więcej informacji, zobacz [kodowanie elementu zawartości](media-services-encode-asset.md).
* Usługi dostarczania klucza buforuje ContentKeyAuthorizationPolicy i jego obiektów pokrewnych (opcje zasad i ograniczeń) przez 15 minut.  Po utworzeniu ContentKeyAuthorizationPolicy i określanie użycia 'Token' ograniczeń, przetestowanie i następnie zaktualizuj zasady do ograniczenia "Otwórz", potrwa około 15 minut, zanim zasad zmienia się na "Otwórz" wersję zasad.

## <a name="how-to-configure-the-key-authorization-policy"></a>Porady: Konfigurowanie zasad autoryzacji klucza
Aby skonfigurować zasady autoryzacji klucza, wybierz **ochrony zawartości** strony.

Usługa Media Services obsługuje wiele sposobów uwierzytelniania użytkowników, którzy tworzą żądania klucza. Zasady autoryzacji klucza zawartości mogą mieć **Otwórz**, **tokenu**, lub **IP** ograniczenia autoryzacji (**IP** można skonfigurować za pomocą przerwę lub zestawu .NET SDK).

### <a name="open-restriction"></a>Ograniczenie otwarte
**Otwórz** ograniczeń oznacza, że system będzie dostarczać klucza dla każdego, kto wysyła żądanie klucza. To ograniczenie może być przydatna do celów testowych.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Ograniczenia tokenu
Aby wybrać zasadzie ograniczenia tokenu, naciśnij klawisz **TOKENU** przycisku.

**Tokenu** zasady ograniczające musi towarzyszyć token wystawiony przez **Secure Token Service** (STS). Usługa Media Services obsługuje tokenów w **proste tokenów sieci Web** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) format i **JSON Web Token** formatu (JWT). Aby uzyskać informacje, zobacz [uwierzytelniania tokenu JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Usługa Media Services dostarcza **Secure Token usługi**. Można utworzyć niestandardowy STS lub korzystać z usługi Microsoft Azure ACS do wydawania tokenów. Usługa tokenu Zabezpieczającego musi być skonfigurowana do utworzenia tokenu podpisany z określonego klucza i problem oświadczenia określony w konfiguracji ograniczenia tokenu. Usługa Media Services klucza dostawy zwróci klucz szyfrowania do klienta, jeśli token jest prawidłowy i oświadczenia w tokenie pasują do klucza zawartości. Aby uzyskać więcej informacji, zobacz [ACS Azure używana do wydawania tokenów](http://mingfeiy.com/acs-with-key-services).

Podczas konfigurowania **TOKENU** zasady ograniczające, należy ustawić wartości dla **klucza weryfikacji**, **wystawcy** i **odbiorców**. Klucz podstawowy weryfikacji zawiera klucz, który został podpisany token, z, wystawca jest bezpieczne usługi tokenu, który wystawia token. Odbiorców (nazywane również zakres) opisano celem tokenu lub zasobu tokenu zezwala na dostęp do. Usługa Media Services klucza dostawy weryfikuje, czy te wartości w tokenie pasują do wartości w szablonie.

### <a name="playready"></a>PlayReady
Podczas ochrony zawartości przy użyciu **PlayReady**, jest jednym z elementów, należy określić w zasadach autoryzacji ciągu XML, który definiuje szablon licencji PlayReady. Domyślnie jest ustawiona następujące zasady:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1"><LicenseTemplates> <PlayReadyLicenseTemplate> <AllowTestDevices>true</AllowTestDevices> <ContentKey i:type="ContentEncryptionKeyFromHeader" /> <LicenseType>Nonpersistent</LicenseType> <PlayRight> <AllowPassingVideoContentToUnknownOutput>dozwolone</AllowPassingVideoContentToUnknownOutput> </PlayRight> </PlayReadyLicenseTemplate> </LicenseTemplates></PlayReadyLicenseResponseTemplate>

Możesz kliknąć **zaimportować pliku xml zasady** przycisk i podaj inny kod XML, który jest zgodny ze schematem XML zdefiniowane [tutaj](media-services-playready-license-template-overview.md).

## <a name="next-step"></a>Następny krok
Przejrzyj ścieżki szkoleniowe dotyczące usługi Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

