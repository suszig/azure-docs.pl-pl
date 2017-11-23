---
title: "Zwiększenie wydajności przez kompresowanie plików w usłudze Azure CDN | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zwiększyć szybkość transferu plików i zwiększa wydajność obciążenia strony przez kompresowanie plików w usłudze Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: b4e27ec57543daed35811fff347f457b0dd2cd5c
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Zwiększenie wydajności przez kompresowanie plików w usłudze Azure CDN
Kompresja jest to prosta i skuteczna metoda zwiększenia szybkości transferu plików i zwiększyć strony obciążenia wydajność przez zmniejszenie rozmiaru pliku przed wysłaniem go z serwera. Zmniejsza kosztów przepustowości i zapewnia bardziej odpowiednie dla użytkowników.

Istnieją dwa sposoby, aby włączyć kompresję:

* Włącz kompresję na serwerze pochodzenia. W takim przypadku CDN przechodzi przez skompresowane pliki i dostarcza je klientom żądającym je.
* Włącz kompresję bezpośrednio na serwerze krawędzi CDN. W takim przypadku CDN kompresuje pliki i służy je dla użytkowników końcowych, nawet jeśli nie są kompresowane przez serwer pochodzenia.

> [!IMPORTANT]
> Zmiany konfiguracji sieci CDN może trochę potrwać propagację za pośrednictwem sieci.  Aby uzyskać <b>Azure CDN from Akamai</b> profile, propagacji zazwyczaj kończy w obszarze jednej minuty.  Aby uzyskać <b>Azure CDN from Verizon</b> profile, zmiany zwykle stosowane w ciągu 90 minut.  Jeśli podczas konfigurowania kompresji po raz pierwszy dla punktu końcowego CDN, należy wziąć pod uwagę oczekiwania 1 – 2 godz. przed Rozwiązywanie problemów z do upewnij się, że ustawienia kompresji zostaną rozpropagowane do lokalizacji POP.
> 
> 

## <a name="enabling-compression"></a>Włączanie kompresji
> [!NOTE]
> Warstwy standardowa i Premium CDN mają te same funkcje kompresji, ale różni się w interfejsie użytkownika.  Aby uzyskać więcej informacji na temat różnic między warstwy standardowa i Premium usługi CDN, zobacz [Omówienie usługi Azure CDN](cdn-overview.md).
> 
> 

### <a name="standard-tier"></a>Warstwa standardowa
> [!NOTE]
> Ta sekcja dotyczy **Azure CDN Standard from Verizon** i **Azure CDN Standard from Akamai** profilów.
> 
> 

1. Na stronie profilu CDN kliknij punkt końcowy CDN, którą chcesz zarządzać.
   
    ![Punkty końcowe profilu CDN](./media/cdn-file-compression/cdn-endpoints.png)
   
    Zostanie otwarta strona punktu końcowego CDN.
2. Kliknij przycisk **Konfiguruj** przycisku.
   
    ![Przycisk Zarządzaj profilu CDN](./media/cdn-file-compression/cdn-config-btn.png)
   
    Zostanie otwarta strona konfiguracji sieci CDN.
3. Włącz **kompresji**.
   
    ![Opcje kompresji CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. Użyj typów domyślnych lub zmodyfikuj listę i usuwając lub dodając typów plików.
   
   > [!TIP]
   > Mimo że jest to możliwe, nie zaleca się zastosowanie kompresji w formatach skompresowany. Na przykład ZIP, MP3, MP4 lub JPG.
   > 
 
5. Po wprowadzeniu zmian, kliknij przycisk **zapisać** przycisku.

### <a name="premium-tier"></a>Warstwa Premium
> [!NOTE]
> Ta sekcja dotyczy **Azure CDN Premium from Verizon** profilów.
> 
> 

1. Na stronie profilu CDN kliknij **Zarządzaj** przycisku.
   
    ![Przycisk Zarządzaj profilu CDN](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Zostanie otwarty w portalu zarządzania usługi CDN.
2. Umieść kursor nad **HTTP dużych** , a następnie umieść kursor nad **ustawienia pamięci podręcznej** wysuwane okno.  Polecenie **kompresji**.

    ![Wybieranie pliku kompresji](./media/cdn-file-compression/cdn-compress-select.png)
   
    Opcje kompresji są wyświetlane.
   
    ![Opcje kompresji pliku](./media/cdn-file-compression/cdn-compress-files.png)
3. Włącz kompresję, klikając **włączyć kompresji,** przycisk radiowy.  Wprowadź typy MIME chcesz kompresować jako listę rozdzielaną przecinkami (bez spacji) **typów plików** pola tekstowego.
   
   > [!TIP]
   > Mimo że jest to możliwe, nie zaleca się zastosowanie kompresji w formatach skompresowany. Na przykład ZIP, MP3, MP4 lub JPG.
   > 
4. Po wprowadzeniu zmian, kliknij przycisk **aktualizacji** przycisku.

## <a name="compression-rules"></a>Reguły kompresji
Te tabele zawierają opis usługi Azure CDN kompresji zachowanie dla każdego scenariusza.

> [!IMPORTANT]
> Aby uzyskać **Azure CDN from Verizon** profile są kompresowane tylko odpowiednich plików (Standard i Premium).  Aby kwalifikować się do kompresji, plik musi:
> 
> * Być większa niż 128 bajtów.
> * Być mniejszy niż 1 MB.
> 
> Obsługuje te profile **gzip** (GNU zip), **deflate**, **bzip2**, lub **br** kodowania (Brotli). Brotli kodowania, kompresji musi odbywać się w źródle. W przeglądarce klienta/musi wysłać żądania do kodowania Brotli i skompresowanego zasobów musi mieć skompresowane po stronie źródła najpierw. 

> [!IMPORTANT]
> Aby uzyskać **Azure CDN from Akamai** profile, wszystkie pliki kwalifikują się do kompresji. Niemniej jednak plik musi być typ MIME, który został [skonfigurowane kompresji](#enabling-compression).
> 
>Te profile obsługują tylko **gzip** kodowania. Gdy punkt końcowy profilu żąda **gzip** zakodowane pliki, zawsze żądanie ze źródła, niezależnie od tego, w żądaniu klienta. 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Kompresja wyłączona lub plik nie kwalifikuje się do kompresji
| Żądany format klienta (za pośrednictwem nagłówka Accept-Encoding) | Format pliku pamięci podręcznej | Odpowiedź z sieci CDN do klienta | Uwagi |
| --- | --- | --- | --- |
| Skompresowane |Skompresowane |Skompresowane | |
| Skompresowane |Nieskompresowanych |Nieskompresowanych | |
| Skompresowane |Nie w pamięci podręcznej |Skompresowane lub nieskompresowane |Zależy od źródła odpowiedzi |
| Nieskompresowanych |Skompresowane |Nieskompresowanych | |
| Nieskompresowanych |Nieskompresowanych |Nieskompresowanych | |
| Nieskompresowanych |Nie w pamięci podręcznej |Nieskompresowanych | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Kompresja włączona i pliku nie kwalifikuje się do kompresji
| Żądany format klienta (za pośrednictwem nagłówka Accept-Encoding) | Format pliku pamięci podręcznej | Odpowiedź z sieci CDN do klienta | Uwagi |
| --- | --- | --- | --- |
| Skompresowane |Skompresowane |Skompresowane |Transcodes CDN pomiędzy obsługiwanych formatów |
| Skompresowane |Nieskompresowanych |Skompresowane |CDN wykonuje kompresji |
| Skompresowane |Nie w pamięci podręcznej |Skompresowane |Jeśli punkt początkowy zwraca nieskompresowanych, CDN wykonuje kompresji.  **Usługi Azure CDN from Verizon** przekazuje nieskompresowanego pliku na pierwsze żądanie i kompresuje i buforuje plik dla kolejnych żądań.  Pliki z `Cache-Control: no-cache` nigdy nie są kompresowane nagłówka. |
| Nieskompresowanych |Skompresowane |Nieskompresowanych |Dekompresja wykonuje CDN |
| Nieskompresowanych |Nieskompresowanych |Nieskompresowanych | |
| Nieskompresowanych |Nie w pamięci podręcznej |Nieskompresowanych | |

## <a name="media-services-cdn-compression"></a>Kompresja CDN usługi multimediów
Dla punktów końcowych przesyłania strumieniowego multimediów usługi CDN włączone kompresji jest domyślnie włączona dla następujących typów zawartości: 
- Aplikacja/vnd.ms-sstr + xml 
- Aplikacja/dash + xml
- application/vnd.Apple.mpegurl
- Aplikacja/f4m + xml. 

Nie można włączyć lub wyłączyć kompresji dla wymienionych typów przy użyciu portalu Azure.  

## <a name="see-also"></a>Zobacz też
* [Rozwiązywanie problemów związanych z kompresją pliku CDN](cdn-troubleshoot-compression.md)    

