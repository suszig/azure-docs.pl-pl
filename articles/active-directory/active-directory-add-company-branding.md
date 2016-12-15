---
title: "Dodawanie znakowania firmowego do stron logowania i panelu dostępu"
description: "Dowiedz się, jak dodać znakowanie firmowe do strony logowania i strony panelu dostępu platformy Azure"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f74621b4-4ef0-4899-8c0e-0c20347a8c31
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/30/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 98c8352152b6cd1817d32c6418597c566d94d44f


---
# <a name="add-company-branding-to-your-sign-in-and-access-panel-pages"></a>Dodawanie znakowania firmowego do stron logowania i panelu dostępu
Aby uniknąć nieporozumień, wiele firm chce zastosować spójny wygląd i zachowanie we wszystkich witrynach sieci Web i usługach, którymi zarządzają. Usługa Azure Active Directory zapewnia tę funkcję, umożliwiając dostosowanie wyglądu następujących stron sieci Web przez dodanie logo firmy i niestandardowych schematów kolorów:

* **Strona logowania** — jest to strona wyświetlana podczas logowania się do usługi Office 365 lub innych aplikacji sieci Web, które używają usługi Azure AD jako dostawcy tożsamości. W interakcję z tą stroną wchodzisz podczas wykrywania obszaru głównego lub wprowadzania poświadczeń. Wykrywanie obszaru głównego umożliwia systemowi przekierowanie użytkowników federacyjnych do ich lokalnej usługi STS (takiej jak usługi AD FS).
* **Strona panelu dostępu** — panel dostępu jest portalem sieci Web umożliwiającym wyświetlanie i uruchamianie aplikacji opartych na chmurze, do których administrator usługi Azure AD udzielił Ci dostępu. Aby otworzyć panel dostępu, użyj następującego adresu URL: [https://myapps.microsoft.com](https://myapps.microsoft.com).

W tym temacie wyjaśniono, jak można dostosować stronę logowania i stronę panelu dostępu.

> [!NOTE]
> * Umieszczanie logo firmy jest funkcją dostępną tylko dla użytkowników usługi Office 365 lub osób, które dokonały uaktualnienia do wersji Premium lub Podstawowa usługi Azure Active Directory. Aby uzyskać więcej informacji, zobacz [Wersje usługi Azure Active Directory](active-directory-editions.md).
> * Klienci w Chinach mogą używać wersji Premium i Podstawowa usługi Azure Active Directory za pomocą wystąpienia usługi Azure Active Directory dostępnego na całym świecie. Wersje Premium i Podstawowa usługi Azure Active Directory nie są obecnie obsługiwane w usłudze Microsoft Azure świadczonej przez 21Vianet w Chinach. Aby uzyskać więcej informacji, skontaktuj się z nami na [forum usługi Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).
> 
> 

## <a name="customizing-the-sign-in-page"></a>Dostosowywanie strony logowania
Zwykle na potrzeby dostępu opartego na przeglądarce do aplikacji i usług w chmurze subskrybowanych przez organizację używana jest strona logowania.

W przypadku zastosowania zmian do strony logowania uwzględnienie ich może zająć godzinę.

Strona logowania z oznaczeniami firmowymi jest wyświetlana tylko podczas odwiedzania usługi za pomocą adresu URL specyficznego dla dzierżawy, takiego jak https://outlook.com/**contoso**.com lub https://mail.**contoso**.com.

W przypadku odwiedzania usługi za pomocą adresów URL innych niż specyficzne dla dzierżawy (np. https://mail.office365.com) wyświetlana jest strona logowania bez firmowego znakowania. W takim przypadku znakowanie zostanie wyświetlone po wprowadzeniu identyfikatora użytkownika lub wybraniu kafelka użytkownika.

> [!NOTE]
> * Nazwa domeny musi być widoczna jako „Aktywna” w sekcji **Active Directory** > **Katalog** > **Domeny** klasycznego portalu Azure, w którym skonfigurowano znakowanie.
> * Znakowanie strony logowania nie jest przenoszone na stronę logowania klienta firmy Microsoft. Jeśli logujesz się za pomocą osobistego konta Microsoft, może być wyświetlana lista kafelków użytkowników renderowana przez usługę Azure AD, ale znakowanie organizacji nie ma zastosowania do strony logowania konta Microsoft.
> 
> 

Jeśli chcesz wyświetlać swoje firmowe oznaczenia, kolory i inne dostosowywalne elementy na tej stronie, zobacz następujące ilustracje, aby zrozumieć różnicę między obydwoma środowiskami.

Poniższy zrzut ekranu przedstawia przykład strony logowania usługi Office 365 na komputerze stacjonarnym **przed** dostosowaniem:

![Strona logowania usługi Office 365 przed dostosowaniem][1]

Poniższy zrzut ekranu przedstawia przykład strony logowania usługi Office 365 na komputerze stacjonarnym **po** dostosowaniu:

![Strona logowania usługi Office 365 po dostosowaniu][2]

Poniższy zrzut ekranu przedstawia przykład strony logowania usługi Office 365 na urządzeniu przenośnym **przed** dostosowaniem:

![Strona logowania usługi Office 365 przed dostosowaniem][3]

Poniższy zrzut ekranu przedstawia przykład strony logowania usługi Office 365 na urządzeniu przenośnym **po** dostosowaniu:

![Strona logowania usługi Office 365 po dostosowaniu][4]

Podczas zmiany rozmiaru okna przeglądarki duża ilustracja, taka jak ta pokazana wcześniej, jest często przycinana w celu dopasowania do różnych współczynników proporcji ekranu. Pamiętając o tym, należy starać się zachować kluczowe elementy wizualne na ilustracji tak, aby zawsze były wyświetlane w lewym górnym rogu (prawym górnym w przypadku języków z zapisem od prawej do lewej). Jest to ważne, ponieważ zmiana rozmiaru zwykle odbywa się od prawego dolnego rogu w kierunku do góry/do lewej lub z dołu do góry.

Na poniższym obrazku przedstawiono sposób przycięcia ilustracji przy zmianie rozmiaru przeglądarki w kierunku do lewej:

![][6]

A oto jak ilustracja wygląda po zmianie rozmiaru przeglądarki w kierunku do góry:

![][7]

## <a name="what-elements-on-the-page-can-i-customize"></a>Jakie elementy na stronie można dostosować?
Na stronie logowania można dostosować następujące elementy:

![][5]

| Element strony | Lokalizacja na stronie |
|:--- | --- |
| Baner logo |Wyświetlany w prawym górnym rogu strony. Zastępuje logo lokacji docelowej, do której się logujesz (na przykład Office 365 lub Azure). |
| Duża ilustracja/kolor tła |Wyświetlany w lewej części strony. Zastępuje obraz lokacji docelowej, do której się logujesz. W przypadku połączeń o niskiej przepustowości lub na wąskich ekranach zamiast dużej ilustracji może być wyświetlany kolor tła. |
| Nie wylogowuj mnie |Tekst wyświetlany pod polem tekstowym Hasło. |
| Tekst strony logowania |Wyświetlany nad stopką strony, gdy chcesz przekazać pomocne informacje przed zalogowaniem się przy użyciu konta służbowego. Na przykład możesz dołączyć numer telefonu działu pomocy technicznej lub klauzulę prawną. |

> [!NOTE]
> Wszystkie elementy są opcjonalne. Na przykład jeśli określisz baner logo, ale nie określisz dużej ilustracji, na stronie logowania będzie pokazywane dostosowane logo oraz ilustracja dla lokacji docelowej (czyli obraz kalifornijskiej autostrady dla usługi Office 365).
> 
> 

Na stronie logowania pole wyboru **Nie wylogowuj mnie** umożliwia użytkownikom zachowanie stanu zalogowania w przypadku zamknięcia i ponownego otworzenia przeglądarki. Opcja nie ma wpływu na okres istnienia sesji. Pole wyboru na stronie logowania usługi Azure Active Directory możesz ukryć.

Wyświetlanie pola wyboru zależy od ustawienia **Ukryj KMSI**.

![][9]

Aby ukryć pole wyboru, skonfiguruj ustawienie, wybierając opcję **Ukryte**. 

> [!NOTE]
> Niektóre funkcje usługi SharePoint Online oraz pakietu Office 2010 zależą od możliwości skorzystania z tego pola wyboru przez użytkowników. Jeśli skonfigurujesz to ustawienia na wartość „Ukryte”, użytkownicy mogą otrzymywać dodatkowe i nieoczekiwane monity o logowanie.
> 
> 

Możesz również dostosować wszystkie elementy na tej stronie. Po skonfigurowaniu „domyślnego” zestawu elementów dostosowania można skonfigurować więcej wersji dla różnych ustawień regionalnych. Możesz także mieszać i dopasowywać różne elementy. Można na przykład:

* Utworzyć „domyślną” dużą ilustrację działającą dla wszystkich języków, a następnie utworzyć specyficzne wersje dla angielskiego i francuskiego. W przypadku ustawienia w przeglądarce jednego z tych dwóch języków wyświetlany będzie określony obraz, podczas gdy ilustracja domyślna będzie wyświetlana dla wszystkich pozostałych języków.
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
3. Na pasku narzędzi u góry kliknij przycisk **Konfiguruj**.
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

## <a name="testing-and-examples"></a>Testowanie i przykłady
Zalecamy wypróbowanie dostosowań za pomocą dzierżawy testowej przed wprowadzeniem zmian w środowisku produkcyjnym.

**Aby sprawdzić, czy znakowanie zostało zastosowane:**

1. Otwórz sesję InPrivate lub Incognito przeglądarki.
2. Odwiedź stronę https://outlook.com/contoso.com, zastępując contoso.com dostosowaną domeną.

Ta metoda działa również z domenami, które mają postać typu contoso.onmicrosoft.com.

Aby ułatwić utworzenie efektywnych zestawów dostosowań, dostosowaliśmy dwie następujące fikcyjne strony logowania:

* [http://aka.ms/aaddemo001](http://aka.ms/aaddemo001)
* [http://aka.ms/aaddemo002](http://aka.ms/aaddemo002)

Aby przetestować ustawienia specyficzne dla języka, należy zmienić domyślne preferencje języka w przeglądarce sieci Web na język ustawiony w dostosowaniu. W programie Internet Explorer do konfigurowania tych preferencji służy menu **Opcje internetowe**.

## <a name="customizable-elements"></a>Elementy dostosowywalne
Niektóre elementy dostosowywalne w usłudze Azure AD mają wiele zastosowań. Można skonfigurować logo firmy raz dla katalogu i będzie ono używane na obu stronach — logowania i panelu dostępu. Niektóre elementy dostosowywalne są specyficzne tylko dla strony logowania. Poniższa tabela zawiera szczegółowe informacje dotyczące różnych elementów dostosowywalnych.

| Nazwa | Opis | Ograniczenia | Zalecenia |
| --- | --- | --- | --- |
| Baner logo |Baner logo jest wyświetlany na stronie logowania i stronie panelu dostępu. |<p>JPG lub PNG</p><p>60 x 280 pikseli</p><p>10 KB</p> |<p>Użyj pełnego logo organizacji (piktogram i logo)</p><p>Utrzymaj wysokość poniżej 30 pikseli, aby uniknąć wprowadzenia pasków przewijania na urządzeniach przenośnych.</p><p>Utrzymaj rozmiar poniżej 4 KB</p><p>Użyj przezroczystego obrazu PNG (nie zakładaj, że strona logowania ma zawsze białe tło)</p> |
| Logo kafelka |(aktualnie nieużywane na stronie logowania). W przyszłości ten tekst może być używany do zastąpienia ogólnego piktogramu „konto służbowe” w różnych miejscach w środowisku. |<p>JPG lub PNG</p><p>120 x 120 pikseli</p><p>10 KB</p> |<p>Zachowaj prostotę (brak małego tekstu), ponieważ rozmiar tego obrazu można zmienić do 50% |
| </p> | | | |
| Etykieta nazwy użytkownika strony logowania |(aktualnie nieużywana na stronie logowania). W przyszłości ten tekst może być używany do zastąpienia ogólnego ciągu „konto służbowe” w różnych miejscach w środowisku. Można ustawić tekst w rodzaju „Konto Contoso” lub „Identyfikator Contoso”. |<p>Tekst Unicode, maksymalnie 50 znaków</p><p>Tylko zwykły tekst (bez linków lub tagów HTML)</p> |<p>Powinna być krótka i prosta</p><p>Zapytaj użytkowników, jak zwykle nazywają udostępnione im konto służbowe.</p> |
| Tekst strony logowania |Ten tekst „standardowy” pojawia się poniżej formularza strony logowania i może służyć do przekazania dodatkowych instrukcji lub informacji o pomocy i obsłudze technicznej. |<p>Tekst Unicode, maksymalnie 256 znaków</p><p>Tylko zwykły tekst (bez linków lub tagów HTML)</p> |Zachowaj długość nie więcej niż 250 znaków (około 3 wiersze tekstu) |
| Ilustracja strony logowania |Ilustracja to duży obraz wyświetlany na stronie logowania na lewo od formularza strony logowania. |<p>JPG lub PNG</p><p>1420 x 1200</p><p>500 KB</p> |<p>1420 x 1200 pikseli</p><p>Ważne: zachowaj jak najmniejszy rozmiar, najlepiej poniżej 200 KB. Jeśli ten obraz jest za duży, pogarsza to wydajność strony logowania, gdy obraz nie jest buforowany</p><p>Ten obraz jest często przycinany w celu dopasowania do różnych współczynników proporcji ekranu. Zachowaj podstawowe elementy wizualne w lewym górnym rogu (prawym górnym dla języków z zapisem od prawej do lewej), ponieważ zmiana rozmiaru podczas zmniejszania okna przeglądarki następuje od prawego dolnego rogu w kierunku do góry/do lewej.</p> |
| Kolor tła strony logowania |Kolor tła strony logowania jest używany w obszarze na lewo od formularza strony logowania. |Wymagany jest kolor RGB w postaci szesnastkowej (przykład: #FFFFFF) |<p>W przypadku połączeń o niskiej przepustowości zamiast dużej ilustracji może być wyświetlany kolor tła.</p><p>Sugerujemy wybranie podstawowego koloru baneru logo</p> |

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do usługi Azure Active Directory — wersja Premium](active-directory-get-started-premium.md)
* [Wyświetlanie raportów dostępu i użycia](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/SignInPage_beforecustomization.png
[2]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization.png
[3]: ./media/active-directory-add-company-branding/SignInPage_mobile_beforecustomization.png
[4]: ./media/active-directory-add-company-branding/SignInPage_mobile_aftercustomization.png
[5]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_elements.png
[6]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedleft.png
[7]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedtop.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png



<!--HONumber=Dec16_HO1-->


