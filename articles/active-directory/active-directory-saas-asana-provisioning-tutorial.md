---
title: "Samouczek: Konfigurowanie Asana dla użytkownika automatycznego inicjowania obsługi administracyjnej z usługą Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować usługi Azure Active Directory, aby automatycznie zapewnianie i usuwanie kont użytkowników do Asana."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 4f3bd11a99f43d6405ea285a7a283179d561f92a
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie Asana dla użytkownika automatycznego inicjowania obsługi administracyjnej.

Celem tego samouczka jest opisano czynności, które należy wykonać w Asana i Azure AD, aby automatycznie zapewnianie i usuwanie kont użytkowników z usługi Azure AD Asana.

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że już następujące elementy:

*   Dzierżawy usługi Azure Active Active directory
*   Dzierżawę Asana z [Enterprise](https://www.asana.com/pricing) plan lub lepiej jest włączone 
*   Konto użytkownika z uprawnieniami administratora w Asana 

> [!NOTE] 
> Zależy od usługi Azure AD, inicjowania obsługi administracyjnej integracji [Asana API](https://app.asana.com/api/1.0/scim/Users) które są dostępne dla Asana.

## <a name="assigning-users-to-asana"></a>Przypisywanie użytkowników do Asana

Usługi Azure Active Directory używa pojęcie o nazwie "przypisania" w celu określenia, którzy użytkownicy powinien otrzymać dostęp do wybranej aplikacji. W kontekście użytkownika automatyczne Inicjowanie obsługi konta użytkowników przypisane "" do aplikacji w usłudze Azure AD będą synchronizowane. 

Przed Skonfiguruj i włącz usługę inicjowania obsługi administracyjnej, należy zdecydować, co użytkownicy w usłudze Azure AD reprezentują użytkowników, którzy potrzebują dostępu do aplikacji Asana. Po decyzję, postępując zgodnie z instrukcjami w tym miejscu można przypisać tych użytkowników do aplikacji Asana:

[Przypisz użytkownika do aplikacji w przedsiębiorstwie](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Ważne porady dotyczące przypisywania użytkowników do Asana

*   Zalecane jest pojedynczego użytkownika usługi Azure AD można przypisać do Asana do testowania konfiguracji inicjowania obsługi administracyjnej. Później można przypisać dodatkowych użytkowników.

## <a name="configuring-user-provisioning-to-asana"></a>Konfigurowanie inicjowania obsługi administracyjnej Asana użytkownika 

Ta sekcja przeprowadzi Cię przez usługę Azure AD nawiązywania połączenia z kontem użytkownika Asana inicjowania obsługi interfejsu API i konfigurowanie usługi inicjowania obsługi administracyjnej do tworzenia, aktualizacji i wyłączyć przypisany kont użytkowników w Asana oparte na przypisanie użytkownika w usłudze Azure AD.

> [!TIP]
> Można też włączyć na języku SAML logowania jednokrotnego dla Asana, wykonując instrukcje podane w [portalu Azure](https://portal.azure.com). Logowanie jednokrotne można skonfigurować niezależnie od automatycznego inicjowania obsługi administracyjnej, chociaż te dwie funkcje uzupełniania siebie nawzajem.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Aby skonfigurować użytkownika automatyczne Inicjowanie obsługi konta do Asana w usłudze Azure AD:

1)  W [portalu Azure](https://portal.azure.com), przejdź do **usługi Azure Active Directory > aplikacje przedsiębiorstwa > wszystkie aplikacje** sekcji.

2) Jeśli Asana został już skonfigurowany dla logowania jednokrotnego, wyszukiwanie wystąpieniem Asana przy użyciu pola wyszukiwania. W przeciwnym razie wybierz **Dodaj** i wyszukaj **Asana** w galerii aplikacji. Wybierz **Asana** z wyników wyszukiwania i dodaj go do listy aplikacji.

3)  Wybierz wystąpienia programu Asana, a następnie wybierz **inicjowania obsługi administracyjnej** kartę.

4)  Ustaw **tryb obsługi administracyjnej** do **automatyczne**.

![Asana Provisioning](./media/active-directory-saas-asana-provisioning-tutorial/asanaazureprovisioning.png)

5) W sekcji poświadczenia administratora, postępuj zgodnie z poniższych instrukcji, aby wygenerować token i wprowadź go w **klucz tajny tokenu** pola tekstowego.

* Zaloguj się do [Asana](https://app.asana.com) przy użyciu konta administratora
* Kliknij zdjęcia profilu, z górnym pasku, a następnie zaznacz bieżące ustawienia Nazwa organizacji
* Przejdź do karty kont usług
* Kliknij pozycję Dodaj konto usługi
* Zaktualizuj o nazwę i zdjęcia profilu, zgodnie z potrzebami, skopiuj **tokenu** i kliknij przycisk Zapisz zmiany

6) W portalu Azure kliknij **Testuj połączenie** zapewniające usługi Azure AD mogą łączyć się z aplikacji Asana. Jeśli połączenie nie powiedzie się, upewnij się, Twoje konto Asana ma uprawnienia administratora i spróbuj **"Test połączenia"** krok ponownie.

7) Wprowadź adres e-mail osoby lub grupy, który powinien zostać wyświetlony inicjowania obsługi administracyjnej powiadomienia o błędach w **wiadomość E-mail z powiadomieniem** pola, a następnie zaznacz pole wyboru poniżej.

8) Kliknij pozycję **Zapisz**. 

9) W sekcji mapowania wybierz **synchronizacji Azure Active Directory użytkownikom Asana**.

10) W **mapowań atrybutów** Przejrzyj atrybutów użytkowników, które są synchronizowane z usługą Azure AD do Asana. Należy pamiętać, że atrybuty wybrany jako **pasujące** właściwości, które będą używane do dopasowania kont użytkowników w Asana dla operacji update. Wybierz **zapisać** przycisk, aby zatwierdzić zmiany. Zobacz [Dostosowywanie użytkownika inicjowania obsługi administracyjnej mapowań atrybutów](active-directory-saas-customizing-attribute-mappings.md) więcej szczegółów

11) Aby włączyć usługi Azure AD usługi dla Asana inicjowania obsługi administracyjnej, zmień **stan inicjowania obsługi administracyjnej** do **na** w **ustawienia** sekcji

12) Kliknij pozycję **Zapisz**. 

Spowoduje to uruchomienie synchronizacji początkowej żadnych użytkowników przypisanych do Asana w sekcji użytkownicy. Początkowy zajmuje więcej czasu wykonywania niż kolejne synchronizacje, występujące co około 20 minut, tak długo, jak usługa jest uruchomiona. Można użyć **szczegóły synchronizacji** sekcji, aby monitorować postęp i skorzystaj z linków do inicjowania obsługi administracyjnej raporty działania, które opisują wszystkie akcje wykonywane przez usługę inicjowania obsługi administracyjnej na Asana aplikacji.

Aby uzyskać więcej informacji na temat usługi Azure AD, inicjowanie obsługi dzienników do odczytu, zobacz [raportowania na użytkownika automatyczne Inicjowanie obsługi konta](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zarządzanie aprowizacja konta użytkowników dla aplikacji przedsiębiorstwa](active-directory-enterprise-apps-manage-provisioning.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurowanie rejestracji jednokrotnej](active-directory-saas-asana-tutorial.md)
