---
title: "Utwórz partnerów wiadomości między firmami (B2B) - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać partnerów do swojego konta integracji z pakiet integracyjny dla przedsiębiorstw i Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 950cb449b53f400f0f0f860caf5415bbb5212269
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a>Dodaj lub zaktualizuj partnerów w umowach business-to-business w przepływie pracy

Partnerzy to podmiotom uczestniczenie w transakcjach (B2B) business-to-business i wymiany wiadomości między sobą. Przed utworzeniem partnerów, które reprezentują i innej organizacji w tych transakcji, należy najpierw zarówno udostępnianie informacji, która identyfikuje i weryfikuje komunikatów wysłanych przez siebie nawzajem. Po omówiono te szczegóły i zacząć z firmą, można utworzyć partnerów na koncie integracji do reprezentowania obie.

## <a name="what-roles-do-partners-have-in-your-integration-account"></a>Jakie role partnerów masz na koncie integracji?

Zdefiniuj szczegóły wiadomości wymieniane między partnerami, utworzysz umów tych partnerów. Jednakże przed utworzeniem umowę, należy dodać co najmniej dwie firmy partnerskie do swojego konta integracji. Twoja organizacja musi być częścią umowy jako **partnera hosta**. Drugiego partnera, lub **partnera gościa** to organizacja, który wymienia wiadomości w swojej organizacji. Partner gościa może być innej firmy lub nawet działów w organizacji.

Po dodaniu tych partnerów, można utworzyć umowy.

Odbierania i wysyłania, ustawienia są obiektowe z punktu widzenia hostowanej partnera. Na przykład odbierania ustawienia w umowie określają sposób hostowanej partnera odbiera wiadomości wysyłane z partnerem gościa. Podobnie, Wyślij ustawienia na umowie wskazują, jak hostowanej partner wysyła komunikaty do partnera gościa.

## <a name="how-to-create-a-partner"></a>Jak utworzyć partnera?

1. W portalu Azure wybierz **Przeglądaj**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. W polu filtru wyszukiwania wprowadź **integracji**, a następnie wybierz pozycję **konta integracji** na liście wyników.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Wybierz konto integracji, w której chcesz dodać partnerów.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Wybierz **partnerów** kafelka.

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. W bloku partnerów, wybierz **Dodaj**.

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Wprowadź nazwę dla partnera, a następnie wybierz **kwalifikator**. Na koniec, wprowadź **wartość** ułatwia zidentyfikowanie dokumentów, które pochodzą ze swoimi aplikacjami.

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Aby wyświetlić postęp procesu tworzenia partnera, wybierz *dzwonka* ikonę powiadomienia.

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. Aby potwierdzić pomyślnie dodano nowe partnerów, wybierz **partnerów** kafelka.

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    Po wybraniu kafelka partnerów, widoczny będzie również nowo dodanego partnerów w bloku partnerów.

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-to-edit-existing-partners-in-your-integration-account"></a>Jak edytować istniejących partnerów w ramach konta integracji

1. Wybierz **partnerów** kafelka.
2. Po otwarciu bloku partnerów, wybierz partnera, który chcesz edytować.
3. Na **partnera aktualizacji** kafelka, wprowadź zmiany.
4. Po zakończeniu wybierz **zapisać**, lub Anuluj zmiany, wybierz **odrzucić**.

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-to-delete-a-partner"></a>Jak usunąć partnera

1. Wybierz **partnerów** kafelka.
2. Po otwarciu bloku partnera, wybierz partnera, którego chcesz usunąć.
3. Wybierz **usunąć**.

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Następne kroki
* [Dowiedz się więcej na temat umów](../logic-apps/logic-apps-enterprise-integration-agreements.md "więcej informacji na temat umowy integracji dla przedsiębiorstw")  

