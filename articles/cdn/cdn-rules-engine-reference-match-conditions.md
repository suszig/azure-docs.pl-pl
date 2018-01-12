---
title: "Pasujące do aparatu reguł Azure CDN | Dokumentacja firmy Microsoft"
description: "Dokumentacja referencyjna dla usługi Azure Content Delivery Network zasady warunków dopasowania aparatu."
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
ms.date: 12/21/2017
ms.author: rli
ms.openlocfilehash: 9986e654b076df099e3912f9da628728723b5c3d
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="match-conditions-for-the-azure-cdn-rules-engine"></a>Warunki dopasowania dla aparatu reguł Azure CDN
W tym artykule przedstawiono szczegółowe opisy warunki dopasowania dostępne dla usługi Azure sieci dostarczania zawartości (CDN) [aparatu reguł](cdn-rules-engine.md).

Druga część reguły jest warunkiem dopasowania. Warunek dopasowania identyfikuje określone typy żądań, dla których będą wykonywane zestawem funkcji.

Na przykład można użyć warunku dopasowania do:
- Filtrowanie żądań dla zawartości w określonej lokalizacji.
- Filtrowanie żądań jest generowany na podstawie określonego adresu IP lub kraju.
- Filtruj żądania według informacje nagłówka.

## <a name="always-match-condition"></a>Zawsze odpowiadać warunku

Warunek dopasowania zawsze stosuje domyślny zestaw funkcji do wszystkich żądań.

Name (Nazwa) | Przeznaczenie
-----|--------
[Zawsze](#always) | Stosuje się domyślny zestaw funkcji do wszystkich żądań.

## <a name="device-match-condition"></a>Urządzenie zgodne warunku

Warunek zgodne urządzenie identyfikuje żądań wysyłanych z urządzenia przenośnego na podstawie jego właściwości.  

Name (Nazwa) | Przeznaczenie
-----|--------
[Urządzenia](#device) | Identyfikuje żądań wysyłanych z urządzenia przenośnego na podstawie jego właściwości.

## <a name="location-match-conditions"></a>Warunki dopasowania lokalizacji

Warunki dopasowania lokalizacji zidentyfikować żądania na podstawie lokalizacji żądającego.

Name (Nazwa) | Przeznaczenie
-----|--------
[JAKO liczba](#as-number) | Identyfikuje żądań pochodzących z określoną siecią.
[Kraju](#country) | Identyfikuje żądań pochodzących z określonego krajów.

## <a name="origin-match-conditions"></a>Warunki dopasowania źródła

Warunki dopasowania pochodzenia zidentyfikować żądań, które wskazują serwer pochodzenia klienta lub magazynu Content Delivery Network.

Name (Nazwa) | Przeznaczenie
-----|--------
[Źródła usługi CDN](#cdn-origin) | Identyfikuje żądania dotyczące zawartości przechowywanej w magazynie sieci dostarczania zawartości.
[Pochodzenie klienta](#customer-origin) | Identyfikuje żądania dotyczące zawartości przechowywanej na serwerze źródłowym określonego klienta.

## <a name="request-match-conditions"></a>Warunki dopasowania żądania

Warunki dopasowania żądania zidentyfikować żądania na podstawie ich właściwości.

Name (Nazwa) | Przeznaczenie
-----|--------
[Adres IP klienta](#client-ip-address) | Identyfikuje żądań pochodzących z określonego adresu IP.
[Parametr pliku cookie](#cookie-parameter) | Sprawdza, czy pliki cookie skojarzone z każdym żądaniem określoną wartość.
[Wyrażenie regularne parametru pliku cookie](#cookie-parameter-regex) | Sprawdza, czy pliki cookie skojarzone z każdym żądaniem dla określonego wyrażenia regularnego.
[Krawędź Cname](#edge-cname) | Identyfikuje żądań, które wskazują określonej krawędzi CNAME.
[Odwoływanie domeny](#referring-domain) | Identyfikuje żądań, które zostały przekazane z nazw określonych hostów.
[Literał nagłówka żądania](#request-header-literal) | Identyfikuje żądań zawierających określony nagłówek równa określonej wartości.
[Wyrażenie regularne nagłówka żądania](#request-header-regex) | Identyfikuje żądań zawierających określony nagłówek ustawioną wartość, która odpowiada określonemu wyrażeniu regularnemu.
[Symbol wieloznaczny nagłówka żądania](#request-header-wildcard) | Identyfikuje żądań zawierających określony nagłówek ustawioną wartość zgodny z określonym wzorcem.
[Request — metoda](#request-method) | Identyfikuje żądań ich metodą HTTP.
[Schemat żądania](#request-scheme) | Identyfikuje żądań przez ich protokołu HTTP.

## <a name="url-match-conditions"></a>Adres URL warunków dopasowania

Adres URL warunków dopasowania zidentyfikować oparte na ich adresy URL żądania.

Name (Nazwa) | Przeznaczenie
-----|--------
Adres URL ścieżki katalogu | Identyfikuje żądania za pomocą ich ścieżek względnych.
Rozszerzenie ścieżki adresu URL | Identyfikuje żądania za pomocą ich rozszerzenia nazwy pliku.
Nazwa pliku ścieżki adresu URL | Identyfikuje żądań według nazwy pliku.
Literał ścieżki adresu URL | Porównuje żądania ścieżkę względną do określonej wartości.
Wyrażenie regularne ścieżki adresu URL | Porównuje żądania ścieżkę względną do określonego wyrażenia regularnego.
Symbol wieloznaczny ścieżki adresu URL | Porównuje żądania ścieżkę względną do określonego wzorca.
Adres URL zapytania literału | Porównuje ciąg zapytania z podaną wartością.
Adres URL parametru zapytania | Identyfikuje żądań zawierających parametr ciągu zapytania określona ustawioną wartość, która jest zgodna z określonym wzorcem.
Adres URL zapytania Regex | Identyfikuje żądań zawierających parametr ciągu zapytania określona wartość, który odpowiada określonemu wyrażeniu regularnemu.
Adres URL zapytania z symboli wieloznacznych | Porównuje określonej wartości z ciągu zapytania żądania.


## <a name="reference-for-rules-engine-match-conditions"></a>Informacje dotyczące reguł aparatu dopasowania warunków

---
### <a name="always"></a>Zawsze

Warunek dopasowania zawsze stosuje domyślny zestaw funkcji do wszystkich żądań.

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="as-number"></a>JAKO liczba 
W sieci jako numer jest zdefiniowana za pomocą numeru systemu autonomicznego (ASN). Opcja podano wskazująca, czy ten stan zostaną spełnione, gdy klient sieci "Pasuje" lub "Jest niezgodne" określony numer ASN.

Informacje o kluczu:
- Oddzielającego każdej z nich z jednego miejsca, aby określić wiele numerów ASN. Na przykład 64514 64515 dopasowuje żądań który odbierane z 64514 lub 64515.
- Niektóre żądania mogą nie zwracać prawidłowy numer ASN. Znak zapytania (?) będzie pasował do żądania, dla których nie można określić prawidłowy numer ASN.
- Należy określić cały numer ASN dla żądanej sieci. Nie będzie można dopasować wartości częściowe.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cdn-origin"></a>Źródła usługi CDN
Pochodzenie CDN dopasowania warunek jest spełniony, gdy są spełnione oba poniższe warunki:
- Zażądano zawartość z magazynu sieci dostarczania zawartości.
- Żądanie identyfikatora URI używany punkt dostępu do zawartości (na przykład /000001), który jest zdefiniowany w tym stanie dopasowania.
  - Adres URL zawartości dostarczania sieci: Identyfikator URI żądania musi zawierać punkt dostępu do zawartości wybranego.
  - Adres URL CNAME krawędzi: Odpowiedniej konfiguracji CNAME krawędzi musi wskazywać dostęp do zawartości wybranego punktu.
  
Informacje o kluczu:
 - Punkt dostępu do zawartości identyfikuje usługę, która powinna obsługiwać żądanej zawartości.
 - Nie używaj instrukcji IF i połączyć określone warunki dopasowania. Na przykład łączenie pochodzenia CDN warunek dopasowania z warunkiem dopasowania pochodzenia klienta utworzyć wzorca dopasowania, który nigdy nie być zgodne. Z tego powodu dwa warunki dopasowania źródła usługi CDN nie można łączyć za pomocą instrukcji IF i.

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="client-ip-address"></a>Adres IP klienta
Opcja podano wskazująca, czy warunek adres IP klienta zostaną spełnione, gdy adres IP "Dopasowań" lub "Jest niezgodne" klienta adresów IP określonego.

Informacje o kluczu:
- Upewnij się użyć notacji CIDR.
- Określ wiele adresów IP i/lub bloki adresów IP przez oddzielającego każdej z nich z jednego miejsca.
  - **Przykład IPv4**: 1.2.3.4 10.20.30.40 dopasowuje wszystkie żądania przychodzące z 1.2.3.4 lub 10.20.30.40.
  - **Przykład IPv6**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 dopasowuje wszystkie żądania przychodzące z 1:2:3:4:5:6:7:8 lub 10:20:30:40:50:60:70:80.
- Składnia blok adresów IP to adres IP podstawowego następuje ukośnik i rozmiar prefiks.
  - **Przykład IPv4**: 5.5.5.64/26 dopasowuje wszystkie żądania przychodzące z 5.5.5.64 za pośrednictwem 5.5.5.127.
  - **Przykład IPv6**: 1:2:3: / 48 dopasowuje wszystkie żądania przychodzące z 1:2:3:0:0:0:0:0 za pośrednictwem 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter"></a>Parametr pliku cookie
**Dopasowań**/**jest niezgodny** opcja umożliwia określenie warunków, zgodnie z którymi parametr plik Cookie pasuje warunek zostanie spełniony.
- **Dopasowuje**: wymaga żądania zawiera określony plik cookie z wartością, która pasuje do co najmniej jednej wartości, które są zdefiniowane w tym stanie dopasowania.
- **Nie odpowiada**: wymaga się, że żądanie spełniają jeden z następujących kryteriów:
  - Nie zawiera określonego pliku cookie.
  - Zawiera on określony plik cookie, ale jego wartość nie pasuje do żadnej wartości, które są zdefiniowane w tym stanie dopasowania.
  
Informacje o kluczu:
- Nazwa pliku cookie: 
  - Znaki specjalne, łącznie z gwiazdką, nie są obsługiwane, gdy określasz, nazwę pliku cookie. Oznacza to, że tylko cookie dokładnego dopasowania nazw kwalifikują się do porównania.
  - Można określić tylko nazwę jednego pliku cookie na każde wystąpienie tego warunku dopasowania.
  - Bez uwzględniania wielkości liter podczas porównywania nazw plików cookie.
- Wartość pliku cookie: 
  - Oddzielającego każdej z nich z jednego miejsca, aby określić wiele wartości pliku cookie.
  - Wartość pliku cookie mogą wykorzystać znaki specjalne. 
  - Jeśli nie został określony symbol wieloznaczny, dokładnego dopasowania będą spełniały warunki tego warunku dopasowania. Na przykład określenie "Value" będzie odpowiadała "Value", ale nie "Wartość1" lub "Wartość2".
  - **Ignoruj przypadku** opcja określa, czy porównania z uwzględnieniem wielkości liter będą wykonywane względem wartość pliku cookie żądania.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter-regex"></a>Wyrażenie regularne parametru pliku cookie
Warunek dopasowanie wyrażenia regularnego parametru plików Cookie definiuje nazwę pliku cookie i wartość. Wyrażenia regularne umożliwia definiowanie wartość żądanego pliku cookie. 

**Dopasowań**/**jest niezgodny** opcja umożliwia określenie warunków, w którym znajduje się ten dopasować stan będą spełnione.
- **Dopasowuje**: wymaga żądania zawiera określony plik cookie z wartością, która odpowiada określonemu wyrażeniu regularnemu.
- **Nie odpowiada**: wymaga się, że żądanie spełniają jeden z następujących kryteriów:
  - Nie zawiera określonego pliku cookie.
  - Zawiera on określony plik cookie, ale jego wartość nie odpowiada określonemu wyrażeniu regularnemu.
  
Informacje o kluczu:
- Nazwa pliku cookie: 
  - Wyrażeń regularnych i znaków specjalnych, łącznie z gwiazdką, nie są obsługiwane, gdy w przypadku określenia nazwy pliku cookie. Oznacza to, że tylko cookie dokładnego dopasowania nazw kwalifikują się do porównania.
  - Można określić tylko nazwę jednego pliku cookie na każde wystąpienie tego warunku dopasowania.
  - Bez uwzględniania wielkości liter podczas porównywania nazw plików cookie.
- Wartość pliku cookie: 
  - Wartość pliku cookie mogą wykorzystać wyrażeń regularnych.
  - **Ignoruj przypadku** opcja określa, czy porównania z uwzględnieniem wielkości liter będą wykonywane względem wartość pliku cookie żądania.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

--- 
### <a name="country"></a>Kraj
Można określić kraju za pośrednictwem jego numer kierunkowy kraju. Podano opcję wskazująca, czy ten stan będzie spełniony, gdy kraj, z której pochodzi żądanie "Dopasowań" lub "Jest niezgodne" określonymi wartościami.

Informacje o kluczu:
- Oddzielającego każdej z nich z jednego miejsca, aby określić wiele numerów kierunkowych krajów.
- Symbole wieloznaczne nie są obsługiwane, gdy określasz kodu kraju.
- Numerów kierunkowych "UE" i "Region" nie obejmują wszystkie adresy IP w regionach.
- Niektóre żądania mogą nie zwracać prawidłowy kod kraju. Znak zapytania (?) będzie pasował do żądania, dla których nie można określić prawidłowy kod kraju.
- Numerów kierunkowych jest rozróżniana wielkość liter.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="customer-origin"></a>Pochodzenie klienta

Informacje o kluczu: 
- Niezależnie od tego, czy żądanie zawartości za pomocą adresu URL sieci dostarczania zawartości lub krawędzi CNAME URL wskazujące pochodzenie odbiorcy będą spełnić warunek dopasowania pochodzenia klienta.
- Nie można usunąć konfiguracji ze źródła — wersja klienta, która odwołuje się do niego reguły ze strony klienta źródła. Przed przystąpieniem do usuwania konfiguracji ze źródła — wersja klienta, upewnij się, następujące konfiguracje go nie odwołania:
  - Warunek dopasowania pochodzenia klienta
  - Konfiguracja CNAME krawędzi
- Nie używaj instrukcji IF i połączyć określone warunki dopasowania. Na przykład łączenie warunek dopasowania pochodzenia klienta z warunkiem dopasowania pochodzenia CDN utworzyć wzorca dopasowania, który nigdy nie być zgodne. Z tego powodu dwa warunki zgodne ze źródła — wersja klienta nie można łączyć za pomocą instrukcji IF i.

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="device"></a>Urządzenie

Warunek zgodne urządzenie identyfikuje żądań wysyłanych z urządzenia przenośnego na podstawie jego właściwości. Wykrywanie urządzenia przenośnego odbywa się za pośrednictwem [WURFL](http://wurfl.sourceforge.net/). W poniższej tabeli wymieniono możliwości WURFL oraz ich zmienne dla aparatu reguł Content Delivery Network.
<br>
> [!NOTE] 
> Obsługiwane są następujące zmienne w **modyfikowania nagłówka żądania klienta** i **zmodyfikować nagłówek odpowiedzi klienta** funkcji.

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
Przeglądarkę dla telefonów | % {wurfl_cap_mobile_browser} | Ciąg, który wskazuje przeglądarki, która służy do żądania zawartości od urządzenia. | Chrome
Wersja przenośnego przeglądarki | % {wurfl_cap_mobile_browser_version} | Ciąg, który wskazuje wersję przeglądarki, która służy do żądania zawartości od urządzenia. | 31
Nazwa modelu | % {wurfl_cap_model_name} | Ciąg, który wskazuje nazwę modelu urządzenia. | S3
Pobierania progresywnego | % {wurfl_cap_progressive_download} | Wartość logiczna, która wskazuje, czy urządzenie obsługuje odtwarzania audio i wideo, gdy nadal są pobierane. | prawda
Data wydania | % {wurfl_cap_release_date} | Ciąg, który wskazuje rok i miesiąc dodawania do bazy danych WURFL urządzenia.<br/><br/>Format:`yyyy_mm` | 2013_december
Wysokość rozwiązania | % {wurfl_cap_resolution_height} | Liczba całkowita, która wskazuje urządzenia wysokość w pikselach. | 768
Szerokość rozwiązania | % {wurfl_cap_resolution_width} | Liczba całkowita, która wskazuje szerokość urządzenia w pikselach. | 1024

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="edge-cname"></a>Krawędź Cname
Informacje o kluczu: 
- Lista dostępnych krawędzi CNAME jest ograniczone do tych, które zostały skonfigurowane na stronie CNAME krawędzi odpowiadające platformie, na którym jest konfigurowane aparatu reguł HTTP.
- Przed przystąpieniem do usunięcia konfiguracji CNAME krawędzi, upewnij się, że warunek dopasowania krawędzi Cname nie odwołuje się on. Nie można usunąć krawędzi CNAME konfiguracje, które zostały zdefiniowane w regule ze strony CNAME krawędzi. 
- Nie używaj instrukcji IF i połączyć określone warunki dopasowania. Na przykład łączenie warunek dopasowania krawędzi Cname z warunkiem dopasowania pochodzenia klienta utworzyć wzorca dopasowania, który nigdy nie być zgodne. Z tego powodu dwa warunki dopasowania krawędzi Cname nie można łączyć za pomocą instrukcji IF i.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="referring-domain"></a>Odwoływanie domeny
Nazwa hosta skojarzony z odwołania za pośrednictwem której zażądano zawartości określa, czy jest spełniony warunek odwołujących się do domeny. Opcja podano wskazująca, czy ten stan zostaną spełnione, gdy odwołujący się nazwa "Pasuje" lub "Jest niezgodne" określonej wartości.

Informacje o kluczu:
- Oddzielającego każdej z nich z jednego miejsca, aby określić wiele nazw hostów.
- Ten warunek dopasowania obsługuje znaków specjalnych.
- Jeśli określona wartość nie zawiera znak gwiazdki, musi być dokładnego dopasowania dla odwołania nazwy hosta. Na przykład określenie "mojadomena.com" będzie niezgodna "www.mydomain.com."
- **Ignoruj przypadku** opcja określa, czy będzie można wykonać porównania z uwzględnieniem wielkości liter.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-literal"></a>Literał nagłówka żądania
**Dopasowań**/**jest niezgodny** opcja umożliwia określenie warunków, w którym znajduje się ten dopasować stan będą spełnione.
- **Dopasowań**: wymaga zawierać określonego nagłówka żądania. Wartość musi być zgodna ten, który jest zdefiniowany w tym stanie dopasowania.
- **Nie odpowiada**: wymaga się, że żądanie spełniają jeden z następujących kryteriów:
  - Nie zawiera określonego nagłówka.
  - Zawiera on określony nagłówek, ale jego wartość jest niezgodna z jedną, która jest zdefiniowana w tym stanie dopasowania.
  
Informacje o kluczu:
- Bez uwzględniania wielkości liter podczas porównywania nazw nagłówka. **Ignoruj przypadku** opcja Określa uwzględnianie wielkości liter porównania wartości nagłówka.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-regex"></a>Wyrażenie regularne nagłówka żądania
**Dopasowań**/**jest niezgodny** opcja określa warunki, w jakich Regex nagłówka żądania zgodne warunkiem będą spełnione.
- **Dopasowań**: wymaga zawierać określonego nagłówka żądania. Wartość musi być zgodna wzorzec, który jest zdefiniowany w określonego wyrażenia regularnego.
- **Nie odpowiada**: wymaga się, że żądanie spełniają jeden z następujących kryteriów:
  - Nie zawiera określonego nagłówka.
  - Zawiera on określony nagłówek, ale jego wartość nie odpowiada określonemu wyrażeniu regularnemu.

Informacje o kluczu:
- Nazwa nagłówka: 
  - Bez uwzględniania wielkości liter podczas porównywania nazw nagłówka.
  - Spacje w nazwie nagłówka powinny być zastąpione przez "% 20". 
- Wartość nagłówka: 
  - Wartość nagłówka mogą wykorzystać wyrażeń regularnych.
  - **Ignoruj przypadku** opcja Określa uwzględnianie wielkości liter porównania wartości nagłówka.
  - Tylko dokładne nagłówka wartość dopasowań, który ma co najmniej jeden z określonych wzorców będą spełniały warunki tego warunku.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna 

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-header-wildcard"></a>Symbol wieloznaczny nagłówka żądania
**Dopasowań**/**jest niezgodny** opcja określa warunki, w jakich symbol wieloznaczny nagłówka żądania zgodne warunkiem będą spełnione.
- **Dopasowań**: wymaga zawierać określonego nagłówka żądania. Wartość musi odpowiadać co najmniej jedna z wartości zdefiniowanych w tym stanie dopasowania.
- **Nie odpowiada**: wymaga się, że żądanie spełniają jeden z następujących kryteriów:
  - Nie zawiera określonego nagłówka.
  - Zawiera on określony nagłówek, ale jego wartość nie pasuje do żadnego z określonymi wartościami.
  
Informacje o kluczu:
- Nazwa nagłówka: 
  - Bez uwzględniania wielkości liter podczas porównywania nazw nagłówka.
  - Spacje w nazwie nagłówka powinny być zastąpione przez "% 20". Ta wartość umożliwia również określić spacje w wartości nagłówka.
- Wartość nagłówka: 
  - Wartość nagłówka, mogą wykorzystać znaki specjalne.
  - **Ignoruj przypadku** opcja Określa uwzględnianie wielkości liter porównania wartości nagłówka.
  - Tylko dokładne nagłówka wartość dopasowań, który ma co najmniej jeden z określonych wzorców będą spełniały warunki tego warunku.
  - Oddzielającego każdej z nich z jednego miejsca, aby określić wiele wartości.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-method"></a>Request — metoda
Tylko zasobach, które są wymagane przez metodę wybranego żądania będą spełniają warunek metoda żądania. Sposoby żądanie dostępne są następujące:
- GET
- HEAD 
- POST 
- OPCJE 
- PUT 
- DELETE 
- ŚLEDZENIA 
- POŁĄCZ 

Informacje o kluczu:
- Domyślnie tylko metodę żądania GET może generować zawartości w pamięci podręcznej w sieci. Wszystkie inne metody żądania są przekazywane przez serwer proxy za pośrednictwem sieci.
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-scheme"></a>Schemat żądania
Tylko zasoby, które są żądane za pomocą wybranego protokołu będzie spełniają warunek schemat żądania. Dostępne protokoły są protokoły HTTP i HTTPS.

Informacje o kluczu:
- Ze względu na sposób, w których pamięci podręcznej ustawienia są śledzone ten warunek dopasowania jest niezgodny z następujących funkcji:
  - Zakończenie wypełnienie pamięci podręcznej
  - Max-Age wewnętrzny domyślne
  - Wymuszanie wewnętrznych Max-Age.
  - Ignoruj pochodzenia No-Cache
  - Wewnętrzny odświeżona maksymalna

[Powrót do początku](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

## <a name="next-steps"></a>Kolejne kroki
* [Omówienie usługi Azure Content Delivery Network](cdn-overview.md)
* [Odwołanie do aparatu reguł](cdn-rules-engine-reference.md)
* [Wyrażenia warunkowe aparatu reguł](cdn-rules-engine-reference-conditional-expressions.md)
* [Funkcje aparatu reguł](cdn-rules-engine-reference-features.md)
* [Zastępowanie domyślnego zachowania HTTP przy użyciu aparatu reguł](cdn-rules-engine.md)

