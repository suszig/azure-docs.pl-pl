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
ms.openlocfilehash: 0abbcf8508cb95d0fb8a9c8e5243426752efe590
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Pasujące aparatu reguł usługi Azure CDN
Ten temat zawiera szczegółowe opisy dostępnych warunków dopasowania dla Azure Content Delivery Network (CDN) [aparatu reguł](cdn-rules-engine.md).

Druga część reguły jest warunkiem dopasowania. Warunek dopasowania identyfikuje określone typy żądań, dla których będą wykonywane zestawem funkcji.

Na przykład może użyć do filtrowania żądań dotyczących zawartości w określonej lokalizacji, generowane z określonego adresu IP lub kraju lub przy użyciu informacji w nagłówku żądania.

## <a name="always"></a>Zawsze

Warunek dopasowania zawsze zaprojektowano w celu zastosowania domyślny zestaw funkcji do wszystkich żądań.

## <a name="device"></a>Urządzenie

Warunek zgodne urządzenie identyfikuje żądań wysyłanych z urządzenia przenośnego na podstawie jego właściwości.  Wykrywanie urządzenia przenośnego odbywa się za pośrednictwem [WURFL](http://wurfl.sourceforge.net/).  Poniżej przedstawiono możliwości WURFL oraz ich zmienne aparatu reguł CDN.
<br>
> [!NOTE] 
> Poniższe zmienne są obsługiwane w **modyfikowania nagłówka żądania klienta** i **zmodyfikować nagłówek odpowiedzi klienta** funkcji.

Możliwości | Zmienna | Opis | Przykładowe wartości
-----------|----------|-------------|----------------
Nazwa marki | % {wurfl_cap_brand_name} | Ciąg, który wskazuje nazwę urządzenia. | Samsung
System operacyjny urządzenia | % {wurfl_cap_device_os} | Ciąg, który wskazuje system operacyjny zainstalowany na urządzeniu. | iOS
Wersja systemu operacyjnego urządzenia | % {wurfl_cap_device_os_version} | Ciąg, który wskazuje numer wersji systemu operacyjnego zainstalowanego na urządzeniu. | 1.0.1
Podwójna orientacji | % {wurfl_cap_dual_orientation} | Wartość logiczna, która wskazuje, czy urządzenie obsługuje dwa orientacji. | prawda
HTML preferowane DTD | % {wurfl_cap_html_preferred_dtd} | Ciąg, który wskazuje definicja typu dokumentu preferowanych urządzeń przenośnych (DTD) dla zawartości HTML. | brak<br/>xhtml_basic<br/>HTML5
Obraz ze Śródwierszowaniem | % {wurfl_cap_image_inlining} | Wartość logiczna wskazująca, czy urządzenie obsługuje Base64 zakodowany obrazów. | fałsz
Jest systemu Android | % {wurfl_vcap_is_android} | Wartość logiczna, która wskazuje, czy urządzenie używa system operacyjny Android. | prawda
IOS | % {wurfl_vcap_is_ios} | Wartość logiczna wskazująca, czy urządzenie korzysta z systemem iOS. | fałsz
Jest inteligentne TV | % {wurfl_cap_is_smarttv} | Wartość logiczna wskazująca, czy urządzenie jest inteligentne TV. | fałsz
Jest Smartphone | % {wurfl_vcap_is_smartphone} | Wartość logiczna wskazująca, czy urządzenie jest smartfona. | prawda
Jest typu Tablet | % {wurfl_cap_is_tablet} | Wartość logiczna wskazująca, czy urządzenie jest na komputerze typu tablet. Jest to opis niezależny od systemu operacyjnego. | prawda
To urządzenie sieci bezprzewodowej | % {wurfl_cap_is_wireless_device} | Wartość logiczna wskazująca, czy urządzenie jest uznawane za urządzeniem sieci bezprzewodowej. | prawda
Nazwa Marketing | % {wurfl_cap_marketing_name} | Ciąg, który wskazuje nazwę marketing dla urządzenia. | Perłowej blackBerry 8100
Przeglądarkę dla telefonów | % {wurfl_cap_mobile_browser} | Ciąg, który wskazuje przeglądarki używane do żądania zawartości od urządzenia. | Chrome
Wersja przenośnego przeglądarki | % {wurfl_cap_mobile_browser_version} | Ciąg, który wskazuje wersję przeglądarki używane do żądania zawartości od urządzenia. | 31
Nazwa modelu | % {wurfl_cap_model_name} | Ciąg, który wskazuje nazwę modelu urządzenia. | S3
Pobierania progresywnego | % {wurfl_cap_progressive_download} | Wartość logiczna, która wskazuje, czy urządzenie obsługuje odtwarzania audio i wideo, gdy nadal są pobierane. | prawda
Data wydania | % {wurfl_cap_release_date} | Ciąg, który wskazuje rok i miesiąc dodawania do bazy danych WURFL urządzenia.<br/><br/>Format:`yyyy_mm` | 2013_december
Wysokość rozwiązania | % {wurfl_cap_resolution_height} | Liczba całkowita, która wskazuje urządzenia wysokość w pikselach. | 768
Szerokość rozwiązania | % {wurfl_cap_resolution_width} | Liczba całkowita, która wskazuje szerokość urządzenia w pikselach. | 1024


## <a name="location"></a>Lokalizacja

Dopasowanie, te warunki zostały zaprojektowane do identyfikowania żądań na podstawie lokalizacji żądającego.

Nazwa | Przeznaczenie
-----|--------
JAKO liczba | Identyfikuje żądań pochodzących z określoną siecią.
Kraj | Identyfikuje żądań pochodzących z określonego krajów.

### <a name="as-number"></a>JAKO liczba 
Ta sieć jest zdefiniowany przez jego numer ASN (Autonomous System). Opcja podano wskazująca, czy ten stan zostaną spełnione, gdy klient sieci "Pasuje" lub "Jest niezgodne" określony jako numer.

**Informacje o kluczu**
- Określić wiele numerów AS oddzielającego każdej z nich z jednego miejsca. Na przykład 64514 64515 dopasowuje żądań przychodzących od 64514 lub 64515.
- Niektórych żądań nie może zwracać prawidłowy jako numer. Znak zapytania (tj.,?) będzie odpowiadała żądania, dla których prawidłowy jako numer nie można określić.
- Całą jako numer dla żądanej sieci musi zostać określony. Nie będzie można dopasować wartości częściowe.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

### <a name="country"></a>Kraj
Kraj można określić za pomocą jego numer kierunkowy kraju. Podano opcję wskazująca, czy ten stan będzie spełniony, gdy kraj, z której pochodzi żądanie "Dopasowań" lub "Jest niezgodne" określonej wartości.


**Informacje o kluczu**
- Oddzielającego każdej z nich z jednego miejsca, aby określić wiele numerów kierunkowych krajów.
- Symbole wieloznaczne nie są obsługiwane podczas określania kodu kraju.
- Numerów kierunkowych "UE" i "Region" nie obejmują wszystkie adresy IP w regionach.
- Niektórych żądań nie może zwracać prawidłowy kod kraju. Znak zapytania (tj.,?) będzie odpowiadała żądania, dla których nie można określić prawidłowy kod kraju.
- Numerów kierunkowych jest rozróżniana wielkość liter.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

## <a name="origin"></a>Origin

Te warunki zostały zaprojektowane do identyfikowania odpowiada żądania prowadzące do sieci CDN w warstwie magazynu lub serwera pochodzenia klienta.

Nazwa | Przeznaczenie
-----|--------
Źródła usługi CDN | Identyfikuje żądania dotyczące zawartości przechowywanej w sieci CDN w warstwie magazynu.
Pochodzenie klienta | Identyfikuje żądania dotyczące zawartości przechowywanej na serwerze źródłowym określonego klienta.

### <a name="cdn-origin"></a>Źródła usługi CDN
To dopasowanie warunek jest spełniony, gdy są spełnione oba poniższe warunki:
- Zażądano zawartość z sieci CDN w warstwie magazynu.
- Identyfikator URI żądania korzysta z punktu dostępu do zawartości (np. /000001) zdefiniowana w tym stanie dopasowania.
  - Adres URL usługi CDN: Identyfikator URI żądania musi zawierać punkt dostępu do zawartości wybranego.
  - Adres URL CNAME krawędzi: Odpowiedniej konfiguracji CNAME krawędzi musi wskazywać dostęp do zawartości wybranego punktu.
  
*Uwagi:*
 - Punkt dostępu do zawartości identyfikuje usługę, która powinna obsługiwać żądanej zawartości.
 - Instrukcji IF i nie powinien umożliwia łączenie określone warunki dopasowania. Na przykład łączenie pochodzenia CDN warunek dopasowania z warunkiem dopasowania pochodzenia klienta utworzyć wzorca dopasowania, który nigdy nie być zgodne. Dla tego samego powodu dwa warunki dopasowania źródła usługi CDN nie można łączyć za pomocą instrukcji IF i.
 
### <a name="customer-origin"></a>Pochodzenie klienta

**Informacje o kluczu** 
- Ten warunek dopasowania będą spełnione niezależnie od tego, czy zawartość jest żądana, za pomocą CDN lub krawędzi CNAME URL wskazujące pochodzenie odbiorcy.
- Konfiguracja źródła klienta odwołuje się reguły nie mogą zostać usunięte ze strony klienta pochodzenia. Przed próbą usunięcia konfiguracji ze źródła — wersja klienta, upewnij się, czy następujące konfiguracje odwołuje się on:
  - Warunek dopasowania pochodzenia klienta
  - Konfiguracja CNAME krawędzi.
- Instrukcji IF i nie powinien umożliwia łączenie określone warunki dopasowania. Na przykład łączenie warunek dopasowania pochodzenia klienta z warunkiem dopasowania pochodzenia CDN utworzyć wzorca dopasowania, który nigdy nie być zgodne. Dla tego samego powodu dwa warunki zgodne ze źródła — wersja klienta nie można łączyć za pomocą instrukcji IF i.

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

### <a name="client-ip-address"></a>Adres IP klienta
Podano opcję wskazująca, czy ten stan zostaną spełnione, gdy klient obiektu IP adres "Pasuje" lub "Jest niezgodne" określonych adresów IP.

**Informacje o kluczu:**
- Upewnij się użyć notacji CIDR.
- Określ wiele adresów IP i/lub bloki adresów IP przez oddzielającego każdej z nich z jednego miejsca.
  - **Przykład IPv4:** 1.2.3.4 10.20.30.40 dopasowuje wszystkie żądania otrzymywanych 1.2.3.4 lub 10.20.30.40.
  - **Przykład IPv6:** 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 dopasowuje wszystkie żądania przychodzące z 1:2:3:4:5:6:7:8 lub 10:20:30:40:50:60:70:80.
- Składnia blok adresów IP to adres IP podstawowego następuje ukośnik i rozmiar prefiks.
  - **Przykład IPv4:** 5.5.5.64/26 dopasowuje wszystkie żądania przychodzące z 5.5.5.64 za pośrednictwem 5.5.5.127.
  - **Przykład IPv6:** 1:2:3: / 48 dopasowuje wszystkie żądania przychodzące z 1:2:3:0:0:0:0:0 za pośrednictwem 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

### <a name="cookie-parameter"></a>Parametr pliku cookie
**Odpowiada / niezgodny** opcja umożliwia określenie warunków, w którym znajduje się ten dopasować stan będą spełnione.
- **Dopasowania:** wymaga żądania zawiera określony plik cookie z wartością, która pasuje do co najmniej jedna z wartości zdefiniowanych w tym stanie dopasowania.
- **Niezgodna:** wymaga się, że żądanie spełniają jeden z następujących kryteriów:
  - Nie zawiera określonego pliku cookie.
  - Zawiera on określony plik cookie, ale jego wartość nie pasuje do żadnego z wartości zdefiniowanych w tym stanie dopasowania.
  
**Informacje o kluczu:**
- **Nazwa pliku cookie:** 
  - Znaki specjalne, łącznie z gwiazdką, nie są obsługiwane podczas określania nazwy pliku cookie. Oznacza to, że tylko cookie dokładnego dopasowania nazw kwalifikują się do porównania.
  - Można określić tylko nazwę jednego pliku cookie na każde wystąpienie tego warunku dopasowania.
  - Bez uwzględniania wielkości liter podczas porównywania nazw plików cookie.
- **Wartość pliku cookie:** 
  - Oddzielającego każdej z nich z jednego miejsca, aby określić wiele wartości pliku cookie.
  - Wartość pliku cookie mogą wykorzystać znaki specjalne. 
  - Jeśli nie został określony symbol wieloznaczny, dokładnego dopasowania zostanie spełnienia tego warunku dopasowania. 
   - **Przykład:** określenie "Wartość" będzie odpowiadała "Value", ale nie "Wartość1" lub "Wartość2".
  - **Ignoruj przypadku** opcja określa, czy porównania z uwzględnieniem wielkości liter będą wykonywane względem wartość pliku cookie żądania.
  - Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
   - Zakończenie wypełnienie pamięci podręcznej
   - Max-Age wewnętrzny domyślne
   - Wymuszanie wewnętrznych Max-Age.
   - Ignoruj pochodzenia No-Cache
   - Wewnętrzny odświeżona maksymalna

### <a name="cookie-parameter-regex"></a>Wyrażenie regularne parametru pliku cookie
Ten warunek dopasowania definiuje nazwę pliku cookie i wartość. Aby zdefiniować wartość żądanego pliku cookie może użyć wyrażeń regularnych. 

**Odpowiada / niezgodny** opcja umożliwia określenie warunków, w których będą spełnione ten warunek dopasowania.
- **Dopasowania:** wymaga żądania zawiera określony plik cookie z wartością, która odpowiada określonemu wyrażeniu regularnemu.
- **Niezgodna:** wymaga się, że żądanie spełniają jeden z następujących kryteriów:
  - Nie zawiera określonego pliku cookie.
  - Zawiera on określony plik cookie, ale jego wartość nie odpowiada określonemu wyrażeniu regularnemu.
  
**Informacje o kluczu:**
- **Nazwa pliku cookie:** 
  - Wyrażenia regularne i znaków specjalnych, łącznie z gwiazdką, nie są obsługiwane podczas określania nazwy pliku cookie. Oznacza to, że tylko cookie dokładnego dopasowania nazw kwalifikują się do porównania.
  - Można określić tylko nazwę jednego pliku cookie na każde wystąpienie tego warunku dopasowania.
  - Bez uwzględniania wielkości liter podczas porównywania nazw plików cookie.
- **Wartość pliku cookie:** 
  - Wartość pliku cookie mogą wykorzystać wyrażeń regularnych.
  - **Ignoruj przypadku** opcja określa, czy porównania z uwzględnieniem wielkości liter będą wykonywane względem wartość pliku cookie żądania.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

### <a name="edge-cname"></a>Krawędź Cname
**Informacje o kluczu** 
- Lista dostępnych krawędzi CNAME jest ograniczone do tych, które zostały skonfigurowane na stronie CNAME krawędzi odpowiadające platformie, na którym jest konfigurowane aparatu reguł HTTP.
- Przed próbą usunięcia konfiguracji CNAME krawędzi, upewnij się, czy warunek dopasowania krawędzi Cname nie odwołuje się on. Nie można usunąć krawędzi CNAME konfiguracje, które zostały zdefiniowane w regule ze strony CNAME krawędzi. 
- Instrukcji IF i nie powinien umożliwia łączenie określone warunki dopasowania. Na przykład łączenie warunek dopasowania krawędzi Cname z warunkiem dopasowania pochodzenia klienta utworzyć wzorca dopasowania, który nigdy nie być zgodne. Dla tego samego powodu dwa warunki dopasowania krawędzi Cname nie można łączyć za pomocą instrukcji IF i.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

### <a name="referring-domain"></a>Odwoływanie domeny
Nazwa hosta skojarzony z odwołania za pośrednictwem której zażądano zawartości określa, czy ten warunek jest spełniony. Podano opcję wskazująca, czy ten stan zostaną spełnione, gdy odwołujący się nazwa hosta "Odpowiada" lub "Nie odpowiada" określonej wartości.
**Informacje o kluczu:**
- Oddzielającego każdej z nich z jednego miejsca, aby określić wiele nazwy hostów.
- Ten warunek dopasowania obsługuje znaków specjalnych.
- Jeśli określona wartość nie zawiera znak gwiazdki, jego musi być dokładnego dopasowania dla odwołania nazwy hosta. Na przykład określenie "mojadomena.com" będzie niezgodna "www.mydomain.com."
- Opcja Ignoruj przypadku określa, czy będzie można wykonać porównania z uwzględnieniem wielkości liter.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna
  
 ### <a name="request-header-literal"></a>Literał nagłówka żądania
**Odpowiada / niezgodny** opcja umożliwia określenie warunków, w którym znajduje się ten dopasować stan będą spełnione.
- **Dopasowania:** wymaga, aby żądania zawierać określonego nagłówka i jego wartość musi odpowiadać nazwie zdefiniowane w tym stanie dopasowania.
- **Niezgodna:** wymaga się, że żądanie spełniają jeden z następujących kryteriów:
  - Nie zawiera określonego nagłówka.
  - Zawiera on określony nagłówek, ale jego wartość jest niezgodna z zdefiniowana w tym stanie dopasowania.
  
**Informacje o kluczu:**
- Bez uwzględniania wielkości liter podczas porównywania nazw nagłówka. Opcja uwzględniania wielkości liter porównania wartości nagłówka jest określana przez opcję Ignoruj Case.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna
  
### <a name="request-header-regex"></a>Wyrażenie regularne nagłówka żądania
**Uwaga:** tej możliwości wymaga aparatu reguł — zaawansowane zasady, które można kupić oddzielnie. Skontaktuj się z menedżerem ds. klientów CDN aby aktywować go. 

**Odpowiada / niezgodny** opcja umożliwia określenie warunków, w którym znajduje się ten dopasować stan będą spełnione.
- **Dopasowania:** wymaga, aby żądania zawierać określonego nagłówka i jego wartość musi odpowiadać wzorcowi określonemu w określonego wyrażenia regularnego.
- **Niezgodna:** wymaga się, że żądanie spełniają jeden z następujących kryteriów:
  - Nie zawiera określonego nagłówka.
  - Zawiera on określony nagłówek, ale jego wartość nie odpowiada określonemu wyrażeniu regularnemu.

**Informacje o kluczu:**
- Nazwa nagłówka: 
  - Bez uwzględniania wielkości liter podczas porównywania nazw nagłówka.
  - Spacje w nazwie nagłówka powinny być zastąpione przez "% 20". 
- Wartość nagłówka: 
  - Wartość nagłówka może korzystać z wyrażeń regularnych.
  - Opcja uwzględniania wielkości liter porównania wartości nagłówka jest określana przez opcję Ignoruj Case.
  - Tylko dokładne nagłówka wartość dopasowań, który ma co najmniej jeden z określonych wzorców będą spełniały warunki tego warunku.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna 

### <a name="request-header-wildcard"></a>Symbol wieloznaczny nagłówka żądania
**Odpowiada / niezgodny** opcja umożliwia określenie warunków, w którym znajduje się ten dopasować stan będą spełnione.
- **Dopasowania:** wymaga, aby żądania zawierać określonego nagłówka i jego wartość musi odpowiadać co najmniej jedna z wartości zdefiniowanych w tym stanie dopasowania.
- **Niezgodna:** wymaga się, że żądanie spełniają jeden z następujących kryteriów:
  - Nie zawiera określonego nagłówka.
  - Zawiera on określony nagłówek, ale jego wartość nie pasuje do żadnego z określonymi wartościami.
  
**Informacje o kluczu:**
- Nazwa nagłówka: 
  - Bez uwzględniania wielkości liter podczas porównywania nazw nagłówka.
  - Spacje w nazwie nagłówka powinny być zastąpione przez "% 20". Ta wartość może również określić spacje w wartości nagłówka.
- Wartość nagłówka: 
  - Wartość nagłówka, mogą wykorzystać znaki specjalne.
  - Opcja uwzględniania wielkości liter porównania wartości nagłówka jest określana przez opcję Ignoruj Case.
  - Tylko dokładne nagłówka wartość dopasowań, który ma co najmniej jeden z określonych wzorców będą spełniały warunki tego warunku.
  - Oddzielającego każdej z nich z jednego miejsca, aby określić wiele wartości.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

### <a name="request-method"></a>Request — metoda
Tylko zasoby, żądanych przy użyciu metody wybranego żądania będą spełniały warunki tego warunku. Sposoby żądanie dostępne są następujące:
- POBIERZ
- HEAD 
- POST 
- OPCJE 
- UMIEŚĆ 
- USUŃ 
- ŚLEDZENIA 
- POŁĄCZ 

**Informacje o kluczu:**
- Domyślnie tylko metodę żądania GET może generować zawartości w pamięci podręcznej w naszej sieci. Wszystkie inne metody żądania są po prostu proxy za pośrednictwem sieci.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

### <a name="request-scheme"></a>Schemat żądania
Tylko zasoby, które są żądane za pomocą wybranego protokołu będą spełniały warunki tego warunku. Dostępne protokoły są protokoły HTTP i HTTPS.

**Informacje o kluczu:**
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

## <a name="url"></a>Adres URL

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

