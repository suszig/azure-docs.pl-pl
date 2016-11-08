---
title: Przegląd usługi Azure Media Services i typowe scenariusze | Microsoft Docs
description: Ten temat zawiera omówienie usługi Azure Media Services
services: media-services
documentationcenter: ''
author: Juliako
manager: erikre
editor: ''

ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/12/2016
ms.author: juliako;anilmur

---
# <a name="azure-media-services-overview-and-common-scenarios"></a>Przegląd usługi Azure Media Services i typowe scenariusze
Microsoft Azure Media Services to oparta na chmurze rozszerzona platforma, która umożliwia deweloperom tworzenie skalowalnych aplikacji do dostarczania multimediów i zarządzania nimi. Usługa Media Services są oparte na interfejsach API REST, które umożliwiają bezpieczne przekazywanie, przechowywanie, kodowanie i tworzenie pakietów zawartości wideo lub audio na potrzeby dostarczania transmisji strumieniowej na żądanie i na żywo do różnych klientów (np. odbiorników TV, komputerów PC i urządzeń przenośnych).

Korzystając wyłącznie z usługi Media Services, można tworzyć kompleksowe przepływy pracy. W przypadku niektórych części przepływu pracy można użyć składników innych firm. Na przykład kodowanie można wykonać przy użyciu kodera innego producenta. Natomiast przekazywanie, zabezpieczanie, tworzenie pakietów i dostarczanie można realizować za pomocą usługi Media Services.

Możliwe jest przesłanie strumieniowe zawartości na żywo lub dostarczanie zawartości na żądanie. W tym temacie przedstawiono typowe scenariusze dostarczania zawartości [na żywo](media-services-overview.md#live_scenarios) lub [na żądanie](media-services-overview.md#vod_scenarios). Temat zawiera także linki do innych powiązanych tematów.

## <a name="sdks-and-tools"></a>Zestawy SDK i narzędzia
W celu tworzenia rozwiązań Media Services można użyć następujących elementów:

* [Interfejs API REST usługi Media Services](https://msdn.microsoft.com/library/azure/hh973617.aspx)
* Jeden z dostępnych zestawów SDK klienta:
* [Zestaw SDK usługi Azure Media Services dla platformy .NET](https://github.com/Azure/azure-sdk-for-media-services)
* [Zestaw Azure SDK dla języka Java](https://github.com/Azure/azure-sdk-for-java)
* [Zestaw Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php)
* [Azure Media Services dla środowiska Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Jest to wersja zestawu Node.js SDK firmy innej niż Microsoft. Jest ona opracowywana przez społeczność i aktualnie nie obsługuje 100% interfejsów API usług AMS.)
* Istniejące narzędzia:
* [Azure Portal](https://portal.azure.com/)
* [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) to aplikacja Winforms/C# dla systemu Windows)

## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
Ścieżki szkoleniowe dotyczące usługi AMS można zobaczyć tutaj:

* [Przepływ pracy transmisji strumieniowej na żywo usługi AMS](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [Przepływ pracy transmisji strumieniowej na żądanie usługi AMS](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Wymagania wstępne
Do rozpoczęcia korzystania z usługi Azure Media Services potrzebne są:

1. Konto platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com).
2. Konto usługi Azure Media Services. Aby utworzyć konto usługi Azure Media Services, użyj witryny Azure Portal, platformy .NET lub interfejsu API REST. Aby uzyskać więcej informacji, zobacz temat [Tworzenie konta](media-services-portal-create-account.md).
3. (Opcjonalnie) Konfigurowanie środowiska deweloperskiego. Wybierz platformę .NET lub interfejs API REST dla środowiska deweloperskiego. Aby uzyskać więcej informacji, zobacz temat [Konfigurowanie środowiska](media-services-dotnet-how-to-use.md).

Poznaj także sposób programowego ustanawiania połączenia przedstawiony w temacie [Nawiązywanie połączenia](media-services-dotnet-connect-programmatically.md).

1. (Zalecane) Przydziel co najmniej jedną jednostkę skalowania. Zalecane jest przydzielenie co najmniej jednej jednostki skalowania dla aplikacji w środowisku produkcyjnym.   Aby uzyskać więcej informacji, zobacz temat [Zarządzanie punktami końcowymi przesyłania strumieniowego](media-services-portal-manage-streaming-endpoints.md).

## <a name="concepts-and-overview"></a>Pojęcia i omówienie
Pojęcia związane z usługą Azure Media Services zostały przedstawione w temacie [Pojęcia](media-services-concepts.md).

Serię poradników ułatwiających rozpoczęcie pracy z wszystkimi głównymi składnikami usługi Azure Media Services można znaleźć na stronie [Azure Media Services Step-by-Step tutorials](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series) (Usługa Azure Media Services — samouczki krok po kroku). Seria zawiera wszechstronne omówienie pojęć i przedstawia zadania AMS przy użyciu narzędzia AMSE. Należy pamiętać, że narzędzie AMSE to narzędzie systemu Windows. To narzędzie obsługuje większość zadań, które można wykonać programowo przy użyciu zestawów [AMS SDK dla platformy .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK dla języka Java](https://github.com/Azure/azure-sdk-for-java) lub [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).

## <a name="<a-id="vod_scenarios"></a>delivering-media-on-demand-with-azure-media-services:-common-scenarios-and-tasks"></a><a id="vod_scenarios"></a>Dostarczanie multimediów na żądanie za pomocą usługi Azure Media Services: typowe scenariusze i zadania
W tej sekcji opisano typowe scenariusze oraz udostępniono linki do powiązanych tematów. Na poniższym diagramie przedstawiono główne elementy platformy Media Services, które biorą udział w dostarczaniu zawartości na żądanie. 

![Wideo na żądanie — przepływ pracy](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

### <a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-(non-encrypted)"></a>Ochrona zawartości w magazynie i dostarczanie multimediów strumieniowych w formie niezaszyfrowanej
1. Przekaż wysokiej jakości plik mezzanine do elementu zawartości.
   
    Zaleca się stosowanie opcji szyfrowania magazynu w odniesieniu do elementu zawartości, aby chronić zawartość podczas przekazywania oraz przechowywania w magazynie.
2. Wykonaj kodowanie do zestawu plików MP4 o adaptacyjnej szybkości transmisji bitów. 
   
    Zaleca się stosowanie opcji szyfrowania magazynu w odniesieniu do elementu zawartości wyjściowej, aby chronić przechowywaną zawartość.
3. Skonfiguruj zasady dostarczania elementu zawartości (stosowane podczas dynamicznego tworzenia pakietów). 
   
    Jeśli element zawartości jest szyfrowany w magazynie, **musisz** skonfigurować zasady dostarczania elementu zawartości. 
4. Opublikuj element zawartości, tworząc lokalizator OnDemand.
   
    Upewnij się, że istnieje co najmniej jedna jednostka zarezerwowanego przesyłania strumieniowego w punkcie końcowym, z którego zawartość ma być przesyłana strumieniowo.
5. Prześlij strumieniowo opublikowaną zawartość.

### <a name="protect-content-in-storage,-deliver-dynamically-encrypted-streaming-media"></a>Ochrona zawartości w magazynie i dostarczanie dynamicznie szyfrowanych multimediów strumieniowych
Aby można było korzystać z dynamicznego szyfrowania, należy najpierw pobrać co najmniej jedną jednostkę zarezerwowanego przesyłania strumieniowego w punkcie końcowym przesyłania strumieniowego, z którego ma być przesyłana strumieniowo zaszyfrowana zawartość.

1. Przekaż wysokiej jakości plik mezzanine do elementu zawartości. Zastosuj opcję szyfrowania magazynu w odniesieniu do elementu zawartości.
2. Wykonaj kodowanie do zestawu plików MP4 o adaptacyjnej szybkości transmisji bitów. Zastosuj opcję szyfrowania magazynu w odniesieniu do elementu zawartości wyjściowej.
3. Utwórz klucz szyfrowania zawartości dla elementu zawartości, który ma zostać dynamicznie zaszyfrowany podczas odtwarzania.
4. Skonfiguruj zasady autoryzacji klucza zawartości.
5. Skonfiguruj zasady dostarczania elementu zawartości (używane podczas dynamicznego tworzenia pakietów i dynamicznego szyfrowania).
6. Opublikuj element zawartości, tworząc lokalizator OnDemand.
7. Prześlij strumieniowo opublikowaną zawartość. 

### <a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Korzystanie z Analizy multimediów w celu uzyskania przydatnych wyników analiz na podstawie danych wideo
Analiza multimediów to kolekcja składników mowy i obrazu, które ułatwiają organizacjom i przedsiębiorstwom uzyskiwanie przydatnych wyników analiz na podstawie posiadanych plików wideo. Aby uzyskać więcej informacji, zobacz temat [Przegląd analiz usługi Azure Media Services](media-services-analytics-overview.md).

1. Przekaż wysokiej jakości plik mezzanine do elementu zawartości.
2. Użyj jednej z następujących usług Analizy multimediów, aby przetwarzać pliki wideo:
   
   * **Indeksator** — [przetwarzanie plików wideo za pomocą procesora Azure Media Indexer 2](media-services-process-content-with-indexer2.md)
   * **Przyspieszone ujęcia poklatkowe** — [tworzenie przyspieszonych ujęć poklatkowych w plikach multimedialnych za pomocą usługi Azure Media Hyperlapse](media-services-hyperlapse-content.md)
   * **Wykrywanie ruchu** — [wykrywanie ruchu na potrzeby Analizy multimediów Azure](media-services-motion-detection.md)
   * **Wykrywanie twarzy i emocji** — [wykrywanie twarzy i emocji na potrzeby Analizy multimediów Azure](media-services-face-and-emotion-detection.md)
   * **Podsumowanie wideo** — [tworzenie podsumowań wideo przy użyciu miniatur wideo multimediów Azure](media-services-video-summarization.md)
3. Procesory multimediów usługi Analiza multimediów tworzą pliki MP4 lub JSON. Plik MP4 utworzony przez procesor multimediów można pobrać progresywnie. Plik JSON utworzony przez procesor multimediów można pobrać z magazynu Azure Blob Storage. 

### <a name="deliver-progressive-download"></a>Dostarczanie pobierania progresywnego
1. Przekaż wysokiej jakości plik mezzanine do elementu zawartości.
2. Wykonaj kodowanie do pojedynczego pliku MP4.
3. Opublikuj element zawartości, tworząc lokalizator OnDemand lub SAS.
   
    W przypadku korzystania z lokalizatora OnDemand upewnij się, że istnieje co najmniej jedna jednostka zarezerwowanego przesyłania strumieniowego w punkcie końcowym, z którego zawartość ma być pobierana progresywnie.
   
    W przypadku użycia lokalizatora SAS zawartość zostanie pobrana z magazynu Azure Blob Storage. W takim przypadku jednostki zarezerwowanego przesyłania strumieniowego nie są konieczne.
4. Pobierz progresywnie zawartość.

## <a name="<a-id="live_scenarios"></a>delivering-live-streaming-events-with-azure-media-services"></a><a id="live_scenarios"></a>Dostarczanie wydarzeń transmisji strumieniowej na żywo za pośrednictwem usługi Azure Media Services
W przypadku realizowania transmisji strumieniowej na żywo zwykle wymagane są następujące elementy:

* Kamera, która umożliwia emisję wydarzenia.
* Koder wideo na żywo, który konwertuje sygnały z kamery na strumienie wysyłane do usługi transmisji strumieniowej na żywo.

Opcjonalnie wiele zsynchronizowanych czasowo koderów na żywo. W przypadku niektórych wydarzeń na żywo o kluczowym znaczeniu, które wymagają bardzo wysokiej dostępności i jakości obsługi, zaleca się wdrożenie nadmiarowych koderów w trybie aktywny/aktywny z synchronizacją czasu w celu uzyskania płynnego trybu failover bez utraty danych.

* Usługa transmisji strumieniowej na żywo, która umożliwia wykonanie następujących czynności:
* pozyskiwanie zawartości na żywo przy użyciu różnych protokołów transmisji strumieniowej na żywo (np. RTMP lub Smooth Streaming);
* (opcjonalnie) kodowanie strumienia do strumienia o adaptacyjnej szybkości transmisji bitów;
* wyświetlanie podglądu transmisji strumieniowej na żywo;
* rejestrowanie i przechowywanie pozyskiwanej zawartości w celu późniejszego przesłania jej strumieniowo (wideo na żądanie);
* dostarczanie zawartości za pośrednictwem wspólnych protokołów przesyłania strumieniowego (np. MPEG DASH, Smooth, HLS, HDS) bezpośrednio do klientów lub do sieci dostarczania zawartości (CDN) w celu dalszej dystrybucji.

Usługa **Microsoft Azure Media Services** (AMS) zapewnia możliwość pozyskiwania, kodowania, podglądu, przechowywania i dostarczania transmisji strumieniowej zawartości na żywo.

Podczas dostarczania zawartości do klientów głównym celem jest przekazywanie zawartości wideo wysokiej jakości do różnych urządzeń bez względu na warunki panujące w sieci. Aby zadbać o jakość i warunki sieciowe, należy korzystać z koderów na żywo w celu kodowania strumienia do strumienia wideo o różnych szybkościach transmisji bitów (adaptacyjnej szybkości transmisji bitów).  W celu obsługi przesyłania strumieniowego na różnych urządzeniach należy korzystać z funkcji [dynamicznego tworzenia pakietów](media-services-dynamic-packaging-overview.md) usługi Media Services, aby ponownie dynamicznie utworzyć pakiety na potrzeby transmisji strumieniowej do różnych protokołów. Usługa Media Services obsługuje dostarczanie następujących technologii przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH i HDS (tylko dla licencjobiorców Adobe PrimeTime/Access).

W usłudze Azure Media Services **kanały**, **programy**, i **punkty końcowe przesyłania strumieniowego** obsługują wszystkie funkcje transmisji strumieniowej na żywo, w tym pozyskiwanie, formatowanie, cyfrowy rejestrator wideo (DVR), zabezpieczenia, skalowalność i nadmiarowość.

**Kanał** reprezentuje potok przetwarzania zawartości transmisji strumieniowej na żywo. Kanał może odbierać przychodzące strumienie na żywo w następujący sposób:

* Lokalny koder na żywo wysyła plik **RTMP** o różnych szybkościach transmisji bitów lub plik **Smooth Streaming** (pofragmentowany plik MP4) do kanału, który jest skonfigurowany do obsługi dostarczania zawartości w formie **przekazywania**. Dostarczanie w formie **przekazywania** występuje wtedy, gdy pozyskiwane strumienie są przekazywane przez **kanał** bez dalszego przetwarzania. Można użyć następujących koderów na żywo, które wysyłają pliki Smooth Streaming o różnych szybkościach transmisji bitów: Elemental, Envivio, Cisco.  Następujące kodery na żywo wysyłają pliki RTMP: Adobe Flash Live, Telestream Wirecast i transkodery Tricaster.  Koder na żywo może także wysłać strumień o pojedynczej szybkości transmisji bitów do kanału, który nie obsługuje kodowania na żywo, nie jest to jednak zalecane. Po odebraniu żądania usługa Media Services dostarcza strumień do klientów.

> [!NOTE]
> Metoda przekazywania to najbardziej ekonomiczne rozwiązanie transmisji strumieniowej na żywo w przypadku organizowania wielu wydarzeń w długim okresie oraz poczynionych inwestycji w kodery lokalne. Zobacz szczegółowe informacje o [cenach](/pricing/details/media-services/).
> 
> 

* Lokalny koder na żywo wysyła strumień o pojedynczej szybkości transmisji bitów do kanału obsługującego kodowanie na żywo za pomocą usługi Media Services w jednym z następujących formatów: RTP (MPEG TS), RTMP lub Smooth Streaming (pofragmentowany plik MP4). Kanał wykonuje następnie kodowanie na żywo przychodzącego strumienia o pojedynczej szybkości transmisji bitów do postaci strumienia wideo o różnych szybkościach transmisji bitów (adaptacyjnej szybkości transmisji bitów). Po odebraniu żądania usługa Media Services dostarcza strumień do klientów.

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-(pass-through)"></a>Praca z kanałami odbierającymi strumień na żywo o różnych szybkościach transmisji bitów z koderów lokalnych (przekazujących)
Na poniższym diagramie przedstawiono główne elementy platformy AMS, które są zaangażowane w przepływ pracy **przekazywania**.

![Przepływ pracy na żywo][live-overview2]

Aby uzyskać więcej informacji, zobacz temat [Praca z kanałami odbierającymi strumień na żywo o różnych szybkościach transmisji bitów z koderów lokalnych](media-services-live-streaming-with-onprem-encoders.md).

### <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Praca z kanałami obsługującymi kodowanie na żywo za pomocą usługi Azure Media Services
Na poniższym diagramie przedstawiono główne elementy platformy AMS biorące udział w przepływie pracy transmisji strumieniowej na żywo, gdy kanał obsługuje kodowanie na żywo za pomocą usługi Media Services.

![Przepływ pracy na żywo][live-overview1]

Aby uzyskać więcej informacji, zobacz temat [Praca z kanałami obsługującymi funkcję Live Encoding w usłudze Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

## <a name="consuming-content"></a>Korzystanie z zawartości
Usługa Azure Media Services udostępnia narzędzia potrzebne do tworzenia zaawansowanych, dynamicznych aplikacji klienckich odtwarzacza dla większości platform, takich jak: urządzenia z systemem iOS, urządzenia z systemem Android, urządzenia z systemem Windows, telefony z systemem Windows Phone, konsole Xbox i dekodery. Poniższy temat zawiera linki do zestawów SDK oraz struktur odtwarzaczy, których można użyć do opracowywania własnych aplikacji klienckich korzystających z multimediów strumieniowych dostarczanych przez usługę Media Services.

[Programowanie aplikacji odtwarzacza wideo](media-services-develop-video-players.md)

## <a name="enabling-azure-cdn"></a>Włączanie usługi Azure CDN
Usługa Media Services obsługuje integrację z usługą Azure CDN. Aby uzyskać informacje o sposobie włączania usługi Azure CDN, zobacz temat [Zarządzanie punktami końcowymi przesyłania strumieniowego na koncie usługi Media Services](media-services-portal-manage-streaming-endpoints.md).

## <a name="scaling-a-media-services-account"></a>Skalowanie konta usługi Media Services
Możesz skalować usługę **Media Services**, określając liczbę **jednostek zarezerwowanego przesyłania strumieniowego** i **jednostek zarezerwowanych do celów związanych z kodowaniem** obsługiwanych przez konto.

Możliwe jest także skalowanie konta usługi Media Services przez dodanie do niego kont magazynu. Pojemność każdego konta magazynu jest ograniczona do 500 TB. Aby rozszerzyć pojemność magazynu poza ograniczenia domyślne, można dołączyć wiele kont magazynu do jednego konta usługi Media Services.

[Ten](media-services-portal-scale-streaming-endpoints.md) temat zawiera linki do powiązanych tematów.

## <a name="support"></a>Pomoc techniczna
[Pomoc techniczna platformy Azure](https://azure.microsoft.com/support/options/) zapewnia opcje wsparcia technicznego dla platformy Azure, w tym dla usługi Media Services.

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="service-level-agreement-(sla)"></a>Umowa dotycząca poziomu usług (SLA)
* W przypadku usługi Media Services Encoding gwarantujemy dostępność transakcji interfejsu API REST na poziomie 99,9%.
* W zakresie transmisji strumieniowej zapewniamy pomyślną obsługę żądań z gwarancją dostępności na poziomie 99,9% dla istniejącej zawartości multimedialnej w przypadku zakupu co najmniej jednej jednostki przesyłania strumieniowego.
* W odniesieniu do kanałów na żywo gwarantujemy, że uruchomione kanały będą utrzymywać łączność zewnętrzną przez co najmniej 99,9% czasu.
* W usłudze Content Protection gwarantujemy pomyślną realizację żądań kluczy przez co najmniej 99,9% czasu.
* Dla odniesieniu do indeksatora zapewniamy pomyślną realizację żądań zadań indeksatora przetwarzanych przez jednostkę zarezerwowaną do celów związanych z kodowaniem przez 99,9% czasu.

Aby uzyskać więcej informacji, zobacz temat [Umowy dotyczące poziomu usług platformy Microsoft Azure](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png




<!--HONumber=Oct16_HO3-->


