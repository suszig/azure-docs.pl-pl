---
title: "Wprowadzenie do usługi Advisor Azure | Dokumentacja firmy Microsoft"
description: "Optymalizowanie wdrożeń platformy Azure przy użyciu klasyfikatora Azure."
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 906450f75557820bb27762707c3328b08b23cccb
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/08/2017
---
# <a name="introduction-to-azure-advisor"></a>Wprowadzenie do usługi Advisor Azure

Więcej informacji na temat kluczowych możliwości usługi Advisor Azure i uzyskać odpowiedzi na często zadawane pytania.

## <a name="what-is-advisor"></a>Co to jest klasyfikator?
Klasyfikator jest konsultanta spersonalizowane chmury ułatwiające należy stosować najlepsze rozwiązania w celu zoptymalizowania wdrożeń platformy Azure. Analizuje konfigurację zasobu i danych telemetrycznych użycia, a następnie zaleca rozwiązania, które ułatwiają zwiększenie opłacalności, wydajność, wysoką dostępność i zabezpieczeń zasobów platformy Azure.

Klasyfikator można:
* Pobierz aktywne, można wykonać i spersonalizowane najlepsze rozwiązania — zalecenia. 
* Poprawa wydajności, zabezpieczeń i wysokiej dostępności zasobów, jak Zidentyfikuj możliwości zmniejszyć ogólną Azure wydatków.
* Pobierz zalecenia z wbudowanym proponowanych działań.

Dostęp można uzyskać Advisor za pomocą [portalu Azure](https://aka.ms/azureadvisordashboard). Zaloguj się do [portal](https://portal.azure.com), zlokalizuj **Advisor** w menu nawigacji, lub wyszukaj go w **więcej usług** menu.

Pulpit nawigacyjny usługi Advisor wyświetla spersonalizowane zalecenia dotyczące wszystkich subskrypcji.  Można zastosować filtry, aby wyświetlić zalecenia dla określonej subskrypcji i typów zasobów.  Zalecenia są podzielone na cztery kategorie: 

* **Wysoka dostępność**: Upewnij się, i poprawa ciągłości aplikacji biznesowych o znaczeniu krytycznym. Aby uzyskać więcej informacji, zobacz [zalecenia usługi Advisor wysokiej dostępności](advisor-high-availability-recommendations.md).
* **Zabezpieczenia**: wykrywanie zagrożeń i luk w zabezpieczeniach, które mogą prowadzić do naruszenia zabezpieczeń. Aby uzyskać więcej informacji, zobacz [zalecenia dotyczące zabezpieczeń usługi Advisor](advisor-security-recommendations.md).
* **Wydajność**: przyspieszyć aplikacji. Aby uzyskać więcej informacji, zobacz [zalecenia usługi Advisor wydajności](advisor-performance-recommendations.md).
* **Koszt**: do optymalizacji i zmniejszenia ogólnego Azure wydatków. Aby uzyskać więcej informacji, zobacz [zalecenia usługi Advisor koszt](advisor-cost-recommendations.md).

  ![Typy zalecenia usługi Advisor](./media/advisor-overview/advisor-dashboard.png)

> [!NOTE]
> Aby użyć klasyfikatora Azure z subskrypcją, subskrypcji *właściciela* wymaga uruchomienia pulpitu nawigacyjnego usługi Advisor.  Ta akcja rejestruje subskrypcji w usłudze Advisor.  Od tego momentu na żadną subskrypcję *właściciela*, *współautora*, lub *czytnika* mogą uzyskiwać dostęp do zalecenia doradcy w zakresie dla subskrypcji. 

Kliknij kategorię, aby wyświetlić listę zaleceń w ramach tej kategorii i wybierz zalecenie, aby uzyskać więcej informacji na ten temat.  Możesz też dowiedzieć się o akcjach, które można wykonać w celu rozwiązania problemu lub korzystać z możliwości.

![Kategoria zalecenia usługi Advisor](./media/advisor-overview/advisor-ha-category-example.png) 

Wybierz akcję zalecaną dla zalecenie wykonania zalecenia.  Prosty interfejs otworzy można zaimplementować zalecenia, lub dotyczą dokumentacji, która pomaga w implementacji.  Po zaimplementować zalecenia, może potrwać dzień klasyfikatorze uznają, że.

Jeśli nie zamierzasz natychmiast podejmuj odpowiednie akcje na podstawie zalecenia, można je odłożyć w określonym przedziale czasu lub je zamknąć.  Jeśli nie chcesz otrzymywać zalecenia dla określonej subskrypcji lub grupy zasobów, można skonfigurować usługi Advisor można generować tylko zalecenia dla określonej subskrypcji i grupy zasobów.

## <a name="frequently-asked-questions"></a>Często zadawane pytania

### <a name="how-do-i-access-advisor"></a>Jak uzyskać dostępu do usługi Advisor
Dostęp można uzyskać Advisor za pomocą [portalu Azure](https://aka.ms/azureadvisordashboard). Zaloguj się do [portal](https://portal.azure.com), zlokalizuj **Advisor** w menu nawigacji, lub wyszukaj go w **więcej usług** menu.

Można również wyświetlić zalecenia doradcy w zakresie za pomocą interfejsu zasobu maszyny wirtualnej. Wybierz maszynę wirtualną, a następnie przewiń do zalecenia doradcy w zakresie w menu. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Jakie uprawnienia należy uzyskać dostępu do usługi Advisor?

Aby uzyskiwać zalecenia doradcy w zakresie subskrypcji, najpierw należy zarejestrować subskrypcji z usługi Advisor. Subskrypcja jest rejestrowane podczas subskrypcji *właściciela* uruchamia pulpitu nawigacyjnego usługi Advisor. Jest to jednorazowa operacja. Po zarejestrowaniu subskrypcji są dostępne zalecenia doradcy w zakresie jako *właściciela*, *współautora*, lub *czytnika* subskrypcji.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Jakie zasoby Advisor oferuje zalecenia dotyczące?

Klasyfikator zawiera zalecenia dotyczące maszyny wirtualnej, zestawów dostępności bramy aplikacji, usług aplikacji, serwerów SQL, baz danych i pamięci podręcznej Redis.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Można odłożyć lub zignorować zalecenie?

Aby odłożyć, lub zignorować zalecenie, kliknij przycisk **Odłóż** łącza. Można określić czas włączenia trybu czuwania okresu lub wybierz opcję **nigdy** aby odrzucić zalecenia.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat zalecenia doradcy w zakresie, zobacz:

* [Wprowadzenie do usługi Advisor](advisor-get-started.md)
* [Zalecenia doradcy w zakresie wysokiej dostępności](advisor-high-availability-recommendations.md)
* [Zalecenia doradcy w zakresie zabezpieczeń](advisor-security-recommendations.md)
* [Zalecenia doradcy w zakresie wydajności](advisor-performance-recommendations.md)
* [Zalecenia doradcy w zakresie koszt](advisor-cost-recommendations.md)
