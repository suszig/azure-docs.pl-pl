---
title: "Zwiększenie wydajności przez kompresowanie plików w usłudze Azure CDN | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zwiększyć szybkość transferu plików i zwiększa wydajność obciążenia strony przez kompresowanie plików w usłudze Azure CDN."
services: cdn
documentationcenter: 
author: dksimpson
manager: akucer
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: mazha
ms.openlocfilehash: 77d889f5d56ed839665588cf359b73e0f9ad28b5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Zwiększenie wydajności przez kompresowanie plików w usłudze Azure CDN
Kompresja jest to prosta i skuteczna metoda zwiększenia szybkości transferu plików i zwiększyć strony obciążenia wydajność przez zmniejszenie rozmiaru pliku przed wysłaniem go z serwera. Zmniejsza kosztów przepustowości i zapewnia bardziej odpowiednie dla użytkowników.

Istnieją dwa sposoby, aby włączyć kompresję:

* Włącz kompresję na serwerze pochodzenia. W takim przypadku CDN przechodzi przez skompresowane pliki i dostarcza je klientom żądającym je.
* Włącz kompresję bezpośrednio na serwerze krawędzi CDN. W takim przypadku CDN kompresuje pliki i służy je dla użytkowników końcowych, nawet jeśli nie są kompresowane przez serwer pochodzenia.

> [!IMPORTANT]
> Zmiany konfiguracji sieci CDN może trochę potrwać propagację za pośrednictwem sieci. Aby uzyskać <b>Azure CDN from Akamai</b> profile, propagacji zazwyczaj kończy w obszarze jednej minuty.  Aby uzyskać <b>Azure CDN from Verizon</b> profile, propagacji zazwyczaj kończy w ciągu 90 minut. Jeśli podczas konfigurowania kompresji po raz pierwszy dla punktu końcowego CDN, należy wziąć pod uwagę oczekiwania 1 – 2 godz. przed Rozwiązywanie problemów z do upewnij się, że ustawienia kompresji zostaną rozpropagowane do lokalizacji POP.
> 
> 

## <a name="enabling-compression"></a>Włączanie kompresji
Warstwy standardowa i Premium CDN mają te same funkcje kompresji, ale różni się w interfejsie użytkownika. Aby uzyskać więcej informacji na temat różnic między warstwy standardowa i Premium usługi CDN, zobacz [Omówienie usługi Azure CDN](cdn-overview.md).

### <a name="standard-tier"></a>Warstwa standardowa
> [!NOTE]
> Ta sekcja dotyczy **Azure CDN Standard from Verizon** i **Azure CDN Standard from Akamai** profilów.
> 
> 

1. Na stronie profilu CDN wybierz punkt końcowy CDN, którą chcesz zarządzać.
   
    ![Punkty końcowe profilu CDN](./media/cdn-file-compression/cdn-endpoints.png)
   
    Zostanie otwarta strona punktu końcowego CDN.
2. Wybierz **kompresji**.

    ![Punkty końcowe profilu CDN](./media/cdn-file-compression/cdn-compress-select-std.png)
   
    Zostanie otwarta strona kompresji.
3. Wybierz **na** włączenie kompresji.
   
    ![Opcje kompresji CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. Użyj typów domyślnych format lub zmodyfikuj listę przez dodanie lub usunięcie typów formatu.
   
   > [!TIP]
   > Mimo że jest to możliwe, nie zaleca się zastosowanie kompresji w formatach skompresowany. Na przykład ZIP, MP3, MP4 lub JPG.
   > 
 
5. Po wprowadzeniu zmiany, wybierz **zapisać**.

### <a name="premium-tier"></a>Warstwa Premium
> [!NOTE]
> W tej sekcji dotyczą tylko **Azure CDN Premium from Verizon** profilów.
> 
> 

1. Na stronie profilu CDN wybierz **Zarządzaj**.
   
    ![Przycisk Zarządzaj profilu CDN](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Zostanie otwarty w portalu zarządzania usługi CDN.
2. Umieść kursor nad **HTTP dużych** , a następnie umieść kursor nad **ustawienia pamięci podręcznej** wysuwane okno. Wybierz **kompresji**.

    ![Wybieranie pliku kompresji](./media/cdn-file-compression/cdn-compress-select.png)
   
    Opcje kompresji są wyświetlane.
   
    ![Opcje kompresji pliku](./media/cdn-file-compression/cdn-compress-files.png)
3. Włącz kompresję, wybierając **włączyć kompresji,**. Wprowadź typy MIME chcesz kompresować jako listę rozdzielaną przecinkami (bez spacji) **typów plików** pole.
   
   > [!TIP]
   > Mimo że jest to możliwe, nie zaleca się zastosowanie kompresji w formatach skompresowany. Na przykład ZIP, MP3, MP4 lub JPG.
   > 
    
4. Po wprowadzeniu zmiany, wybierz **aktualizacji**.

## <a name="compression-rules"></a>Reguły kompresji

### <a name="azure-cdn-from-verizon-profiles-both-standard-and-premium"></a>Azure CDN from Verizon profile (standard i premium)

Aby uzyskać **Azure CDN from Verizon** profile, tylko odpowiednie pliki są skompresowane. Aby kwalifikować się do kompresji, plik musi:
- Być większa niż 128 bajtów.
- Być mniejszy niż 1 MB.
 
Obsługuje te profile **gzip** (GNU zip), **deflate**, **bzip2**, lub **br** kodowania (Brotli). Jeśli żądanie obsługuje więcej niż jeden typ kompresji, te typy kompresji mają pierwszeństwo przed Brotli kompresji.

Gdy żądanie dla zasobu Określa kodowanie Brotli (zawiera nagłówek HTTP `Accept-Encoding: br`) i z żądania powoduje Chybienie pamięci podręcznej, usługa Azure CDN wykonuje kompresji Brotli zasobu na serwerze źródłowym. Potem skompresowany plik jest udostępniany bezpośrednio z pamięci podręcznej.

### <a name="azure-cdn-from-akamai-profiles"></a>Azure CDN from Akamai profilów

Aby uzyskać **Azure CDN from Akamai** profile, wszystkie pliki kwalifikują się do kompresji. Niemniej jednak plik musi być typu MIME, który został [skonfigurowane kompresji](#enabling-compression).

Te profile obsługują tylko **gzip** kodowania. Gdy punkt końcowy profilu żąda **gzip** zakodowane pliki, zawsze żądanie ze źródła, niezależnie od tego, w żądaniu klienta. 

## <a name="compression-behavior-tables"></a>Kompresja tabel zachowanie
W poniższych tabelach opisano zachowanie kompresji Azure CDN dla każdego scenariusza:

### <a name="compression-is-disabled-or-file-is-ineligible-for-compression"></a>Kompresja jest wyłączona lub plik nie kwalifikuje się do kompresji
| Klient zażądał formatu (za pośrednictwem nagłówka Accept-Encoding) | Format pliku pamięci podręcznej | Odpowiedź z sieci CDN do klienta | Uwagi |
| --- | --- | --- | --- |
| Skompresowane |Skompresowane |Skompresowane | |
| Skompresowane |Nieskompresowanych |Nieskompresowanych | |
| Skompresowane |Nie w pamięci podręcznej |Skompresowane lub nieskompresowane |Zależy od źródła odpowiedzi |
| Nieskompresowanych |Skompresowane |Nieskompresowanych | |
| Nieskompresowanych |Nieskompresowanych |Nieskompresowanych | |
| Nieskompresowanych |Nie w pamięci podręcznej |Nieskompresowanych | |

### <a name="compression-is-enabled-and-file-is-eligible-for-compression"></a>Kompresja jest włączona i pliku nie kwalifikuje się do kompresji
| Klient zażądał formatu (za pośrednictwem nagłówka Accept-Encoding) | Format pliku pamięci podręcznej | Odpowiedź z sieci CDN do klienta | Uwagi |
| --- | --- | --- | --- |
| Skompresowane |Skompresowane |Skompresowane |Transcodes CDN pomiędzy obsługiwanych formatów |
| Skompresowane |Nieskompresowanych |Skompresowane |CDN wykonuje kompresji |
| Skompresowane |Nie w pamięci podręcznej |Skompresowane |CDN wykonuje kompresji, jeśli punkt początkowy zwraca nieskompresowanego pliku. **Usługi Azure CDN from Verizon** przekazuje nieskompresowanego pliku na pierwsze żądanie i kompresuje i buforuje plik dla kolejnych żądań. Pliki z Cache-Control: nie-cache nagłówka nigdy nie są kompresowane. |
| Nieskompresowanych |Skompresowane |Nieskompresowanych |Dekompresja wykonuje CDN |
| Nieskompresowanych |Nieskompresowanych |Nieskompresowanych | |
| Nieskompresowanych |Nie w pamięci podręcznej |Nieskompresowanych | |

## <a name="media-services-cdn-compression"></a>Kompresja CDN usługi multimediów
Punkty końcowe włączone do przesyłania strumieniowego multimediów usługi CDN kompresji jest domyślnie włączona dla następujących typów MIME: 
- application/vnd.ms-sstr+xml 
- Aplikacja/dash + xml
- application/vnd.apple.mpegurl
- application/f4m+xml. 

Nie można włączyć lub wyłączyć kompresję dla tych typów MIME przy użyciu portalu Azure.  

## <a name="see-also"></a>Zobacz także
* [Rozwiązywanie problemów związanych z kompresją pliku CDN](cdn-troubleshoot-compression.md)    

