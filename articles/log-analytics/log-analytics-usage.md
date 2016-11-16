---
title: "Analizowanie użycia danych w usłudze Log Analytics | Microsoft Docs"
description: "Za pomocą strony Użycie w usłudze Log Analytics możesz wyświetlić informacje o tym, ile danych jest wysyłanych do usługi OMS."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/11/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 4c1d0319e768eec69718d8b49db30069ec1331d9


---
# <a name="analyze-data-usage-in-log-analytics"></a>Analizowanie użycia danych w usłudze Log Analytics
Usługa Log Analytics w pakiecie Operations Management Suite (OMS) gromadzi dane i okresowo wysyła je do usługi OMS.  Za pomocą strony **Użycie** możesz wyświetlić informacje o tym, ile danych jest wysyłanych do usługi OMS. Na stronie **Użycie** są również wyświetlane informacje o tym, ile danych jest wysyłanych dziennie przez rozwiązania i jak często Twoje serwery wysyłają dane.

> [!NOTE]
> Jeśli masz bezpłatne konto utworzone za pomocą [witryny internetowej usługi OMS](http://www.microsoft.com/oms), dziennie możesz wysłać do usługi OMS maksymalnie 500 MB danych. Jeśli przekroczysz dzienny limit, analiza danych zostanie zatrzymana, a następnie zostanie wznowiona na początku następnego dnia. Ponadto konieczne będzie ponowne przesłanie wszystkich danych, które nie zostały przyjęte lub przetworzone przez usługę OMS.
> 
> 

Możesz wyświetlić swoje użycie za pomocą kafelka **Użycie** na pulpicie nawigacyjnym **Przegląd** w usłudze OMS.

![kafelek Użycie](./media/log-analytics-usage/usage-tile.png)

W przypadku przekroczenia dziennego limitu użycia lub zbliżenia się do niego możesz opcjonalnie usunąć rozwiązanie, aby ograniczyć ilość danych przesyłanych do usługi OMS. Aby uzyskać więcej informacji o usuwaniu rozwiązań, zobacz [Dodawanie rozwiązań usługi Log Analytics z galerii rozwiązań](log-analytics-add-solutions.md).

![pulpit nawigacyjny Użycie](./media/log-analytics-usage/usage-dashboard.png)

Na stronie **Użycie** są wyświetlane następujące informacje:

* Średnie użycie na dzień
* Użycie danych dla poszczególnych rozwiązań w ciągu ostatnich 30 dni
* Ile danych przesłały serwery w Twoim środowisku do usługi OMS w ciągu ostatnich 30 dni
* Warstwa cenowa i szacowany koszt Twojego planu danych
* Informacje o Twojej umowie dotyczącej poziomu usług (SLA), w tym informacje o tym, ile czasu potrzebuje usługa OMS na przetworzenie Twoich danych

## <a name="to-work-with-usage-data"></a>Aby pracować z danymi użycia
1. Na stronie **Przegląd** kliknij kafelek **Użycie**.
2. Na stronie **Użycie** przejrzyj kategorie użycia zawierające interesujące Cię obszary.
3. Jeśli masz rozwiązanie, która zużywa zbyt dużo dziennego przydziału przekazywania, możesz rozważyć usunięcie tego rozwiązania.

## <a name="to-view-your-estimated-cost-and-billing-information"></a>Aby wyświetlić szacowany koszt i informacje rozliczeniowe
1. Na stronie **Przegląd** kliknij kafelek **Użycie**.
2. Na stronie **Użycie** w obszarze **Użycie** kliknij cudzysłów ostrokątny (**>**) obok pozycji **Szacowany koszt**.
3. W rozwiniętym okienku szczegółów **Twój plan danych** możesz sprawdzić szacowany koszt miesięczny.  
    ![Okienko szczegółów Twój plan danych](./media/log-analytics-usage/usage-data-plan.png)
4. Aby wyświetlić informacje rozliczeniowe, kliknij pozycję **Wyświetl mój rachunek**. Spowoduje to wyświetlenie informacji o Twojej subskrypcji.
   * Na stronie Subskrypcje kliknij swoją subskrypcję, aby wyświetlić szczegóły użycia i listę wierszy z jego pozycjami.  
       ![subskrypcja](./media/log-analytics-usage/usage-sub01.png)
   * Na stronie Podsumowanie dla Twojej subskrypcji możesz wykonywać różne zadania związane z zarządzaniem i wyświetlaniem dodatkowych szczegółów dotyczących Twojej subskrypcji.  
       ![szczegóły subskrypcji](./media/log-analytics-usage/usage-sub02.png)

## <a name="to-view-data-batches-for-your-sla"></a>Aby wyświetlić partie danych dla Twojej umowy dotyczącej poziomu usług (SLA)
1. Na stronie **Przegląd** kliknij kafelek **Użycie**.
2. W obszarze **Umowa SLA** kliknij pozycję **Pobierz szczegóły umowy SLA**.
3. Zostanie pobrany plik XLSX programu Excel, który możesz przejrzeć.  
    ![Szczegóły umowy SLA](./media/log-analytics-usage/usage-sla-details.png)

## <a name="next-steps"></a>Następne kroki
* Zobacz [Wyszukiwania w dziennikach w usłudze Log Analytics](log-analytics-log-searches.md), aby dowiedzieć się, jak przeglądać szczegółowe informacje zebrane przez rozwiązania.




<!--HONumber=Nov16_HO2-->


