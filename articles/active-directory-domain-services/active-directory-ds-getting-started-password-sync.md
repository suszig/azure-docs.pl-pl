---
title: "Azure Active Directory Domain Services: Włączanie synchronizacji haseł | Microsoft Docs"
description: "Wprowadzenie do usługi Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/18/2017
ms.author: maheshu
ms.openlocfilehash: ea65659b1d35246bc3e7235b3faed8d2a5368010
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="enable-password-synchronization-to-azure-active-directory-domain-services"></a>Włączanie synchronizacji haseł w usługach Azure Active Directory Domain Services
W poprzednich zadaniach włączono usługi Azure Active Directory Domain Services dla dzierżawy usługi Azure Active Directory (Azure AD). Kolejnym krokiem jest włączenie synchronizacji skrótów poświadczeń wymaganych do uwierzytelniania NT LAN Manager (NTLM) i Kerberos w usługach Azure AD Domain Services. Po skonfigurowaniu synchronizacji poświadczeń użytkownik może zalogować się do domeny zarządzanej przy użyciu poświadczeń firmowych.

Wykonywane czynności są różne dla kont użytkowników tylko w chmurze i kont użytkowników synchronizowanych z katalogu lokalnego przy użyciu programu Azure AD Connect. 

<br>
| **Typ konta użytkownika** | **Czynności do wykonania** |
| --- |---|
| **Konta użytkowników chmury utworzone w usłudze Azure AD** |**&#x2713;** [Postępuj zgodnie z instrukcjami w tym artykule](active-directory-ds-getting-started-password-sync.md#task-5-enable-password-synchronization-to-your-managed-domain-for-cloud-only-user-accounts) |
| **Konta użytkownika synchronizowane z poziomu katalogu lokalnego** |**&#x2713;** [Zsynchronizuj hasła dla kont użytkowników synchronizowanych z lokalnej usługi AD do domeny zarządzanej](active-directory-ds-getting-started-password-sync-synced-tenant.md) | 

<br>

> [!TIP]
> **Może być konieczne wykonanie obu zestawów czynności.**
> Jeśli dzierżawa usługi Azure AD ma kombinację użytkowników tylko w chmurze i użytkowników z lokalnej usługi AD, należy ukończyć oba zestawy czynności.
>

## <a name="task-5-enable-password-synchronization-to-your-managed-domain-for-cloud-only-user-accounts"></a>Zadanie 5. Włączanie synchronizacji haseł do domeny zarządzanej dla kont użytkowników tylko w chmurze
Usługi Azure Active Directory Domain Services wymagają skrótów poświadczeń w formacie odpowiednim do uwierzytelniania NTLM i Kerberos, aby uwierzytelnić użytkowników w domenie zarządzanej. Usługa Azure AD nie będzie generować ani przechowywać skrótów poświadczeń w formacie wymaganym podczas uwierzytelniania NTLM i Kerberos, jeśli nie zostaną włączone usługi Azure Active Directory Domain Services dla dzierżawy. Ze względów bezpieczeństwa usługa Azure AD nie przechowuje również żadnych poświadczeń haseł w postaci zwykłego tekstu. W związku z tym usługa Azure AD nie ma możliwości automatycznego generowania skrótów poświadczeń protokołu NTLM ani Kerberos w oparciu o istniejące poświadczenia użytkownika.

> [!NOTE]
> **Jeśli organizacja ma konta użytkowników tylko w chmurze, wszyscy użytkownicy, którzy muszą korzystać z usług Azure Active Directory Domain Services, muszą zmienić swoje hasła.** Konto użytkownika tylko w chmurze to konto, które zostało utworzone w katalogu usługi Azure AD przy użyciu witryny Azure Portal lub poleceń cmdlet programu Azure AD PowerShell. Takie konta użytkownika nie są synchronizowane z poziomu katalogu lokalnego.
>
>

Ten proces zmiany haseł powoduje generowanie w usłudze Azure AD skrótów poświadczeń wymaganych przez usługi Azure Active Directory Domain Services na potrzeby uwierzytelniania Kerberos i NTLM. Możesz unieważnić hasła wszystkich użytkowników w dzierżawie, którzy muszą korzystać z usług Azure Active Directory Domain Services, lub poinformować ich o konieczności zmiany haseł.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-user-account"></a>Włączanie generowania skrótów poświadczeń NTLM i Kerberos dla kont użytkowników tylko w chmurze
Poniżej znajdują się instrukcje zmieniania haseł dla użytkowników:

1. Przejdź do strony [panelu dostępu do usługi Azure AD](http://myapps.microsoft.com) dla organizacji.

    ![Uruchamianie panelu dostępu usługi Azure AD](./media/active-directory-domain-services-getting-started/access-panel.png)

2. W prawym górnym rogu kliknij swoją nazwę i wybierz pozycję **Profil** z menu.

    ![Wybieranie profilu](./media/active-directory-domain-services-getting-started/select-profile.png)

3. Na stronie **Profil** kliknij pozycję **Zmień hasło**.

    ![Kliknięcie pozycji „Zmień hasło”](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!TIP]
   > Jeśli opcja **Zmień hasło** nie jest wyświetlana w oknie panelu dostępu, upewnij się, że Twoja organizacja ma skonfigurowane [zarządzanie hasłami w usłudze Azure AD](../active-directory/active-directory-passwords-getting-started.md).
   >
   >
4. Na stronie **Zmiana hasła** wpisz istniejące hasło (stare), a następnie wpisz nowe hasło i je zatwierdź.

    ![Utwórz sieć wirtualną dla Usług domenowych Azure AD.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. Kliknij przycisk **prześlij**.

Nowego hasła w usługach Azure Active Directory Domain Services można używać już kilka minut po jego zmianie. Po około 20 minutach użytkownicy będą mogli logować się na komputerach dołączonych do domeny zarządzanej przy użyciu nowo zmienionego hasła.

## <a name="related-content"></a>Powiązana zawartość
* [Jak zaktualizować własne hasło](../active-directory/active-directory-passwords-update-your-own-password.md)
* [Wprowadzenie do zarządzania hasłami w usłudze Azure AD](../active-directory/active-directory-passwords-getting-started.md)
* [Włączanie synchronizacji haseł w usługach Azure Active Directory Domain Services dla zsynchronizowanej dzierżawy usługi Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Administrowanie domeną zarządzaną usług Azure Active Directory Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Dołączanie maszyny wirtualnej z systemem Windows do domeny zarządzanej usług Azure Active Directory Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Dołączanie maszyny wirtualnej z systemem Red Hat Enterprise Linux do domeny zarządzanej usług Azure Active Directory Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
