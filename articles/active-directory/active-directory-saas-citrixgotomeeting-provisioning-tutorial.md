---
title: 'Samouczek: Integracji Azure Active Directory z Citrix GoToMeeting | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i Citrix GoToMeeting."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1ddfcd991431a11e5c3e306bd5905003d094ac18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-citrix-gotomeeting-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie Citrix GoToMeeting dla użytkownika automatycznego inicjowania obsługi administracyjnej.

Celem tego samouczka jest opisano czynności, które należy wykonać w Citrix GoToMeeting i Azure AD, aby automatycznie zapewnianie i usuwanie kont użytkowników z usługi Azure AD Citrix GoToMeeting.

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że już następujące elementy:

*   Dzierżawy usługi Azure Active directory.
*   Citrix GoToMeeting jednokrotnego włączone subskrypcji.
*   Konto użytkownika w Citrix GoToMeeting z uprawnieniami administratora zespołu.

## <a name="assigning-users-to-citrix-gotomeeting"></a>Przypisywanie użytkowników do Citrix GoToMeeting

Usługi Azure Active Directory używa pojęcie o nazwie "przypisania" w celu określenia, którzy użytkownicy powinien otrzymać dostęp do wybranej aplikacji. W kontekście użytkownika automatyczne Inicjowanie obsługi konta tylko użytkownicy i grupy, które "przypisano" do aplikacji w usłudze Azure AD jest zsynchronizowany.

Przed Skonfiguruj i włącz usługę inicjowania obsługi administracyjnej, należy zdecydować, jakie użytkownicy i/lub grup w usłudze Azure AD reprezentują użytkowników, którzy potrzebują dostępu do aplikacji Citrix GoToMeeting. Po decyzję, można przypisać tych użytkowników do aplikacji Citrix GoToMeeting, postępując zgodnie z instrukcjami poniżej:

[Przypisanie użytkownika lub grupę do aplikacji w przedsiębiorstwie](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-citrix-gotomeeting"></a>Ważne porady dotyczące przypisywania użytkowników do Citrix GoToMeeting

*   Zalecane jest pojedynczego użytkownika usługi Azure AD jest przypisana do Citrix GoToMeeting, aby przetestować konfigurację inicjowania obsługi administracyjnej. Później można przypisać dodatkowych użytkowników i/lub grup.

*   Przypisanie użytkownika do Citrix GoToMeeting, należy wybrać poprawnej roli użytkownika. Rola "Domyślnego dostępu" nie działa w przypadku inicjowania obsługi administracyjnej.

## <a name="enable-automated-user-provisioning"></a>Włącz automatyczne Inicjowanie obsługi użytkowników

Ta sekcja przeprowadzi Cię przez łączenie usługi Azure AD z konta użytkownika Citrix GoToMeeting inicjowania obsługi interfejsu API i konfigurowanie inicjowania obsługi usługi do tworzenia, aktualizacji i wyłączania przypisany użytkownik kont w Citrix GoToMeeting opartych na przypisanie użytkowników i grup w usłudze Azure AD.

> [!TIP]
> Można też włączyć na języku SAML logowania jednokrotnego dla Citrix GoToMeeting, wykonując instrukcje podane w [portalu Azure](https://portal.azure.com). Logowanie jednokrotne można skonfigurować niezależnie od automatycznego inicjowania obsługi administracyjnej, chociaż te dwie funkcje uzupełniania siebie nawzajem.

### <a name="to-configure-automatic-user-account-provisioning"></a>Aby skonfigurować konto użytkownika automatycznego inicjowania obsługi administracyjnej:

1. W [portalu Azure](https://portal.azure.com), przejdź do **usługi Azure Active Directory > aplikacje przedsiębiorstwa > wszystkie aplikacje** sekcji.

2. Jeśli Citrix GoToMeeting został już skonfigurowany dla logowania jednokrotnego, wyszukiwanie wystąpieniem Citrix GoToMeeting, korzystając z pola wyszukiwania. W przeciwnym razie wybierz **Dodaj** i wyszukaj **Citrix GoToMeeting** w galerii aplikacji. Wybierz Citrix GoToMeeting w wynikach wyszukiwania i dodaj ją do listy aplikacji.

3. Wybierz wystąpienia programu Citrix GoToMeeting, a następnie wybierz **inicjowania obsługi administracyjnej** kartę.

4. Ustaw **inicjowania obsługi administracyjnej** tryb **automatyczne**. 

    ![Inicjowanie obsługi administracyjnej](./media/active-directory-saas-citrixgotomeeting-provisioning-tutorial/provisioning.png)

5. W sekcji poświadczenia administratora wykonaj następujące czynności:
   
    a. W **nazwa użytkownika administratora GoToMeeting Citrix** tekstowym, wpisz nazwę użytkownika administratora.

    b. W **hasło administratora GoToMeeting Citrix** pole tekstowe, hasło administratora.

6. W portalu Azure kliknij **Testuj połączenie** zapewniające usługi Azure AD mogą łączyć się z aplikacji Citrix GoToMeeting. Jeśli połączenie nie powiedzie się, upewnij się, Twoje konto Citrix GoToMeeting ma uprawnienia administratora zespołu i spróbuj **"Poświadczeń administratora"** krok ponownie.

7. Wprowadź adres e-mail osoby lub grupy, który powinien zostać wyświetlony inicjowania obsługi administracyjnej powiadomienia o błędach w **wiadomość E-mail z powiadomieniem** pola, a następnie zaznacz pole wyboru.

8. Kliknij przycisk **zapisać.**

9. W sekcji mapowania wybierz **synchronizacji Azure Active Directory użytkownikom Citrix GoToMeeting.**

10. W **mapowań atrybutów** Przejrzyj atrybutów użytkowników, które są synchronizowane z usługi Azure AD Citrix GoToMeeting. Atrybuty wybrany jako **pasujące** właściwości są używane do dopasowania kont użytkowników w Citrix GoToMeeting dla operacji update. Wybierz przycisk Zapisz, aby zatwierdzić zmiany.

11. Aby włączyć usługi Azure AD, świadczenie usługi dla Citrix GoToMeeting, zmień **stan inicjowania obsługi administracyjnej** do **na** w sekcji Ustawienia

12. Kliknij przycisk **zapisać.**

Rozpoczyna się wstępnej synchronizacji użytkowników i/lub grupy przypisane do Citrix GoToMeeting w sekcji Użytkownicy i grupy. Synchronizacji początkowej zajmuje więcej czasu wykonywania niż kolejne synchronizacje, występujące co około 20 minut, tak długo, jak usługa jest uruchomiona. Można użyć **szczegóły synchronizacji** sekcji, aby monitorować postęp i skorzystaj z linków do inicjowania obsługi administracyjnej raporty działania, które opisują wszystkie akcje wykonywane przez usługę inicjowania obsługi administracyjnej w aplikacji Citrix GoToMeeting.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zarządzanie aprowizacja konta użytkowników dla aplikacji przedsiębiorstwa](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurowanie rejestracji jednokrotnej](active-directory-saas-citrix-gotomeeting-tutorial.md)


