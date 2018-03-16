---
title: "Samouczek: Konfigurowanie Samanage dla użytkownika automatycznego inicjowania obsługi administracyjnej z usługą Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować usługi Azure Active Directory, aby automatycznie zapewnianie i usuwanie kont użytkowników do Samanage."
services: active-directory
documentationcenter: 
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: v-ant
ms.openlocfilehash: 8f11beff2c78386f4c3e8130c8e5d72465b8fe87
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie Samanage dla użytkownika automatycznego inicjowania obsługi administracyjnej.

Celem tego samouczka jest aby zademonstrować czynności wykonywanych w Samanage i Azure Active Directory (Azure AD), aby skonfigurować usługi Azure AD automatycznie zapewnianie i usuwanie użytkowników i/lub grup w celu Samanage.

> [!NOTE]
> W tym samouczku opisano łącznika, rozszerzający usługi dostarczania użytkownika programu Azure AD. Uzyskać ważne szczegóły dotyczące tej usługi nie, jak to działa i często zadawane pytania, zobacz [zautomatyzować użytkownika alokowania i anulowania alokowania do aplikacji SaaS w usłudze Azure Active Directory](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że masz następujące:

*   Dzierżawy usługi Azure Active Directory
*   Dzierżawcy Samanage z [Professional](https://www.samanage.com/pricing/) plan lub lepiej jest włączone 
*   Konto użytkownika z uprawnieniami administratora w Samanage 

> [!NOTE]
> Zależy od usługi Azure AD, inicjowania obsługi administracyjnej integracji [interfejsu API REST Samanage](https://www.samanage.com/api/), który jest dostępny dla zespołów Samanage w planie Professional lub większą.

## <a name="adding-samanage-from-the-gallery"></a>Dodawanie Samanage z galerii
Przed skonfigurowaniem Samanage dla użytkownika automatycznego inicjowania obsługi administracyjnej z usługą Azure AD, należy dodać Samanage z galerii aplikacji Azure AD do listy zarządzanych aplikacji SaaS.

**Aby dodać Samanage z galerii aplikacji Azure AD, wykonaj następujące czynności:**

1. W  **[portalu Azure](https://portal.azure.com)**, w panelu nawigacyjnym po lewej stronie kliknij **usługi Azure Active Directory** ikony. 

    ![Przycisk usługi Azure Active Directory][1]

2. Przejdź do **aplikacje dla przedsiębiorstw** > **wszystkie aplikacje**.

    ![Aplikacje przedsiębiorstwa sekcji][2]
    
3. Aby dodać Samanage, kliknij przycisk **nowej aplikacji** przycisk w górnej części okna dialogowego.

    ![Nowy przycisk aplikacji][3]

4. W polu wyszukiwania wpisz **Samanage**.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage4.png)

5. W panelu wyników wybierz **Samanage**, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać Samanage do listy aplikacji SaaS.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage6.png)

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage5.png)

## <a name="assigning-users-to-samanage"></a>Przypisywanie użytkowników do Samanage

Usługi Azure Active Directory używa pojęcie o nazwie "przypisania" w celu określenia, którzy użytkownicy powinien otrzymać dostęp do wybranej aplikacji. W kontekście użytkownika automatycznego inicjowania obsługi administracyjnej są synchronizowane tylko użytkowników i grupy, które zostały "przypisane" do aplikacji w usłudze Azure AD. 

Przed Konfigurowanie i włączanie automatycznej aprowizacji użytkowników, należy podjąć decyzję dotyczącą użytkowników i/lub grup w usłudze Azure AD będą potrzebować dostępu do Samanage. Po decyzję, można przypisać użytkowników i/lub grup Samanage postępując zgodnie z instrukcjami poniżej:

*   [Przypisanie użytkownika lub grupę do aplikacji w przedsiębiorstwie](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Ważne porady dotyczące przypisywania użytkowników do Samanage

*   Zalecane jest jeden jest przypisany użytkownik usługi Azure AD Samanage próba automatycznego przypisywania konfiguracji użytkowników. Później można przypisać dodatkowych użytkowników i/lub grup.

*   Podczas przypisywania Samanage użytkownika, należy wybrać prawidłową rolą specyficzne dla aplikacji (jeśli jest dostępny) w oknie dialogowym przypisania. Użytkownicy z **domyślnego dostępu** roli są wykluczone z inicjowania obsługi administracyjnej.

## <a name="configuring-automatic-user-provisioning-to-samanage"></a>Konfigurowanie użytkownika automatycznego inicjowania obsługi administracyjnej Samanage

W tej sekcji przedstawiono kroki, aby skonfigurować usługi Azure AD świadczenie usługi do tworzenia, aktualizacji i wyłączanie użytkowników i/lub grup Samanage — w oparciu o przypisania użytkownika i/lub grup w usłudze Azure AD.

> [!TIP]
> Można też włączyć na języku SAML rejestracji jednokrotnej dla Samanage, wykonując instrukcje podane w [Samanage pojedynczego logowania jednokrotnego samouczek](active-directory-saas-samanage-tutorial.md). Logowanie jednokrotne można skonfigurować niezależnie od użytkownika automatycznego inicjowania obsługi administracyjnej, że te dwie funkcje uzupełniania siebie nawzajem. Aby uzyskać więcej informacji, zobacz [Samanage pojedynczego logowania jednokrotnego samouczek](active-directory-saas-samanage-tutorial.md).

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Aby skonfigurować automatyczne Inicjowanie obsługi użytkowników dla Samanage w usłudze Azure AD:

1. Zaloguj się do [portalu Azure](https://portal.azure.com) i przejdź do **usługi Azure Active Directory > aplikacje dla przedsiębiorstw > wszystkie aplikacje**.

2. Wybierz Samanage z listy aplikacji SaaS.
 
    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage7.png)

3. Wybierz **inicjowania obsługi administracyjnej** kartę.
    
    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage8.png)

4. Ustaw **tryb obsługi administracyjnej** do **automatyczne**.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage9.png)

5. W obszarze **poświadczeń administratora** sekcji wejściowych **administratora**, i **hasło administratora** Twojego Samanages konta. Przykłady te wartości są:

    *   W **administratora** pola, nazwa użytkownika konta administratora dzierżawy Samanage wypełniania. Przykład: admin@contoso.com.

    *   W **hasło administratora** pola, wypełnij hasła odpowiadającego wcześniej określona nazwa użytkownika.

6. Przy wprowadzaniu pola pokazane w kroku 5, kliknij przycisk **Testuj połączenie** zapewniające usługi Azure AD mogą łączyć się z Samanage. Jeśli połączenie nie powiedzie się, upewnij się, że Twoje konto Samanage ma uprawnienia administratora i spróbuj ponownie.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage10.png)

7. W **wiadomość E-mail z powiadomieniem** wprowadź adres e-mail osoby lub grupy, które powinny odbierać powiadomienia błąd inicjowania obsługi administracyjnej i zaznacz pole wyboru **wysłać wiadomość e-mail z powiadomieniem, gdy wystąpi błąd**.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage11.png)

8. Kliknij pozycję **Zapisz**.

9. W obszarze **mapowania** zaznacz **synchronizacji Azure Active Directory użytkownikom Samanage**.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage12.png)

10. Przejrzyj atrybuty użytkowników, które są synchronizowane z usługi Azure AD Samanage w **mapowanie atrybutu** sekcji. Atrybuty wybrany jako **pasujące** właściwości są używane do dopasowania kont użytkowników w Samanage dla operacji update. Wybierz **zapisać** przycisk, aby zatwierdzić zmiany.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage13.png)

12. W obszarze **mapowania** zaznacz **synchronizacji Azure grup usługi Active Directory do Samanage**.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage14.png)

13. Przejrzyj atrybuty grupy, które są synchronizowane z usługi Azure AD Samanage w **mapowanie atrybutu** sekcji. Atrybuty wybrany jako **pasujące** właściwości są używane do dopasowania grup w Samanage dla operacji update. Wybierz **zapisać** przycisk, aby zatwierdzić zmiany.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage15.png)

14. Do konfigurowania filtrów zakresu, zapoznaj się z następujących instrukcji podanych w [Scoping filtru samouczek](./active-directory-saas-scoping-filters.md).

15. Aby włączyć usługi Azure AD usługi dla Samanage inicjowania obsługi administracyjnej, zmień **stan inicjowania obsługi administracyjnej** do **na** w **ustawienia** sekcji.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage16.png)

16. Zdefiniuj użytkowników i/lub grup, które chcesz aby obsługiwać Samanage, wybierając odpowiednie wartości w **zakres** w **ustawienia** sekcji.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage17.png)

17. Gdy wszystko będzie gotowe, aby udostępnić, kliknij przycisk **zapisać**.

    ![Samanage Provisioning](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage18.png)

Ta operacja uruchamia synchronizacji początkowej wszystkich użytkowników i/lub grup zdefiniowanych w **zakres** w **ustawienia** sekcji. Synchronizacji początkowej zajmuje więcej czasu wykonywania niż kolejne synchronizacje, występujące co około 40 minut tak długo, jak działa usługi Azure AD, inicjowania obsługi usługi. Można użyć **szczegóły synchronizacji** sekcji, aby monitorować postęp i skorzystaj z linków do inicjowania obsługi administracyjnej działania raport, który opisuje wszystkie akcje wykonywane przez usługę Azure AD usługi na Samanage inicjowania obsługi administracyjnej.

Aby uzyskać więcej informacji na temat usługi Azure AD, inicjowanie obsługi dzienników do odczytu, zobacz [raportowania na użytkownika automatyczne Inicjowanie obsługi konta](./active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Zarządzanie aprowizacja konta użytkowników dla aplikacji przedsiębiorstwa](active-directory-enterprise-apps-manage-provisioning.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Kolejne kroki

* [Dowiedz się, jak należy przejrzeć dzienniki i Uzyskaj raporty dotyczące inicjowania obsługi administracyjnej działania](active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/active-directory-saas-samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
