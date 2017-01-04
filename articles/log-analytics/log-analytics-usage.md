---
title: "Analizowanie użycia danych w usłudze Log Analytics | Microsoft Docs"
description: "Za pomocą pozycji pulpitu nawigacyjnego Użycie usługi Log Analytics możesz wyświetlić informacje o tym, ile danych jest wysyłanych do usługi OMS."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/02/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 668cde23cb717fcad52fd7823561d10735e6df1b


---
# <a name="analyze-data-usage-in-log-analytics"></a>Analizowanie użycia danych w usłudze Log Analytics
Usługa Log Analytics gromadzi dane i okresowo wysyła je do usługi OMS.  Pulpitu nawigacyjnego **Użycie usługi Log Analytics** możesz użyć do wyświetlenia ilości danych wysyłanych do usługi OMS. Pulpit nawigacyjny wyświetla również informacje o tym, ile danych jest wysyłanych przez rozwiązania i jak często Twoje serwery wysyłają dane.

> [!NOTE]
> Jeśli masz bezpłatne konto, dziennie możesz wysłać do usługi OMS maksymalnie 500 MB danych. Jeśli przekroczysz dzienny limit, analiza danych zostanie zatrzymana, a następnie zostanie wznowiona na początku następnego dnia. W takim przypadku konieczne będzie ponowne przesłanie wszelkich danych, które nie zostały przyjęte lub przetworzone przez usługę OMS.

W przypadku przekroczenia dziennego limitu użycia lub zbliżenia się do niego możesz opcjonalnie usunąć rozwiązanie, aby ograniczyć ilość danych do przesłania do usługi OMS. Aby uzyskać więcej informacji o usuwaniu rozwiązań, zobacz [Dodawanie rozwiązań usługi Log Analytics z galerii rozwiązań](log-analytics-add-solutions.md).

![pulpit nawigacyjny Użycie](./media/log-analytics-usage/usage-dashboard01.png)

Pulpit nawigacyjny **Użycie usługi Log Analytics** udostępnia następujące informacje:

- Ilość danych
    - Ilość danych w czasie (na podstawie bieżącego zakresu czasu)
    - Ilość danych wg rozwiązania
    - Dane nieskojarzone z komputerem
- Komputery
    - Komputery wysyłające dane
    - Komputery bez danych w ostatnich 24 godzinach
- Oferty
    - Węzły wglądu w dane i analizy
    - Węzły automatyzacji i kontroli
    - Węzły zabezpieczeń
- Wydajność
    - Ilość czasu zbierania i indeksowania danych
- Lista zapytań

## <a name="to-work-with-usage-data"></a>Aby pracować z danymi użycia
1. Jeśli nie zostało to jeszcze zrobione, zaloguj się do witryny [Azure Portal](https://portal.azure.com) przy użyciu subskrypcji platformy Azure.
2. W menu **Centrum** kliknij pozycję **Więcej usług** i na liście zasobów wpisz ciąg **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Kliknij pozycję **Log Analytics**.  
    ![Centrum platformy Azure](./media/log-analytics-usage/hub.png)
3. Pulpit nawigacyjny **Log Analytics** wyświetla listę obszarów roboczych. Wybierz obszar roboczy.
4. Na pulpicie nawigacyjnym *obszar roboczy* kliknij pozycję **Użycie usługi Log Analytics**.
5. Na pulpicie nawigacyjnym **Użycie usługi Log Analytics** kliknij pozycję **Czas: ostatnie 24 godziny**, aby zmienić przedział czasu.  
    ![przedział czasu](./media/log-analytics-usage/time.png)
6. Wyświetl bloki kategorii użycia pokazujące obszary, które Cię interesują. Wybierz blok, a następnie kliknij w nim pozycję, aby wyświetlić więcej szczegółów w obszarze [Wyszukiwanie w dzienniku](log-analytics-log-searches.md).  
    ![przykładowy blok użycia danych](./media/log-analytics-usage/blade.png)
7. Na pulpicie nawigacyjnym wyszukiwania w dzienniku przejrzyj wyniki zwrócone w wyniku wyszukiwania.  
    ![przykład wyszukiwania w dzienniku użycia](./media/log-analytics-usage/usage-log-search.png)


## <a name="next-steps"></a>Następne kroki
* Aby wyświetlić szczegółowe informacje, które są zbierane i przesyłane do usługi OMS przez funkcje i rozwiązania, zobacz [Wyszukiwanie w dzienniku w usłudze Log Analytics](log-analytics-log-searches.md).



<!--HONumber=Dec16_HO5-->


