---
title: "Dodawanie znakowania firmowego do stron logowania i panelu dostępu w usłudze Azure Active Directory"
description: "Dowiedz się, jak dodać znakowanie firmowe do strony logowania i strony panelu dostępu platformy Azure"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f74621b4-4ef0-4899-8c0e-0c20347a8c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/07/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 6d4fbfe97288fcb76628b45649b8b678152198a9
ms.lasthandoff: 04/07/2017


---
# <a name="add-company-branding-to-sign-in-and-access-panel-pages"></a>Dodawanie znakowania firmowego do stron logowania i panelu dostępu
Wiele organizacji chce mieć spójny wygląd i zachowanie wszystkich witryn sieci Web i usług, którymi zarządzają. Usługa Azure Active Directory zapewnia tę funkcję, umożliwiając informatykom dostosowanie wyglądu następujących stron sieci Web przez dodanie obrazów i logo firmy:

* **Strona logowania** — jest to strona wyświetlana, gdy pracownicy i goście biznesowi logują się do usługi Office 365 lub innych aplikacji sieci Web, które korzystają z usługi Azure AD.
* **Strona panelu dostępu** — panel dostępu jest portalem sieci Web umożliwiającym wyświetlanie i uruchamianie aplikacji opartych na chmurze, do których administrator usługi Azure AD udzielił Ci dostępu. Panel dostępu znajduje się pod adresem: [https://myapps.microsoft.com](https://myapps.microsoft.com).

W tym temacie wyjaśniono, jak można dostosować stronę logowania i stronę panelu dostępu.

> [!NOTE]
> * Dodawanie znakowania firmowego jest funkcją dostępną tylko w przypadku uaktualnienia do wersji Premium lub Podstawowa usługi Azure Active Directory lub posiadania licencji usługi Office 365. Aby uzyskać więcej informacji, zobacz Wersje usługi Azure Active Directory.
> 
> * Klienci w Chinach mogą używać wersji Premium i Podstawowa usługi Azure Active Directory za pomocą wystąpienia usługi Azure Active Directory dostępnego na całym świecie. Wersje Premium i Podstawowa usługi Azure Active Directory nie są obecnie obsługiwane w usłudze Microsoft Azure świadczonej przez 21Vianet w Chinach. Aby uzyskać więcej informacji, skontaktuj się z nami na forum usługi Azure Active Directory.


## <a name="customizing-the-sign-in-page"></a>Dostosowywanie strony logowania
Użytkownicy zazwyczaj korzystają ze strony logowania usługi Azure AD podczas próby uzyskania dostępu do usług i aplikacji w chmurze subskrybowanych przez organizację.

W przypadku zastosowania zmian w znakowaniu na stronie logowania może minąć godzina, zanim będą one widoczne dla użytkowników końcowych.

Elementy znakowania firmowego będą wyświetlane na stronie logowania usługi Azure AD, gdy użytkownicy uzyskają dostęp do adresu URL specyficznego dla dzierżawy, np. https://outlook.com/contoso.com.

Jeśli użytkownik odwiedzi usługę, korzystając z ogólnego adresu URL, np. www.office.com, strona logowania nie będzie zawierać znakowania firmowego, ponieważ system jeszcze nie wie, kim jest użytkownik. Znakowanie firmowe zostanie jednak wyświetlone, gdy użytkownik wprowadzi swój identyfikator lub wybierze kafelek użytkownika.

> [!NOTE]
> * Nazwa domeny musi być widoczna jako „Aktywna” w sekcji **Active Directory** > **Katalog** > **Domeny** klasycznego portalu Azure, w którym skonfigurowano znakowanie.
> * Znakowanie na stronie logowania nie jest przenoszone na stronę logowania do osobistych kont Microsoft. Jeśli pracownicy lub goście biznesowi będą się logować przy użyciu osobistego konta Microsoft, ich strony logowania nie będą zawierać znakowania organizacji.
>

Na poniższych zrzutach ekranu wyjaśniono sposób dostosowywania stron logowania.

### <a name="scenario-1-contoso-employee-goes-to-a-generic-app-url-for-example-wwwofficecom"></a>Scenariusz 1: pracownik firmy Contoso przechodzi do ogólnego adresu URL aplikacji (na przykład www.office.com)

W tym przykładzie użytkownik firmy Contoso loguje się do aplikacji mobilnej lub do aplikacji sieci Web za pomocą ogólnego adresu URL. Ilustracja po lewej stronie zawsze będzie reprezentować aplikację, a okienko interakcji z prawej strony w odpowiednim momencie zostanie zaktualizowane, aby wyświetlić elementy marki firmy Contoso.

![Strona logowania usługi Office 365 przed dostosowaniem i po nim][1]

### <a name="scenario-2-contoso-employee-goes-to-contoso-app-thats-restricted-to-internal-users"></a>Scenariusz 2: pracownik firmy Contoso przechodzi do aplikacji firmy Contoso, która jest ograniczona do użytkowników wewnętrznych

W tym przykładzie użytkownik firmy Contoso loguje się do wewnętrznej aplikacji przy użyciu adresu URL specyficznego dla firmy. Ilustracja po lewej stronie reprezentuje markę firmy (Contoso). Okienko interakcji z prawej strony jest zawsze wyświetlane w wersji dla firmy Contoso i pomaga pracownikom zalogować się.

![Strona logowania ograniczonej aplikacji][2]

### <a name="scenario-3-contoso-employee-goes-to-a-contoso-app-thats-open-to-external-users"></a>Scenariusz 3: pracownik firmy Contoso przechodzi do aplikacji firmy Contoso, która jest otwarta dla użytkowników zewnętrznych

W tym przykładzie użytkownicy logują się do aplikacji biznesowej firmy Contoso, ale użytkownik nie musi być pracownikiem firmy Contoso. Ilustracja po lewej stronie reprezentuje właściciela zasobu (Contoso), podobnie jak w przypadku nr \#2 powyżej. Jednak tym razem okienko interakcji po prawej stronie nie jest w wersji dla firmy Contoso, co oznacza, że użytkownicy zewnętrzni mogą się zalogować.

![Logowanie się do aplikacji z otwartym dostępem][3]

### <a name="scenario-4-fabrikam-business-guest-goes-to-contoso-app-thats-open-to-external-users"></a>Scenariusz 4: gość biznesowy z firmy Fabrikam przechodzi do aplikacji firmy Contoso, która jest otwarta dla użytkowników zewnętrznych

W tym przykładzie użytkownik firmy Contoso loguje się do wewnętrznej aplikacji przy użyciu adresu URL specyficznego dla firmy. Ilustracja po lewej stronie reprezentuje markę firmy (Contoso). Okienko interakcji z prawej strony jest zawsze wyświetlane w wersji dla firmy Contoso i pomaga pracownikom zalogować się.

![Logowanie jako użytkownik zewnętrzny][4]


## <a name="what-elements-on-the-page-can-i-customize"></a>Jakie elementy na stronie można dostosować?

Na stronie logowania można dostosować następujące elementy:

![][5]

| Element strony | Lokalizacja na stronie |
|:--- | --- |
| Baner logo | Wyświetlany w prawym górnym rogu strony. Zastępuje logo aplikacji po określeniu organizacji użytkownika (zazwyczaj po wprowadzeniu nazwy użytkownika). |
| Ilustracja w tle | Wyświetlana jako obraz z pełnym spadem po lewej stronie strony logowania. Zastępuje ilustrację aplikacji w scenariuszach logowania z określoną dzierżawą (gdy użytkownicy uzyskują dostęp do aplikacji opublikowanych przez ich własną organizację lub organizację, w której są gośćmi biznesowymi).<br>W przypadku połączeń o niskiej przepustowości ilustracja w tle jest zastępowana kolorem tła. Ilustracja nie jest wyświetlana na wąskich ekranach, np. na telefonach.<br>Gdy użytkownik zmieni rozmiar przeglądarki, ilustracja w tle zostanie przycięta. Podczas projektowania ilustracji kluczowe elementy wizualne umieść w lewym górnym rogu, aby nie zostały przycięte. | 
| Pole wyboru „Nie wylogowuj mnie” | Wyświetlane pod polem **Hasło**. |
| Tekst strony logowania | Standardowy tekst, który ma być wyświetlany powyżej stopki strony. Może służyć do przekazywania użytkownikom przydatnych informacji, np. numeru telefonu do działu pomocy technicznej lub klauzuli prawnej. |

> [!NOTE]
> Wszystkie elementy są opcjonalne. Jeśli na przykład określisz baner logo, ale nie określisz ilustracji w tle, na stronie logowania będzie pokazywane Twoje logo oraz ilustracja dla witryny docelowej (np. obraz kalifornijskiej autostrady dla usługi Office 365).
>

Pole wyboru **Nie wylogowuj mnie** na stronie logowania umożliwia użytkownikom zachowanie stanu zalogowania w przypadku zamknięcia i ponownego otworzenia przeglądarki. Nie ma ono wpływu na okres istnienia sesji.

Wyświetlanie pola wyboru zależy od ustawienia **Ukryj KMSI**.

![Ustawienie Ukryj KMSI][6]

Aby ukryć pole wyboru, skonfiguruj ustawienie, wybierając opcję **Ukryte**.

> [!NOTE]
> Niektóre funkcje usługi SharePoint Online oraz pakietu Office 2010 zależą od możliwości zaznaczenia tego pola wyboru przez użytkowników. Jeśli skonfigurujesz to ustawienia na wartość „Ukryte”, użytkownicy mogą otrzymywać dodatkowe i nieoczekiwane monity o logowanie.
>
>

Możesz również dostosować wszystkie elementy na tej stronie. Po skonfigurowaniu „domyślnego” zestawu elementów dostosowania można skonfigurować więcej wersji dla różnych ustawień regionalnych. Możesz także mieszać i dopasowywać różne elementy. Można na przykład:

* Utworzyć „domyślną” ilustrację w tle działającą dla wszystkich języków, a następnie utworzyć specyficzne wersje dla języka angielskiego i francuskiego. W przypadku ustawienia w przeglądarce jednego z tych dwóch języków wyświetlany będzie określony obraz, podczas gdy ilustracja domyślna będzie wyświetlana dla wszystkich pozostałych języków.
* Skonfigurować różne wersje logo dla organizacji (np. wersję japońską lub hebrajską).

## <a name="access-panel-page-customization"></a>Dostosowywanie strony panelu dostępu
Strona panelu dostępu jest zasadniczo stroną portalu umożliwiającą szybki dostęp do aplikacji w chmurze, do których administrator udzielił użytkownikowi dostępu. Na tej stronie aplikacje są wyświetlane jako aktywne kafelki aplikacji do kliknięcia.

Poniższy zrzut ekranu przedstawia przykład strony panelu dostępu po dostosowaniu.

![][8]

## <a name="configure-your-directory-with-company-branding"></a>Konfigurowanie katalogu za pomocą znaków firmowych
W klasycznym portalu Azure dla każdego katalogu można skonfigurować jeden domyślny zestaw elementów dostosowywalnych. Po zapisaniu ustawień domyślnych administrator może dodać zlokalizowane wersje każdego elementu dla różnych języków/ustawień regionalnych. Wszystkie elementy dostosowywalne są opcjonalne.

Na przykład jeśli skonfigurujesz domyślny baner logo, ale nie skonfigurujesz dużej ilustracji, na stronie logowania w prawym górnym rogu wyświetlane będzie dostosowane logo. Jako duża ilustracja wyświetlana będzie domyślna ilustracja lokacji.

Wyobraź sobie następującą konfigurację:

* domyślny baner logo i tekst strony logowania w języku angielskim
* tekst strony logowania specyficzny dla języka skonfigurowany dla języka niemieckiego

Jeśli preferowanym językiem jest niemiecki, na stronie logowania wyświetlany będzie domyślny baner logo, ale niemiecki tekst.

Pomimo że technicznie można skonfigurować różne zestawy dla każdego z języków obsługiwanych przez usługę Azure AD, zalecamy utrzymanie małej liczby wersji ze względu na wydajność i konserwację.
 
**Aby dodać znakowanie firmowe do katalogu, wykonaj następujące czynności:**

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) jako administrator katalogu, który chcesz dostosować.
2. Wybierz katalog, który chcesz dostosować.
3. Na pasku narzędzi u góry kliknij przycisk **Konfiguruj**.
4. Kliknij pozycję **Customize Branding (Dostosuj znakowanie)**.
5. Zmodyfikuj elementy, które chcesz dostosować. Wszystkie pola są opcjonalne.
6. Kliknij pozycję **Zapisz**.

Uwzględnienie nowych zmian znakowania na stronie logowania może potrwać do godziny.

**Aby dodać znakowanie firmowe specyficzne dla języka, wykonaj następujące czynności:**

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) jako administrator katalogu, który chcesz dostosować.
2. Wybierz katalog, który chcesz dostosować.
fs3. Na pasku narzędzi u góry kliknij przycisk **Konfiguruj**.
4. Kliknij pozycję **Customize Branding (Dostosuj znakowanie)**.
5. Kliknij pozycję **Add branding for a specific language (Dodaj znakowanie dla określonego języka)**.
6. Wybierz język, dla którego chcesz dostosować logo, a następnie kliknij przycisk **Dalej**.
7. Edytuj tylko te elementy, dla których chcesz skonfigurować zastąpienia specyficzne dla języka. Wszystkie pola są opcjonalne. Jeśli pole zostanie zostawione puste, wyświetlana będzie niestandardowa wartość domyślna (lub wartość domyślna firmy Microsoft, jeśli nie skonfigurowano niestandardowej wartości domyślnej).
8. Kliknij pozycję **Zapisz**.

**Aby usunąć znakowanie firmowe z katalogu, wykonaj następujące czynności:**

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) jako administrator katalogu, który chcesz dostosować.
2. Wybierz katalog, który chcesz dostosować.
3. Na pasku narzędzi u góry kliknij przycisk **Konfiguruj**.
4. Kliknij pozycję **Customize Branding (Dostosuj znakowanie)**.
5. Na stronie Customize Branding (Dostosowywanie znakowania) wybierz pozycję **Edit Existing Branding Settings (Edytuj istniejące ustawienia znakowania)**, a następnie przejdź do następnej strony.
6. W zależności od tego, które elementy chcesz usunąć, wykonaj jedną lub więcej z następujących czynności:

    a. W obszarze **Banner Logo (Baner logo)** wybierz opcję **Remove uploaded logo (Usuń przekazane logo)**.

    b. W obszarze **Tile Logo (Logo kafelka)** wybierz opcję **Remove uploaded logo (Usuń przekazane logo)**.

    c. Usuń tekst ze wszystkich pól tekstowych.

    d. Kliknij przycisk **Dalej**.

    e. Usuń tekst ze wszystkich pól tekstowych.
7. Kliknij pozycję **Zapisz**, aby usunąć elementy.
8. W razie potrzeby kliknij ponownie pozycję **Customize Branding (Dostosuj znakowanie)** i powtórz te kroki dla całego znakowania specyficznego dla języka, które musi zostać usunięte.
    Jeśli po kliknięciu pozycji **Customize Branding (Dostosuj znakowanie)** zobaczysz formularz **Customize Default Branding (Dostosuj domyślne znakowanie)** bez żadnych skonfigurowanych ustawień, oznacza to, że wszystkie ustawienia znakowania zostały usunięte.


## <a name="customizable-elements"></a>Elementy dostosowywalne
Logo firmy jest używane na stronach logowania i panelu dostępu, natomiast inne elementy są używane tylko na stronie logowania. Poniższa tabela zawiera szczegółowe informacje dotyczące różnych elementów dostosowywalnych.

| Nazwa | Opis | Ograniczenia | Zalecenia |
| --- | --- | --- | --- |
| Baner logo |Baner logo jest wyświetlany na stronie logowania i stronie panelu dostępu. |<p>JPG lub PNG</p><p>60 x 280 pikseli</p><p>10 KB</p> |<p>Użyj pełnego logo organizacji (piktogram i logo)</p><p>Utrzymaj wysokość poniżej 30 pikseli, aby uniknąć wprowadzenia pasków przewijania na urządzeniach przenośnych.</p><p>Utrzymaj rozmiar poniżej 4 KB</p><p>Użyj przezroczystego obrazu PNG (nie zakładaj, że strona logowania ma zawsze białe tło)</p> |
| Logo kafelka | Obecnie nieużywane |<p>JPG lub PNG</p><p>120 x 120 pikseli</p><p>10 KB</p> |<p>Zachowaj prostotę (brak małego tekstu), ponieważ rozmiar tego obrazu można zmienić do 50% |
| </p> | | | |
| Etykieta nazwy użytkownika logowania | Obecnie nieużywane |<p>Tekst Unicode, maksymalnie 50 znaków</p><p>Tylko zwykły tekst (bez linków lub tagów HTML)</p> |<p>Powinna być krótka i prosta</p><p>Zapytaj użytkowników, jak zwykle nazywają udostępnione im konto służbowe.</p> |
| Standardowy tekst strony logowania |Ten standardowy tekst pojawia się poniżej formularza strony logowania i może służyć do przekazania dodatkowych instrukcji lub informacji o pomocy i obsłudze technicznej. |<p>Tekst Unicode, maksymalnie 256 znaków</p><p>Tylko zwykły tekst (bez linków lub tagów HTML)</p> |Zachowaj długość nie więcej niż 250 znaków (około 3 wiersze tekstu) |
| Ilustracje w tle strony logowania | Duży obraz, który jest wyświetlany po lewej stronie strony logowania (po prawej stronie w przypadku języków pisanych od prawej do lewej), gdy użytkownicy uzyskują dostęp do adresów URL specyficznych dla dzierżawy. |<p>JPG lub PNG</p><p>1420 x 1200</p><p>500 KB</p> |<p>1420 x 1200 pikseli</p><p>Ważne: zachowaj jak najmniejszy rozmiar, najlepiej poniżej 200 KB. Jeśli ten obraz jest za duży, pogarsza to wydajność strony logowania, gdy obraz nie jest buforowany</p><p>Ten obraz prawie zawsze zostanie przycięty, aby dopasować go do różnych współczynników proporcji ekranu. Najważniejsze elementy wizualne umieść w lewym górnym rogu.</p> |
| Kolor tła strony logowania | W przypadku połączeń o niskiej przepustowości ten jednolity kolor jest używany zamiast ilustracji w tle. | Wymagany jest kolor RGB w postaci szesnastkowej (przykład: \#FFFFFF) | Sugerujemy wybranie podstawowego koloru banera logo. |

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do usługi Azure Active Directory — wersja Premium](active-directory-get-started-premium.md)
* [Wyświetlanie raportów dostępu i użycia](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/signin-page_before-customization.png
[2]: ./media/active-directory-add-company-branding/signin-page-restricted-app.png
[3]: ./media/active-directory-add-company-branding/signin-page-open-access.png
[4]: ./media/active-directory-add-company-branding/signin-page-external-guest.png
[5]: ./media/active-directory-add-company-branding/which-elements-can-i-customize.png
[6]: ./media/active-directory-add-company-branding/hide-kmsi.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png

