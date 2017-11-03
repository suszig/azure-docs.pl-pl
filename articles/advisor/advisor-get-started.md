---
title: "Wprowadzenie do usługi Advisor Azure | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do usługi Advisor Azure."
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: makohli
ms.openlocfilehash: a662841bebda460d4225e080f16705b3f16fdc46
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-advisor"></a>Wprowadzenie do usługi Azure Advisor

Dowiedz się, jak dostęp do usługi Advisor za pośrednictwem portalu Azure, Uzyskaj zalecenia, zaimplementować zalecenia, wyszukaj zalecenia i zalecenia dotyczące odświeżania.

## <a name="get-advisor-recommendations"></a>Uzyskuj zalecenia usługi Advisor

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. W okienku po lewej stronie kliknij **więcej usług**.

3. W okienku usługi menu w obszarze **monitorowanie i zarządzanie**, kliknij przycisk **Azure Advisor**.  
 Zostanie wyświetlony pulpit nawigacyjny usługi Advisor.

   ![Klasyfikator Azure dostępu przy użyciu portalu Azure](./media/advisor-overview/advisor-azure-portal-menu.png) 

4. Na pulpicie nawigacyjnym usługi Advisor Wybierz subskrypcję, dla którego chcesz otrzymywać zalecenia.  
Pulpit nawigacyjny usługi Advisor wyświetla spersonalizowane zalecenia dla wybranej subskrypcji. 

5. Aby uzyskać zalecenia dotyczące określonej kategorii, kliknij jedną z kart: **wysokiej dostępności**, **zabezpieczeń**, **wydajności**, lub **koszt**.
 
> [!NOTE]
> Aby uzyskać dostęp do zalecenia doradcy w zakresie, należy najpierw *zarejestrować swoją subskrypcję* usłudze Advisor. Subskrypcja jest zarejestrowana przy *właściciela subskrypcji* uruchamia pulpitu nawigacyjnego usługi Advisor i klika pozycję **Uzyskaj zalecenia** przycisk. Jest to *jednorazowa operacja*. Po zarejestrowaniu subskrypcji są dostępne zalecenia doradcy w zakresie jako *właściciela*, *współautora*, lub *czytnika* dla określonego zasobu, grupy zasobów lub subskrypcji.

  ![Azure Advisor pulpitu nawigacyjnego](./media/advisor-overview/advisor-all-tab.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a>Pobierz szczegóły zalecenia usługi Advisor i wdrożyć rozwiązanie

**Zalecenie** bloku klasyfikatora zawiera dodatkowe informacje na temat zalecenia. 

1. Zaloguj się do [portalu Azure](https://portal.azure.com), a następnie uruchom [Azure Advisor](https://aka.ms/azureadvisordashboard).

2. Na **zalecenia doradcy w zakresie** pulpitu nawigacyjnego, kliknij przycisk **Uzyskaj zalecenia**.

3. Na liście zalecenia kliknij zalecenie, którego chcesz przejrzeć szczegóły.  
**Zalecenie** bloku jest wyświetlany.

4. Na **zalecenia** bloku, przejrzyj informacje o akcjach wykonać, aby rozwiązać potencjalny problem, lub skorzystać z możliwości oszczędności. 
  
  ![W bloku zalecenia usługi Advisor](./media/advisor-overview/advisor-recommendation-action-example.png)

## <a name="search-for-advisor-recommendations"></a>Wyszukaj zalecenia doradcy w zakresie

Możesz wyszukać zalecenia dla określonej grupy zasobów lub subskrypcji. Możesz również wyszukać zaleceń według stanu.

1. Zaloguj się do [portalu Azure](https://portal.azure.com), a następnie uruchom [Azure Advisor](https://aka.ms/azureadvisordashboard).

2. Wyszukaj zaleceń według filtrowania dla subskrypcji, grupy zasobów i stan zalecenie (**Active** lub **Snoozed**).

3. Aby wyświetlić listę zalecenia doradcy w zakresie oparte na kryteria filtru wyszukiwania, kliknij przycisk **Uzyskaj zalecenia**.

  ![Kryteria filtru wyszukiwania usługi Advisor](./media/advisor-get-started/advisor-search.png)

## <a name="snooze-or-dismiss-advisor-recommendations"></a>Odłóż lub odrzucić zalecenia doradcy w zakresie

1. Zaloguj się do [portalu Azure](https://portal.azure.com), a następnie uruchom [Azure Advisor](https://aka.ms/azureadvisordashboard).

2. Kliknij przycisk **Uzyskaj zalecenia**, a następnie na liście zalecenia, kliknij zalecenie.

3. Na **zalecenie** bloku, kliknij przycisk **Odłóż**.  

   ![Przykład działania zalecenia usługi Advisor](./media/advisor-get-started/advisor-snooze.png)

4. Określ włączenia trybu czuwania przedziale czasu, lub wybierz **nigdy** aby odrzucić zalecenia.


## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o usłudze Advisor, zobacz:
* [Wprowadzenie do usługi Advisor Azure](advisor-overview.md)
* [Zalecenia doradcy w zakresie wysokiej dostępności](advisor-high-availability-recommendations.md)
* [Zalecenia doradcy w zakresie zabezpieczeń](advisor-security-recommendations.md)
-  [Zalecenia doradcy w zakresie wydajności](advisor-performance-recommendations.md)
* [Zalecenia doradcy w zakresie koszt](advisor-performance-recommendations.md)
