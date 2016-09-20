<properties
    pageTitle=" Wprowadzenie do dostarczania zawartości na żądanie przy użyciu klasycznego portalu Azure | Microsoft Azure"
    description="W tym samouczku przedstawiono kroki wdrażania aplikacji do dostarczania zawartości wideo na żądanie (VoD) w usłudze Azure Media Services przy użyciu klasycznego portalu Azure."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/22/2016"
    ms.author="juliako"/>


# Wprowadzenie do dostarczania zawartości na żądanie przy użyciu klasycznego portalu Azure


[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


W tym samouczku przedstawiono kroki wdrażania podstawowej aplikacji do dostarczania zawartości wideo na żądanie (VoD) przy użyciu klasycznego portalu Azure.

> [AZURE.NOTE] Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](/pricing/free-trial/?WT.mc_id=A261C142F). 


W tym samouczku opisano następujące zadania:

1.  Tworzenie konta usługi Azure Media Services.
2.  Konfigurowanie punktów końcowych przesyłania strumieniowego.
1.  Ładowanie pliku wideo.
1.  Kodowanie pliku źródłowego do zestawu plików MP4 z adaptacyjną szybkością transmisji bitów.
1.  Publikowanie elementu zawartości i uzyskiwanie adresów URL na potrzeby przesyłania strumieniowego i pobierania progresywnego.  
1.  Odtwarzanie zawartości.


## Tworzenie konta usługi Azure Media Services

1. W [klasycznym portalu Azure](https://manage.windowsazure.com/) kliknij opcję **Nowe**, kliknij pozycję **Media Service**, a następnie kliknij pozycję **Szybkie tworzenie**.

    ![Szybkie tworzenie konta w usłudze Media Services](./media/media-services-portal-get-started/wams-QuickCreate.png)

2. W polu **NAZWA** wpisz nazwę nowego konta. Nazwa konta usługi Media Services składa się z małych liter i cyfr (bez spacji) i może zawierać od 3 do 24 znaków.

3. W polu **REGION** wybierz region geograficzny używany do przechowywania rekordów metadanych dla konta usługi Media Services. Na liście rozwijanej są wyświetlane tylko regiony dostępne w usłudze Media Services.

4. W polu **KONTO MAGAZYNU** wybierz konto magazynu, aby udostępnić magazyn obiektów Blob dla zawartości multimedialnej z konta usługi Media Services. Można wybrać istniejące konto magazynu w tym samym regionie geograficznym co konto usługi Media Services albo utworzyć nowe konto magazynu. Nowe konto magazynu jest tworzone w tym samym regionie.

5. Jeśli utworzono nowe konto magazynu, w polu **NAZWA NOWEGO KONTA MAGAZYNU** wprowadź nazwę konta magazynu. Reguły dotyczące nazw kont magazynów są takie same, jak w przypadku kont usługi Media Services.

6. Kliknij opcję **Szybkie tworzenie** w dolnej części formularza.

    Stan procesu można monitorować w obszarze wiadomości w dolnej części okna.

    Po pomyślnym utworzeniu konta stan zmieni się na aktywny.

    W dolnej części strony widoczny będzie przycisk **ZARZĄDZAJ KLUCZAMI**. Po kliknięciu tego przycisku zostanie wyświetlone okno dialogowe z nazwą konta usługi Media Services oraz kluczami: podstawowym i pomocniczym. Do uzyskania programowego dostępu do konta usługi Media Services potrzebna będzie nazwa konta i informacje o kluczu podstawowym.

    ![Strona usługi Media Services](./media/media-services-portal-get-started/wams-mediaservices-page.png)

    Dwukrotne kliknięcie nazwy konta domyślnie powoduje wyświetlenie strony Szybki start. Ta strona umożliwia wykonywanie niektórych zadań zarządzania, które są również dostępne na innych stronach portalu. Na przykład plik wideo można załadować z tej strony lub ze strony ZAWARTOŚĆ.


## Konfigurowanie punktu końcowego przesyłania strumieniowego przy użyciu portalu

Podczas pracy w usłudze Azure Media Services jednym z najbardziej typowych scenariuszy jest zapewnianie klientom przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów. W przypadku przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów klient może przełączyć się na przesyłanie strumieniowe o większej lub mniejszej szybkości transmisji bitów, ponieważ zawartość wideo jest wyświetlana w oparciu o bieżącą przepustowość sieci, wykorzystanie procesora CPU i inne czynniki. Usługa Media Services obsługuje następujące technologie przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH i HDS (tylko dla posiadaczy licencji Adobe PrimeTime/Access).

Usługa Media Services udostępnia funkcję dynamicznego tworzenia pakietów, która pozwala dostarczać kodowaną zawartość plików MP4 lub Smooth Streaming z adaptacyjną szybkością transmisji bitów w formatach transmisji strumieniowej obsługiwanych przez usługę Media Services (MPEG DASH, HLS, Smooth Streaming, HDS) bez konieczności ponownego tworzenia pakietów w tych formatach.

Aby skorzystać z funkcji dynamicznego tworzenia pakietów, należy wykonać następujące czynności:

- Zakoduj plik (źródłowy) mezzanine do zestawu plików MP4 lub Smooth Streaming z adaptacyjną szybkością transmisji bitów (kroki kodowania przedstawiono w dalszej części tego samouczka).  
- Pobierz co najmniej jedną jednostkę przesyłania strumieniowego dla *punktu końcowego przesyłania strumieniowego*, z którego planujesz dostarczać zawartość.

Dzięki funkcji dynamicznego tworzenia pakietów wystarczy przechowywać i opłacać pliki w jednym formacie magazynu, a usługa Media Services skompiluje oraz udostępni właściwą odpowiedź na podstawie żądań klienta.

Aby zmienić liczbę jednostek zarezerwowanego przesyłania strumieniowego, wykonaj następujące czynności:

1. W [klasycznym portalu Azure](https://manage.windowsazure.com/) kliknij pozycję **Media Services**. Następnie kliknij nazwę usługi multimediów.

2. Wybierz stronę PUNKTY KOŃCOWE PRZESYŁANIA STRUMIENIOWEGO. Następnie kliknij punkt końcowy przesyłania strumieniowego, który chcesz zmodyfikować.

3. Aby określić liczbę jednostek przesyłania strumieniowego, wybierz kartę **SKALA** i przesuń suwak **wydajności rezerwowej**.

    ![Strona Skala](./media/media-services-portal-get-started/media-services-origin-scale.png)

4. Kliknij przycisk **ZAPISZ**, aby zapisać zmiany.

    Alokacja nowych jednostek trwa około 20 minut.

    >[AZURE.NOTE] Obecnie przejście od dowolnej wartości dodatniej do zera może spowodować wyłączenie przesyłania strumieniowego nawet na godzinę.
    >
    > Podczas obliczania kosztów brana jest pod uwagę największa liczba jednostek określona dla okresu 24 godzin. Aby dowiedzieć się więcej o cenach, zobacz artykuł [Szczegółowe informacje o cenach usługi Media Services](http://go.microsoft.com/fwlink/?LinkId=275107).

## Przekazywanie zawartości


1. W [klasycznym portalu Azure](http://go.microsoft.com/fwlink/?LinkID=256666&clcid=0x409) kliknij przycisk **Media Services**, a następnie kliknij nazwę konta usługi Media Services.
2. Wybierz stronę ZAWARTOŚĆ.
3. Kliknij przycisk **Przekaż** na stronie lub w dolnej części portalu.
4. W oknie dialogowym **Przekaż zawartość** przejdź do wybranego pliku elementu zawartości. Kliknij plik, a następnie kliknij przycisk **Otwórz** lub naciśnij klawisz Enter.

    ![UploadContentDialog][uploadcontent]

5. W oknie dialogowym **Przekaż zawartość** kliknij przycisk wyboru, aby zaakceptować opcje **Plik** i **Nazwa zawartości**.
6. Rozpocznie się przekazywanie, a jego postęp można śledzić w dolnej części portalu.  

    ![JobStatus][status]

Po zakończeniu przekazywania na liście zawartości pojawi się nowy element zawartości. Zgodnie z konwencją do końca nazwy zostanie dodana wartość "**-Source**", co ułatwi śledzenie nowej zawartości jako zawartości źródłowej do kodowania zadań.

![ContentPage][contentpage]

Jeśli wartość rozmiaru pliku nie zostanie zaktualizowania po zatrzymaniu procesu przekazywania kliknij przycisk **Synchronizuj metadane**. Spowoduje to synchronizację rozmiaru pliku elementu zawartości z rzeczywistym rozmiarem pliku w magazynie oraz odświeżenie wartości na stronie zawartości.


## Kodowanie zawartości

### Omówienie

Aby dostarczyć film wideo w formie cyfrowej przez Internet trzeba skompresować nośniki. Usługa Media Services zapewnia koder nośników, który pozwala określić sposób kodowania zawartości (na przykład kodeki do użycia, format pliku, rozdzielczość i szybkość transmisji bitów.)

Podczas pracy w usłudze Azure Media Services jednym z najbardziej typowych scenariuszy jest zapewnianie klientom przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów. W przypadku przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów klient może przełączyć się na przesyłanie strumieniowe o większej lub mniejszej szybkości transmisji bitów, ponieważ zawartość wideo jest wyświetlana w oparciu o bieżącą przepustowość sieci, wykorzystanie procesora CPU i inne czynniki. Usługa Media Services obsługuje następujące technologie przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH i HDS (tylko dla posiadaczy licencji Adobe PrimeTime/Access).

Usługa Media Services udostępnia funkcję dynamicznego tworzenia pakietów, która pozwala dostarczać kodowaną zawartość plików MP4 lub Smooth Streaming z adaptacyjną szybkością transmisji bitów w formatach transmisji strumieniowej obsługiwanych w usłudze Media Services (MPEG DASH, HLS, Smooth Streaming, HDS) bez konieczności ponownego tworzenia pakietów w tych formatach.

Aby skorzystać z funkcji dynamicznego tworzenia pakietów, należy wykonać następujące czynności:

- Zakoduj plik (źródłowy) mezzanine do zestawu plików MP4 lub Smooth Streaming z adaptacyjną szybkością transmisji bitów (kroki kodowania przedstawiono w dalszej części tego samouczka).
- Pobierz co najmniej jedną jednostkę przesyłania strumieniowego na żądanie dla punktu końcowego przesyłania strumieniowego, z którego planujesz dostarczać zawartość. Aby uzyskać więcej informacji, zobacz temat [Sposób skalowania jednostek zarezerwowanego przesyłania strumieniowego na żądanie](media-services-manage-origins.md#scale_streaming_endpoints/).

Dzięki funkcji dynamicznego tworzenia pakietów wystarczy przechowywać i opłacać pliki w jednym formacie magazynu, a usługa Media Services skompiluje oraz udostępni właściwą odpowiedź na podstawie żądań klienta.

Oprócz możliwości używania funkcji dynamicznego tworzenia pakietów jednostki zarezerwowanego przesyłania strumieniowego na żądanie zapewniają funkcję dedykowanej komunikacji wyjściowej, którą można zakupić według przyrostów 200 Mbps. Domyślnie przesyłanie strumieniowe na żądanie jest konfigurowane w modelu udostępnionego wystąpienia, w którym zasoby serwera (na przykład obliczanie lub funkcja komunikacji wyjściowej) są udostępniane wszystkim pozostałym użytkownikom. W celu zwiększenia wydajności przesyłania strumieniowego na żądanie zaleca się zakup jednostek zarezerwowanego przesyłania strumieniowego na żądanie.

### Kodowanie

W tej sekcji opisano kroki, które należy wykonać w celu zakodowania zawartości przy użyciu standardu Media Encoder Standard za pomocą klasycznego portalu Azure.

1.  Wybierz plik, który chcesz zakodować.
    Jeśli kodowanie dla tego typu pliku jest obsługiwane, włączony będzie przycisk **PRZETWARZAJ** w dolnej części strony ZAWARTOŚĆ.
4. W oknie dialogowym **Przetwarzaj** wybierz procesor **Media Encoder Standard**.
5. Wybierz jedną z **konfiguracji kodowania**.

    ![Process2][process2]

    W temacie [Ciągi ustawień wstępnych zadań dla standardu Media Encoder Standard](https://msdn.microsoft.com/en-US/library/mt269960) wyjaśniono, co oznaczają poszczególne ustawienia wstępne.  

5. Następnie wprowadź wybraną przyjazną nazwę zawartości wyjściowej lub zaakceptuj nazwę domyślną. Następnie kliknij przycisk wyboru, aby uruchomić operację kodowania. Postęp możesz śledzić w dolnej części portalu.
6. Kliknij przycisk **OK**.

    Po ukończeniu kodowania na stronie ZAWARTOŚĆ będzie znajdować się zakodowany plik.

    Aby wyświetlić postęp zadania kodowania, przejdź na stronę **ZADANIA**.  

    Jeśli wartość rozmiaru pliku nie zostanie zaktualizowana po ukończeniu kodowania, kliknij przycisk **Synchronizuj metadane**. Spowoduje to zsynchronizowanie rozmiaru pliku elementu zawartości wyjściowej z rzeczywistym rozmiarem pliku w magazynie oraz odświeżenie wartości na stronie zawartości.


## Publikowanie zawartości

### Omówienie

Aby podać użytkownikowi adres URL, który może służyć do przesyłania strumieniowego lub pobierania zawartości, należy najpierw „opublikować” element zawartości przez utworzenie lokalizatora. Lokalizatory zapewniają dostęp do plików znajdujących się w elemencie zawartości. Usługa Media Services obsługuje dwa typy lokalizatorów: lokalizatory OnDemandOrigin używane do strumieniowego przesyłania plików multimedialnych (na przykład w formacie MPEG DASH, HLS i Smooth Streaming) oraz lokalizatory sygnatury dostępu współdzielonego (SAS) używane do pobierania plików multimedialnych.

W przypadku korzystania z klasycznego portalu Azure w celu opublikowania elementów zawartości zostają utworzone lokalizatory oraz zostaje podany adres URL oparty na funkcji OnDemand (jeśli element zawartości zawiera plik .ism) lub adres URL SAS.

Adres URL SAS ma następujący format.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Adres URL przesyłania strumieniowego ma następujący format i służy do odtwarzania elementów zawartości przy użyciu protokołu Smooth Streaming.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Aby utworzyć adres URL przesyłania strumieniowego w protokole HLS, dołącz do adresu URL ciąg (format=m3u8-aapl).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Aby utworzyć adres URL przesyłania strumieniowego w protokole MPEG DASH, dołącz do adresu URL ciąg (format=mpd-time-csf).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Lokalizatory mają datę wygaśnięcia. W przypadku użycia portalu do opublikowania elementów zawartości zostają utworzone lokalizatory z okresem ważności trwającym 100 lat.

>[AZURE.NOTE] Jeśli użyto portalu do utworzenia lokalizatorów przed marcem 2015 r., zostały utworzone lokalizatory z dwuletnim okresem ważności.  

Do aktualizacji daty wygaśnięcia na lokalizatorze użyj interfejsu API [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) lub [.NET](http://go.microsoft.com/fwlink/?LinkID=533259). Należy pamiętać, że po zaktualizowaniu daty wygaśnięcia lokalizatora SAS następuje zmiana adresu URL.

### Publikowanie

Aby opublikować element zawartości za pomocą portalu, wykonaj następujące czynności:

1. Wybierz element zawartości. 
2. Następnie kliknij przycisk Publikuj.

 ![PublishedContent][publishedcontent]


## Odtwarzanie zawartości z portalu

Klasyczny portal Azure udostępnia odtwarzacz zawartości, którego można użyć do przetestowania filmu wideo.

Kliknij wybrany film wideo, a następnie kliknij przycisk **Odtwórz** w dolnej części portalu.

Zagadnienia do rozważenia:

- Zadbaj o to, aby film wideo został opublikowany.
- **Odtwarzacz zawartości MEDIA SERVICES** odtwarza z domyślnego punktu końcowego przesyłania strumieniowego. Do odtwarzania z punktu końcowego przesyłania strumieniowego innego niż domyślny należy użyć innego odtwarzacza (np. [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html)).


![AMSPlayer][AMSPlayer]



##Następne kroki: ścieżki szkoleniowe dotyczące usługi Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## Szukasz czegoś innego?

Jeśli ten temat nie zawiera oczekiwanych treści, brakuje w nim informacji lub w inny sposób nie spełnia Twoich potrzeb, podziel się swoją opinią w wątku Disqus poniżej.

### Dodatkowe zasoby
- <a href="http://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-101-Get-your-video-online-now-">Azure Media Services 101 — opublikuj teraz swój film wideo w Internecie!</a>
- <a href="http://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-102-Dynamic-Packaging-and-Mobile-Devices">Azure Media Services 102 — dynamiczne tworzenie pakietów i urządzenia mobilne</a>


<!-- Anchors. -->


<!-- URLs. -->
[klasyczny portal Azure]: http://manage.windowsazure.com/


<!-- Images -->
[portaloverview]: ./media/media-services-portal-get-started/media-services-content-page.png
[publishedcontent]: ./media/media-services-portal-get-started/media-services-upload-content-published.png
[uploadcontent]: ./media/media-services-portal-get-started/UploadContent.png
[status]: ./media/media-services-portal-get-started/Status.png
[encoder]: ./media/media-services-manage-content/EncoderDialog2.png
[branding]: ./media/branding-reporting.png
[contentpage]: ./media/media-services-portal-get-started/media-services-content-page.png
[process]: ./media/media-services-manage-content/media-services-process-video.png
[process2]: ./media/media-services-portal-get-started/media-services-process-video2.png
[encrypt]: ./media/media-services-manage-content/media-services-encrypt-content.png
[AMSPlayer]: ./media/media-services-portal-get-started/media-services-portal-player.png



<!--HONumber=sep16_HO1-->


