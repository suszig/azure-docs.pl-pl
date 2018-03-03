---
title: "Przy użyciu języka dostosowania — usługi Azure AD B2C | Dokumentacja firmy Microsoft"
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 02/26/2018
ms.author: sama
ms.openlocfilehash: 332c6b4ffd841a2c7aef9db5c8ba9e843790f4d6
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Usługi Azure Active Directory B2C: Dostosowywanie języka przy użyciu

>[!NOTE]
>Ta funkcja jest dostępna w publicznej wersji zapoznawczej.
>

Dostosowywanie języka umożliwia zasady, aby zmieścił się w różnych językach, w zależności od potrzeb klientów.  Microsoft oferuje tłumaczenie 36 języków (zobacz [dodatkowe informacje](#additional-information)), ale można też podać własne tłumaczenia dla żadnego języka.  Nawet jeśli środowiska jest dostępna wyłącznie dla jednego języka, można dostosować tekst na stronach.  

## <a name="how-does-language-customization-work"></a>Jak działa dostosowania języka?
Dostosowywanie języka umożliwia wybierz języki, które są dostępne w podróży użytkownika.  Po włączeniu tej funkcji, musisz podać parametr ciągu zapytania ui_locales, z poziomu aplikacji.  Po wywołaniu do usługi Azure AD B2C, firma Microsoft tłumaczenie ze strony ustawień regionalnych, który został wskazany.  Ten typ konfiguracji zapewnia pełną kontrolę nad językach w podróży użytkownika i ignoruje ustawienia języka w przeglądarce klienta. Można również nie może być konieczne tego poziomu kontroli nad jakich języków, zobacz klienta.  Jeśli nie podasz parametru ui_locales obsługi klienta jest zależna ustawienia ich przeglądarki.  Można wybrać języki podróży użytkownika jest translacji na, dodając ją jako obsługiwanego języka.  Jeśli przeglądarka klienta jest ustawiona do pokazania języka, które nie mają być obsługiwane, język, w którym wybrana jako domyślna w obsługiwanych kultur jest wyświetlany zamiast tego.

1. **Ustawienia regionalne interfejsu użytkownika określony język** — po włączeniu dostosowania języka podróży użytkownika jest przetłumaczony na język określony w tym miejscu
2. **Przeglądarki żądany język** — Jeśli nie określono żadnych interfejs użytkownika ustawień regionalnych, dokonuje translacji w przeglądarce żądany język **Jeśli została ona uwzględniona w obsługiwane języki**
3. **Język domyślny zasad** — Jeśli przeglądarka nie określa język, lub określa, który nie jest obsługiwany, dokonuje translacji na język domyślny zasad

>[!NOTE]
>Jeśli używane są atrybuty niestandardowe użytkownika, musisz podać własne tłumaczenia. Zobacz "[dostosować z ciągów](#customize-your-strings)" Aby uzyskać więcej informacji.
>

## <a name="support-uilocales-requested-languages"></a>Obsługa ui_locales żądane języki 
Zasady, które zostały utworzone przed wersji ogólnodostępnej dostosowania języka, należy najpierw włączyć tę funkcję.  Zasady utworzone po mają dostosowania języka domyślnie włączone.  Przez włączenie "Języka dostosowania" zasady, teraz można kontrolować języka podróży użytkownika przez dodanie parametru ui_locales.
1. [Wykonaj następujące kroki, aby przejść do strony funkcji B2C w portalu Azure.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Przejdź do zasad, które chcesz włączyć tłumaczenia.
3. Kliknij przycisk **dostosowywania języka**.  
4. Polecenie **włączenia dostosowywania języka** u góry.
5. Przeczytaj okna dialogowego, a następnie kliknij przycisk 'Tak'.

## <a name="select-which-languages-in-your-user-journey-are-enabled"></a>Wybierz języki w podróży użytkownika są włączone 
Włącz zestaw języków dla podróży użytkownika ma zostać poddany translacji w, jeśli nie podano parametru ui_locales.
1. Sprawdź, czy zasady zostały "Języka dostosowania" włączyć za pomocą czynności opisanych.
2. Z sieci **edytować zasady** wybierz pozycję **dostosowywania języka**.
3. Wybierz język, w którym chcesz obsługiwać.
4. W okienku właściwości Przełącz **włączone** Yes (tak).  
5. Kliknij przycisk **zapisać** w górnej części okienka właściwości.

>[!NOTE]
>Jeśli nie podano parametru ui_locales, następnie strony jest translacja na język przeglądarki klienta tylko wtedy, gdy jest włączone
>

## <a name="customize-your-strings"></a>Dostosowywanie z ciągów
"Język dostosowania" umożliwia dostosowanie dowolny ciąg w podróży użytkownika.
1. Sprawdź, czy zasady zostały "Języka dostosowania" włączyć za pomocą czynności opisanych.
2. Z sieci **edytować zasady** wybierz pozycję **dostosowywania języka**.
3. Wybierz język, który chcesz dostosować.
4. Wybierz stronę, którą chcesz edytować.
5. Kliknij przycisk **pobrać ustawień domyślnych** (lub **Pobierz zastąpienia** po zakończeniu edycji wcześniej ten język). 

Następujące kroki umożliwiają pliku JSON, który służy do edytowana z ciągów.

### <a name="changing-any-string-on-the-page"></a>Zmiana dowolny ciąg na stronie
1. Otwórz plik JSON pobrane z czynności opisanych w części edytora JSON.
2. Znajdź element, który chcesz zmienić.  Można znaleźć `StringId` ciągu szukasz, lub Wyszukaj `Value` chcesz zmienić.
3. Aktualizacja `Value` atrybut o to, co ma być wyświetlany.
4. Dla parametrów, co chcesz zmienić, pamiętaj przełączyć `Override` do **True**.
5. Zapisz plik i przekazać zmiany (w tym samym miejscu jako którego został pobrany plik JSON można znaleźć formantu przekazywania). 

>[!IMPORTANT]
>Jeśli potrzebujesz zastąpić ciąg, należy ustawić `Override` do wartości `true`.  Jeśli wartość nie ulega zmianie, wpis zostanie zignorowany. 
>

### <a name="changing-extension-attributes"></a>Zmienianie atrybutów rozszerzenia
Jeśli chcesz zmienić ciąg atrybutu niestandardowego użytkownika lub aby dodać je do JSON jest w następującym formacie:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

Zastąp `<ExtensionAttribute>` o nazwie atrybut użytkownika niestandardowego.  

Zastąp `<ExtensionAttributeValue>` o nowe parametry, które mają być wyświetlane.

### <a name="using-localizedcollections"></a>Przy użyciu LocalizedCollections
Jeśli chcesz udostępnić listę zestaw wartości dla odpowiedzi, należy utworzyć `LocalizedCollections`.  A `LocalizedCollections` jest tablicą `Name` i `Value` pary.  `Name` Jest wyświetlanych i `Value` to, co jest zwracany w oświadczeniu.  Aby dodać `LocalizedCollections`, ma następujący format:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType", 
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId` jest atrybut użytkownika, że `LocalizedCollections` odpowiedzi
* `Name` wartość jest pokazywana użytkownikowi
* `Value` jest to, co jest zwracany w oświadczenia po wybraniu tej opcji

### <a name="upload-your-changes"></a>Przekazywanie zmian
1. Po zakończeniu zmiany do pliku JSON, przejdź z powrotem do dzierżawy usługi B2C.
2. Z sieci **edytować zasady** wybierz pozycję **dostosowywania języka**.
3. Wybierz język, aby zapewnić tłumaczenia.
4. Wybierz stronę, aby zapewnić tłumaczenia.
5. Kliknij ikonę i wybierz plik JSON do przekazania.
6. To zmienić jest zapisywany do zasad automatycznie.

## <a name="using-page-ui-customization-with-language-customization"></a>Używanie dostosowywania interfejsu użytkownika strony z języka dostosowania

Istnieją dwa sposoby do zlokalizowania zawartości HTML.  Przez włączenie ["Języka dostosowania"](active-directory-b2c-reference-language-customization.md).  Włączenie tej funkcji umożliwia usłudze Azure AD B2C do przekazywania parametru Open ID Connect `ui-locales`, do punktu końcowego.  Serwer zawartości można Użyj tego parametru, aby zapewnić dostosowanych stron HTML, które są specyficzne dla języka.

Alternatywnie możemy pobierać zawartość z różnych miejsc, w oparciu o ustawienia regionalne używane.  W włączonego mechanizmu CORS punktu końcowego, można ustawić strukturę folderów do hosta zawartości dla określonych języków i Zadzwonimy właściwy, jeśli wartość symbolu wieloznacznego `{Culture:RFC5646}`.  Na przykład, jeśli ma to identyfikator URI Moje niestandardowe strony:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
Można załadować strony w `fr` i gdy jest on ściąganie zawartości html i css, będzie pobierać z:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="custom-locales"></a>Niestandardowe ustawienia regionalne

Można również dodać języki, które aktualnie nie oferuje tłumaczenia.  Należy podać tłumaczenia dla wszystkich ciągów w zasadach.

1. Z sieci **edytować zasady** wybierz pozycję **dostosowywania języka**.
2. Wybierz **dodać niestandardowe język** w górnej części strony.
3. W okienku kontekstu zidentyfikować języka, w którym udostępniasz tłumaczenia podając kod ustawień regionalnych prawidłowe.
4. Dla każdej strony można pobrać zestaw zastąpień dla języka angielskiego i pracować nad tłumaczenia.
5. Po wykonaniu pliki w formacie JSON, możesz przekazać je dla poszczególnych stron.
6. Wybierz **włączyć** i zasad można teraz Pokaż ten język dla użytkownika.
7. Pamiętaj, aby zapisać języka po jej włączeniu.

## <a name="additional-information"></a>Dodatkowe informacje

### <a name="page-ui-customization-labels-are-persisted-as-your-first-set-of-overrides-once-language-customization-is-enabled"></a>Etykiety dostosowania interfejsu użytkownika strony są zachowywane jako swój pierwszy zestaw zastąpienia po włączeniu "Dostosowania języka"
Po włączeniu "Dostosowania języka" wcześniejszych zmian dla etykiet przy użyciu interfejsu użytkownika strony dostosowania są zachowywane w pliku JSON dla języka angielskiego (en).  Możesz zmienić etykiety i innych ciągów przekazując zasobów językowych w "Dostosowania język".
### <a name="microsoft-is-committed-to-provide-the-most-up-to-date-translations-for-your-use"></a>Firma Microsoft dba o Podaj aktualne tłumaczenia do użycia
Firma Microsoft będzie stale poprawy tłumaczenia i przechowywać w zgodności dla Ciebie.  Firma Microsoft będzie zidentyfikować usterki i zmiany w terminologii globalne i bezproblemowo udostępnić aktualizacji, które będą działać w podróży użytkownika.
### <a name="support-for-right-to-left-languages"></a>Obsługa języków od prawej do lewej
Firma Microsoft obecnie nie są oferuje obsługę języków od prawej do lewej, jeśli potrzebujesz tej funkcji należy głosowania dla tej funkcji na [opinii Azure](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Społecznościowych tłumaczeń dostawcy tożsamości
Udostępniamy parametru OIDC ui_locales społecznościowych logowania, ale nie są honorowane przez niektórych dostawców tożsamości społecznościowych, w tym Facebook i Google. 
### <a name="browser-behavior"></a>Zachowanie przeglądarki
Chrome i Firefox żądania zarówno dla języka ich zestawu i jeśli jest obsługiwany, pojawi się przed domyślny.  Krawędź obecnie nie żąda języka i przechodzą wprost do język domyślny.

### <a name="what-languages-are-supported"></a>Jakich języków są obsługiwane?

| Język              | Kod języka |
|-----------------------|---------------|
| Bengalski                | bn            |
| Czeski                 | cs            |
| Duński                | da            |
| Niemiecki                | de            |
| Grecki                 | el            |
| Polski               | pl            |
| Hiszpański               | es            |
| Fiński               | fi            |
| Francuski                | fr            |
| Gudżarati              | gu            |
| Hindi                 | hi            |
| Chorwacki              | godz.            |
| Węgierski             | hu            |
| Włoski               | it            |
| Japoński              | ja            |
| Kannada               | kn            |
| Koreański                | ko            |
| Malajalam             | ml            |
| Marathi               | mr            |
| Malajski                 | ms            |
| Norwegian Bokmal      | nb            |
| Holenderski                 | nl            |
| Pendżabski               | pa            |
| Polski                | pl            |
| Portugalski (Brazylia)   | pt-br         |
| Portugalski (Portugalia) | pt-pt         |
| Rumuński              | ro            |
| Rosyjski               | ru            |
| Słowacki                | SK            |
| Szwedzki               | sv            |
| Tamilski                 | ta            |
| Telugu                | te            |
| Tajlandzki                  | TH            |
| Turecki               | TR            |
| Chiński (uproszczony)  | zh-hans       |
| Chiński — tradycyjny | zh-hant       |
