---
title: "Tworzenie i udostępnianie pulpitów nawigacyjnych danych Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Ten samouczek ułatwia zrozumienie sposobu analizy dzienników pulpitów nawigacyjnych można Wizualizuj wszystkie zapisany dziennik wyszukiwania, umożliwiając pojedynczego obiektyw, aby wyświetlić środowiska."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 272945134b534a5ded794379ce5e96b0902a4227
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="create-and-share-dashboards-of-log-analytics-data"></a>Tworzenie i udostępnianie pulpitów nawigacyjnych danych analizy dzienników

Analiza dzienników pulpitów nawigacyjnych można zwizualizować wszystkie zapisany dziennik wyszukiwania, dzięki czemu mogą znaleźć, grupowania i udostępnianie danych operacyjnych IT w organizacji.  Ten samouczek obejmuje utworzenie wyszukiwania dziennika, która będzie służyć do obsługi udostępnionych pulpit nawigacyjny, który będzie dostępna przez Twój zespół pomocy technicznej operacji IT.  Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie udostępnionego pulpitu nawigacyjnego w portalu Azure
> * Wizualizuj wyszukiwania dziennika wydajności 
> * Dodaj wyszukiwanie dziennika do udostępnionego pulpitu nawigacyjnego 
> * Dostosowywanie kafelka w udostępnionego pulpitu nawigacyjnego

Aby ukończyć przykład, w tym samouczku, musisz mieć istniejącej maszyny wirtualnej [podłączony do obszaru roboczego analizy dzienników](log-analytics-quick-collect-azurevm.md).  
 
## <a name="log-in-to-azure-portal"></a>Zaloguj się do portalu Azure
Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-shared-dashboard"></a>Tworzenie udostępnionego pulpitu nawigacyjnego

Najpierw Zobacz po zalogowaniu się do portalu Microsoft Azure jest [pulpitu nawigacyjnego](../azure-portal/azure-portal-dashboards.md).<br> ![Pulpitu nawigacyjnego portalu Azure](media/log-analytics-tutorial-dashboards/log-analytics-portal-dashboard.png)

W tym miejscu można zebrać razem danych operacyjnych, które najważniejszych IT we wszystkich zasobów platformy Azure, w tym dane telemetryczne z usługi Analiza dzienników Azure.  Zanim firma Microsoft wkroczyć do wizualizacji wyszukiwania dziennika, umożliwia najpierw utworzyć pulpit nawigacyjny i udostępnić go.  Pozwala to uzyskać przeszkadza przed traktujemy naszym przykładzie wydajności dziennik wyszukiwania, co będzie renderowane jako wykres liniowy i dodaj go do pulpitu nawigacyjnego.  

Aby utworzyć pulpit nawigacyjny, wybierz **nowego pulpitu nawigacyjnego** przycisk Dalej, aby nazwa bieżącego pulpitu nawigacyjnego.<br> ![Tworzenie nowego pulpitu nawigacyjnego w portalu Azure](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-01.png)

Ta akcja tworzy nowy, pusty, prywatne pulpitu nawigacyjnego i umieszcza Tryb dostosowywania, w którym można nazwa pulpitu nawigacyjnego i dodać lub zmienić Kafelki. Edytuj nazwę pulpitu nawigacyjnego i określ *przykładowy pulpit nawigacyjny* dla tego samouczka wybierz **gotowe dostosowywanie**.<br><br> ![Zapisz dostosowanego pulpitu nawigacyjnego platformy Azure](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-02.png)

Podczas tworzenia pulpitu nawigacyjnego jest prywatna domyślnie, co oznacza, że jesteś jedyną osobą, która to sprawdzić. Aby umożliwić innym osobom, użyj **udziału** przycisku, który pojawi się równolegle z innymi poleceniami pulpitu nawigacyjnego.<br> ![Udostępnianie nowego pulpitu nawigacyjnego w portalu Azure](media/log-analytics-tutorial-dashboards/log-analytics-share-dashboard.png) 

Zostanie wyświetlona prośba o wybranie subskrypcji i grupy zasobów dla pulpitu nawigacyjnego do opublikowania dla. Dla wygody portalu do publikowania przewodniki środowisko możesz kierunku wzorzec, gdzie umieścić pulpitów nawigacyjnych w grupie zasobów o nazwie **pulpity nawigacyjne**.  Sprawdź do wybranej subskrypcji i kliknij przycisk **publikowania**.  Dostęp do informacji wyświetlanych na pulpicie nawigacyjnym jest kontrolowany przy użyciu [kontroli dostępu na podstawie zasobów Azure](../active-directory/role-based-access-control-configure.md).   

## <a name="visualize-a-log-search"></a>Wizualizuj wyszukiwania dziennika

Można utworzyć zapytania podstawowego w jednym wierszu z portalu wyszukiwania dziennika w portalu Azure. Portal dziennik wyszukiwania może być używany bez uruchamiania zewnętrznego portalu i służy do wykonywania różnych funkcji z dziennika wyszukiwania w tym tworzenie reguły alertu, tworzenie grup komputerów i eksportowanie wyników zapytania. 

[Portal analityka zaawansowane](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-the-Analytics-portal) to dedykowany portal, który udostępnia zaawansowane funkcje, które nie jest dostępne w portalu wyszukiwania dziennika. Funkcje obejmują możliwość edytowania zapytania w wielu wierszach, selektywnie wykonania kodu, kontekstowej Intellisense i analiza inteligentne. W portalu analityka zaawansowane będzie utworzyć widok wydajności w formie graficznej, zapisz go w przyszłości wyszukiwania i przypiąć go do udostępnionego pulpitu nawigacyjnego utworzonego wcześniej.   

Można uruchomić portal analityka Zaawansowane z linku w portalu wyszukiwania dziennika.<br> ![Uruchamianie portal analityka zaawansowane](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-01.png)

W portalu analityka wprowadź następujące zapytanie zwracało tylko procesora rekordów użycia dla komputerów z systemami Windows i Linux, pogrupowane według komputera i TimeGenerated i wyświetlane na wykresie visual:

```
Perf | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1m), Computer | render timechart
```

Zapisz zapytanie, wybierając **Zapisz zapytanie** przycisk w prawym górnym rogu.<br> ![Zapisz zapytanie z portalu analityka zaawansowane](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-02.png)<br><br> W **Zapisz zapytanie** w Panelu sterowania, podaj nazwę, taki jak *maszynach wirtualnych platformy Azure — użycie procesora* , a następnie kliknij przycisk **zapisać**.  W ten sposób można utworzyć biblioteki wspólne zapytania wyszukiwania z lub zmodyfikuj go bez konieczności ponownego zapisania go całkowicie.  Na koniec przypiąć to do udostępnionego pulpitu nawigacyjnego utworzonego wcześniej, wybierając **wykresu numeru Pin do pulpitu nawigacyjnego Azure** przycisk od środka górnym rogu strony.  

Teraz, gdy mamy kwerendy przypięty do pulpitu nawigacyjnego Zauważ, że ma rodzajowy tytuł i komentarz poniżej.<br> ![Przykładowy pulpit nawigacyjny platformy Azure](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-01.png)<br><br>  Firma Microsoft należy zmienić jego nazwę wpisując tekst opisowy, który można łatwo zrozumiałe dla tych, wyświetlając go.  Kliknij prawym przyciskiem myszy Kafelek i wybierz **kafelka edycji**.  Po zakończeniu Dostosowywanie tytułu i podtytułu kafelka kliknij **aktualizacji**.  Transparent pojawi się monitem o Opublikuj zmiany lub odrzucić.  Kliknij przycisk **Opublikuj zmiany** , a następnie Zamknij **edytowanie kafelka** Panelu sterowania.  

![Ukończono konfigurację przykładowy pulpit nawigacyjny](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-02.png)

## <a name="next-steps"></a>Następne kroki
W tym samouczku przedstawiono sposób tworzenia pulpitu nawigacyjnego w portalu Azure i Dodaj do niej wyszukiwania dziennika.  ADVANCE do samouczka dalej, aby dowiedzieć się więcej różnych odpowiedzi, które można zaimplementować oparte na wyniki wyszukiwania dziennika.  

> [!div class="nextstepaction"]
> [Odpowiadanie na zdarzenia z dziennika analizy alerty](log-analytics-tutorial-response.md)