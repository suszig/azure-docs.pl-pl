---
title: "Wprowadzenie do dostarczania zawartości wideo (VoD) na żądanie przy użyciu witryny Azure Portal | Microsoft Docs"
description: "W tym samouczku przedstawiono kolejne kroki wdrażania podstawowej usługi do dostarczania zawartości wideo na żądanie (VoD) za pomocą aplikacji Azure Media Services (AMS) przy użyciu portalu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 555e0b6340d09517bfd87efe209f0304f3266788
ms.openlocfilehash: 76fd245f91e1bfab3df68120859c69e459283e5b


---
# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Wprowadzenie do dostarczania zawartości na żądanie przy użyciu portalu Azure
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

W tym samouczku przedstawiono kolejne kroki wdrażania podstawowej usługi do dostarczania zawartości wideo na żądanie (VoD) za pomocą aplikacji Azure Media Services (AMS) przy użyciu portalu Azure.

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania czynności przedstawionych w tym samouczku są niezbędne następujące elementy:

* Konto platformy Azure. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/). 
* Konto usługi Media Services. Aby utworzyć konto usługi Media Services, zobacz temat [Jak utworzyć konto usługi Media Services](media-services-portal-create-account.md).

W tym samouczku opisano następujące zadania:

1. Uruchamianie punktu końcowego przesyłania strumieniowego.
2. Ładowanie pliku wideo.
3. Kodowanie pliku źródłowego do zestawu plików MP4 z adaptacyjną szybkością transmisji bitów.
4. Publikowanie elementu zawartości i uzyskiwanie adresów URL na potrzeby przesyłania strumieniowego i pobierania progresywnego.  
5. Odtwarzanie zawartości.

## <a name="start-streaming-endpoints"></a>Uruchamianie punktu końcowego przesyłania strumieniowego 

Podczas pracy w usłudze Azure Media Services jednym z najbardziej typowych scenariuszy jest dostarczanie obrazu wideo za pośrednictwem przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów. Usługa Media Services udostępnia funkcję dynamicznego tworzenia pakietów, która pozwala dostarczać kodowaną zawartość plików MP4 z adaptacyjną szybkością transmisji bitów w formatach przesyłania strumieniowego obsługiwanych przez usługę Media Services (MPEG DASH, HLS, Smooth Streaming) w odpowiednim czasie bez konieczności przechowywania wersji wstępnie utworzonych pakietów poszczególnych formatów przesyłania strumieniowego.

>[!NOTE]
>Po utworzeniu konta usługi AMS zostanie do niego dodany **domyślny** punkt końcowy przesyłania strumieniowego mający stan **Zatrzymany**. Aby rozpocząć przesyłanie strumieniowe zawartości oraz korzystać z dynamicznego tworzenia pakietów i szyfrowania dynamicznego, punkt końcowy przesyłania strumieniowego, z którego chcesz strumieniowo przesyłać zawartość, musi mieć stan **Uruchomiony**. 

Aby uruchomić punkt końcowy przesyłania strumieniowego, wykonaj następujące czynności:

1. Zaloguj się w [portalu Azure](https://portal.azure.com/).
2. W oknie Ustawienia kliknij pozycję Punkty końcowe przesyłania strumieniowego. 
3. Kliknij domyślny punkt końcowy przesyłania strumieniowego. 

    Zostanie wyświetlone okno SZCZEGÓŁY DOMYŚLNEGO PUNKTU KOŃCOWEGO PRZESYŁANIA STRUMIENIOWEGO.

4. Kliknij ikonę Uruchom.
5. Kliknij przycisk Zapisz, aby zapisać zmiany.

## <a name="upload-files"></a>Przekazywanie plików
Aby przesłać strumieniowo pliki wideo przy użyciu usługi Azure Media Services, musisz przekazać źródłowe pliki wideo, zakodować je do wielokrotnych szybkości transmisji bitów oraz opublikować wynik. Pierwszy krok został omówiony w tej sekcji. 

1. W oknie **Ustawienie** kliknij przycisk **Elementy zawartości**.
   
    ![Przekazywanie plików](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. Kliknij przycisk **Przekaż**.
   
    Zostanie wyświetlone okno **Przekaż element zawartości wideo**.
   
   > [!NOTE]
   > Nie ma żadnego limitu rozmiaru pliku.
   > 
   > 
3. Przejdź do wybranego pliku wideo na komputerze, zaznacz go, a następnie kliknij przycisk OK.  
   
    Rozpocznie się przekazywanie, a postęp będzie widoczny pod nazwą pliku.  

Po zakończeniu przekazywania na liście w oknie **Elementy zawartości** pojawi się nowy element zawartości. 

## <a name="encode-assets"></a>Kodowanie elementów zawartości
Podczas pracy w usłudze Azure Media Services jednym z najbardziej typowych scenariuszy jest zapewnianie klientom przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów. Usługa Media Services obsługuje następujące technologie przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów: HTTP Live Streaming (HLS), Smooth Streaming i MPEG DASH. Aby przygotować pliki wideo do przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów, należy zakodować źródłowy plik wideo do plików o różnej szybkości transmisji bitów. Do kodowania plików wideo należy używać kodera **Media Encoder Standard**.  

Usługa Media Services udostępnia również funkcję dynamicznego tworzenia pakietów, która pozwala dostarczać pliki MP4 o różnej szybkości transmisji bitów w formatach przesyłania strumieniowego MPEG DASH, HLS i Smooth Streaming bez konieczności ponownego tworzenia pakietów w tych formatach. Dzięki funkcji dynamicznego tworzenia pakietów wystarczy przechowywać i opłacać pliki w jednym formacie magazynu, a usługa Media Services skompiluje oraz udostępni właściwą odpowiedź na podstawie żądań klienta.

Aby korzystać z dynamicznego tworzenia pakietów, wykonaj kodowanie pliku źródłowego do zestawu plików MP4 o różnej szybkości transmisji bitów (kroki kodowania przedstawiono w dalszej części tej sekcji).

### <a name="to-use-the-portal-to-encode"></a>Aby korzystać z portalu do kodowania
W tej sekcji opisano kroki, które należy wykonać w celu zakodowania zawartości przy użyciu standardu Media Encoder Standard.

1. W oknie **Ustawienia** wybierz opcję **Elementy zawartości**.  
2. W oknie **Elementy zawartości** wybierz element zawartości, który chcesz kodować.
3. Kliknij przycisk **Koduj**.
4. W oknie **Kodowanie elementu zawartości** wybierz procesor „Media Encoder Standard” oraz ustawienie wstępne. Na przykład: jeśli wejściowy plik wideo ma rozdzielczość 1920 x 1080 pikseli, można użyć ustawienia wstępnego „Wielokrotna szybkość transmisji bitów H264 1080p”. Aby uzyskać więcej informacji dotyczących ustawień wstępnych, zobacz [ten artykuł](media-services-mes-presets-overview.md) — ważne jest, aby wybrać ustawienie wstępne, które jest najbardziej odpowiednie dla wejściowego pliku wideo. Jeśli wideo jest w niskiej rozdzielczości (640 x 360), nie używaj ustawienia wstępnego „Wielokrotna szybkość transmisji bitów H264 1080p”.
   
   Aby ułatwić zarządzanie istnieje możliwość edytowania nazwy wyjściowego elementu zawartości oraz nazwy zadania.
   
   ![Kodowanie elementów zawartości](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Kliknij przycisk **Utwórz**.

### <a name="monitor-encoding-job-progress"></a>Monitorowanie postępu zadania kodowania
Aby monitorować postęp zadania kodowania, kliknij pozycję **Ustawienia** (w górnej części strony), a następnie wybierz pozycję **Zadania**.

![Zadania](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Publikowanie zawartości
Aby podać użytkownikowi adres URL, który może służyć do przesyłania strumieniowego lub pobierania zawartości, należy najpierw „opublikować” element zawartości przez utworzenie lokalizatora. Lokalizatory zapewniają dostęp do plików znajdujących się w elemencie zawartości. Usługa Media Services obsługuje dwa typy lokalizatorów: 

* Lokalizatory przesyłania strumieniowego (OnDemandOrigin) używane do adaptacyjnego przesyłania strumieniowego (na przykład, przesyłania MPEG DASH, HLS lub Smooth Streaming). Aby utworzyć lokalizator przesyłania strumieniowego element zawartości musi zawierać plik .ism. 
* Progresywne lokalizatory (SAS) używane do dostarczania plików wideo przy użyciu pobierania progresywnego.

Adres URL przesyłania strumieniowego ma następujący format i służy do odtwarzania elementów zawartości przy użyciu protokołu Smooth Streaming.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Aby utworzyć adres URL przesyłania strumieniowego w protokole HLS, dołącz do adresu URL ciąg (format=m3u8-aapl).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Aby utworzyć adres URL przesyłania strumieniowego w protokole MPEG DASH, dołącz do adresu URL ciąg (format=mpd-time-csf).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Adres URL SAS ma następujący format.

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> Jeśli użyto portalu do utworzenia lokalizatorów przed marcem 2015 r., zostały utworzone lokalizatory z dwuletnim okresem ważności.  
> 
> 

Do aktualizacji daty wygaśnięcia na lokalizatorze użyj interfejsu API [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) lub [.NET](http://go.microsoft.com/fwlink/?LinkID=533259). Po zaktualizowaniu daty wygaśnięcia lokalizatora SAS następuje zmiana adresu URL.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Aby opublikować element zawartości za pomocą portalu
Aby opublikować element zawartości za pomocą portalu, wykonaj następujące czynności:

1. Wybierz kolejno pozycje **Ustawienia** > **Elementy zawartości**.
2. Wybierz element zawartości, który chcesz opublikować.
3. Kliknij przycisk **Publikuj**.
4. Wybierz typ lokalizatora.
5. Kliknij przycisk **Dodaj**.
   
    ![Publikowanie](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Adres URL jest dodawany do listy **Opublikowane adresy URL**.

## <a name="play-content-from-the-portal"></a>Odtwarzanie zawartości z portalu
Portal Azure udostępnia odtwarzacz zawartości, który może służyć do testowania pliku wideo.

Kliknij wybrany plik wideo, a następnie kliknij przycisk **Odtwórz**.

![Publikowanie](./media/media-services-portal-vod-get-started/media-services-play.png)

Zagadnienia do rozważenia:

* Zadbaj o to, aby film wideo został opublikowany.
* Ten **odtwarzacz multimediów** odtwarza z domyślnego punktu końcowego przesyłania strumieniowego. Aby odtworzyć z punktu końcowego przesyłania strumieniowego innego niż domyślny, kliknij, aby skopiować adres URL i użyć innego odtwarzacza (np. [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html)).

## <a name="next-steps"></a>Następne kroki
Przejrzyj ścieżki szkoleniowe dotyczące usługi Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!--HONumber=Jan17_HO4-->


