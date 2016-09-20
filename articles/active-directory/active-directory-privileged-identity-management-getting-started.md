<properties
   pageTitle="Wprowadzenie do aplikacji Azure AD Privileged Identity Management | Microsoft Azure"
   description="Informacje o sposobie zarządzania uprzywilejowanymi tożsamościami przy użyciu aplikacji Azure Active Directory Privileged Identity Management w witrynie Azure Portal."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/29/2016"
   ms.author="kgremban"/>

# Wprowadzenie do aplikacji Azure AD Privileged Identity Management


Korzystając z aplikacji Azure Active Directory (AD) Privileged Identity Management, można zarządzać tożsamościami uprzywilejowanymi oraz kontrolować i monitorować je, a także uzyskiwać dostęp do zasobów w usłudze Azure AD i innych usługach online firmy Microsoft, takich jak Office 365 lub Microsoft Intune.  

W tym artykule wyjaśniono, jak dodać aplikację Azure AD PIM do pulpitu nawigacyjnego witryny Azure Portal.

## Dodawanie aplikacji Privileged Identity Management

Przed użyciem aplikacji Azure AD Privileged Identity Management należy dodać ją do pulpitu nawigacyjnego witryny Azure Portal.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/) jako administrator globalny katalogu.
2. Jeśli organizacja ma więcej niż jeden katalog, kliknij swoją nazwę użytkownika w prawym górnym rogu witryny Azure Portal, a następnie wybierz katalog, w którym będziesz używać aplikacji PIM.
3. Wybierz pozycję **Nowy** > **Bezpieczeństwo i obsługa tożsamości** > **Azure AD Privileged Identity Management**.

    ![Włącz usługę PIM w portalu][1]

4. Zaznacz opcję **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij pozycję **Utwórz**. Zostanie otwarta aplikacja Privileged Identity Management.


Jeśli jesteś pierwszą osobą, która będzie używać aplikacji Azure AD Privileged Identity Management w danym katalogu, [kreator zabezpieczeń](active-directory-privileged-identity-management-security-wizard.md) przeprowadzi Cię przez początkowy etap przypisania. Następnie automatycznie zostaniesz pierwszym **administratorem zabezpieczeń** i **administratorem ról uprzywilejowanych** katalogu. Tylko administrator ról uprzywilejowanych może uzyskać dostęp do tej aplikacji w celu zarządzania dostępem dla innych administratorów.  

Jeśli masz przypisaną co najmniej jedną rolę, dostępna jest opcja **Uaktywnij moje role**. Jeśli jesteś administratorem ról uprzywilejowanych, widoczna jest także opcja **Zarządzanie uprawnionymi rolami**.  


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Następne kroki

Temat [Przegląd aplikacji Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) zawiera informacje na temat zarządzania dostępem administracyjnym w organizacji.

[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png



<!--HONumber=sep16_HO1-->


