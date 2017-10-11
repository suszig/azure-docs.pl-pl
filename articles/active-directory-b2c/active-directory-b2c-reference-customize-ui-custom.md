---
title: "Usługa Azure Active Directory B2C: Odwołanie: Dostosowywanie interfejsu użytkownika w podróży użytkownika z zasady niestandardowe | Dokumentacja firmy Microsoft"
description: "Temat dotyczący zasad niestandardowych usługi Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: 68f40aa638a687398512278a0b77d1ba392859cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="customize-the-ui-of-a-user-journey-with-custom-policies"></a>Dostosowywanie interfejsu użytkownika w podróży użytkownika przy użyciu zasad niestandardowych

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

> [!NOTE]
> W tym artykule jest zaawansowane opis działania dostosowywania interfejsu użytkownika i jak włączyć za pomocą zasad niestandardowych B2C, przy użyciu platformy obsługi tożsamości


Nie zakłóca pracy użytkowników jest kluczem dla dowolnego rozwiązania biznesowe z klientem. Przez nie zakłóca pracy użytkowników możemy oznacza środowisko na urządzeniu lub w przeglądarce, której przebieg użytkownika za pomocą naszej usługi nie wyodrębnione od obsługi klienta, którego używają.

## <a name="understand-the-cors-way-for-ui-customization"></a>Zrozumienie sposobu CORS do dostosowania interfejsu użytkownika

Usługa Azure AD B2C umożliwia dostosowanie wyglądu i działania środowisko użytkownika (UX) na różnych stronach, które mogą potencjalnie obsłużonych i wyświetlane przez usługę Azure AD B2C za pomocą niestandardowych zasad.

W tym celu usługi Azure AD B2C kod w przeglądarce sieci klienta, korzysta z podejścia nowoczesnych i standard [udostępniania zasobów między źródłami (CORS)](http://www.w3.org/TR/cors/) załadować niestandardowej zawartości z określonym adresem URL określonym w zasadach niestandardowych, aby wskazywał szablonów HTML5/CSS. CORS jest mechanizm umożliwiający ograniczone zasoby, takie jak czcionki, na stronie sieci web wymagane z innej domeny spoza domeny, z którego pochodzi zasobu.

W porównaniu do starego tradycyjny sposób, gdzie strony szablonu należą do firmy przy użyciu rozwiązania, gdzie podane ograniczone tekstu i obrazów, w którym została przyjęta ograniczone kontrolę nad układ i sposób działania prowadzące do więcej niż trudności do osiągnięcia bezproblemowej występują, sposób CORS obsługuje HTML5 i CSS i umożliwiają:

- Hostowanie zawartości i rozwiązanie injects jego formantów za pomocą skryptu po stronie klienta.
- Mają pełną kontrolę nad każdego piksela układ i działania.

Możesz podać przez obsługuje tworzenie pliki HTML5/CSS, takich jak dowolną liczbę stron zawartości.

> [!NOTE]
> Ze względów bezpieczeństwa użycie JavaScript jest obecnie zablokowany do dostosowania. Aby odblokować JavaScript, nazwy domeny niestandardowej dla dzierżawy usługi Azure AD B2C są potrzebne.

W każdej z szablonów HTML5/CSS, można zapewnić *zakotwiczenia* element, który odpowiada wymaganych `<div id=”api”>` element w kodzie HTML lub strony zawartości, jak ilustrują poniżej. Usługa Azure AD B2C musi mieć wszystkie strony z zawartością tego określonego div.

```
<!DOCTYPE html>
<html>
  <head>
    <title>Your page content’s tile!</title>
  </head>
  <body>
    <div id="api"></div>
  </body>
</html>
```

Azure AD B2C związane z zawartości dla strony zostaną dodane do tego div podczas pozostałej części strony jest Twój do formantu. Kod JavaScript usługi Azure AD B2C ściąga zawartości i injects naszych HTML do tego elementu div określone. Usługa Azure AD B2C injects następujące formanty odpowiednio: konta formantu selektora, kontrolek logowania, wieloskładnikowego formantów (obecnie telefoniczny) i atrybutu kolekcji formantów. Usługa Azure AD B2C zapewnia, że wszystkie formanty są HTML5 zgodne i jest dostępny, wszystkie formanty można całkowicie stylem i czy nie będzie zbadanie kontroli wersji.

Scalone zawartości ostatecznie jest wyświetlana jako dynamiczny dokumentu z klientem.

Aby zapewnić postąpić, zgodnie z oczekiwaniami, należy:

- Upewnij się, że zawartość jest HTML5 zgodne i jest dostępny
- Upewnij się, że serwer zawartości jest włączona dla CORS.
- Udostępniać zawartość za pośrednictwem protokołu HTTPS.
- Używać bezwzględnych adresów URL, takie jak https://yourdomain/content dla wszystkich łączy i zawartość arkusza CSS.

> [!TIP]
> Aby sprawdzić, czy w lokacji, które prowadzą hosting zawartości na włączonego mechanizmu CORS i żądań CORS testu, można użyć http://test-cors.org/ lokacji. Dzięki tej lokacji możesz po prostu Wyślij żądanie CORS do serwera zdalnego (do testowania, jeśli jest obsługiwana przez CORS), lub wysłać żądania CORS na serwerze testowym (aby eksplorować niektórych funkcji CORS).

> [!TIP]
> Http://enable-cors.org/ lokacji stanowi również ponad przydatne zasoby na CORS.

Dzięki tej metody na podstawie specyfikacji CORS użytkownicy końcowi będzie miał spójne działanie pomiędzy aplikacją i stron obsługiwanych przez usługę Azure AD B2C.

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu

Warunkiem wstępnym musisz utworzyć konto magazynu. Konieczne będzie subskrypcji platformy Azure, aby utworzyć konto magazynu obiektów Blob Azure. Możesz utworzyć konto bezpłatnej wersji próbnej na [witryny sieci Web Azure](https://azure.microsoft.com/en-us/pricing/free-trial/).

1. Otwórz sesję przeglądania i przejdź do [portalu Azure](https://portal.azure.com).
2. Zaloguj się przy użyciu poświadczeń administracyjnych.
3. Kliknij przycisk **nowy** > **dane i magazyn** > **konta magazynu**.  A **utworzyć konto magazynu** Otwiera blok.
4. W **nazwa**, podaj nazwę konta magazynu, na przykład *contoso369b2c*. Ta wartość będzie później określane jako *storageAccountName*.
5. Wybierz odpowiednie opcje dla warstwy cenowej, grupy zasobów i subskrypcji. Upewnij się, że masz **Przypnij do tablicy startowej** zaznaczoną opcją. Kliknij przycisk **Utwórz**.
6. Wróć do tablicy startowej, a następnie kliknij konto magazynu, który został właśnie utworzony.
7. W **usług** kliknij **obiekty BLOB**. A **bloku usługi Blob** otwiera.
8. Kliknij przycisk **+ kontener**.
9. W **nazwa**, podaj nazwę kontenera, na przykład *b2c*. Ta wartość zostanie później określane jako *containerName*.
9. Wybierz **obiektu Blob** jako **dostęp typu**. Kliknij przycisk **Utwórz**.
10. Kontener, w którym utworzono będą wyświetlane na liście na **bloku usługi Blob**.
11. Zamknij **obiekty BLOB** bloku.
12. Na **bloku konto magazynu**, kliknij przycisk **klucza** ikony. **Bloku klucze dostępu** otwiera.  
13. Zanotuj wartość **klucz1**. Ta wartość zostanie później określonego jako *klucz1*.

## <a name="downloading-the-helper-tool"></a>Pobranie narzędzia pomocy

1.  Pobierz narzędzie Pomocnik z [GitHub](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).
2.  Zapisz *B2C-AzureBlobStorage-klienta master.zip* plik na komputerze lokalnym.
3.  Wyodrębnij zawartość pliku B2C-AzureBlobStorage-klienta master.zip na lokalnym dysku, na przykład w obszarze **pakietu w przypadku dostosowania interfejsu użytkownika** folderu. Spowoduje to utworzenie *B2C-AzureBlobStorage-klienta master* folderu poniżej.
4.  Otwórz ten folder i Wyodrębnij zawartość pliku archiwum *B2CAzureStorageClient.zip* w nim.

## <a name="upload-the-ui-customization-pack-sample-files"></a>Przekaż przykładowych plików pakietu w przypadku dostosowania interfejsu użytkownika

1.  Przy użyciu Eksploratora Windows przejdź do folderu *B2C-AzureBlobStorage-klienta master* znajduje się w obszarze *pakietu w przypadku dostosowania interfejsu użytkownika* folder utworzony w poprzedniej sekcji.
2.  Uruchom *B2CAzureStorageClient.exe* pliku. Ten program będzie po prostu Przekaż wszystkie pliki w katalogu wskazanym na koncie magazynu i włączyć CORS dostęp do tych plików.
3.  Po wyświetleniu monitu podaj:.  Nazwa konta magazynu *storageAccountName*, na przykład *contoso369b2c*.
    b.  Podstawowy klucz dostępu magazynu obiektów blob platformy azure, *klucz1*, na przykład *contoso369b2c*.
    c.  Nazwa kontenera magazynu obiektów blob z magazynu *containerName*, na przykład *b2c*.
    d.  Ścieżka *początkowego pakietu* próbki plików, na przykład *... \B2CTemplates\wingtiptoys*.

Po wykonaniu kroków powyżej pliki CSS i HTML5 *pakietu w przypadku dostosowania interfejsu użytkownika* w fikcyjnej firmie **wingtiptoys** teraz będzie wskazywać na koncie magazynu.  Aby sprawdzić, czy zawartość, został przesłany poprawnie przez otwarcie bloku pokrewne kontenera w portalu Azure. Alternatywnie można sprawdzić, czy zawartość, został przesłany prawidłowo po zalogowaniu się do strony w przeglądarce. Aby uzyskać więcej informacji, zobacz [usługi Azure Active Directory B2C: Narzędzie Pomocnik, używany do przedstawiania funkcji dostosowywania interfejsu użytkownika strony](active-directory-b2c-reference-ui-customization-helper-tool.md).

## <a name="ensure-the-storage-account-has-cors-enabled"></a>Upewnij się, że konto magazynu ma włączonego mechanizmu CORS

CORS (Cross-Origin Resource Sharing) musi być włączona punktu końcowego dla usługi Azure AD B2C Premium w celu załadowania zawartości. Jest to spowodowane zawartości znajduje się w innej domenie niż domeny Premium usługi Azure AD B2C będzie obsługująca stronę z.

Aby sprawdzić, czy magazyn, które prowadzą hosting zawartości na ma włączonego mechanizmu CORS, kontynuować następujące czynności:

1. Otwórz sesję przeglądania i przejdź do strony *unified.html* przy użyciu pełny adres URL lokalizacji na koncie magazynu `https://<storageAccountName>.blob.core.windows.net/<containerName>/unified.html`. Na przykład https://contoso369b2c.blob.core.windows.net/b2c/unified.html.
2. Przejdź do http://test-cors.org. Tej lokacji pozwala sprawdzić, czy strona, której używasz ma włączonego mechanizmu CORS.  
<!--
![test-cors.org](../../media/active-directory-b2c-customize-ui-of-a-user-journey/test-cors.png)
-->

3. W **zdalnego adresu URL**, wprowadź pełny adres URL dla zawartości unified.html i kliknij przycisk **Wyślij żądanie**.
4. Sprawdź, czy dane wyjściowe w **wyniki** sekcja zawiera *stan XHR: 200*. Oznacza to, że włączono CORS.
<!--
![CORS enabled](../../media/active-directory-b2c-customize-ui-of-a-user-journey/cors-enabled.png)
-->
Konto magazynu powinien zawierać teraz kontenera obiektów blob o nazwie *b2c* naszych ilustracji, który zawiera następujące szablony wingtiptoys z *początkowego pakietu*.

<!--
![Correctly configured storage account](../../articles/active-directory-b2c/media/active-directory-b2c-reference-customize-ui-custom/storage-account-final.png)
-->

W poniższej tabeli opisano celem powyżej stron HTML5.

| Szablon HTML5 | Opis |
|----------------|-------------|
| *phonefactor.HTML* | Ta strona może służyć jako szablon dla strony uwierzytelniania wieloskładnikowego. |
| *ResetPassword.HTML* | Ta strona może służyć jako szablon dla nie pamiętasz hasła strony. |
| *selfasserted.HTML* | Ta strona może służyć jako szablon dla kont społecznościowych stronę tworzenia konta, stronę tworzenia konta lokalnego konta lub stronę logowania konta lokalnego. |
| *Unified.HTML* | Ta strona może służyć jako szablon ujednoliconego strony rejestracji lub logowania. |
| *updateprofile.HTML* | Ta strona może służyć jako szablon strony aktualizacji profilu. |

## <a name="add-a-link-to-your-html5css-templates-to-your-user-journey"></a>Dodaj łącze do szablonów HTML5/CSS do podróży użytkownika

Można dodać łącza do szablonów HTML5/CSS do podróży użytkownika bezpośrednio edytując zasadę niestandardową.

Szablony niestandardowe HTML5/CSS do użycia w podróży użytkownika ma określonych na liście zawartości definicje, które mogą być używane w podróży tych użytkowników. W tym celu opcjonalny  *<ContentDefinitions>*  — element XML musi być zadeklarowana w obszarze  *<BuildingBlocks>*  sekcji w pliku XML zasady niestandardowe.

W poniższej tabeli opisano zestaw identyfikatorów definicji zawartości rozpoznawany przez aparat obsługi tożsamości usługi Azure AD B2C i typ strony, które odnoszą się do nich.

| Identyfikator definicji zawartości | Opis |
|-----------------------|-------------|
| *API.error* | **Strona błędu**. Ta strona jest wyświetlana po napotkaniu wyjątku lub wystąpił błąd. |
| *API.idpselections* | **Strona wyboru dostawcy tożsamości**. Ta strona zawiera listę dostawców tożsamości, które użytkownik może wybrać podczas logowania. Są to przedsiębiorstwa dostawców tożsamości, dostawców tożsamości społecznościowych, takich jak Facebook i Google + lub kont lokalnych (w oparciu nazwa użytkownika lub adres e-mail). |
| *API.idpselections.Signup* | **Wybór dostawcy tożsamości dla rejestracji**. Ta strona zawiera listę dostawców tożsamości, które użytkownik może wybrać podczas tworzenia konta. Są to przedsiębiorstwa dostawców tożsamości, dostawców tożsamości społecznościowych, takich jak Facebook i Google + lub kont lokalnych (w oparciu nazwa użytkownika lub adres e-mail). |
| *API.localaccountpasswordreset* | **Nie pamiętasz hasła strony**. Ta strona zawiera formularz, który użytkownik ma do wypełnienia zainicjować ich resetowania hasła.  |
| *API.localaccountsignin* | **Strona logowania konta lokalnego**. Ta strona zawiera formularz logowania, który użytkownik musi podać podczas logowania się za pomocą konta lokalnego, która jest oparta na adres e-mail lub nazwę użytkownika. Formularz może zawierać pola do wprowadzania tekstu, a w polu wprowadzania hasła. |
| *API.localaccountsignup* | **Stronę tworzenia konta lokalnego konta**. Ta strona zawiera wypełnieniu formularza, który użytkownik musi podać podczas zapisywania się do konta lokalnego, która jest oparta na adres e-mail lub nazwę użytkownika. Formularz może zawierać różne kontrolki wejściowe, takich jak pola do wprowadzania tekstu, pole wprowadzania hasła przycisk radiowy, jednokrotnym zaznaczeniem pola listy rozwijanej i pól wyboru wielokrotnego wyboru. |
| *API.phonefactor* | **Strona uwierzytelniania wieloskładnikowego**. Na tej stronie użytkowników można sprawdzić ich numery telefonów (przy użyciu tekstowych lub głosowych) podczas tworzenia konta lub logowania. |
| *API.selfasserted* | **Strony rejestracji społecznościowych konta**. Ta strona zawiera wypełnieniu formularza, który użytkownik musi podać podczas logowania przy użyciu istniejącego konta od dostawcy tożsamości społecznościowych, takich jak Facebook lub Google +. Ta strona jest podobny do powyżej społecznego konta stronę tworzenia konta, z wyjątkiem hasło pola wejścia. |
| *API.selfasserted.profileupdate* | **Strona aktualizacji profilu**. Ta strona zawiera formularz, który użytkownik może użyć do zaktualizowania swój profil. Ta strona jest podobny do powyżej społecznego konta stronę tworzenia konta, z wyjątkiem hasło pola wejścia. |
| *API.signuporsignin* | **Ujednolicone stronę tworzenia konta lub logowania**.  Ta strona obsługuje zarówno rejestracji i logowania użytkowników, którzy można używać w organizacji dostawcy tożsamości, dostawców tożsamości społecznościowych, takich jak Facebook lub Google + lub kont lokalnych.

## <a name="next-steps"></a>Następne kroki
[Odwołanie: Zrozumieć, jak niestandardowe zasady współpracują w ramach obsługi tożsamości w B2C](active-directory-b2c-reference-custom-policies-understanding-contents.md)
