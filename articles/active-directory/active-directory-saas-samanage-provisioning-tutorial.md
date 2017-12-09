---
title: "Samouczek: Konfigurowanie Samanage dla użytkownika automatycznego inicjowania obsługi administracyjnej z usługą Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować usługi Azure Active Directory, aby automatycznie zapewnianie i usuwanie kont użytkowników do Samanage."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: 3f3b2b01d909b5c1d9373a33644e661723178dee
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="tutorial-configuring-samanage-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie Samanage dla użytkownika automatycznego inicjowania obsługi administracyjnej.


Celem tego samouczka jest opisano czynności, które należy wykonać w Samanage i Azure AD, aby automatycznie zapewnianie i usuwanie kont użytkowników z usługi Azure AD Samanage. 

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że już następujące elementy:

*   Dzierżawy usługi Azure Active directory
*   Dzierżawcy Samanage z [Professional planu](https://www.samanage.com/pricing/) lub lepiej jest włączone 
*   Konto użytkownika z uprawnieniami administratora w Samanage 

> [!NOTE]
> Zależy od usługi Azure AD, inicjowania obsługi administracyjnej integracji [interfejsu API REST Samanage](https://www.samanage.com/api/), który jest dostępny dla zespołów Samanage w planie Professional lub większą.

## <a name="assigning-users-to-samanage"></a>Przypisywanie użytkowników do Samanage

Usługi Azure Active Directory używa pojęcie o nazwie "przypisania" w celu określenia, którzy użytkownicy powinien otrzymać dostęp do wybranej aplikacji. W kontekście użytkownika automatyczne Inicjowanie obsługi konta tylko użytkownicy i grupy, które "przypisano" do aplikacji w usłudze Azure AD jest zsynchronizowany. 

Przed Skonfiguruj i włącz usługę inicjowania obsługi administracyjnej, należy zdecydować, jakie użytkownicy i/lub grup w usłudze Azure AD reprezentują użytkowników, którzy potrzebują dostępu do aplikacji Samanage. Po decyzję, postępując zgodnie z instrukcjami w tym miejscu można przypisać tych użytkowników do aplikacji Samanage:

[Przypisanie użytkownika lub grupę do aplikacji w przedsiębiorstwie](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Ważne porady dotyczące przypisywania użytkowników do Samanage

*   Zalecane jest jeden jest przypisany użytkownik usługi Azure AD Samanage do testowania konfiguracji inicjowania obsługi administracyjnej. Później można przypisać dodatkowych użytkowników i/lub grup.

*   Przypisanie użytkownika do Samanage, należy wybrać opcję **użytkownika** rola, lub inny prawidłowy specyficzne dla aplikacji (jeśli jest dostępny) w oknie dialogowym przypisania. **Domyślnego dostępu** roli nie działa w przypadku inicjowania obsługi administracyjnej, a użytkownicy są pomijane.

> [!NOTE]
> Dodano funkcję inicjowania obsługi usługi odczytuje wszystkie role niestandardowe zdefiniowane w Samanage i zaimportowanie ich do usługi Azure AD, w którym można je wybrać w oknie dialogowym Wybierz rolę. Te role nie będą widoczne w portalu Azure po włączeniu usługi inicjowania obsługi administracyjnej i jednego cyklu synchronizacji zostało ukończone.

## <a name="configuring-user-provisioning-to-samanage"></a>Konfigurowanie inicjowania obsługi administracyjnej Samanage użytkownika 

Ta sekcja przeprowadzi Cię przez łączenie usługi Azure AD z konta użytkownika w Samanage inicjowania obsługi interfejsu API i konfigurowanie inicjowania obsługi usługi do tworzenia, aktualizacji i wyłączania konta użytkowników przypisane w Samanage w oparciu o przypisania użytkowników i grup w usłudze Azure AD.

> [!TIP]
> Można też włączyć na języku SAML logowania jednokrotnego dla Samanage, wykonując instrukcje podane w [portalu Azure](https://portal.azure.com). Logowanie jednokrotne można skonfigurować niezależnie od automatycznego inicjowania obsługi administracyjnej, chociaż te dwie funkcje uzupełniania siebie nawzajem.


### <a name="configure-automatic-user-account-provisioning-to-samanage-in-azure-ad"></a>Skonfiguruj użytkownika automatyczne Inicjowanie obsługi konta do Samanage w usłudze Azure AD:


1. W [portalu Azure](https://portal.azure.com), przejdź do **usługi Azure Active Directory > aplikacje przedsiębiorstwa > wszystkie aplikacje** sekcji.

2. Jeśli Samanage został już skonfigurowany dla logowania jednokrotnego, wyszukiwanie wystąpieniem Samanage przy użyciu pola wyszukiwania. W przeciwnym razie wybierz **Dodaj** i wyszukaj **Samanage** w galerii aplikacji. Wybierz Samanage w wynikach wyszukiwania, a następnie dodaj go do listy aplikacji.

3. Wybierz wystąpienia programu Samanage, a następnie wybierz **inicjowania obsługi administracyjnej** kartę.

4. Ustaw **tryb obsługi administracyjnej** do **automatyczne**.

    ![Samanage inicjowania obsługi administracyjnej.](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage1.png)

5. W obszarze **poświadczeń administratora** sekcji wejściowych **nazwa użytkownika i hasło administratora** Twojego Samanage konta. 

6. W portalu Azure kliknij **Testuj połączenie** zapewniające usługi Azure AD mogą łączyć się z aplikacji Samanage. Jeśli połączenie nie powiedzie się, upewnij się, że Twoje konto Samanage ma uprawnienia administratora i spróbuj ponownie wykonać krok 5.

7. Wprowadź adres e-mail osoby lub grupy, który powinien zostać wyświetlony inicjowania obsługi administracyjnej powiadomienia o błędach w **wiadomość E-mail z powiadomieniem** pola, a następnie zaznacz pole wyboru "Wyślij wiadomość e-mail z powiadomieniem, gdy wystąpi błąd".

8. Kliknij pozycję **Zapisz**. 

9. W sekcji mapowania wybierz **synchronizacji Azure Active Directory użytkownikom Samanage**.

10. W **mapowań atrybutów** Przejrzyj atrybutów użytkowników, które są synchronizowane z usługi Azure AD Samanage. Atrybuty wybrany jako **pasujące** właściwości są używane do dopasowania kont użytkowników w Samanage dla operacji update. Wybierz przycisk Zapisz, aby zatwierdzić zmiany.

11. Aby włączyć usługi Azure AD usługi dla Samanage inicjowania obsługi administracyjnej, zmień **stan inicjowania obsługi administracyjnej** do **na** w **ustawienia** sekcji

12. Kliknij pozycję **Zapisz**. 

Ta operacja uruchamia wstępnej synchronizacji użytkowników i/lub grupy przypisane do Samanage w sekcji Użytkownicy i grupy. Synchronizacji początkowej zajmuje więcej czasu wykonywania niż kolejne synchronizacje, występujące co około 20 minut, tak długo, jak usługa jest uruchomiona. Można użyć **szczegóły synchronizacji** sekcji, aby monitorować postęp i skorzystaj z linków do inicjowania obsługi administracyjnej raporty działania, które opisują wszystkie akcje wykonywane przez usługę inicjowania obsługi administracyjnej.

Aby uzyskać więcej informacji na temat usługi Azure AD, inicjowanie obsługi dzienników do odczytu, zobacz [raportowania na użytkownika automatyczne Inicjowanie obsługi konta](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zarządzanie aprowizacja konta użytkowników dla aplikacji przedsiębiorstwa](active-directory-enterprise-apps-manage-provisioning.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Następne kroki

* [Dowiedz się, jak należy przejrzeć dzienniki i Uzyskaj raporty dotyczące inicjowania obsługi administracyjnej działania](active-directory-saas-provisioning-reporting.md)
