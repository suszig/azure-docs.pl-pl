<properties
    pageTitle=" Tworzenie konta usługi Azure Media Services za pomocą portalu Azure | Microsoft Azure"
    description="Ten samouczek przeprowadzi Cię przez kroki tworzenia konta usługi Azure Media Services za pomocą portalu Azure."
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
    ms.date="09/07/2016"
    ms.author="juliako"/>



# Tworzenie konta usługi Azure Media Services za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/). 

Portal Azure umożliwia szybkie utworzenie konta usługi Azure Media Services (AMS). Korzystając z konta, możesz uzyskiwać dostęp do usługi Media Services, która umożliwia przechowywanie, szyfrowanie, kodowanie i przesyłanie strumieniowe zawartości oraz zarządzanie nią na platformie Azure. Podczas tworzenia konta usługi Media Services możesz również utworzyć skojarzone konto magazynu (lub użyć istniejącego konta) w tym samym regionie geograficznym, co konto usługi Media Services.

W tym artykule omówiono niektóre typowe pojęcia i pokazano, jak utworzyć konto usługi Media Services za pomocą portalu Azure.

## Pojęcia

Uzyskiwanie dostępu do usługi Media Services wymaga dwóch skojarzonych kont:

- Konto usługi Media Services. Twoje konto umożliwia dostęp do opartej na chmurze usługi Media Services dostępnej na platformie Azure. Na koncie usługi Media Services nie jest przechowywana zawartość multimedialna. Zamiast tego na koncie są przechowywane metadane dotyczące zawartości multimedialnej oraz zadania przetwarzania multimediów. Podczas tworzenia konta można wybrać dostępny region usługi Media Services. Wybrany region określa centrum danych, w którym są przechowywane rekordy metadanych konta użytkownika.

    Dostępne są następujące regiony usługi Media Services (AMS): Europa Północna, Europa Zachodnia, Zachodnie stany USA, Wschodnie stany USA, Azja Południowo-Wschodnia, Azja Wschodnia, Japonia Zachodnia, Japonia Wschodnia. Usługa Media Services nie korzysta z grup koligacji.
    
    Usługa AMS jest obecnie również dostępna w następujących centrach danych: Brazylia Południowa, Indie Zachodnie, Indie Południowe i Indie Środkowe. Portalu Azure można teraz używać do tworzenia kont usługi Media Services i wykonywania różnych zadań opisanych tutaj. Jednak w tych centrach danych nie jest obsługiwana funkcja Live Encoding. Ponadto nie wszystkie typy jednostek zarezerwowanych do celów związanych z kodowaniem są dostępne w tych centrach danych.
    
    - Brazylia Południowa: dostępne są wyłącznie podstawowe i standardowe jednostki zarezerwowane do celów związanych z kodowaniem.
    - Indie Zachodnie, Indie Południowe: zapewnianie obiektów blob magazynu dla plików multimedialnych; konta magazynu muszą znajdować się w tym samym regionie geograficznym co konto usługi Media Services. Podczas tworzenia konta usługi Media Services można wybrać istniejące konto magazynu w tym samym regionie lub można utworzyć nowe konto magazynu w tym samym regionie. Jeśli usuniesz konto usługi Media Services, obiekty blob na powiązanym koncie magazynu nie zostaną usunięte.

## Tworzenie konta AMS

W tej sekcji opisano kroki w procesie tworzenia konta usługi AMS.

1. Zaloguj się w [portalu Azure](https://portal.azure.com/).
2. Kliknij kolejno pozycje **+Nowe** > **Media + CDN** > **Media Services**.

    ![Tworzenie usługi Media Services](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Na stronie **TWORZENIE KONTA USŁUGI MEDIA SERVICES** wprowadź wymagane wartości.

    ![Tworzenie usługi Media Services](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. W polu **Nazwa konta** wprowadź nazwę nowego konta usługi AMS. Nazwa konta usługi Media Services składa się z małych liter i cyfr (bez spacji) i może zawierać od 3 do 24 znaków.
    2. W subskrypcji wybierz jedną z różnych subskrypcji Azure, do których masz dostęp.
    
    2. W polu **Grupa zasobów** wybierz nowy lub istniejący zasób.  Grupa zasobów jest kolekcją zasobów, które mają ten sam cykl życia, uprawnienia i zasady. Więcej informacji można znaleźć [tutaj](resource-group-overview.md#resource-groups).
    3. W polu **Lokalizacja** wybierz region geograficzny używany do przechowywania nośników i rekordów metadanych dla konta usługi Media Services. Ten region będzie służyć do przetwarzania i przesyłania strumieniowego multimediów. Na liście rozwijanej są wyświetlane tylko regiony dostępne w usłudze Media Services. 
    
    3. W polu **Konto magazynu** wybierz konto magazynu, aby udostępnić magazyn obiektów Blob dla zawartości multimedialnej z konta usługi Media Services. Można wybrać istniejące konto magazynu w tym samym regionie geograficznym co konto usługi Media Services albo utworzyć konto magazynu. Nowe konto magazynu jest tworzone w tym samym regionie. Reguły dotyczące nazw kont magazynów są takie same, jak w przypadku kont usługi Media Services.

        Więcej informacji o magazynie można znaleźć [tutaj](storage-introduction.md).

    4. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**, aby wyświetlić postęp wdrażania konta.
    
7. Kliknij opcję **Utwórz** w dolnej części formularza.

    Po pomyślnym utworzeniu konta stan zmieni się na **Uruchomiony**. 

    ![Ustawienia usługi Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Do zarządzania kontem AMS (na przykład przekazywania plików wideo, kodowania elementów zawartości, monitorowania postępu zadania) używaj okna **Ustawienia**.

## Zarządzanie kluczami

Do uzyskania programowego dostępu do konta usługi Media Services będą wymagane nazwa konta i informacje o kluczu podstawowym.

1. W portalu Azure wybierz konto. 

    Po prawej stronie zostanie wyświetlone okno **Ustawienia**. 

2. W oknie **Ustawienia** wybierz opcję **Klucze**. 

    W oknie **Zarządzanie kluczami** widoczna jest nazwa konta oraz wyświetlane są klucze podstawowe i pomocnicze. 
3. Naciśnij przycisk kopiowania, aby skopiować wartości.
    
    ![Klucze usługi Media Services](./media/media-services-portal-vod-get-started/media-services-keys.png)

## Następne kroki

Teraz możesz przekazać pliki na konto usługi AMS. Więcej informacji znajduje się na stronie [Przekazywanie plików](media-services-portal-upload-files.md).

## Ścieżki szkoleniowe dotyczące usługi Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





<!--HONumber=Sep16_HO3-->


