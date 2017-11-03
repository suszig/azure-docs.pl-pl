---
title: "Tworzenie, link, usunąć lub przenieść konta integracji w aplikacjach logiki platformy Azure | Dokumentacja firmy Microsoft"
description: "Jak utworzyć konto integracji i połącz go z aplikacji logiki"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 716e7b5bab8725dea0fd2b760d0e46e8e892c5b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-an-integration-account"></a>Co to jest konto integracji?

Konta integracji umożliwia enterprise integracji aplikacji do zarządzania artefaktów, w tym schematów, map, certyfikaty, partnerzy i umów. Dowolną aplikację integracji, tworzonych przy użyciu konta integracji dostęp do tych schematów, map, certyfikaty i itd.

## <a name="create-an-integration-account"></a>Tworzenie konta usługi integracji

1.  Zaloguj się w witrynie [Azure Portal](http://portal.azure.com "Azure Portal"). Wybierz z menu po lewej stronie **więcej usług**.

    ![Wybierz pozycję "Więcej usług"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. W polu wyszukiwania wpisz "Integracja" filtru. Na liście wyników wybierz **konta integracji**.

    ![Odfiltruj "integrację", wybierz "Konta integracji"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. W górnej części strony, wybierz **Dodaj**.

    ![Wybierz opcję Dodaj](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Nazwa konta integracji i wybierz subskrypcję platformy Azure, którego chcesz używać. Możesz utworzyć nową **grupy zasobów** lub wybierz istniejącą grupę zasobów. Następnie wybierz **lokalizacji** dla Twojego konta integracji hostingu i **warstwy cenowej**. 

    Gdy wszystko jest gotowe, wybierz pozycję **Utwórz**.

    ![Podaj szczegóły konta integracji](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    Azure udostępnia konta integracji w wybranej lokalizacji, która powinna zakończyć się w ciągu 1 minuty.

5. Odśwież stronę. Widzisz konta integracji na liście.

    ![Pojawi się nowego konta integracji](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

Następnie połącz konto integracji utworzoną aplikację logiki. 

## <a name="link-an-integration-account-to-a-logic-app"></a>Połącz konto integracji aplikacji logiki

Aby zapewnić logiki aplikacji dostęp do map, schematów, umów oraz pozostałych artefaktów na koncie integracji Połącz konto integracji aplikacji logiki.

### <a name="prerequisites"></a>Wymagania wstępne

* Konta integracji
* Aplikacji logiki

> [!NOTE] 
> Upewnij się, że integracja aplikacji logiki i konta znajdują się w *tej samej lokalizacji platformy Azure* przed rozpoczęciem.


1. W portalu Azure wybierz aplikację logiki i sprawdź lokalizację aplikację logiki.

    ![Wybierz aplikację logiki, sprawdź lokalizację](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. W obszarze **ustawienia**, wybierz pozycję **konta integracji**.

    ![Wybierz opcję "Konto Integration"](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. Z **wybierz konto integracji** listy, wybierz konto integracji, aby połączyć aplikację logiki. Aby zakończyć połączenie, wybierz **zapisać**.

    ![Wybierz konto integracji](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    Otrzymasz powiadomienie, które pokazuje integracją konto jest połączone z aplikacji logiki, a wszystkie artefakty na koncie integracji są teraz dostępne dla aplikacji logiki.

    ![Aplikację logiki jest połączony z kontem integracji](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Teraz, kiedy Twoje konto integracji jest połączone z aplikacji logiki, można użyć łączniki B2B w aplikacjach logiki. Niektóre typowe łączniki B2B obejmują sprawdzanie poprawności kodu XML i pliku prostego kodowania/dekodowania.  

## <a name="delete-your-integration-account"></a>Usuwanie konta integracji

1. Wybierz **więcej usług**.

    ![Wybierz pozycję "Więcej usług"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. W polu wyszukiwania wpisz "Integracja" filtru. Na liście wyników wybierz **konta integracji**.

    ![Odfiltruj "integrację", wybierz "Konta integracji"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. Wybierz konto integracji, które chcesz usunąć.

    ![Wybierz konto integracji do usunięcia](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. W menu, wybierz **usunąć**.

    ![Wybierz pozycję "Delete"](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. Potwierdź zamiar usunięcia konta integracji.

## <a name="move-your-integration-account"></a>Przenoszenie konta integracji

Aby przenieść konta integracji innego Azure subskrypcji lub grupy zasobów, wykonaj następujące kroki.

> [!IMPORTANT]
> Należy zaktualizować wszystkie skrypty do używania nowych identyfikatorów zasobów. po przeniesieniu konta integracji.

1. Wybierz **więcej usług**.

    ![Wybierz pozycję "Więcej usług"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. W polu wyszukiwania wpisz "Integracja" filtru. Na liście wyników wybierz **konta integracji**.

    ![Odfiltruj "integrację", wybierz "Konta integracji"](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. Wybierz konto integracji, które mają zostać przeniesione. W obszarze **ustawienia**, wybierz **właściwości**.

    ![Wybierz konto integracji można przenieść. W obszarze Ustawienia wybierz polecenie Właściwości](./media/logic-apps-enterprise-integration-accounts/move.png)

5. Zmiana grupy zasobów lub subskrypcji platformy Azure, który został skojarzony z Twoim kontem integracji.

    ![Wybierz opcję Zmień grupę zasobów lub zmiany subskrypcji](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Następne kroki
* [Dowiedz się więcej na temat umów](../logic-apps/logic-apps-enterprise-integration-agreements.md "więcej informacji na temat umowy integracji dla przedsiębiorstw")  

