---
title: 'Samouczek: Integracji Azure Active Directory z piaskownicy Salesforce | Dokumentacja firmy Microsoft'
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i usług Salesforce piaskownicy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 7d3c655a754f83284c386d2007c604a731367814
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-configuring-salesforce-sandbox-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie piaskownicy Salesforce użytkownika automatycznego inicjowania obsługi administracyjnej.

Celem tego samouczka jest opisano czynności, które należy wykonać w piaskownicy Salesforce i Azure AD, aby automatycznie zapewnianie i usuwanie kont użytkowników z usługi Azure AD do usługi Salesforce piaskownicy.

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że już następujące elementy:

*   Dzierżawy usługi Azure Active directory.
*   Musi mieć prawidłową dzierżawy piaskownicy Salesforce pracy lub Salesforce piaskownicy dla instytucji edukacyjnych. Można użyć bezpłatnego konta wersji próbnej dla każdej usługi.
*   Konto użytkownika w piaskownicy usługi Salesforce z uprawnieniami administratora zespołu.

## <a name="assigning-users-to-salesforce-sandbox"></a>Przypisywanie użytkowników do usługi Salesforce piaskownicy

Usługi Azure Active Directory używa pojęcie o nazwie "przypisania" w celu określenia, którzy użytkownicy powinien otrzymać dostęp do wybranej aplikacji. W kontekście użytkownika automatyczne Inicjowanie obsługi konta tylko użytkownicy i grupy, które "przypisano" do aplikacji w usłudze Azure AD są synchronizowane.

Przed Skonfiguruj i włącz usługę inicjowania obsługi administracyjnej, należy zdecydować, jakie użytkownicy i/lub grup w usłudze Azure AD reprezentują użytkowników, którzy potrzebują dostępu do aplikacji Salesforce piaskownicy. Po decyzję, postępując zgodnie z instrukcjami w tym miejscu można przypisać tych użytkowników do aplikacji Salesforce piaskownicy:

[Przypisanie użytkownika lub grupę do aplikacji w przedsiębiorstwie](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-salesforce-sandbox"></a>Ważne porady dotyczące przypisywania użytkowników do usługi Salesforce piaskownicy

* Zalecane jest pojedynczego użytkownika usługi Azure AD jest przypisana do piaskownicy usług Salesforce, aby przetestować konfigurację inicjowania obsługi administracyjnej. Później można przypisać dodatkowych użytkowników i/lub grup.

* Przypisanie użytkownika do piaskownicy Salesforce, musisz wybrać poprawnej roli użytkownika. Rola "Domyślnego dostępu" nie działa w przypadku inicjowania obsługi administracyjnej.

> [!NOTE]
> Ta aplikacja importuje role niestandardowe w piaskownicy Salesforce jako część procesu inicjowania obsługi administracyjnej klienta może chcesz wybrać podczas przypisywania użytkowników.

## <a name="enable-automated-user-provisioning"></a>Włącz automatyczne Inicjowanie obsługi użytkowników

Ta sekcja przeprowadzi Cię przez łączenie usługi Azure AD z konta użytkownika usługi Salesforce piaskownicy inicjowania obsługi interfejsu API i konfigurowanie usługi inicjowania obsługi administracyjnej do tworzenia, aktualizacji i wyłączania przypisany użytkownik, który kont w piaskownicy Salesforce na podstawie użytkownika i grupy przypisania w usłudze Azure AD.

>[!Tip]
>Można też włączone na języku SAML logowania jednokrotnego dla piaskownicy Salesforce, postępując zgodnie z instrukcjami zawarte w [portalu Azure](https://portal.azure.com). Logowanie jednokrotne można skonfigurować niezależnie od automatycznego inicjowania obsługi administracyjnej, chociaż te dwie funkcje uzupełniania siebie nawzajem.

### <a name="to-configure-automatic-user-account-provisioning"></a>Aby skonfigurować konto użytkownika automatycznego inicjowania obsługi administracyjnej:

Celem tej sekcji jest przedstawiają sposób włączania kont użytkowników usługi Active Directory do usługi Salesforce piaskownicy Inicjowanie obsługi użytkowników.

1. W [portalu Azure](https://portal.azure.com), przejdź do **usługi Azure Active Directory > aplikacje przedsiębiorstwa > wszystkie aplikacje** sekcji.

2. Salesforce piaskownicy został już skonfigurowany dla logowania jednokrotnego, wyszukaj wystąpieniem piaskownicy Salesforce za pomocą pola wyszukiwania. W przeciwnym razie wybierz **Dodaj** i wyszukaj **piaskownicy Salesforce** w galerii aplikacji. Wybierz piaskownicy usługi Salesforce z wyników wyszukiwania i dodaj go do listy aplikacji.

3. Wybierz wystąpienie usługi Salesforce piaskownicy, a następnie wybierz **inicjowania obsługi administracyjnej** kartę.

4. Ustaw **tryb obsługi administracyjnej** do **automatyczne**. 
    ![Inicjowanie obsługi administracyjnej](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)

5. W obszarze **poświadczeń administratora** sekcji, skonfiguruj następujące ustawienia konfiguracji:
   
    a. W **nazwa użytkownika administratora** tekstowym, wpisz nazwę, która ma konto Salesforce piaskownicy **administratorem** profilu w witrynie Salesforce.com przypisane.
   
    b. W **hasło administratora** tekstowym, wpisz hasło dla tego konta.

6. Aby uzyskać token zabezpieczeń piaskownicy Salesforce, otwórz nową kartę i zaloguj do tego samego konta administratora usługi Salesforce piaskownicy. W prawym górnym rogu strony, kliknij swoją nazwę, a następnie kliknij **Moje ustawienia**.

     ![Włącz inicjowanie obsługi użytkowników](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "Włącz inicjowanie obsługi użytkowników")
7. W lewym okienku nawigacji, kliknij polecenie **osobistych** rozwiń sekcję powiązane, a następnie kliknij przycisk **zresetować moje tokenu zabezpieczeń**.
  
    ![Włącz inicjowanie obsługi użytkowników](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "Włącz inicjowanie obsługi użytkowników")
8. Na **zresetować moje tokenu zabezpieczeń** kliknij przycisk **zresetować tokenu zabezpieczeń** przycisku.

    ![Włącz inicjowanie obsługi użytkowników](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "Włącz inicjowanie obsługi użytkowników")
9. Sprawdź skrzynki odbiorczej poczty e-mail, skojarzone z tym kontem administratora. Poszukaj wiadomości e-mail z Salesforce Sandbox.com, który zawiera nowy token zabezpieczający.
10. Skopiuj token, przejdź do okna usługi Azure AD i wklej ją do **gniazda tokenu** pola.

11. W portalu Azure kliknij **Testuj połączenie** zapewniające usługi Azure AD mogą łączyć się z aplikacji Salesforce piaskownicy.

12. W **wiadomość E-mail z powiadomieniem** wprowadź adres e-mail osoby lub grupy, który powinien otrzymywać powiadomienia błąd inicjowania obsługi administracyjnej i zaznacz pole wyboru.

13. Kliknij przycisk **zapisać.**  
    
14.  W sekcji mapowania wybierz **synchronizacji Azure użytkownicy usługi Active Directory do usługi Salesforce piaskownicy.**

15. W **mapowań atrybutów** Przejrzyj atrybuty użytkowników, które są synchronizowane z usługi Azure AD do usługi Salesforce piaskownicy. Atrybuty wybrany jako **pasujące** właściwości są używane do dopasowania kont użytkowników w piaskownicy Salesforce dla operacji update. Wybierz przycisk Zapisz, aby zatwierdzić zmiany.

16. Aby włączyć usługi Azure AD usługi Salesforce piaskownica inicjowania obsługi administracyjnej, zmień **stan inicjowania obsługi administracyjnej** do **na** w sekcji Ustawienia

17. Kliknij przycisk **zapisać.**


Rozpoczyna się wstępnej synchronizacji użytkowników i/lub grupy przypisane do piaskownicy Salesforce w sekcji Użytkownicy i grupy. Synchronizacji początkowej zajmuje więcej czasu wykonywania niż kolejne synchronizacje, występujące co około 20 minut, tak długo, jak usługa jest uruchomiona. Można użyć **szczegóły synchronizacji** sekcji, aby monitorować postęp i skorzystaj z linków do inicjowania obsługi administracyjnej raporty działania, które opisują wszystkie akcje wykonywane przez usługę inicjowania obsługi administracyjnej w aplikacji Salesforce piaskownicy.

Można teraz utworzyć konta testowego. Poczekaj maksymalnie 20 minut, aby sprawdzić, czy konto zostało zsynchronizowane z usług salesforce.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zarządzanie aprowizacja konta użytkowników dla aplikacji przedsiębiorstwa](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurowanie rejestracji jednokrotnej](active-directory-saas-salesforcesandbox-tutorial.md)