---
title: "Azure Active Directory Domain Services: Włączanie synchronizacji haseł | Microsoft Docs"
description: "Wprowadzenie do usługi Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 6ed35ca1b83f5f7221824d99817800c1e42e68c1
ms.lasthandoff: 04/12/2017


---
# <a name="enable-password-synchronization-with-azure-active-directory-domain-services"></a>Włączanie synchronizacji haseł w usługach Azure Active Directory Domain Services
W poprzednich zadaniach włączono usługi Azure Active Directory Domain Services (AD DS) dla dzierżawy usługi Azure Active Directory (Azure AD). Kolejnym krokiem jest włączenie skrótów poświadczeń wymaganych do uwierzytelniania NT LAN Manager (NTLM) i Kerberos w celu synchronizacji z usługami Azure Active Directory Domain Services. Po skonfigurowaniu synchronizacji poświadczeń użytkownik może zalogować się do domeny zarządzanej przy użyciu poświadczeń firmowych.

Procedury różnią się w zależności od tego, czy organizacja ma dzierżawę usługi Azure AD tylko w chmurze, czy też wykonuje synchronizację z katalogiem lokalnym za pomocą programu Azure AD Connect.

> [!div class="op_single_selector"]
> * [Dzierżawa usługi Azure AD tylko w chmurze](active-directory-ds-getting-started-password-sync.md)
> * [Zsynchronizowana dzierżawa usługi Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

## <a name="task-5-enable-password-synchronization-with-azure-active-directory-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Zadanie 5. Włączanie synchronizacji haseł w usługach Azure Active Directory Domain Services dla dzierżawy usługi Azure AD tylko w chmurze
Usługi Azure Active Directory Domain Services wymagają skrótów poświadczeń w formacie odpowiednim do uwierzytelniania NTLM i Kerberos, aby uwierzytelnić użytkowników w domenie zarządzanej. Jeśli nie zostaną włączone usługi Azure Active Directory Domain Services dla dzierżawy, usługa Azure AD nie będzie generować ani przechowywać skrótów poświadczeń w formacie wymaganym podczas uwierzytelniania NTLM i Kerberos. Ze względów bezpieczeństwa usługa Azure AD nie przechowuje również żadnych poświadczeń w postaci zwykłego tekstu. W związku z tym usługa Azure AD nie ma możliwości generowania skrótów poświadczeń protokołu NTLM ani Kerberos w oparciu o istniejące poświadczenia użytkownika.

> [!NOTE]
> Jeśli organizacja ma dzierżawę usługi Azure AD tylko w chmurze, użytkownicy, którzy muszą korzystać z usług Azure Active Directory Domain Services, muszą zmienić swoje hasła.
>
>

Ten proces zmiany haseł powoduje generowanie w usłudze Azure AD skrótów poświadczeń wymaganych przez usługi Azure Active Directory Domain Services na potrzeby uwierzytelniania Kerberos i NTLM. Możesz unieważnić hasła wszystkich użytkowników w dzierżawie, którzy muszą korzystać z usług Azure Active Directory Domain Services, lub poinformować ich o konieczności zmiany haseł.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Włączanie generowania skrótów poświadczeń NTLM i Kerberos w dzierżawie usługi Azure AD tylko w chmurze
Poniżej znajdują się instrukcje zmieniania haseł dla użytkowników:

1. Przejdź do strony [panelu dostępu do usługi Azure AD](http://myapps.microsoft.com) dla organizacji.
2. W oknie Panel dostępu wybierz kartę **Profil**.
3. Kliknij kafelek **Zmień hasło**.

    ![Kafelek „Zmień hasło” w panelu dostępu usługi Azure AD](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > Jeśli opcja **Zmień hasło** nie jest wyświetlana w oknie panelu dostępu, upewnij się, że Twoja organizacja ma skonfigurowane [zarządzanie hasłami w usłudze Azure AD](../active-directory/active-directory-passwords-getting-started.md).
   >
   >
4. Na stronie **Zmiana hasła** wpisz istniejące hasło (stare), a następnie wpisz nowe hasło i je zatwierdź. 

    ![Utwórz sieć wirtualną dla Usług domenowych Azure AD.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. Kliknij przycisk **prześlij**.

Nowego hasła w usługach Azure Active Directory Domain Services można używać już kilka minut po jego zmianie. Po kilku kolejnych minutach (zazwyczaj po około 20 minutach) przy użyciu nowego hasła będzie można się logować na komputerach dołączonych do domeny zarządzanej.

## <a name="next-steps"></a>Następne kroki
* [Jak zaktualizować własne hasło](../active-directory/active-directory-passwords-update-your-own-password.md#reset-your-password)
* [Wprowadzenie do zarządzania hasłami w usłudze Azure AD](../active-directory/active-directory-passwords-getting-started.md)
* [Włączanie synchronizacji haseł w usługach Azure Active Directory Domain Services dla zsynchronizowanej dzierżawy usługi Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Administrowanie domeną zarządzaną usług Azure Active Directory Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Dołączanie maszyny wirtualnej z systemem Windows do domeny zarządzanej usług Azure Active Directory Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Dołączanie maszyny wirtualnej z systemem Red Hat Enterprise Linux do domeny zarządzanej usług Azure Active Directory Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

