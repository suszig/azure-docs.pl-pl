---
title: "Microsoft Azure Multi-Factor Authentication użytkownika stanów"
description: "Więcej informacji na temat stanu użytkowników w usłudze Azure MFA."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 0b9fde23-2d36-45b3-950d-f88624a68fbd
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: joflore
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 97fc645ca85d3405bb065fb0cf0145e828196fe9
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="how-to-require-two-step-verification-for-a-user-or-group"></a>Jak zażądać weryfikacji dwuetapowej dla użytkownika lub grupy

Istnieją dwa podejścia do wymagania weryfikacji dwuetapowej. Pierwsza opcja jest umożliwienie każdy użytkownik usługi Azure Multi-Factor Authentication (MFA). Gdy użytkownicy są włączone pojedynczo, zawsze wykonywać weryfikacji dwuetapowej (z pewnymi wyjątkami, takimi jak po zalogowaniu z zaufanych adresów IP lub jeśli jest włączona funkcja zapamiętanych urządzeniach). Drugą opcją jest do konfigurowania zasad dostępu warunkowego, która wymaga weryfikacji dwuetapowej w niektórych warunkach.

>[!TIP] 
>Wybierz jedną z tych metod, aby wymagać weryfikacji dwuetapowej, nie oba. Włączenie użytkownika dla usługi Azure MFA zastępuje wszystkie zasady dostępu warunkowego.

## <a name="which-option-is-right-for-you"></a>Która opcja jest dla Ciebie

**Włączanie usługi Azure MFA, zmieniając stanów użytkownika** jest tradycyjne podejście, wymagająca weryfikacji dwuetapowej. Działa zarówno usługi Azure MFA w chmurze i serwera usługi Azure MFA. Wszyscy użytkownicy, którzy włączeniu ma to samo środowisko korzystania, czyli do przeprowadzenia weryfikacji dwuetapowej, za każdym razem zalogują się w. Włączenie użytkownik zastępuje wszystkie zasady dostępu warunkowego, które mogą mieć wpływ na tego użytkownika. 

**Włączanie usługi Azure MFA z zasad dostępu warunkowego** jest bardziej elastyczne podejście wymagająca weryfikacji dwuetapowej. Jego prawidłowe tylko dla usługi Azure MFA w chmurze, a dostęp warunkowy jest [płatnej funkcji usługi Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features). Można utworzyć zasady dostępu warunkowego, które są stosowane do grupy, a także poszczególnych użytkowników. O wysokim ryzyku grup można przydzielić więcej ograniczeń, niż niskiego ryzyka grup lub weryfikacji dwuetapowej mogą być wymagane tylko dla aplikacji w chmurze o wysokim ryzyku i pomijana dla nich niskiego ryzyka. 

Obie te opcje monitować użytkowników o zarejestrować uwierzytelnianie wieloskładnikowe Azure po raz pierwszy zalogowaniu po Włącz wymagania. Obie te opcje są również współpracować przy konfigurowalne [ustawienia uwierzytelnianie wieloskładnikowe Azure](multi-factor-authentication-whats-next.md)

## <a name="enable-azure-mfa-by-changing-user-status"></a>Włączyć usługi Azure MFA przez zmianę stanu użytkownika

Konta użytkowników w usłudze Azure Multi-Factor Authentication mają następujące trzy różne stany:

| Stan | Opis | Aplikacje korzystające z przeglądarki, których to dotyczy |
|:---:|:---:|:---:|
| Disabled (Wyłączony) |Stan domyślny dla nowych użytkowników niezarejestrowanych Azure Multi-Factor Authentication (MFA). |Nie |
| Enabled (Włączony) |Użytkownik został zarejestrowany w usłudze Azure MFA, ale nie został zarejestrowany. Aby zarejestrować przy następnym zalogowaniu zostanie wyświetlony monit. |Nie.  One nadal działać do czasu ukończenia procesu rejestracji. |
| Enforced (Wymuszony) |Użytkownik został zarejestrowany i ukończył proces rejestracji dla usługi Azure MFA. |Tak.  Aplikacje wymagają hasła aplikacji. |

Stan użytkownika odzwierciedla czy administrator zarejestrował je w usłudze Azure MFA i czy zostały ukończone w procesie rejestracji.

Wszyscy użytkownicy uruchamiane *wyłączone*. Po zarejestrowaniu użytkowników usługi Azure MFA, ich zmian stanu *włączone*. Gdy użytkownicy włączone Zaloguj się i ukończenie procesu rejestracji, ich stan zmieni się na *wymuszane*.  

### <a name="view-the-status-for-a-user"></a>Wyświetlanie stanu użytkownika

Aby uzyskać dostęp do strony, gdzie można przeglądać i zarządzać stanu użytkowników, wykonaj następujące kroki:

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) jako administrator.
2. Przejdź do **usługi Azure Active Directory** > **użytkowników i grup** > **wszyscy użytkownicy**.
3. Wybierz **uwierzytelnianie wieloskładnikowe**.
   ![Wybierz uwierzytelnianie wieloskładnikowe](./media/multi-factor-authentication-get-started-user-states/selectmfa.png)
4. Zostanie otwarta strona nowe, wyświetlającego stanu użytkownika.
   ![Stan użytkownika usługi Multi-Factor authentication — zrzut ekranu](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Zmiana stanu użytkownika

1. Użyj powyższych kroków, aby przejść do strony użytkownicy usługi Multi-Factor authentication.
2. Znajdź użytkownika, którego chcesz włączyć dla usługi Azure MFA. Może być konieczna zmiana widoku w górnej części strony. 
   ![Znajdź użytkownika — zrzut ekranu](./media/multi-factor-authentication-get-started-cloud/enable1.png)
3. Zaznacz pole przy jego nazwie.
4. Po prawej stronie w obszarze Szybkie kroki wybierz **włączyć** lub **wyłączyć**.
   ![Włącz wybranego użytkownika — zrzut ekranu](./media/multi-factor-authentication-get-started-cloud/user1.png)

   >[!TIP]
   >*Włączone* użytkowników pozwala automatycznie przełączać się *wymuszane* po rejestracji dla usługi Azure MFA. Nie należy ręcznie zmienić stan użytkownika do wymuszane. 

5. Potwierdź wybór w otwartym oknie podręcznym. 

Po włączeniu użytkowników, należy powiadomić ich za pośrednictwem poczty e-mail. Poinformuj go, pojawi się pytanie do zarejestrowania przy następnym zalogowaniu. Ponadto jeśli Twoja organizacja korzysta z aplikacji korzystających z przeglądarki, które nie obsługują nowoczesnego uwierzytelniania, będzie muszą utworzyć hasła aplikacji. Możesz również uwzględnić łącze do naszej [przewodnik dla użytkowników końcowych usługi Azure MFA](./end-user/multi-factor-authentication-end-user.md) ułatwiającego rozpoczęcie pracy.

### <a name="use-powershell"></a>Korzystanie z programu PowerShell
Aby zmienić użytkownika stanu stanu za pomocą [programu Azure AD PowerShell](/powershell/azure/overview), zmień `$st.State`. Istnieją trzy stany:

* Enabled (Włączony)
* Enforced (Wymuszony)
* Disabled (Wyłączony)  

Nie przenoś bezpośrednio do użytkowników *wymuszone* stanu. Aplikacje nieoparte na przeglądarce przestaną działać, ponieważ użytkownik nie przeszedł procesu rejestracji w usłudze MFA i nie uzyskał [hasła aplikacji](multi-factor-authentication-whats-next.md#app-passwords). 

Przy użyciu programu PowerShell jest dobrym rozwiązaniem, gdy konieczne jest umożliwienie użytkownikom zbiorczego. Utwórz skrypt programu PowerShell w pętli listę użytkowników i umożliwia ich:

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

## <a name="enable-azure-mfa-with-a-conditional-access-policy"></a>Włącz zasady dostępu warunkowego usługi Azure MFA

Dostęp warunkowy jest płatnych funkcji usługi Azure Active Directory o wiele opcji konfiguracji możliwe. Te kroki przeprowadzenie jedną z metod tworzenia zasad. Aby uzyskać więcej informacji, przeczytaj o [dostępu warunkowego w usłudze Azure Active Directory](../active-directory/active-directory-conditional-access-azure-portal.md).

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) jako administrator.
2. Przejdź do **usługi Azure Active Directory** > **dostępu warunkowego**.
3. Wybierz **nowe zasady**.
4. W obszarze **przypisania**, wybierz pozycję **użytkowników i grup**. Użyj **Include** i **wykluczyć** karty, aby określić, którzy użytkownicy i grupy będą zarządzane przez zasady.
5. W obszarze **przypisania**, wybierz pozycję **aplikacji w chmurze**. Wybierz uwzględnić **wszystkich aplikacji w chmurze**.
6. W obszarze **dostęp do formantów**, wybierz pozycję **Grant**. Wybierz **wymusić uwierzytelnianie wieloskładnikowe**.
7. Włącz **Włącz zasady** do **na** , a następnie wybierz **zapisać**.

Inne opcje zasad dostępu warunkowego umożliwiają określenie dokładnie, gdy weryfikacja dwuetapowa powinno być wymagane. Na przykład można utworzyć zasad, które stany: gdy wykonawców próbują uzyskać dostęp do aplikacji zamówień z niezaufanymi sieciami, na urządzeniach, które nie są przyłączone do domeny, wymaga weryfikacji dwuetapowej. 

## <a name="next-steps"></a>Następne kroki

- Uzyskać porady na temat [najlepszych rozwiązań dotyczących dostępu warunkowego](../active-directory/active-directory-conditional-access-best-practices.md)

- Zarządzaj ustawieniami usługi Multi-Factor Authentication [użytkowników i urządzeń](multi-factor-authentication-manage-users-and-devices.md)