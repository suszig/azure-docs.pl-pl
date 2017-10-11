---
title: "Dodawanie użytkowników z innych katalogów lub firm partnerskich w usłudze Azure Active Directory | Microsoft Docs"
description: "Wyjaśnia, jak dodać użytkowników lub zmienić informacje o użytkowniku w usłudze Azure Active Directory, w tym gości i użytkowników zewnętrznych."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 564a04ec-53c1-470b-9ab9-f3db57da0a89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 399230584d01986dd0f793a6ff8245ef2b4f8fb1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory"></a>Dodawanie użytkowników z innych katalogów lub firm partnerskich w usłudze Azure Active Directory

W tym artykule opisano sposób dodawania użytkowników z innych katalogów w usłudze Azure Active Directory lub dodawania użytkowników z firm partnerskich. Aby uzyskać informacje dotyczące dodawania nowych użytkowników w organizacji i użytkowników, którzy mają konta Microsoft, zobacz [Dodawanie nowych użytkowników do usługi Azure Active Directory](active-directory-create-users.md). 

> [!IMPORTANT]
> Firma Microsoft zaleca zarządzanie usługą Azure AD przy użyciu [centrum administracyjnego usługi Azure AD](https://aad.portal.azure.com) w witrynie Azure Portal zamiast korzystania z klasycznej witryny Azure Portal przywołanej w niniejszym artykule. Aby dowiedzieć się, jak dodać użytkowników gości współpracy B2B w centrum administracyjnym usługi Azure AD, zobacz [Czym jest współpraca B2B w usłudze Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)

Dodani użytkownicy domyślnie nie mają uprawnień administratora, ale możesz przypisać im role w dowolnym momencie.

## <a name="add-a-user"></a>Dodawanie użytkownika
1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz usługę **Active Directory**, a następnie otwórz swój katalog.
3. Wybierz kartę **Użytkownicy**, a następnie na pasku poleceń wybierz polecenie **Dodaj użytkownika**.
4. Na stronie **Poinformuj nas o tym użytkowniku** w obszarze **Typ użytkownika** wybierz jedną z następujących opcji:

   * **Użytkownik w innym katalogu usługi Azure AD** — dodaje do katalogu konto użytkownika, które pochodzi z innego katalogu usługi Azure AD. Możesz wybrać użytkownika w innym katalogu tylko wtedy, gdy również jesteś członkiem tego katalogu.
   * **Użytkownicy w firmach partnerskich** — aby zaprosić użytkowników firm partnerskich i autoryzować ich do katalogu (zobacz [Współpraca w usłudze Azure Active Directory B2B](active-directory-b2b-what-is-azure-ad-b2b.md)). Konieczne będzie [przekazanie pliku CSV zawierającego adresy e-mail](active-directory-b2b-references-csv-file-format.md).
5. Na stronie **Profil** użytkownika podaj imię i nazwisko, nazwę przyjazną użytkownikowi i rolę użytkownika z listy **Role**. Aby uzyskać więcej informacji dotyczących ról użytkowników i administratorów, zobacz [Przypisywanie ról administratorów w usłudze Azure AD](active-directory-assign-admin-roles.md). Określ, czy **włączyć usługę Multi-Factor Authentication** dla użytkownika.
6. Na stronie **Uzyskaj hasło tymczasowe** wybierz opcję **Utwórz**.

> [!IMPORTANT]
> Jeśli Twoja organizacja korzysta z więcej niż jednej domeny, podczas dodawania konta użytkownika musisz wiedzieć o następujących kwestiach:
>
> * Aby dodać konta użytkowników z tą samą główną nazwą użytkownika (UPN) między domenami, **najpierw** dodaj np. adres geoffgrisso@contoso.onmicrosoft.com, **a następnie** geoffgrisso@contoso.com.
> * **Nie** dodawaj adresu geoffgrisso@contoso.com przed dodaniem adresu geoffgrisso@contoso.onmicrosoft.com.
>

Jeśli wprowadzasz zmiany w informacjach dla użytkownika, którego tożsamość jest zsynchronizowana z lokalną usługą Active Directory, nie możesz wprowadzać zmian w informacjach o użytkowniku w klasycznym portalu Azure. Aby zmienić informacje o użytkowniku, użyj narzędzi zarządzania lokalnej usługi Active Directory.

## <a name="add-external-users"></a>Dodawanie użytkowników zewnętrznych
Przekazując plik CSV, możesz również dodać użytkowników z innego katalogu usługi Azure AD, do którego należysz, lub z firm partnerskich. Aby dodać użytkownika zewnętrznego, dla opcji **Typ użytkownika** określ, czy jest to **Użytkownik w innym katalogu usługi Microsoft Azure AD**, czy **Użytkownicy w firmach partnerskich**.

Użytkownicy obu typów pochodzą z innego katalogu i są dodawani jako **użytkownicy zewnętrzni**. Użytkownicy zewnętrzni mogą współpracować z innymi użytkownikami w katalogu bez konieczności dodawania nowych kont i poświadczeń. Użytkownicy zewnętrzni są uwierzytelniani za pomocą własnego katalogu macierzystego podczas rejestracji, a uwierzytelnianie działa w przypadku innych katalogów, do których zostali dodani.

## <a name="external-user-management-and-limitations"></a>Zarządzanie użytkownikami zewnętrznymi i ograniczenia
Gdy dodajesz do swojego katalogu użytkownika z innego katalogu, użytkownik ten jest użytkownikiem zewnętrznym w Twoim katalogu. Nazwa wyświetlana i nazwa użytkownika są kopiowane z katalogu macierzystego tego użytkownika i są używane dla użytkownika zewnętrznego w Twoim katalogu. Od tego momentu właściwości konta użytkownika zewnętrznego są całkowicie niezależne. Zmiany wprowadzone we właściwościach użytkownika w jego katalogu macierzystym nie są przenoszone do konta użytkownika zewnętrznego w Twoim katalogu.

Jedyne połączenie dwóch kont polega na tym, że użytkownik zawsze jest uwierzytelniany względem swojego katalogu macierzystego lub przy użyciu konta Microsoft. Dlatego nie jest widoczna opcja resetowania hasła lub włączania uwierzytelniania wieloskładnikowego dla użytkownika zewnętrznego. Obecnie zasada uwierzytelniania katalogu macierzystego lub konta Microsoft polega na tym, że tylko jedno z nich jest oceniane w czasie logowania użytkownika.

> [!NOTE]
> Wciąż możesz zablokować użytkownika zewnętrznego w katalogu, co powoduje zablokowanie dostępu do katalogu.
>
>

Jeśli użytkownik zostanie usunięty z katalogu macierzystego lub anuluje swoje konto Microsoft, użytkownik zewnętrzny będzie nadal istnieć w Twoim katalogu. Jednak użytkownik w Twoim katalogu nie będzie mieć dostępu do zasobów, ponieważ nie będzie mógł uwierzytelnić się przy użyciu katalogu macierzystego ani konta Microsoft.

### <a name="services-that-currently-support-access-by-azure-ad-external-users"></a>Usługi obsługujące obecnie dostęp dla użytkowników zewnętrznych usługi Azure AD
* **Klasyczny portal Azure**: umożliwia użytkownikowi zewnętrznemu, który jest administratorem wielu katalogów, zarządzanie każdym z nich.
* **SharePoint Online**: umożliwia użytkownikowi zewnętrznemu dostęp do autoryzowanych zasobów usługi SharePoint Online, jeśli udostępnianie zewnętrzne jest włączone.
* **Dynamics CRM**: umożliwia użytkownikowi zewnętrznemu dostęp do autoryzowanych zasobów programu Dynamics CRM, jeśli użytkownik ma licencję za pośrednictwem programu PowerShell.
* **Dynamics AX**: umożliwia użytkownikowi zewnętrznemu dostęp do autoryzowanych zasobów programu Dynamics AX, jeśli użytkownik ma licencję za pośrednictwem programu PowerShell. Ograniczenia dotyczące [użytkowników zewnętrznych usługi Azure AD](#known-limitations-of-azure-ad-external-users) mają również zastosowanie w przypadku użytkowników zewnętrznych programu Dynamics AX.

## <a name="next-steps"></a>Następne kroki
* [Dodawanie nowych użytkowników do usługi Azure Active Directory](active-directory-create-users.md)
* [Administrowanie usługą Azure AD](active-directory-administer.md)
* [Zarządzanie hasłami w usłudze Azure AD](active-directory-manage-passwords.md)
* [Zarządzanie grupami w usłudze Azure AD](active-directory-manage-groups.md)
