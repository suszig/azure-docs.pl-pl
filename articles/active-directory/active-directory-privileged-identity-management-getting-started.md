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
   ms.date="09/16/2016"
   ms.author="kgremban"/>


# Wprowadzenie do aplikacji Azure AD Privileged Identity Management


Aplikacja Azure Active Directory (AD) Privileged Identity Management umożliwia kontrolę i monitorowanie dostępu, a także zarządzanie nim w ramach danej organizacji. Dotyczy to również dostępu do zasobów usługi Azure AD i innych usług online firmy Microsoft, takich jak Office 365 lub Microsoft Intune.

W tym artykule wyjaśniono, jak dodać aplikację Azure AD PIM do pulpitu nawigacyjnego witryny Azure Portal.

## Dodawanie aplikacji Privileged Identity Management

Przed użyciem aplikacji Azure AD Privileged Identity Management należy dodać ją do pulpitu nawigacyjnego witryny Azure Portal.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/) jako administrator globalny katalogu.
2. Jeśli organizacja dysponuje więcej niż jednym katalogiem, wybierz swoją nazwę użytkownika w prawym górnym rogu witryny Azure Portal. Wybierz katalog, w którym będziesz używać aplikacji PIM.
3. Wybierz polecenie **Więcej usług** i użyj pola tekstowego filtru, aby wyszukać **Azure AD Privileged Identity Management**.
4. Zaznacz opcję **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij pozycję **Utwórz**. Nastąpi otwarcie aplikacji Privileged Identity Management.


Jeśli jesteś pierwszą osobą, która będzie używać aplikacji Azure AD Privileged Identity Management w danym katalogu [kreator zabezpieczeń](active-directory-privileged-identity-management-security-wizard.md) przeprowadzi Cię przez początkowy etap przypisania. Następnie automatycznie zostaniesz pierwszym **administratorem zabezpieczeń** i **administratorem ról uprzywilejowanych** katalogu. Tylko administrator ról uprzywilejowanych może uzyskać dostęp do tej aplikacji w celu zarządzania dostępem dla innych administratorów.  

## Przejdź do zadań

Po skonfigurowaniu usługi Azure AD Privileged Identity Management zobaczysz blok nawigacji pojawiający się przy każdym otwarciu aplikacji. Użyj tego bloku w celu wykonywania zadań związanych z zarządzaniem tożsamościami.

![Zrzut ekranu przedstawiający zadania najwyższego poziomu dla aplikacji PIM](./media/active-directory-privileged-identity-management-getting-started/pim_tasks.png)

- Opcja **Uaktywnij moje role** przenosi do listy ról przypisanych do użytkownika. W tym miejscu będzie można uaktywniać wszystkie role, które danemu użytkownikowi przysługują.

- Opcja **Zarządzanie rolami uprzywilejowanymi** jest pulpitem nawigacyjnym dla administratorów roli uprzywilejowanej, który umożliwia m.in. zarządzanie przypisaniami ról, zmienianie ustawień aktywacji roli i rozpoczynanie przeglądów dostępu. Opcje na tym pulpicie nawigacyjnym są wyłączone dla każdego, kto nie jest administratorem roli uprzywilejowanej.

- Opcja **Sprawdź dostęp uprzywilejowany** przenosi do oczekujących przeglądów dostępu, które należy wykonać niezależnie od tego, czy przeglądasz dostęp dla siebie lub kogoś innego. 


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Następne kroki

Temat [Azure AD Privileged Identity Management overview](active-directory-privileged-identity-management-configure.md) (Omówienie aplikacji Azure AD Privileged Identity Management) zawiera więcej informacji dotyczących zarządzania dostępem administracyjnym w organizacji.

[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png



<!--HONumber=Sep16_HO3-->


