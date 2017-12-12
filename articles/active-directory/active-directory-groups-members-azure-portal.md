---
title: "Zarządzanie członkami grupy w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dodawanie i usuwanie użytkowników i urządzeń z grupy w usłudze Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: d399a97d-fd2a-4b2d-b73d-0975db83f41b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 49c975aa34f28c6c00591435726e538bb79a78cb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Zarządzanie członkostwami grup użytkowników w dzierżawie usługi Azure Active Directory
W tym artykule wyjaśniono, jak można zarządzać członkami grupy w usłudze Azure Active Directory (Azure AD).

## <a name="how-do-i-find-the-members-and-manage-them"></a>Jak znaleźć członków i zarządzać nimi?
1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie wybierz **Enter**.

   ![Otwieranie Zarządzanie użytkownikami](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. Na **użytkowników i grup** bloku, wybierz opcję **wszystkich grup**.

   ![Otwieranie bloku grupy](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. Na **użytkowników i grup — wszystkie grupy** bloku, wybierz grupę.
5. Na **grupy - *groupname***  bloku, wybierz opcję **członków**.

   ![Otwieranie bloku elementy członkowskie](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. Aby dodać członków do grupy, na **grupy - członków** bloku, wybierz opcję **Dodaj członków**.

   ![Dodaj polecenie elementy członkowskie](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. Na **członków** bloku, wybierz jeden lub więcej użytkowników lub urządzeń do dodania do grupy, a następnie wybierz **wybierz** przycisk w dolnej części bloku, aby dodać je do grupy. **Użytkownika** okno filtry wyświetlania na podstawie zgodności wpis do dowolnej części nazwy użytkownika lub urządzenia. Nie symbole wieloznaczne są akceptowane w tym polu.
8. Aby usunąć członków z grupy, na **grupy - członków** bloku, wybierz element członkowski.
9. Na ***membername*** bloku, wybierz opcję **Usuń** polecenie i Potwierdź wybór w wierszu.

   ![Usuń elementy członkowskie, polecenie](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Po zakończeniu zmiana członków grupy, wybierz **zapisać**.

## <a name="additional-information"></a>Dodatkowe informacje
Te artykuły zawierają dodatkowe informacje o usłudze Azure Active Directory.

* [Zobacz istniejących grup](active-directory-groups-view-azure-portal.md)
* [Utwórz nową grupę i dodawanie członków](active-directory-groups-create-azure-portal.md)
* [Zarządzanie ustawieniami grupy](active-directory-groups-settings-azure-portal.md)
* [Zarządzanie członkostwami grup](active-directory-groups-membership-azure-portal.md)
* [Dynamiczne reguły dla użytkowników w grupie zarządzania](active-directory-groups-dynamic-membership-azure-portal.md)
