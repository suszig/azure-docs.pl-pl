---
title: "Utwórz klipy z funkcji tworzenia wycinków Azure Media | Dokumentacja firmy Microsoft"
description: "Omówienie usługi Azure Media funkcji tworzenia wycinków, narzędzia do tworzenia nośnika klipy z zasobów"
services: media-services
keywords: "klip subclip; kodowanie; nośnika"
author: dbgeorge
manager: jasonsue
ms.author: dwgeo
ms.date: 11/10/2017
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 6a5bb66db889dc1c52252773e3f129ba2c3d55f1
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="create-clips-with-azure-media-clipper"></a>Utwórz klipy z funkcji tworzenia wycinków multimediów Azure
Azure Media funkcji tworzenia wycinków jest bezpłatna biblioteka języka JavaScript, który umożliwia deweloperom sieci web użytkownikom ich interfejs służący do tworzenia nośnika klipy. To narzędzie można zintegrować dowolną stronę sieci web i udostępnia interfejsy API do ładowania zasobów i przesyłanie zadań wycinka.

Azure Media funkcji tworzenia wycinków umożliwia:
- TRIM łupków przed i po slate z na żywo archiwa 
- Redagowanie prezentuje wideo z wydarzeń na żywo usług AMS, archiwa na żywo lub fMP4 VOD plików 
- Łączenie filmy wideo z wieloma źródłami 
- Tworzy podsumowanie klipów zasobów nośnika AMS 
- Klip wideo z dokładnością ramki 
- Generowanie manifestu filtrów dynamicznych przez istniejące na żywo i VOD zasoby z dokładnością grupy z obrazów (GOP) 
- Tworzy kodowania zadań przed zasoby w Twojego konta usługi media services

Aby poprosić o nowe funkcje, podaj pomysłów lub opinie, przedstawia [UserVoice dla usługi Azure Media Services](http://aka.ms/amsvoice/). Jeśli masz i określonych problemów, pytania lub Znajdź usterek, Porzuć Media Services team linii w amcinfo@microsoft.com.

## <a name="release-notes"></a>Informacje o wersji
Na poniższej liście opisano różne znane problemy i wykaz zmian w najnowszej wersji programu funkcji tworzenia wycinków:
- [Lista znanych problemów](https://amp.azure.net/libs/amc/latest/docs/known_issues.html)
- [Wykaz zmian](https://amp.azure.net/libs/amc/latest/docs/changelog.html)

## <a name="browser-support"></a>Obsługa przeglądarek
Azure Media funkcji tworzenia wycinków jest utworzony przy użyciu nowoczesnych technologii HTML5 i obsługuje następujące przeglądarki:

- Przeglądarka Microsoft Edge 13 +
- Program Internet Explorer 11 +
- Chrome 54 +
- Safari 10 +
- Firefox 50 +

> [!NOTE]
> Tylko HTML5 odtwarzania strumieni z usługi Azure Media Services jest obecnie obsługiwane.

## <a name="language-support"></a>Obsługa języków
Widżet funkcji tworzenia wycinków jest dostępna w następujących językach 18:
- Chiński (uproszczony)
- Chiński (tradycyjny)
- Czeski
- Holenderski, flamandzki
- Polski
- Francuski
- Niemiecki
- Węgierski
- Włoski
- Japoński
- Koreański
- Polski
- Portugalski (Brazylia)
- Portugalski (Portugalia)
- Rosyjski
- Hiszpański
- Szwedzki
- Turecki

## <a name="next-steps"></a>Następne kroki
Aby rozpocząć korzystanie z funkcji tworzenia wycinków multimediów Azure, przeczytaj [wprowadzenie](media-services-azure-media-clipper-getting-started.md) artykułu, aby uzyskać więcej informacji na temat wdrażania elementu widget.