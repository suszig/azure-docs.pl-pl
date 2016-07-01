<properties 
    pageTitle="Korzystanie z usługi Azure Media Services i klasycznego portalu Azure do prowadzenia transmisji strumieniowych na żywo ze strumieniami o wielokrotnej szybkości transmisji bitów" 
    description="Ten samouczek przedstawia tworzenie kanału, który odbiera strumień na żywo o pojedynczej szybkości transmisji bitów i koduje go jako strumień o wielokrotnej szybkości transmisji bitów przy użyciu klasycznego portalu Azure." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako,anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="05/05/2016" 
    ms.author="juliako"/>


#Korzystanie z usługi Azure Media Services i klasycznego portalu Azure do prowadzenia transmisji strumieniowych na żywo ze strumieniami o wielokrotnej szybkości transmisji bitów

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [Interfejs API REST](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Ten samouczek przedstawia tworzenie **kanału**, który odbiera strumień na żywo o pojedynczej szybkości transmisji bitów i koduje go jako strumień o wielokrotnej szybkości transmisji bitów.

>[AZURE.NOTE]Aby uzyskać więcej informacji o pojęciach związanych z kanałami obsługującymi kodowanie na żywo, zobacz temat [Korzystanie z usługi Azure Media Services do prowadzenia transmisji strumieniowych na żywo ze strumieniami o wielokrotnej szybkości transmisji bitów](media-services-manage-live-encoder-enabled-channels.md).

##Typowy scenariusz transmisji strumieniowej na żywo

Poniżej przedstawiono ogólne etapy tworzenia typowych aplikacji transmisji strumieniowej na żywo.

>[AZURE.NOTE] Obecnie maksymalny zalecany czas trwania wydarzenia na żywo wynosi 8 godzin. Skontaktuj się z nami pod adresem amslived@microsoft.com, jeśli chcesz uruchomić kanał na dłużej.

1. Podłącz kamerę wideo do komputera. Uruchom i skonfiguruj lokalny koder na żywo, który wysyła strumień o pojedynczej szybkości transmisji bitów przy użyciu jednego z następujących protokołów: RTMP, Smooth Streaming lub RTP (MPEG-TS). Aby uzyskać więcej informacji, zobacz temat [Obsługa protokołu RTMP i kodery na żywo w usłudze Azure Media Services](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Ten krok można również wykonać po utworzeniu kanału.

1. Utwórz i uruchom kanał. 

1. Pobierz adres URL pozyskiwania kanału. 

    Koder na żywo używa adresu URL pozyskiwania do wysyłania strumienia do kanału.
1. Pobierz adres URL podglądu kanału. 

    Użyj tego adresu URL, aby sprawdzić, czy kanał prawidłowo odbiera strumień na żywo.

3. Utwórz program (który spowoduje również utworzenie elementu zawartości). 
1. Opublikuj program (który spowoduje utworzenie lokalizatora OnDemand dla skojarzonego elementu zawartości).  

    Upewnij się, że istnieje co najmniej jedna jednostka zarezerwowanego przesyłania strumieniowego w punkcie końcowym, z którego zawartość ma być przesyłana strumieniowo.
1. Uruchom program, gdy wszystko będzie gotowe do rozpoczęcia przesyłania strumieniowego i archiwizacji.
2. Opcjonalnie można przesłać do kodera na żywo sygnał o rozpoczęciu reklamy. Reklama jest wstawiana do strumienia wyjściowego.
1. Zatrzymaj program w dowolnym momencie, w którym chcesz zatrzymać przesyłanie strumieniowe i archiwizowanie wydarzenia.
1. Usuń program (opcjonalnie można również usunąć zasób).   

##Informacje o tym samouczku

W tym samouczku klasyczny portal Azure jest używany do wykonywania następujących zadań: 

2.  Skonfigurowanie punktów końcowych przesyłania strumieniowego.
3.  Utworzenie kanału, który jest skonfigurowany do przeprowadzania kodowania na żywo.
1.  Pobranie adresu URL pozyskiwania w celu dostarczenia go do kodera na żywo. Koder na żywo użyje tego adresu URL na potrzeby wysyłania strumienia do kanału. .
1.  Utworzenie programu (i elementu zawartości)
1.  Opublikowanie elementu zawartości i pobranie adresów URL przesyłania strumieniowego  
1.  Odtwarzanie zawartości 
2.  Czyszczenie

##Wymagania wstępne
Następujące elementy są wymagane do wykonania czynności przedstawionych w samouczku.

- Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz temat [Bezpłatna wersja próbna systemu Azure](https://azure.microsoft.com/pricing/free-trial/).
- Konto usługi Media Services. Aby utworzyć konto usługi Media Services, zobacz temat [Tworzenie konta](media-services-create-account.md).
- Kamera internetowa i koder, który może wysyłać strumień na żywo o pojedynczej szybkości transmisji bitów.

##Konfigurowanie punktu końcowego przesyłania strumieniowego za pomocą portalu

Podczas korzystania z usługi Azure Media Services jednym z najbardziej typowych scenariuszy jest dostarczanie klientom usługi przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów. W przypadku przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów klient może przełączyć się na przesyłanie strumieniowe o większej lub mniejszej szybkości transmisji bitów, ponieważ zawartość wideo jest wyświetlana w oparciu o bieżącą przepustowość sieci, wykorzystanie procesora CPU i inne czynniki. Usługa Media Services obsługuje następujące technologie przesyłania strumieniowego z adaptacyjną szybkością transmisji bitów: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH i HDS (tylko dla posiadaczy licencji Adobe PrimeTime/Access). 

Podczas pracy z transmisją strumieniową na żywo lokalny koder na żywo (w tym przypadku Wirecast) wysyła strumień o wielokrotnej szybkości transmisji bitów na żywo do kanału. Gdy użytkownik zażąda strumienia, usługa Media Services używa funkcji dynamicznego tworzenia pakietów do ponownego skompilowania pakietów strumienia źródłowego w formie żądanego przesyłania strumieniowego o adaptacyjnej szybkości transmisji bitów (HLS, DASH lub Smooth). 

Aby skorzystać z dynamicznego tworzenia pakietów, pobierz co najmniej jedną jednostkę przesyłania strumieniowego **punktu końcowego przesyłania strumieniowego**, z którego planujesz dostarczać zawartość.

Aby zmienić liczbę jednostek zarezerwowanego przesyłania strumieniowego, wykonaj następujące czynności:

1. W [klasycznym portalu Azure](https://manage.windowsazure.com/) kliknij pozycję **Media Services**. Następnie kliknij nazwę usługi multimediów.

2. Wybierz stronę PUNKTY KOŃCOWE PRZESYŁANIA STRUMIENIOWEGO. Następnie kliknij punkt końcowy przesyłania strumieniowego, który chcesz zmodyfikować.

3. Aby określić liczbę jednostek przesyłania strumieniowego, wybierz kartę SKALA i przesuń suwak **wydajności rezerwowej**.

    ![Strona Skala](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-origin-scale.png)

4. Kliknij przycisk ZAPISZ, aby zapisać zmiany.

    Alokacja nowych jednostek trwa około 20 minut. 

     
    >[AZURE.NOTE] Obecnie przejście od dowolnej wartości dodatniej do zera może spowodować wyłączenie przesyłania strumieniowego nawet na godzinę.
    >
    > Podczas obliczania kosztów brana jest pod uwagę największa liczba jednostek określona dla okresu 24 godzin. Aby dowiedzieć się więcej o cenach, zobacz artykuł [Szczegółowe informacje o cenach usługi Media Services](http://go.microsoft.com/fwlink/?LinkId=275107).

 
##Tworzenie KANAŁU

1.  W [klasycznym portalu Azure](http://manage.windowsazure.com/) kliknij pozycję Media Services, a następnie kliknij nazwę konta usługi Media Services.
2.  Wybierz stronę KANAŁY.
3.  Wybierz opcję Dodaj+, aby dodać nowy kanał.

Wybierz **Standardowe** typy kodowania. Ten typ określa, że chcesz utworzyć kanał obsługujący kodowanie na żywo. Oznacza to, że przychodzący strumień o pojedynczej szybkości transmisji bitów jest wysyłany do kanału i kodowany do strumienia o wielokrotnej szybkości transmisji bitów przy użyciu określonych ustawień kodera na żywo. Aby uzyskać więcej informacji na ten temat, zobacz artykuł [Korzystanie z usługi Azure Media Services do prowadzenia transmisji strumieniowych na żywo ze strumieniami o wielokrotnej szybkości transmisji bitów.](media-services-manage-live-encoder-enabled-channels.md).

![standard0][standard0]

Dla typu kodowania **Standardowy** poprawne opcje protokołu pozyskiwania to:

- Pojedyncza szybkość transmisji bitów podzielonej zawartości w formacie MP4 (Smooth Streaming)
- Pojedyncza szybkość transmisji bitów w formacie RTMP
- RTP (MPEG TS): strumień transportu MPEG-2 przy użyciu protokołu RTP.

Aby uzyskać szczegółowe informacje na temat wszystkich protokołów, zobacz artykuł [Korzystanie z usługi Azure Media Services do prowadzenia transmisji strumieniowych na żywo ze strumieniami o wielokrotnej szybkości transmisji bitów](media-services-manage-live-encoder-enabled-channels.md).

![standard1][standard1]

Nie można zmienić protokołu wejściowego, gdy kanał lub skojarzone z nim programy są uruchomione. Jeśli potrzebujesz różnych protokołów, utwórz osobny kanał dla każdego protokołu wejściowego.  

Na stronie **Konfiguracja reklamy** można określić źródło sygnałów znaczników reklamowych. Przy użyciu portalu można wybrać tylko interfejs API, który wskazuje, że koder na żywo w kanale powinien nasłuchiwać asynchronicznego Interfejsu API znacznika reklamy. Podczas używania portalu można wybrać tylko interfejs API.

Aby uzyskać więcej informacji na ten temat, zobacz artykuł [Korzystanie z usługi Azure Media Services do prowadzenia transmisji strumieniowych na żywo ze strumieniami o wielokrotnej szybkości transmisji bitów.](media-services-manage-live-encoder-enabled-channels.md).

![standard2][standard2]

Na stronie **Ustawienie wstępne kodowania** można zaznaczyć ustawienia systemu. Obecnie jedyną wartością ustawienia wstępnego systemu, którą można wybrać, jest **Domyślna rozdzielczość 720p**.

![standard3][standard3]

Na stronie **Tworzenie kanału** można zdefiniować adresy IP, które mogą publikować pliki wideo w tym kanale. Dozwolone adresy IP można określić jako pojedynczy adres IP (np. „10.0.0.1”), zakres adresów IP przy użyciu adresu IP i maski podsieci CIDR (np. „10.0.0.1/22”) lub zakres adresów IP przy użyciu adresu IP i maski podsieci w zapisie kropkowo-cyfrowym (np. „10.0.0.1(255.255.252.0)”).

Jeśli adresy IP nie zostaną określone i brakuje definicji reguły, to żaden adres IP nie będzie dozwolony. Aby zezwolić na jakikolwiek adres IP, utwórz regułę i ustaw wartość 0.0.0.0/0.


![standard4][standard4]

>[AZURE.NOTE] Uruchomienie kanału może obecnie potrwać do 30 minut. Resetowanie kanału może potrwać maksymalnie 5 minut.

Po utworzeniu kanału możesz wybrać kartę **KODER**, na której można zobaczyć konfiguracje kanałów. Można na niej również zarządzać reklamami i planszami. 

![standard5][standard5]

Aby uzyskać więcej informacji na ten temat, zobacz artykuł [Korzystanie z usługi Azure Media Services do prowadzenia transmisji strumieniowych na żywo ze strumieniami o wielokrotnej szybkości transmisji bitów.](media-services-manage-live-encoder-enabled-channels.md).


##Pobieranie adresów URL pozyskiwania

Po utworzeniu kanału można pobrać adresy URL pozyskiwania, które należy udostępnić koderowi na żywo. Koder używa tych adresów URL do wprowadzenia strumienia na żywo.

![readychannel](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ready-channel.png)


![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##Tworzenie programu i zarządzanie nim

###Omówienie

Kanał jest skojarzony z programami, które umożliwiają kontrolowanie publikowania i przechowywania segmentów strumienia na żywo. Kanały zarządzają programami. Relacja kanału i programu jest bardzo podobna do relacji w tradycyjnych multimediach, gdzie kanał ma stały strumień zawartości, a program obejmuje niektóre zdarzenia czasowe na tym kanale.

Można określić liczbę godzin, aby zachować zarejestrowaną zawartość na potrzeby programu przez ustawienie długości **Okna archiwum**. Ta wartość musi mieścić się w zakresie od 5 minut do maksymalnie 25 godzin. Długość okna archiwum określa również dostępny dla klientów zakres cofania odtwarzania pliku od bieżącego momentu transmisji na żywo. Programy mogą być transmitowane w określonym czasie, ale zawartość, która wykracza poza długość okna, jest stale odrzucana. Wartość tej właściwości określa również, jak długie mogą być manifesty na kliencie.

Każdy program jest skojarzony z zasobem. Aby opublikować program, należy utworzyć lokalizator OnDemand dla skojarzonego elementu zawartości. Lokalizator umożliwia utworzenie adresu URL przesyłania strumieniowego, który można udostępnić klientom.

Kanał obsługuje maksymalnie trzy jednocześnie uruchomione programy, dzięki czemu można tworzyć wiele archiwów tego samego strumienia przychodzącego. Umożliwia to w razie potrzeby publikowanie i archiwizację różnych części wydarzenia. Na przykład zgodnie z wymaganiami biznesowymi potrzebna jest archiwizacja sześciu godzin programu, ale do emisji przeznaczonych jest tylko ostatnich 10 minut. W tym celu należy utworzyć dwa jednocześnie uruchomione programy. Jeden z programów jest skonfigurowany na potrzeby archiwizacji sześciu godzin zdarzenia, ale ten program nie jest publikowany. Drugi z programów jest skonfigurowany w celu archiwizacji 10 minut wydarzenia i ten program zostanie opublikowany.

Nie należy używać ponownie istniejących programów dla nowych zdarzeń. Zamiast tego należy utworzyć i uruchomić nowy program dla każdego zdarzenia.

Uruchom program, gdy wszystko będzie gotowe do rozpoczęcia przesyłania strumieniowego i archiwizacji. Zatrzymaj program w dowolnym momencie, w którym chcesz zatrzymać przesyłanie strumieniowe i archiwizowanie wydarzenia. 

Aby usunąć zarchiwizowaną zawartość, zatrzymaj i usuń program, a następnie usuń skojarzony element zawartości. Nie można usunąć elementu zawartości, jeśli jest on używany przez program. Najpierw należy usunąć program. 

Nawet po zatrzymaniu i usunięciu programu użytkownicy będą mogli przesyłać strumieniowo zarchiwizowaną zawartość wideo na żądanie tak długo, jak zasoby nie zostaną usunięte.

Jeśli chcesz zachować zarchiwizowaną zawartość, ale bez udostępniania jej do przesyłania strumieniowego, usuń lokalizator przesyłania strumieniowego.

###Tworzenie, uruchamianie i zatrzymywanie programów

Po przesłaniu strumienia do kanału można rozpocząć zdarzenie przesyłania strumieniowego poprzez utworzenie elementu zawartości, programu i lokalizatora przesyłania strumieniowego. Spowoduje to archiwizację strumienia i udostępni go użytkownikom za pośrednictwem punktu końcowego przesyłania strumieniowego. 

Istnieją dwa sposoby rozpoczęcia zdarzenia: 

1. Na stronie **KANAŁ** kliknij przycisk **DODAJ**, aby dodać nowy program.

    Określ nazwę programu, nazwę elementu zawartości, okno archiwum i opcję szyfrowania.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Jeśli opcja **Opublikuj ten program teraz** była zaznaczona, zostaną utworzone adresy URL PUBLIKOWANIA.
    
    Kliknij przycisk **START**, gdy wszystko będzie gotowe do rozpoczęcia przesyłania strumieniowego programu.

    Po uruchomieniu programu możesz nacisnąć przycisk Odtwórz, aby rozpocząć odtwarzanie zawartości.


    ![createdprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-created-program.png)

2. Możesz również użyć skrótu i kliknąć przycisk **ROZPOCZNIJ PRZESYŁANIE STRUMIENIOWE** na stronie **KANAŁ**. Spowoduje to utworzenie elementu treści, programu i lokalizatora przesyłania strumieniowego.

    Program ma nazwę DefaultProgram (Program domyślny) i okno archiwum jest ustawione na 1 godzinę.

    Opublikowany program można odtworzyć na stronie KANAŁ. 

    ![channelpublish](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-channel-play.png)


Jeśli klikniesz przycisk **ZATRZYMAJ PRZESYŁANIE STRUMIENIOWE** na stronie **KANAŁ**, program domyślny zostanie zatrzymany i usunięty. Element zawartości będzie nadal istniał i będzie go można opublikować lub usunąć publikację na stronie **ZAWARTOŚĆ**.

Po przejściu na stronę **ZAWARTOŚĆ** zobaczysz elementy zawartości, które zostały utworzone dla programów.

![contentasset](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-content-assets.png)


##Odtwarzanie zawartości

Aby udostępnić użytkownikowi adres URL, który może służyć do strumieniowego przesyłania zawartości, należy najpierw opublikować element zawartości (jak zostało opisane w poprzedniej sekcji), tworząc lokalizator (podczas publikowania zasobów przy użyciu portalu lokalizatory są tworzone automatycznie). Lokalizatory zapewniają dostęp do plików znajdujących się w elemencie zawartości. 

W zależności od tego, jakiego protokołu przesyłania strumieniowego chcesz użyć do odtwarzania zawartości, może być konieczne zmodyfikowanie adresu URL, który można pobrać z linku kanał\program **PUBLIKUJ ADRES URL**.

Dynamiczne tworzenie pakietów spowoduje utworzenie pakietu transmisji strumieniowej na żywo w określonym protokole. 

Domyślnie adres URL przesyłania strumieniowego ma następujący format i służy do odtwarzania elementów zawartości przy użyciu protokołu Smooth Streaming:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Aby utworzyć adres URL przesyłania strumieniowego w protokole HLS, dołącz do adresu URL ciąg (format=m3u8-aapl).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Aby utworzyć adres URL przesyłania strumieniowego w protokole MPEG DASH, dołącz do adresu URL ciąg (format=mpd-time-csf).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Aby uzyskać więcej informacji na temat dostarczania zawartości, zobacz artykuł [Dostarczanie zawartości](media-services-deliver-content-overview.md).

Możesz odtwarzać zawartość w protokole Smooth Stream przy użyciu odtwarzacza [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) lub urządzeń iOS i Android w celu odtwarzania zawartości w protokole HLS w wersji 3.

##Czyszczenie

Aby po zakończeniu strumieniowego przesyłania zdarzeń, wyczyścić udostępnione wcześniej zasoby, postępuj zgodnie z poniższą procedurą.

- Zatrzymaj wypychanie strumienia z kodera.
- Zatrzymaj kanał. Po zatrzymaniu kanału opłaty nie będą naliczane. W razie potrzeby ponownego uruchomienia kanał będzie miał ten sam adres URL pozyskiwania, więc nie trzeba będzie ponownie konfigurować kodera.
- Można zatrzymać punkt końcowy przesyłania strumieniowego, chyba że chcesz nadal udostępniać archiwum zdarzenia na żywo w formie przesyłania strumieniowego na żądanie. Jeśli kanał jest zatrzymany, żadne opłaty nie będą naliczane.
  

##Zagadnienia do rozważenia

- Obecnie maksymalny zalecany czas trwania wydarzenia na żywo wynosi 8 godzin. Skontaktuj się z nami pod adresem amslived@microsoft.com, jeśli chcesz uruchomić kanał na dłużej.
- Upewnij się, że istnieje co najmniej jedna jednostka zarezerwowanego przesyłania strumieniowego w punkcie końcowym, z którego zawartość ma być przesyłana strumieniowo.


##Ścieżki szkoleniowe dotyczące usługi Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



[standard0]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard0.png
[standard1]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard1.png
[standard2]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard2.png
[standard3]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard3.png
[standard4]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard4.png
[standard5]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard_encode.png 


<!--HONumber=Jun16_HO2-->


