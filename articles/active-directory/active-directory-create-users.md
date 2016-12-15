---
title: "Dodawanie nowych użytkowników do usługi Azure Active Directory | Microsoft Docs"
description: "Wyjaśnia, jak dodać nowych użytkowników lub zmienić informacje o użytkowniku w usłudze Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e3673727-6bec-4fdc-87a4-d65b213c4c3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/22/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f3787f72dbd8ee865899b71538816d2e8d30af32


---
# <a name="add-new-users-or-users-with-microsoft-accounts-to-azure-active-directory"></a>Dodawanie nowych użytkowników lub użytkowników z kontami Microsoft do usługi Azure Active Directory
Dodaj użytkowników, aby wypełnić katalog. W tym artykule opisano sposób dodawania nowych użytkowników do organizacji oraz sposób dodawania użytkowników, którzy mają konta Microsoft. Aby uzyskać więcej informacji na temat dodawania użytkowników z innych katalogów w usłudze Azure Active Directory lub dodawania użytkowników z firm partnerskich, zobacz [Dodawanie użytkowników z innych katalogów lub firm partnerskich w usłudze Azure Active Directory](active-directory-create-users-external.md). Dodani użytkownicy domyślnie nie mają uprawnień administratora, ale możesz przypisać im role w dowolnym momencie.

## <a name="add-a-user"></a>Dodawanie użytkownika
1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz usługę **Active Directory**, a następnie wybierz nazwę katalogu organizacji.
3. Wybierz kartę **Użytkownicy**, a następnie na pasku poleceń wybierz polecenie **Dodaj użytkownika**.
4. Na stronie **Poinformuj nas o tym użytkowniku** w obszarze **Typ użytkownika** wybierz jedną z następujących opcji:
   
   * **Nowy użytkownik w organizacji** — dodaje nowe konto użytkownika w katalogu.
   * **Użytkownik z istniejącym kontem Microsoft** — dodaje istniejące konto klienta Microsoft do katalogu (na przykład konto programu Outlook)
5. W zależności od **typu użytkownika** wprowadź nazwę użytkownika (dla nowego użytkownika) lub adres e-mail (dla użytkownika z kontem Microsoft).
6. Na stronie **Profil** użytkownika podaj imię i nazwisko, nazwę przyjazną użytkownikowi i rolę użytkownika z listy **Role**. Aby uzyskać więcej informacji dotyczących ról użytkowników i administratorów, zobacz [Przypisywanie ról administratorów w usłudze Azure AD](active-directory-assign-admin-roles.md). Określ, czy **włączyć usługę Multi-Factor Authentication** dla użytkownika.
7. Na stronie **Uzyskaj hasło tymczasowe** wybierz opcję **Utwórz**.

> [!IMPORTANT]
> Jeśli Twoja organizacja korzysta z więcej niż jednej domeny, podczas dodawania konta użytkownika musisz wiedzieć o następujących kwestiach:
> 
> * Aby dodać konta użytkowników z tą samą główną nazwą użytkownika (UPN) między domenami, **najpierw** dodaj np. adres geoffgrisso@contoso.onmicrosoft.com,, **a następnie** geoffgrisso@contoso.com.
> * **Nie** dodawaj adresu geoffgrisso@contoso.com przed dodaniem adresu geoffgrisso@contoso.onmicrosoft.com. Ta kolejność jest ważna, a jej cofnięcie może być kłopotliwe.
> 
> 

## <a name="change-user-information"></a>Zmiana informacji o użytkowniku
Możesz zmienić dowolny atrybut użytkownika z wyjątkiem identyfikatora obiektu.

1. Otwórz katalog.
2. Wybierz kartę **Użytkownicy**, a następnie wybierz nazwę wyświetlaną użytkownika, którą chcesz zmienić.
3. Wprowadź zmiany, a następnie kliknij przycisk **Zapisz**.

Nie możesz zmienić informacji o użytkowniku przy użyciu tej procedury, jeśli użytkownik, którego informacje chcesz zmienić, jest zsynchronizowany z lokalną usługą Active Directory. Aby zmienić informacje o takim użytkowniku, użyj narzędzi zarządzania lokalnej usługi Active Directory.

## <a name="guest-user-management-and-limitations"></a>Zarządzanie użytkownikami typu Gość i ograniczenia
Z konta gościa korzystają użytkownicy z innych katalogów, którzy otrzymali zaproszenie do katalogu w celu uzyskania dostępu do dokumentów programu SharePoint, aplikacji lub innych zasobów platformy Azure. Podstawowy atrybut UserType na koncie gościa w katalogu jest ustawiony na opcję „Gość”. Atrybut UserType normalnych użytkowników (w szczególności członków katalogu) jest ustawiony na opcję „Członek”.

Goście mają ograniczony zestaw praw w katalogu. Te prawa ograniczają gościom możliwość wyszukiwania informacji o innych użytkownikach w katalogu. Goście mogą jednak nadal współdziałać z użytkownikami i grupami skojarzonymi z zasobami, nad którymi pracują. Goście mogą:

* Wyświetlać innych użytkowników i grupy skojarzone z subskrypcją platformy Azure, do której są przypisani
* Wyświetlać członków grup, do których należą
* Wyszukiwać innych użytkowników w katalogu, jeśli znają pełny adres e-mail użytkownika
* Wyświetlać ograniczony zestaw atrybutów wyszukiwanych użytkowników — ograniczony do nazwy wyświetlanej, adresu e-mail, głównej nazwy użytkownika (UPN) i miniatury zdjęcia
* Pobierać listę zweryfikowanych domen w katalogu
* Wyrazić zgodę na aplikacje przydzielające im takie same prawa dostępu w katalogu, jakie mają członkowie

## <a name="set-guest-user-access-policies"></a>Ustawianie zasad dostępu dla gościa
Karta katalogu **Konfiguruj** zawiera opcje kontroli dostępu dla gości. Te opcje mogą zostać zmienione wyłącznie za pośrednictwem klasycznego portalu Azure przez globalnego administratora katalogu. Obecnie nie ma metody robienia tego przez interfejs API lub program PowerShell.

Aby otworzyć kartę **Konfiguruj** w klasycznym portalu Azure, wybierz opcję **usługi Active Directory**, a następnie wybierz nazwę katalogu.

![Karta Konfigurowanie w usłudze Azure Active Directory][1]

Następnie możesz edytować opcje kontroli dostępu dla gości.

![Opcje kontroli dostępu dla gości][2]

## <a name="whats-next"></a>Co dalej
* [Dodawanie użytkowników z innych katalogów lub firm partnerskich w usłudze Azure Active Directory](active-directory-create-users-external.md)
* [Administrowanie usługą Azure AD](active-directory-administer.md)
* [Zarządzanie hasłami w usłudze Azure AD](active-directory-manage-passwords.md)
* [Zarządzanie grupami w usłudze Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png



<!--HONumber=Dec16_HO1-->


