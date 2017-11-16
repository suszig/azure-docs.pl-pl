---
title: "Wprowadzenie do korzystania z funkcji tworzenia wycinków Azure Media | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do korzystania z funkcji tworzenia wycinków Azure Media, narzędzia do tworzenia nośnika klipy z zasobów"
services: media-services
keywords: "klip subclip; kodowanie; nośnika"
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 8a4f2c79131664ca0d078fa58c6a75b54243e705
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="create-clips-with-azure-media-clipper"></a>Utwórz klipy z funkcji tworzenia wycinków multimediów Azure
W tej sekcji przedstawiono podstawowe kroki wprowadzenie do korzystania z funkcji tworzenia wycinków Azure Media. Sekcje zawierają szczegółowe informacje na temat konfigurowania funkcji tworzenia wycinków Azure nośnika.

- Najpierw dodaj poniższe linki do usługi Azure Media Player i funkcji tworzenia wycinków Azure Media head dokumentu. Firma Microsoft zaleca, jawnie określając wersję funkcji tworzenia wycinków i usługi Azure Media Player w adresach URL. Nie należy używać najnowszej wersji tych zasobów w środowisku produkcyjnym, ponieważ są one może ulec zmianie na żądanie.

```javascript
<!--Azure Media Player 2.1.4 or later is a prerequisite-->
<link href="//amp.azure.net/libs/amp/2.1.4/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amp/2.1.4/azuremediaplayer.min.js"></script>
<!--Azure Media Clipper script and stylesheet-->
<link href="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.css" rel="stylesheet">
<script src="//amp.azure.net/libs/amc/0.1.0/azuremediaclipper.min.js"></script>
```

- Następnie dodaj następujące klasy, do której chcesz utworzyć wystąpienia funkcji tworzenia wycinków element div.

```javascript
<div id="root" class="azure-subclipper" />
```

Opcjonalnie Aby włączyć motywu ciemny, należy dodać klasy skórki ciemny:

```javascript
<div id="root" class="azure-subclipper dark-skin" />
```

- Następnie można utworzyć wystąpienia funkcji tworzenia wycinków z następujące wywołanie interfejsu API:

```javascript
var subclipper = new subclipper({
    selector: '#root',
    logLevel: 'info',
    restVersion: '2.0',
    minimumMarkerGap: 6,
    submitSubclipCallback: onSubmitSubclip,
    singleBitrateMp4Profile: {
            Codecs: [{
                    // Video Codec with single H264Layers
                }, {
                    // Audio Codec
                }]
    },
    multiBitrateMp4Profile: {
            Codecs: [{
                    // Video Codec with multiple H264Layers
                }, {
                    // Audio Codec
                }]
    },
    keymap: {
      // See below for more info
    },
   // Enable the Video Assets panel in the widget to automatically load assets (input contract)
    assetsPanelLoaderCallback: onLoadAssets,
    height: 600,
    subclippingMode: 'all',
    filterAssetsTypes: true,
    speedLevels: [
        { name: '4x', value: 4.0 },
        { name: '3x', value: 3.0 },
        { name: '2x', value: 2.0 },
        { name: 'normal', value: 1.0 },
        { name: '0.75x', value: 0.75 },
        { name: '0.5x', value: 0.5 },
    ],
    resetOnJobDone: false,
    autoplayVideo: true,
    language: 'en'    
});
```

Parametry dla wywołania metody inicjowania są:
- `selector`{WYMAGANE, string}: selektor CSS pasującego elementu HTML, gdzie mają być renderowane elementu widget.
- `restVersion`{WYMAGANE, string}: Azure Media Services wersja interfejsu API REST do docelowego. Wersja REST definiuje format danych wyjściowych wygenerowanych przez elementu widget. Aktualnie obsługiwana jest tylko 2.0.
- `submitSubclipCallback`{WYMAGANE, promise} Funkcja wywołania zwrotnego wywoływana, gdy kliknięto przycisk "przesłać" widżetu. Funkcja wywołania zwrotnego należy oczekiwane dane wyjściowe, generowane przez element widget (Konfiguracja zadania renderowania lub definicji filtru). Aby uzyskać więcej informacji zobacz Wywołanie zwrotne subclip przesyłania.
- `logLevel`{OPCJONALNE, {"info", "ostrzeżenie", "błąd"}}: poziom rejestrowania, który będzie wyświetlany w konsoli przeglądarki. Wartość domyślna: błąd
- `minimumMarkerGap`{OPCJONALNE, int}: minimalny rozmiar subclip (w sekundach). Uwaga: wartość powinna być większa lub równa 6, która jest również wartość domyślna.
- `singleBitrateMp4Profile`{Obiekt JSON opcjonalne} Profil mp4 pojedynczej szybkości transmisji bitów do użycia na potrzeby konfiguracji zadania renderowania generowane przez elementu widget. Jeśli nie zostanie podana, używa [pojedynczej szybkości transmisji bitów MP4 profil domyślny](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-single-bitrate-1080p).
- `multiBitrateMp4Profile`{Obiekt JSON opcjonalne} Profil wielokrotnej szybkości transmisji bitów mp4 do użycia na potrzeby renderowania zadania konfiguracji wygenerowane przez elementu widget. Jeśli nie zostanie podana, używa [wielokrotnej szybkości transmisji bitów MP4 profil domyślny](https://docs.microsoft.com/azure/media-services/media-services-mes-preset-h264-multiple-bitrate-1080p).
- `keymap`{Obiekt json opcjonalne} Umożliwia dostosowywanie skrótów klawiaturowych widżetu. Aby uzyskać więcej informacji, zobacz [można dostosować skróty klawiaturowe](media-services-azure-media-clipper-keyboard-shortcuts.md).
- `assetsPanelLoaderCallback`{Promise opcjonalne} Funkcja wywołania zwrotnego wywoływana w celu załadowania (asynchronicznie) nową stronę zasobów na do panelu zasoby za każdym razem, gdy użytkownik przewija widok w dół do dolnej części okienka. Aby uzyskać więcej informacji zobacz wywołania zwrotnego modułu ładującego okienku zasobów.
- `height`{Numer opcjonalne} Całkowita wysokość elementu widget (wysokość minimalna wynosi 600 pikseli bez okienku zasobów i 850 pikseli z okienkiem zasobów).
- `subclippingMode`(Opcjonalne, {"all", "renderowania", "Filtruj"}): tryb subclipping dozwolone. Wartość domyślna to wszystko.
- `filterAssetsTypes`(Opcjonalne, wartość logiczna): filterAssetsTypes umożliwiają pokazywanie listy rozwijanej filtrów z okienka zasoby. Wartość domyślna to true.
- `speedLevels`(Opcjonalne, array): speedLevels pozwala na ustawienie szybkości różne poziomy dla odtwarzacza wideo, zobacz [dokumentacji usługi Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/#amp.player.playbackspeedoptions) Aby uzyskać więcej informacji.
- `resetOnJobDone`(Opcjonalne, wartość logiczna): resetOnJobDone umożliwia funkcji tworzenia wycinków zresetować subclipper do stanu początkowego, gdy zadanie jest zostało pomyślnie przesłane.
- `autoplayVideo`(Opcjonalne, wartość logiczna): autoplayVideo umożliwia funkcji tworzenia wycinków Autoodtwarzanie wideo na obciążenia. Wartość domyślna to true.
- `language`{OPCJONALNY, ciąg}: język ustawia język elementu widget. Jeśli nie określono elementu widget próbuje localize komunikaty oparte na języku przeglądarki. Jeśli języka nie zostanie wykryty w przeglądarce, widżetu domyślnie jest język angielski. Aby uzyskać więcej informacji zobacz sekcję obsługiwanych języków.
- `languages`{OPCJONALNE, JSON}: parametr języków zastępuje słownik domyślne języki słownika zdefiniowanych przez użytkownika. Aby uzyskać więcej informacji zobacz sekcję obsługiwanych języków.
- `extraLanguages`(Opcjonalne, JSON): parametr extraLanaguages dodaje nowe języki do domyślnego słownika. Aby uzyskać więcej informacji zobacz sekcję obsługiwanych języków.

## <a name="typescript-definition"></a>Definicji typeScript
A [TypeScript](https://www.typescriptlang.org/) można znaleźć pliku definicji dla funkcji tworzenia wycinków [tutaj](http://amp.azure.net/libs/amc/latest/azuremediaclipper.d.ts).

## <a name="azure-media-clipper-api"></a>Azure Media funkcji tworzenia wycinków interfejsu API
W tej sekcji omówiono powierzchni interfejsu API dostarczonych przez funkcji tworzenia wycinków.

- `load(assets)`: ładuje listę zasobów do okienka zasoby (nie należy używać razem z `assetsPanelLoaderCallback`). Zobacz to [artykułu](media-services-azure-media-clipper-load-assets.md) szczegółowe informacje na temat sposobu ładowania zasobów do funkcji tworzenia wycinków.
- `setLogLevel(level)`: Ustawia poziom rejestrowania, który będzie wyświetlany w konsoli przeglądarki. Możliwe wartości to: `info`, `warn`, `error`.
- `setHeight(height)`: ustawia całkowity wysokość w pikselach (minimalna wysokość wynosi 600 pikseli bez okienku zasobów i 850 pikseli z okienkiem zasobów).
- `version`: pobiera wersję elementu widget.

## <a name="configuring-azure-media-clipper"></a>Konfigurowanie funkcji tworzenia wycinków multimediów Azure
Zobacz następne kroki konfigurowania funkcji tworzenia wycinków Azure nośnika:
- [Ładowanie zasobów do funkcji tworzenia wycinków multimediów Azure](media-services-azure-media-clipper-load-assets.md)
- [Konfigurowanie niestandardowych skrótów klawiaturowych](media-services-azure-media-clipper-keyboard-shortcuts.md)
- [Przesyłanie zadań wycinka z funkcji tworzenia wycinków](media-services-azure-media-clipper-submit-job.md)

## <a name="supported-languages"></a>Obsługiwane języki
Widżet funkcji tworzenia wycinków są dostępne w językach 18. Aby ustawić język elementu widget, należy zdefiniować `language` parametr podczas inicjowania. Podaj ciąg kodu żądany język z poniższej listy:
- Chiński (uproszczony): zh-hans
- Chiński (tradycyjny): zh-hant
- Czeski: cs
- Holenderski, Flemish: nl
- Język angielski: en
- Francuski: fr
- Niemiecki: de
- Węgierski: hu
- Włoski: go
- Japoński: Japonia
- Koreański: ko
- Polski: pl
- Portugalski (Brazylia): pt-br
- Portugalski (Portugalia): pt-pt
- Rosyjski: ru
- Hiszpański: es
- Szwedzki: sv
- Turecki: tr

Określanie niestandardowych słownika lub rozszerzyć domyślnego słownika, należy zdefiniować `languages` lub `extraLanguages` parametru odpowiednio. Przekazywanie słownika przy użyciu następującego formatu JSON:

```javascript
{
      "{language-code}":
          "{message-id}": "{message}"
          ...
      }
      ...
}
```

Na przykład następujący przykładowy definiuje zlokalizowanych ciągów angielskiej wersji językowej:

```javascript
export default {
  'VideoPlayer.noPreview': 'No video preview',
  'VideoPlayer.loadAsset': 'You must provide a valid asset',
  'AssetsPanel.name': 'Name',
  'AssetsPanel.type': 'Asset type',
  'AssetsPanel.actions': 'Actions',
  'AssetsPanel.loading': 'Loading...',
  'AssetsPanel.duration': 'Duration',
  'AssetsPanel.resolution': 'Resolution',
  'AssetsPanel.pluralFiles': '{0} assets',
  'AssetsPanel.searchFiles': 'Search assets',
  'AssetsPanel.showTypes': 'Show:',
  'AssetsPanel.typesInfo': 'Rendered assets are actual MP4 files. Dynamic manifest filters are filters applied to a parent asset\'s video segment playlist.',
  'AssetsPanel.filterTypes': 'Filters',
  'AssetsPanel.assetTypes': 'Assets',
  'AssetsPanel.assetsAll': 'All',
  'AssetsPanel.addAsset': 'Add asset to the end',
  'AssetsPanel.addFilter': 'Add filter to the timeline',
  'AssetsPanel.invalidAsset': 'The metadata of this asset is not compatible with the other assets in the timeline',
  'AssetsPanel.addAssetWarning': 'Subclipping on assets with different resolutions may cause resolution autoscaling.',
  'AssetsPanel.live': 'LIVE',
  'AssetsPanel.unknown': 'UNKNOWN',
  'AssetsPanel.minimGapNotMet': 'The asset duration must be greater than the minimum clip duration ({0} seconds)',
  'VideoPlayer.openAdvancedSettings': 'Advanced settings',
  'VideoPlayer.singleBitrate': 'Single-bitrate MP4 (rendered)',
  'VideoPlayer.multiBitrate': 'Multi-bitrate MP4 (rendered)',
  'VideoPlayer.dynamicManifest': 'Dynamic manifest filter',
  'VideoPlayer.ErrorWithMessage': 'There was an error in the video player, code {0}, message: {1}',
  'Common.cancel': 'Cancel',
  'Common.OK': 'OK',
  'AdvancedSettings.framerate': 'Frame rate',
  'Dropdown.select': 'Select an option...',
  'InputAsset.RemoveInput': 'Remove source',
  'Zoom.startTime': 'Start time',
  'Zoom.endTime': 'End time',
  'VideoPlayer.subclips': 'Subclips:',
  'VideoPlayer.length': 'Clip length:',
  'Accordion.scrollLeft': 'Scroll to the left',
  'Accordion.scrollRight': 'Scroll to the right',
  'AdvancedSettings.title': 'Advanced settings',
  'AdvancedSettings.subclipName': 'Subclip name',
  'AdvancedSettings.subclipType': 'Subclipping mode',
  'AdvancedSettings.includeAudioTracks': 'Include audio tracks',
  'AdvancedSettings.subclipTypeInfo': 'Single-bitrate and multi-bitrate MP4s are frame accurate rendered assets. Dynamic manifest filters are group-of-pictures (GOP) accurate filters applied to a parent asset. Creating filters does not create a new asset and does not require encoding. Subclipping jobs on live assets are valid as long as their mark times are within the archive window of the parent asset. Filters are valid as long as the parent asset exists and mark times are within its archive window.',
  'AdvancedSettings.frameRateInfo': 'We autodetect frame rate under most scenarios. however, If we cannot autodetect, choose a frame rate from the dropdown for the selected asset(s).',
  'AdvancedSettings.frameRateError': 'Unable to determine frame rate',
  'AdvancedSettings.subclipNameInfo': 'Choose a name for your subclip.',
  'AdvancedSettings.singleAudioTrack': '1 audio track selected',
  'AdvancedSettings.allAudioTracks': 'All audio tracks selected',
  'AdvancedSettings.someAudioTracks': '{0} audio tracks selected',
  'AdvancedSettings.includeAllAudioTracks': 'Include all audio tracks',
  'AssetsPanel.loadingError': 'Failed to retreive assets from server.',
  'AssetsPanel.retry': 'Retry?',
  'CommandBar.prevFrameTitle': 'Back up one frame',
  'CommandBar.prevKeyFrameTitle': 'Back up one GOP',
  'CommandBar.cleanJob': 'Remove all assets',
  'CommandBar.cleanJobTitle': 'Remove all assets from timeline',
  'CommandBar.cleanJobMessage': 'This will empty all video clips from your timeline.',
  'CommandBar.update': 'Update filter',
  'CommandBar.createFilter': 'Create filter',
  'CommandBar.submit': 'Submit subclipper job',
  'CommandBar.jobErrorTitle': 'Operation failed',
  'CommandBar.jobErrorMessage': 'Your subclip failed to submit. Please try again.',
  'CommandBar.markInTitle': 'Set in at playhead',
  'CommandBar.markInPosition': 'Mark in timecode',
  'CommandBar.markOutTitle': 'Set out at playhead',
  'CommandBar.markOutPosition': 'Mark out timecode',
  'CommandBar.nextFrameTitle': 'Advance one frame',
  'CommandBar.nextKeyFrameTitle': 'Advance one GOP',
  'CommandBar.play': 'Play video',
  'CommandBar.pause': 'Pause video',
  'CommandBar.playPreviewTitle': 'Play subclip preview',
  'CommandBar.pausePreviewTitle': 'Pause subclip preview',
  'CommandBar.redoTitle': 'Redo last action',
  'CommandBar.removeAsset': 'Remove current asset',
  'CommandBar.undoTitle': 'Undo last action',
  'VideoPlayer.errorTitle': 'Error',
  'VideoPlayer.errorMessage': 'There was an error loading the selected asset.',
  'Timeline.markIn': 'Mark in bracket',
  'Timeline.markOut': 'Mark out bracket',
  'Timeline.playHead': 'Play head',
};
```