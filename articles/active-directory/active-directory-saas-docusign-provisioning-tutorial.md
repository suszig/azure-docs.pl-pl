---
title: 'Samouczek: Integracji Azure Active Directory z DocuSign | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i DocuSign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 294cd6b8-74d7-44bc-92bc-020ccd13ff12
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 3b509ffa934949200277ae431761d2accd4a02d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-configuring-docusign-for-user-provisioning"></a>Samouczek: Konfigurowanie DocuSign Inicjowanie obsługi użytkowników

Celem tego samouczka jest opisano czynności, które należy wykonać w DocuSign i Azure AD, aby automatycznie zapewnianie i usuwanie kont użytkowników z usługi Azure AD DocuSign.

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że już następujące elementy:

*   Dzierżawy usługi Azure Active directory.
*   DocuSign logowanie jednokrotne włączone subskrypcji.
*   Konto użytkownika w DocuSign z uprawnieniami administratora zespołu.

## <a name="assigning-users-to-docusign"></a>Przypisywanie użytkowników do DocuSign

Usługi Azure Active Directory używa pojęcie o nazwie "przypisania" w celu określenia, którzy użytkownicy powinien otrzymać dostęp do wybranej aplikacji. W kontekście użytkownika automatyczne Inicjowanie obsługi konta tylko użytkownicy i grupy, które "przypisano" do aplikacji w usłudze Azure AD są synchronizowane.

Przed Skonfiguruj i włącz usługę inicjowania obsługi administracyjnej, należy zdecydować, jakie użytkownicy i/lub grup w usłudze Azure AD reprezentują użytkowników, którzy potrzebują dostępu do aplikacji DocuSign. Po decyzję, postępując zgodnie z instrukcjami w tym miejscu można przypisać tych użytkowników do aplikacji DocuSign:

[Przypisanie użytkownika lub grupę do aplikacji w przedsiębiorstwie](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-docusign"></a>Ważne porady dotyczące przypisywania użytkowników do DocuSign

*   Zalecane jest jeden jest przypisany użytkownik usługi Azure AD DocuSign do testowania konfiguracji inicjowania obsługi administracyjnej. Później można przypisać dodatkowych użytkowników i/lub grup.

*   Przypisanie użytkownika do DocuSign, musisz wybrać poprawnej roli użytkownika. Rola "Domyślnego dostępu" nie działa w przypadku inicjowania obsługi administracyjnej.

## <a name="enable-user-provisioning"></a>Włącz inicjowanie obsługi użytkowników

Ta sekcja przeprowadzi Cię przez łączenie usługi Azure AD z konta użytkownika w DocuSign inicjowania obsługi interfejsu API i konfigurowanie inicjowania obsługi usługi do tworzenia, aktualizacji i wyłączania konta użytkowników przypisane w DocuSign w oparciu o przypisania użytkowników i grup w usłudze Azure AD.

> [!Tip]
> Można też włączyć na języku SAML logowania jednokrotnego dla DocuSign, wykonując instrukcje podane w [portalu Azure](https://portal.azure.com). Logowanie jednokrotne można skonfigurować niezależnie od automatycznego inicjowania obsługi administracyjnej, chociaż te dwie funkcje uzupełniania siebie nawzajem.

### <a name="to-configure-user-account-provisioning"></a>Aby skonfigurować, inicjowanie obsługi konta użytkownika:

Celem tej sekcji jest przedstawiają sposób włączania kont użytkowników usługi Active Directory do DocuSign Inicjowanie obsługi użytkowników.

1. W [portalu Azure](https://portal.azure.com), przejdź do **usługi Azure Active Directory > aplikacje przedsiębiorstwa > wszystkie aplikacje** sekcji.

2. Jeśli DocuSign został już skonfigurowany dla logowania jednokrotnego, wyszukiwanie wystąpieniem DocuSign przy użyciu pola wyszukiwania. W przeciwnym razie wybierz **Dodaj** i wyszukaj **DocuSign** w galerii aplikacji. Wybierz DocuSign w wynikach wyszukiwania, a następnie dodaj go do listy aplikacji.

3. Wybierz wystąpienia programu DocuSign, a następnie wybierz **inicjowania obsługi administracyjnej** kartę.

4. Ustaw **tryb obsługi administracyjnej** do **automatyczne**. 

    ![Inicjowanie obsługi administracyjnej](./media/active-directory-saas-docusign-provisioning-tutorial/provisioning.png)

5. W obszarze **poświadczeń administratora** sekcji, skonfiguruj następujące ustawienia konfiguracji:
   
    a. W **nazwa użytkownika administratora** pole tekstowe, typ, nazwa, która ma konta DocuSign **Administrator systemu** profil DocuSign.com przypisane.
   
    b. W **hasło administratora** tekstowym, wpisz hasło dla tego konta.

6. W portalu Azure kliknij **Testuj połączenie** zapewniające usługi Azure AD mogą łączyć się z aplikacji DocuSign.

7. W **wiadomość E-mail z powiadomieniem** wprowadź adres e-mail osoby lub grupy, który powinien otrzymywać powiadomienia błąd inicjowania obsługi administracyjnej i zaznacz pole wyboru.

8. Kliknij przycisk **zapisać.**

9. W sekcji mapowania wybierz **synchronizacji Azure Active Directory użytkownikom DocuSign.**

10. W **mapowań atrybutów** Przejrzyj atrybutów użytkowników, które są synchronizowane z usługi Azure AD DocuSign. Atrybuty wybrany jako **pasujące** właściwości są używane do dopasowania kont użytkowników w DocuSign dla operacji update. Wybierz przycisk Zapisz, aby zatwierdzić zmiany.

11. Aby włączyć usługi Azure AD usługi dla DocuSign inicjowania obsługi administracyjnej, zmień **stan inicjowania obsługi administracyjnej** do **na** w sekcji Ustawienia

12. Kliknij przycisk **zapisać.**

Rozpoczyna się wstępnej synchronizacji użytkowników i/lub grupy przypisane do DocuSign w sekcji Użytkownicy i grupy. Synchronizacji początkowej zajmuje więcej czasu wykonywania niż kolejne synchronizacje, występujące co około 20 minut, tak długo, jak usługa jest uruchomiona. Można użyć **szczegóły synchronizacji** sekcji, aby monitorować postęp i skorzystaj z linków do inicjowania obsługi administracyjnej raporty działania, które opisują wszystkie akcje wykonywane przez usługę inicjowania obsługi administracyjnej na DocuSign aplikacji.

Można teraz utworzyć konta testowego. Aby sprawdzić, czy konto zostało zsynchronizowane z DocuSign poczekaj maksymalnie 20 minut.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zarządzanie aprowizacja konta użytkowników dla aplikacji przedsiębiorstwa](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurowanie rejestracji jednokrotnej](active-directory-saas-docusign-tutorial.md)