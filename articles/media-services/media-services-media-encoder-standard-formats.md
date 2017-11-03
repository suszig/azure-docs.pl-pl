---
title: "Koderów-dekoderów i formaty Media Encoder Standard"
description: "Ten temat zawiera omówienie koderów-dekoderów i formaty Media Encoder Standard."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: f334b1ce-2f56-4968-a019-f0a2b0016d9f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 1115408443e11c8b0d26b83217c5f63e4b6ba819
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="media-encoder-standard-formats-and-codecs"></a>Formaty i kodeki narzędzia Media Encoder Standard
Ten dokument zawiera listę typowych importu i eksportu formatów plików, które można używać z Media Encoder Standard.

## <a name="input-containerfile-formats"></a>Kontener/formatów wejściowych
| Formaty plików (rozszerzenia plików) | Obsługiwane |
| --- | --- | --- | --- |
| FLV (za pomocą koderów-dekoderów H.264 i AAC) (.flv) |Tak |
| MXF (.mxf) |Tak |
| GXF (.gxf) |Tak |
| PS MPEG2, MPEG2 TS 3GP (TS, PS, .3gp, .3gpp, mpg) |Tak |
| Program Windows Media Video (WMV) / ASF (WMV, .asf) |Tak |
| AVI (nieskompresowanych 8-bitową/10 bitów) (AVI) |Tak |
| MP4 (plik MP4, .m4a, M4V) / ISMV (.isma, .ismv) |Tak |
| [Microsoft cyfrowy wideo Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (dvr ms) |Tak |
| Matroska/WebM (.mkv) |Tak |
| WAVE/WAV (wav) |Tak |
| QuickTime (mov) |Tak |

> [!NOTE]
> Powyżej znajduje się lista rozszerzeń plików najczęściej spotykanych. Media Encoder Standard obsługi wielu innych (na przykład: .m2ts, .mpeg2video, .qt). Jeśli spróbujesz kodowania pliku i otrzymasz komunikat o błędzie dotyczący format nie jest obsługiwane, Prześlij opinię [tutaj](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).
> 
> 

### <a name="audio-formats-in-input-containers"></a>Formaty audio w kontenerach wejściowych
Media Encoder Standard obsługuje wykonywanie następujących formatów audio w kontenerach wejściowych:

* MXF, GXF i QuickTime pliki, które mają ścieżki audio z przeplotem stereo lub 5.1 próbki

lub

* Pliki MXF, GXF i QuickTime dźwięk jest przenoszona jako oddzielne ścieżki PCM, ale dla których kanału mapowanie programu (stereo lub 5.1) można określić na podstawie metadanych pliku

Należy zwrócić uwagę obsługujące dla mapowania kanałów jawne/dostarczone przez użytkownika będą udostępniane w najbliższej przyszłości.

## <a name="input-video-codecs"></a>Dane wejściowe kodery-dekodery wideo
| Dane wejściowe kodery-dekodery wideo | Obsługiwane |
| --- | --- | --- | --- |
| AVC 8-bitowych/10-bitowy, maksymalnie 4:2:2, w tym AVCIntra |8 bitów 4:2:0 a 4:2:2 |
| Avid DNxHD (w MXF) |Tak |
| DVCPro/DVCProHD (w MXF) |Tak |
| Cyfrowy wideo (w plikach AVI) |Tak |
| JPEG 2000 |Tak |
| MPEG-2 (maksymalnie 422 profilu i wysokiego poziomu, m.in wariantów, takich jak XDCAM, XDCAM HD, XDCAM IMX, CableLabs® i D10) |Do 422 profilu |
| MPEG-1 |Tak |
| VC-1/WMV9 |Tak |
| Canopus HQ/HQX |Nie |
| MPEG-4 część 2 |Tak |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Tak |
| YUV420 nieskompresowane lub mezzanine |Tak |
| Apple ProRes 422 |Tak |
| Apple ProRes 422 LT |Tak |
| HQ ProRes 422 firmy Apple |Tak |
| Serwer Proxy ProRes firmy Apple |Tak |
| Apple ProRes 4444 |Tak |
| XQ 4444 ProRes firmy Apple |Tak |

## <a name="input-audio-codecs"></a>Dane wejściowe Audio koderów-dekoderów
| Dane wejściowe Audio koderów-dekoderów | Obsługiwane |
| --- | --- | --- | --- |
| AAC (AAC-LC, AAC HE i AAC-HEv2; maksymalnie 5.1) |Tak |
| MPEG warstwy 2 |Tak |
| Mp3 (MPEG-1 Audio warstwy 3) |Tak |
| Program Windows Media Audio |Tak |
| WAV/PCM |Tak |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Tak |
| [Dziele](http://go.microsoft.com/fwlink/?LinkId=822667) |Tak |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Tak |
| AMR (adaptacyjnej szybkości wielu) |Tak |
| AES (SMPTE 331 M i 302 M, AES3 2003) |Nie |
| Dolby® E |Nie |
| Dolby® cyfrowy (AC3) |Nie |
| Cyfrowy Dolby® Plus (E-AC3) |Nie |

## <a name="output-formats-and-codecs"></a>Koderów-dekoderów i formatów wyjściowych
Poniższa tabela zawiera listę formatów koderów-dekoderów i plików, które są obsługiwane w przypadku eksportowania.

| Format pliku | Kodera-dekodera wideo | Kodera-dekodera audio |
| --- | --- | --- |
| MP4 <br/><br/>(w tym kontenery MP4 wielokrotnej szybkości transmisji bitów) |H.264 (wysoki, Main i profile linii bazowej) |AAC-LC, HE-AAC v1, v2 HE-AAC |
| MPEG2 TS |H.264 (wysoki, Main i profile linii bazowej) |AAC-LC, HE-AAC v1, v2 HE-AAC |

## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zobacz też
[Kodowanie zawartości na żądanie za pomocą usługi Azure Media Services](media-services-encode-asset.md)

[Sposób kodowania przy użyciu Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

