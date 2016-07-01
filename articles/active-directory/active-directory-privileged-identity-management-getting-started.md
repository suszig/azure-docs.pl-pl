<properties
   pageTitle="Wprowadzenie do aplikacji Azure AD Privileged Identity Management | Microsoft Azure"
   description="Informacje o sposobie zarządzania uprzywilejowanymi tożsamościami przy użyciu aplikacji Azure Active Directory Privileged Identity Management w portalu Azure."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="05/19/2016"
   ms.author="kgremban"/>

# Wprowadzenie do aplikacji Azure AD Privileged Identity Management


Korzystając z aplikacji Azure Active Directory (AD) Privileged Identity Management, można zarządzać tożsamościami uprzywilejowanymi oraz kontrolować i monitorować je, a także uzyskiwać dostęp do zasobów w usłudze Azure AD i innych usługach online firmy Microsoft, takich jak Office 365 lub Microsoft Intune.  

W tym artykule wyjaśniono, jak dodać aplikację Azure AD PIM do pulpitu nawigacyjnego portalu Azure.

## Dodawanie aplikacji Privileged Identity Management

Przed użyciem aplikacji Azure AD Privileged Identity Management należy dodać ją do pulpitu nawigacyjnego portalu Azure.

1. Zaloguj się do [portalu Azure](https://portal.azure.com/) jako administrator globalny katalogu.
2. Jeśli organizacja ma więcej niż jeden katalog, kliknij swoją nazwę użytkownika w prawym górnym rogu portalu Azure, a następnie wybierz katalog, w którym będziesz używać aplikacji PIM.
3. Wybierz ikonę **Nowy** na lewym pasku nawigacyjnym.
4. Wybierz pozycję **Bezpieczeństwo i obsługa tożsamości**.
5. Wybierz pozycję **Azure AD Privileged Identity Management**.
6. Zaznacz opcję **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij przycisk **Utwórz**. Zostanie otwarta aplikacja Privileged Identity Management.


Jeśli jesteś pierwszą osobą, która będzie używać aplikacji Azure AD Privileged Identity Management w danym katalogu, [kreator zabezpieczeń](active-directory-privileged-identity-management-security-wizard.md) przeprowadzi Cię przez początkowy etap przypisania. Następnie automatycznie zostaniesz pierwszym **administratorem zabezpieczeń** i **administratorem ról uprzywilejowanych** katalogu. Tylko administrator ról uprzywilejowanych może uzyskać dostęp do tej aplikacji w celu zarządzania dostępem dla innych administratorów.  

W przeciwnym razie, jeśli inny administrator ról uprzywilejowanych przypisał do Ciebie co najmniej jedną rolę, możesz wybrać rolę, która zostanie uaktywniona. Jeśli jesteś administratorem ról uprzywilejowanych, widoczna będzie również opcja **Zarządzaj tożsamościami**.  


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Następne kroki

Temat [Przegląd aplikacji Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) zawiera informacje na temat zarządzania dostępem administracyjnym w organizacji.

[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]



<!--HONumber=Jun16_HO2-->


