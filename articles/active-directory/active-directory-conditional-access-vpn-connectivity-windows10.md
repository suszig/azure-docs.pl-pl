---
title: "Azure Active Directory dostępu warunkowego dla połączenia sieci VPN (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak działa usługa Azure Active Directory dostępu warunkowego dla połączenia sieci VPN. "
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: e9dadb3291ee760e7b05caedfa6b4128be77aa7d
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-conditional-access-for-vpn-connectivity-preview"></a>Azure Active Directory dostępu warunkowego dla połączenia sieci VPN (wersja zapoznawcza)

Z [dostępu warunkowego w usłudze Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), można dostosować sposób autoryzowanym użytkownikom dostępu do zasobów. Przy użyciu dostępu warunkowego usługi Azure AD dla połączenia wirtualnej sieci prywatnej (VPN) może pomóc chronić połączenia sieci VPN.


Aby skonfigurować dostęp warunkowy dla połączenia sieci VPN, należy wykonać następujące czynności: 

1.  Skonfiguruj serwer sieci VPN.
2.  Konfigurowanie klienta sieci VPN.
3.  Skonfiguruj zasady dostępu warunkowego.


## <a name="before-you-begin"></a>Przed rozpoczęciem

W tym temacie założono, że znasz następujące tematy:

- [Dostęp warunkowy w usłudze Azure Active Directory](active-directory-conditional-access-azure-portal.md)
- [Sieć VPN i dostępu warunkowego](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Aby uzyskać wgląd w sposób Microsoft implementuje tej funkcji, zobacz [udoskonalanie dostępu zdalnego w systemie Windows 10 z automatycznego profil sieci VPN](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile).   


## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować dostęp warunkowy usługi Azure Active Directory dla połączenia sieci VPN, należy skonfigurować serwer sieci VPN. 



## <a name="step-1-configure-your-vpn-server"></a>Krok 1: Skonfiguruj serwer sieci VPN 

Ten krok obejmuje skonfigurowanie certyfikaty główne na potrzeby uwierzytelniania sieci VPN z usługą Azure AD. Aby skonfigurować dostęp warunkowy dla połączenia sieci VPN, należy:

1. Utwórz certyfikat sieci VPN w portalu Azure.
2. Pobierz certyfikat sieci VPN.
2. Wdróż certyfikat serwera sieci VPN.

Usługi Azure AD używa certyfikatu sieci VPN do podpisywania certyfikatów wystawionych dla klientów systemu Windows 10 podczas uwierzytelniania usługi Azure AD dla połączenia sieci VPN. Token żądający klienta systemu Windows 10 jest certyfikat, który następnie stanowi do aplikacji, w tym przypadku jest serwer sieci VPN.

![Pobierz certyfikat dla dostępu warunkowego](./media/active-directory-conditional-access-vpn-connectivity-windows10/06.png)

W portalu Azure można utworzyć dwa certyfikaty zarządzania przejście, kiedy zbliża się data wygaśnięcia jest jeden certyfikat. Podczas tworzenia certyfikatu, można wybrać, czy jest podstawowy certyfikat, który jest używany podczas uwierzytelniania do podpisania certyfikatu dla połączenia.

Aby utworzyć certyfikat sieci VPN:

1. Zaloguj się do Twojego [portalu Azure](https://portal.azure.com) jako administrator globalny.

2. W menu po lewej stronie kliknij **usługi Azure Active Directory**. 

    ![Wybierz pozycję Azure Active Directory](./media/active-directory-conditional-access-vpn-connectivity-windows10/01.png)

3. Na **usługi Azure Active Directory** strony w **Zarządzaj** kliknij **dostępu warunkowego**.

    ![Wybierz dostępu warunkowego](./media/active-directory-conditional-access-azure-portal-get-started/02.png)

4. Na **dostępu warunkowego** strony w **Zarządzaj** kliknij **łączność w sieci VPN (wersja zapoznawcza)**.

    ![Wybierz połączenie z siecią VPN](./media/active-directory-conditional-access-vpn-connectivity-windows10/03.png)

5. Na **połączenie z siecią VPN** kliknij przycisk **nowego świadectwa**.

    ![Wybierz nowy certyfikat](./media/active-directory-conditional-access-vpn-connectivity-windows10/04.png)

6. Na **nowy** wykonaj następujące czynności:

    ![Wybierz czas trwania i podstawowej](./media/active-directory-conditional-access-vpn-connectivity-windows10/05.png)

    a. Aby uzyskać **wybierz czas trwania**, wybierz pozycję **1 rok**.

    b. Aby uzyskać **głównej**, wybierz pozycję **tak**.

    c. Kliknij przycisk **Utwórz**.

7. Na stronie połączenie sieci VPN kliknij **pobierania certyfikatu**.


Teraz możesz wdrożyć nowo utworzonego certyfikatu do serwera sieci VPN. Na serwer sieci VPN, należy dodać pobranego certyfikatu jako *zaufanego głównego urzędu certyfikacji dla uwierzytelniania sieci VPN*.

Do wdrożenia oparte na RRAS z systemem Windows, na serwerze NPS, Dodaj certyfikat główny do *Enterprise NTauth* przechowywania, uruchamiając następujące polecenia:

1. `certutil -dspublish <CACERT> RootCA`
2. `certutil -dspublish <CACERT> NtAuthCA`



## <a name="step-2-configure-your-vpn-client"></a>Krok 2: Konfigurowanie klienta sieci VPN 

W tym kroku konfigurowania profilu połączenia VPN klienta w sposób opisany w [sieci VPN i dostępu warunkowego](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).


## <a name="step-3-configure-your-conditional-access-policy"></a>Krok 3: Konfigurowanie zasad dostępu warunkowego

Ta sekcja zawiera instrukcje dotyczące konfigurowania zasady dostępu warunkowego dla połączenia sieci VPN.


1. Na **dostępu warunkowego** na pasku narzędzi u góry, kliknij przycisk **Dodaj**.

    ![Wybierz opcję Dodaj na stronie dostępu warunkowego](./media/active-directory-conditional-access-vpn-connectivity-windows10/07.png)

2. Na **nowy** strony w **nazwa** wpisz nazwę zasady. Na przykład wpisz **zasad sieci VPN**.

    ![Dodaj nazwę zasad, na stronie dostępu warunkowego](./media/active-directory-conditional-access-vpn-connectivity-windows10/08.png)

5. W **przypisania** kliknij **użytkowników i grup**.

    ![Wybieranie użytkowników i grup](./media/active-directory-conditional-access-vpn-connectivity-windows10/09.png)

6. Na **użytkowników i grup** wykonaj następujące czynności:

    ![Wybierz użytkownika](./media/active-directory-conditional-access-vpn-connectivity-windows10/10.png)

    a. Kliknij przycisk **Wybierz użytkowników i grupy**.

    b. Kliknij pozycję **Wybierz**.

    c. Na **wybierz** , wybierz użytkownika testowego, a następnie kliknij przycisk **wybierz**.

    d. Na **użytkowników i grup** kliknij przycisk **gotowe**.

7. Na **nowy** wykonaj następujące czynności:

    ![Wybierz aplikacje w chmurze](./media/active-directory-conditional-access-vpn-connectivity-windows10/11.png)

    a. W **przypisania** kliknij **aplikacji w chmurze**.

    b. Na **aplikacji w chmurze** kliknij przycisk **Wybierz aplikacje**, a następnie kliknij przycisk **wybierz**.

    c. Na **wybierz** strony w **aplikacji** wpisz **vpn**.

    d. Wybierz **serwer sieci VPN**.

    e. Kliknij pozycję **Wybierz**.


13. Na **nowy** strony, aby otworzyć **Grant** strony w **formanty** kliknij **Grant**.

    ![Wybierz grant](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. Na **Grant** wykonaj następujące czynności:

    ![Zaznacz opcję Wymagaj uwierzytelniania wieloskładnikowego](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Wybierz **wymusić uwierzytelnianie wieloskładnikowe**.

    b. Kliknij pozycję **Wybierz**.

15. Na **nowy** w obszarze **Włącz zasady**, kliknij przycisk **na**.

    ![Włącz zasady](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. Na **nowy** kliknij przycisk **Utwórz**.



## <a name="next-steps"></a>Następne kroki

Aby uzyskać wgląd w sposób Microsoft implementuje tej funkcji, zobacz [udoskonalanie dostępu zdalnego w systemie Windows 10 z automatycznego profil sieci VPN](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile).    

