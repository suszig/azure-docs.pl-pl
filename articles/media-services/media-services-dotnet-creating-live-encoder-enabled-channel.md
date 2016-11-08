---
title: Korzystanie z usługi Azure Media Services do prowadzenia transmisji strumieniowych na żywo ze strumieniami o różnych szybkościach transmisji bitów | Microsoft Docs
description: Ten samouczek przedstawia kroki tworzenia kanału, który odbiera strumień na żywo o pojedynczej szybkości transmisji bitów i koduje go jako strumień o wielokrotnej szybkości transmisji bitów przy użyciu zestawu SDK programu .NET.
services: media-services
documentationcenter: ''
author: anilmur
manager: erikre
editor: ''

ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/12/2016
ms.author: juliako;anilmur

---
# <a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-.net"></a>Transmisja strumieniowa na żywo korzystająca z usługi Azure Media Services do tworzenia strumieni o wielokrotnej szybkości transmisji bitów z użyciem programu .NET
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
> * [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
> * [Interfejs API REST](https://msdn.microsoft.com/library/azure/dn783458.aspx)
> 
> [!NOTE]
> Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> 

## <a name="overview"></a>Omówienie
Ten samouczek przedstawia kroki tworzenia **kanału**, który odbiera strumień na żywo o pojedynczej szybkości transmisji bitów i koduje go jako strumień o wielokrotnej szybkości transmisji bitów.

Aby uzyskać więcej informacji o pojęciach związanych z kanałami obsługującymi kodowanie na żywo, zobacz temat [Korzystanie z usługi Azure Media Services do prowadzenia transmisji strumieniowych na żywo ze strumieniami o wielokrotnej szybkości transmisji bitów](media-services-manage-live-encoder-enabled-channels.md).

## <a name="common-live-streaming-scenario"></a>Typowy scenariusz transmisji strumieniowej na żywo
W poniższych krokach opisano zadania związane z tworzeniem typowych aplikacji transmisji strumieniowej na żywo.

> [!NOTE]
> Obecnie maksymalny zalecany czas trwania wydarzenia na żywo wynosi 8 godzin. Skontaktuj się pod adresem amslived@microsoft.com, jeśli chcesz uruchomić kanał na dłuższe okresy.
> 
> 

1. Podłącz kamerę wideo do komputera. Uruchom i skonfiguruj lokalny koder na żywo, który wysyła strumień o pojedynczej szybkości transmisji bitów przy użyciu jednego z następujących protokołów: RTMP, Smooth Streaming lub RTP (MPEG-TS). Aby uzyskać więcej informacji, zobacz temat [Obsługa protokołu RTMP i kodery na żywo w usłudze Azure Media Services](http://go.microsoft.com/fwlink/?LinkId=532824).

Ten krok można również wykonać po utworzeniu kanału.

1. Utwórz i uruchom kanał.
2. Pobierz adres URL pozyskiwania kanału.

Koder na żywo używa adresu URL pozyskiwania do wysyłania strumienia do kanału.

1. Pobierz adres URL podglądu kanału.

Użyj tego adresu URL, aby sprawdzić, czy kanał prawidłowo odbiera strumień na żywo.

1. Utwórz zasób.
2. Aby zasób był dynamicznie szyfrowany podczas odtwarzania , należy wykonać następujące czynności:
3. Utwórz klucz zawartości.
4. Skonfiguruj zasady autoryzacji klucza zawartości.
5. Skonfiguruj zasady dostarczania zasobu (stosowane podczas pakowania dynamicznego i szyfrowania dynamicznego).
6. Utwórz program i określ użycie utworzonego zasobu.
7. Opublikuj zasób skojarzony z programem przez utworzenie lokalizatora OnDemand.

Upewnij się, że istnieje co najmniej jedna jednostka zarezerwowanego przesyłania strumieniowego w punkcie końcowym, z którego zawartość ma być przesyłana strumieniowo.

1. Uruchom program, gdy wszystko będzie gotowe do rozpoczęcia przesyłania strumieniowego i archiwizacji.
2. Opcjonalnie można przesłać do kodera na żywo sygnał o rozpoczęciu reklamy. Reklama jest wstawiana do strumienia wyjściowego.
3. Zatrzymaj program w dowolnym momencie, w którym chcesz zatrzymać przesyłanie strumieniowe i archiwizowanie wydarzenia.
4. Usuń program (opcjonalnie można również usunąć zasób).

## <a name="what-you'll-learn"></a>Zawartość
W tym temacie opisano sposób wykonywania różnych operacji na kanałach i programach przy użyciu zestawu SDK .NET usługi Media Services. Ponieważ czas trwania wielu operacji jest długi, użyto interfejsów API platformy .NET służących do zarządzania operacjami długotrwałymi.

W temacie przedstawiono sposób wykonywania następujących czynności:

1. Tworzenie i uruchamianie kanału. Używane są interfejsy API do operacji długotrwałych.
2. Pobieranie punktu końcowego odbioru (wejścia) kanału. Ten punkt końcowy należy przekazać do kodera, który może wysyłać strumień na żywo o pojedynczej szybkości transmisji bitów.
3. Pobieranie punktu końcowego podglądu. Ten punkt końcowy jest używany do podglądu strumienia.
4. Tworzenie zasobu, który będzie używany do przechowywania zawartości. Należy również skonfigurować zasady dostarczania zasobów zgodnie z tym przykładem.
5. Tworzenie programu i określanie użycia wcześniej utworzonego zasobu. Uruchamianie programu. Używane są interfejsy API do operacji długotrwałych.
6. Tworzenie lokalizatora dla zasobu, tak aby zawartość została opublikowana i mogła być przesłana strumieniowo do klientów.
7. Wyświetlanie i ukrywanie plansz. Uruchamianie i zatrzymywanie anonsów. Używane są interfejsy API do operacji długotrwałych.
8. Czyszczenie kanału i wszystkich skojarzonych zasobów.

## <a name="prerequisites"></a>Wymagania wstępne
Następujące elementy są wymagane do wykonania czynności przedstawionych w samouczku.

* Do ukończenia tego samouczka jest potrzebne konto platformy Azure.

Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](/pricing/free-trial/?WT.mc_id=A261C142F). Otrzymasz kredyt, który można wykorzystać do wypróbowania płatnych usług Azure. Nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług i funkcji platformy Azure, takich jak Web Apps w usłudze Azure App Service.

* Konto usługi Media Services. Aby utworzyć konto usługi Media Services, zobacz temat [Tworzenie konta](media-services-portal-create-account.md).
* Visual Studio 2010 z dodatkiem SP1 (Professional, Premium, Ultimate lub Express) lub nowszy.
* Należy użyć zestawu .NET SDK usługi Media Services w wersji 3.2.0.0 lub nowszej.
* Kamera internetowa i koder, który może wysyłać strumień na żywo o pojedynczej szybkości transmisji bitów.

## <a name="considerations"></a>Zagadnienia do rozważenia
* Obecnie maksymalny zalecany czas trwania wydarzenia na żywo wynosi 8 godzin. Skontaktuj się z nami pod adresem amslived@microsoft.com, jeśli chcesz uruchomić kanał na dłużej.
* Upewnij się, że istnieje co najmniej jedna jednostka zarezerwowanego przesyłania strumieniowego w punkcie końcowym, z którego zawartość ma być przesyłana strumieniowo.

## <a name="download-sample"></a>Pobieranie próbki
Pobierz i uruchom próbkę [z tego miejsca](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).

## <a name="set-up-for-development-with-media-services-sdk-for-.net"></a>Konfigurowanie środowiska deweloperskiego przy użyciu zestawu .NET SDK usługi Media Services
1. Utwórz aplikację konsoli przy użyciu programu Visual Studio.
2. Dodaj zestaw SDK Usługi multimediów dla platformy .NET do aplikacji konsoli przy użyciu pakietu NuGet usługi Media Services.

## <a name="connect-to-media-services"></a>Łączenie się z usługą Media Services
Najlepszą praktyką jest użycie pliku app.config do przechowywania nazwy i klucza konta usługi Media Services.

> [!NOTE]
> Aby znaleźć wartości nazwy i klucza, przejdź do portalu Azure, a następnie wybierz swoje konto. Po prawej stronie zostanie wyświetlone okno Ustawienia. W oknie Ustawienia wybierz opcję Klucze. Kliknięcie ikony obok każdego pola tekstowego powoduje skopiowanie wartości do schowka systemowego.
> 
> 

Dodaj sekcję appSettings w pliku app.config i ustaw wartości nazwy i klucza konta usługi Media Services.

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>


## <a name="code-example"></a>Przykładowy kod
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";

            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];

            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;


            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                IChannel channel = CreateAndStartChannel();

                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();

                Console.WriteLine("Intest URL: {0}", ingestUrl);


                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();

                Console.WriteLine("Preview URL: {0}", previewEndpoint);

                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();

                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);

                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();

                IProgram program = CreateAndStartProgram(channel, asset);

                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);

                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);

                // Once you are done streaming, clean up your resources.
                Cleanup(channel);

            }

            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();


                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };

                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");

                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();

                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");

                return channel;
            }

            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }

            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelPreview CreateChannelPreview()
            {
                return new ChannelPreview
                {
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }

            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }

            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);

                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

                asset.DeliveryPolicies.Add(policy);

                return asset;
            }

            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);

                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");

                return program;
            }

            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );

                return locator;
            }

            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));

                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);

                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();

                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");

                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");

                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");

                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");

                Log("Deleting slate asset");
                slateAsset.Delete();
            }

            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();

                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");

                        program.Delete();

                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();

                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }

                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");

                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }


            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;

                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);

                return entityId;
            }

            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {

                bool completed = false;

                entityId = null;

                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }


            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


## <a name="next-step"></a>Następny krok
Przejrzyj ścieżki szkoleniowe dotyczące usługi Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else?"></a>Szukasz czegoś innego?
Jeśli ten temat nie zawiera oczekiwanych treści, brakuje w nim informacji lub w inny sposób nie spełnia Twoich potrzeb, podziel się swoją opinią za pomocą wątku usługi Disqus poniżej.

<!--HONumber=Oct16_HO3-->


