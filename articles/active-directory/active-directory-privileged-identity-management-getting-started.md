---
title: Wprowadzenie do aplikacji Azure AD Privileged Identity Management | Microsoft Docs
description: "Informacje o sposobie zarządzania uprzywilejowanymi tożsamościami przy użyciu aplikacji Azure Active Directory Privileged Identity Management w witrynie Azure Portal."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 17cdff033cc3dbb199d11c3b8ac1acbc92499877
ms.contentlocale: pl-pl
ms.lasthandoff: 07/10/2017

---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Rozpoczynanie używania aplikacji Azure AD Privileged Identity Management
Aplikacja Azure Active Directory (AD) Privileged Identity Management umożliwia kontrolę i monitorowanie dostępu, a także zarządzanie nim w ramach danej organizacji. Ten zakres obejmuje dostęp do zasobów usługi Azure AD i innych usług online firmy Microsoft, takich jak Office 365 lub Microsoft Intune.

W tym artykule wyjaśniono, jak dodać aplikację Azure AD PIM do pulpitu nawigacyjnego witryny Azure Portal.

## <a name="add-the-privileged-identity-management-application"></a>Dodawanie aplikacji Privileged Identity Management
Przed użyciem aplikacji Azure AD Privileged Identity Management należy dodać ją do pulpitu nawigacyjnego witryny Azure Portal.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/) jako administrator globalny katalogu.
2. Jeśli organizacja dysponuje więcej niż jednym katalogiem, wybierz swoją nazwę użytkownika w prawym górnym rogu witryny Azure Portal. Wybierz katalog, w którym chcesz używać aplikacji PIM.
3. Wybierz polecenie **Więcej usług** i użyj pola tekstowego filtru, aby wyszukać **Azure AD Privileged Identity Management**.
4. Zaznacz opcję **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij pozycję **Utwórz**. Nastąpi otwarcie aplikacji Privileged Identity Management.

Jeśli jesteś pierwszą osobą używającą usługi Azure AD Privileged Identity Management w katalogu, będziesz mieć automatycznie przypisane role **Administrator zabezpieczeń** i **Administrator ról uprzywilejowanych** w katalogu. Tylko administratorzy ról uprzywilejowanych mogą zarządzać przypisaniami ról użytkowników. Ponadto można zdecydować się na uruchomienie [kreatora zabezpieczeń](active-directory-privileged-identity-management-security-wizard.md), który przeprowadzi Cię przez środowisko początkowego odnajdywania i przypisywania.

## <a name="navigate-to-your-tasks"></a>Przejdź do zadań
Po skonfigurowaniu usługi Azure AD Privileged Identity Management zobaczysz blok nawigacji pojawiający się przy każdym otwarciu aplikacji. Użyj tego bloku w celu wykonywania zadań związanych z zarządzaniem tożsamościami.

![Zrzut ekranu przedstawiający zadania najwyższego poziomu dla aplikacji PIM](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

* Opcja **Moje role** przenosi Cię do listy ról przypisanych do użytkownika. W tej sekcji będzie można aktywować wszystkie role, które danemu użytkownikowi przysługują.
* Opcja **Zatwierdzanie żądań (wersja zapoznawcza)** powoduje wyświetlenie listy oczekujących żądań aktywacji od użytkowników w katalogu. [Dowiedz się więcej.](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
* Opcja **Oczekujące żądania (wersja zapoznawcza)** powoduje wyświetlenie bieżących żądań oczekujących na aktywację.
* Opcja **Sprawdź dostęp** przenosi Cię do oczekujących przeglądów dostępu, które należy wykonać niezależnie od tego, czy przeglądasz dostęp dla siebie lub kogoś innego.
* Opcja **Role w katalogach usługi Azure AD** dostępna w obszarze zarządzania jest pulpitem nawigacyjnym dla administratorów ról uprzywilejowanych, który umożliwia m.in. zarządzanie przypisaniami ról, zmienianie ustawień aktywacji roli i rozpoczynanie przeglądów dostępu. Opcje na tym pulpicie nawigacyjnym są wyłączone dla każdego, kto nie jest administratorem roli uprzywilejowanej.

## <a name="next-steps"></a>Następne kroki
Temat [Azure AD Privileged Identity Management overview](active-directory-privileged-identity-management-configure.md) (Omówienie aplikacji Azure AD Privileged Identity Management) zawiera więcej informacji dotyczących zarządzania dostępem administracyjnym w organizacji.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png

