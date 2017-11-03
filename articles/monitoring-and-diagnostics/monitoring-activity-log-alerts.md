---
title: "Utwórz działanie alertów dziennika | Dokumentacja firmy Microsoft"
description: "Otrzymasz powiadomienie w wiadomości SMS, webhook i wiadomości e-mail po wystąpieniu określonego zdarzenia w dzienniku aktywności."
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
ms.date: 08/03/2017
ms.author: johnkem
ms.openlocfilehash: 3885469ec0e1fcc31386dd0ad7fe6cb5d03ab28e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-activity-log-alerts"></a>Utwórz działanie alertów dziennika

## <a name="overview"></a>Omówienie
Alerty dziennika aktywności są alerty, które aktywować po wystąpieniu nowe zdarzenie dziennika aktywności, która spełnia warunki określone w alercie. Są one zasobów platformy Azure, dlatego mogą być tworzone za pomocą szablonu usługi Azure Resource Manager. One również może być utworzone, zaktualizowane lub usunięte w portalu Azure. W tym artykule przedstawiono pojęć dotyczących alertów dotyczących działań w dzienniku. Go następnie przedstawiono sposób ustawiania alertu na zdarzenia dziennika aktywności za pomocą portalu Azure.

Zazwyczaj w celu utworzenia działania alerty dziennika, aby otrzymywać powiadomienia po:

* Określone zmiany zostaną wprowadzone do zasobów w Twojej subskrypcji platformy Azure, często są ograniczone do określonych grupach zasobów lub zasobów. Na przykład można otrzymać powiadomienie, gdy wszystkie maszyny wirtualne w myProductionResourceGroup zostanie usunięty. Możesz też być powiadamiani, jeśli wszystkie nowe role są przypisane do użytkownika w ramach subskrypcji.
* Usługa kondycji zdarzenie. Usługa kondycji zdarzenia zawierają zgłaszania zdarzeń i zdarzenia konserwacji, które są stosowane do zasobów w ramach subskrypcji.

W obu przypadkach alert dziennika aktywności monitoruje tylko w przypadku zdarzeń w ramach subskrypcji, w której jest tworzony alert.

Możesz skonfigurować alert dziennika działania na podstawie żadnych najwyższego poziomu właściwości w obiekcie JSON dla działania dziennika zdarzeń. Jednak portalu przedstawia najbardziej typowe opcje:

- **Kategoria**: administracyjne, usługa kondycji, skalowania automatycznego i zalecenia. Aby uzyskać więcej informacji, zobacz [omówienie dziennik aktywności platformy Azure](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Aby dowiedzieć się więcej na temat zdarzeń usługi kondycji, zobacz [odbierzesz alerty dziennika działania dotyczące powiadomień dotyczących usługi](./monitoring-activity-log-alerts-on-service-notifications.md).
- **Grupa zasobów**
- **Zasób**
- **Typ zasobu**
- **Nazwa operacji**: Nazwa operacji The Resource Manager Role-Based kontroli dostępu.
- **Poziom**: poziom ważności zdarzenia (pełne, informacyjny, ostrzeżenie, błąd lub krytyczny).
- **Stan**: stan zdarzeń, zwykle uruchomiona, nie powiodło się lub zakończyło się pomyślnie.
- **Zdarzenie inicjowane przez**: nazywane również "wywołującego." Adres e-mail lub identyfikator użytkownika, który wykonał działanie usługi Azure Active Directory.

>[!NOTE]
>Należy określić co najmniej dwa powyższe kryteria w alertu o jeden kategorii. Nie można utworzyć alertu, który uaktywnia za każdym razem, gdy zdarzenie jest tworzone w dziennikach działania.
>
>

Po aktywowaniu alert dziennika aktywności używa grupy akcji do wygenerowania akcje lub powiadomienia. Grupy akcji jest zestawem wielokrotnego użytku odbiorców powiadomień, takie jak adresy e-mail, numerów telefonów adresy URL elementu webhook lub wiadomości SMS. Odbiorcy mogą być przywoływane z wielu alertów scentralizować i grupach kanałów powiadomień. Podczas definiowania alertu dziennika aktywności, dostępne są dwie opcje. Możesz:

* Użyj istniejącej grupy działań w alertu dziennika aktywności. 
* Utwórz nową grupę akcji. 

Aby dowiedzieć się więcej o grupach akcji, zobacz [tworzenia grup działań w portalu Azure i zarządzanie nimi](monitoring-action-groups.md).

Aby dowiedzieć się więcej na temat powiadomień o kondycji usługi, zobacz [odbierzesz alerty dziennika działania dotyczące powiadomień o kondycji usługi](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-the-azure-portal"></a>Utwórz alert na zdarzenie dziennika działania z nową grupą akcji przy użyciu portalu Azure
1. W [portal](https://portal.azure.com), wybierz pozycję **Monitor**.

    ![Usługa "Monitora"](./media/monitoring-activity-log-alerts/home-monitor.png)
2. W **dziennik aktywności** zaznacz **alerty**.

    ![Karta "Alertów"](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. Wybierz **alert dziennika aktywności Dodaj**, a następnie wypełnij pola.

4. Wprowadź nazwę w **nazwa alertu dziennika aktywności** i wybierz **opis**.

    ![Polecenie "Dodaj działanie dziennika alert"](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. **Subskrypcji** polu autofills z Twojej bieżącej subskrypcji. Ta subskrypcja jest jeden, w którym jest zapisany grupy akcji. Zasób alertu jest wdrażana dla tej subskrypcji i monitoruje zdarzenia dziennika aktywności od niego.

    ![Okno dialogowe "Dodaj alert dziennika aktywności"](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. Wybierz **grupy zasobów** w jest tworzony zasób alertu. Nie jest to grupa zasobów, który jest monitorowany przez alert. Zamiast tego jest grupy zasobów, w którym znajduje się zasób alertu.

7. Opcjonalnie wybierz **kategorii zdarzeń** do modyfikowania dodatkowe filtry, które są wyświetlane. Zdarzenia administracyjne obejmują filtry **grupy zasobów**, **zasobów**, **typu zasobu**, **nazwy operacji**, **poziom**, **stan**, i **zdarzenie inicjowane przez**. Identyfikuje te zdarzenia, które należy monitorować ten alert.

    >[!NOTE]
    >Należy określić co najmniej jednego z poprzednich kryteriów w alertu. Nie można utworzyć alertu, który uaktywnia za każdym razem, gdy zdarzenie jest tworzone w dziennikach działania.
    >
    >

8. Wprowadź nazwę w **nazwy grupy akcji** i wprowadzić nazwę w **krótką nazwę** pole. Krótka nazwa jest używana zamiast akcji Pełna nazwa grupy, podczas powiadomienia są wysyłane przy użyciu tej grupy.

9.  Zdefiniuj listę działań, zapewniając akcji:

    a. **Nazwa**: Wprowadź nazwę, aliasu lub identyfikator akcji.

    b. **Typ akcji**: Wybierz SMS, wiadomości e-mail lub elementu webhook.

    c. **Szczegóły**: oparte na typ akcji, wprowadź numer telefonu, adres e-mail lub identyfikator URI elementu webhook.

10. Wybierz **OK** można utworzyć alertu.

Alert zajmuje kilka minut w pełni propagacji i stanie się aktywne. Uruchamia go, gdy nowe zdarzenia spełniających kryteria alertu.

Aby uzyskać więcej informacji, zobacz [zrozumieć schematu elementu webhook używane w alertach dziennika aktywności](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Do ponownego użycia istniejącej grupy akcji dla wszystkich przyszłych definicji alertu jest grupa akcji zdefiniowane w tych krokach.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-the-azure-portal"></a>Utwórz alert na zdarzenie dziennika aktywności dla istniejącej grupy akcji przy użyciu portalu Azure
1. Wykonaj kroki od 1 do 7 w poprzedniej sekcji, aby utworzyć alert dziennika aktywności.

2. W obszarze **powiadomienia za pośrednictwem**, wybierz pozycję **istniejące** przycisku grupy akcji. Wybierz istniejącą grupę akcji z listy.

3. Wybierz **OK** można utworzyć alertu.

Alert zajmuje kilka minut w pełni propagacji i stanie się aktywne. Uruchamia go, gdy nowe zdarzenia spełniających kryteria alertu.

## <a name="manage-your-alerts"></a>Zarządzanie alertami

Po utworzeniu alertu jest widoczna w sekcji Alerty bloku monitora. Wybierz alert, który ma być zarządzany do:

* Czy chcesz go edytować.
* Usuń go.
* Wyłącz lub włącz go, jeśli chcesz tymczasowo zatrzymać lub wznowić odbieranie powiadomień o alercie.

## <a name="next-steps"></a>Następne kroki
- Pobierz [Przegląd alertów](monitoring-overview-alerts.md).
- Dowiedz się więcej o [limitów szybkości powiadomień](monitoring-alerts-rate-limiting.md).
- Przegląd [schemat alertu elementu webhook dziennika aktywności](monitoring-activity-log-alerts-webhook.md).
- Dowiedz się więcej o [grupy akcji](monitoring-action-groups.md).  
- Dowiedz się więcej o [usługi powiadomień o kondycji](monitoring-service-notifications.md).
- Utwórz [alert dziennika aktywności monitorować wszystkie operacje aparat skalowania automatycznego na subskrypcję](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Utwórz [alert dziennika aktywności monitorować wszystkie operacje w/skali skalowalnych w poziomie skalowania automatycznego nie powiodło się w ramach subskrypcji](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).
