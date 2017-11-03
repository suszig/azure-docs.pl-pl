---
title: "Samouczek: Skonfiguruj miejsca pracy przez Facebook dla Inicjowanie obsługi użytkowników | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak automatycznie zapewnianie i usuwanie kont użytkowników z usługi Azure AD do miejsca pracy przez usługi Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: a347eedbf5511dc83e1bc7721667441cfb87cb59
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-configure-workplace-by-facebook-for-user-provisioning"></a>Samouczek: Skonfiguruj miejsca pracy przez Facebook dla Inicjowanie obsługi użytkowników

W tym samouczku przedstawiono kroki niezbędne do automatycznego udostępniania i usuwanie kont użytkowników z usługi Azure Active Directory (Azure AD) do miejsca pracy przez usługi Facebook.

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z miejsca pracy przez usługi Facebook, potrzebne są następujące elementy:

- Subskrypcję usługi Azure AD
- Dołączanie w serwisie Facebook rejestracji jednokrotnej (SSO) włączone subskrypcji

Aby przetestować kroki opisane w tym samouczku, wykonaj te zalecenia:

- Nie należy używać środowiska produkcyjnego, jeśli jest to konieczne.
- Jeśli nie masz środowisko wersji próbnej usługi Azure AD, możesz pobrać [miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-to-workplace-by-facebook"></a>Przypisywanie użytkowników do miejsca pracy przez usługi Facebook

Usługi Azure AD używa pojęcie o nazwie "przypisania" w celu określenia, którzy użytkownicy powinien otrzymać dostęp do wybranej aplikacji. W kontekście użytkownika automatyczne Inicjowanie obsługi konta tylko użytkownicy i grupy, które zostały przypisane do aplikacji w usłudze Azure AD są synchronizowane.

Przed Skonfiguruj i włącz usługę inicjowania obsługi administracyjnej, zdecyduj, jakie użytkowników i grup w usłudze Azure AD reprezentują użytkowników, którzy potrzebują dostępu do miejsca pracy przez aplikację usługi Facebook. Następnie można przypisać użytkowników do miejsca pracy przez aplikację usługi Facebook zgodnie z instrukcjami w [przypisać użytkownika lub grupy do aplikacji przedsiębiorstwa](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

>[!IMPORTANT]
>*   Przetestuj konfigurację inicjowania obsługi administracyjnej, przypisując pojedynczego użytkownika usługi Azure AD do miejsca pracy przez usługi Facebook. Przypisz później dodatkowych użytkowników i grup.
>*   Gdy użytkownik jest przypisany do miejsca pracy przez usługi Facebook, musisz wybrać poprawnej roli użytkownika. Rola domyślnego dostępu nie działa w przypadku inicjowania obsługi administracyjnej.

## <a name="enable-automated-user-provisioning"></a>Włącz inicjowanie obsługi użytkowników automatycznych

Ta sekcja przeprowadzi Cię przez łączenie usługi Azure AD z konta użytkownika, inicjowanie obsługi interfejsu API z miejsca pracy przez usługi Facebook. Możesz również Dowiedz się, jak skonfigurować usługę inicjowania obsługi administracyjnej do tworzenia, aktualizacji i wyłączenie konta użytkowników przypisane w pracy przez usługi Facebook. To jest ustalane na podstawie użytkownika i przypisanie do grupy w usłudze Azure AD.

>[!Tip]
>Można również włączyć na języku SAML logowania jednokrotnego dla miejsca pracy przez Facebook, przez zgodnie z instrukcjami podanymi w [portalu Azure](https://portal.azure.com). Chociaż te dwie funkcje uzupełniają logowania jednokrotnego można skonfigurować niezależnie od automatyczne udostępnianie.

### <a name="configure-user-account-provisioning-to-workplace-by-facebook-in-azure-ad"></a>Skonfiguruj konto użytkownika do miejsca pracy przez Facebook inicjowania obsługi administracyjnej w usłudze Azure AD

Usługi Azure AD umożliwia automatyczną synchronizację szczegóły konta użytkowników przypisane do miejsca pracy przez usługi Facebook. To automatyczną synchronizację umożliwia dołączanie przez Facebook, aby pobrać dane, należy go autoryzacji użytkowników dostępu przed ich próbował zarejestrować się po raz pierwszy. Również cofnąć udostępnia użytkowników z miejsca pracy przez usługi Facebook, gdy został odwołany dostępu w usłudze Azure AD.

1. W [portalu Azure](https://portal.azure.com), wybierz pozycję **usługi Azure Active Directory** > **aplikacje przedsiębiorstwa** > **wszystkie aplikacje**.

2. Jeśli skonfigurowano już miejsca pracy przez Facebook dla logowania jednokrotnego, wyszukiwanie wystąpienia miejsca pracy przez Facebook przy użyciu pola wyszukiwania. W przeciwnym razie wybierz **Dodaj** i wyszukaj **miejsca pracy przez Facebook** w galerii aplikacji. Wybierz **miejsca pracy przez Facebook** w wynikach wyszukiwania i dodaj go do listy aplikacji.

3. Wybierz wystąpienie miejsca pracy przez usługi Facebook, a następnie wybierz **inicjowania obsługi administracyjnej** kartę.

4. Ustaw **tryb obsługi administracyjnej** do **automatyczne**. 

    ![Zrzut ekranu z miejsca pracy przez opcje inicjowania obsługi usługi Facebook](./media/active-directory-saas-facebook-at-work-provisioning-tutorial/provisioning.png)

5. W obszarze **poświadczeń administratora** wprowadź **klucz tajny tokenu** i **adres URL dzierżawy** z miejsca pracy przez administratora usługi Facebook.

6. W portalu Azure wybierz **Testuj połączenie** zapewniające usługi Azure AD mogą łączyć się z miejsca pracy przez aplikację usługi Facebook. Jeśli połączenie nie powiedzie się, upewnij się, że miejsca pracy przez Facebook konto ma uprawnienia administratora zespołu.

7. Wprowadź adres e-mail osoby lub grupy, który powinien zostać wyświetlony inicjowania obsługi administracyjnej powiadomienia o błędach w **wiadomość E-mail z powiadomieniem** pola, a następnie zaznacz pole wyboru.

8. Wybierz pozycję **Zapisz**.

9. W sekcji mapowania wybierz **synchronizacji Azure użytkownicy usługi Active Directory do miejsca pracy przez Facebook**.

10. W **mapowań atrybutów** Przejrzyj atrybutów użytkowników, które są synchronizowane z usługi Azure AD do miejsca pracy przez usługi Facebook. Atrybuty wybrany jako **pasujące** właściwości są używane do dopasowania kont użytkowników w miejscu pracy przez Facebook dla operacji update. Aby zatwierdzić zmiany, wybierz **zapisać**.

11. Aby włączyć usługi Azure AD usługi dla miejsca pracy przez Facebook, inicjowania obsługi administracyjnej w **ustawienia** Zmień **stan inicjowania obsługi administracyjnej** do **na**.

12. Wybierz pozycję **Zapisz**.

Aby uzyskać więcej informacji na temat konfigurowania automatycznego inicjowania obsługi administracyjnej, zobacz [dokumentacji Facebook](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers).

Można teraz utworzyć konta testowego. Poczekaj maksymalnie 20 minut, aby sprawdzić, czy konto zostało zsynchronizowane w miejscu pracy przez usługi Facebook.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zarządzanie aprowizacja konta użytkowników dla aplikacji przedsiębiorstwa](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurowanie rejestracji jednokrotnej](active-directory-saas-facebook-at-work-tutorial.md)

