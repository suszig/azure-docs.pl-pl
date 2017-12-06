---
title: "Porównanie Azure na żądanie nośnika koderów | Dokumentacja firmy Microsoft"
description: "W tym temacie porównuje kodowania możliwości ** Media Encoder Standard ** i ** Media Encoder Premium przepływu pracy **."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: a79437c0-4832-423a-bca8-82632b2c47cc
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 72b4a7b746d446e47b52cf34726a50dd52eaba97
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="comparison-of-azure-on-demand-media-encoders"></a>Porównanie Azure na żądanie nośnika koderów

W tym temacie porównuje kodowania możliwości **Media Encoder Standard** i **Media Encoder Premium w przepływie pracy**.

## <a name="video-and-audio-processing-capabilities"></a>Możliwości przetwarzania audio i wideo

W poniższej tabeli porównano funkcje między Media Encoder Standard (rynkowej) a Media Encoder Premium przepływu pracy (MEPW). 

|Możliwości|Usługa Media Encoder Standard|Przepływ pracy usługi Media Encoder w warstwie Premium|
|---|---|---|
|Zastosuj logikę warunkową podczas kodowania<br/>(na przykład, jeśli dane wejściowe są HD, następnie zakodować 5.1 audio)|Nie|Tak|
|Kodowane|Nie|[Tak](media-services-premium-workflow-encoder-formats.md#closed_captioning)|
|[Dolby® Professional głośności korekty](http://www.dolby.com/us/en/technologies/dolby-professional-loudness-solutions.pdf)<br/> z Intelligence™ dialog|Nie|Tak|
|Telecine usuwania przeplotu, odwrotność funkcji|Podstawowa|Jakość emisji|
|Wykryć i usunąć czarne ramki <br/>(pillarboxes letterboxes)|Nie|Tak|
|Generowanie miniatur|[Tak](media-services-dotnet-generate-thumbnail-with-mes.md)|[Tak](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)|
|Wycinka/przycinanie i łączenia plików wideo|[Tak](media-services-advanced-encoding-with-mes.md#trim_video)|Tak|
|Nakładki audio i wideo|[Tak](media-services-advanced-encoding-with-mes.md#overlay)|[Tak](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-1--overlay-an-image-on-top-of-the-video)|
|Nakładki grafiki|Od źródła obrazu|Od źródła obrazu i tekstu|
|Wiele wersji językowych audio|Ograniczone|[Tak](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-2--multiple-audio-language-encoding)|

## <a id="billing"></a>Licznik rozliczeń używane przez każdego kodera
| Nazwa procesora nośnika | Uzyskać odpowiednie ceny | Uwagi |
| --- | --- | --- |
| **Usługa Media Encoder Standard** |KODER |Kodowanie zadania zostanie naliczona opłata oparte na całkowity czas trwania (w minutach) wszystkie pliki multimedialne utworzone jako dane wyjściowe z szybkością określoną [tutaj][1], w kolumnie KODERA. |
| **Przepływ pracy usługi Media Encoder w warstwie Premium** |KODER PREMIUM |Kodowanie zadania zostanie naliczona opłata oparte na całkowity czas trwania (w minutach) wszystkie pliki multimedialne utworzone jako dane wyjściowe z szybkością określoną [tutaj][1], w kolumnie KODER PREMIUM. |

## <a name="input-containerfile-formats"></a>Kontener/formatów wejściowych
| Kontener/formatów wejściowych | Usługa Media Encoder Standard | Przepływ pracy usługi Media Encoder w warstwie Premium |
| --- | --- | --- |
| Adobe® Flash® F4V |Tak |Tak |
| MXF/SMPTE 377M |Tak |Tak |
| GXF |Tak |Tak |
| Strumienie transportu MPEG-2 |Tak |Tak |
| Strumienie programu MPEG-2 |Tak |Tak |
| MPEG-4/MP4 |Tak |Tak |
| ASF Media systemu Windows |Tak |Tak |
| AVI (nieskompresowanych 8-bitową/10 bity) |Tak |Tak |
| 3GPP/3GPP2 |Tak |Nie |
| Płynnego przesyłania strumieniowego Format pliku (PIFF 1.3) |Tak |Nie |
| [Microsoft cyfrowy wideo Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) |Tak |Nie |
| Matroska/WebM |Tak |Nie |
| QuickTime (mov) |Tak |Nie |

## <a name="input-video-codecs"></a>Dane wejściowe kodery-dekodery wideo
| Dane wejściowe kodery-dekodery wideo | Usługa Media Encoder Standard | Przepływ pracy usługi Media Encoder w warstwie Premium |
| --- | --- | --- |
| AVC 8-bitowych/10-bitowy, maksymalnie 4:2:2, w tym AVCIntra |8 bitów 4:2:0 a 4:2:2 |Tak |
| Avid DNxHD (w MXF) |Tak |Tak |
| DVCPro/DVCProHD (w MXF) |Tak |Tak |
| JPEG2000 |Tak |Tak |
| MPEG-2 (maksymalnie 422 profilu i wysokiego poziomu, m.in wariantów, takich jak XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10) |Do 422 profilu |Tak |
| MPEG-1 |Tak |Tak |
| Windows Media wideo/VC-1 |Tak |Tak |
| Canopus HQ/HQX |Nie |Nie |
| MPEG-4 część 2 |Tak |Nie |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Tak |Nie |
| Apple ProRes 422 |Tak |Nie |
| Apple ProRes 422 LT |Tak |Nie |
| HQ ProRes 422 firmy Apple |Tak |Nie |
| Serwer Proxy ProRes firmy Apple |Tak |Nie |
| Apple ProRes 4444 |Tak |Nie |
| XQ 4444 ProRes firmy Apple |Tak |Nie |

## <a name="input-audio-codecs"></a>Dane wejściowe audio koderów-dekoderów
| Dane wejściowe Audio koderów-dekoderów | Usługa Media Encoder Standard | Przepływ pracy usługi Media Encoder w warstwie Premium |
| --- | --- | --- |
| AES (SMPTE 331 M i 302 M, AES3 2003) |Nie |Tak |
| Dolby® E |Nie |Tak |
| Dolby® cyfrowy (AC3) |Nie |Tak |
| Cyfrowy Dolby® Plus (E-AC3) |Nie |Tak |
| AAC (AAC-LC, AAC HE i AAC-HEv2; maksymalnie 5.1) |Tak |Tak |
| MPEG warstwy 2 |Tak |Tak |
| Mp3 (MPEG-1 Audio warstwy 3) |Tak |Tak |
| Program Windows Media Audio |Tak |Tak |
| WAV/PCM |Tak |Tak |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Tak |Nie |
| [Dziele](https://en.wikipedia.org/wiki/Opus_\(audio_format\)) |Tak |Nie |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Tak |Nie |

## <a name="output-containerfile-formats"></a>Kontener i plik formatów
| Kontener i plik formatów | Usługa Media Encoder Standard | Przepływ pracy usługi Media Encoder w warstwie Premium |
| --- | --- | --- |
| Adobe® Flash® F4V |Nie |Tak |
| MXF (OP1a, XDCAM i AS02) |Nie |Tak |
| DPP (w tym AS11) |Nie |Tak |
| GXF |Nie |Tak |
| MPEG-4/MP4 |Tak |Tak |
| MPEG-TS |Tak |Tak |
| ASF Media systemu Windows |Nie |Tak |
| AVI (nieskompresowanych 8-bitową/10 bity) |Nie |Tak |
| Płynnego przesyłania strumieniowego Format pliku (PIFF 1.3) |Nie |Tak |

## <a name="output-video-codecs"></a>Dane wyjściowe kodery-dekodery wideo
| Dane wyjściowe kodery-dekodery wideo | Usługa Media Encoder Standard | Przepływ pracy usługi Media Encoder w warstwie Premium |
| --- | --- | --- |
| AVC (H.264; 8-bitową; do profilu wysokiego poziomu 5.2; 4 K bardzo HD; Wewnątrz AVC) |Tylko 8 bitów 4:2:0 |Tak |
| HEVC (H.265; 8-bitową i 10-bitowy)  |Nie |Tak |
| Avid DNxHD (w MXF) |Nie |Tak |
| MPEG-2 (maksymalnie 422 profilu i wysokiego poziomu, m.in wariantów, takich jak XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10) |Nie |Tak |
| MPEG-1 |Nie |Tak |
| Windows Media wideo/VC-1 |Nie |Tak |
| Tworzenie miniatur JPEG |Tak |Tak |
| Tworzenie miniatur PNG |Tak |Tak |
| Tworzenie miniatur BMP |Tak |Nie |

## <a name="output-audio-codecs"></a>Dane wyjściowe audio koderów-dekoderów
| Dane wyjściowe Audio koderów-dekoderów | Usługa Media Encoder Standard | Przepływ pracy usługi Media Encoder w warstwie Premium |
| --- | --- | --- |
| AES (SMPTE 331 M i 302 M, AES3 2003) |Nie |Tak |
| Dolby® cyfrowy (AC3) |Nie |Tak |
| Dolby® Digital Plus (E-AC3) do 7.1 |Nie |Tak |
| AAC (AAC-LC, AAC HE i AAC-HEv2; maksymalnie 5.1) |Tak |Tak |
| MPEG warstwy 2 |Nie |Tak |
| Mp3 (MPEG-1 Audio warstwy 3) |Nie |Tak |
| Program Windows Media Audio |Nie |Tak |

>[!NOTE]
>Gdy na cyfrowe® Dolby (AC3), dane wyjściowe można zapisywać tylko do pliku ISO MP4.

## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Pokrewne artykuły:
* [Wykonywania zaawansowanych zadań kodowania dostosowując ustawienia standardu Media Encoder Standard](media-services-custom-mes-presets-with-dotnet.md)
* [Przydziały i ograniczenia](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
