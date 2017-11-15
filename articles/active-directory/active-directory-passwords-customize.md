---
title: 'Dostosowywanie: Azure AD SSPR | Dokumentacja firmy Microsoft'
description: "Dostosowywanie opcji dla usługi Azure AD samodzielnego resetowania hasła usługi"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: f2b172208185e343c9c10d55036c20d60346778c
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a>Dostosowywanie funkcji usługi Azure AD dla samoobsługowego resetowania hasła

Specjaliści IT, które chcą wdrożyć Samoobsługowe Resetowanie hasła można dostosować środowiska, aby dopasować użytkowników.

## <a name="customize-the-contact-your-administrator-link"></a>Dostosowywanie kontaktu link do administratora

Nawet jeśli nie włączono funkcji SSPR portal resetowania użytkownicy nadal "Skontaktuj się z administratorem" łącze hasło.  Kliknięcie tego łącza wiadomości e-mail z prośbą o pomoc w odniesieniu do zmiany hasła administratorami lub wysyła użytkowników do adresu URL, który określisz. Firma Microsoft zaleca, ustaw tę inną takie jak adres e-mail lub witrynę sieci Web użytkownikom są używane do korzystania z pomocy technicznej.

![Skontaktuj się z][Contact]

Ten adres e-mail jest wysyłane do następujących adresatów w następującej kolejności:

1. Jeśli **hasło administratora** rola jest przypisywana, administratorów z tej roli są powiadomienia.
2. Jeśli hasło administratorów nie ma przypisanych, następnie Administratorzy z **użytkownika administratora** są powiadamiani o roli
3. Jeśli żaden z poprzednich role zostały przypisane, następnie **Administratorzy globalni** są powiadamiani o

We wszystkich przypadkach są powiadamiani o maksymalnie 100 adresatów.

Aby dowiedzieć się więcej o różnych administrator ról i przypisywania im można znaleźć w dokumencie [przypisywanie ról administratorów w usłudze Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)

### <a name="disable-contact-your-administrator-emails"></a>Wyłącz skontaktuj się z administratorem wiadomości e-mail

Jeśli Twoja organizacja nie chce żądań resetowania Administratorzy otrzymywać powiadomienia o hasło, można włączyć następującą konfigurację

* Włącz samoobsługowego resetowania haseł dla wszystkich użytkowników końcowych. Ta opcja jest w obszarze **resetowania hasła > właściwości**.
    * Jeśli nie chcesz, aby użytkownikom na resetowanie własnych haseł, można określić zakres dostępu do pustej grupy **nie zaleca się opcja**.
* Dostosowywanie link pomocy technicznej, aby podać adres URL sieci web lub mailto: adres, który użytkownicy mogą używać, aby uzyskać pomoc. Ta opcja jest w obszarze **resetowania hasła > dostosowania > niestandardowe pomoc techniczna e-mail lub adres URL**.

## <a name="customize-adfs-sign-in-page-for-sspr"></a>Dostosowywanie strony logowania usług AD FS dla usługi SSPR

Administratorzy usług AD FS można dodać łącza do strony przy użyciu wskazówek znaleźć w artykule w ich logowania [dodawanie opisu strony logowania](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).

Za pomocą polecenia znajdujący się na serwerze usług AD FS dodaje łącze do strony logowania usług AD FS, dzięki czemu użytkownicy mogą wprowadzić hasło samoobsługi resetowania bezpośrednio przepływu pracy.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-and-access-panel-look-and-feel"></a>Dostosowywanie logowania i dostępu do panelu Wygląd i działanie

Gdy użytkownicy uzyskują dostęp do strony logowania, można dostosować logo, które pojawia się wraz z obrazem strony logowania, aby dopasować znakowaniu firmy.

Elementy te są wyświetlane w następujących okolicznościach:

* Po użytkownik wpisuje swoją nazwę użytkownika
* Użytkownik uzyskuje dostęp do adresu url dostosowane
    * Przez przekazanie "godz. pracy" strony, takich jak "https://login.microsoftonline.com/?whr=contoso.com" resetowania parametr hasło
    * Przez przekazanie "nazwa_użytkownika" Parametr hasło strony resetowania, tak samo, jak "https://login.microsoftonline.com/?username=admin@contoso.com"

### <a name="graphics-details"></a>Szczegóły grafiki

Następujące ustawienia pozwalają na zmiany wizualne cechy strony logowania i znajduje się w obszarze **usługi Azure Active Directory**, **firmy znakowania**, **znakowanie firmowe do edycji**

* Obraz strony logowania powinien być PNG lub JPG pliku 1420 x 1200 pikseli i nie większa niż 500KB. Firma Microsoft zaleca, aby wartość była około 200 KB w celu uzyskania najlepszych wyników.
* Kolor tła strony logowania jest używany podczas połaczeń dużymi opóźnieniami i musi być w formacie szesnastkowym RGB.
* Transparent obraz powinien być pikseli 60 x 280 plik PNG lub JPG i nie większa niż 10 KB.
* Kwadratowe logo (normalne i ciemnego motywu) PNG lub JPG 240 x 240 (rozmiar) nie większą niż 10 KB.

### <a name="sign-in-text-options"></a>Opcje logowania tekstu

Następujące ustawienia pozwalają na dodawanie tekstu do strony logowania istotne dla Twojej organizacji. Te ustawienia można znaleźć w **usługi Azure Active Directory**, **firmy znakowania**, **znakowanie firmowe do edycji**

* **Wskazówka nazwy użytkownika** zastępuje tekst przykład someone@example.com na coś bardziej odpowiednie dla użytkowników, zaleca, aby pozostawić domyślne podczas obsługi użytkowników wewnętrznych i zewnętrznych
* **Tekst strony logowania** maksymalnie 256 znaków. Ten tekst jest wyświetlany wszędzie logowanie użytkowników online i w środowisku Azure AD Join w systemie Windows 10. Użyj tego tekstu warunków użytkowania, instrukcje i wskazówki dla użytkowników. **Każda osoba, która jest zobacz stronę logowania, więc nie zawierają żadnych poufnych informacji.**

### <a name="keep-me-signed-in-disabled"></a>Wylogowuj mnie wyłączone

Opcji "Zachowaj wylogowuj mnie wyłączone" umożliwia użytkownikom wylogować po zamknięciu i ponownie otworzyć ich okna przeglądarki. Ta opcja nie będzie mieć wpływu okresy istnienia sesji. To ustawienie znajduje się w obszarze **usługi Azure Active Directory > firmy znakowania > Edytuj firmowe**.

Niektóre funkcje pakietu Office 2010 i SharePoint Online zależy od użytkownicy będą mogli przeprowadzić zaznacz to pole wyboru. Ukrycie tę opcję, użytkownicy mogą pobrać dodatkowe i nieoczekiwane logowania monitów.

### <a name="directory-name"></a>Nazwa katalogu

Można zmienić atrybutu nazwy w obszarze **usługi Azure Active Directory > właściwości** pokazanie organizacji przyjazną nazwę widoczne w portalu i automatyczne komunikację. Ta opcja jest najbardziej widoczne w formularzu zautomatyzowane wiadomości e-mail w kolejnych formularzach

* Przyjazna nazwa w wiadomości e-mail "Microsoft w imieniu pokaz firmy CONTOSO"
* Wiersz tematu wiadomości e-mail "CONTOSO demonstracyjna konta e-mail kod weryfikacyjny"

## <a name="next-steps"></a>Następne kroki

* [Jak wykonać pomyślne wdrożenie funkcji samoobsługowego resetowania haseł?](active-directory-passwords-best-practices.md)
* [Resetowanie lub zmienianie hasła](active-directory-passwords-update-your-own-password.md).
* [Rejestrowanie na potrzeby samoobsługowego resetowania haseł](active-directory-passwords-reset-register.md).
* [Czy masz pytanie dotyczące licencjonowania?](active-directory-passwords-licensing.md)
* [Jakie dane są używane przez funkcję samoobsługowego resetowania haseł i jakie dane powinny zostać wypełnione dla użytkowników?](active-directory-passwords-data.md)
* [Jakie metody uwierzytelniania są dostępne dla użytkowników?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jakie są opcje zasad dla funkcji samoobsługowego resetowania haseł?](active-directory-passwords-policy.md)
* [Co to jest funkcja zapisywania zwrotnego haseł i dlaczego jest ona tak ważna?](active-directory-passwords-writeback.md)
* [Jak zgłosić działanie funkcji samoobsługowego resetowania haseł?](active-directory-passwords-reporting.md)
* [Jakie są dostępne opcje funkcji samoobsługowego resetowania haseł i do czego one służą?](active-directory-passwords-how-it-works.md)
* [Myślę, że coś działa niewłaściwie. Jak rozwiązywać problemy z funkcją samoobsługowego resetowania haseł?](active-directory-passwords-troubleshoot.md)
* [Mam pytanie, na które nie mogę znaleźć odpowiedzi](active-directory-passwords-faq.md)

[Contact]: ./media/active-directory-passwords-customize/sspr-contact-admin.png "Skontaktuj się z administratorem, aby uzyskać pomoc zresetowanie Twojego hasła przykład poczty e-mail"