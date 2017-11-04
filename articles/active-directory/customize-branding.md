---
title: "Dostosowywanie strony logowania w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać znakowanie firmowe do platformy Azure strony logowania"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: curtand
ms.reviewer: kexia
custom: it-pro
ms.openlocfilehash: 41f4ceea4842303760c5b156e90bd9e0746a7825
ms.sourcegitcommit: d6ad3203ecc54ab267f40649d3903584ac4db60b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2017
---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Szybki Start: Dodawanie znakowania firmowego do stron logowania w usłudze Azure AD
Aby uniknąć nieporozumień, wiele firm chce zastosować spójny wygląd i zachowanie we wszystkich witrynach sieci Web i usługach, którymi zarządzają. Azure Active Directory (Azure AD) zapewnia tę funkcję, umożliwiając dostosowanie wyglądu strony logowania z logo firmy i niestandardowych schematów kolorów. Strona logowania jest to strona wyświetlana podczas logowania się do usługi Office 365 lub innych aplikacji opartych na sieci web, które używają usługi Azure AD jako dostawcy tożsamości. Użytkownik interakcji z tej strony, aby wprowadzić swoje poświadczenia.

> [!NOTE]
> * Logo firmy jest dostępna tylko w przypadku uaktualnienia do wersji Premium lub podstawowa usługi Azure AD lub ma licencji usługi Office 365. Aby dowiedzieć się, czy funkcja jest obsługiwana przez użytkownika typu licencji, sprawdź [cenową strona informacji o usłudze Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Klienci w Chinach mogą używać wersji Premium i Podstawowa usługi Azure Active Directory za pomocą wystąpienia usługi Azure Active Directory dostępnego na całym świecie. Wersje Premium i Podstawowa usługi Azure Active Directory nie są obecnie obsługiwane w usłudze Microsoft Azure świadczonej przez 21Vianet w Chinach. Aby uzyskać więcej informacji, skontaktuj się z nami na [forum usługi Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customizing-the-sign-in-page"></a>Dostosowywanie strony logowania

<!--You can customize the following elements on the sign-in page: <attach image>-->

Dostosowywanie znakowania wyświetlane na stronie logowania usługi Azure AD, gdy użytkownicy uzyskują dostęp do adresu URL specyficznego dla dzierżawy takich jak firmy [ *https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

Podczas odwiedzania ogólnego adresu URL, takie jak www.office.com, strony logowania nie zawiera firmowe dostosowań, ponieważ system nie może ustalić, który jest użytkownik. Logo firmy będą wyświetlane po użytkowników, wprowadź swój identyfikator użytkownika lub wybierz kafelka użytkownika.

> [!NOTE]
> * Nazwa domeny musi występować jako "Aktywna" w **domen** części portalu Azure, w którym skonfigurowano znakowanie. Aby uzyskać więcej informacji, zobacz [Dodawanie niestandardowej nazwy domeny](add-custom-domain.md).
> * Znakowanie na stronie logowania nie jest przenoszone na stronę logowania do osobistych kont Microsoft. Jeśli firm gości lub pracowników, na których Zaloguj się za pomocą osobistego konta Microsoft, ich strony logowania nie odzwierciedla znakowanie organizacji.


### <a name="banner-logo"></a>Baner logo 

Opis | Ograniczenia | Zalecenia
------- | ------- | ----------
Baner logo jest wyświetlany na przy logowaniu i strony panelu dostępu.<br>Na stronie logowania oznacza to, gdy organizacja użytkownika jest określana, zwykle po wprowadzeniu nazwy użytkownika.  | Przezroczysty JPG lub PNG<br>Maksymalna wysokość: 36 pikseli<br>Maksymalna szerokość: 245 pikseli | W tym miejscu użyć logo organizacji.<br>Użyj przezroczystego obrazu. Nie wolno zakładać, że będzie białe tło.<br>Nie dodawaj wypełnienia wokół logo obraz lub logo będzie wyglądać nieproporcjonalnie małych.

### <a name="username-hint"></a>Wskazówka dotycząca nazwy użytkownika   
Opis | Ograniczenia | Zalecenia
------- | ------- | ----------
Dostosowuje to tekst wskazówki w polu nazwy użytkownika. | Tekst Unicode, maksymalnie 64 znaki.<br>Tylko zwykły tekst | Zaleca się, że nie zostały ustawione to jeśli oczekujesz gościa użytkowników spoza organizacji, aby zalogować się do aplikacji.
            
### <a name="sign-in-page-text"></a>Tekst strony logowania   
Opis | Ograniczenia | Zalecenia
------- | ------- | ----------
To jest wyświetlana w dolnej części formularza logowania i może służyć do komunikowania się dodatkowe informacje, takie jak numer telefonu do działu pomocy technicznej lub klauzulę prawną. | Tekst Unicode, maksymalnie 256 znaków<br>Tylko zwykły tekst (bez linków lub tagów HTML)   

### <a name="sign-in-page-image"></a>Obraz strony logowania  
Opis | Ograniczenia | Zalecenia
------- | ------- | ----------
To pojawia się w tle strony logowania, jest zakotwiczona centrum obszaru wyświetlanego będzie skalować i zbioru do wypełnienia okna przeglądarki.    <br>Ten obraz nie jest wyświetlany na wąskich ekranach takich jak telefony komórkowe.<br>Czarne maski z 0.55 nieprzezroczystość zostaną zastosowane za pośrednictwem tego obrazu przez naszego kodu podczas ładowania strony. | JPG lub PNG<br>Wymiary obrazu: 1920 x 1080 pikseli<br>Rozmiar pliku: &lt; 300 KB | <br>Obrazy w przypadku, gdy nie ma fokusu silne podmiotu. Nieprzezroczyste formularz logowania zostanie wyświetlony w Centrum ten obraz i może obejmować dowolną część obrazu, w zależności od rozmiaru okna przeglądarki.<br>Zachowaj jak można zapewnić szybkie ładować rozmiar pliku. 

### <a name="background-color"></a>Kolor tła
Opis | Ograniczenia | Zalecenia
------- | ------- | ----------
Kolor używany zamiast obraz tła połączeń o niskiej przepustowości. | Kolor RGB w formacie szesnastkowym (przykład: #FFFFFF | Zaleca się przy użyciu podstawowego koloru baneru logo lub kolor Twojej organizacji.

### <a name="show-option-to-remain-signed-in"></a>Pokaż opcję, aby wylogować
Opis | Ograniczenia | Zalecenia
------- | ------- | ----------
Konta usługi Azure AD zapewnia opcję, aby wylogować po zamknięciu i ponownie otwórz w przeglądarce. Służy do ukrycia tej opcji.<br>Ustaw tę wartość na "Nie", aby ukryć tej opcji od użytkowników. | &nbsp; | Nie ma to wpływu na okres istnienia sesji.<br>Niektóre funkcje pakietu Office 2010 i SharePoint Online są zależne od użytkownicy będą mogli wybrać wylogować. Jeśli ustawisz ukryte, użytkownicy mogą pojawić dodatkowe i nieoczekiwane monit logowania.

> [!NOTE]
> Wszystkie elementy są opcjonalne. Na przykład jeśli określisz Baner logo z żadnego obrazu tła strony logowania zostaną wyświetlone logo i obraz tła dla lokacji docelowej (na przykład usługi Office 365).

## <a name="add-company-branding-to-your-directory"></a>Dodawanie znakowania firmowego do katalogu

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie wybierz **Enter**.

   ![Otwieranie Zarządzanie użytkownikami](./media/customize-branding/user-management.png)
3. Na **użytkowników i grup** bloku, wybierz opcję **firmy znakowania**.
4. Na **użytkowników i grup - firmy znakowania** bloku, wybierz opcję **Edytuj** polecenia.

    ![Edytuj niestandardowe znakowanie](./media/customize-branding/edit-branding.png)
5. Zmodyfikuj elementy, które chcesz dostosować. Wszystkie elementy są opcjonalne.
6. Kliknij pozycję **Zapisz**.

Może potrwać do godziny wszelkie zmiany wprowadzone do strony logowania znakowania na.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Dodaj specyficzne dla języka znakowanie firmowe do katalogu

1. Zaloguj się w [centrum administracyjnym usługi Azure AD](https://aad.portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz **użytkowników i grup** w polu tekstowym, a następnie wybierz **Enter**.

   ![Otwieranie Zarządzanie użytkownikami](./media/customize-branding/user-management.png)
3. Na **użytkowników i grup** bloku, wybierz opcję **firmy znakowania**.
4. Na **użytkowników i grup - firmy znakowania** bloku, wybierz opcję **Dodaj język** polecenia.

    ![Dodawanie znakowania elementy specyficzne dla języka](./media/customize-branding/add-language.png)
5. Zmodyfikuj elementy, które chcesz dostosować. Wszystkie elementy są opcjonalne.
6. Kliknij pozycję **Zapisz**.

Może potrwać do godziny wszelkie zmiany wprowadzone do strony logowania znakowania na.

## <a name="next-steps"></a>Następne kroki
W tym szybkiego startu kiedy znasz już jak dodać znakowanie firmowe do katalogu usługi Azure AD. 

Poniższe łącze służy do konfigurowania firmowe w usłudze Azure AD z portalu Azure.

> [!div class="nextstepaction"]
> [Configure company branding (Konfigurowanie oznaczenia marką firmy)](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 