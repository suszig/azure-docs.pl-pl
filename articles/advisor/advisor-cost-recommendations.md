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
ms.openlocfilehash: 51320d93689da3e37c0946d8877b27a11793d9c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="advisor-cost-recommendations"></a>Zalecenia doradcy w zakresie koszt

Advisor pomaga zoptymalizować i zmniejszyć ogólną Azure wydatków, określając bezczynności i wyczerpaniu zasobów. Zalecenia można uzyskać koszt **koszt** na pulpicie nawigacyjnym usługi Advisor.

![Karta koszt usługi Advisor](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a>Optymalizacja spędzają na zmieniając niedostatecznie wystąpień maszyny wirtualnej 
Mimo że niektóre scenariusze aplikacji może spowodować niskiego poziomu wykorzystania zgodnie z projektem, można często zaoszczędzić, zarządzając rozmiaru i liczby maszyn wirtualnych. Klasyfikator monitoruje użycie maszyny wirtualnej w ciągu ostatnich 14 dni, a następnie identyfikuje niskiego wykorzystania maszyn wirtualnych. Maszyny wirtualne, których użycie procesora CPU wynosi 5 procent lub mniej i użycie sieci jest 7 MB lub mniej przez cztery lub więcej dni są traktowane jako niskiego wykorzystania maszyn wirtualnych.

Klasyfikator pokazuje szacowany koszt kontynuowania działania maszyny wirtualnej, dzięki czemu można zamknąć lub zmiany rozmiaru.  

![Klasyfikator koszt zalecenia dotyczące zmiany rozmiaru maszyny wirtualne](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a>Używanie ekonomiczne rozwiązanie do zarządzania cele dotyczące wydajności wielu baz danych serwera SQL
Klasyfikator identyfikuje wystąpienia programu SQL server, które mogą korzystać z tworzenie elastycznych pul baz danych. Elastyczne pule baz danych zapewniają proste i ekonomiczne rozwiązanie do zarządzania cele wydajności wielu baz danych, które mają różnych wzorców użycia. Aby uzyskać więcej informacji na temat Azure pule elastyczne, zobacz [co to jest pula elastyczna Azure?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).

![Klasyfikator koszt zalecenia dotyczące elastyczne pule baz danych](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Jak zalecenia kosztem dostępu w programie Azure Advisor

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. W okienku po lewej stronie kliknij **więcej usług**.

3. W okienku usługi menu w obszarze **monitorowanie i zarządzanie**, kliknij przycisk **Azure Advisor**.  
 Zostanie wyświetlony pulpit nawigacyjny usługi Advisor.

4. Na pulpicie nawigacyjnym usługi Advisor, kliknij przycisk **koszt** kartę.

5. Wybierz subskrypcję, dla którego chcesz otrzymywać zalecenia, a następnie kliknij przycisk **Uzyskaj zalecenia**.

> [!NOTE]
> Aby uzyskać dostęp do zalecenia doradcy w zakresie, należy najpierw *zarejestrować swoją subskrypcję* usłudze Advisor. Subskrypcja jest zarejestrowana przy *właściciela subskrypcji* uruchamia pulpitu nawigacyjnego usługi Advisor i klika pozycję **Uzyskaj zalecenia** przycisk. Jest to *jednorazowa operacja*. Po zarejestrowaniu subskrypcji są dostępne zalecenia doradcy w zakresie jako *właściciela*, *współautora*, lub *czytnika* dla określonego zasobu, grupy zasobów lub subskrypcji.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat zalecenia doradcy w zakresie, zobacz:
* [Wprowadzenie do usługi Advisor](advisor-overview.md)
* [Rozpoczęcie pracy](advisor-get-started.md)
* [Zalecenia doradcy w zakresie wydajności](advisor-cost-recommendations.md)
* [Zalecenia doradcy w zakresie wysokiej dostępności](advisor-cost-recommendations.md)
* [Zalecenia doradcy w zakresie zabezpieczeń](advisor-cost-recommendations.md)
