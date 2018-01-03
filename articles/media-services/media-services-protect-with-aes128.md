---
title: "Za pomocą dynamicznego szyfrowania AES-128 i usługi dostarczania klucza | Dokumentacja firmy Microsoft"
description: "Microsoft Azure Media Services umożliwia dostarczanie zawartości zaszyfrowane za pomocą kluczy szyfrowania AES 128-bitowego. Usługi Media Services udostępnia usługę dostarczenie klucza, która dostarcza klucze szyfrowania do autoryzowanych użytkowników. W tym temacie przedstawiono sposób dynamicznego szyfrowania z AES-128 i korzystania z usługi dostarczania klucza."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: fd90c63baaf254f5086cbc99a2a22d61587ee365
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>Za pomocą dynamicznego szyfrowania AES-128 i usługi dostarczania klucza
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

> [!NOTE]
> Aby uzyskać najnowszą wersję zestawu SDK języka Java i zacząć programować w języku Java, zobacz [Rozpoczynanie korzystania z zestawu SDK klienta Java dla usług Azure Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Aby pobrać najnowszy zestaw SDK języka PHP dla usługi Media Services, poszukaj wersji 0.5.7 pakietu Microsoft/WindowAzure w [repozytorium Packagist](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="overview"></a>Przegląd
> [!NOTE]
> Zobacz to [wpis w blogu](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/) szyfrowanie zawartości z użyciem standardu AES do dostarczenia **Safari na macOS**.
> Zobacz [to](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) wideo omówienie sposobu ochrony zawartości z nośnika z szyfrowania AES.
> 
> 

Microsoft Azure Media Services umożliwia dostarczanie Http-Live-Streaming (HLS) i płynnego przesyłania przesyłane zaszyfrowane z Standard AES (Advanced Encryption) (przy użyciu kluczy szyfrowania 128-bitowe). Usługi Media Services udostępnia usługę dostarczenie klucza, która dostarcza klucze szyfrowania do autoryzowanych użytkowników. Dla usługi Media Services zaszyfrować element zawartości, należy skojarzyć klucz szyfrowania z elementu zawartości, a także skonfigurować zasady autoryzacji klucza. Strumień zleconą przez odtwarzacz usługi Media Services używa określonego klucza do dynamicznego szyfrowania zawartości przy użyciu szyfrowania AES. Aby odszyfrować strumienia, odtwarzacza żądań klucz z usługi dostarczania klucza. Aby zdecydować, czy użytkownik jest autoryzowany do uzyskania klucza, usługa oblicza zasad autoryzacji, które podane dla klucza.

Usługa Media Services obsługuje wiele sposobów uwierzytelniania użytkowników, którzy tworzą żądania klucza. Zasady autoryzacji klucza zawartości mogą mieć jedno lub więcej ograniczeń: ograniczenie otwarte lub ograniczenie tokenu. Zasadzie ograniczenia tokenu musi towarzyszyć token wystawiony przez usługę STS (Secure Token Service). Usługa Media Services obsługuje następujące formaty tokenów: [SWT (Simple Web Token)](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) i [JWT (JSON Web Token)](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3). Aby uzyskać więcej informacji, zobacz [Skonfiguruj zasady autoryzacji klucza zawartości](media-services-protect-with-aes128.md#configure_key_auth_policy).

Aby móc skorzystać z szyfrowania dynamicznego, należy posiadać element zawartości zawierający zestaw plików MP4 o różnych szybkościach transmisji bitów lub pliki źródłowe Smooth Streaming o różnych szybkościach transmisji bitów. Należy również skonfigurować zasady dostarczania elementu zawartości (opisane w dalszej części tego artykułu). Następnie na podstawie formatu określonego w adresie URL przesyłania strumieniowego, serwer przesyłania strumieniowego na żądanie zapewni, że strumień jest dostarczany w protokole wybrana. W związku z tym wystarczy przechowywać i opłacać pliki w jednym formacie magazynu i usługa Media Services, tworzy i służy właściwą odpowiedź na podstawie żądań klienta.

W tym artykule powinien być przydatny dla deweloperów pracujących nad aplikacjami, które dostarczają nośnik zabezpieczony przed zapisem. Artykuł przedstawia sposób konfigurowania usługi dostarczania klucza przy użyciu zasad autoryzacji, aby tylko autoryzowani klienci mogli odbierać kluczy szyfrowania. Widoczny jest również sposób korzystać z dynamicznego szyfrowania.


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Przepływ pracy usługi dostarczania klucza i dynamicznego szyfrowania AES-128

Poniżej przedstawiono ogólne kroki, które należy wykonać w przypadku zasobów z użyciem standardu AES, za pomocą usługi Media Services klucza dostawy, a także za pomocą szyfrowania dynamicznego szyfrowania.

1. [Utworzenie elementu zawartości i przekazywanie plików do niego](media-services-protect-with-aes128.md#create_asset).
2. [Przekodowanie elementu zawartości zawierającego plik o adaptacyjnej szybkości bitowej MP4 zestawu](media-services-protect-with-aes128.md#encode_asset).
3. [Utwórz klucz zawartości i skojarz go z zakodowanym elementem zawartości](media-services-protect-with-aes128.md#create_contentkey). W usłudze Media Services klucz zawartości zawiera klucz szyfrowania elementu zawartości.
4. [Skonfiguruj zasady autoryzacji klucza zawartości](media-services-protect-with-aes128.md#configure_key_auth_policy). Zasady autoryzacji klucza zawartości muszą zostać skonfigurowane przez użytkownika i muszą być spełnione przez klienta, aby klucz zawartości został dostarczony do klienta.
5. [Skonfiguruj zasady dostarczania zasobu](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Konfiguracja zasady dostarczania obejmuje: kluczy adres URL pozyskiwania i wektor inicjowania (IV) (AES 128 wymaga tego samego IV należy podać podczas szyfrowania i odszyfrowywania), protokół dostarczania (na przykład MPEG DASH, HLS, Smooth Streaming lub wszystkie), typ szyfrowania dynamicznego (na przykład, koperty lub nie szyfrowania dynamicznego).

    Dla każdego protokołu dotyczącego danego elementu zawartości można stosować inne zasady. Na przykład dla protokołu Smooth/DASH można zastosować szyfrowanie PlayReady, zaś dla protokołu HLS szyfrowanie AES Envelope. Protokoły, które nie są zdefiniowane w zasadzie dostarczania (można na przykład dodać jedną zasadę, która określa tylko protokół HLS), nie mogą korzystać z przesyłania strumieniowego. Wyjątkiem od tej reguły jest przypadek, w którym nie zdefiniowano żadnej zasady dostarczania elementów zawartości. Następnie wszystkie protokoły są dozwolone w Wyczyść.

6. [Utwórz Lokalizator OnDemand](media-services-protect-with-aes128.md#create_locator) w celu pobrania adresu URL przesyłania strumieniowego.

Artykuł opisuje również [jak aplikacji klienckiej mogą żądać klucza z usługi dostarczania klucza](media-services-protect-with-aes128.md#client_request).

Znajdź pełną .NET [przykład](media-services-protect-with-aes128.md#example) na końcu tego artykułu.

Poniższa ilustracja pokazuje opisany wyżej przepływ pracy. Token jest tu używany do uwierzytelniania.

![Ochrona z zastosowaniem standardu AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Pozostała część tego artykułu zawiera szczegółowe wyjaśnienia, przykłady kodu i linki do tematów, w których pokazano, jak wykonać zadania opisane powyżej.

## <a name="current-limitations"></a>Bieżące ograniczenia
W przypadku dodania lub zaktualizowania zasad dostarczania elementów zawartości należy usunąć skojarzony lokalizator (jeśli istnieje) i utworzyć nowy.

## <a id="create_asset"></a>Utworzenie elementu zawartości i przekazywanie plików do zawartości
W celu kodowania i przesyłania strumieniowego filmów oraz zarządzania nimi należy najpierw przekazać zawartość do usługi Microsoft Azure Media Services. Po przekazaniu plików ich zawartość jest bezpiecznie przechowywana w chmurze na potrzeby dalszego przetwarzania i przesyłania strumieniowego. 

Aby uzyskać szczegółowe informacje, zobacz artykuł [Upload Files into a Media Services account](media-services-dotnet-upload-files.md) (Przekazywanie plików na konto usługi Media Services).

## <a id="encode_asset"></a>Przekodowanie elementu zawartości zawierającego plik o adaptacyjnej szybkości bitowej MP4 zestawu
W przypadku szyfrowania dynamicznego wystarczy utworzyć element zawartości zawierający zestaw plików MP4 o różnych szybkościach transmisji bitów lub pliki źródłowe Smooth Streaming o różnych szybkościach transmisji bitów. Następnie na podstawie formatu określonego w żądaniu manifestu lub fragmentu przesyłania strumieniowego na żądanie serwera zapewnia strumienia w wybranego protokołu. Dzięki temu wystarczy przechowywać i opłacać pliki w jednym formacie magazynu, a usługa Media Services utworzy oraz udostępni właściwą odpowiedź na podstawie żądań klienta. Aby uzyskać więcej informacji, zobacz artykuł [Omówienie dynamicznego pakowania](media-services-dynamic-packaging-overview.md).

>[!NOTE]
>Po utworzeniu konta usługi AMS zostanie do niego dodany **domyślny** punkt końcowy przesyłania strumieniowego mający stan **Zatrzymany**. Aby rozpocząć przesyłanie strumieniowe zawartości oraz korzystać z dynamicznego tworzenia pakietów i szyfrowania dynamicznego, punkt końcowy przesyłania strumieniowego, z którego chcesz strumieniowo przesyłać zawartość, musi mieć stan **Uruchomiony**. 
>
>Ponadto aby można było korzystać z dynamicznego tworzenia pakietów i dynamicznego szyfrowania zawartości musi zawierać zestaw o adaptacyjnej szybkości bitowej MP4s lub pliki Smooth Streaming adaptacyjną szybkością transmisji bitów.

Aby uzyskać instrukcje na temat kodowania, zobacz artykuł [How to encode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) (Sposób kodowania elementów zawartości przy użyciu standardowego kodera multimediów).

## <a id="create_contentkey"></a>Tworzenie klucza zawartości i kojarzenie go z zakodowanym elementem zawartości
W usłudze Media Services klucz zawartości zawiera klucz, za pomocą którego chcesz zaszyfrować element zawartości.

Aby uzyskać szczegółowe informacje, zobacz artykuł [Create content key](media-services-dotnet-create-contentkey.md) (Tworzenie klucza zawartości).

## <a id="configure_key_auth_policy"></a>Konfigurowanie zasady autoryzacji klucza zawartości
Usługa Media Services obsługuje wiele sposobów uwierzytelniania użytkowników, którzy tworzą żądania klucza. Zasady autoryzacji klucza zawartości muszą zostać skonfigurowane przez użytkownika i muszą być spełnione przez klienta (odtwarzacz), aby klucz zawartości został dostarczony do klienta. Zasady autoryzacji klucza zawartości może mieć jeden lub więcej ograniczeń: Otwórz, token ograniczenia lub ograniczenia adresów IP.

Aby uzyskać więcej informacji, zobacz artykuł [Configure Content Key Authorization Policy](media-services-dotnet-configure-content-key-auth-policy.md) (Konfigurowanie zasad autoryzacji klucza zawartości).

## <a id="configure_asset_delivery_policy"></a>Konfigurowanie zasad dostarczania elementów zawartości
Skonfiguruj zasady dostarczania dla swojego elementu zawartości. Niektóre elementy objęte konfiguracją zasad dostarczania elementów zawartości:

* Adres URL pozyskiwania klucza. 
* Inicjowanie wektora (IV) do użycia na potrzeby szyfrowania koperty. AES 128 wymaga tego samego IV należy podać podczas szyfrowania i odszyfrowywania. 
* Protokół dostarczenia elementów zawartości (na przykład MPEG DASH, HLS, Smooth Streaming lub wszystkie z nich).
* Typ szyfrowania dynamicznego (na przykład szyfrowanie AES envelope) lub nie szyfrowania dynamicznego. 

Aby uzyskać szczegółowe informacje, zobacz [Skonfiguruj zasady dostarczania zasobu](media-services-dotnet-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Tworzenie lokalizatora OnDemand przesyłania strumieniowego w celu pobrania adresu URL przesyłania strumieniowego
Należy podać użytkownikowi adres URL przesyłania strumieniowego dla Smooth, DASH lub HLS.

> [!NOTE]
> W przypadku dodania lub zaktualizowania zasad dostarczania elementów zawartości należy usunąć skojarzony lokalizator (jeśli istnieje) i utworzyć nowy.
> 
> 

Aby uzyskać instrukcje dotyczące sposobu publikowania elementów zawartości i tworzenia adresu URL przesyłania strumieniowego, zobacz artykuł [Build a streaming URL](media-services-deliver-streaming-content.md) (Tworzenie adresu URL przesyłania strumieniowego).

## <a name="get-a-test-token"></a>Pobieranie tokenu testowego
Pobierz token testowy, uwzględniając ograniczenia tokenu wybrane w zasadach autoryzacji klucza.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

Do przetestowania strumienia można użyć aplikacji [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

## <a id="client_request"></a>Jak klient Zażądaj klucza z usługi dostarczania klucza
W poprzednim kroku należy konstruować adres URL, który wskazuje plik manifestu. Klient musi wyodrębnić niezbędne informacje z przesyłania strumieniowego pliki manifestu aby można było wysłać żądanie do usługi dostarczania klucza.

### <a name="manifest-files"></a>Pliki manifestu
Klient musi wyodrębnić adres URL (zawierający zawartości (Kącik) Identyfikatora klucza) wartości z pliku manifestu. Klient spróbuje uzyskać klucz szyfrowania z usługi dostarczania klucza. Klient musi wyodrębnić wartość IV i użyj go odszyfrować strumienia. Poniższy fragment kodu przedstawia <Protection> element manifestu Smooth Streaming.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

W przypadku HLS manifest głównego jest dzielony na pliki segmentu. 

Na przykład plik manifestu głównego to: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) zawierający listę nazw plików segmentu.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Po otwarciu pliku segmentu w edytorze tekstu (na przykład http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should contain #EXT-X-klucz, który wskazuje, że plik jest zaszyfrowany.

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
>Jeśli planujesz odtwarzania AES szyfrowane HLS w programie Safari, zobacz [ten blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

### <a name="request-the-key-from-the-key-delivery-service"></a>Żądania z usługi klucza dostawy klucza

Poniższy kod przedstawia sposób wysłania żądania do usługi dostarczania klucza usługi Media Services przy użyciu klucza dostawy identyfikatora Uri (który został wyodrębniony z manifestu) i tokenu (w tym artykule nie opisano jak uzyskać proste tokenów sieci Web z usługi tokenu z bezpiecznego).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }

## <a name="protect-your-content-with-aes-128-using-net"></a>Ochrona zawartości przy użyciu standardu AES-128 przy użyciu platformy .NET

### <a name="create-and-configure-a-visual-studio-project"></a>Tworzenie i konfigurowanie projektu programu Visual Studio

1. Skonfiguruj środowisko projektowe i wypełnij plik app.config przy użyciu informacji dotyczących połączenia, zgodnie z opisem w sekcji [Projektowanie usługi Media Services na platformie .NET](media-services-dotnet-how-to-use.md). 
2. Dodaj następujące elementy do węzła **appSettings** zdefiniowanego w pliku app.config:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <a id="example"></a>Przykład

Zastąp kod w pliku Program.cs kodem przedstawionym w tej sekcji.
 
>[!NOTE]
>Limit różnych zasad usługi AMS wynosi 1 000 000 (na przykład zasad lokalizatorów lub ContentKeyAuthorizationPolicy). Należy używać tego samego identyfikatora zasad, jeśli zawsze są używane uprawnienia dotyczące tych samych dni lub tego samego dostępu, na przykład dla lokalizatorów przeznaczonych do długotrwałego stosowania (nieprzekazywanych zasad). Więcej informacji znajduje się w [tym](media-services-dotnet-manage-entities.md#limit-access-policies) artykule.

Upewnij się, że zaktualizowano zmienne, tak aby wskazywały foldery, w których znajdują się pliki danych wejściowych.

[!code-csharp[Main](../../samples-mediaservices-encryptionaes/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs)]

## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

