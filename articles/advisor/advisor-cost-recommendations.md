---
title: "Azure zalecenia usługi Advisor koszt | Dokumentacja firmy Microsoft"
description: "Optymalizowanie koszt wdrożeń platformy Azure przy użyciu klasyfikatora Azure."
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
ms.openlocfilehash: 7b9c7037271fabd67c1ada80420ad72c340e46bb
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/08/2017
---
# <a name="advisor-cost-recommendations"></a>Zalecenia doradcy w zakresie koszt

Advisor pomaga zoptymalizować i zmniejszyć ogólną Azure wydatków, określając bezczynności i wyczerpaniu zasobów. Zalecenia można uzyskać koszt **koszt** na pulpicie nawigacyjnym usługi Advisor.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Optymalizacja maszyny wirtualnej spędzają na zmianę rozmiaru lub zamykanie niedostatecznie wystąpień 
Mimo że niektóre scenariusze aplikacji może spowodować niskiego poziomu wykorzystania zgodnie z projektem, można często zaoszczędzić, zarządzając rozmiaru i liczby maszyn wirtualnych. Klasyfikator monitoruje użycie maszyny wirtualnej w ciągu ostatnich 14 dni, a następnie identyfikuje niskiego wykorzystania maszyn wirtualnych. Maszyny wirtualne, których użycie procesora CPU wynosi 5 procent lub mniej i użycie sieci jest 7 MB lub mniej przez cztery lub więcej dni są traktowane jako niskiego wykorzystania maszyn wirtualnych.

Klasyfikator pokazuje szacowany koszt kontynuowania działania maszyny wirtualnej, dzięki czemu można zamknąć lub zmiany rozmiaru.

Jeśli chcesz mieć bardziej agresywną na określenie niedostatecznie maszyn wirtualnych, można dostosować średni reguły wykorzystanie Procesora na podstawie subskrypcji na.

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a>Używanie ekonomiczne rozwiązanie do zarządzania cele dotyczące wydajności wielu baz danych serwera SQL
Klasyfikator identyfikuje wystąpienia programu SQL server, które mogą korzystać z tworzenie elastycznych pul baz danych. Elastyczne pule baz danych zapewniają proste i ekonomiczne rozwiązanie do zarządzania cele wydajności wielu baz danych, które mają różnych wzorców użycia. Aby uzyskać więcej informacji na temat Azure pule elastyczne, zobacz [co to jest pula elastyczna Azure?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Obniżenie kosztów przez wyeliminowanie nieudostępniany obwody usługi ExpressRoute
Klasyfikator identyfikuje obwody usługi ExpressRoute, które zostały w widoku stanu dostawcy *nieudostępniane* więcej niż jeden miesiąc i zaleca usunięcie obwodu, jeśli nie planuje udostępnić obwodu łączność Dostawca.

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Jak uzyskać dostęp zalecenia koszt klasyfikatora Azure

1. Zaloguj się do [portalu Azure](https://portal.azure.com), a następnie otwórz [Advisor](https://aka.ms/azureadvisordashboard).

2.  Na pulpicie nawigacyjnym usługi Advisor, kliknij przycisk **koszt** kartę.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat zalecenia doradcy w zakresie, zobacz:
* [Wprowadzenie do usługi Advisor](advisor-overview.md)
* [Rozpoczęcie pracy](advisor-get-started.md)
* [Zalecenia doradcy w zakresie wydajności](advisor-cost-recommendations.md)
* [Zalecenia doradcy w zakresie wysokiej dostępności](advisor-cost-recommendations.md)
* [Zalecenia doradcy w zakresie zabezpieczeń](advisor-cost-recommendations.md)
