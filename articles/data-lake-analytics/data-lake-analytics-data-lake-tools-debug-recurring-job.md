---
title: "Jak rozwiązywać problemy z nietypowe cyklicznych zadań | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak debugowanie nietypowe cyklicznych zadań przy użyciu narzędzia Azure Data Lake Tools dla programu Visual Studio."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/27/2017
ms.author: yanacai
ms.openlocfilehash: a358f94b117c12511028a875e56b5c9dba8d3382
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-troubleshoot-an-abnormal-recurring-job"></a>Jak rozwiązywać problemy z nietypowe cyklicznych zadań

W tym dokumencie, firma Microsoft wprowadzi sposób użycia [Azure Data Lake Tools dla programu Visual Studio](http://aka.ms/adltoolsvs) do rozwiązywania problemów z cyklicznego zadania. Więcej informacji na temat potoku i cyklicznych zadań z [tutaj](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/).
Cyklicznych zadań zwykle udostępniać samej logiki kwerendy i podobnych danych wejściowych. Na przykład masz cyklicznego zadania uruchomione w poniedziałek codziennie o 8: 00 do obliczenia co tydzień aktywnego użytkownika ostatniego tygodnia, skryptów dla tych zadań udostępniać jeden szablon skryptu, który zawiera logikę kwerendy i dane wejściowe dla tych zadań są dane użycia ostatniego tygodnia. Udostępnianie sam zapytania logiki i podobne wejściowego zazwyczaj oznacza wykonywania tych zadań jest podobnych i stały, jeśli cyklicznych zadań nagle wykonują nietypowe, nie powiodło się lub spowolnić znacznie, możesz chcieć:

1.  Zobacz raporty statystyk dla przebiegów podglądy cyklicznego zadania, aby zobaczyć, co się stało.
2.  Porównaj nietypowe zadania o normalnym jeden, aby dowiedzieć się, co zostało zmienione.

**Wyświetl zadania dotyczące** w narzędzia Azure Data Lake Tools dla Visual Studio pomaga przyspieszyć rozwiązywania problemów z postępem w obu przypadkach.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>Krok 1: Znajdowanie cyklicznych zadań i otwieranie widoku Zadania pokrewne

Aby użyć powiązane zadanie, które widoku rozwiązać cyklicznego zadania problem, należy najpierw odnaleźć cyklicznych zadań w programie Visual Studio, a następnie otwórz widok zadania pokrewne.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Przypadek 1: Ma adres URL cyklicznych zadań

Za pomocą **Narzędzia > Data Lake > Wyświetl zadania**, możesz wkleić adres URL zadania, aby otworzyć widoku zadania w programie Visual Studio i za pośrednictwem widoku powiązanych zadań, aby otworzyć związane z widoku zadania.

![Data Lake Analytics Tools Wyświetl powiązane zadania](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Przypadek 2: Masz potoku dla cyklicznego zadania, ale nie jej adres URL

W programie Visual Studio, możesz otworzyć potoku przeglądarce za pośrednictwem **Eksploratora serwera > konto usługi Data Lake Analytics > potoki** (Jeśli nie możesz znaleźć tego węzła w Eksploratorze serwera, Pobierz najnowszą wersję apletu narzędzia [tutaj](http://aka.ms/adltoolsvs)). W przeglądarce potoku potoki wszystkie konta ADLA są wyświetlane po lewej stronie, można rozszerzyć potoki, aby znaleźć wszystkich zadań cyklicznych, kliknij ten, który ma problemy, powiązane widoku zadania zostanie otwarty po prawej.

![Data Lake Analytics Tools Wyświetl powiązane zadania](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

![Data Lake Analytics Tools Wyświetl powiązane zadania](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-statistics-report"></a>Krok 2: Analizowanie raportu statystyk

Podsumowanie i Raport statystyki są wyświetlane u góry widoku Zadania pokrewne, za pomocą którego można uzyskać potencjalną główną przyczynę nietypowe. 

1.  Najpierw należy odnaleźć nietypowe zadania w raporcie. Na osi X zawiera czas przesyłania zadania, za pomocą którego można zlokalizować nietypowe zadania.
2.  Wykonaj poniżej proces sprawdzania statystyk i uzyskać szczegółowe informacje o nieprawidłowej i możliwych rozwiązań.

![Data Lake Analytics Tools Wyświetl powiązane zadania](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-recurring-job-to-a-normal-job"></a>Krok 3: Porównaj nietypowe cyklicznych zadań do normalnej zadania

Przesłane wszystkich cyklicznych zadań za pomocą listy zadań u dołu widoku Zadania pokrewne można znaleźć. Za pomocą kliknij prawym przyciskiem myszy można porównać nietypowe zadania o normalnym poprzedniego można znaleźć w widoku zadania Diff więcej szczegółowych informacji i potencjalne rozwiązania.

![Data Lake Analytics Tools Wyświetl powiązane zadania](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Zazwyczaj konieczne należy zwrócić uwagę na duże różnice między te zadania 2, jak są one prawdopodobnie powodów powodujące problemy z wydajnością i można również wykonać następujące czynności, aby wykonać dalsze sprawdzanie.

![Data Lake Analytics Tools Wyświetl powiązane zadania](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Następne kroki

* [Debugowanie i rozwiąż problemy pochylenia danych](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Debugowanie skryptu U-SQL niepowodzenie zadania dla zdefiniowanej przez użytkownika kod błędu](data-lake-analytics-debug-u-sql-jobs.md)