---
title: "Samouczek: Konfigurowanie Pingboard dla użytkownika automatycznego inicjowania obsługi administracyjnej z usługą Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować usługi Azure Active Directory, aby automatycznie zapewnianie i usuwanie kont użytkowników do Pingboard."
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
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 0492a59852a6a2dd424c98e44969eab1ac4697e8
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="tutorial-configuring-pingboard-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie Pingboard dla użytkownika automatycznego inicjowania obsługi administracyjnej.

Celem tego samouczka jest opisano czynności, które należy wykonać, aby włączyć automatyczną aprowizację i anulowanie obsługi kont użytkowników z usługi Azure Active Directory do Pingboard.

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że już następujące elementy:

*   Dzierżawy usługi Azure Active Directory
*   Dzierżawy Pingboard [Pro konta](https://pingboard.com/pricing) 
*   Konto użytkownika z uprawnieniami administratora w Pingboard 

> [!NOTE] 
> Azure Active Directory inicjowania obsługi administracyjnej integracji zależy od interfejsu API Pingboard (`https://your_domain.pingboard.com/scim/v2`) który jest dostępny do Twojego konta.

## <a name="assigning-users-to-pingboard"></a>Przypisywanie użytkowników do Pingboard

Usługa Azure Active Directory korzysta pojęcie o nazwie "przypisania", aby określić, użytkowników, którzy powinien otrzymać dostęp do wybranej aplikacji. W kontekście użytkownika automatyczne Inicjowanie obsługi konta użytkowników przypisane "" do aplikacji w usłudze Azure Active Directory są synchronizowane. 

Przed Skonfiguruj i włącz usługę inicjowania obsługi administracyjnej, należy zdecydować, co użytkownicy w usłudze Azure Active Directory reprezentują użytkowników, którzy potrzebują dostępu do aplikacji Pingboard. Po decyzję, postępując zgodnie z instrukcjami w tym miejscu można przypisać tych użytkowników do aplikacji Pingboard:

[Przypisz użytkownika do aplikacji w przedsiębiorstwie](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Ważne porady dotyczące przypisywania użytkowników do Pingboard

*   Zaleca się przypisanie pojedynczego użytkownika usługi Azure Active Directory do Pingboard do testowania konfiguracji inicjowania obsługi administracyjnej. Później można przypisać dodatkowych użytkowników.

## <a name="configuring-user-provisioning-to-pingboard"></a>Konfigurowanie inicjowania obsługi administracyjnej Pingboard użytkownika 

Ta sekcja przeprowadzi Cię przez łączenie usługi Azure Active Directory do konta użytkownika Pingboard inicjowania obsługi interfejsu API i konfigurowanie inicjowania obsługi usługi do tworzenia, aktualizacji i wyłączania konta użytkowników przypisane w Pingboard oparte na przypisanie użytkownika w usłudze Azure Usługi Active Directory.

> [!TIP]
> Można też włączyć na języku SAML logowania jednokrotnego dla Pingboard, wykonując instrukcje podane w [portalu Azure](https://portal.azure.com). Logowanie jednokrotne można skonfigurować niezależnie od automatycznego inicjowania obsługi administracyjnej, chociaż te dwie funkcje uzupełniania siebie nawzajem.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-active-directory"></a>Aby skonfigurować automatyczne użytkownika konta inicjowania obsługi administracyjnej Pingboard w usłudze Azure Active Directory:

1)  W [portalu Azure](https://portal.azure.com), przejdź do **usługi Azure Active Directory > aplikacje przedsiębiorstwa > wszystkie aplikacje** sekcji.

2) Jeśli Pingboard został już skonfigurowany dla logowania jednokrotnego, wyszukiwanie wystąpieniem Pingboard przy użyciu pola wyszukiwania. W przeciwnym razie wybierz **Dodaj** i wyszukaj **Pingboard** w galerii aplikacji. Wybierz **Pingboard** z wyników wyszukiwania i dodaj go do listy aplikacji.

3)  Wybierz wystąpienia programu Pingboard, a następnie wybierz **inicjowania obsługi administracyjnej** kartę.

4)  Ustaw **tryb obsługi administracyjnej** do **automatyczne**.

![Pingboard inicjowania obsługi administracyjnej.](./media/active-directory-saas-pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
5) W sekcji poświadczenia administratora wykonaj następujące czynności:

* W **adres URL dzierżawy** pole tekstowe, wprowadź `https://your_domain.pingboard.com/scim/v2` i Zastąp format domena_użytkownika rzeczywistych domeny
* Zaloguj się do [Pingboard](https://pingboard.com/) przy użyciu konta administratora.
* Polecenie Dodatki > integracji > Azure Active Directory.
* Kliknij kartę Konfiguracja i wybierz polecenie **Włącz inicjowanie obsługi użytkowników z usługi Azure**.
* Kopiuj **tokenu elementu nośnego OAuth** pola, a następnie wprowadź do **klucz tajny tokenu** pola tekstowego.

6) W portalu Azure kliknij **Testuj połączenie** zapewniające usługi Azure Active Directory można połączyć się Pingboard aplikacji. Jeśli połączenie nie powiedzie się, upewnij się, Twoje konto Pingboard ma uprawnienia administratora i spróbuj **"Test połączenia"** krok ponownie.

7) Wprowadź adres e-mail osoby lub grupy, który powinien zostać wyświetlony inicjowania obsługi administracyjnej powiadomienia o błędach w **wiadomość E-mail z powiadomieniem** pola i sprawdź następujące pole wyboru.

8) Kliknij pozycję **Zapisz**. 

9) W sekcji mapowania wybierz **synchronizacji Azure Active Directory użytkownikom Pingboard**.

10) W **mapowań atrybutów** Przejrzyj atrybuty użytkownika mają być synchronizowane z usługi Azure Active Directory do Pingboard. Atrybuty wybrany jako **pasujące** właściwości są używane do dopasowania kont użytkowników w Pingboard dla operacji update. Wybierz **zapisać** przycisk, aby zatwierdzić zmiany. Aby uzyskać więcej informacji, zobacz [Dostosowywanie użytkownika inicjowania obsługi administracyjnej mapowań atrybutów](active-directory-saas-customizing-attribute-mappings.md).

11) Aby włączyć usługi Azure Active Directory, usługi dla Pingboard inicjowania obsługi administracyjnej, zmień **stan inicjowania obsługi administracyjnej** do **na** w **ustawienia** sekcji

12) Kliknij przycisk **zapisać** można rozpocząć synchronizacji początkowej z użytkowników przypisanych do Pingboard.

Początkowy zajmuje więcej czasu wykonywania niż kolejne synchronizacje, występujące co około 20 minut, tak długo, jak usługa jest uruchomiona. Można użyć **szczegóły synchronizacji** sekcji, aby monitorować postęp i skorzystaj z linków do inicjowania obsługi administracyjnej raporty działania, które opisują wszystkie akcje wykonywane przez usługę inicjowania obsługi administracyjnej na Pingboard aplikacji.

Aby uzyskać więcej informacji na temat usługi Azure Active Directory inicjowania obsługi dzienników do odczytu, zobacz [raportowania na użytkownika automatyczne Inicjowanie obsługi konta](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zarządzanie aprowizacja konta użytkowników dla aplikacji przedsiębiorstwa](active-directory-enterprise-apps-manage-provisioning.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurowanie rejestracji jednokrotnej](active-directory-saas-pingboard-tutorial.md)
