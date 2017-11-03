---
title: Podstawowe raporty from Verizon | Dokumentacja firmy Microsoft
description: "Możesz wyświetlić wzorców użycia sieci CDN za pomocą następujących raportów: przepustowości, dane przesyłane, trafień, stany pamięci podręcznej, Stosunek trafień w pamięci podręcznej, przesłanych danych IPV4 i IPV6."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 5a0d9018-8bdb-48ff-84df-23648ebcf763
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e9ee0041061296e313b3372dce13b5b86b2369c8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="core-reports-from-verizon"></a>Raporty Core from Verizon

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Za pomocą raportów Core Verizon za pośrednictwem portalu zarządzania dla profilów Verizon, można wyświetlić wzorców użycia sieci CDN następujące raporty:

* Przepustowość
* Przesyłane dane
* Liczba trafień
* Stany pamięci podręcznej
* Stosunek trafień w pamięci podręcznej
* Przesyłane dane IPv4 i IPV6

## <a name="accessing-verizon-core-reports"></a>Uzyskiwanie dostępu do raportów Verizon Core
1. Blok profilu CDN, kliknij **Zarządzaj** przycisku.
   
    ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-reports/cdn-manage-btn.png)
   
    Zostanie otwarty w portalu zarządzania usługi CDN.
2. Umieść kursor nad **Analytics** , a następnie umieść kursor nad **raporty Core** wysuwane okno.  Kliknij żądany raport w menu.
   
    ![Portal zarządzania CDN — menu Raporty podstawowe](./media/cdn-reports/cdn-core-reports.png)

## <a name="bandwidth"></a>Przepustowość
Raport przepustowości składa się z tabeli wykresu i danych, która wskazuje użycie przepustowości dla protokołu HTTP i HTTPS w określonym czasie. Możesz wyświetlić użycie przepustowości przez wszystkie POP w sieci CDN lub określonego POP w sieci CDN. Ten raport umożliwia wyświetlenie impulsy ruchu i dystrybucji w sieci CDN POP w MB/s.

* Zaznacz wszystkie węzły krawędzi, aby wyświetlić ruch z wszystkich węzłów lub wybierz określonego regionu/węzła z listy rozwijanej.
* Wybierz zakres dat, aby wyświetlić dane dla bieżącej/ten tydzień/w tym miesiącu, itp. lub wprowadź niestandardowe daty, a następnie kliknij przycisk **Przejdź** się upewnić, że wybór jest aktualizowany.
* Możesz wyeksportować i pobrać dane, klikając ikonę arkusza programu excel, znajdujący się obok pola **Przejdź**.

Raport jest aktualizowany co pięć minut.

![Raport przepustowości](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Przesyłane dane
Ten raport składa się z tabeli wykresu i danych, która określić obciążenie ruchu HTTP i HTTPS w określonym czasie. Obciążenie ruchu można wyświetlić wszystkie POP w sieci CDN lub określonego protokołu POP. Ten raport umożliwia wyświetlenie impulsy ruchu i dystrybucji w sieci CDN POP w GB.

* Wybierz wszystkie węzły krawędzi widoczność ruchu z uwagi na wszystkie lub wybierz określone region/węzeł z listy rozwijanej.
* Wybierz zakres dat, aby wyświetlić dane dla bieżącej/ten tydzień/w tym miesiącu, itp. lub wprowadź niestandardowe daty, a następnie kliknij przycisk **Przejdź** się upewnić, że wybór jest aktualizowany.
* Możesz wyeksportować i pobrać dane, klikając ikonę arkusza programu excel, znajdujący się obok pola **Przejdź**.

Raport jest aktualizowany co pięć minut.

![Dane przesyłane raportu](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Trafienia (kodów stanu)
Ten raport zawiera opis dystrybucji kodów stanu żądania dla zawartości. Każde żądanie zawartości generuje kod stanu HTTP. Kod stanu w tym artykule opisano sposób POP krawędzi obsługi żądania. Na przykład kod stanu 2xx oznacza, że żądanie zostało pomyślnie obsłużył operację na kliencie, podczas kod stanu 4xx oznacza, że wystąpił błąd. Aby uzyskać więcej informacji o kodach stanu HTTP, zobacz [kodów stanu](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

* Wybierz zakres dat, aby wyświetlić dane dla bieżącej/ten tydzień/w tym miesiącu, itp. lub wprowadź niestandardowe daty, a następnie kliknij przycisk **Przejdź** się upewnić, że wybór jest aktualizowany.
* Możesz wyeksportować i pobrać dane, klikając arkuszu programu excel, znajdujący się obok **Przejdź**.

![Liczba raportów](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Stan pamięci podręcznej
Ten raport zawiera opis dystrybucji trafień w pamięci podręcznej i Chybienia w pamięci podręcznej dla żądania klienta. Ponieważ największą wydajność pochodzi z trafień w pamięci podręcznej, aby zoptymalizować szybkości dostarczania danych, minimalizując Chybienia pamięci podręcznej i trafień w pamięci podręcznej wygasła. Chybienia pamięci podręcznej można zmniejszyć przez skonfigurowanie serwera pochodzenia można przypisywać nagłówki odpowiedzi "no-cache", unikając buforowanie ciągu zapytania z wyjątkiem, w razie potrzeby ściśle i dzięki unikaniu kody odpowiedzi buforowalnej. Można uniknąć trafień w pamięci podręcznej wygasłe dokonując zasobów max wieku tak długo, jak można zminimalizować liczbę żądań do serwera pochodzenia.

![Raport stany pamięci podręcznej](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Stany głównej pamięci podręcznej obejmują:
* TCP_HIT: Obsługiwane od krawędzi. Obiekt został w pamięci podręcznej i nie przekroczył jego maksymalny wiek.
* TCP_MISS: Obsługiwane pochodzenia. Nie znaleziono w pamięci podręcznej i odpowiedź była powrotem do źródła.
* TCP_EXPIRED _MISS: źródłem pochodzenia po ponowna Walidacja ze źródła. Obiekt był w pamięci podręcznej, ale przekroczył jego maksymalny wiek. Ponowna Walidacja ze źródła spowodowała obiektu pamięci podręcznej, zostanie zastąpiony nową odpowiedź od źródła.
* TCP_EXPIRED _HIT: pochodzący z krawędzi po ponowna Walidacja ze źródła. Obiekt się w pamięci podręcznej, ale przekroczył jego maksymalny wiek. Ponowna Walidacja w serwerze źródłowym w wyniku obiektu pamięci podręcznej jest nie mają być modyfikowane.
* Wybierz zakres dat, aby wyświetlić dane dla bieżącej/ten tydzień/w tym miesiącu, itp. lub wprowadź niestandardowe daty, a następnie kliknij przycisk **Przejdź** się upewnić, że wybór jest aktualizowany.
* Możesz wyeksportować i pobrać dane, klikając ikonę arkusza programu excel, znajdujący się obok pola **Przejdź**.

### <a name="full-list-of-cache-statuses"></a>Pełna lista stanów pamięci podręcznej
* TCP_HIT — ten stan jest zgłaszany, gdy żądanie jest udostępniane bezpośrednio z punktu obecności klienta. Zasób jest udostępniany z punktu obecności natychmiast, kiedy są buforowane na POP najbliżej klienta i ma prawidłową time to live (TTL). Czas wygaśnięcia jest określany przez następujące nagłówki odpowiedzi:
  
  * Cache-Control: s-maxage
  * Cache-Control: Maksymalna liczba wieku
  * Wygasa
* TCP_MISS — ten stan wskazuje, że wersja buforowana żądanego zasobu nie został znaleziony na POP najbliżej klienta. Element zawartości zażąda serwera pochodzenia lub serwer tarczy pochodzenia. Jeśli serwer pochodzenia lub serwer tarczy pochodzenia zwraca zasób, jest udostępniane przez klienta i w pamięci podręcznej na kliencie i serwerze krawędzi. W przeciwnym razie kod stanu – 200 (na przykład 403 zabronione lub 404 Nie znaleziono) jest zwracany.
* _HIT TCP_EXPIRED — ten stan jest zgłaszany, gdy przeznaczonego dla zasobu TTL wygasłe żądanie zostało obsłużone bezpośrednio z punktu obecności do klienta. Na przykład, jeśli element zawartości elementu maksymalny wiek wygasła. 
  
    Wygasłe żądanie zwykle powoduje żądanie ponownego sprawdzania poprawności do serwera pochodzenia. Dla _HIT TCP_EXPIRED nastąpić serwerze źródłowym musi wskazywać nowsza wersja zasobów nie istnieje. Ta sytuacja zwykle powoduje aktualizację Cache-Control i Expires headers elementu zawartości.
* _MISS TCP_EXPIRED — ten stan jest zgłaszany, gdy nowszej wersji elementu wygasłe zawartości pamięci podręcznej jest udostępniany z POP do klienta. Ten stan występuje, jeśli wygasł czas TTL buforowanych zasobów (na przykład wygasł maksymalny wiek) i serwera pochodzenia zwraca nowsza wersja tego zasobu. Nowa wersja zasobu jest udostępniany klienta, a nie wersja buforowana. Ponadto są buforowane na serwerze granicznym i klienta.
* CONFIG_NOCACHE — ten stan wskazuje, że odbiorcy konfiguracyjne na naszych krawędzi POP uniemożliwił elementu zawartości pamięci podręcznej.
* Brak — ten stan wskazuje, czy Sprawdzanie aktualności zawartości pamięci podręcznej nie została wykonana.
* _MISS TCP_ CLIENT_REFRESH — ten stan jest zgłaszany, gdy klient HTTP, takie jak przeglądarki, wymusza POP, aby pobrać nową wersję starych zasobów z serwera pochodzenia krawędzi.
  
    Domyślnie serwery zapobiega HTTP klienta wymuszania nasze serwery krawędzi, aby pobrać nową wersję elementu zawartości z serwera pochodzenia.
* PARTIAL_HIT TCP_ — ten stan jest zgłaszany, gdy żądanie zakresu bajtów powoduje trafień dla częściowo buforowane zasobów. Żądany zakres bajtów jest natychmiast udostępniane przez punkt obecności klienta.
* UNCACHEABLE — ten stan jest zgłaszany, gdy zasobów Cache-Control i Expires nagłówki wskazują, że go mają nie być buforowane punktu obecności lub przez klienta HTTP. Te typy żądań są obsługiwane z serwera pochodzenia.

## <a name="cache-hit-ratio"></a>Stosunek trafień w pamięci podręcznej
Ten raport wskazuje procent żądań z pamięci podręcznej, które były przekazywane bezpośrednio z pamięci podręcznej.

Raport zawiera następujące informacje:

* Żądana zawartość została buforowane na POP najbliżej obiektu żądającego.
* Żądanie zostało obsłużone bezpośrednio z krawędzią naszej sieci.
* Żądanie nie wymagało ponownego sprawdzania poprawności w serwerze źródłowym.

Raport nie zawiera:

* Żądań odrzuconych z powodu kraju opcje filtrowania.
* Żądania dotyczące zasobów, których nagłówki wskazują, że nie powinny one zapisywane. Na przykład Cache-Control: prywatny, Cache-Control: nie-cache lub Pragma: nagłówków pamięci podręcznej nie zabrania buforowania zasobów.
* Żądania zakresu bajtów częściowo buforowane zawartości.

Formuła jest: (TRAFIEŃ TCP_ / (TCP_ TRAFIEŃ + TCP_MISS)) * 100

* Wybierz zakres dat, aby wyświetlić dane dla bieżącej/ten tydzień/w tym miesiącu, itp. lub wprowadź niestandardowe daty, a następnie kliknij przycisk **Przejdź** się upewnić, że wybór jest aktualizowany.
* Możesz wyeksportować i pobrać dane, klikając ikonę arkusza programu excel, znajdujący się obok pola **Przejdź**.

![Raport Stosunek trafień w pamięci podręcznej](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>Przesyłane dane IPv4 i IPV6
Ten raport przedstawia rozkład użycia ruchu IPV4 vs IPV6.

![Przesyłane dane IPv4 i IPV6](./media/cdn-reports/cdn-ipv4-ipv6.png)

* Wybierz zakres dat do wyświetlania danych dla bieżącej/ten tydzień/w tym miesiącu, itp., lub wprowadź niestandardowe daty.
* Następnie kliknij przycisk **Przejdź** się upewnić, że wybór jest aktualizowany.

## <a name="considerations"></a>Zagadnienia do rozważenia
Raporty mogą być generowane tylko w ciągu ostatnich 18 miesięcy.

