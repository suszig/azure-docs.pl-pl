<properties
    pageTitle="Dodawanie użytkowników z innych katalogów lub firm partnerskich w usłudze Azure Active Directory | Microsoft Azure"
    description="Wyjaśnia, jak dodać użytkowników lub zmienić informacje o użytkowniku w usłudze Azure Active Directory, w tym gości i użytkowników zewnętrznych."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/02/2016"
    ms.author="curtand"/>

# Dodawanie użytkowników z innych katalogów lub firm partnerskich w usłudze Azure Active Directory

W tym artykule opisano sposób dodawania użytkowników z innych katalogów w usłudze Azure Active Directory lub dodawania użytkowników z firm partnerskich. Aby uzyskać informacje dotyczące dodawania nowych użytkowników w organizacji i użytkowników, którzy mają konta Microsoft, zobacz [Dodawanie nowych użytkowników do usługi Azure Active Directory](active-directory-create-users.md). Dodani użytkownicy domyślnie nie mają uprawnień administratora, ale możesz przypisać im role w dowolnym momencie.

## Dodawanie użytkownika

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) przy użyciu konta, które jest administratorem globalnym katalogu.

2. Wybierz usługę **Active Directory**, a następnie otwórz swój katalog.

3. Wybierz kartę **Użytkownicy**, a następnie na pasku poleceń wybierz polecenie **Dodaj użytkownika**.

4. Na stronie **Poinformuj nas o tym użytkowniku** w obszarze **Typ użytkownika** wybierz jedną z następujących opcji:

    - **Użytkownik w innym katalogu usługi Azure AD** — dodaje do katalogu konto użytkownika, które pochodzi z innego katalogu usługi Azure AD. Możesz wybrać użytkownika w innym katalogu tylko wtedy, gdy również jesteś członkiem tego katalogu.
    - **Użytkownicy w firmach partnerskich** — aby zaprosić użytkowników firm partnerskich i autoryzować ich do katalogu (zobacz [Współpraca w usłudze Azure Active Directory B2B](active-directory-b2b-what-is-azure-ad-b2b.md)). Konieczne będzie [przekazanie pliku CSV zawierającego adresy e-mail](active-directory-b2b-references-csv-file-format.md).

6. Na stronie **Profil** użytkownika podaj imię i nazwisko, nazwę przyjazną użytkownikowi i rolę użytkownika z listy **Role**. Aby uzyskać więcej informacji dotyczących ról użytkowników i administratorów, zobacz [Przypisywanie ról administratorów w usłudze Azure AD](active-directory-assign-admin-roles.md). Określ, czy **włączyć usługę Multi-Factor Authentication** dla użytkownika.

7. Na stronie **Uzyskaj hasło tymczasowe** wybierz opcję **Utwórz**.

> [AZURE.IMPORTANT] Jeśli Twoja organizacja korzysta z więcej niż jednej domeny, podczas dodawania konta użytkownika musisz wiedzieć o następujących kwestiach:
>
> - Aby dodać konta użytkowników z tą samą główną nazwą użytkownika (UPN) między domenami, **najpierw** dodaj np. adres geoffgrisso@contoso.onmicrosoft.com, **a następnie** adres geoffgrisso@contoso.com.
> - **Nie dodawaj** adresu geoffgrisso@contoso.com przed dodaniem adresu geoffgrisso@contoso.onmicrosoft.com. Ta kolejność jest ważna, a jej cofnięcie może być kłopotliwe.

Jeśli wprowadzasz zmiany w informacjach dla użytkownika, którego tożsamość jest zsynchronizowana z lokalną usługą Active Directory, nie możesz wprowadzać zmian w informacjach o użytkowniku w klasycznym portalu Azure. Aby zmienić informacje o użytkowniku, użyj narzędzi zarządzania lokalnej usługi Active Directory.

## Dodawanie użytkowników zewnętrznych

Przekazując plik CSV, możesz również dodać użytkowników z innego katalogu usługi Azure AD, do którego należysz, lub z firm partnerskich. Aby dodać użytkownika zewnętrznego, dla opcji **Typ użytkownika** określ, czy jest to **Użytkownik w innym katalogu usługi Microsoft Azure AD**, czy **Użytkownicy w firmach partnerskich**.

Użytkownicy obu typów pochodzą z innego katalogu i są dodawani jako **użytkownicy zewnętrzni**. Użytkownicy zewnętrzni mogą współpracować z innymi użytkownikami w katalogu bez konieczności dodawania nowych kont i poświadczeń. Użytkownicy zewnętrzni są uwierzytelniani za pomocą własnego katalogu macierzystego podczas rejestracji, a uwierzytelnianie działa w przypadku innych katalogów, do których zostali dodani.

## Zarządzanie użytkownikami zewnętrznymi i ograniczenia

Gdy dodajesz do swojego katalogu użytkownika z innego katalogu, użytkownik ten jest użytkownikiem zewnętrznym w Twoim katalogu. Nazwa wyświetlana i nazwa użytkownika są kopiowane z katalogu macierzystego tego użytkownika i są używane dla użytkownika zewnętrznego w Twoim katalogu. Od tego momentu właściwości konta użytkownika zewnętrznego są całkowicie niezależne. Zmiany wprowadzone we właściwościach użytkownika w jego katalogu macierzystym nie są przenoszone do konta użytkownika zewnętrznego w Twoim katalogu.

Jedyne połączenie dwóch kont polega na tym, że użytkownik zawsze jest uwierzytelniany względem swojego katalogu macierzystego lub przy użyciu konta Microsoft. Dlatego nie jest widoczna opcja resetowania hasła lub włączania uwierzytelniania wieloskładnikowego dla użytkownika zewnętrznego. Obecnie zasada uwierzytelniania katalogu macierzystego lub konta Microsoft polega na tym, że tylko jedno z nich jest oceniane w czasie logowania użytkownika.

> [AZURE.NOTE]
> Wciąż możesz zablokować użytkownika zewnętrznego w katalogu, co powoduje zablokowanie dostępu do katalogu.

Jeśli użytkownik zostanie usunięty z katalogu macierzystego lub anuluje swoje konto Microsoft, użytkownik zewnętrzny będzie nadal istnieć w Twoim katalogu. Jednak użytkownik w Twoim katalogu nie będzie mieć dostępu do zasobów, ponieważ nie będzie mógł uwierzytelnić się przy użyciu katalogu macierzystego ani konta Microsoft.

### Usługi obsługujące obecnie dostęp dla użytkowników zewnętrznych usługi Azure AD

- **Klasyczny portal Azure**: umożliwia użytkownikowi zewnętrznemu, który jest administratorem wielu katalogów, zarządzanie każdym z nich.
- **SharePoint Online**: umożliwia użytkownikowi zewnętrznemu dostęp do autoryzowanych zasobów usługi SharePoint Online, jeśli udostępnianie zewnętrzne jest włączone.
- **Dynamics CRM**: umożliwia użytkownikowi zewnętrznemu dostęp do autoryzowanych zasobów programu Dynamics CRM, jeśli użytkownik ma licencję za pośrednictwem programu PowerShell.
- **Dynamics AX**: umożliwia użytkownikowi zewnętrznemu dostęp do autoryzowanych zasobów programu Dynamics AX, jeśli użytkownik ma licencję za pośrednictwem programu PowerShell. Ograniczenia dotyczące [użytkowników zewnętrznych usługi Azure AD](#known-limitations-of-azure-ad-external-users) i [gości](#guest-user-management-and-limitations) również mają zastosowanie w przypadku użytkowników zewnętrznych programu Dynamics AX.

### Znane ograniczenia dotyczące użytkowników zewnętrznych usługi Azure AD

- Użytkownicy zewnętrzni, którzy są administratorami, nie mogą dodawać użytkowników z firm partnerskich do katalogów (współpraca B2B) poza katalogiem macierzystym
- Użytkownicy zewnętrzni nie mogą wyrazić zgody na aplikacje wielodostępne w katalogach poza katalogiem macierzystym
- Usługa Power BI nie obsługuje obecnie dostępu użytkowników zewnętrznych
- Portal pakietu Office nie obsługuje licencji użytkowników zewnętrznych
- W odniesieniu do programu PowerShell usługi Azure AD użytkownicy zewnętrzni logują się do katalogu macierzystego i nie mogą zarządzać katalogami, w których występują jako użytkownicy zewnętrzni


## Co dalej

- [Dodawanie nowych użytkowników do usługi Azure Active Directory](active-directory-create-users.md)
- [Administrowanie usługą Azure AD](active-directory-administer.md)
- [Zarządzanie hasłami w usłudze Azure AD](active-directory-manage-passwords.md)
- [Zarządzanie grupami w usłudze Azure AD](active-directory-manage-groups.md)



<!--HONumber=sep16_HO1-->


