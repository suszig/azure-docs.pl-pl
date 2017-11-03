---
title: "Usługi Azure Active Directory B2C: Dostosowywanie języka przy użyciu | Dokumentacja firmy Microsoft"
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
ms.date: 04/25/2017
ms.author: sama
ms.openlocfilehash: 3c7c49ee5fbd98762da0eef6f02e7c2f036453cb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Usługi Azure Active Directory B2C: Dostosowywanie języka przy użyciu

>[!NOTE] 
>Ta funkcja jest dostępna w publicznej wersji zapoznawczej.  Zalecane jest użycie dzierżawy testowej podczas używania tej funkcji.  Nie planujemy zmiany podziału w wersji zapoznawczej w wersji ogólnodostępnej, ale możemy zarezerwować prawo do wprowadzania takie zmiany, aby poprawić funkcję.  Gdy już masz szansę wypróbować funkcję, podaj opinii na temat doświadczeniami i jak firma Microsoft może ją ulepszyć.  Musisz podać opinię za pośrednictwem portalu Azure za pomocą narzędzia krój uśmiech na górnym rogu.   Jeśli istnieje wymaganie biznesowe, można przejść na żywo, za pomocą tej funkcji w fazie podglądu, Daj nam znać scenariuszy i firma Microsoft może udostępnić odpowiednie wskazówki i uzyskać pomoc.  Skontaktuj się z nami pod adresem [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).

Dostosowywanie języka umożliwia zmianę podróży użytkownika na inny język, w zależności od potrzeb klientów.  Firma Microsoft udostępnia tłumaczenia dla 36 języków (zobacz [dodatkowe informacje](#additional-information)).  Nawet jeśli środowiska jest dostępna wyłącznie dla jednego języka, można dostosować tekst na stronach w zależności od potrzeb.  

## <a name="how-does-language-customization-work"></a>Jak działa dostosowania języka?
Dostosowywanie języka umożliwia wybierz języki, które są dostępne w podróży użytkownika.  Po włączeniu tej funkcji, musisz podać parametr ciągu zapytania ui_locales, z poziomu aplikacji.  Po wywołaniu do usługi Azure AD B2C, firma Microsoft tłumaczenie ze strony ustawień regionalnych, który został wskazany.  Za pomocą typ konfiguracji zapewnia pełną kontrolę nad językach w podróży użytkownika i ignoruje ustawienia języka w przeglądarce klienta. Można również nie może być konieczne tego poziomu kontroli nad jakich języków, zobacz klienta.  Jeśli nie podasz parametru ui_locales obsługi klienta jest zależna ustawienia ich przeglądarki.  Można wybrać języki podróży użytkownika jest translacji na, dodając ją jako obsługiwanego języka.  Jeśli przeglądarka klienta jest ustawiona do pokazania języka, które nie mają być obsługiwane, język, w którym wybrana jako domyślna w obsługiwanych kultur jest wyświetlany zamiast tego.

1. **Ustawienia regionalne interfejsu użytkownika określony język** — po włączeniu dostosowania języka podróży użytkownika jest przetłumaczony na język określony w tym miejscu
2. **Przeglądarki żądany język** — Jeśli nie określono żadnych interfejs użytkownika ustawień regionalnych, dokonuje translacji w przeglądarce żądany język **Jeśli została ona uwzględniona w obsługiwane języki**
3. **Język domyślny zasad** — Jeśli przeglądarka nie określa język, lub określa, który nie jest obsługiwany, dokonuje translacji na język domyślny zasad

>[!NOTE]
>Jeśli używane są atrybuty niestandardowe użytkownika, musisz podać własne tłumaczenia. Zobacz "[dostosować z ciągów](#customize-your-strings)" Aby uzyskać więcej informacji.
>

## <a name="support-uilocales-requested-languages"></a>Obsługa ui_locales żądane języki 
Przez włączenie "Języka dostosowania" zasady, teraz można kontrolować języka podróży użytkownika przez dodanie parametru ui_locales.
1. [Wykonaj następujące kroki, aby przejść do bloku funkcji B2C w portalu Azure.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Przejdź do zasad, które chcesz włączyć tłumaczenia.
3. Kliknij przycisk **dostosowywania języka**.
4. Zapoznać się z dokładnie.  Po włączeniu, nie można wyłączyć "Dostosowania język".
5. Włącz funkcję, a następnie kliknij przycisk **OK**.
6. Zapisz zasady w górnym lewym rogu Twojej **edytować zasady** bloku.

## <a name="select-which-languages-your-user-journey-supports"></a>Wybierz języki, które obsługuje podróży użytkownika 
Utwórz listę języków dozwolonych dla podróży użytkownika ma zostać poddany translacji w, jeśli nie podano parametru ui_locales.
1. Sprawdź, czy zasady zostały "Języka dostosowania" włączyć za pomocą czynności opisanych.
2. Z sieci **edytować zasady** bloku, wybierz opcję **dostosowywania języka**.
3. Zostają przeniesieni do Twojej **obsługiwanych języków** bloku.  W tym miejscu można wybrać **Dodaj język**.
4. Wybierz wszystkie języki, które mają być obsługiwane.  

>[!NOTE]
>Jeśli nie podano parametru ui_locales, następnie strony jest przetłumaczony na język przeglądarki klienta tylko wtedy, gdy znajduje się na tej liście
>

5. Kliknij przycisk **Ok** na dole
6. Zamknij **dostosowywania języka** bloku i **zapisać** zgodnie z zasadami.

## <a name="customize-your-strings"></a>Dostosowywanie z ciągów
"Język dostosowania" umożliwia dostosowanie dowolny ciąg w podróży użytkownika.
1. Sprawdź, czy zasady zostały "Języka dostosowania" włączyć za pomocą czynności opisanych.
2. Z sieci **edytować zasady** bloku, wybierz opcję **dostosowywania języka**.
3. Wybierz z menu nawigacji po lewej stronie **pobierania zawartości**.
4. Wybierz stronę, którą chcesz edytować.
5. Na liście rozwijanej wybierz język, który chcesz edytować.
6. Kliknij pozycję **Pobierz**. 

Następujące kroki umożliwiają pliku JSON, który służy do edytowana z ciągów.

### <a name="changing-any-string-on-the-page"></a>Zmiana dowolny ciąg na stronie
1. Otwórz plik JSON pobrane z czynności opisanych w części edytora JSON.
2. Znajdź element, który chcesz zmienić.  Można znaleźć `StringId` ciągu szukasz, lub Wyszukaj `Value` chcesz zmienić.
3. Aktualizacja `Value` atrybut o to, co ma być wyświetlany.
4. Zapisz plik i Przekaż zmiany.

### <a name="changing-extension-attributes"></a>Zmienianie atrybutów rozszerzenia
Jeśli chcesz zmienić ciąg atrybutu niestandardowego użytkownika lub aby dodać je do JSON jest w następującym formacie:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": false,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```
>[!IMPORTANT]
>Jeśli potrzebujesz zastąpić ciąg, należy ustawić `Override` do wartości `true`.  Jeśli wartość nie ulega zmianie, wpis zostanie zignorowany. 
>

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
      "Override": false,
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
>[!IMPORTANT]
>Jeśli potrzebujesz zastąpić ciąg, należy ustawić `Override` do wartości `true`.  Jeśli wartość nie ulega zmianie, wpis zostanie zignorowany. 
>

* `ElementId`jest atrybut użytkownika, że `LocalizedCollections` odpowiedzi
* `Name`wartość jest pokazywana użytkownikowi
* `Value`jest to, co jest zwracany w oświadczenia po wybraniu tej opcji

### <a name="upload-your-changes"></a>Przekazywanie zmian
1. Po zakończeniu zmiany do pliku JSON, przejdź z powrotem do dzierżawy usługi B2C.
2. Z sieci **edytować zasady** bloku, wybierz opcję **dostosowywania języka**.
3. Wybierz z menu nawigacji po lewej stronie **przekazać zawartość**.
4. Wybierz stronę, aby przekazać zmiany do.
5. Jeśli chcesz przekazać wcześniej podane JSON dla języka, należy usunąć poprzednie wpis.  Usuń go, klikając `...` po prawej stronie tego języka i wybierz **usunąć**.
6. Kliknij przycisk **Dodaj** w lewym górnym rogu.
7. Wybierz język pliku JSON.
8. Kliknij przycisk folder po prawej stronie, a następnie przejdź do pliku JSON.
9. Kliknij przycisk **przekazać** przycisk w dolnej części bloku.
10. Wróć do Twojej **edytować zasady** bloku i kliknij przycisk **zapisać**.

## <a name="additional-information"></a>Dodatkowe informacje
### <a name="recommendations-for-language-customization"></a>Zalecenia dotyczące dostosowywania"język"
Zaleca się wprowadzenie tylko wpisy do języka zasobów dla ciągów, który chcesz zamienić jawnie.  Firma Microsoft wymusić limit rozmiaru pliku, który ma być kompilowana poza tłumaczeń JSON.  Jeśli pliki zbyt duży, ma wpływ na wydajność pracy użytkownika.
### <a name="page-ui-customization-labels-are-removed-once-language-customization-is-enabled"></a>Etykiety dostosowania interfejsu użytkownika strony są usuwane po włączeniu "Dostosowania języka"
Po włączeniu "Dostosowania języka" wcześniejszych zmian dla etykiet przy użyciu interfejsu użytkownika strony dostosowania zostaną usunięte z wyjątkiem atrybutów niestandardowych użytkownika.  Aby uniknąć konfliktów, w którym można edytować użytkownika ciągi odbywa się tej zmiany.  Możesz zmienić etykiety i innych ciągów przekazując zasobów językowych w "Dostosowania język".
### <a name="microsoft-is-committed-to-provide-the-most-up-to-date-translations-for-your-use"></a>Firma Microsoft dba o Podaj aktualne tłumaczenia do użycia
Firma Microsoft będzie stale poprawy tłumaczenia i przechowywać w zgodności dla Ciebie.  Firma Microsoft będzie zidentyfikować usterki i zmiany w terminologii globalne i bezproblemowo udostępnić aktualizacji, które będą działać w podróży użytkownika.
### <a name="support-for-right-to-left-languages"></a>Obsługa języków od prawej do lewej
Nie jest obsługiwane dla języków od prawej do lewej, jeśli potrzebujesz tej funkcji należy głosowania dla tej funkcji na [opinii Azure](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Społecznościowych tłumaczeń dostawcy tożsamości
Udostępniamy parametru OIDC ui_locales społecznościowych logowania, ale nie są honorowane przez niektórych dostawców tożsamości społecznościowych, w tym Facebook i Google. 
### <a name="browser-behavior"></a>Zachowanie przeglądarki
Chrome i Firefox żądania zarówno dla języka ich zestawu i jeśli jest obsługiwany, pojawi się przed domyślny.  Krawędź obecnie nie żąda języka i przechodzą wprost do język domyślny.
### <a name="known-issues"></a>Znane problemy
* Przekazywanie zasobów językowych dla strony MFA w zasadach edycji profilu jest obecnie niedostępny.
* `LocalizedCollections`nie są generowane dla wartości, gdy jest to wymagane przez typ odpowiedzi
### <a name="what-if-i-want-a-language-that-isnt-supported"></a>Co zrobić, jeśli ma języka, który nie jest obsługiwany?
Firma Microsoft planuje zapewniają rozszerzenie tej funkcji, który umożliwia przekazywanie zasobu JSON kierunku "języki niestandardowe".  Funkcja umożliwia określenie nazwy i języka kodu dla żadnego języka i podaj *wszystkie* tłumaczenia dla tego języka.  Jeśli chcesz dodać tę funkcję, Wyślij do nas scenariusz w [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).  

### <a name="what-languages-are-supported"></a>Jakich języków są obsługiwane?

| Język              | Kod języka |
|-----------------------|---------------|
| Bengalski                | BN            |
| Czeski                 | cs            |
| Duński                | da            |
| Niemiecki                | de            |
| Grecki                 | el            |
| Polski               | en            |
| Hiszpański               | es            |
| Fiński               | fi            |
| Francuski                | fr            |
| Gudżarati              | gu            |
| Hindi                 | Cześć            |
| Chorwacki              | hr            |
| Węgierski             | hu            |
| Włoski               | it            |
| Japoński              | ja            |
| Kannada               | kN            |
| Koreański                | ko            |
| Malayalam             | uczenie maszynowe            |
| Marathi               | MR            |
| Malajski                 | MS            |
| Norweski (Nynorsk)      | nb            |
| Holenderski                 | nl            |
| Pendżabski               | dostawcy            |
| Polski                | pl            |
| Portugalski (Brazylia)   | pt-br         |
| Portugalski (Portugalia) | pt-pt         |
| Rumuński              | ro            |
| Rosyjski               | ru            |
| Słowacki                | SK            |
| Szwedzki               | sv            |
| Tamilski                 | Ta            |
| Telugu                | te            |
| Tajski                  | TH            |
| Turecki               | TR            |
| Chiński (uproszczony)  | zh-hans       |
| Chiński — tradycyjny | zh-hant       |
