---
title: "Wprowadzenie do usługi Azure MFA w chmurze | Microsoft Docs"
description: "Ta strona dotyczy usługi Microsoft Azure Multi-Factor Authentication i zawiera informacje umożliwiające rozpoczęcie korzystania z usługi Azure MFA w chmurze."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6b2e6549-1a26-4666-9c4a-cbe5d64c4e66
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 79c6ef627d20be52b6067d1f6dfea2a53505c590
ms.lasthandoff: 03/17/2017


---
# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Wprowadzenie do usługi Azure Multi-Factor Authentication w chmurze
W tym artykule opisano, jak rozpocząć korzystanie z usługi Azure Multi-Factor Authentication w chmurze.

> [!NOTE]
> Poniższa dokumentacja zawiera informacje dotyczące umożliwiania użytkownikom korzystania z **klasycznego portalu Azure**. Jeśli szukasz informacji na temat konfigurowania usługi Azure Multi-Factor Authentication dla użytkowników usługi O365, zobacz temat [Konfigurowanie usługi Multi-Factor Authentication dla usługi Office 365](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US).

![Usługa MFA w chmurze](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisite"></a>Wymagania wstępne
[Utworzenie konta na potrzeby subskrypcji platformy Azure](https://azure.microsoft.com/pricing/free-trial/) — jeśli nie masz jeszcze subskrypcji platformy Azure, musisz utworzyć konto. Jeśli dopiero zaczynasz pracę i używasz usługi Azure MFA, możesz skorzystać z subskrypcji wersji próbnej.

## <a name="enable-azure-multi-factor-authentication"></a>Włączanie usługi Azure Multi-Factor Authentication
Dopóki użytkownicy będą mieli licencje obejmujące usługę Azure Multi-Factor Authentication, do jej włączenia nie będzie wymagane wykonanie żadnych czynności. Możliwe jest rozpoczęcie wymagania weryfikacji dwuetapowej dla indywidualnych użytkowników. Licencje, które umożliwiają włączenie usługi Azure MFA, to:
- Azure Multi-Factor Authentication
- Usługa Azure Active Directory Premium
- Enterprise Mobility + Security

Jeśli nie masz żadnej z tych trzech licencji ani nie masz wystarczającej liczby licencji, aby obejmowały wszystkich użytkowników, to też nie stanowi problemu. Konieczne jest tylko wykonanie dodatkowego kroku w celu [utworzenia dostawcy usługi Multi-Factor Authentication](multi-factor-authentication-get-started-auth-provider.md) w katalogu.

## <a name="turn-on-two-step-verification-for-users"></a>Włączanie weryfikacji dwuetapowej dla użytkowników
Aby zacząć wymagać od użytkownika weryfikacji dwuetapowej, należy zmienić jego stan z wyłączonego na włączony.  Więcej informacji na temat stanów użytkowników można znaleźć w artykule [User States in Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md) (Stany użytkowników w usłudze Azure Multi-Factor Authentication)

Aby włączyć usługę MFA dla użytkowników, postępuj zgodnie z poniższą procedurą.

### <a name="to-turn-on-multi-factor-authentication"></a>Aby włączyć uwierzytelnianie wieloskładnikowe
1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) jako administrator.
2. W obszarze po lewej stronie kliknij pozycję **Active Directory**.
3. W obszarze Katalog wybierz katalog użytkownika, któremu chcesz umożliwić korzystanie z usługi.
   ![Kliknięcie katalogu](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4. Kliknij pozycję **Użytkownicy** w górnej części strony.
5. W dolnej części strony kliknij pozycję **Zarządzaj usługą Multi-Factor Auth**. Zostanie otwarta nowa karta przeglądarki.
   ![Kliknięcie katalogu](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6. Znajdź użytkownika, dla którego chcesz włączyć weryfikację dwuetapową. Może być konieczna zmiana widoku w górnej części strony. Upewnij się, że stan ma wartość **Wyłączony.**
   ![Włączanie użytkownika](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7. **Zaznacz** pole obok nazwy użytkownika.
8. Kliknij pozycję **Włącz** wyświetlaną z prawej strony.
   ![Włączanie użytkownika](./media/multi-factor-authentication-get-started-cloud/user1.png)
9. Kliknij pozycję **Włącz usługę Multi-Factor Authentication**.
   ![Włączanie użytkownika](./media/multi-factor-authentication-get-started-cloud/enable2.png)
10. Stan użytkownika zmieni się z opcji **Wyłączony** na **Włączony**.
    ![Włączanie użytkowników](./media/multi-factor-authentication-get-started-cloud/user.png)

Po włączeniu użytkowników należy ich o tym powiadomić za pośrednictwem poczty e-mail. Przy następnej próbie logowania zostaną poproszeni o zarejestrowanie swojego konta w celu korzystania z weryfikacji dwuetapowej. Po rozpoczęciu używania weryfikacji dwuetapowej będą również musieli skonfigurować hasła aplikacji, aby uniknąć zablokowania dostępu do aplikacji niekorzystających z przeglądarki.

## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>Automatyczne włączanie weryfikacji dwuetapowej za pomocą programu PowerShell
Aby zmienić [stan](multi-factor-authentication-whats-next.md) przy użyciu [programu PowerShell usługi Azure AD](/powershell/azureps-cmdlets-docs), skorzystaj z następującej metody.  Możesz zmienić właściwość `$st.State`, przypisując jej jeden z następujących stanów:

* Enabled (Włączony)
* Enforced (Wymuszony)
* Disabled (Wyłączony)  

> [!IMPORTANT]
> Nie zalecamy przenoszenia użytkowników bezpośrednio ze stanu Disabled do stanu Enforced. Aplikacje nieoparte na przeglądarce przestaną działać, ponieważ użytkownik nie przeszedł procesu rejestracji w usłudze MFA i nie uzyskał [hasła aplikacji](multi-factor-authentication-whats-next.md#app-passwords). W przypadku aplikacji niekorzystających z przeglądarki, które wymagają hasła, zalecamy przejście ze stanu Disabled do stanu Enabled. Pozwoli to użytkownikom na zarejestrowanie się i uzyskanie haseł aplikacji. Następnie będzie można zmienić ich stan na Enforced.

Używanie programu PowerShell to dobra metoda na zbiorcze włączanie użytkowników. Obecnie portal Azure nie umożliwia zbiorczego włączania — trzeba wybierać pojedynczych użytkowników. W przypadku dużej liczby użytkowników może to być pracochłonne. Utworzenie skryptu programu PowerShell przy użyciu następujących instrukcji umożliwia jednoczesne włączenie wszystkich użytkowników z listy.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Oto przykład:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Aby uzyskać więcej informacji, zobacz temat [User States in Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md) (Stany użytkowników w usłudze Azure Multi-Factor Authentication)

## <a name="next-steps"></a>Następne kroki
Po skonfigurowaniu usługi Azure Multi-Factor Authentication w chmurze można przystąpić do konfigurowania wdrożenia. Aby uzyskać bardziej szczegółowe informacje, zobacz temat [Configuring Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md) (Konfigurowanie usługi Azure Multi-Factor Authentication).


