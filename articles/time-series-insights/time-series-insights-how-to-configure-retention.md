---
title: "Sposób konfigurowania przechowywania w środowisku Azure czas serii Insights | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób konfigurowania przechowywania w środowisku Azure czas serii Insights."
services: time-series-insights
ms.service: time-series-insights
author: anshan
ms.author: anshan
manager: kfile
editor: MicrosoftDocs/tsidocs
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 02/09/2018
ms.openlocfilehash: bd688f516e200a37a6c88a8779282f7391eaf8b8
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2018
---
# <a name="configuring-retention-in-time-series-insights"></a>Konfigurowanie przechowywania w czasie serii Insights
W tym artykule opisano sposób konfigurowania **czas przechowywania danych** i **zachowanie przekroczony limit magazynu** w usłudze Azure czas serii Insights.

Każde środowisko czasu serii Insights (TSI) ma ustawienie, aby skonfigurować **czas przechowywania danych**. Wartość jest liczony od 1 do 400 dni. Dane zostaną usunięte, oparte na środowisku pojemności lub przechowywania okresem magazynu (1-400), zależnie od zostanie osiągnięty jako pierwszy.

Każde środowisko TSI ma dodatkowe ustawienie **zachowanie przekroczony limit magazynu**. To ustawienie określa przychodzące i przeczyszczania zachowanie po osiągnięciu maksymalnej pojemności środowisku. Istnieją dwa zachowania do wyboru:
- **Przeczyść stare dane** (ustawienie domyślne)  
- **Transfer danych przychodzących Wstrzymaj**

Aby uzyskać szczegółowe informacje lepiej zrozumieć te ustawienia, przejrzyj [opis czas przechowywania danych w czasie serii Insights](time-series-insights-concepts-retention.md).  

## <a name="configure-data-retention"></a>Konfigurowanie przechowywania danych

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Znajdź istniejące środowisko Insights serii czasu. Wybierz **wszystkie zasoby** w menu po lewej stronie portalu Azure. Wybierz środowisko usługi Time Series Insights.

3. W obszarze **ustawienia** nagłówek, wybierz **Konfiguruj**.

4. Wybierz **czas przechowywania danych** konfigurowania przechowywania za pomocą suwaka, lub wpisz liczbę w polu tekstowym.

5. Uwaga **pojemności** ustawienie, ponieważ ta konfiguracja ma wpływ na maksymalną ilość danych dotyczących zdarzeń i pojemność pamięci masowej do przechowywania danych. 

6. Przełącz **zachowanie przekroczony limit magazynu** ustawienie. Wybierz **przeczyścić stare dane** lub **wstrzymać wejściowych** zachowanie.

7. Wybierz **zapisać** skonfigurować zmiany.

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać więcej informacji, przejrzyj [opis czas przechowywania danych w czasie serii Insights](time-series-insights-concepts-retention.md).
