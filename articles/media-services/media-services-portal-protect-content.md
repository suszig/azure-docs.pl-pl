---
title: "Konfigurowanie zasad ochrony zawartości przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule pokazano, jak skonfigurować zasady ochrony zawartości przy użyciu portalu Azure. Artykuł opisuje również sposób włączania szyfrowania dynamicznego trwałych."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: ecc766abb5df38813b3eb6dde98cdc9afd24ac6b
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Konfigurowanie zasad ochrony zawartości przy użyciu portalu Azure
Microsoft Azure Media Services (AMS) umożliwia zabezpieczenie od momentu, gdy opuszczą komputera za pośrednictwem przechowywania, przetwarzania i dostarczania multimediów. Usługa Media Services umożliwia dostarczanie zawartości szyfrowane dynamicznie z Standard AES (Advanced Encryption) (przy użyciu 128-bitowe klucze szyfrowania), szyfrowania common encryption (CENC) za pomocą PlayReady lub Widevine DRM i FairPlay firmy Apple. 

AMS udostępnia usługę dostarczania licencji DRM i AES wyczyść klucze do autoryzowanych klientów. Azure portal umożliwia utworzenie jednego **zasady autoryzacji klucza/licencji** dla wszystkich typów szyfrowania.

W tym artykule pokazano, jak skonfigurować zasady ochrony zawartości z portalu Azure. Artykuł opisuje również sposób stosowania szyfrowania dynamicznego do zasobów.

## <a name="start-configuring-content-protection"></a>Rozpocząć konfigurowanie ochrony zawartości
Aby korzystać z portalu, aby rozpocząć konfigurowanie ochrony zawartości, do konta usługi AMS, globalne wykonaj następujące czynności:
1. W witrynie [Azure Portal](https://portal.azure.com/) wybierz swoje konto usługi Azure Media Services.
2. Wybierz **ustawienia** > **zawartości ochrony**.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Zasady autoryzacji kluczy/licencji
AMS obsługuje wiele sposobów uwierzytelniania użytkowników, którzy tworzą żądania licencji lub klucza. Zasady autoryzacji klucza zawartości należy skonfigurowane przez użytkownika i spełnione przez klienta w kolejności klucz/licencję do można delived do klienta. Zasady autoryzacji klucza zawartości może mieć jeden lub więcej ograniczeń: **Otwórz** lub **tokenu** ograniczeń.

Azure portal umożliwia utworzenie jednego **zasady autoryzacji klucza/licencji** dla wszystkich typów szyfrowania.

### <a name="open-authorization"></a>Otwórz autoryzacji
Otwórz ograniczeń oznacza, że system będzie dostarczać klucza dla każdego, kto wysyła żądanie klucza. To ograniczenie mogą być przydatne do celów testowych. 

### <a name="token-authorization"></a>Token autoryzacji
Zasadzie ograniczenia tokenu musi towarzyszyć token wystawiony przez usługę STS (Secure Token Service). Usługa Media Services obsługuje tokenów w formacie proste tokenów sieci Web (SWT) i format tokenu Web JSON (JWT). Usługi Media Services nie zapewnia bezpieczny tokenu usługi. Można utworzyć niestandardowy STS lub korzystać z usługi Microsoft Azure ACS do wydawania tokenów. Usługa tokenu Zabezpieczającego musi być skonfigurowana do utworzenia tokenu podpisany z określonego klucza i problem oświadczenia określony w konfiguracji ograniczenia tokenu. Usługa Media Services klucza dostawy zwróci żądanego (licencji lub klucza) do klienta, jeśli token jest prawidłowy i oświadczeń z tokenu dopasowania tych skonfigurowana dla klucza (lub licencji).

Podczas konfigurowania token ograniczony zasad, należy określić klucz podstawowy weryfikacji, wystawcy i parametry odbiorców. Klucz podstawowy weryfikacji zawiera klucz, który został podpisany token, z, wystawca jest bezpieczne usługi tokenu, który wystawia token. Odbiorców (nazywane również zakres) opisano celem tokenu lub zasobu tokenu zezwala na dostęp do. Usługa Media Services klucza dostawy weryfikuje, czy te wartości w tokenie pasują do wartości w szablonie.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-license-template"></a>Szablon licencji PlayReady
Szablon licencji PlayReady Ustawia funkcje włączone licencja PlayReady. Aby uzyskać szczegółowe informacje o szablonie licencji PlayReady, zobacz [nośnika — omówienie szablon licencji PlayReady usług](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Inne niż stałe
Jeśli skonfigurujesz licencji jako trwałe go tylko przechowywana w pamięci podczas odtwarzacz używa licencji.  

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Trwała
Jeśli skonfigurujesz licencji jako trwałe jest zapisywany w magazynu trwałego na komputerze klienckim.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-license-template"></a>Szablon licencji Widevine
Szablon licencji Widevine Ustawia funkcje włączone licencji Widevine.

### <a name="basic"></a>Podstawowa
Po wybraniu **podstawowe**, zostanie utworzony szablon z wszystkie wartości domyślne.

### <a name="advanced"></a>Advanced
Aby uzyskać szczegółowe informacje o szablonie prawa Widevine, zobacz [omówienie szablonu licencji Widevine](media-services-widevine-license-template-overview.md).

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>Konfiguracja technologii FairPlay
Aby włączyć szyfrowanie FairPlay, musisz podać aplikacji certyfikat i klucz tajny aplikacji (poproś) za pomocą opcji konfiguracji FairPlay. Aby uzyskać szczegółowe informacje o konfiguracji FairPlay i wymagania, zobacz [to](media-services-protect-hls-with-FairPlay.md) artykułu.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Zastosowanie szyfrowania dynamicznego do zawartości
Aby korzystać z szyfrowania dynamicznego, należy kodowanie pliku źródłowego do zestawu plików MP4 z adaptacyjną szybkością transmisji bitów.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Wybierz zasób, który chcesz zaszyfrować
Aby wyświetlić wszystkich zasobów, wybierz **ustawienia** > **zasoby**.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Szyfrowanie AES lub DRM
Po naciśnięciu **Szyfruj** na zasób, dostępne są dwie opcje: **AES** lub **DRM**. 

#### <a name="aes"></a>AES
Wyczyść klucza szyfrowania zostanie włączona na wszystkich protokołów transmisji strumieniowej AES: Smooth Streaming, HLS i MPEG-DASH.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
Jeśli zostanie wybrany na karcie DRM, dostępne są różne opcje zasady ochrony zawartości (co należy skonfigurować już) + zestaw protokoły przesyłania strumieniowego.

* **PlayReady i Widevine z MPEG-DASH** -będzie dynamicznego szyfrowania strumienia MPEG-DASH PlayReady i Widevine DRMs.
* **PlayReady i Widevine z MPEG-DASH + FairPlay z HLS** -dynamicznie są szyfrowane możesz strumieni MPEG-DASH PlayReady i Widevine DRMs. Również są szyfrowane strumienie HLS z FairPlay.
* **PlayReady tylko w przypadku funkcji Smooth Streaming, HLS i MPEG-DASH** -będzie dynamicznego szyfrowania Smooth Streaming, HLS, strumieni MPEG-DASH z PlayReady DRM.
* **Widevine tylko z MPEG-DASH** -dynamicznego szyfrowania należy MPEG-DASH z Widevine DRM.
* **FairPlay tylko z HLS** -będzie dynamicznego szyfrowania strumienia HLS z FairPlay.

Aby włączyć szyfrowanie FairPlay, musisz podać aplikacji certyfikat i klucz tajny aplikacji (poproś) za pomocą opcji konfiguracji FairPlay bloku ustawienia ochrony zawartości.

![Ochrona zawartości](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Po dokonaniu wyboru szyfrowania, naciśnij klawisz **Zastosuj**.

>[!NOTE] 
>Jeśli planujesz odtwarzania AES szyfrowane HLS w programie Safari, zobacz [szyfrowane HLS w blogu Safari](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Następne kroki
Przejrzyj ścieżki szkoleniowe dotyczące usługi Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

