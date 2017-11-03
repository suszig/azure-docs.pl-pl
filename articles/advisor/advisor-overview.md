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
ms.openlocfilehash: 96925f251cf4984a11516a962740e19a7b9589dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-advisor"></a>Wprowadzenie do usługi Advisor Azure

Dowiedz się więcej o Azure Advisor i jego kluczowych możliwości i odpowiedzi na często zadawane pytania.

## <a name="what-is-advisor"></a>Co to jest klasyfikator?
Klasyfikator jest konsultanta spersonalizowane chmury ułatwiające należy stosować najlepsze rozwiązania w celu zoptymalizowania wdrożeń platformy Azure. Analizuje konfigurację zasobu i danych telemetrycznych użycia, a następnie zaleca rozwiązania, które ułatwiają zwiększenie opłacalności, wydajność, wysoką dostępność i zabezpieczeń zasobów platformy Azure.

Klasyfikator można:
* Pobierz aktywne, można wykonać i spersonalizowane najlepsze rozwiązania — zalecenia. 
* Poprawa wydajności, zabezpieczeń i wysokiej dostępności zasobów, jak Zidentyfikuj możliwości zmniejszyć ogólną Azure wydatków.
* Pobierz zalecenia z wbudowanym proponowanych działań.

Dostęp można uzyskać Advisor za pomocą [portalu Azure](https://aka.ms/azureadvisordashboard). Zaloguj się do [portal](https://portal.azure.com), wybierz pozycję **Przeglądaj**, a następnie przewiń do **Azure Advisor**. Pulpit nawigacyjny usługi Advisor wyświetla spersonalizowane zalecenia dla wybranej subskrypcji. 

Zalecenia są podzielone na cztery kategorie: 

* **Wysoka dostępność**: Upewnij się, i poprawa ciągłości aplikacji biznesowych o znaczeniu krytycznym. Aby uzyskać więcej informacji, zobacz [zalecenia usługi Advisor wysokiej dostępności](advisor-high-availability-recommendations.md).

* **Zabezpieczenia**: wykrywanie zagrożeń i luk w zabezpieczeniach, które mogą prowadzić do naruszenia zabezpieczeń. Aby uzyskać więcej informacji, zobacz [zalecenia dotyczące zabezpieczeń usługi Advisor](advisor-security-recommendations.md).

* **Wydajność**: przyspieszyć aplikacji. Aby uzyskać więcej informacji, zobacz [zalecenia usługi Advisor wydajności](advisor-performance-recommendations.md).

* **Koszt**: optymalizacji i obniżyć ogólną Azure spędzają. Aby uzyskać więcej informacji, zobacz [zalecenia usługi Advisor koszt](advisor-cost-recommendations.md).

  ![Typy zalecenia usługi Advisor](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> Aby uzyskać dostęp do zalecenia doradcy w zakresie, należy najpierw *zarejestrować swoją subskrypcję* usłudze Advisor. Subskrypcja jest zarejestrowana przy *właściciela subskrypcji* uruchamia pulpitu nawigacyjnego usługi Advisor i klika pozycję **Uzyskaj zalecenia** przycisk. Jest to *jednorazowa operacja*. Po zarejestrowaniu subskrypcji są dostępne zalecenia doradcy w zakresie jako *właściciela*, *współautora*, lub *czytnika* dla określonego zasobu, grupy zasobów lub subskrypcji.

Możesz kliknąć zalecenie, aby uzyskać więcej informacji na ten temat. Możesz też dowiedzieć się o akcjach, które można wykonać w celu rozwiązania problemu lub korzystać z możliwości. 

Klasyfikator zalecenia z wbudowanym akcje lub łącza do dokumentacji. Kliknięcie przycisku akcji wbudowanego prezentująca "przebieg użytkownika z przewodnikiem" Aby go zaimplementować. Kliknięcie łącza dokumentacji punktów można się z dokumentacją, który opisuje sposób ręcznego wykonania akcji. 

Klasyfikator co godzinę aktualizuje zalecenia. Jeśli nie zamierzasz natychmiast podejmuj odpowiednie akcje na podstawie zalecenia, można je odłożyć w określonym przedziale czasu lub je zamknąć. 

## <a name="frequently-asked-questions"></a>Często zadawane pytania

### <a name="how-do-i-access-advisor"></a>Jak uzyskać dostępu do usługi Advisor
Dostęp można uzyskać Advisor za pomocą [portalu Azure](https://aka.ms/azureadvisordashboard). Zaloguj się do [portal](https://portal.azure.com), wybierz pozycję **Przeglądaj**, a następnie przewiń do **Azure Advisor**. Pulpit nawigacyjny usługi Advisor wyświetla spersonalizowane zalecenia dla wybranej subskrypcji. 

Można również wyświetlić zalecenia doradcy w zakresie za pomocą bloku zasobu maszyny wirtualnej. Wybierz maszynę wirtualną, a następnie przewiń do zalecenia doradcy w zakresie w menu. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Jakie uprawnienia należy uzyskać dostępu do usługi Advisor?

Aby uzyskać dostęp do zalecenia doradcy w zakresie, należy najpierw *zarejestrować swoją subskrypcję* usłudze Advisor. Subskrypcja jest zarejestrowana przy *właściciela subskrypcji* uruchamia pulpitu nawigacyjnego usługi Advisor i klika pozycję **Uzyskaj zalecenia** przycisk. Jest to *jednorazowa operacja*. Po zarejestrowaniu subskrypcji są dostępne zalecenia doradcy w zakresie jako *właściciela*, *współautora*, lub *czytnika* dla określonego zasobu, grupy zasobów lub subskrypcji.

### <a name="how-often-are-advisor-recommendations-updated"></a>Jak często są zalecenia doradcy w zakresie aktualizacji?

Zalecenia doradcy w zakresie są aktualizowane co godzinę.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Jakie zasoby Advisor oferuje zalecenia dotyczące?

Klasyfikator zawiera zalecenia dotyczące maszyny wirtualnej, zestawów dostępności bramy aplikacji, usług aplikacji, serwerów SQL, baz danych i pamięci podręcznej Redis.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Można odłożyć lub zignorować zalecenie?

Aby odłożyć, lub zignorować zalecenie, kliknij przycisk **Odłóż** przycisk lub łącze. Można określić czas włączenia trybu czuwania okresu lub wybierz opcję **nigdy** aby odrzucić zalecenia.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat zalecenia doradcy w zakresie, zobacz:

* [Wprowadzenie do usługi Advisor](advisor-get-started.md)
* [Zalecenia doradcy w zakresie wysokiej dostępności](advisor-high-availability-recommendations.md)
* [Zalecenia doradcy w zakresie zabezpieczeń](advisor-security-recommendations.md)
* [Zalecenia doradcy w zakresie wydajności](advisor-performance-recommendations.md)
* [Zalecenia doradcy w zakresie koszt](advisor-cost-recommendations.md)
