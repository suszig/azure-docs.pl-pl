---
title: "Zarządzanie koszt przechowywania danych w Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zmienić cenową planu i dane zasad przechowywania obszaru roboczego analizy dzienników w portalu Azure."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 35064c792b72222d59b1d3f0913a92a4a2b34612
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="manage-cost-of-data-retention-with-your-log-analytics-workspace"></a>Zarządzanie koszt przechowywania danych z obszaru roboczego analizy dzienników
Gdy zarejestrujesz się w celu wykonania analizy dziennika, w zależności od wybranego planu ma limitu jak długo są przechowywane dane generowane przez swoich połączonych źródeł w obszarze roboczym.  W tym artykule omówiono zagadnienia, które mogą mieć wpływ na koszty przechowywania tych danych w różnych okresach czasu i sposób konfigurowania tej granicy.   

Ponieważ analizy dzienników mogą wymagać dużych ilości danych ze źródła lokalnego, środowisk chmury i hybrydowych koszt przechowywania danych przez pewien czas mogą być znaczące w zależności od następujących czynników:

* Liczba systemów, składników infrastruktury, zasobów w chmurze, itp., które są zbierane z
* Typ danych utworzonych przez źródło, takich jak kolejki komunikatów, dzienniki, zdarzenia, dane związane z zabezpieczeniami lub metryki wydajności
* Ilość danych wygenerowanych przez tych źródeł 
* Liczba rozwiązania do zarządzania włączone, źródła danych i częstotliwość kolekcji

> [!NOTE]
> Zapoznaj się z dokumentacją, dla każdego rozwiązania, ponieważ zapewnia szacunkową ilość danych, które zbiera.   

Jeśli pracujesz w *wolne* planu, dane są ograniczone do przechowywania siedem dni.  Aby uzyskać *autonomiczny* lub *zapłacone* warstwy, zbierane są dostępne dane ciągu ostatnich 31 dni.  

Podczas używania *wolne* plan, jeśli okaże się stale przekracza dozwolone kwoty, obszaru roboczego można zmienić na plan płatnej do zbierania danych po przekroczeniu tego limitu. 

> [!NOTE]
> Opłaty za wybranie opcji wybierz dłuższy okres przechowywania dla warstwy płatną. Możesz zmienić typ planu w dowolnym momencie, a także aby uzyskać więcej informacji o cenach, zobacz [szczegóły cennika](https://azure.microsoft.com/pricing/details/log-analytics/). 

## <a name="change-the-data-retention-period"></a>Zmienić okres przechowywania danych 

1. Zaloguj się do [Azure Portal](http://portal.azure.com). 
2. Kliknij przycisk **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.
3. W okienku subskrypcje analizy dzienników wybierz obszar roboczy, aby zmodyfikować z listy.
4. Na stronie obszaru roboczego kliknij **przechowywania** w lewym okienku.
5. W okienku obszaru roboczego przechowywania poruszając suwakiem, możesz zwiększyć lub zmniejszyć liczbę dni, a następnie kliknij przycisk **zapisać**.  Jeśli pracujesz w *wolnego* warstwy, nie można zmodyfikować okres przechowywania danych i trzeba Uaktualnij do planu płatną w celu kontrolowania tego ustawienia.<br><br> ![Zmiana ustawienia przechowywania danych obszaru roboczego](media/log-analytics-manage-cost/manage-cost-change-retention.png)

## <a name="next-steps"></a>Następne kroki  

Aby określić, ile danych są zbierane, jakie źródła są wysyłania, a także różne typy danych wysyłanych do zarządzania konsumenckich i kosztów, zobacz [analizowanie danych użycia w analizy dzienników](log-analytics-usage.md)