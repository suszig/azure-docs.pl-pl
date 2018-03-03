---
title: "Multimediów strumieniowych optymalizacji za pośrednictwem usługi Azure CDN"
description: "Optymalizacja strumieniowego przesyłania plików multimedialnych w celu dostarczania smooth"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: 3c42f54a99a85377ebe4df6959237f906d37591b
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="media-streaming-optimization-via-azure-cdn"></a>Multimediów strumieniowych optymalizacji za pośrednictwem usługi Azure CDN 
 
Użyj wideo wysokiej rozdzielczości zwiększa się w Internecie, co powoduje trudności dla dostarczania dużych plików. Klienci oczekują smooth odtwarzania wideo na żądanie lub live wideo zasobów na różnych klientów i sieci na całym świecie. Mechanizm dostarczania szybkich i wydajnych multimediów strumieniowych plików jest krytyczne zapewnienie obsługi płynne i jest przyjemne konsumenta.  

Multimediów strumieniowych na żywo jest szczególnie trudne do dostarczania ze względu na duże rozmiary i liczba równoczesnych przeglądarki. Długie opóźnienia, że użytkownicy pozostawić. Ponieważ strumienie na żywo nie można buforować wcześniejsze i duże opóźnienia nie są akceptowane przeglądarki, należy dostarczyć fragmenty wideo w odpowiednim czasie. 

Wzorce żądań strumieniowych zapewniają również pewne wyzwania. Po wydaniu popularnego strumień na żywo lub utworzenie nowej serii wideo na żądanie, tysięcy do milionów osób przeglądających może zażądać strumienia, w tym samym czasie. W takim przypadku żądanie inteligentnych konsolidacji jest nie przeciąży serwerów pochodzenia, kiedy zasoby nie są jeszcze buforowane.
 
Azure Content Delivery Network zainstalowane from Akamai oferuje teraz funkcja, która dostarcza przesyłania strumieniowego multimediów zasoby wydajnie użytkownikom na całym świecie na dużą skalę. Funkcja zmniejsza opóźnienia, ponieważ zmniejsza obciążenie serwerów pochodzenia. Ta funkcja jest dostępna z Akamai standardowe, warstwy cenowej. 

Azure Content Delivery Network zainstalowane from Verizon zapewnia multimediów strumieniowych bezpośrednio w typie optymalizacji dostarczania ogólne sieci web.
 
## <a name="configure-an-endpoint-to-optimize-media-streaming-in-the-azure-content-delivery-network-from-akamai"></a>Konfigurowanie punktu końcowego w celu zoptymalizowania multimediów strumieniowych w Azure Content Delivery Network zainstalowane from Akamai
 
Można skonfigurować punktu końcowego (CDN) sieci dostarczania zawartości w celu zoptymalizowania dostarczania dla dużych plików za pośrednictwem portalu Azure. Aby to zrobić, można użyć naszych interfejsów API REST lub dowolny z zestawów SDK klienta. W poniższej procedurze pokazano proces za pośrednictwem portalu Azure:

1. Aby dodać nowy punkt końcowy na **profilu CDN** wybierz pozycję **punktu końcowego**.
  
    ![Nowy punkt końcowy](./media/cdn-media-streaming-optimization/01_Adding.png)

2. W **zoptymalizowane pod kątem** listy rozwijanej wybierz **wideo na żądanie przesyłania strumieniowego multimediów** zasobów wideo na żądanie. Jeśli to zrobisz, kombinację na żywo i przesyłania strumieniowego wideo na żądanie, wybierz **przesyłania strumieniowego multimediów ogólne**.

    ![Wybrane przesyłania strumieniowego](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
Po utworzeniu punktu końcowego stosuje optymalizacji dla wszystkich plików spełniających określone kryteria. W poniższej sekcji opisano tego procesu. 
 
## <a name="media-streaming-optimizations-for-the-azure-content-delivery-network-from-akamai"></a>Przesyłanie strumieniowe optymalizacji dla usługi Azure Content Delivery Network zainstalowane from Akamai multimediów
 
Nośnika, który optymalizacji transmisji strumieniowej from Akamai ma zastosowanie w przypadku na żywo lub wideo multimediów strumieniowych używa nośnika poszczególne fragmenty dla dostawy. Ten proces różni się od pojedynczego zasobu dużych przesyłane przy użyciu pobierania progresywnego lub za pomocą żądania zakresu bajtów. Dla informacji o stylu dostarczania multimediów, zobacz [optymalizacji plików o dużym](cdn-large-file-optimization.md).


Ogólne nośnika dostarczania lub wideo na żądanie dostarczania optymalizacji typów nośników za pomocą CDN optymalizacje zaplecza do dostarczania multimediów zasoby szybciej. Konfiguracje korzysta również trwałych nośnika, na podstawie najlepszych rozwiązań rozpoznane w czasie.

### <a name="caching"></a>Buforowanie

Jeśli Azure Content Delivery Network zainstalowane from Akamai wykryje, że zasób manifestu przesyłania strumieniowego lub fragmentu, używa innej buforowania czas wygaśnięcia od dostarczania ogólne sieci web. (Zobacz pełną listę w poniższej tabeli). Jak zawsze honorowane cache-control lub Expires nagłówki wysyłane ze źródła. Jeśli element zawartości nie jest zasób nośnika, buforuje jest przy użyciu czas wygaśnięcia w celu dostarczania ogólne sieci web.

Krótki czas buforowania ujemna jest przydatna do odciążania źródła, gdy wielu użytkowników żądają fragmentu, która jeszcze nie istnieje. Przykładem jest strumień na żywo, gdy pakiety nie są dostępne ze źródła tego drugiego. Interwał buforowania już pomaga również w odciążania żądań ze źródła, ponieważ zawartość wideo zwykle nie jest zmodyfikowany.
 

|   | Ogólne dostarczanie w sieci Web | Ogólne transmisje strumieniowe multimediów | Przesyłanie strumieniowe multimediów wideo na żądanie  
--- | --- | --- | ---
Buforowanie: dodatnią <br> HTTP 200, 203, 300, <br> 301, 302 i 410 | 7 dni |365 dni | 365 dni   
Buforowanie: ujemna <br> HTTP 204, 305, 404, <br> i 405 | None | 1 sekunda | 1 sekunda
 
### <a name="deal-with-origin-failure"></a>Postępowania w przypadku niepowodzenia źródła  

Dostarczanie multimediów ogólne i dostarczanie multimediów wideo na żądanie mieć również limit czasu źródła i dziennika ponownych prób, na podstawie najlepszych rozwiązań dla żądania typowe wzorce. Na przykład, ponieważ dostarczania ogólne nośnika jest dla na żywo i dostarczania multimediów wideo na żądanie, używa krótszy limit czasu połączenia z powodu harmonogramów rodzaj transmisja strumieniowa na żywo.

Jeśli upłynie limit czasu połączenia, sieć CDN ponowi próbę wiele razy przed wysłaniem ich do klienta błąd "504 — Limit czasu bramy". 

Gdy plik jest zgodna z listy warunków typ i rozmiar pliku, sieć CDN używa zachowania do przesyłania strumieniowego multimediów. W przeciwnym razie używa dostarczania ogólne sieci web.
   
### <a name="conditions-for-media-streaming-optimization"></a>Warunki dotyczące multimediów strumieniowych optymalizacji 

W poniższej tabeli wymieniono zestaw kryteriów dotyczące multimediów strumieniowych optymalizacji: 
 
Obsługiwane typy przesyłania strumieniowego | Rozszerzenia plików  
--- | ---  
Apple HLS | m3u8, m3u, m3ub, klucza usług terminalowych, aac
Adobe obr. / min | f4m, f4x, drmmeta, bootstrap, f4f,<br>Adres URL seg Frag — struktura <br> (dopasowanie wyrażenia regularnego: ^(/.*)Seq(\d+)-Frag(\d+)
DASH | mpd, kreska, divx, ismv, m4s, m4v, mp4, mp4v, <br> sidx, webm, mp4a, m4a, isma
Funkcji Smooth streaming | / manifest/fragmenty/QualityLevels / /
  

 
## <a name="media-streaming-optimizations-for-the-azure-content-delivery-network-from-verizon"></a>Przesyłanie strumieniowe optymalizacji dla usługi Azure Content Delivery Network zainstalowane from Verizon multimediów

Azure Content Delivery Network zainstalowane from Verizon zapewnia przesyłania strumieniowego multimediów zasoby bezpośrednio, za pomocą typu optymalizacji dostarczania ogólne sieci web. Kilka funkcji w sieci CDN bezpośrednio pomagają w dostarczaniu zasoby nośnika domyślnie.

### <a name="partial-cache-sharing"></a>Udostępnianie częściowe pamięci podręcznej

Udostępnianie częściowe pamięci podręcznej umożliwia CDN z obsługą częściowo buforowane zawartości do nowych żądań. Na przykład jeśli pierwsze żądanie do sieci CDN powoduje Chybienie pamięci podręcznej, żądanie jest wysyłana do źródła. Chociaż ten niekompletną zawartość jest ładowany do pamięci podręcznej CDN, inne żądania do sieci CDN można uruchomić pobierania tych danych. 

### <a name="cache-fill-wait-time"></a>Czas oczekiwania wypełnienie pamięci podręcznej

 Funkcja czas oczekiwania wypełnienie pamięci podręcznej wymusza serwer graniczny, aby pomieścić wszystkie kolejne żądania dla tego samego zasobu, do momentu osiągnięcia nagłówków odpowiedzi HTTP z serwera pochodzenia. Jeśli nagłówki odpowiedzi HTTP z punktu początkowego napływają przed wygaśnięciem, wszystkie żądania, które zostały wstrzymane są obsługiwane z rosnącym pamięci podręcznej. W tym samym czasie pamięci podręcznej jest wypełniane przez dane ze źródła. Domyślnie czas oczekiwania wypełnienie pamięci podręcznej wynosi 3 000 milisekund. 

