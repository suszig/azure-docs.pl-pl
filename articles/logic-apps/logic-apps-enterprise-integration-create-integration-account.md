---
title: "Tworzenie, link, usunąć lub przenieść konta integracji w aplikacjach logiki platformy Azure | Dokumentacja firmy Microsoft"
description: "Jak utworzyć konto integracji i połącz go z aplikacji logiki"
services: logic-apps
documentationcenter: .net,nodejs,java
author: divyaswarnkar
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; divswa
ms.openlocfilehash: fb1d0ceb26c5ed792f22051e2af10a7572200bdc
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="what-is-an-integration-account"></a>Co to jest konto integracji?

Konta integracji, umożliwia firmie integracji aplikacji, w szczególności aplikacje logiki, aby uzyskać dostęp i zarządzać nimi artefaktów B2B, na przykład handlowymi partnerów, umów, map, schematów, certyfikaty i tak dalej. Aby zapewnić dostęp, połącz swoje konto integracji z aplikacji logiki, po upewnieniu się, że oba integracji konta i logiki aplikacji *tej samej lokalizacji platformy Azure*.

## <a name="create-an-integration-account"></a>Tworzenie konta integracji

1. Zaloguj się w witrynie [Azure Portal](http://portal.azure.com "Azure Portal"). 

2. Wybierz z menu głównego Azure **wszystkie usługi**. W polu wyszukiwania wpisz "integrację", a następnie wybierz **konta integracji**.

   ![Tworzenie konta integracji](./media/logic-apps-enterprise-integration-accounts/account-1.png)

3. W górnej części strony, wybierz **Dodaj**.

   ![Wybierz opcję Dodaj](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Nazwa konta integracji i wybierz subskrypcję platformy Azure, którego chcesz używać. Możesz utworzyć nową **grupy zasobów** lub wybierz istniejącą grupę zasobów. Wybierz **lokalizacji** do obsługi konta integracji i **warstwy cenowej**. Gdy wszystko jest gotowe, wybierz pozycję **Utwórz**.

   ![Podaj szczegóły konta integracji](./media/logic-apps-enterprise-integration-accounts/account-4.png)

   Azure udostępnia konta integracji w wybranej lokalizacji, która powinna zakończyć się w ciągu jednej minuty.

5. Odśwież stronę. Widzisz konta integracji na liście.

   ![Pojawi się nowego konta integracji](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

Następnie połącz konto integracji utworzoną aplikację logiki. 

## <a name="link-an-integration-account-to-a-logic-app"></a>Połącz konto integracji aplikacji logiki

Aby udzielić dostępu aplikacji logiki do B2B artefaktów, takich jak handlowymi partnerów, umów map i schematy na koncie integracji Połącz konto integracji aplikacji logiki. 

1. W portalu Azure wybierz aplikację logiki i sprawdź lokalizację aplikację logiki.

   ![Wybierz aplikację logiki, sprawdź lokalizację](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. W obszarze **ustawienia**, wybierz pozycję **konta integracji**.

   ![Wybierz opcję "Konto Integration"](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. Z **wybierz konto integracji** listy, wybierz konto integracji, aby połączyć aplikację logiki. Aby zakończyć połączenie, wybierz **zapisać**.

   ![Wybierz konto integracji](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

   Otrzymasz powiadomienie, które pokazuje integracją konto jest połączone z aplikacji logiki, a wszystkie artefakty na koncie integracji są teraz dostępne dla aplikacji logiki.

   ![Aplikację logiki jest połączony z kontem integracji](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Teraz, kiedy Twoje konto integracji jest połączone z aplikacji logiki, można użyć łączniki B2B w aplikacji logiki. Niektóre typowe łączniki B2B obejmują sprawdzanie poprawności kodu XML i pliku prostego kodowania/dekodowania.  

## <a name="delete-your-integration-account"></a>Usuwanie konta integracji

1. Wybierz z menu głównego Azure **wszystkie usługi**. W polu wyszukiwania wpisz "integrację", a następnie wybierz **konta integracji**.

   ![Znajdź konta integracji](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Wybierz konto integracji, które chcesz usunąć.

    ![Wybierz konto integracji do usunięcia](./media/logic-apps-enterprise-integration-accounts/account-5.png)

3. W menu, wybierz **usunąć**.

    ![Wybierz pozycję "Delete"](./media/logic-apps-enterprise-integration-accounts/delete.png)

4. Potwierdź zamiar usunięcia konta integracji.

## <a name="move-your-integration-account"></a>Przenoszenie konta integracji

Aby przenieść konta integracji innego Azure subskrypcji lub grupy zasobów, wykonaj następujące kroki. Po przeniesieniu konta integracji upewnij się, czy należy zaktualizować wszystkie skrypty do używania nowych identyfikatorów zasobów.

1. Wybierz z menu głównego Azure **wszystkie usługi**. W polu wyszukiwania wpisz "integrację", a następnie wybierz **konta integracji**.

   ![Znajdź konta integracji](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Wybierz konto integracji, które mają zostać przeniesione. W obszarze **ustawienia**, wybierz **właściwości**.

   ![Wybierz konto integracji można przenieść. W obszarze Ustawienia wybierz polecenie Właściwości](./media/logic-apps-enterprise-integration-accounts/move.png)

3. Zmiana grupy zasobów lub subskrypcji platformy Azure, który został skojarzony z Twoim kontem integracji.

   ![Wybierz opcję Zmień grupę zasobów lub zmiany subskrypcji](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Kolejne kroki

* [Dowiedz się więcej na temat umów](../logic-apps/logic-apps-enterprise-integration-agreements.md "więcej informacji na temat umowy integracji dla przedsiębiorstw")  

