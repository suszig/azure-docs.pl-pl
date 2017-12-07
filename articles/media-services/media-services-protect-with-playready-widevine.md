---
title: "Używanie dynamicznego szyfrowania Common Encryption w usługach PlayReady i Widevine | Microsoft Docs"
description: "Usługa Microsoft Azure Media Services umożliwia dostarczanie strumieni MPEG-DASH, Smooth Streaming i HTTP-Live-Streaming (HLS) chronionych przy użyciu usługi Microsoft PlayReady DRM. Możliwe jest również dostarczanie strumienia DASH szyfrowanego przy użyciu usługi Widevine DRM. W tym temacie przedstawiono sposób dynamicznego szyfrowania przy użyciu usług PlayReady i Widevine DRM."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 548d1a12-e2cb-45fe-9307-4ec0320567a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: b30a444150c6ace87e9d506da9335373b5e017af
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="using-playready-andor-widevine-dynamic-common-encryption"></a>Używanie dynamicznego szyfrowania Common Encryption w usługach PlayReady i Widevine

> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-playready-widevine.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>
>

Usługa Microsoft Azure Media Services umożliwia dostarczanie strumieni MPEG-DASH, Smooth Streaming i HTTP-Live-Streaming (HLS) chronionych przy użyciu usługi [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). Umożliwia ona także dostarczanie zaszyfrowanych strumienie DASH korzystających z licencji Widevine DRM. Obie usługi, PlayReady i Widevine, szyfrują dane zgodnie ze specyfikacją Common Encryption (ISO/IEC CENC 23001-7). Aby skorzystać z usługi Widevine, można skonfigurować obiekt AssetDeliveryConfiguration przy użyciu pakietu [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (począwszy od wersji 3.5.1) lub interfejsu API REST.

Usługa Media Services udostępnia usługę dostarczania licencji PlayReady i Widevine DRM. Usługa Media Services dostarcza również interfejsy API, które umożliwiają skonfigurowanie uprawnień i ograniczeń, które powinny być wymuszane w uruchomionym środowisku PlayReady lub Widevine DRM, gdy użytkownik odtwarza chronioną zawartość. Gdy użytkownik zażąda zawartości chronionej przy użyciu DRM, aplikacja odtwarzacza zażąda licencji od usługi licencjonowania AMS. Usługa licencjonowania AMS wystawi licencję dla odtwarzacza, jeśli jest on autoryzowany. Licencja PlayReady lub Widevine zawiera klucz odszyfrowujący, który może zostać użyty przez odtwarzacz klienta do odszyfrowania i strumieniowego przesyłania zawartości.

Licencje Widevine są dostępne również u następujących partnerów usługi AMS: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) i [castLabs](http://castlabs.com/company/partners/azure/). Aby uzyskać więcej informacji, zobacz: integracja z [Axinom](media-services-axinom-integration.md) i [castLabs](media-services-castlabs-integration.md).

Usługa Media Services obsługuje wiele sposobów autoryzacji użytkowników, którzy tworzą żądania klucza. Zasady autoryzacji klucza zawartości mogą mieć jedno lub więcej ograniczeń: ograniczenie otwarte lub ograniczenie tokenu. Zasadzie ograniczenia tokenu musi towarzyszyć token wystawiony przez usługę STS (Secure Token Service). Usługa Media Services obsługuje następujące formaty tokenów: [SWT (Simple Web Token)](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) i [JWT (JSON Web Token)](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3). Aby uzyskać więcej informacji, zobacz Konfigurowanie zasad autoryzacji klucza zawartości.

Aby móc skorzystać z szyfrowania dynamicznego, należy posiadać element zawartości zawierający zestaw plików MP4 o różnych szybkościach transmisji bitów lub pliki źródłowe Smooth Streaming o różnych szybkościach transmisji bitów. Należy również skonfigurować zasady dostarczania tego elementu zawartości (opisane w dalszej części niniejszego tematu). Następnie na podstawie formatu określonego w adresie URL przesyłanego strumienia serwer przesyłania strumieniowego na żądanie upewni się, czy strumień jest dostarczany za pomocą wybranego protokołu. Dzięki temu wystarczy przechowywać i opłacać pliki w jednym formacie magazynu, a usługa Media Services utworzy oraz udostępni właściwą odpowiedź HTTP na podstawie żądań klienta.

Ten temat powinien być przydatny dla deweloperów pracujących nad aplikacjami, które dostarczają zawartość chronioną przy użyciu wielu protokołów DRM, takich jak PlayReady i Widevine. W tym temacie opisano sposób konfigurowania usługi dostarczania licencji PlayReady przy użyciu zasad autoryzacji, tak aby tylko autoryzowani klienci mogli odbierać licencje PlayReady lub Widevine. Przedstawiono także sposób korzystania z szyfrowania przy użyciu usług PlayReady lub Widevine DRM dla strumienia DASH.

>[!NOTE]
>Po utworzeniu konta usługi AMS zostanie do niego dodany **domyślny** punkt końcowy przesyłania strumieniowego mający stan **Zatrzymany**. Aby rozpocząć przesyłanie strumieniowe zawartości oraz korzystać z dynamicznego tworzenia pakietów i szyfrowania dynamicznego, punkt końcowy przesyłania strumieniowego, z którego chcesz strumieniowo przesyłać zawartość, musi mieć stan **Uruchomiony**. 

## <a name="download-sample"></a>Pobieranie przykładu
Opisany w tym artykule przykład możesz pobrać [tutaj](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).

## <a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Konfigurowanie dynamicznego szyfrowania Common Encryption i usług dostarczania licencji DRM

Poniżej przedstawiono ogólne kroki, które należy wykonać podczas ochrony swoich elementów zawartości przy użyciu usługi PlayReady, usługi dostarczania licencji Media Services, a także za pomocą szyfrowania dynamicznego.

1. Utwórz element zawartości i przekaż do niego pliki.
2. Przekoduj element zawartości zawierający plik na zestaw o adaptacyjnej szybkości bitowej MP4.
3. Utwórz klucz zawartości i skojarz go z zakodowanym elementem zawartości. W usłudze Media Services klucz zawartości zawiera klucz szyfrowania elementu zawartości.
4. Skonfiguruj zasady autoryzacji klucza zawartości. Zasady autoryzacji klucza zawartości muszą zostać skonfigurowane przez użytkownika i muszą być spełnione przez klienta, aby klucz zawartości został dostarczony do klienta.

    Podczas tworzenia zasady autoryzacji klucza zawartości należy określić następujące elementy: metodę dostarczania (PlayReady lub Widevine), ograniczenia (otwarte lub tokenu) i informacje specyficzne dla typu klucza dostawy, które definiują sposób dostawy klucza do klienta (szablon licencji [PlayReady](media-services-playready-license-template-overview.md) lub [Widevine](media-services-widevine-license-template-overview.md)).

5. Skonfiguruj zasadę dostarczania elementu zawartości. Konfiguracja zasad dostarczania obejmuje: protokół dostarczenia (na przykład MPEG DASH, HLS, Smooth Streaming lub wszystkie z nich), typ szyfrowania dynamicznego (na przykład Common Encryption) oraz adres URL pozyskiwania licencji PlayReady lub Widevine.

    Dla każdego protokołu dotyczącego danego elementu zawartości można stosować inne zasady. Na przykład dla protokołu Smooth/DASH można zastosować szyfrowanie PlayReady, zaś dla protokołu HLS szyfrowanie AES Envelope. Protokoły, które nie są zdefiniowane w zasadzie dostarczania (można na przykład dodać jedną zasadę, która określa tylko protokół HLS), nie mogą korzystać z przesyłania strumieniowego. Wyjątkiem od tej reguły jest przypadek, w którym nie zdefiniowano żadnej zasady dostarczania elementów zawartości. Wówczas wszystkie protokoły mogą być przesyłane bez zabezpieczeń.

6. Utwórz lokalizator OnDemand w celu pobrania adresu URL przesyłania strumieniowego.

Na końcu tego tematu znajduje się pełny przykład dla środowiska .NET.

Poniższa ilustracja pokazuje opisany wyżej przepływ pracy. Token jest tu używany do uwierzytelniania.

![Ochrona za pomocą PlayReady](media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Pozostała część tego tematu zawiera szczegółowe wyjaśnienia, przykłady kodu i linki do tematów, w których pokazano, jak wykonać zadania opisane powyżej.

## <a name="current-limitations"></a>Bieżące ograniczenia
W przypadku dodania lub zaktualizowania zasad dostarczania elementów zawartości należy usunąć skojarzony lokalizator (jeśli istnieje) i utworzyć nowy.

Ograniczenie w przypadku szyfrowania przy użyciu metody Widevine dla usługi Azure Media Services: obecnie nie jest obsługiwanych wiele kluczy zawartości.

## <a name="create-an-asset-and-upload-files-into-the-asset"></a>Utworzenie elementu zawartości i przekazanie do niego plików
W celu kodowania i przesyłania strumieniowego filmów oraz zarządzania nimi należy najpierw przekazać zawartość do usługi Microsoft Azure Media Services. Po przekazaniu plików ich zawartość jest bezpiecznie przechowywana w chmurze na potrzeby dalszego przetwarzania i przesyłania strumieniowego.

Aby uzyskać szczegółowe informacje, zobacz artykuł [Upload Files into a Media Services account](media-services-dotnet-upload-files.md) (Przekazywanie plików na konto usługi Media Services).

## <a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Przekodowanie elementu zawartości zawierającego plik na zestaw MP4 o adaptacyjnej szybkości bitowej
W przypadku szyfrowania dynamicznego wystarczy utworzyć element zawartości zawierający zestaw plików MP4 o różnych szybkościach transmisji bitów lub pliki źródłowe Smooth Streaming o różnych szybkościach transmisji bitów. Następnie na podstawie formatu określonego w manifeście i żądaniu fragmentu serwer przesyłania strumieniowego na żądanie upewni się, czy strumień jest dostarczany za pomocą wybranego protokołu. Dzięki temu wystarczy przechowywać i opłacać pliki w jednym formacie magazynu, a usługa Media Services utworzy oraz udostępni właściwą odpowiedź na podstawie żądań klienta. Aby uzyskać więcej informacji, zobacz artykuł [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) (Dynamiczne tworzenie pakietów — przegląd).

Aby uzyskać instrukcje na temat kodowania, zobacz artykuł [How to encode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) (Sposób kodowania elementów zawartości przy użyciu standardowego kodera multimediów).

## <a id="create_contentkey"></a>Tworzenie klucza zawartości i kojarzenie go z zakodowanym elementem zawartości
W usłudze Media Services klucz zawartości zawiera klucz, za pomocą którego chcesz zaszyfrować element zawartości.

Aby uzyskać szczegółowe informacje, zobacz artykuł [Create content key](media-services-dotnet-create-contentkey.md) (Tworzenie klucza zawartości).

## <a id="configure_key_auth_policy"></a>Konfigurowanie zasady autoryzacji klucza zawartości
Usługa Media Services obsługuje wiele sposobów uwierzytelniania użytkowników, którzy tworzą żądania klucza. Zasady autoryzacji klucza zawartości muszą zostać skonfigurowane przez użytkownika i muszą być spełnione przez klienta (odtwarzacz), aby klucz zawartości został dostarczony do klienta. Zasady autoryzacji klucza zawartości mogą mieć jedno lub więcej ograniczeń: ograniczenie otwarte lub ograniczenie tokenu.

Aby uzyskać więcej informacji, zobacz artykuł [Configure Content Key Authorization Policy](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption) (Konfigurowanie zasad autoryzacji klucza zawartości).

## <a id="configure_asset_delivery_policy"></a>Konfigurowanie zasad dostarczania elementów zawartości
Skonfiguruj zasady dostarczania dla swojego elementu zawartości. Niektóre elementy objęte konfiguracją zasad dostarczania elementów zawartości:

* Adres URL pozyskiwania licencji DRM.
* Protokół dostarczenia elementów zawartości (na przykład MPEG DASH, HLS, Smooth Streaming lub wszystkie z nich).
* Typ szyfrowania dynamicznego (w tym przypadku jest to Common Encryption).

Aby uzyskać szczegółowe informacje, zobacz artykuł [Configure asset delivery policy](media-services-rest-configure-asset-delivery-policy.md) (Konfigurowanie zasad dostarczania elementów zawartości).

## <a id="create_locator"></a>Tworzenie lokalizatora OnDemand przesyłania strumieniowego w celu pobrania adresu URL przesyłania strumieniowego
W przypadku metod Smooth, DASH lub HLS należy podać użytkownikowi adres URL przesyłania strumieniowego.

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

## <a name="create-and-configure-a-visual-studio-project"></a>Tworzenie i konfigurowanie projektu programu Visual Studio

1. Skonfiguruj środowisko projektowe i wypełnij plik app.config przy użyciu informacji dotyczących połączenia, zgodnie z opisem w sekcji [Projektowanie usługi Media Services na platformie .NET](media-services-dotnet-how-to-use.md). 
2. Dodaj następujące elementy do węzła **appSettings** zdefiniowanego w pliku app.config:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a>Przykład

W poniższym przykładzie pokazano funkcje wprowadzone w programie Azure Media Services SDK dla platformy .NET — wersja 3.5.2 (w szczególności jest wykorzystywana możliwość definiowania szablonu licencji Widevine i żądania licencji Widevine przez usługę Azure Media Services).

Zastąp kod w pliku Program.cs kodem przedstawionym w tej sekcji.

>[!NOTE]
>Limit różnych zasad usługi AMS wynosi 1 000 000 (na przykład zasad lokalizatorów lub ContentKeyAuthorizationPolicy). Należy używać tego samego identyfikatora zasad, jeśli zawsze są używane uprawnienia dotyczące tych samych dni lub tego samego dostępu, na przykład dla lokalizatorów przeznaczonych do długotrwałego stosowania (nieprzekazywanych zasad). Aby uzyskać więcej informacji, zobacz [ten](media-services-dotnet-manage-entities.md#limit-access-policies) temat.

Upewnij się, że zaktualizowano zmienne, tak aby wskazywały foldery, w których znajdują się pliki danych wejściowych.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
    using Newtonsoft.Json;

    namespace DynamicEncryptionWithDRM
    {
        class Program
        {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        private static readonly Uri _sampleAudience =
            new Uri(ConfigurationManager.AppSettings["Audience"]);

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateCommonTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
            Console.WriteLine();

            if (tokenRestriction)
            tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
            AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
            // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
            // back into a TokenRestrictionTemplate class instance.
            TokenRestrictionTemplate tokenTemplate =
                TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

            // Generate a test token based on the the data in the given TokenRestrictionTemplate.
            // Note, you need to pass the key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
            // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
            Console.WriteLine("File does not exist.");
            return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
        {
            var encodingPreset = "Adaptive Streaming";

            IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                        inputAsset.Name,
                        encodingPreset));

            var mediaProcessors =
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();

            var latestMediaProcessor =
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();

            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
            encodeTask.InputAssets.Add(inputAsset);
            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }


        static public IContentKey CreateCommonTypeContentKey(IAsset asset)
        {

            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryption);

            // Associate the key with the asset.
            asset.ContentKeys.Add(key);

            return key;
        }

        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {

            // Create ContentKeyAuthorizationPolicy with Open restrictions
            // and create authorization policy          

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Open",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                    Requirements = null
                }
                };

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with no restrictions").
                Result;


            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
            // Associate the content key authorization policy with the content key.
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString,
                }
                };

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);

            // Associate the content key authorization policy with the content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        static private string GenerateTokenRequirements()
        {
            TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

            template.PrimaryVerificationKey = new SymmetricVerificationKey();
            template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
            template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

            return TokenRestrictionTemplateSerializer.Serialize(template);
        }

        static private string ConfigurePlayReadyLicenseTemplate()
        {
            // The following code configures PlayReady License Template using .NET classes
            // and returns the XML string.

            //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user.
            //It contains a field for a custom data string between the license server and the application
            //(may be useful for custom app logic) as well as a list of one or more license templates.
            PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

            // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
            // to be returned to the end users.
            //It contains the data on the content key in the license and any rights or restrictions to be
            //enforced by the PlayReady DRM runtime when using the content key.
            PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
            //Configure whether the license is persistent (saved in persistent storage on the client)
            //or non-persistent (only held in memory while the player is using the license).  
            licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

            // AllowTestDevices controls whether test devices can use the license or not.  
            // If true, the MinimumSecurityLevel property of the license
            // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
            licenseTemplate.AllowTestDevices = true;

            // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class.
            // It grants the user the ability to playback the content subject to the zero or more restrictions
            // configured in the license and on the PlayRight itself (for playback specific policy).
            // Much of the policy on the PlayRight has to do with output restrictions
            // which control the types of outputs that the content can be played over and
            // any restrictions that must be put in place when using a given output.
            // For example, if the DigitalVideoOnlyContentRestriction is enabled,
            //then the DRM runtime will only allow the video to be displayed over digital outputs
            //(analog video outputs won’t be allowed to pass the content).

            //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience.
            // If the output protections are configured too restrictive,
            // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

            // For example:
            //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

            responseTemplate.LicenseTemplates.Add(licenseTemplate);

            return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
        }

        private static string ConfigureWidevineLicenseTemplate()
        {
            var template = new WidevineMessage
            {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                    new ContentKeySpecs
                    {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                    }
                },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
            };

            string configuration = JsonConvert.SerializeObject(template);
            return configuration;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            // Get the PlayReady license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

            // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
            // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
            // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption
            // to append /? KID =< keyId > to the end of the url when creating the manifest.
            // As a result Widevine license acquisition URL will have KID appended twice,
            // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

            Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
            UriBuilder uriBuilder = new UriBuilder(widevineUrl);
            uriBuilder.Query = String.Empty;
            widevineUrl = uriBuilder.Uri;

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                    {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}

            };

            // In this case we only specify Dash streaming protocol in the delivery policy,
            // All other protocols will be blocked from streaming.
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }

        /// <summary>
        /// Gets the streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference to the streaming manifest file from the  
            // collection of files in the asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator to the streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL to the manifest file.
            return originLocator.Path + assetFile.Name;
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int length)
        {
            var returnValue = new byte[length];

            using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
            {
            rng.GetBytes(returnValue);
            }

            return returnValue;
        }
        }
    }


## <a name="next-steps"></a>Następne kroki
Przejrzyj ścieżki szkoleniowe dotyczące usługi Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zobacz też
[Szyfrowanie CENC przy użyciu technologii Multi-DRM i kontroli dostępu](media-services-cenc-with-multidrm-access-control.md)

[Konfigurowanie tworzenia pakietów Widevine przy użyciu usługi AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Powiadomienie o usługach dostarczania licencji Google Widevine w usłudze Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
