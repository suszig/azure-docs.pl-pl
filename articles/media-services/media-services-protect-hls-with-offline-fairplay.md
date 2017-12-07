---
title: "Ochrona zawartości z firmy Apple w trybie offline, FairPlay - Azure HLS | Dokumentacja firmy Microsoft"
description: "Ten temat zawiera omówienie i pokazuje, jak używać usługi Azure Media Services do dynamicznego szyfrowania zawartości HTTP Live Streaming (HLS) przy użyciu firmy Apple FairPlay w trybie offline."
services: media-services
keywords: "W trybie Offline, iOS FairPlay przesyłania strumieniowego (kl. / s), HLS, DRM, 10"
documentationcenter: 
author: willzhan
manager: steveng
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: willzhan, dwgeo
ms.openlocfilehash: bf5828ecd6b6bd2e862c4d7709014ecac47c6be0
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="offline-fairplay-streaming"></a>W trybie offline FairPlay przesyłania strumieniowego
Microsoft Azure Media Services zawiera zestaw dobrze zaprojektowanego [zawartości usługi ochrony](https://azure.microsoft.com/services/media-services/content-protection/), pokrycie:
- Microsoft PlayReady
- Google Widevine
- FairPlay firmy Apple
- Szyfrowanie AES-128

Szyfrowanie DRM/AES zawartości jest wykonywane dynamicznie na żądanie dla różnych protokołów przesyłania strumieniowego. Podawane są również DRM licencji/AES odszyfrowywania klucza dostawy usług przez usługi Azure Media Services.

Oprócz ochrony zawartości do przesyłania strumieniowego online za pośrednictwem różnych protokołów przesyłania strumieniowego, trybu offline dla zawartości chronionej jest również często żądanej funkcji. Potrzebna jest obsługa trybu offline w następujących scenariuszach:
1. Odtwarzanie podczas połączenia internetowego jest niedostępny, takich jak podczas podróży;
2. Niektórzy dostawcy zawartości może nie zezwalaj na dostarczania licencji DRM poza obramowania kraju. Jeśli użytkownik chce obejrzeć zawartość podczas podróży granicą, pobierania w trybie offline są wymagane.
3. W niektórych krajach dostępności Internet i/lub przepustowości jest nadal ograniczona. Użytkownicy mogą wybrać najpierw Pobierz mogli oglądać zawartość wystarczająco wysokiej rozdzielczości do wyświetlania zadowalające. W takim przypadku częściej, problem nie ma dostępności sieci, a nie jest ograniczona przepustowość sieci. Dostawców OTT/OVP prośbę o pomocy technicznej w trybie offline.

W tym artykule omówiono przeznaczonych dla urządzeń z systemem iOS, 10 lub nowszej w trybie offline FairPlay przesyłania strumieniowego (kl. / s) w pomocy technicznej. Ta funkcja nie jest obsługiwana dla innych platform firmy Apple, takie jak watchOS, systemu tvOS lub Safari na macOS.

## <a name="preliminary-steps"></a>Czynności wstępne
Przed wdrożeniem w trybie offline DRM dla FairPlay na urządzeniu z systemem iOS 10 +, należy najpierw:
1. Zapoznanie się z ochrony zawartości w trybie online dla FairPlay. To jest omówiona szczegółowo w artykułach/próbek:
- [Apple FairPlay przesyłania strumieniowego dla usługi Azure Media Services jest ogólnie dostępna](https://azure.microsoft.com/blog/apple-FairPlay-streaming-for-azure-media-services-generally-available/)
- [Ochrona programu HLS zawartości z FairPlay firmy Apple lub Microsoft PlayReady](https://docs.microsoft.com/azure/media-services/media-services-protect-hls-with-FairPlay)
- [Przykładowe przesyłania strumieniowego online kl. / s](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-FairPlay/)
2. Uzyskać firmy Apple Developer Network SDK klatek na Sekundę. Klatek na Sekundę SDK zawiera dwa składniki:
- Kl. / s serwera SDK, która zawiera KSM (klucz modułu zabezpieczeń), przykłady klienta specyfikację i zestaw testów wektorów;
- Pakietu wdrażania kl. / s, który zawiera funkcję D specyfikacji wraz z instrukcjami dotyczącymi sposobu generowania certyfikatu klatek na Sekundę, klucza prywatnego właściwe dla klienta i klucz tajny aplikacji (poproś). Apple wystawia pakietu wdrażania klatek na Sekundę tylko licencjonowane dostawców zawartości.

## <a name="configuration-in-azure-media-services"></a>Konfiguracja w usłudze Azure Media Services
Klatek na Sekundę w trybie offline konfiguracji za pomocą [zestawu .NET SDK usługi Azure Media Services](https://www.nuget.org/packages/windowsazure.mediaservices), należy użyć zestawu .NET SDK usługi Azure Media Services v. 4.0.0.4 lub nowszym, które dostarczane niezbędne interfejsu API, aby skonfigurować tryb offline kl. / s.
Jak wskazano w powyższej założeń, przyjęto założenie, że masz kod pracy konfigurowania ochrony zawartości klatek na Sekundę w trybie online. Po utworzeniu kodu do konfigurowania ochrony zawartości w trybie online na kl. / s, wystarczy tylko następujące dwie zmiany.

## <a name="code-change-in-fairplay-configuration"></a>Zmiana kodu w konfiguracji FairPlay
Umożliwia zdefiniowanie "Włącz tryb offline" boolean, o nazwie objDRMSettings.EnableOfflineMode o wartości PRAWDA podczas włączania scenariusz DRM w trybie offline. W zależności od tego wskaźnika możemy wprowadź następujące zmiany w konfiguracji FairPlay:

```csharp
if (objDRMSettings.EnableOfflineMode)
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv, 
            RentalAndLeaseKeyType.PersistentUnlimited,
            0x9999);
    }
    else
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv);
    }
```

## <a name="code-change-in-asset-delivery-policy-configuration"></a>Zmiana kodu w konfiguracji zasady dostarczania zasobów
Druga zmiana polega na dodaniu trzeciego klucza do słownika słownika < AssetDeliveryPolicyConfigurationKey ciągu >.
Jest trzeci musi AssetDeliveryPolicyConfigurationKey można dodać jak poniżej: 
```csharp
// FPS offline mode
    if (drmSettings.EnableOfflineMode)
    {
        objDictionary_AssetDeliveryPolicyConfigurationKey.Add(AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense, "true");
        Console.WriteLine("FPS OFFLINE MODE: AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense added into asset delivery policy configuration.");
    }

    // for IAssetDelivery for FPS
    IAssetDeliveryPolicy objIAssetDeliveryPolicy = objCloudMediaContext.AssetDeliveryPolicies.Create(
            drmSettings.AssetDeliveryPolicyName,
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            objDictionary_AssetDeliveryPolicyConfigurationKey);
```

Po wykonaniu tego kroku słownika < ciąg AssetDeliveryPolicyConfigurationKey > zasady dostarczania elementu zawartości klatek na Sekundę będzie zawierać trzy następujące wpisy:
1. AssetDeliveryPolicyConfigurationKey.FairPlayBaseLicenseAcquisitionUrl lub AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl w zależności od czynników, takich jak serwer KSM klatek na Sekundę/klucz używany i czy chcemy, aby ponownie użyć tego samego dostarczania zasobów Zasady przez wiele zasobów
2. AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs
3. AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense

Teraz Twojego konta usługi media services jest skonfigurowany do dostarczyć licencje FairPlay w trybie offline.

## <a name="sample-ios-player"></a>Przykładowe iOS Player
Należy najpierw należy zanotować czy klatek na Sekundę w trybie offline obsługa jest dostępna tylko w systemie iOS 10 i nowszych. Możemy pobrać zestawu SDK serwera kl. / s (w wersji 3.0 lub nowszej) zawierający dokumentu i próbki dla klatek na Sekundę w trybie offline. W szczególności SDK serwera kl. / s (w wersji 3.0 lub nowszej) zawiera dwa następujące elementy związane z trybu offline:
1. Dokument: Odtwarzanie w trybie Offline z FairPlay Streaming i HTTP Live przesyłania strumieniowego. Apple, 2016-9-14. W kl. / s Server SDK v 4.0 tego dokumentu została scalona głównego dokumentu przesyłania strumieniowego klatek na Sekundę.
2. Przykładowy kod: próbka HLSCatalog do trybu offline kl. / s w \FairPlay v3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\ przesyłania strumieniowego Server SDK. W przykładowej aplikacji HLSCatalog następujące pliki kodu są szczególnie w przypadku implementowania funkcji w trybie offline:
- Plik kodu AssetPersistenceManager.swift: AssetPersistenceManager jest główna klasa w tym przykładzie, który demonstruje
    - Jak zarządzać pobieranie strumienie HLS, takich jak interfejsów API uruchamianie i anulowanie pobierania, usuwania istniejących zasobów poza urządzeń użytkownika.
    - Jak monitorować postęp pobierania.
- AssetListTableViewController.swift i AssetListTableViewCell.swift kodu plików: AssetListTableViewController jest głównym interfejsu tego przykładu. Zapewnia listę zasobów próbki można odtworzyć, Pobierz, usuń lub anulować pobieranie. 

Poniżej przedstawiono szczegółowe informacje na temat konfigurowania uruchomionych player systemu iOS. Załóżmy, że w przypadku uruchomienia próbki HLSCatalog v kl. / s Server SDK 4.0.1.  Należy wprowadzić następujące zmiany kodu:

W HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, zaimplementuj metodę `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` przy użyciu następującego kodu: Let drmUr być zmienną przypisane do HLS URL przesyłania strumieniowego.

```swift
    var ckcData: Data? = nil
    
    let semaphore = DispatchSemaphore(value: 0)
    let postString = "spc=\(spcData.base64EncodedString())&assetId=\(assetIDString)"
    
    if let postData = postString.data(using: .ascii, allowLossyConversion: true), let drmServerUrl = URL(string: self.drmUrl) {
        var request = URLRequest(url: drmServerUrl)
        request.httpMethod = "POST"
        request.setValue(String(postData.count), forHTTPHeaderField: "Content-Length")
        request.setValue("application/x-www-form-urlencoded", forHTTPHeaderField: "Content-Type")
        request.httpBody = postData
        
        URLSession.shared.dataTask(with: request) { (data, _, error) in
            if let data = data, var responseString = String(data: data, encoding: .utf8) {
                responseString = responseString.replacingOccurrences(of: "<ckc>", with: "").replacingOccurrences(of: "</ckc>", with: "")
                ckcData = Data(base64Encoded: responseString)
            } else {
                print("Error encountered while fetching FairPlay license for URL: \(self.drmUrl), \(error?.localizedDescription ?? "Unknown error")")
            }
            
            semaphore.signal()
            }.resume()
    } else {
        fatalError("Invalid post data")
    }
    
    semaphore.wait()
    return ckcData
```

W HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, zaimplementuj metodę `requestApplicationCertificate()`. Ta implementacja zależy od tego, czy osadzić certyfikatu (tylko klucz publiczny) z urządzenia lub hosta certyfikatu w sieci web. Poniżej znajdują się przy użyciu certyfikatu hostowanej aplikacji używane w naszych próbki implementację. Let certUrl być zmienną zawierający adres URL aplikacji certyfikatu.

```swift
func requestApplicationCertificate() throws -> Data {

        var applicationCertificate: Data? = nil
        do {
            applicationCertificate = try Data(contentsOf: URL(string: certUrl)!)
        } catch {
            print("Error loading FairPlay application certificate: \(error)")
        }
        
        return applicationCertificate
    }
```

Ostateczny test zintegrowanego zarówno wideo adresu URL i adresu URL certyfikatu aplikacji będzie należy podać w sekcji "Test zintegrowane".

W HLSCatalog\Shared\Resources\Streams.plist, Dodaj adres URL wideo testu, a dla klucza zawartości identyfikator, możemy użyć FairPlay adres URL pozyskiwania licencji z protokołem skd jako unikatowych wartości.

![W trybie offline iOS FairPlay strumieni aplikacji](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-ios-app-streams.png)

Wideo testowego adresu URL, adres URL pozyskiwania licencji FairPlay i adres URL certyfikatu aplikacji można użyć własnych, należy je skonfigurować, czy kontynuowaniem do następnej sekcji, która zawiera próbki.

## <a name="integrated-test"></a>Zintegrowane testu
Trzy próbki są skonfigurowane w usłudze Azure Media Services obejmuje następujące trzy scenariusze:
1.  Chronione, wideo, audio i alternatywnej ścieżki audio; kl. / s
2.  Chronione, wideo, audio, ale nie alternatywną ścieżkę audio; kl. / s
3.  Klatek na Sekundę chronione przy użyciu wideo, audio nie.

Te przykłady znajdują się w tej [lokacji pokaz](http://aka.ms/poc#22), przy użyciu odpowiedniego certyfikatu aplikacji hostowanej w aplikacji sieci web platformy Azure.
Firma Microsoft zauważyć, że w wersji 3 lub 4 próbki SDK serwera kl. / s, jeśli główny listy odtwarzania zawiera alternatywny audio w trybie offline, odtwarzania audio tylko. W związku z tym musimy paska alternatywny audio. Innymi słowy spośród trzech próbki powyżej, (2) i (3) działają w trybie zarówno w trybie online, jak i w trybie offline. Ale (1) będzie Odtwórz dźwięk tylko w trybie offline podczas online działa przesyłania strumieniowego, prawidłowo.

## <a name="faq"></a>Często zadawane pytania
Niektóre często zadawane pytania dotyczące rozwiązywania problemów:
- **Dlaczego play tylko audio, ale brak obrazu w trybie offline?** To zachowanie jest prawdopodobnie zgodnie z założeniami przykładowej aplikacji. Po alternatywną ścieżkę audio prezentować (czyli w przypadku HLS), w trybie offline, zarówno dla systemu iOS 10 i 11 systemu iOS zostanie domyślnie alternatywną ścieżkę audio. Kompensacji to zachowanie klatek na Sekundę w trybie offline, należy usunąć alternatywną ścieżkę audio ze strumienia. Aby to zrobić na stronie usługi Azure Media Services, możemy po prostu Dodaj Filtr dynamiczny manifestu "tylko dźwięk = false." Innymi słowy adres URL HLS czy kończy się .ism/manifest(format=m3u8-aapl,audio-only=false). 
- **Dlaczego jest on nadal odtwarzany audio tylko bez wideo w trybie offline po dodaniu tylko dźwięk = false?** W zależności od CDN klucza pamięci podręcznej zawartość może być buforowane. Należy przeczyścić pamięci podręcznej.
- **Jest w trybie offline kl. / s również obsługiwane w systemie iOS 11 oprócz iOS 10?** Tak, klatek na Sekundę w trybie offline jest obsługiwane dla systemu iOS 10 i 11 systemu iOS.
- **Dlaczego nie można znaleźć dokumentu w trybie Offline odtwarzania z FairPlay Streaming i HTTP transmisji strumieniowej na żywo w zestawie SDK serwera kl. / s?** Od zestawu SDK serwera klatek na Sekundę v 4 tego dokumentu została scalona przewodnik programowania w języku przesyłania strumieniowego FairPlay dokumentu.
- **Co to ostatni parametr oznaczać w następujący interfejs API na kl. / s w trybie offline?**
`Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(objX509Certificate2, pfxPassword, pfxPasswordId, askId, iv, RentalAndLeaseKeyType.PersistentUnlimited, 0x9999);`

Można znaleźć w dokumentacji tego interfejsu API [tutaj](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.mediaservices.client.FairPlay.FairPlayconfiguration.createserializedFairPlayoptionconfiguration?view=azure-dotnet). Parametr reprezentuje czas trwania dzierżawy w trybie offline o godzinę jako jednostki.
- **Co to jest struktura pliku pobrane offline na urządzeniach z systemem iOS?** Struktura pobrany plik na urządzeniu z systemem iOS wygląda jak poniżej (zrzut ekranu). `_keys`magazynów folderów pobrać licencji na kl. / s, jeden magazyn plików dla każdego hosta usługi licencji. `.movpkg`folder przechowuje zawartości audio i wideo. Pierwszy folder o nazwie z łącznikiem następuje liczbową zawartość wideo. Wartość liczbowa jest "PeakBandwidth" wideo wersji. Drugi folder o nazwie z łącznikiem następuje 0 zawartość audio. Trzeci folder o nazwie "Dane" zawiera wzorca listy odtwarzania zawartości klatek na Sekundę. Pełny opis zawiera Boot.XML `.movpkg` zawartość folderu (poniżej podano przykładowy plik boot.xml).

![W trybie offline iOS FairPlay struktury plików przykładowej aplikacji](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-file-structure.png)

Przykładowy plik boot.xml:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<HLSMoviePackage xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://apple.com/IMG/Schemas/HLSMoviePackage" xsi:schemaLocation="http://apple.com/IMG/Schemas/HLSMoviePackage /System/Library/Schemas/HLSMoviePackage.xsd">
  <Version>1.0</Version>
  <HLSMoviePackageType>PersistedStore</HLSMoviePackageType>
  <Streams>
    <Stream ID="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" Path="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(127000)/Manifest(aac_eng_2_127,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
    <Stream ID="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" Path="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(161000)/Manifest(video,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
  </Streams>
  <MasterPlaylist>
    <NetworkURL>https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/manifest(format=m3u8-aapl,audio-only=false)</NetworkURL>
  </MasterPlaylist>
  <DataItems Directory="Data">
    <DataItem>
      <ID>CB50F631-8227-477A-BCEC-365BBF12BCC0</ID>
      <Category>Playlist</Category>
      <Name>master.m3u8</Name>
      <DataPath>Playlist-master.m3u8-CB50F631-8227-477A-BCEC-365BBF12BCC0.data</DataPath>
      <Role>Master</Role>
    </DataItem>
  </DataItems>
</HLSMoviePackage>
```

## <a name="summary"></a>Podsumowanie
W tym dokumencie, firma Microsoft umieściła szczegółowe instrukcje i informacje o implementacji trybu offline kl. / s, w tym:
1. Konfiguracja ochrony zawartości Azure Media Services za pośrednictwem interfejsu API usług AMS .NET. Spowoduje to skonfigurowanie szyfrowania dynamicznego FairPlay i dostarczania licencji FairPlay w AMS.
2. iOS player na podstawie próbki z zestawu SDK serwera kl. / s firmy Apple. To należy skonfigurować player systemu iOS, które można odtwarzać klatek na Sekundę zawartości w trybie przesyłania strumieniowego online lub w trybie offline.
3. Przykładowe wideo kl. / s do testowania trybu offline i online przesyłania strumieniowego.
4. Często zadawane pytania dotyczące klatek na Sekundę w trybie offline.
