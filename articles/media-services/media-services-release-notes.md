---
title: "Informacje o wersji usługi Media Services | Dokumentacja firmy Microsoft"
description: "Informacje o wersji usługi multimediów"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ca2d7af-1cf0-45fa-9585-3b73f3ee057d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 10/18/2017
ms.author: juliako
ms.openlocfilehash: 0da456e13042168f3c8e871f180e6477b73392d5
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="azure-media-services-release-notes"></a>Informacje o wersji usługi Azure Media Services
Te informacje o wersji zawierają podsumowanie zmian z poprzednich wersji i znane problemy.

> [!NOTE]
> Chcemy skontaktuje się z naszym klientom i skoncentrować się na rozwiązywanie problemów, które dotyczy. Aby zgłosić problem lub zadać pytania, Opublikuj w [Forum MSDN usług Media Azure].
> 
> 

## <a id="issues"></a>Obecnie znane problemy
### <a id="general_issues"></a>Ogólne problemy dotyczące usługi Media Services
| Problem | Opis |
| --- | --- |
| Nie podano kilka typowych nagłówków HTTP w interfejsie API REST. |W przypadku tworzenia aplikacji usługi Media Services przy użyciu interfejsu API REST, stwierdzisz, że niektóre typowe pola nagłówka HTTP (w tym CLIENT-REQUEST-ID REQUEST-ID, a ZWRACANY-CLIENT-REQUEST-ID) nie są obsługiwane. Nagłówki zostanie dodana w przyszłej aktualizacji. |
| Kodowanie procent jest niedozwolone. |Usługa Media Services używa wartości właściwości IAssetFile.Name podczas kompilowania adresy URL przesyłania strumieniowego zawartości (na przykład http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tego powodu kodowania procent jest niedozwolone. Wartość **nazwa** właściwość nie może mieć następujące [procent kodowanie zarezerwowanych znaków](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Ponadto może istnieć tylko jeden "." dla rozszerzenia nazwy pliku. |
| Metoda ListBlobs, która jest częścią zestawu SDK usługi Magazyn Azure w wersji 3.x kończy się niepowodzeniem. |Usługa Media Services generuje SAS adresy URL na podstawie [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) wersji. Jeśli chcesz użyć zestawu SDK usługi Magazyn Azure do listy obiektów blob, w kontenerze obiektów blob, użyj [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) metodę, która jest częścią zestawu SDK usługi Magazyn Azure w wersji 2.x. Metoda ListBlobs, który jest częścią wersji zestawu SDK usługi Magazyn Azure 3.x zakończy się niepowodzeniem. |
| Usługa Media Services mechanizm ograniczania ogranicza użycie zasobów dla aplikacji, które należy nadmiernego żądania do usługi. Usługa może zwróciła kod stanu HTTP Usługa niedostępna (503). |Aby uzyskać więcej informacji, zobacz opis kod stanu HTTP 503 w [kodów błędów systemu Azure Media Services](media-services-encoding-error-codes.md) artykułu. |
| Podczas wykonywania zapytania jednostek, istnieje limit 1000 jednostek zwracane w tym samym czasie, ponieważ publiczny v2 REST ogranicza wyniki zapytania do 1000 wyników. |Należy użyć **Pomiń** i **zająć** (.NET) / **górnej** (REST) zgodnie z opisem w [w tym przykładzie .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) i [w tym przykładzie interfejsu API REST](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Niektórzy klienci mogą napotkać problemem powtarzania tag w manifeście Smooth Streaming. |Więcej informacji znajduje się w [tej](media-services-deliver-content-overview.md#known-issues) sekcji. |
| Azure obiekty .NET SDK usługi Media Services nie może być serializowany i w związku z tym nie działają z buforowaniem Azure. |Jeśli spróbujesz się do serializacji obiektu AssetCollection zestawu SDK, aby dodać go do buforowania Azure, jest zwracany wyjątek. |


## <a id="rest_version_history"></a>Historia wersji interfejsu API REST
Informacje o historii wersji interfejsu API REST usług nośnika, zobacz [dokumentacja interfejsu API REST usługi Azure Media Services].

## <a name="october-2017-release"></a>2017 października zlecenia
> [!IMPORTANT] 
> Przypomnienie: Usługa Azure Media Services ma być wycofano obsługę usług ACS klucze uwierzytelniania.  Na 1 czerwca 2018 nie będzie można do uwierzytelniania za pomocą zaplecza usług AMS za pośrednictwem kodu za pomocą kluczy usługi ACS. Zaktualizuj swój kod, aby używać usługi Azure Active Directory (AAD) dla artykułu [usługi Azure Active Directory (Azure AD)-uwierzytelniania opartego na](media-services-use-aad-auth-to-access-ams-api.md). Otrzymasz również ostrzeżenia w portalu Azure tej zmiany.

### <a name="updates-for-october-2017-include"></a>Aktualizacje dla 2017 października obejmują:
#### <a name="sdks"></a>Zestawy SDK
* .NET SDK zaktualizowany do obsługi uwierzytelniania w usłudze AAD.  Usunięto obsługę uwierzytelniania ACS z najnowszego zestawu .NET SDK na Nuget.org wspieranie szybsze migracji do usługi AAD. 
* JAVA SDK zaktualizowany do obsługi uwierzytelniania w usłudze AAD.  Dodaliśmy obsługę uwierzytelniania usługi AAD do naszego zestawu Java SDK. Możesz przeczytać więcej informacji na temat używania zestawu Java SDK przy użyciu usługi AMS w artykule [rozpocząć pracę z klientem programu Java SDK dla usługi Azure Media Services](media-services-java-how-to-use.md)

#### <a name="file-based-encoding"></a>Kodowanie opartą na plikach
1.  Koder Premium można teraz używać do zakodowania zawartości do kodera-dekodera wideo H.265(HEVC). Nie ma żadnego cenową wpływu dotyczące wybierania H.265 za pośrednictwem innych koderów-dekoderów, takich jak H.264. Zapoznaj się z [warunki dotyczące usług Online](https://azure.microsoft.com/support/legal/) ważne uwagi dotyczące HEVC patent licencje.
2.  Jeśli masz źródła wideo, które są zakodowane za pomocą H.265(HEVC) kodera-dekodera wideo, takich jak przechwycić za pomocą iOS11 lub GoPro bohater 6, wideo koder Premium i standardowa — koder można teraz używać do kodowania tych klipów wideo. Zapoznaj się z [warunki dotyczące usług Online](https://azure.microsoft.com/support/legal/) ważne uwagi dotyczące patentowe licencje.
3.  Jeśli masz zawartość, która zawiera wiele ścieżek audio języka, następnie tak długo, jak wartości języka są poprawnie etykietą zgodnie z odpowiedniego specyfikacją formatu pliku (np. MP4 ISO), można standardowego kodera do tej zawartości do kodowania przesyłania strumieniowego. Wynikowe Lokalizator przesyłania strumieniowego spowoduje wyświetlenie listy dostępnych języków audio.
4.  Standardowa — koder obsługuje teraz dwa nowe ustawienia systemu tylko dźwięk, "AAC Audio" i "AAC dobrej jakości Audio". Zarówno odpowiednio utworzyć stereo AAC danych wyjściowych w szybkości transmisji bitów 128 Kb/s i 192 kb/s.
5.  Koder Premium obsługuje teraz formatów plików QuickTime/MOV jako danych wejściowych kodera-dekodera wideo jest jedną z [Apple ProRes odmian wymienione w tym miejscu](https://docs.microsoft.com/azure/media-services/media-services-media-encoder-standard-formats), i jest audio AAC lub PCM.

> [!NOTE]
> Koder Premium nie obsługuje, na przykład DVC/DVCPro wideo opakowane w plikach QuickTime/MOV jako dane wejściowe.  Standardowa — koder obsługuje jednak te kodery-dekodery wideo.
>
>

6.  Poprawki błędów w kodery:
    * Można teraz przesyłania zadań przy użyciu zasobów danych wejściowych i po tych pełną zmodyfikować zasobów (na przykład przez dodawanie/usuwanie/zmiana nazwy plików w ramach elementu zawartości) i przesyłania dodatkowych zadań. 
    * Poprawy jakości produkowane przez koder standardowe miniatur JPEG
    * Ulepszenia standardowa — koder filmy krótkoterminowe. Lepszej obsługi metadanych wejściowych oraz generowanie miniatur w bardzo krótki czas trwania wideo.
    * Ulepszeń dekoder H.264 używane w Standard Encoder, eliminuje niektórych rzadkich artefaktów. 

#### <a name="media-analytics"></a>Analiza multimediów
* GA usługi Azure Media Redactor - tego procesora nośnika (MP) wykona anonymization przez rozmycia kroje wybrane osoby i nadaje się do użytku w publicznych scenariusze bezpieczeństwa i nośnika wiadomości. Omówienie na tego nowego procesora, zobacz wpis w blogu [tutaj](https://azure.microsoft.com/blog/azure-media-redactor/). Szczegółową dokumentację i ustawień, zobacz [redagowanie krojów z analizy multimediów Azure](media-services-face-redaction.md).



## <a name="june-2017-release"></a>2017 czerwca zlecenia

Usługa Media Services obsługuje teraz [usługi Azure Active Directory (Azure AD)-uwierzytelniania opartego na](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> Obecnie usługa Media Services obsługuje model uwierzytelniania usługi kontroli dostępu platformy Azure. Jednak na 1 czerwca 2018 zostaną wycofane autoryzacji kontroli dostępu. Zalecamy jak najszybszą migrację do modelu uwierzytelniania za pomocą usługi Azure AD.

## <a name="march-2017-release"></a>2017 marca zlecenia

Teraz możesz używać usługi Azure Media Standard, aby [automatycznego generowania drabinę szybkości transmisji bitów](media-services-autogen-bitrate-ladder-with-mes.md) przez określenie "Adaptacyjne przesyłanie strumieniowe" ustawienia wstępnego ciągu podczas tworzenia zadania kodowania. "Adaptacyjnego przesyłania strumieniowego" jest zalecane ustawienia domyślne, jeśli chcesz kodować wideo do przesyłania strumieniowego z usługi Media Services. Jeśli musisz dostosować ustawienia wstępnego dla konkretnego scenariusza kodowania, można przejść do [te](media-services-mes-presets-overview.md) ustawienia.

Teraz można używać usługi Azure Media Standard lub Media Encoder Premium przepływu pracy [Tworzenie zadania kodowania, które generuje fragmentów fMP4](media-services-generate-fmp4-chunks.md). 


## <a name="february-2017-release"></a>Lutego 2017 zlecenia

Począwszy od 1 kwietnia 2017 roku, wszystkie rekordy zadań na Twoim koncie, które są starsze niż 90 dni, będą automatycznie usuwane wraz ze skojarzonymi rekordami zadań podrzędnych nawet wtedy, gdy całkowita liczba rekordów jest mniejsza niż maksymalny limit przydziału. Jeśli chcesz zarchiwizować informacje dotyczące zadania lub zadania podrzędnego, możesz użyć kodu opisanego [tutaj](media-services-dotnet-manage-entities.md).

## <a name="january-2017-release"></a>Wersja stycznia 2017 r

W programie Microsoft Azure Media Services (AMS), **punktu końcowego przesyłania strumieniowego** reprezentuje przesyłania strumieniowego usługa, która może dostarczać zawartość bezpośrednio do aplikacji klienckich odtwarzacza lub do Content Delivery Network (CDN) w celu dalszej dystrybucji. Usługa Media Services udostępnia również bezproblemową integrację usługi Azure CDN. Strumienia wychodzącego z usługi StreamingEndpoint może być strumień na żywo, wideo na żądanie lub pobierania progresywnego zawartości na koncie usługi Media Services. Każde konto usługi Azure Media Services zawiera domyślne StreamingEndpoint. Dodatkowe punkty końcowe mogą być tworzone w ramach konta. Istnieją dwie wersje punkty końcowe, 1.0 i 2.0. Począwszy od 10 stycznia 2017 żadnych AMS nowo utworzonego konta zostaną uwzględnione w wersji 2.0 **domyślne** StreamingEndpoint. Dodatkowe punktów końcowych przesyłania strumieniowego dodanych do tego konta będą również w wersji 2.0. Ta zmiana nie ma wpływu na istniejące konta; istniejące punkty końcowe jest w wersji 1.0 i może zostać uaktualnione do wersji 2.0. Ta zmiana będzie zachowanie, rozliczeń i zmiany funkcji (Aby uzyskać więcej informacji, zobacz [to](media-services-streaming-endpoints-overview.md) artykułu).

Ponadto, począwszy od wersji 2.15, usługi Azure Media Services dodane następujące właściwości jednostki przesyłania strumieniowego punktu końcowego: **CdnProvider**, **CdnProfile**, **FreeTrialEndTime** , **StreamingEndpointVersion**. Aby uzyskać szczegółowe informacje o tych właściwości, zobacz [to](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>Wersja grudnia 2016

Usługa Azure Media Services umożliwia teraz uzyskać dostępu do swoich usług danych telemetrycznych/metryki. Bieżąca wersja AMS umożliwia zbieranie danych telemetrycznych dla kanału na żywo, StreamingEndpoint, i na żywo jednostek archiwum. Aby uzyskać więcej informacji, zobacz [to](media-services-telemetry-overview.md) artykułu.

## <a id="july_changes16"></a>Wersja z lipca 2016 r.
### <a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Aktualizacje do pliku manifestu (*. ISM) generowany przez kodowania zadań
Po przesłaniu zadania kodowania Media Encoder Standard lub usługi Azure Media Encoder kodowania zadań generuje [przesyłania strumieniowego pliku manifestu](media-services-deliver-content-overview.md) (* .ism) plików w danych wyjściowych zasobów. Najnowsza wersja usługi została zaktualizowana Składnia tego pliku manifestu przesyłania strumieniowego.

> [!NOTE]
> Składnia przesyłania strumieniowego pliku manifestu (.ism) jest zarezerwowany do użytku wewnętrznego i może ulec zmianie w przyszłych wersjach. Nie wolno modyfikować ani modyfikowania zawartości tego pliku.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Nowy klient manifestu (*. Plik ISMC) jest generowany w danych wyjściowych zasobów, gdy jeden lub więcej plików MP4 wyniki kodowania zadań
Uruchamianie z najnowszej wersji usługi, po zakończeniu zadania kodowania, które generuje go więcej plików MP4, dane wyjściowe zasobów zawiera również przesyłania strumieniowego pliku manifestu (*.ismc) klienta. Plik .ismc pomaga zwiększyć wydajność przesyłania strumieniowego dynamicznych. 

> [!NOTE]
> Składnia pliku manifestu (.ismc) klienta jest zarezerwowany do użytku wewnętrznego i może ulec zmianie w przyszłych wersjach. Nie wolno modyfikować ani modyfikowania zawartości tego pliku.
> 
> 

Aby uzyskać więcej informacji, zobacz [to](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blogu.

### <a name="known-issues"></a>Znane problemy
Niektórzy klienci mogą napotkać problemem powtarzania tag w manifeście Smooth Streaming. Więcej informacji znajduje się w [tej](media-services-deliver-content-overview.md#known-issues) sekcji.

## <a id="apr_changes16"></a>Wersja kwietnia 2016
### <a name="azure-media-analytics"></a>Analiza multimediów Azure
Usługa Azure Media Services wprowadzono analizy multimediów Azure, zaawansowane analizy wideo. Aby uzyskać szczegółowe informacje, zobacz [Omówienie usługi Azure Media Services Analytics](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (wersja zapoznawcza)
Usługa Azure Media Services umożliwia teraz dynamicznego szyfrowania z HTTP Live Streaming (HLS) zawartości z FairPlay firmy Apple. Umożliwia także usługi dostarczania licencji AMS dostarczać licencje FairPlay do klientów. Aby uzyskać szczegółowe informacje Zobacz [użycia usługi Azure Media Services do przesyłania strumieniowego z HLS chronione zawartości z FairPlay firmy Apple.

## <a id="feb_changes16"></a>Wersja lutego 2016
Najnowszą wersję programu Azure Media Services SDK dla platformy .NET (3.5.3) zawiera poprawkę pokrewne Widevine. Problem został: AssetDeliveryPolicy nie można użyć ponownie dla wielu zasobów szyfrowane przy użyciu metody Widevine. W ramach tej poprawki błędów następującą właściwość została dodana do zestawu SDK: **WidevineBaseLicenseAcquisitionUrl**.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>Wersja styczeń 2016
Zastrzeżone jednostki kodowania, zmieniono jego nazwę, aby uniknąć pomyłek z nazwami kodera.

Podstawowa, standardowa i Premium kodowania jednostki zarezerwowanego zostały zmienione na S1, S2, i odpowiednio jednostki, zarezerwowane S3.  Klienci korzystający z podstawowych RUs kodowanie dzisiaj będą widzieli S1 jako etykieta w portalu Azure (i w BOM) podczas Standard i Premium zobaczą etykiety S2 i S3 odpowiednio. 

## <a id="dec_changes_15"></a>Wersja grudnia 2015 r.

### <a name="azure-media-encoder-deprecation-announcement"></a>Azure Media Encoder amortyzacja anonsu

Azure Media Encoder będzie przestarzałe, począwszy od wersji Media Encoder Standard około 12 miesięcy.

### <a name="azure-sdk-for-php"></a>Zestaw Azure SDK dla języka PHP
Nowa wersja opublikowane zespołu zestawu Azure SDK [zestaw Azure SDK for PHP](http://github.com/Azure/azure-sdk-for-php) pakiet, który zawiera aktualizacje i nowe funkcje dla usługi Microsoft Azure Media Services. W szczególności Azure Media Services SDK dla programu PHP obsługuje teraz najnowsze [zawartości ochrony](media-services-content-protection-overview.md) funkcji: dynamicznego szyfrowania AES i DRM (PlayReady i Widevine) z i bez ograniczeń tokenu. Obsługuje także skalowanie [jednostki kodowania](media-services-dotnet-encoding-units.md).

Aby uzyskać więcej informacji, zobacz:

* [Microsoft Azure Media Services SDK dla programu PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) blogu.
* Następujące [przykłady kodu](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) i ułatwiają rozpoczęcie szybko rozpocząć:
  * **vodworkflow_aes.php**: jest to plik PHP, który przedstawia sposób użycia dynamicznego szyfrowania AES-128 i usługi dostarczania klucza. Jest on oparty na przykład .NET wyjaśniono w [to](media-services-protect-with-aes128.md) artykułu.
  * **vodworkflow_aes.php**: jest to plik PHP, który pokazuje, jak korzystać z szyfrowania dynamicznego PlayReady i usługi dostarczania licencji. Jest on oparty na przykład .NET wyjaśniono w [to](media-services-protect-with-playready-widevine.md) artykułu.
  * **scale_encoding_units.php**: jest to plik PHP, pokazujący sposób kodowania zastrzeżone jednostki skali.

## <a id="nov_changes_15"></a>Listopad 2015 r.
Usługa Azure Media Services udostępnia teraz usługi dostarczania licencji Google Widevine w chmurze. Aby uzyskać więcej informacji, zobacz [ten blog anonsu](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Zobacz też [w tym samouczku](media-services-protect-with-playready-widevine.md) i [repozytorium GitHub](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Usług dostarczania licencji Widevine udostępniane przez usługi Azure Media usługami są w wersji zapoznawczej. Aby uzyskać więcej informacji, zobacz [ten blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

## <a id="oct_changes_15"></a>Października 2015 r.
Azure Media Services (AMS) znajduje się teraz na żywo w następujących centrach danych: Brazylia Południowa, Indie Zachodnie, Indie Południowe i Indie środkowe. Można teraz używać portalu Azure, aby [tworzenia kont usługi multimediów](media-services-portal-create-account.md) i wykonywania różnych zadań opisanych [tutaj](https://azure.microsoft.com/documentation/services/media-services/). Jednak w tych centrach danych nie jest obsługiwana funkcja Live Encoding. Ponadto nie wszystkie typy jednostek zarezerwowanych do celów związanych z kodowaniem są dostępne w tych centrach danych.

* Brazylia Południowa: dostępne są wyłącznie podstawowe i standardowe jednostki zarezerwowane do celów związanych z kodowaniem
* Indie Zachodnie, Indie Południowe i Indie środkowe: dostępne są tylko podstawowe kodowanie zastrzeżone jednostki

## <a id="september_changes_15"></a>Września 2015 r.
* AMS oferuje możliwość ochrony zarówno wideo na żądanie (VOD), jak i strumienie na żywo przy użyciu technologii Widevine DRM modułowych. Można użyć następujących partnerów usługi dostarczania ułatwiające licencje Widevine: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Aby uzyskać więcej informacji, zobacz [ten blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).
  
    Aby skorzystać z usługi Widevine, można skonfigurować obiekt AssetDeliveryConfiguration przy użyciu pakietu [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (począwszy od wersji 3.5.1) lub interfejsu API REST.  
* AMS dodano obsługę wideo ProRes firmy Apple. Możesz teraz przekazać pliki wideo źródła QuickTime korzystających z ProRes firmy Apple lub inne kodery-dekodery. Aby uzyskać więcej informacji, zobacz [ten blog](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).
* Teraz można Media Encoder Standard czy wyodrębniania archiwum subclipping i na żywo. Aby uzyskać więcej informacji, zobacz [ten blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).
* Wprowadzono następujące aktualizacje filtrowania: 
  
  * Można teraz używać formatu Apple HTTP Live Streaming (HLS) z tylko dźwięk filtru. Ta aktualizacja umożliwia usunięcie Śledź tylko dźwięk, określając (tylko dźwięk = false) w adresie URL.
  * Podczas definiowania filtry dla zasobów, teraz mieć możliwość połączenia wielu filtrów (3) w jeden adres URL.
    
    Aby uzyskać więcej informacji, zobacz [to](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.
* AMS obsługuje teraz-ramki w HLS w wersji 4. Obsługa ramki optymalizuje operacje szybkie przewijanie do przodu i do tyłu. Domyślnie wszystkie dane wyjściowe v4 HLS zawierają listy odtwarzania ramki (EXT-X-I-FRAME-STREAM-INF).
  
    Aby uzyskać więcej informacji, zobacz [to](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

## <a id="august_changes_15"></a>Sierpnia 2015 r.
* SDK usługi Azure Media Services dla wersji Java V0.8.0 i nowe próbki jest teraz dostępna. Aby uzyskać więcej informacji, zobacz:
  
  * [Wpis w blogu](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
  * [Repozytorium przykłady w języku Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
* Azure Media Player aktualizacji z obsługą wielu audio strumienia. Aby uzyskać więcej informacji, zobacz:
  * [Wpis w blogu](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

## <a id="july_changes_15"></a>Wersja z lipca 2015 r.
* Informuje o dostępności ogólne Media Encoder Standard. Aby uzyskać więcej informacji, zobacz [ten wpis w blogu](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Media Encoder Standard używa ustawienia opisane w [to](http://go.microsoft.com/fwlink/?LinkId=618336) sekcji. Korzystając z ustawienia domyślne dla koduje 4k, należy pobrać **Premium** zastrzeżone typu jednostki. Aby uzyskać więcej informacji, zobacz [sposób kodowania skali](media-services-scale-media-processing-overview.md).
* Podpisy w czasie rzeczywistym na żywo i usługi Azure Media Services Player. Aby uzyskać więcej informacji, zobacz [ten wpis w blogu](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

### <a name="media-services-net-sdk-updates"></a>Aktualizacje zestawu SDK .NET usługi Media Services
Azure SDK .NET usługi Media Services jest teraz wersji 3.4.0.0. W tej wersji dodano następujące funkcje:  

* Obsługa zaimplementowanym archiwum na żywo. Nie można pobrać zawartości, który zawiera archiwum na żywo.
* Obsługa zaimplementowanym filtrów dynamicznych.
* Zaimplementowane funkcje, które umożliwia użytkownikom zachować kontenera magazynu podczas usuwania zasobów.
* Poprawki błędów związanych z ponów zasady w kanałach.
* Włączone **Media Encoder Premium w przepływie pracy**.

## <a id="june_changes_15"></a>Czerwca 2015 r.
### <a name="media-services-net-sdk-updates"></a>Aktualizacje zestawu SDK .NET usługi Media Services
Azure SDK .NET usługi Media Services jest teraz wersji 3.3.0.0. W tej wersji dodano następujące funkcje:  

* Obsługa specyfikacji OpenId Connect odnajdywania
* obsługę przerzucania kluczy po stronie dostawcy tożsamości. 

Jeśli używasz ujawniający OpenID Connect dokumentu odnajdywania dostawcy tożsamości (jak następujących dostawców: Azure Active Directory, Google, Salesforce), można zmodyfikować usługi Azure Media Services można uzyskać klucze podpisywania do sprawdzania poprawności tokenu JWT z OpenID Połącz specyfikacji odnajdywania. 

Aby uzyskać więcej informacji, zobacz [przy użyciu Json klawiszy sieci Web z OpenID Connect specyfikacji odnajdywania do pracy z JWT token uwierzytelniania w usłudze Azure Media Services](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>Maja 2015 r.
Anonsowanie następujące nowe funkcje:

* [Podgląd Live Encoding w usłudze Media Services](media-services-manage-live-encoder-enabled-channels.md)
* [Dynamiczne manifestu](media-services-dynamic-manifest-overview.md)
* [Podgląd procesor multimediów Azure Media Hyperlapse](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

## <a id="april_changes_15"></a>Wersji z kwietnia 2015 r.
### <a name="general-media-services-updates"></a>Usługi multimediów ogólne aktualizacji
* [Anonsowanie Azure Media Player](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
* Począwszy od Media Services REST 2.10, kanałów, które są skonfigurowane do pozyskiwania protokołu RTMP są tworzone z podstawowym, a adresy URL pozyskiwania pomocniczy. Aby uzyskać więcej informacji, zobacz [konfiguracje pozyskiwania kanału](media-services-live-streaming-with-onprem-encoders.md#channel_input)
* Aktualizacje indeksatora multimediów Azure
* Obsługa języka hiszpańskiego
* Nowy format xml konfiguracji

Aby uzyskać więcej informacji, zobacz [ten blog](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

### <a name="media-services-net-sdk-updates"></a>Aktualizacje zestawu SDK .NET usługi Media Services
Azure SDK .NET usługi Media Services jest teraz wersji 3.2.0.0.

Poniżej przedstawiono niektóre aktualizacje klientów:

* **Fundamentalne zmiany**: zmienić **TokenRestrictionTemplate.Issuer** i **TokenRestrictionTemplate.Audience** być typu string.
* Zasady aktualizacji dotyczących tworzenia niestandardowych, następnie spróbuj ponownie.
* Poprawki błędów związanych z przekazywania/pobierania plików.
* **MediaServicesCredentials** klasy akceptuje teraz dostęp do podstawowych i pomocniczych kontrolnego punktu końcowego do uwierzytelniania.

## <a id="march_changes_15"></a>Wersja marca 2015 roku
### <a name="general-media-services-updates"></a>Usługi multimediów ogólne aktualizacji
* Usługa Media Services udostępnia teraz integracji usługi Azure CDN. Do obsługi integracji, **CdnEnabled** właściwość została dodana do **StreamingEndpoint**.  **CdnEnabled** mogą być używane z interfejsów API REST, począwszy od wersji 2.9 (Aby uzyskać więcej informacji, zobacz [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint)).  **CdnEnabled** mogą być używane z zestawu .NET SDK, począwszy od wersji 3.1.0.2 (Aby uzyskać więcej informacji, zobacz [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx)).
* Ogłoszenie **Media Encoder Premium w przepływie pracy**. Aby uzyskać więcej informacji, zobacz [wprowadzenie Premium kodowania w usłudze Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>Lutego 2015 r.
### <a name="general-media-services-updates"></a>Usługi multimediów ogólne aktualizacji
Interfejs API REST usługi multimediów jest teraz wersji 2.9. Począwszy od tej wersji, można włączyć integrację usługi Azure CDN z punkty końcowe przesyłania strumieniowego. Aby uzyskać więcej informacji, zobacz [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>Stycznia 2015 r.
### <a name="general-media-services-updates"></a>Usługi multimediów ogólne aktualizacji
Powiadomienia o ogólne dostępności (GA) ochrony zawartości w przypadku szyfrowania dynamicznego. Aby uzyskać więcej informacji, zobacz [usługi Azure Media Services zwiększa bezpieczeństwo przesyłania strumieniowego z technologią ogólne dostępności DRM](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>Aktualizacje zestawu SDK .NET usługi Media Services
Azure SDK .NET usługi Media Services jest teraz wersji 3.1.0.1.

Ta wersja oznaczony jako domyślny konstruktor Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate jako przestarzałe. Nowy konstruktor ma TokenType jako argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>Wersja z grudnia 2014
### <a name="general-media-services-updates"></a>Usługi multimediów ogólne aktualizacji
* Niektóre aktualizacje i nowe funkcje zostały dodane do procesora Azure Media indeksatora. Aby uzyskać więcej informacji, zobacz [informacje o wersji usługi Azure Media indeksatora wersji 1.1.6.7](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Dodaje nowy interfejs API REST, która pozwala zaktualizować kodowanie jednostki zarezerwowane: [EncodingReservedUnitType z POZOSTAŁĄ](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* Dodano Obsługa mechanizmu CORS usługi dostarczania klucza.
* Ulepszenia wydajności kwerendy opcji zasad autoryzacji zostały wykonane.
* W centrum danych w Chinach [adres URL dostawy klucza](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) jest teraz na klienta (podobnie jak w innych centrach danych).
* Dodano HLS automatycznie docelowego czasu trwania. W trakcie transmisji strumieniowych na żywo, HLS zawsze jest dostarczana dynamicznie. Domyślnie usługi Media Services automatycznie oblicza HLS segmentu współczynnik opakowania (FragmentsPerSegment) na podstawie klatki kluczowej interwału (KeyFrameInterval), nazywany również grupy obrazów — GOP, które zostały odebrane z kodera na żywo. Aby uzyskać więcej informacji, zobacz [Praca z usługi Azure Media Services transmisji strumieniowej na żywo].

### <a name="media-services-net-sdk-updates"></a>Aktualizacje zestawu SDK .NET usługi Media Services
* [Azure SDK .NET usługi Media Services](http://www.nuget.org/packages/windowsazure.mediaservices/) jest teraz wersji 3.1.0.0.
* Zależności zestawu .net SDK uaktualniona do wersji .NET 4.5 Framework.
* Dodaje nowy interfejs API, która pozwala zaktualizować zastrzeżone jednostki kodowania. Aby uzyskać więcej informacji, zobacz [aktualizowanie typ jednostki zarezerwowane i zwiększenie RUs kodowanie przy użyciu platformy .NET](media-services-dotnet-encoding-units.md).
* Dodano JWT (JSON Web Token) obsługę uwierzytelniania tokenu. Aby uzyskać więcej informacji, zobacz [JWT token uwierzytelniania w usłudze Azure Media Services i szyfrowania dynamicznego](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* Dodano względną przesunięć BeginDate i ExpirationDate w szablonie licencji PlayReady.

## <a id="november_changes_14"></a>Listopadzie 2014 roku
* Usługa Media Services umożliwia teraz pozyskiwania zawartości Smooth Streaming (FMP4) na żywo za pośrednictwem połączenia SSL. Pozyskiwanie za pośrednictwem protokołu SSL, upewnij się, że aktualizacja adresu URL pozyskiwania HTTPS.  Obecnie usługa AMS nie obsługuje protokołu SSL z domen niestandardowych.  Aby uzyskać więcej informacji na temat transmisja strumieniowa na żywo, zobacz [Praca z usługi Azure Media Services transmisji strumieniowej na żywo].
* Nie można obecnie pozyskiwania RTMP strumień na żywo, za pośrednictwem połączenia SSL.
* Można tylko strumienia za pośrednictwem protokołu SSL Jeśli punktu końcowego przesyłania strumieniowego, z którego dostarczyć zawartość została utworzona po 10 września 2014 r. Jeśli Twoje adresy URL przesyłania strumieniowego są oparte na punktów końcowych przesyłania strumieniowego utworzone po 10 września, adres URL zawiera "streaming.mediaservices.windows.net" (nowy format). Adresy URL przesyłania strumieniowego zawierające "origin.mediaservices.windows.net" (stary format) nie obsługują protokołu SSL. Jeśli adres URL jest w starym formacie i chcesz mieć możliwość przesyłania strumieniowego za pośrednictwem protokołu SSL, [utworzyć nowy punkt końcowy przesyłania strumieniowego](media-services-portal-manage-streaming-endpoints.md). Użyj adresów URL tworzone w oparciu nowego punktu końcowego przesyłania strumieniowego, przesyłać strumieniowo zawartość przy użyciu protokołu SSL.

## <a id="october_changes_14"></a>Wersji z października 2014 r.
### <a id="new_encoder_release"></a>Koder wersji usługi multimediów
Informuje o nowej wersji nośnika usługi Azure Media Encoder. Z najnowszych kodera multimediów Azure naliczane są tylko opłaty dla output GB, ale w przeciwnym razie nowy koder jest funkcją zgodny z poprzednim kodera. Aby uzyskać więcej informacji [szczegóły cennika usługi Media Services]).

### <a id="oct_sdk"></a>Zestaw .NET SDK usługi multimediów
SDK usługi Media Services dla platformy .NET, rozszerzenia jest teraz wersji 2.0.0.3.

SDK usługi Media Services dla platformy .NET jest teraz wersji 3.0.0.8.

Wprowadzono następujące zmiany:

* Refaktoryzacja klas zasad ponawiania.
* Dodawanie ciąg agenta użytkownika do nagłówków żądań http.
* Dodawanie kroku kompilacji przywracania NuGet.
* Ustalania scenariusza testów do użycia x509 certyfikatu z repozytorium.
* Sprawdzanie poprawności ustawienia po zakończeniu aktualizowania kanału i przesyłania strumieniowego.

### <a name="new-github-repository-to-host-media-services-samples"></a>Nowe repozytorium GitHub do hosta usługi Media Services próbek
Przykłady znajdują się w [repozytorium GitHub przykłady Azure Media Services](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>Września 2014 roku
Metadane Media Services REST jest teraz wersji 2.7. Aby uzyskać więcej informacji dotyczących najnowszych aktualizacji REST, zobacz [dokumentacja interfejsu API REST usługi Azure Media Services].

SDK usługi Media Services dla platformy .NET jest teraz wersji 3.0.0.7

### <a id="sept_14_breaking_changes"></a>Fundamentalne zmiany
* **Pochodzenie** została zmieniona na [StreamingEndpoint].
* Zmiany w domyślnym zachowaniem podczas używania **portalu Azure** do kodowania, a następnie opublikować pliki MP4.

### <a id="sept_14_GA_changes"></a>Nowych funkcji/scenariuszy, które są częścią wersja Ogólnodostępna
* **Procesor multimediów indeksatora**. Aby uzyskać więcej informacji, zobacz [indeksowania plików multimedialnych na pliki z usługi Azure Media indeksatora].
* [StreamingEndpoint] jednostki umożliwia teraz dodać nazwy domen niestandardowych (hosta).
  
    Dla nazwy domeny niestandardowej ma być używany jako nazwa punktu końcowego przesyłania strumieniowego usługi Media Services musisz dodać nazwy hosta niestandardowego do punktu końcowego przesyłania strumieniowego. Użyj interfejsów API REST usługi Media lub zestawu .NET SDK, aby dodać nazwy hosta niestandardowego.
  
    Następujące kwestie:
  
  * Musi mieć własność nazwy domeny niestandardowej.
  * Własność nazwy domeny musi zostać zweryfikowany przez usługi Azure Media Services. Aby zweryfikować domeny, należy utworzyć rekord CName, który mapuje <MediaServicesAccountId> <parent domain> sprawdzenie DNS. < mediaservices-dns strefy >. 
  * Należy utworzyć inny CName, który mapuje nazwę hosta niestandardowego (np: sports.contoso.com) StreamingEndpont usług Media nazwy hosta (np: amstest.streaming.mediaservices.windows.net).

    Aby uzyskać więcej informacji, zobacz **CustomHostNames** właściwości w [StreamingEndpoint] artykułu.

### <a id="sept_14_preview_changes"></a>Nowych funkcji/scenariuszy, które są częścią w publicznej wersji zapoznawczej
* Podgląd transmisji strumieniowej na żywo. Aby uzyskać więcej informacji, zobacz [Praca z usługi Azure Media Services transmisji strumieniowej na żywo].
* Usługa klucza dostawy. Aby uzyskać więcej informacji, zobacz [za pomocą dynamicznego szyfrowania AES-128 i klucz usługi dostarczania].
* Dynamicznego szyfrowania AES. Aby uzyskać więcej informacji, zobacz [za pomocą dynamicznego szyfrowania AES-128 i klucz usługi dostarczania].
* Usługi dostarczania licencji PlayReady. Aby uzyskać więcej informacji, zobacz [za pomocą szyfrowania dynamicznego PlayReady i usługi dostarczania licencji].
* Szyfrowanie PlayReady dynamicznie. Aby uzyskać więcej informacji, zobacz [za pomocą szyfrowania dynamicznego PlayReady i usługi dostarczania licencji].
* Szablon licencji PlayReady usługi multimediów. Aby uzyskać więcej informacji, zobacz [nośnika — omówienie szablon licencji PlayReady usług].
* Przesyłanie strumieniowe magazynu szyfrowane zasoby. Aby uzyskać więcej informacji, zobacz [zawartości szyfrowane przesyłania strumieniowego magazynu].

## <a id="august_changes_14"></a>W wersji 2014 r. sierpnia
Podczas kodowania zasób zawartości wyjściowej jest generowany po zakończeniu zadania kodowania. Do tej wersji usługi Azure Media Services Encoder wyprodukowanych metadanych o zasobach danych wyjściowych. Począwszy od tej wersji kodera również tworzy metadane dotyczące zasobów wejściowych. Aby uzyskać więcej informacji, zobacz [metadanych danych wejściowych] i [metadanych dane wyjściowe] artykułów.

## <a id="july_changes_14"></a>Lipiec 2014 roku
Azure Media Services Pakowarka i modułu szyfrującego wprowadzono następujące poprawki:

* Tylko dźwięk jest odtwarzany po transmultipleksing zasobów archiwum na żywo, aby HTTP transmisji strumieniowej na żywo — ten został rozwiązany i teraz odtwarzania audio i wideo.
* Podczas pakowania zasób HTTP transmisji strumieniowej na żywo i AES 128-bitowego szyfrowania koperty, spakowanych strumieni nie są odtwarzane na urządzeniach z systemem Android — ten problem został rozwiązany i spakowane strumienia odtwarza na urządzeniach z systemem Android, które obsługują HTTP transmisji strumieniowej na żywo.

## <a id="may_changes_14"></a>Maja 2014 roku
### <a id="may_14_changes"></a>Usługi multimediów ogólne aktualizacji
Można teraz używać [dynamicznego tworzenia pakietów] ze strumienia HTTP Live Streaming (HLS) w wersji 3. Do strumienia HLS w wersji 3, Dodaj następujący format źródła ścieżkę lokalizatora: * .ism/manifest(format=m3u8-aapl-v3). Aby uzyskać więcej informacji, zobacz [blogu Nick Drouin].

Dynamiczne tworzenie pakietów, teraz również obsługuje dostarczanie HLS (w wersji 3 i 4) szyfrowany za pomocą PlayReady oparte na Smooth Streaming statycznie zaszyfrowane za pomocą PlayReady. Aby uzyskać informacje na temat szyfrowania, Smooth Streaming z PlayReady, zobacz [ochrona Smooth Stream przy użyciu usług PlayReady].

### <a name="may_14_donnet_changes"></a>Aktualizacje zestawu SDK .NET usługi Media Services
Uwzględniono następujące ulepszenia w wersji SDK .NET usługi Media Services 3.0.0.5:

* Lepsze szybkość i odporności przekazywania/pobierania zasoby nośnika.
* Ulepszenia w obsłudze wyjątków logiki i przejściowy ponownych prób: 
  
  * Błąd przejściowy logikę wykrywania i ponów próbę udoskonalone wyjątków spowodowanych przez zapytanie, trwa zapisywanie zmian, przekazywanie lub pobieranie plików. 
  * Podczas pobierania wyjątki sieci Web (na przykład podczas żądania tokenu usługi ACS), można zauważyć, że błędy krytyczne kończą się niepowodzeniem szybciej teraz.

Aby uzyskać więcej informacji, zobacz [ponów logikę w zestawie SDK usługi multimediów dla platformy .NET].

## <a id="april_changes_14"></a>Wersja kodera kwietnia 2014 r.
### <a name="april_14_enocer_changes"></a>Koder aktualizacje usługi multimediów
* Dodano obsługę pobierania plików AVI utworzone za pomocą edytora rożne EDIUS doliny trawy, gdzie wideo jest lekkim skompresowane za pomocą kodera-dekodera doliny trawy HQ/HQX. Aby uzyskać więcej informacji, zobacz [trawy doliny ogłasza EDIUS 7 przesyłania strumieniowego za pośrednictwem chmury].
* Dodano obsługę określanie konwencji nazewnictwa plików tworzone przez Media Encoder. Aby uzyskać więcej informacji, zobacz [kontrolowanie Media Service kodera nazw plików wyjściowych].
* Dodano obsługę nakładki wideo lub audio. Aby uzyskać więcej informacji, zobacz [tworzenie nakładki].
* Dodano obsługę razem łączenia wielu segmentach wideo. Aby uzyskać więcej informacji, zobacz [łączenia segmentów wideo].
* Stałe błędów związanych z transkodowanie MP4s, w którym został zakodowany audio MPEG-1 Audio warstwy 3 (alias MP3).

## <a id="jan_feb_changes_14"></a>Stycznia/lutego 2014 roku
### <a name="jan_fab_14_donnet_changes"></a>Zestaw .NET SDK 3.0.0.1, 3.0.0.2 i 3.0.0.3 usługi Azure Media Services
Zmiany w 3.0.0.1 i 3.0.0.2 obejmują:

* Rozwiązane problemy związane z użycia zapytań LINQ z deklaracjami OrderBy.
* Podziel rozwiązań testu w [GitHub] do testów opartych na jednostki i oparta na scenariuszu.

Aby uzyskać więcej informacji o zmianach, zobacz: [zwalnia Azure .NET SDK usługi Media Services 3.0.0.1 i 3.0.0.2].

Następujące zmiany wprowadzono w 3.0.0.3:

* Uaktualnić zależności usługi Magazyn Azure w wersji 3.0.3.0. 
* Rozwiązany problem zgodności z poprzednimi wersjami 3.0. *.* wersje. 

## <a id="december_changes_13"></a>Wersja grudniu 2013
### <a name="dec_13_donnet_changes"></a>Zestaw .NET SDK 3.0.0.0 usługi Azure Media Services
> [!NOTE]
> wersje 3.0.x.x nie są zgodne z wersjami 2.4.x.x.
> 
> 

Najnowszą wersję zestawu SDK usługi Media Services jest teraz 3.0.0.0. Możesz pobrać najnowszy pakiet NuGet lub pobrać z usługi bits [GitHub].

Począwszy od zestawu SDK usługi Media wersji 3.0.0.0, można użyć ponownie [Azure Active Directory kontroli dostępu usługi (ACS)] tokenów. Aby uzyskać więcej informacji, zobacz sekcję "Ponowne użycie dostępu kontroli usługi tokenami" w [nawiązywania połączenia z usługi Media Services przy użyciu zestawu SDK usługi multimediów dla platformy .NET] artykułu.

### <a name="dec_13_donnet_ext_changes"></a>2.0.0.0 rozszerzenia zestawu .NET SDK usługi Azure Media Services
Rozszerzenia zestawu SDK .NET w programie Azure Media Services to zestaw metod rozszerzeń i funkcji pomocniczych, które uprościć kod i ułatwiają tworzenie w usłudze Azure Media Services. Możesz uzyskać najnowsze usługi bits z [rozszerzenia SDK .NET usługi Azure Media Services].

## <a id="november_changes_13"></a>Wersja listopad 2013
### <a name="nov_13_donnet_changes"></a>Zmiany zestawu SDK .NET usługi Azure Media Services
Począwszy od tej wersji zestawu SDK usługi multimediów dla platformy .NET obsługuje błędów przejściowych błędów, które mogą wystąpić podczas wykonywania wywołań do warstwy interfejsu API REST usługi Media.

## <a id="august_changes_13"></a>Wersja sierpnia 2013
### <a name="aug_13_powershell_changes"></a>Nośnik: usługi poleceń cmdlet programu PowerShell wchodzi w skład narzędzi zestawu SDK platformy Azure
Następujące polecenia cmdlet programu PowerShell usługi nośnika znajdują się teraz w [azure-sdk-tools].

* Get-AzureMediaServices 
  
    Na przykład `Get-AzureMediaServicesAccount`.
* New-AzureMediaServicesAccount 
  
    Na przykład `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.
* New-AzureMediaServicesKey 
  
    Na przykład `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.
* Remove-AzureMediaServicesAccount 
  
    Na przykład `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

## <a id="june_changes_13"></a>Wersja czerwca 2013
### <a name="june_13_general_changes"></a>Zmiany usługi Azure Media Services
Zmiany wymienione w tej sekcji są aktualizacje zawarte w wersjach czerwca 2013 Media Services.

* Możliwość łączenia wielu kont magazynu do konta usługi Media. 
  
    Konto magazynu
  
    Asset.StorageAccountName i Asset.StorageAccount
* Możliwość Job.Priority aktualizacji. 
* Powiadomienia dotyczące jednostki i właściwości: 
  
    JobNotificationSubscription
  
    NotificationEndPoint
  
    Zadanie
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Zmiany usługi Azure .NET SDK usługi Media Services
Następujące zmiany są uwzględniane w czerwca 2013 zwalnia SDK usługi Media Services. Najnowszy zestaw SDK usługi multimediów jest dostępna w witrynie GitHub.

* Począwszy od wersji 2.3.0.0 obsługuje SDK usługi Media Services łączenia wielu magazynu kont do konta usługi Media Services. Następujące interfejsy API obsługuje tę funkcję:
  
    Typ IStorageAccount.
  
    Właściwość Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts.
  
    Właściwość konto magazynu.
  
    Właściwość StorageAccountName.
  
    Aby uzyskać więcej informacji, zobacz [Zarządzanie zasoby usługi multimediów między wiele kont magazynu].
* Powiadomienie związane z interfejsów API. Począwszy od wersji 2.2.0.0, że masz możliwość nasłuchiwania powiadomień magazynu kolejek Azure. Aby uzyskać więcej informacji zobacz, [powiadomienia zadanie obsługi usług Media].
  
    Właściwość Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions.
  
    Typ Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint.
  
    Typ Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription.
  
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection.
  
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType.
  
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState.
* Zależność od klienta magazynu Azure SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).
* Zależność od OData 5.5 (Microsoft.Data.OData.dll).

## <a id="december_changes_12"></a>Wersja grudnia 2012
### <a name="dec_12_dotnet_changes"></a>Zmiany usługi Azure .NET SDK usługi Media Services
* IntelliSense: Dodaje brakujące dokumentacji Intellisense dla wielu typów.
* Microsoft.Practices.TransientFaultHandling.Core: Rozwiązano problem, gdy zestaw SDK nadal miały zależności do starsza wersja tego zestawu. Zestaw SDK teraz odwołuje się do wersji 5.1.1209.1 tego zestawu.

Rozwiązuje problemy znalezione w listopad 2012 SDK:

* IAsset.Locators.Count: Ta liczba jest teraz prawidłowo raportowane na nowe interfejsy IAsset po usunięciu wszystkich lokalizatorów.
* IAssetFile.ContentFileSize: Ta wartość jest teraz prawidłowo ustawione po przesłaniu przez IAssetFile.Upload (filepath).
* IAssetFile.ContentFileSize: Ta właściwość teraz można ustawić podczas tworzenia pliku zasobów. Było wcześniej tylko do odczytu.
* IAssetFile.Upload (filepath): rozwiązano problem, gdy ta metoda synchroniczna przekazywania została zgłaszanie następujący błąd podczas przekazywania wielu plików zasobów. Wystąpił błąd "serwer nie można uwierzytelnić żądania. Upewnij się, że wartość nagłówka autoryzacji jest sformatowany poprawnie tym podpisu. "
* IAssetFile.UploadAsync: Rozwiązano problem, w którym nie więcej niż pięć plików można przekazać jednocześnie.
* IAssetFile.UploadProgressChanged: To zdarzenie jest teraz udostępniany przez zestaw SDK.
* IAssetFile.DownloadAsync (ciąg, BlobTransferClient, ILocator, CancellationToken): przeciążenie tej metody jest teraz udostępniany.
* IAssetFile.DownloadAsync: Rozwiązano problem, w którym nie więcej niż pięć można pobrać plików jednocześnie.
* IAssetFile.Delete(): Rozwiązano problem, w którym delete wywołujący może zgłoszenia wyjątku, jeśli nie przekazano pliku dla IAssetFile.
* Zadania: Rozwiązano problem gdzie łańcucha "MP4 do sprawnego strumieni zadania" z "Zadania ochrony PlayReady" przy użyciu szablonu zadania nie spowodować wszystkie zadania w ogóle.
* EncryptionUtils.GetCertificateFromStore(): Ta metoda nie jest już zgłasza wyjątek odwołania zerowego z powodu błędu Znajdowanie certyfikatów oparte na problemy z konfiguracją certyfikatu.

## <a id="november_changes_12"></a>Wersja listopad 2012
Zmiany wymienione w tej sekcji zostały aktualizacje zawarte w listopad 2012 (wersja 2.0.0.0) zestawu SDK. Te zmiany mogą wymagać żadnego kodu napisane dla podglądu czerwca 2012 SDK wersji modyfikację lub napisany od nowa.

* Elementy zawartości
  
    IAsset.Create(assetName) jest tylko funkcja tworzenia zasobów. Nie jest już IAsset.Create operacji przekazywania plików jako część wywołania metody. Użyj IAssetFile do przekazywania.
  
    Metoda IAsset.Publish i wartość wyliczenia AssetState.Publish zostały usunięte z usługi SDK. Należy ponownie zapisać kod korzystający z tą wartością.
* FileInfo
  
    Ta klasa została usunięta i zastępuje IAssetFile.
  
    IAssetFiles
  
    IAssetFile zastępuje FileInfo i działa w różny sposób. Aby go użyć, należy utworzyć wystąpienia obiektu IAssetFiles, a następnie przekazywania pliku za pomocą zestawu SDK usługi Media Services albo zestawu SDK usługi Magazyn Azure. Można użyć następujących przeciążeń IAssetFile.Upload:
  
  * IAssetFile.Upload(filePath): Synchroniczna metoda, która blokuje wątku i jest zalecane tylko w przypadku przekazywania jednego pliku.
  * IAssetFile.UploadAsync (filePath, blobTransferClient, lokalizatora, cancellationToken): metody asynchronicznej. Jest to mechanizm przekazywania preferowany. 
    
    Znaną usterką: przy użyciu cancellationToken rzeczywiście spowoduje anulowanie przekazywania; jednak stan anulowania zadań może być dowolną liczbę stanów. Należy poprawnie catch i obsługi wyjątków.
* Lokalizatory
  
    Wersje specyficzne dla źródła zostały usunięte. Kontekst specyficzne dla sygnatury dostępu Współdzielonego. Locators.CreateSasLocator (zasobów, accessPolicy) zostaną oznaczone jako przestarzałe lub usunięty przez po W sekcji lokalizatorów w obszarze nowe funkcje zaktualizowane zachowanie.

## <a id="june_changes_12"></a>Wersja zapoznawcza czerwiec 2012
Następujące funkcje to nowość w wersji listopada zestawu SDK.

* Usuwanie jednostek
  
    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey obiekty są teraz usuwane na poziomie obiektu, to znaczy IObject.Delete() zamiast usuwania w kolekcji, która jest cloudMediaContext.ObjCollection.Delete(objInstance).
* Lokalizatory
  
    Lokalizatory musi zostać utworzone przy użyciu metody CreateLocator i używać wartości wyliczenia LocatorType.SAS lub LocatorType.OnDemandOrigin jako argument dla określonego typu lokalizatora się, że chcesz utworzyć.
  
    Dodano nowe właściwości lokalizatorów, aby ułatwić uzyskiwanie można używać identyfikatorów URI dla zawartości. To one lokalizatorów zostało przeznaczone do zapewniają większą elastyczność dla przyszłych rozszerzeń innych firm i zwiększa łatwość użycia dla aplikacji klienckich.
* Obsługa metod asynchronicznych
  
    Dodano obsługę asynchroniczne do wszystkich metod.

## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Forum MSDN usług Media Azure]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[dokumentacja interfejsu API REST usługi Azure Media Services]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[szczegóły cennika usługi Media Services]: http://azure.microsoft.com/pricing/details/media-services/
[metadanych danych wejściowych]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[metadanych dane wyjściowe]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Delivering Content]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[indeksowania plików multimedialnych na pliki z usługi Azure Media indeksatora]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Praca z usługi Azure Media Services transmisji strumieniowej na żywo]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[za pomocą dynamicznego szyfrowania AES-128 i klucz usługi dostarczania]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[za pomocą szyfrowania dynamicznego PlayReady i usługi dostarczania licencji]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[nośnika — omówienie szablon licencji PlayReady usług]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[zawartości szyfrowane przesyłania strumieniowego magazynu]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://manage.windowsazure.com
[dynamicznego tworzenia pakietów]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[blogu Nick Drouin]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[ochrona Smooth Stream przy użyciu usług PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[ponów logikę w zestawie SDK usługi multimediów dla platformy .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[trawy doliny ogłasza EDIUS 7 przesyłania strumieniowego za pośrednictwem chmury]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[kontrolowanie Media Service kodera nazw plików wyjściowych]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[tworzenie nakładki]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[łączenia segmentów wideo]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[zwalnia Azure .NET SDK usługi Media Services 3.0.0.1 i 3.0.0.2]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure Active Directory kontroli dostępu usługi (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[nawiązywania połączenia z usługi Media Services przy użyciu zestawu SDK usługi multimediów dla platformy .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[rozszerzenia SDK .NET usługi Azure Media Services]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[azure-sdk-tools]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Zarządzanie zasoby usługi multimediów między wiele kont magazynu]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[powiadomienia zadanie obsługi usług Media]: http://msdn.microsoft.com/library/azure/dn261241.aspx

