<properties
    pageTitle="Usługi domenowe Azure AD: włączanie synchronizacji haseł | Microsoft Azure"
    description="Wprowadzenie do usługi Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/06/2016"
    ms.author="maheshu"/>

# Usługi domenowe Azure AD *(wersja zapoznawcza)* — włączanie synchronizacji haseł w Usługach domenowych Azure AD

## Zadanie 5. Włączanie synchronizacji haseł w Usługach domenowych AAD dla katalogu usługi Azure AD tylko w chmurze
Następnym zadaniem po włączeniu Usług domenowych Azure AD dla dzierżawy usługi Azure AD jest umożliwienie synchronizacji przy użyciu poświadczeń w Usługach domenowych Azure AD. Dzięki temu użytkownicy mogą logować się do domeny zarządzanej za pomocą poświadczeń firmowych.

Kroki do wykonania różnią się w zależności od tego, czy organizacja ma katalog usługi Azure AD tylko w chmurze, czy też wykonuje synchronizację z katalogiem lokalnym za pomocą programu Azure AD Connect.

<br>

> [AZURE.SELECTOR]
- [Katalog usługi Azure AD tylko w chmurze](active-directory-ds-getting-started-password-sync.md)
- [Zsynchronizowany katalog usługi Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>

### Włączanie generowania skrótów poświadczeń NTLM i Kerberos w katalogu usługi Azure AD tylko w chmurze
Jeśli organizacja ma katalog usługi Azure AD tylko w chmurze, użytkownicy, którzy muszą korzystać z Usług domenowych Azure AD, będą musieli zmienić swoje hasła. Ten proces zmiany haseł powoduje generowanie w usłudze Azure AD skrótów poświadczeń wymaganych przez Usługi domenowe Azure AD na potrzeby uwierzytelniania Kerberos i NTLM. Możesz unieważnić hasła wszystkich użytkowników w dzierżawie, którzy muszą korzystać z Usług domenowych Azure AD, lub poinformować ich o konieczności zmiany haseł.

Poniżej znajdują się instrukcje zmieniania haseł dla użytkowników końcowych:

1. Przejdź do strony panelu dostępu do usługi Azure AD dla organizacji. Jest ona zazwyczaj dostępna pod adresem [http://myapps.microsoft.com](http://myapps.microsoft.com).

2. Wybierz kartę **profilu** na tej stronie.

3. Kliknij kafelek **Zmień hasło** na tej stronie, aby zainicjować zmianę hasła.

    ![Utwórz sieć wirtualną dla Usług domenowych Azure AD.](./media/active-directory-domain-services-getting-started/user-change-password.png)

4. Spowoduje to przejście do strony **zmiany hasła**. Użytkownicy mogą wprowadzić istniejące (stare) hasło i kontynuować jego zmienianie.

    ![Utwórz sieć wirtualną dla Usług domenowych Azure AD.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Nowego hasła w Usługach domenowych Azure AD można używać krótko po jego zmienieniu. Po kilku minutach użytkownicy będą mogli logować się na komputerach dołączonych do domeny zarządzanej przy użyciu nowo zmienionego hasła.


<br>

## Powiązana zawartość

- [Włączanie synchronizacji haseł w Usługach domenowych AAD dla zsynchronizowanego katalogu Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Administrowanie domeną zarządzaną Usług domenowych Azure AD](active-directory-ds-admin-guide-administer-domain.md)

- [Dołączanie maszyny wirtualnej z systemem Windows do domeny zarządzanej Usług domenowych Azure AD](active-directory-ds-admin-guide-join-windows-vm.md)

- [Dołączanie maszyny wirtualnej z systemem Red Hat Enterprise Linux do domeny zarządzanej Usług domenowych Azure AD](active-directory-ds-admin-guide-join-rhel-linux-vm.md)



<!--HONumber=sep16_HO1-->


