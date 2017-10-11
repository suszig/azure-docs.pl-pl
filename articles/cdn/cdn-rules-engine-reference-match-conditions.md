---
title: "Aparat dopasowania warunki zasady usługi Azure CDN | Dokumentacja firmy Microsoft"
description: "Dokumentacja referencyjna dla usługi Azure CDN zasady warunków dopasowania aparatu i funkcje."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: f4886b1d78dfa87cf25737fb46c12b5963034f27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Pasujące aparatu reguł usługi Azure CDN
Ten temat zawiera szczegółowe opisy dostępnych warunków dopasowania dla Azure Content Delivery Network (CDN) [aparatu reguł](cdn-rules-engine.md).

Druga część reguły jest warunkiem dopasowania. Warunek dopasowania identyfikuje określone typy żądań, dla których będą wykonywane zestawem funkcji.

Na przykład może użyć do filtrowania żądań dotyczących zawartości w określonej lokalizacji, generowane z określonego adresu IP lub kraju lub przy użyciu informacji w nagłówku żądania.

## <a name="always"></a>Zawsze

Warunek dopasowania zawsze zaprojektowano w celu zastosowania domyślny zestaw funkcji do wszystkich żądań.

## <a name="device"></a>Urządzenie

Warunek zgodne urządzenie identyfikuje żądań wysyłanych z urządzenia przenośnego na podstawie jego właściwości.  Wykrywanie urządzenia przenośnego odbywa się za pośrednictwem [WURFL](http://wurfl.sourceforge.net/).  Poniżej przedstawiono możliwości WURFL oraz ich zmienne aparatu reguł CDN.

> [!NOTE] 
> Poniższe zmienne są obsługiwane w **modyfikowania nagłówka żądania klienta** i **zmodyfikować nagłówek odpowiedzi klienta** funkcji.

Możliwości | Zmienna | Opis | Przykładowe wartości
-----------|----------|-------------|----------------
Nazwa marki | % {wurfl_cap_brand_name} | Ciąg, który wskazuje nazwę urządzenia. | Samsung
System operacyjny urządzenia | % {wurfl_cap_device_os} | Ciąg, który wskazuje system operacyjny zainstalowany na urządzeniu. | DLA SYSTEMU IOS
Wersja systemu operacyjnego urządzenia | % {wurfl_cap_device_os_version} | Ciąg, który wskazuje numer wersji systemu operacyjnego zainstalowanego na urządzeniu. | 1.0.1
Podwójna orientacji | % {wurfl_cap_dual_orientation} | Wartość logiczna, która wskazuje, czy urządzenie obsługuje dwa orientacji. | Wartość true
HTML preferowane DTD | % {wurfl_cap_html_preferred_dtd} | Ciąg, który wskazuje definicja typu dokumentu preferowanych urządzeń przenośnych (DTD) dla zawartości HTML. | Brak<br/>xhtml_basic<br/>HTML5
Obraz ze Śródwierszowaniem | % {wurfl_cap_image_inlining} | Wartość logiczna wskazująca, czy urządzenie obsługuje Base64 zakodowany obrazów. | wartość false
Jest systemu Android | % {wurfl_vcap_is_android} | Wartość logiczna, która wskazuje, czy urządzenie używa system operacyjny Android. | Wartość true
IOS | % {wurfl_vcap_is_ios} | Wartość logiczna wskazująca, czy urządzenie korzysta z systemem iOS. | wartość false
Jest inteligentne TV | % {wurfl_cap_is_smarttv} | Wartość logiczna wskazująca, czy urządzenie jest inteligentne TV. | wartość false
Jest Smartphone | % {wurfl_vcap_is_smartphone} | Wartość logiczna wskazująca, czy urządzenie jest smartfona. | Wartość true
Jest typu Tablet | % {wurfl_cap_is_tablet} | Wartość logiczna wskazująca, czy urządzenie jest na komputerze typu tablet. Jest to opis niezależny od systemu operacyjnego. | Wartość true
To urządzenie sieci bezprzewodowej | % {wurfl_cap_is_wireless_device} | Wartość logiczna wskazująca, czy urządzenie jest uznawane za urządzeniem sieci bezprzewodowej. | Wartość true
Nazwa Marketing | % {wurfl_cap_marketing_name} | Ciąg, który wskazuje nazwę marketing dla urządzenia. | Perłowej blackBerry 8100
Przeglądarkę dla telefonów | % {wurfl_cap_mobile_browser} | Ciąg, który wskazuje przeglądarki używane do żądania zawartości od urządzenia. | Chrome
Wersja przenośnego przeglądarki | % {wurfl_cap_mobile_browser_version} | Ciąg, który wskazuje wersję przeglądarki używane do żądania zawartości od urządzenia. | 31
Nazwa modelu | % {wurfl_cap_model_name} | Ciąg, który wskazuje nazwę modelu urządzenia. | S3
Pobierania progresywnego | % {wurfl_cap_progressive_download} | Wartość logiczna, która wskazuje, czy urządzenie obsługuje odtwarzania audio i wideo, gdy nadal są pobierane. | Wartość true
Data wydania | % {wurfl_cap_release_date} | Ciąg, który wskazuje rok i miesiąc dodawania do bazy danych WURFL urządzenia.<br/><br/>Format:`yyyy_mm` | 2013_december
Wysokość rozwiązania | % {wurfl_cap_resolution_height} | Liczba całkowita, która wskazuje urządzenia wysokość w pikselach. | 768
Szerokość rozwiązania | % {wurfl_cap_resolution_width} | Liczba całkowita, która wskazuje szerokość urządzenia w pikselach. | 1024


## <a name="location"></a>Lokalizacja

Dopasowanie, te warunki zostały zaprojektowane do identyfikowania żądań na podstawie lokalizacji żądającego.

Nazwa | Przeznaczenie
-----|--------
JAKO liczba | Identyfikuje żądań pochodzących z określoną siecią.
Kraj | Identyfikuje żądań pochodzących z określonego krajów.


## <a name="origin"></a>Origin

Te warunki zostały zaprojektowane do identyfikowania odpowiada żądania prowadzące do sieci CDN w warstwie magazynu lub serwera pochodzenia klienta.

Nazwa | Przeznaczenie
-----|--------
Źródła usługi CDN | Identyfikuje żądania dotyczące zawartości przechowywanej w sieci CDN w warstwie magazynu.
Pochodzenie klienta | Identyfikuje żądania dotyczące zawartości przechowywanej na serwerze źródłowym określonego klienta.


## <a name="request"></a>Żądanie

Dopasowanie, te warunki zostały zaprojektowane do identyfikowania żądań na podstawie ich właściwości.

Nazwa | Przeznaczenie
-----|--------
Adres IP klienta | Identyfikuje żądań pochodzących z określonego adresu IP.
Parametr pliku cookie | Sprawdza, czy pliki cookie skojarzone z każdym żądaniem określoną wartość.
Wyrażenie regularne parametru pliku cookie | Sprawdza, czy pliki cookie skojarzone z każdym żądaniem dla określonego wyrażenia regularnego.
Krawędź Cname | Identyfikuje żądań, które wskazują określonej krawędzi CNAME.
Odwoływanie domeny | Identyfikuje żądań, które zostały przekazane z określonym hostname(s).
Literał nagłówka żądania | Identyfikuje żądań zawierających określony nagłówek równa określonej wartości.
Wyrażenie regularne nagłówka żądania | Identyfikuje żądań zawierających określony nagłówek ustawioną wartość, która odpowiada określonemu wyrażeniu regularnemu.
Symbol wieloznaczny nagłówka żądania | Identyfikuje żądań zawierających określony nagłówek ustawioną wartość zgodny z określonym wzorcem.
Request — metoda | Identyfikuje żądań ich metodą HTTP.
Schemat żądania | Identyfikuje żądań przez ich protokołu HTTP.

## <a name="url"></a>ADRES URL

Dopasowanie, te warunki zostały zaprojektowane do identyfikowania oparte na ich adresy URL żądania.

Nazwa | Przeznaczenie
-----|--------
Adres URL ścieżki katalogu | Identyfikuje żądania za pomocą ich ścieżek względnych.
Rozszerzenie ścieżki adresu URL | Identyfikuje żądania za pomocą ich rozszerzenia nazwy pliku.
Nazwa pliku ścieżki adresu URL | Identyfikuje żądań według ich nazwy pliku.
Literał ścieżki adresu URL | Porównuje żądania ścieżkę względną do określonej wartości.
Wyrażenie regularne ścieżki adresu URL | Porównuje żądania ścieżkę względną do określonego wyrażenia regularnego.
Symbol wieloznaczny ścieżki adresu URL | Porównuje żądania ścieżkę względną do określonego wzorca.
Adres URL zapytania literału | Porównuje ciąg zapytania z podaną wartością.
Adres URL parametru zapytania | Identyfikuje żądań zawierających parametr ciągu zapytania określona ustawioną wartość, która jest zgodna z określonym wzorcem.
Adres URL zapytania Regex | Identyfikuje żądań zawierających parametr ciągu zapytania określona wartość, który odpowiada określonemu wyrażeniu regularnemu.
Adres URL zapytania z symboli wieloznacznych | Porównuje określonej wartości z ciągu zapytania żądania.


## <a name="next-steps"></a>Następne kroki
* [Omówienie usługi Azure CDN](cdn-overview.md)
* [Odwołanie do aparatu reguł](cdn-rules-engine-reference.md)
* [Wyrażenia warunkowe aparatu reguł](cdn-rules-engine-reference-conditional-expressions.md)
* [Funkcje aparatu reguł](cdn-rules-engine-reference-features.md)
* [Zastępowanie domyślnego zachowania HTTP przy użyciu aparatu reguł](cdn-rules-engine.md)

