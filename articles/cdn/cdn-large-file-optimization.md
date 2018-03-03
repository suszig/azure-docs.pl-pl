---
title: "Optymalizacja pobierania dużych plików za pomocą usługi Azure CDN"
description: "Optymalizacja wyjaśniono szczegółowo pobieranie dużych plików"
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
ms.openlocfilehash: 6e6266fdaaac6a1a1a5d3a5595c10f79fd9f01a7
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="large-file-download-optimization-via-azure-cdn"></a>Optymalizacja pobierania dużych plików za pomocą usługi Azure CDN

Rozmiary plików zawartości dostarczane za pośrednictwem Internetu będzie się rozrastać z powodu rozszerzoną funkcjonalność, ulepszonemu i zawartość multimedialną. Wzrostu jest wymuszany przez wiele czynników: penetracji komórkowej łączności szerokopasmowej, większe urządzeń niedrogich magazynów, powszechnie zwiększenia wysokiej rozdzielczości wideo i podłączonej do Internetu urządzeń (IoT). Mechanizm dostarczania szybkich i wydajnych dużych plików jest krytyczne zapewnienie obsługi płynne i jest przyjemne konsumenta.

Dostarczanie dużych plików ma kilka problemów. Najpierw Średni czas pobierania dużych plików może być istotne, ponieważ aplikacji nie może pobrać wszystkie dane sekwencyjnie. W niektórych przypadkach aplikacje mogą pobierać ostatnia część pliku przed pierwszej części. Wymagany jest mała ilość pliku lub zatrzymaniu pobieranie, pobierania może zakończyć się niepowodzeniem. Pobieranie również mogą być opóźnione aż do po sieci dostarczania zawartości (CDN) pobiera całego pliku z serwera pochodzenia. 

Po drugie opóźnienia między komputerem użytkownika i plik Określa szybkość, jaką można wyświetlić zawartości. Ponadto sieci przeciążona i pojemności problemów miały wpływ na przepustowość. Większej odległości między serwerami i użytkownicy utworzyć dodatkowe możliwości utraty pakietów, występują, co zmniejsza jakości. Obniżenie jakości spowodowane ograniczonej przepustowości i utraty pakietów zwiększona może wydłużyć czas oczekiwania pobierania plików zakończyć. 

Trzecie wielu dużych plików nie są dostarczane w całości. Użytkownicy mogą anulować pobieranie w połowie lub Obejrzyj tylko pierwszy kilka minut długi wideo MP4. W związku z tym oprogramowania oraz nośnika firm dostarczania chce dostarczyć tylko część pliku, który jest wymagane. Efektywne dystrybucji żądanego porcji zmniejsza ruch wychodzący z serwera pochodzenia. Efektywne dystrybucji zmniejsza wykorzystanie we/wy na serwerze źródłowym i ilość pamięci. 

Azure Content Delivery Network zainstalowane from Akamai oferuje teraz funkcja, która dostarcza dużych plików wydajnie użytkownikom na całym świecie na dużą skalę. Funkcja zmniejsza opóźnienia, ponieważ zmniejsza obciążenie serwerów pochodzenia. Ta funkcja jest dostępna z Akamai standardowe, warstwy cenowej.

## <a name="configure-a-cdn-endpoint-to-optimize-delivery-of-large-files"></a>Konfigurowanie punktu końcowego usługi CDN, aby zoptymalizować dostarczanie dużych plików

Można skonfigurować punktu końcowego CDN do optymalizacji dostarczania dla dużych plików za pośrednictwem portalu Azure. Aby to zrobić, można użyć naszych interfejsów API REST lub dowolny z zestawów SDK klienta. W poniższej procedurze pokazano proces za pośrednictwem portalu Azure.

1. Aby dodać nowy punkt końcowy na **profilu CDN** wybierz pozycję **punktu końcowego**.

    ![Nowy punkt końcowy](./media/cdn-large-file-optimization/01_Adding.png)  
 
2. W **zoptymalizowane pod kątem** listy rozwijanej wybierz **pobierania plików o dużym**.

    ![Wybrane optymalizacji dużych plików](./media/cdn-large-file-optimization/02_Creating.png)


Po utworzeniu punktu końcowego CDN ma to zastosowanie optymalizacje dużych plików dla wszystkich plików spełniających określone kryteria. W poniższej sekcji opisano tego procesu.

## <a name="optimize-for-delivery-of-large-files-with-the-azure-content-delivery-network-from-akamai"></a>Optymalizuj pod kątem dostarczania dużych plików z usługi Azure Content Delivery Network from Akamai

Funkcja typu optymalizacji plików o dużym włącza optymalizacje sieci i konfiguracji, aby dostarczyć dużych plików szybciej i bardziej responsively. Dostarczania ogólne sieci web z Akamai przechowuje pliki tylko poniżej 1,8 GB i można tunelu (nie pamięci podręcznej) pliki do 150 GB. Optymalizacja dużych plików w pamięci podręcznej plików do 150 GB.

Optymalizacja dużych plików ma zastosowanie, gdy są spełnione określone warunki. Takie jak sposób działania serwera pochodzenia oraz rozmiarów i typów plików, które są wymagane. Aby uzyskać szczegółowe informacje na następujące tematy, uzyskujemy zapoznaj się działanie optymalizacji. 

### <a name="object-chunking"></a>Obiekt podziału 

Azure Content Delivery Network zainstalowane from Akamai stosowana jest metoda o nazwie obiektu podziału. Po zażądaniu dużego pliku CDN pobiera mniejsze części pliku ze źródła. Po CDN krawędzi/serwer protokołu POP odbiera żądanie pliku full lub zakresu bajtów, sprawdza, czy typ pliku jest obsługiwany dla tego rodzaju optymalizacji. Sprawdza również, czy typ pliku spełnia wymagania dotyczące rozmiaru pliku. Jeśli rozmiar pliku jest większy niż 10 MB, CDN serwer krawędzi żąda pliku ze źródła w fragmentów 2 MB. 

Po odebraniu fragmentów na brzegu sieci CDN ma w pamięci podręcznej i obsługiwane bezpośrednio do użytkownika. Usługa CDN następnie wstępnie wyłapuje dalej fragmentu równolegle. To pobieranie z wyprzedzeniem gwarantuje, że zawartość pozostaje jednym fragmencie wcześniejsze dla użytkownika, co zmniejsza opóźnienia. Ten proces jest kontynuowany do momentu cały plik jest pobierany (jeśli jest to wymagane), wszystkich zakresów bajtów dostępnych (jeśli jest to wymagane), lub klient zakończy połączenie. 

Aby uzyskać więcej informacji dotyczących żądania zakresu bajtów, zobacz [RFC 7233](https://tools.ietf.org/html/rfc7233).

Po odebraniu sieć CDN ma buforować wszystkie fragmenty. Cały plik nie ma być buforowane w pamięci podręcznej usługi CDN. Kolejne żądania dotyczące plików lub bajtów zakresów są obsługiwane z pamięci podręcznej usługi CDN. Jeśli nie wszystkie fragmenty są buforowane w sieci CDN, pobieranie z wyprzedzeniem służy do żądania fragmentów ze źródła. Tego rodzaju optymalizacji opiera się na serwerze źródłowym możliwość obsługi żądania zakresu bajtów. _Jeśli na serwerze źródłowym nie obsługuje żądania zakresu bajtów, optymalizacja nie jest skuteczne._ 

### <a name="caching"></a>Buforowanie
Optymalizacja dużych plików używa czas buforowania wygaśnięcia różne domyślne od dostarczania ogólne sieci web. Program rozróżnia buforowanie dodatnią i ujemną buforowanie na podstawie kodów odpowiedzi HTTP. Jeśli serwer pochodzenia określa czas wygaśnięcia za pośrednictwem nagłówka cache-control lub wygasa nagłówka w odpowiedzi, sieć CDN będzie honorować tej wartości. Gdy nie określono źródła i plik zgodny typ i rozmiar warunki dla tego typu optymalizacji, CDN używa domyślnych wartości dla optymalizacji dużych plików. W przeciwnym razie CDN używa ustawień domyślnych w celu dostarczania ogólne sieci web.


|    | Ogólne sieci web | Optymalizacja dużych plików 
--- | --- | --- 
Buforowanie: dodatnią <br> HTTP 200, 203, 300, <br> 301, 302 i 410 | 7 dni |1 dzień  
Buforowanie: ujemna <br> HTTP 204, 305, 404, <br> i 405 | None | 1 sekunda 

### <a name="deal-with-origin-failure"></a>Postępowania w przypadku niepowodzenia źródła

Długość limitu czasu odczytu pochodzenia zwiększa się z dwóch sekund w celu dostarczania ogólne sieci web do dwóch minut dla typu optymalizacji dużych plików. Liczbę kont większych plików uniknąć przedwczesnej limit czasu połączenia.

Jeśli upłynie limit czasu połączenia, sieć CDN ponowi próbę wiele razy przed wysłaniem ich do klienta błąd "504 — Limit czasu bramy". 

### <a name="conditions-for-large-file-optimization"></a>Warunki dla optymalizacji dużych plików

W poniższej tabeli wymieniono zestaw kryteriów należy spełnić w celu optymalizacji dużych plików:

Warunek | Wartości 
--- | --- 
Obsługiwane typy plików | 3g2, 3gp, asf, avi, bz2, dmg, exe, f4v, flv, <br> gz, hdp, iso, jxr, m4v, mkv, mov, mp4, <br> mpeg, mpg, mts, pkg, qt, rm, swf, tar, <br> tgz, wdp, webm, webp, wma, wmv, zip  
Minimalny rozmiar pliku | 10 MB 
Maksymalny rozmiar pliku | 150 GB 
Właściwości serwera pochodzenia | Musi obsługiwać żądania zakresu bajtów 

## <a name="optimize-for-delivery-of-large-files-with-the-azure-content-delivery-network-from-verizon"></a>Optymalizuj pod kątem dostarczania dużych plików z usługi Azure Content Delivery Network from Verizon

Azure Content Delivery Network zainstalowane from Verizon oferuje duże pliki bez ograniczenie rozmiaru pliku. Dodatkowe funkcje są włączone domyślnie, aby przyspieszyć dostarczanie dużych plików.

### <a name="complete-cache-fill"></a>Wypełnienie pamięci podręcznej ukończone

Funkcja wypełnienia pełną pamięci podręcznej domyślne umożliwia sieci CDN może kopiować plik w pamięci podręcznej podczas żądania początkowego jest porzucone lub utracony. 

Najbardziej przydatny w przypadku dużych zasobów jest wypełnienie pamięci podręcznej ukończone. Zwykle użytkownicy nie mogą pobierać je od początku do końca. Używają pobierania progresywnego. Domyślne zachowanie wymusza serwer graniczny, aby zainicjować pobieranie w tle zasobu z serwera pochodzenia. Później zasób znajduje się w lokalnej pamięci podręcznej serwera granicznego. Po pełnej obiektu w pamięci podręcznej, serwer graniczny spełnia żądania zakresu bajtów do sieci CDN dla obiektu w pamięci podręcznej.

Domyślne zachowanie można wyłączyć za pomocą aparatu reguł w warstwie Verizon Premium.

### <a name="peer-cache-fill-hot-filing"></a>Równorzędna pamięć podręczna wypełnienia hot zgłoszenia

Funkcja dynamicznego zgłoszenia domyślnych równorzędnej pamięci podręcznej wypełnienia używa zaawansowane algorytmu. Używa dodatkowe krawędzi buforowanie serwery oparte na przepustowość i agregacji żąda metryk do spełnienia żądania klienta dla dużych, bardzo popularny obiektów. Ta funkcja zapobiega sytuacji, w którym dużej liczby dodatkowych żądań są wysyłane do serwera pochodzenia użytkownika. 

### <a name="conditions-for-large-file-optimization"></a>Warunki dla optymalizacji dużych plików

Funkcje optymalizacji dla Verizon jest domyślnie włączona. Na maksymalny rozmiar pliku nie ma żadnych ograniczeń. 

## <a name="additional-considerations"></a>Dodatkowe zagadnienia

Należy wziąć pod uwagę następujące aspekty dodatkowych dla tego typu optymalizacji.
 
### <a name="azure-content-delivery-network-from-akamai"></a>Usługa Azure Content Delivery Network from Akamai

- Proces segmentu generuje dodatkowych żądań do serwera pochodzenia. Jednak ogólną ilość danych dostarczonych ze źródła jest znacznie mniejszy. Powoduje segmentu lepsze buforowania właściwości w sieci CDN.

- Pamięci i wykorzystanie we/wy zostały zredukowane w źródle, ponieważ są dostarczane mniejsze części pliku.

- Dla fragmentów w sieci CDN w pamięci podręcznej nie ma żadnych dodatkowych żądań do źródła, dopóki zawartość wygaśnie lub nie zostanie usunięty z pamięci podręcznej.

- Użytkownicy mogą wysyłać żądania zakresu do sieci CDN i traktowane jak normalne pliki. Optymalizacja ma zastosowanie tylko wtedy, gdy jest prawidłowy typ pliku i zakres bajtów jest od 10 MB do 150 GB. Średni rozmiar plików wymagane jest mniejsza niż 10 MB, możesz zamiast tego użyj dostarczania ogólne sieci web.

### <a name="azure-content-delivery-network-from-verizon"></a>Usługa Azure Content Delivery Network from Verizon

Typ optymalizacji dostarczania ogólne sieci web mogą dostarczać dużych plików.
