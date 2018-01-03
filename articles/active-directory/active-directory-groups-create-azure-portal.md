---
title: "Utwórz grupę dla użytkowników w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Jak utworzyć grupę w usłudze Azure Active Directory i dodawanie członków do grupy"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1be9947c43c70b7248201b9f470fb3cf5a11519e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Utwórz grupy i Dodaj elementy członkowskie w usłudze Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure portal](active-directory-groups-create-azure-portal.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

W tym artykule opisano sposób tworzenia i wypełniania nową grupę w usłudze Azure Active Directory. Grupa służy do wykonywania zadań zarządzania, takich jak przypisywanie licencji lub uprawnień do liczby użytkowników lub urządzeń naraz.

## <a name="how-do-i-create-a-group"></a>Jak utworzyć grupę?
1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.
2. Wybierz **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie wybierz **Enter**.

   ![Otwieranie Zarządzanie użytkownikami](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. Na **użytkowników i grup** bloku, wybierz opcję **wszystkich grup**.

   ![Otwieranie bloku grupy](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. Na **użytkowników i grup — wszystkie grupy** bloku, wybierz opcję **Dodaj** polecenia.

   ![Dodaj polecenie](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. Na **grupy** bloku, Dodaj nazwę i opis grupy.
6. Aby wybrać elementy członkowskie, aby dodać do grupy, wybierz **przypisane** w **Typ członkostwa** , a następnie wybierz **członków**. Aby uzyskać więcej informacji o sposobie zarządzania członkostwa w grupie dynamicznie, zobacz [tworzenie zaawansowanych reguł członkostwa w grupie za pomocą atrybutów](active-directory-groups-dynamic-membership-azure-portal.md).

   ![Wybieranie członków do dodania](./media/active-directory-groups-create-azure-portal/select-members.png)
7. Na **członków** bloku, wybierz jeden lub więcej użytkowników lub urządzeń do dodania do grupy, a następnie wybierz **wybierz** przycisk w dolnej części bloku, aby dodać je do grupy. **Użytkownika** okno filtry wyświetlania na podstawie zgodności wpis do dowolnej części nazwy użytkownika lub urządzenia. Nie symbole wieloznaczne są akceptowane w tym polu.
8. Po zakończeniu dodawania członków do grupy, wybierz **Utwórz** na **grupy** bloku.    

   ![Tworzenie grupy potwierdzenia](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Kolejne kroki
Te artykuły zawierają dodatkowe informacje o usłudze Azure Active Directory.

* [Zobacz istniejących grup](active-directory-groups-view-azure-portal.md)
* [Zarządzanie ustawieniami grupy](active-directory-groups-settings-azure-portal.md)
* [Elementy członkowskie grupy zarządzania](active-directory-groups-members-azure-portal.md)
* [Zarządzanie członkostwami grup](active-directory-groups-membership-azure-portal.md)
* [Dynamiczne reguły dla użytkowników w grupie zarządzania](active-directory-groups-dynamic-membership-azure-portal.md)
