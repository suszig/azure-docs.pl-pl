---
title: "Dołącz nowe urządzenie z systemem Windows 10 z usługą Azure AD przy pierwszym uruchomieniu | Dokumentacja firmy Microsoft"
description: "Temat, który objaśnia, jak użytkownicy mogli skonfigurować Azure AD Join podczas pierwszego uruchomienia komputera."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 0e71df2333dee9c4eb9935d3397d343be246be65
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2018
---
# <a name="join-a-new-windows-10-device-with-azure-ad-during-a-first-run"></a>Dołącz nowe urządzenie z systemem Windows 10 z usługą Azure AD przy pierwszym uruchomieniu

Z zarządzania urządzeniami w usłudze Azure Active Directory (Azure AD) można zapewnić, że użytkownicy uzyskują dostęp do zasobów z urządzeń, które spełniają standardy zabezpieczeń i zgodności. Aby uzyskać więcej informacji, zobacz [wprowadzenie do zarządzania urządzeniami w usłudze Azure Active Directory](device-management-introduction.md).

Z systemem Windows 10 można dołączyć nowego urządzenia do usługi Azure AD podczas pierwszego uruchomienia komputera (FRX).  
Dzięki temu można rozpowszechniać porządnie zapakowane urządzeń do pracowników lub studentów.

Jeśli masz system Windows 10 Professional lub Windows 10 Enterprise zainstalowana na urządzeniu, środowisko domyślnie procesu instalacji urządzenia należące do firmy.

W oknach *out-of-box experience*, przyłączania do domeny usługi Active Directory (AD) lokalnymi nie jest obsługiwane. Jeśli planujesz przyłączyć komputer do domeny usługi AD podczas instalacji, należy wybrać link **instalacji systemu Windows przy użyciu konta lokalnego**. Można następnie przyłączyć do domeny z ustawień, na komputerze.
 


## <a name="before-you-begin"></a>Przed rozpoczęciem

Aby przyłączyć urządzenie z systemem Windows 10, usługi rejestracji urządzeń muszą być skonfigurowane do włączenia rejestracji urządzeń. Oprócz mając uprawnienia do przyłączania urządzeń w Twojej dzierżawie usługi Azure AD, musi mieć mniej urządzeń zarejestrowanych niż skonfigurowana wartość maksymalna. Aby uzyskać więcej informacji, zobacz [skonfigurować ustawienia urządzenia](device-management-azure-portal.md#configure-device-settings).

## <a name="joining-a-device"></a>Dołączenie urządzenia

**Aby dołączyć urządzenie z systemem Windows 10 do usługi Azure AD podczas FRX:**


1. Po włączeniu nowe urządzenie i rozpocząć proces instalacji, powinna zostać wyświetlona **pierwsze gotowe** wiadomości. Postępuj zgodnie z monitami, aby skonfigurować urządzenie.

2. Rozpocznij od Dostosowywanie region i język. Następnie zaakceptuj postanowienia licencyjne dotyczące oprogramowania firmy Microsoft.
 
    ![Dostosowywanie w Twoim regionie](./media/device-management-azuread-joined-devices-frx/01.png)

3. Wybierz sieć, który ma być używany do łączenia się z Internetem.

4. Kliknij przycisk **to urządzenie należy do mojej organizacji**. 

    ![Kto jest właścicielem tego ekranu komputera](./media/device-management-azuread-joined-devices-frx/02.png)

5. Wprowadź poświadczenia, które zostały przekazane użytkownikowi przez organizację, a następnie kliknij przycisk **Zaloguj**.

    ![Ekran logowania](./media/device-management-azuread-joined-devices-frx/03.png)

6. Urządzenia można znajduje dopasowywania dzierżawy w usłudze Azure AD. Jeśli w domenie federacyjnych, nastąpi przekierowanie do lokalnego serwera Secure Token Service (STS), na przykład Active Directory Federation Services (AD FS).

7. Jeśli użytkownik jest użytkownikiem domeny z systemem innym niż federacyjnych, wprowadź swoje poświadczenia bezpośrednio na stronie hostowanych przez usługi AD platformy Azure. 

8. Zostanie wyświetlony monit o żądanie uwierzytelniania wieloskładnikowego. 
 
9. Usługi Azure AD sprawdza, czy wymagana jest rejestracja w zarządzania urządzeniami przenośnymi.

10. System Windows zarejestrowanie urządzenia w katalogu organizacji w usłudze Azure AD i rejestruje w przystawce Zarządzanie urządzeniami przenośnymi, jeśli ma to zastosowanie.

11. Jeśli masz:
    - Zarządzane użytkownika systemu Windows umożliwia przejście do pulpitu za pośrednictwem proces automatycznego logowania.

    - Użytkownik federacyjny, są kierowane do ekranu logowania systemu Windows, aby wprowadzić swoje poświadczenia.

## <a name="verification"></a>Weryfikacja

Aby sprawdzić, czy urządzenie jest dołączane do usługi Azure AD, przejrzyj **dostępu służbowego** okna dialogowego na urządzeniu z systemem Windows. Okno dialogowe powinny wskazywać, że nawiązano połączenie katalogu usługi Azure AD.

![Dostęp do służbowych.](./media/device-management-azuread-joined-devices-frx/13.png)


## <a name="next-steps"></a>Kolejne kroki

- Aby uzyskać więcej informacji, zobacz [wprowadzenie do zarządzania urządzeniami w usłudze Azure Active Directory](device-management-introduction.md).

- Aby uzyskać więcej informacji na temat zarządzania urządzeniami w portalu usługi Azure AD, zobacz [zarządzanie urządzeniami przy użyciu portalu Azure](device-management-azure-portal.md).
