---
title: "Inspekcja i otrzymywanie powiadomień o ważne akcje w ramach subskrypcji platformy Azure | Dokumentacja firmy Microsoft"
description: "Zrozumienie historii zarządzania zasobami, usługa kondycji i innych działań subskrypcji w dzienniku aktywności, a następnie użyj alert dziennik aktywności otrzymywać wiadomość e-mail z powiadomieniem po wykonaniu operacji uprawnieniach w ramach subskrypcji."
author: johnkemnetz
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: johnkem
ms.custom: mvc
ms.openlocfilehash: 636dc0fcae1bc2647cd59add5957884971015ce2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="audit-and-receive-notifications-about-important-actions-in-your-azure-subscription"></a>Inspekcja i otrzymywanie powiadomień o ważne akcje w ramach subskrypcji platformy Azure

**Dziennika aktywności platformy Azure** zawiera historię zdarzenia na poziomie subskrypcji platformy Azure. Zapewnia informacje o *kto* utworzone, zaktualizować lub usunąć *co* zasobów i *podczas* ich działanie. Można utworzyć **alert dziennik aktywności** do odbierania wiadomości e-mail, SMS lub elementu webhook powiadomienia, gdy występuje działania, która odpowiada warunki alertu. Ta procedura szybkiego startu poprzez tworzenie prostego sieciowej grupy zabezpieczeń, przeglądanie dziennika aktywności, aby zrozumieć zdarzeń, który wystąpił i następnie tworzenia alert dziennik aktywności, aby stać się powiadomienie, gdy wszystkie grupy zabezpieczeń sieci jest tworzony przekazuje będzie.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-a-network-security-group"></a>Utwórz grupę zabezpieczeń sieci

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

2. Wybierz **sieci**, wybierz pozycję **sieciowej grupy zabezpieczeń**.

3. Wprowadź "myNetworkSG" jako **nazwa** i utworzyć nową grupę zasobów o nazwie **myResourceGroup**. Kliknij przycisk **Utwórz**.

    ![Utwórz grupę zabezpieczeń sieci w portalu](./media/monitor-quick-audit-notify-action-in-subscription/create-network-security-group.png)

## <a name="browse-the-activity-log-in-the-portal"></a>Przeglądanie dziennika aktywności w portalu

Teraz dodano zdarzenie do dziennika aktywności opisujący Tworzenie grupy zabezpieczeń sieci. Użyj poniższych instrukcji, aby zidentyfikować tego zdarzenia.

1. Kliknij przycisk **Monitor** znaleziono na liście nawigacji po lewej stronie przycisku. Zostanie otwarta sekcja dziennik aktywności. Ta sekcja zawiera historię wszystkie akcje, które użytkownicy wykonali zasobów w ramach subskrypcji, można filtrować według właściwości kilka takich jak **grupy zasobów**, **Timespan**, i  **Kategoria**.

2. W **dziennik aktywności** kliknij **grupy zasobów** listy rozwijanej i wybierz **myResourceGroup**. Zmień **Timespan** listy rozwijanej, aby **ostatni 1 godzina**. Kliknij przycisk **Zastosuj**.

    ![Filtrowanie dziennika aktywności](./media/monitor-quick-audit-notify-action-in-subscription/browse-activity-log.png)

3. Polecenie **zapisu Networksecuritygroup** zdarzeń w tabeli przedstawianych zdarzeń.

## <a name="browse-an-event-in-the-activity-log"></a>Przeglądaj zdarzenia w dzienniku aktywności

Sekcja, która pojawia się zawiera podstawowe informacje na temat operacji, która została wykonana, w tym nazwę, timestamp, użytkownika lub aplikacji, która go wykonać.

![Wyświetl zdarzenie w dzienniku aktywności podsumowania](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-summary.png)

Polecenie **JSON** kartę, aby wyświetlić szczegóły zdarzenia pełna. Obejmuje to szczegóły jak użytkownik lub aplikacja został autoryzowany do wykonania operacji, kategorię zdarzenia i poziom oraz stan operacji.

![Wyświetl szczegóły zdarzenia w dzienniku aktywności](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-json.png)

## <a name="create-an-activity-log-alert"></a>Utwórz alert dziennik aktywności

1. Polecenie **podsumowania** kartę, aby powrócić do podsumowania zdarzeń.

2. W sekcji podsumowania jest wyświetlany, kliknij przycisk **alert dziennika aktywności Dodaj**.

    ![Utwórz grupę zabezpieczeń sieci w portalu](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-summary.png)

3. W sekcji nadaj alert dziennik aktywności, nazwę i opis.

4. W obszarze **kryteria** upewnij się, że **kategorii zdarzeń** ustawiono **administracyjne**, **typu zasobu** ustawiono **sieci grupy zabezpieczeń**, **nazwy operacji** ustawiono **Tworzenie lub aktualizacja sieciowej grupy zabezpieczeń**, **stan** ma ustawioną wartość  **Pomyślnie** i wszystkich innych pól kryteriów są puste lub ustawić do **wszystkich**. Kryteria zdefiniować reguły używane do określenia, czy alert powinien aktywowany, gdy jest to nowe zdarzenie zostanie wyświetlony w dzienniku aktywności.

    ![Utwórz grupę zabezpieczeń sieci w portalu](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-alert-criteria.png)

5. W obszarze **alertów za pośrednictwem** wybierz **nowy** akcji grupy i podaj **nazwa** i **krótką nazwę** grupy działań. Grupa działań określa zbiór akcje wykonywane, gdy alert jest aktywny (gdy kryteria zgodne nowe zdarzenie).

6. W obszarze **akcje** Dodaj 1 lub więcej akcji, zapewniając **nazwa** dla akcji, **typ akcji** (na przykład wiadomości e-mail lub SMS) i **szczegóły**dla tego typu określonej akcji (na przykład adres URL elementu webhook, adres e-mail lub numer SMS).

    ![Utwórz grupę zabezpieczeń sieci w portalu](./media/monitor-quick-audit-notify-action-in-subscription/activity-log-alert-actions.png)

7. Kliknij przycisk **Ok** można zapisać alertu dziennik aktywności.

## <a name="test-the-activity-log-alert"></a>Testowanie alertu dziennik aktywności

> [!NOTE]
> Trwa około 10 minut alert dziennik aktywności, aby stać się w pełni włączone. Nowe zdarzenia, które wystąpić, zanim alert dziennik aktywności jest całkowicie włączone nie generują powiadomienia.
>
>

Aby przetestować alertu, powtórz poprzedniej sekcji, aby **Utwórz grupę zabezpieczeń sieci**, ale mieć inną nazwę tej grupy zabezpieczeń sieci i ponowne użycie istniejącej grupy zasobów. W ciągu kilku minut otrzymasz powiadomienie, że została utworzona grupa zabezpieczeń sieci.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, Usuń grupę zasobów i grupy zabezpieczeń sieci. Aby to zrobić, wpisz nazwę grupy zasobów utworzonej w polu wyszukiwania w górnej części portalu, a następnie kliknij nazwę grupy zasobów. W sekcji, która jest wyświetlana, kliknij przycisk **Usuń grupę zasobów** przycisku, wpisz nazwę grupy zasobów, a następnie kliknij przycisk **usunąć**.

## <a name="next-steps"></a>Następne kroki

W tym szybki start wykonana operacja generują zdarzenie dziennika aktywności, a następnie tworzone alert dziennik aktywności, aby stać się powiadomienie, gdy ta operacja odbywa się ponownie w przyszłości. Następnie alert został przetestowany przez wykonanie tej operacji ponownie. Azure sprawia, że dostępne zdarzenia dziennika aktywności w ciągu ostatnich 90 dni. Jeśli chcesz zachować zdarzeń dłużej niż 90 dni, spróbuj archiwizowanie danych dziennika aktywności obok innych danych monitorowania.

> [!div class="nextstepaction"]
> [Archiwizowanie danych monitorowania](./monitor-tutorial-archive-monitoring-data.md)
