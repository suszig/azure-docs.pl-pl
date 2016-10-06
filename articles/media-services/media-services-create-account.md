<properties
    pageTitle="Tworzenie konta usługi Media Services | Microsoft Azure"
    description="Opis sposobu tworzenia nowego konta usługi Azure Media Services na platformie Azure."
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
    ms.date="09/26/2016"
    ms.author="juliako"/>



# Tworzenie konta usługi Azure Media Services

> [AZURE.SELECTOR]
- [Portal](media-services-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)


> [AZURE.NOTE] Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Aby uzyskać szczegółowe informacje, zobacz temat [Azure Free Trial](/pricing/free-trial/?WT.mc_id=A261C142F) (Bezpłatna wersja próbna platformy Azure).
 
Azure Classic Portal umożliwia szybkie utworzenie konta usługi Azure Media Services. Korzystając z konta, możesz uzyskiwać dostęp do usługi Media Services, która umożliwia przechowywanie, szyfrowanie, kodowanie i przesyłanie strumieniowe zawartości oraz zarządzanie nią na platformie Azure. Podczas tworzenia konta usługi Media Services możesz również utworzyć skojarzone konto magazynu (lub użyć istniejącego konta) w tym samym regionie geograficznym, co konto usługi Media Services.

W tym artykule opisano sposób utworzenia nowego konta usługi Media Services metodą szybkiego tworzenia oraz kojarzenia go z kontem magazynu.

<a id="concepts"></a>
## Pojęcia

Uzyskiwanie dostępu do usługi Media Services wymaga dwóch skojarzonych kont:

-   **Konto usługi Media Services**. Twoje konto umożliwia dostęp do opartej na chmurze usługi Media Services dostępnej na platformie Azure. Na koncie usługi Media Services nie jest przechowywana zawartość multimedialna. Zamiast tego na koncie są przechowywane metadane dotyczące zawartości multimedialnej oraz zadania przetwarzania multimediów. Podczas tworzenia konta można wybrać dostępny region usługi Media Services. Wybrany region określa centrum danych, w którym są przechowywane rekordy metadanych konta użytkownika.

    Dostępne są następujące regiony usługi Media Services (AMS): Europa Północna, Europa Zachodnia, Zachodnie stany USA, Wschodnie stany USA, Azja Południowo-Wschodnia, Azja Wschodnia, Japonia Zachodnia, Japonia Wschodnia. Usługa Media Services nie korzysta z grup koligacji.
    
    Usługa AMS jest obecnie również dostępna w następujących centrach danych: Brazylia Południowa, Indie Zachodnie, Indie Południowe i Indie Środkowe. Azure Classic Portal można teraz używać do [tworzenia kont Usługi multimediów](media-services-create-account.md#create-a-media-services-account-using-quick-create) i wykonywania różnych zadań opisanych [tutaj](https://azure.microsoft.com/documentation/services/media-services/). Jednak w tych centrach danych nie jest obsługiwana funkcja Live Encoding. Ponadto nie wszystkie typy jednostek zarezerwowanych do celów związanych z kodowaniem są dostępne w tych centrach danych.
    
    - Brazylia Południowa: dostępne są wyłącznie podstawowe i standardowe jednostki zarezerwowane do celów związanych z kodowaniem
    - Indie Zachodnie, Indie Południowe i Indie Środkowe: dostępne są wyłącznie podstawowe jednostki zarezerwowane do celów związanych z kodowaniem


-   **Skojarzone konto magazynu**. Twoje konto magazynu jest kontem usługi Azure Storage skojarzonym z kontem usługi Media Services. Konto magazynu stanowi magazyn obiektów blob dla plików multimedialnych i musi znajdować się w tym samym regionie geograficznego, co konto usługi Media Services. Podczas tworzenia konta usługi Media Services można wybrać istniejące konto magazynu w tym samym regionie lub można utworzyć nowe konto magazynu w tym samym regionie. Jeśli usuniesz konto usługi Media Services, obiekty blob na powiązanym koncie magazynu nie zostaną usunięte.

<a id="quick"></a>
## Tworzenie konta usługi Media Services metodą szybką

1. W [Azure Classic Portal][] kliknij opcję **Nowe**, kliknij pozycję **Usługa multimediów**, a następnie kliknij pozycję **Szybkie tworzenie**.

![Szybkie tworzenie konta w usłudze Media Services](./media/media-services-create-account/wams-QuickCreate.png)

2. W polu **NAZWA** wpisz nazwę nowego konta. Nazwa konta usługi Media Services składa się z małych liter i cyfr (bez spacji) i może zawierać od 3 do 24 znaków.

3. W polu **REGION** wybierz region geograficzny używany do przechowywania rekordów metadanych dla konta usługi Media Services. Na liście rozwijanej są wyświetlane tylko dostępne regiony usługi Media Services.

4. W polu **KONTO MAGAZYNU** wybierz konto magazynu, aby udostępnić magazyn obiektów Blob dla zawartości multimedialnej z konta usługi Media Services. Można wybrać istniejące konto magazynu w tym samym regionie geograficznym co konto usługi Media Services albo utworzyć nowe konto magazynu. Nowe konto magazynu jest tworzone w tym samym regionie.

5. Jeśli utworzono nowe konto magazynu, w polu **NAZWA NOWEGO KONTA MAGAZYNU** wprowadź nazwę konta magazynu. Reguły dotyczące nazw kont magazynów są takie same, jak w przypadku kont usługi Media Services.

6. Kliknij opcję **Szybkie tworzenie** w dolnej części formularza.

Stan procesu można monitorować w obszarze wiadomości w dolnej części okna.

Po pomyślnym utworzeniu konta stan zmieni się na aktywny. Zostanie otwarta strona **Media Services** z wyświetlonym nowym kontem.

W dolnej części strony widoczny będzie przycisk **ZARZĄDZAJ KLUCZAMI**. Po kliknięciu tego przycisku zostanie wyświetlona strona z nazwą konta usługi Media Services oraz kluczami: podstawowym i pomocniczym. Do uzyskania programowego dostępu do konta usługi Media Services będą wymagane nazwa konta i informacje o kluczu podstawowym.

![Strona usługi Media Services](./media/media-services-create-account/wams-mediaservices-page.png)

Dwukrotne kliknięcie nazwy konta domyślnie powoduje wyświetlenie strony **Szybki start**. Ta strona umożliwia wykonywanie niektórych zadań zarządzania, które są również dostępne na innych stronach portalu. Na przykład plik wideo można przesłać z tej strony lub ze strony **ZAWARTOŚĆ**.

Ponadto można wyświetlić kod korzystający z zestawu SDK usługi Media Services w celu wykonania zadań przekazywania, kodowania i publikowania plików wideo. Możesz kliknąć jeden z linków w sekcji **TUTAJ WPISZ SWÓJ KOD**, skopiować kod, a następnie wykorzystać go w aplikacji.



##Ścieżki szkoleniowe dotyczące usługi Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## Następne kroki

- [Wprowadzenie do dostarczania wideo na żądanie (VoD) przy użyciu zestawu SDK programu .NET](media-services-dotnet-get-started.md)

- [Korzystanie z zestawu SDK programu .NET do tworzenia kanałów wykonujących kodowanie na żywo strumienia z pojedynczej do wielokrotnej szybkości transmisji bitów](media-services-dotnet-creating-live-encoder-enabled-channel.md)

<!-- Reusable paths. -->

<!-- Anchors. -->
  [Pojęcia]: #concepts
  [Przed rozpoczęciem]: #begin
  [Porady: tworzenie konta usługi Media Services metodą szybką]: #quick

<!-- URLs. -->
  [Instalator platformy sieci Web]: http://go.microsoft.com/fwlink/?linkid=255386

  [Azure Classic Portal]: http://manage.windowsazure.com/



<!--HONumber=Sep16_HO4-->


