---
title: "Odbierzesz alerty dziennika działania dotyczące powiadomień dotyczących usługi | Dokumentacja firmy Microsoft"
description: "Otrzymuj powiadomienia za pomocą programu SMS, wiadomości e-mail lub elementu webhook w przypadku wystąpienia usługi Azure."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: bf6a98fd7e7e11764bef174f9efd0635fa7efe9a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Utwórz działanie alertów dziennika na powiadomień usługi
## <a name="overview"></a>Omówienie
W tym artykule pokazano, jak skonfigurować alerty dziennika działania dotyczące powiadomień o kondycji usługi za pomocą portalu Azure.  

Użytkownik otrzyma alert, gdy Azure wysyła powiadomienia o kondycji usługi do subskrypcji platformy Azure. Można skonfigurować alerty na podstawie:

- Klasa usługi kondycji powiadomienia (zdarzenia konserwacji, informacje, itp.).
- Usług, których to dotyczy.
- Regionów, których to dotyczy.
- Stan powiadomienia (aktywny lub rozwiązany).
- Poziom powiadomienia (błąd informacyjny, ostrzeżenie,).

Można także skonfigurować kogo wysyłane alertu:

- Wybierz istniejącą grupę akcji.
- Utwórz nową grupę akcję (która może służyć do następnych alertów).

Aby dowiedzieć się więcej o grupach akcji, zobacz [Utwórz i Zarządzaj grupami akcji](monitoring-action-groups.md).

Aby uzyskać informacje na temat konfigurowania alertów powiadomień kondycji usługi przy użyciu szablonów usługi Azure Resource Manager, zobacz [szablonów Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a>Utwórz alert na powiadomienia usługi kondycji dla nowej grupy akcji przy użyciu portalu Azure
1. W [portal](https://portal.azure.com), wybierz pozycję **Monitor**.

    ![Usługa "Monitora"](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. W **dziennik aktywności** zaznacz **alerty**.

    ![Karta "Alertów"](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. Wybierz **alert dziennika aktywności Dodaj**, a następnie wypełnij pola.

    ![Polecenie "Dodaj działanie dziennika alert"](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. Wprowadź nazwę w **nazwa alertu dziennika aktywności** polu i podaj **opis**.

    ![Okno dialogowe "Dodaj alert dziennika aktywności"](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. **Subskrypcji** polu autofills z Twojej bieżącej subskrypcji. Ta subskrypcja służy do zapisania alert dziennika aktywności. Zasób alertu jest wdrażana dla tej subskrypcji i monitorować zdarzenia w dzienniku aktywności.

6. Wybierz **grupy zasobów** w jest tworzony zasób alertu. Nie jest to grupa zasobów, który jest monitorowany przez alert. Zamiast tego jest grupy zasobów, w którym znajduje się zasób alertu.

7. W **kategorii zdarzeń** wybierz opcję **kondycja usługi**. Opcjonalnie wybierz **usługi**, **Region**, **typu**, **stan**, i **poziom** z powiadomień o kondycji usługi, które chcesz otrzymywać.

8. W obszarze **alertów za pośrednictwem**, wybierz pozycję **nowy** przycisku grupy akcji. Wprowadź nazwę w **nazwy grupy akcji** i wprowadzić nazwę w **krótką nazwę** pole. Krótka nazwa odwołuje się do powiadomień wysyłanych po zgłoszeniu tego alertu.

9. Zdefiniować listę odbiornikami, zapewniając odbiorcy:

    a. **Nazwa**: Wprowadź nazwę odbiorcy, aliasu lub identyfikator.

    b. **Typ akcji**: Wybierz SMS, wiadomości e-mail lub elementu webhook.

    c. **Szczegóły**: oparte na wybrany typ akcji, wprowadź numer telefonu, adres e-mail lub identyfikator URI elementu webhook.

10. Wybierz **OK** można utworzyć alertu.

W ciągu kilku minut alert jest aktywny i rozpoczyna się do wyzwalania na podstawie wybranych warunków, które zostały określone podczas tworzenia.

Uzyskać informacji o schemacie elementu webhook dla alertów dotyczących działań w dzienniku, zobacz [elementów Webhook dla działania Azure rejestrowania alertów](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Do ponownego użycia istniejącej grupy akcji dla wszystkich przyszłych definicji alertu jest grupa akcji zdefiniowane w tych krokach.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a>Utwórz alert na powiadomienia usługi kondycji dla istniejącej grupy akcji przy użyciu portalu Azure

1. Wykonaj kroki od 1 do 7 w poprzedniej sekcji, aby utworzyć powiadomienia usługi kondycji. 

2. W obszarze **alertów za pośrednictwem**, wybierz pozycję **istniejące** przycisku grupy akcji. Wybierz grupę odpowiednią akcję.

3. Wybierz **OK** można utworzyć alertu.

W ciągu kilku minut alert jest aktywny i rozpoczyna się do wyzwalania na podstawie wybranych warunków, które zostały określone podczas tworzenia.

## <a name="manage-your-alerts"></a>Zarządzanie alertami

Po utworzeniu alertu nie jest widoczna **alerty** sekcji **Monitor** bloku. Wybierz alert, który ma być zarządzany do:

* Czy chcesz go edytować.
* Usuń go.
* Wyłącz lub włącz go, jeśli chcesz tymczasowo zatrzymać lub wznowić odbieranie powiadomień o alercie.

## <a name="next-steps"></a>Następne kroki
- Dowiedz się więcej o [usługi powiadomień o kondycji](monitoring-service-notifications.md).
- Dowiedz się więcej o [limitów szybkości powiadomień](monitoring-alerts-rate-limiting.md).
- Przegląd [schemat alertu elementu webhook dziennika aktywności](monitoring-activity-log-alerts-webhook.md).
- Pobierz [Przegląd alertów dotyczących działań w dzienniku](monitoring-overview-alerts.md)i dowiedzieć się, jak otrzymywać alerty. 
- Dowiedz się więcej o [grupy akcji](monitoring-action-groups.md).
