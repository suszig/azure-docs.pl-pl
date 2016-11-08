---
title: Wprowadzenie do usługi Microsoft Azure Multi-Factor Authentication w chmurze
description: Ta strona dotyczy usługi Microsoft Azure Multi-Factor Authentication i zawiera informacje umożliwiające rozpoczęcie korzystania z usługi Azure MFA w chmurze.
services: multi-factor-authentication
documentationcenter: ''
author: kgremban
manager: femila
editor: curtand

ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2016
ms.author: kgremban

---
# Wprowadzenie do usługi Azure Multi-Factor Authentication w chmurze
Po przeczytaniu tego artykułu dowiesz się, jak rozpocząć korzystanie z usługi Azure Multi-Factor Authentication w chmurze.

> [!NOTE]
> Poniższa dokumentacja zawiera informacje dotyczące umożliwiania użytkownikom korzystania z **klasycznego portalu Azure**. Jeśli szukasz informacji na temat konfigurowania usługi Azure Multi-Factor Authentication dla użytkowników usługi O365, zobacz temat [Konfigurowanie uwierzytelniania wieloskładnikowego dla użytkowników usługi Office 365](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)
> 
> 

![Usługa MFA w chmurze](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## Wymagania wstępne
Aby umożliwić użytkownikom korzystanie z usługi Azure Multi-Factor Authentication, konieczne jest spełnienie następujących wymagań wstępnych.

* [Utworzenie konta na potrzeby subskrypcji platformy Azure](https://azure.microsoft.com/pricing/free-trial/) — jeśli nie masz jeszcze subskrypcji platformy Azure, musisz utworzyć konto. Jeśli dopiero zaczynasz pracę i używasz usługi Azure MFA, możesz skorzystać z subskrypcji wersji próbnej.
* [Utworzenie dostawcy usługi Multi-Factor Authentication](multi-factor-authentication-get-started-auth-provider.md) i przypisanie go do katalogu lub [przypisanie licencji do użytkowników](multi-factor-authentication-get-started-assign-licenses.md).

> [!NOTE]
> Licencje są dostępne dla użytkowników, którzy mają usługę Azure MFA, usługę Azure AD w wersji Premium lub pakiet Enterprise Mobility Suite (EMS).  Usługa MFA jest zawarta w wersji Premium usługi Azure AD i pakiecie EMS. Jeśli masz wystarczającą liczbę licencji, nie musisz tworzyć dostawcy uwierzytelniania.
> 
> 

## Włączanie uwierzytelniania wieloskładnikowego dla użytkowników
Aby włączyć uwierzytelnianie wieloskładnikowe dla użytkownika, wystarczy zmienić jego stan z wyłączonego na włączony.  Więcej informacji na temat stanów użytkowników można znaleźć w artykule [User States in Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md) (Stany użytkowników w usłudze Azure Multi-Factor Authentication).

Aby włączyć usługę MFA dla użytkowników, postępuj zgodnie z poniższą procedurą.

### Aby włączyć uwierzytelnianie wieloskładnikowe
- - -
1. Zaloguj się do **klasycznego portalu Azure** jako administrator.
2. W obszarze po lewej stronie kliknij pozycję **Active Directory**.
3. W obszarze **Katalog** kliknij katalog użytkownika, któremu chcesz umożliwić korzystanie z usługi.
   ![Kliknięcie katalogu](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4. Kliknij pozycję **Użytkownicy** w górnej części strony.
5. W dolnej części strony kliknij pozycję **Zarządzaj usługą Multi-Factor Auth**.
   ![Kliknięcie katalogu](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6. Spowoduje to otwarcie nowej karty w przeglądarce.  Znajdź użytkownika, dla którego chcesz włączyć uwierzytelnianie wieloskładnikowe. Może być konieczna zmiana widoku w górnej części strony. Upewnij się, że stan ma wartość **Wyłączony.**
   ![Włączanie użytkownika](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7. **Zaznacz** pole obok nazwy użytkownika.
8. Kliknij pozycję **Włącz** wyświetlaną z prawej strony.
   ![Włączanie użytkownika](./media/multi-factor-authentication-get-started-cloud/user1.png)
9. Kliknij pozycję **Włącz usługę Multi-Factor Authentication**.
   ![Włączanie użytkownika](./media/multi-factor-authentication-get-started-cloud/enable2.png)
10. Stan użytkownika zmieni się z **Wyłączony** na **łączony**.
    ![Włączanie użytkowników](./media/multi-factor-authentication-get-started-cloud/user.png)
11. Po włączeniu użytkowników zaleca się powiadomienie ich o tym za pośrednictwem poczty e-mail.  Należy ich również poinformować, jak powinni używać aplikacji niekorzystających z przeglądarki, aby uniknąć zablokowania.

## Automatyzowanie włączania uwierzytelniania wieloskładnikowego przy użyciu programu PowerShell
Aby zmienić [stan](multi-factor-authentication-whats-next.md) przy użyciu [programu PowerShell usługi Azure AD](../powershell-install-configure.md), skorzystaj z następującej metody.  Możesz zmienić właściwość `$st.State`, przypisując jej jeden z następujących stanów:

* Enabled (Włączony)
* Enforced (Wymuszony)
* Disabled (Wyłączony)  

> [!IMPORTANT]
> Pamiętaj o tym, że jeśli bezpośrednio zmienisz stan z wyłączonego na wymuszony, stare klienty uwierzytelniania przestaną działać, ponieważ dla danego użytkownika nie została przeprowadzona rejestracja w usłudze MFA i nie uzyskał on [hasła aplikacji](multi-factor-authentication-whats-next.md#app-passwords).  Jeśli używasz starych klientów uwierzytelniania i korzystasz z haseł aplikacji, zaleca się zmianę stanu z wyłączonego na włączony.  Pozwoli to użytkownikom na zarejestrowanie się i uzyskanie haseł aplikacji.   
> 
> 

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Używanie programu PowerShell to dobra metoda na zbiorcze włączanie użytkowników.  Obecnie portal Azure nie umożliwia zbiorczego włączania — trzeba wybierać pojedynczych użytkowników.  W przypadku dużej liczby użytkowników może to być pracochłonne.  Utworzenie skryptu programu PowerShell przy użyciu powyższych instrukcji umożliwia jednoczesne włączenie wszystkich użytkowników z listy.  Oto przykład:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Więcej informacji na temat stanów użytkowników można znaleźć w artykule [User States in Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md) (Stany użytkowników w usłudze Azure Multi-Factor Authentication).

## Następne kroki
Po skonfigurowaniu uwierzytelniania wieloskładnikowego w chmurze można przystąpić do konfigurowania wdrożenia.  Zobacz artykuł Configuring Azure Multi-Factor Authentication (Konfigurowanie usługi Azure Multi-Factor Authentication).

<!--HONumber=Sep16_HO3-->


