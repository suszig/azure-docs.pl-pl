---
title: "Samouczek: Konfigurowanie usługi ZenDesk dla użytkownika automatycznego inicjowania obsługi administracyjnej z usługą Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować usługi Azure Active Directory, aby automatycznie zapewnianie i usuwanie kont użytkowników do usługi ZenDesk."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: mtillman
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: d9dcc7f81b32498802d52e864c834d785386985c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie usługi ZenDesk dla użytkownika automatycznego inicjowania obsługi administracyjnej.


Celem tego samouczka jest opisano czynności, które należy wykonać w ZenDesk i Azure AD, aby automatycznie zapewnianie i usuwanie kont użytkowników z usługi Azure AD do usługi ZenDesk. 

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że już następujące elementy:

*   Dzierżawy usługi Azure Active directory
*   Dzierżawcy ZenDesk z [Enterprise plan](https://www.zendesk.com/product/pricing/) lub lepiej jest włączone 
*   Konto użytkownika z uprawnieniami administratora w ZenDesk 

> [!NOTE]
> Zależy od usługi Azure AD, inicjowania obsługi administracyjnej integracji [interfejsu API REST usługi ZenDesk](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), który jest dostępny dla zespołów ZenDesk w planie podstawowych lub większą.

## <a name="assigning-users-to-zendesk"></a>Przypisywanie użytkowników do usługi ZenDesk

Usługi Azure Active Directory używa pojęcie o nazwie "przypisania" w celu określenia, którzy użytkownicy powinien otrzymać dostęp do wybranej aplikacji. W kontekście użytkownika automatyczne Inicjowanie obsługi konta tylko użytkownicy i grupy, które "przypisano" do aplikacji w usłudze Azure AD są synchronizowane. 

Przed Skonfiguruj i włącz usługę inicjowania obsługi administracyjnej, należy zdecydować, jakie użytkownicy i/lub grup w usłudze Azure AD reprezentują użytkowników, którzy potrzebują dostępu do aplikacji usługi ZenDesk. Po decyzję, można przypisać tych użytkowników do aplikacji usługi ZenDesk zgodnie z instrukcjami poniżej:

[Przypisanie użytkownika lub grupę do aplikacji w przedsiębiorstwie](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Ważne porady dotyczące przypisywania użytkowników do usługi ZenDesk

*   Zalecane jest jeden jest przypisany użytkownik usługi Azure AD ZenDesk do testowania konfiguracji inicjowania obsługi administracyjnej. Później można przypisać dodatkowych użytkowników i/lub grup.

*   Przypisanie użytkownika do usługi ZenDesk, należy wybrać opcję **użytkownika** rola, lub inny prawidłowy specyficzne dla aplikacji (jeśli jest dostępny) w oknie dialogowym przypisania. **Domyślnego dostępu** roli nie działa w przypadku inicjowania obsługi administracyjnej, a użytkownicy są pomijane.

> [!NOTE]
> Dodano funkcję inicjowania obsługi usługi odczytuje wszystkie role niestandardowe zdefiniowane w Zendesk i zaimportowanie ich do usługi Azure AD, w którym można je wybrać w oknie dialogowym Wybierz rolę. Te role nie będą widoczne w portalu Azure po włączeniu usługi inicjowania obsługi administracyjnej i jednego cyklu synchronizacji zostało ukończone.

## <a name="configuring-user-provisioning-to-zendesk"></a>Konfigurowanie inicjowania obsługi administracyjnej ZenDesk użytkownika 

Ta sekcja przeprowadzi Cię przez łączenie usługi Azure AD z konta użytkownika w ZenDesk inicjowania obsługi interfejsu API i konfigurowanie inicjowania obsługi usługi do tworzenia, aktualizacji i wyłączania konta użytkowników przypisane w ZenDesk w oparciu o przypisania użytkowników i grup w usłudze Azure AD.

> [!TIP] 
> Można też włączone na języku SAML logowania jednokrotnego dla ZenDesk, postępując zgodnie z instrukcjami zawarte w [portalu Azure](https://portal.azure.com). Logowanie jednokrotne można skonfigurować niezależnie od automatycznego inicjowania obsługi administracyjnej, chociaż te dwie funkcje uzupełniania siebie nawzajem.


### <a name="configure-automatic-user-account-provisioning-to-zendesk-in-azure-ad"></a>Skonfiguruj użytkownika automatyczne Inicjowanie obsługi konta do ZenDesk w usłudze Azure AD


1. W [portalu Azure](https://portal.azure.com), przejdź do **usługi Azure Active Directory > aplikacje przedsiębiorstwa > wszystkie aplikacje** sekcji.

2. Jeśli skonfigurowano już program ZenDesk dla logowania jednokrotnego, wyszukiwanie wystąpienia programu ZenDesk przy użyciu pola wyszukiwania. W przeciwnym razie wybierz **Dodaj** i wyszukaj **ZenDesk** w galerii aplikacji. Wybierz ZenDesk w wynikach wyszukiwania, a następnie dodaj go do listy aplikacji.

3. Wybierz wystąpienia programu ZenDesk, a następnie wybierz **inicjowania obsługi administracyjnej** kartę.

4. Ustaw **tryb obsługi administracyjnej** do **automatyczne**.

    ![Inicjowanie obsługi usługi ZenDesk](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. W obszarze **poświadczeń administratora** sekcji wejściowych **administratora & tokenkey & domeny** generowane przez konto użytkownika usługi ZenDesk (przy użyciu tego konta można znaleźć tokenu: **Admin**  >  **Interfejsu API** > **ustawienia**). 

    ![Inicjowanie obsługi usługi ZenDesk](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. W portalu Azure kliknij **Testuj połączenie** zapewniające usługi Azure AD mogą łączyć się z aplikacji ZenDesk. Jeśli połączenie nie powiedzie się, upewnij się, że Twoje konto usługi ZenDesk ma uprawnienia administratora i spróbuj ponownie wykonać krok 5.

7. Wprowadź adres e-mail osoby lub grupy, który powinien zostać wyświetlony inicjowania obsługi administracyjnej powiadomienia o błędach w **wiadomość E-mail z powiadomieniem** pola, a następnie zaznacz pole wyboru "Wyślij wiadomość e-mail z powiadomieniem, gdy wystąpi błąd".

8. Kliknij pozycję **Zapisz**. 

9. W sekcji mapowania wybierz **synchronizacji Azure Active Directory użytkownikom ZenDesk**.

10. W **mapowań atrybutów** Przejrzyj atrybutów użytkowników, które są synchronizowane z usługi Azure AD ZenDesk. Atrybuty wybrany jako **pasujące** właściwości są używane do dopasowania kont użytkowników w ZenDesk dla operacji update. Wybierz przycisk Zapisz, aby zatwierdzić zmiany.

11. Aby włączyć usługi Azure AD, inicjowania obsługi usługi ZenDesk, zmień **stan inicjowania obsługi administracyjnej** do **na** w **ustawienia** sekcji

12. Kliknij pozycję **Zapisz**. 

Ta operacja uruchamia wstępnej synchronizacji użytkowników i/lub grupy przypisane do usługi ZenDesk w sekcji Użytkownicy i grupy. Synchronizacji początkowej zajmuje więcej czasu wykonywania niż kolejne synchronizacje, występujące co około 20 minut, tak długo, jak usługa jest uruchomiona. Można użyć **szczegóły synchronizacji** sekcji, aby monitorować postęp i skorzystaj z linków do inicjowania obsługi administracyjnej raporty działania, które opisują wszystkie akcje wykonywane przez usługę inicjowania obsługi administracyjnej.

Aby uzyskać więcej informacji na temat usługi Azure AD, inicjowanie obsługi dzienników do odczytu, zobacz [raportowania na użytkownika automatyczne Inicjowanie obsługi konta](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zarządzanie aprowizacja konta użytkowników dla aplikacji przedsiębiorstwa](active-directory-enterprise-apps-manage-provisioning.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Następne kroki

* [Dowiedz się, jak należy przejrzeć dzienniki i Uzyskaj raporty dotyczące inicjowania obsługi administracyjnej działania](active-directory-saas-provisioning-reporting.md)
