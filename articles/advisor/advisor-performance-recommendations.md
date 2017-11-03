---
title: "Azure zalecenia usługi Advisor wydajności | Dokumentacja firmy Microsoft"
description: "Użyj usługi Advisor w celu zoptymalizowania wydajności wdrożeń platformy Azure."
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
ms.openlocfilehash: 995a1f37a3fd68b39c14a95d46109c0f7814018d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="advisor-performance-recommendations"></a>Zalecenia doradcy w zakresie wydajności

Azure zalecenia wydajności doradcy w zakresie zwiększyć szybkość i czas odpowiedzi aplikacji biznesowych o znaczeniu krytycznym. Zalecenia dotyczące wydajności z usługi Advisor można uzyskać **wydajności** pulpitu nawigacyjnego usługi Advisor.

![Karta wydajność usługi Advisor](./media/advisor-performance-recommendations/advisor-performance-tab.png)

## <a name="improve-database-performance-with-sql-db-advisor"></a>Poprawić wydajność bazy danych w usłudze Advisor bazy danych SQL

Advisor zapewnia spójne, skonsolidowanego widoku zaleceń dla wszystkich zasobów na platformie Azure. Umożliwia integrację z Doradcę bazy danych SQL, aby wyświetlić zalecenia dotyczące poprawy wydajności bazy danych SQL Azure. Doradca bazy danych programu SQL ocenia wydajności baz danych SQL Azure, analizując Twojej historii użycia. Oferuje zaleceń, które są najbardziej odpowiednie do uruchamiania typowych zadań bazy danych. 

> [!NOTE]
> Aby uzyskać zalecenia, bazy danych musi mieć o tydzień użycia, a w ciągu tygodnia musi być pewne spójnej działania. Doradca bazy danych SQL można zoptymalizować łatwiej wzorców zapytania spójna niż dla losowych seria działań.

Aby uzyskać więcej informacji o usłudze Advisor bazy danych SQL, zobacz [doradcy bazy danych SQL](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).

![Zalecenia dotyczące bazy danych SQL](./media/advisor-performance-recommendations/advisor-performance-sql.png)

## <a name="improve-redis-cache-performance-and-reliability"></a>Zwiększyć wydajność pamięci podręcznej Redis i niezawodności

Klasyfikator identyfikuje wystąpienia pamięci podręcznej Redis, gdzie mogą być niekorzystny wpływ na wydajność wysokie użycie pamięci, obciążenie serwera, przepustowości sieci lub dużej liczby połączeń klientów. Klasyfikator także najlepsze rozwiązania w zakresie zalecenia, aby uniknąć potencjalnych problemów. Aby uzyskać więcej informacji o pamięci podręcznej Redis, zobacz [Advisor pamięci podręcznej Redis](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>Zwiększyć wydajność aplikacji usługi i niezawodności

Klasyfikator Azure integruje się poniżej rekomendowane najlepsze rozwiązania dla poprawy środowiska usługi aplikacji i wykrywania możliwości odpowiednie platformy. Przykłady zalecenia usługi aplikacji:
* Wykrywanie wystąpień, w którym na wyczerpaniu pamięci lub zasobów procesora CPU przez środowisk uruchomieniowych aplikacji z opcjami środki zaradcze.
* Wykrywanie wystąpień, w którym collocating zasoby, takie jak aplikacje sieci web i baz danych można zwiększyć wydajność i tańsze. 

Aby uzyskać więcej informacji na temat zalecenia usługi aplikacji, zobacz [najlepsze rozwiązania dotyczące usługi Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).
![Zalecenia dotyczące usług aplikacji](./media/advisor-performance-recommendations/advisor-performance-app-service.png)

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Jak uzyskać dostęp zalecenia dotyczące wydajności w usługi Advisor

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. W okienku po lewej stronie kliknij **więcej usług**.

3. W okienku usługi menu w obszarze **monitorowanie i zarządzanie**, kliknij przycisk **Azure Advisor**.  
 Zostanie wyświetlony pulpit nawigacyjny usługi Advisor.

4. Na pulpicie nawigacyjnym usługi Advisor, kliknij przycisk **wydajności** kartę.

5. Wybierz subskrypcję, dla którego chcesz otrzymywać zalecenia, a następnie kliknij przycisk **Uzyskaj zalecenia**.

> [!NOTE]
> Aby uzyskać dostęp do zalecenia doradcy w zakresie, należy najpierw *zarejestrować swoją subskrypcję* usłudze Advisor. Subskrypcja jest zarejestrowana przy *właściciela subskrypcji* uruchamia pulpitu nawigacyjnego usługi Advisor i klika pozycję **Uzyskaj zalecenia** przycisk. Jest to *jednorazowa operacja*. Po zarejestrowaniu subskrypcji są dostępne zalecenia doradcy w zakresie jako *właściciela*, *współautora*, lub *czytnika* dla określonego zasobu, grupy zasobów lub subskrypcji.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat zalecenia doradcy w zakresie, zobacz:

* [Wprowadzenie do usługi Advisor](advisor-overview.md)
* [Wprowadzenie do usługi Advisor](advisor-get-started.md)
* [Zalecenia doradcy w zakresie koszt](advisor-performance-recommendations.md)
* [Zalecenia doradcy w zakresie wysokiej dostępności](advisor-high-availability-recommendations.md)
* [Zalecenia doradcy w zakresie zabezpieczeń](advisor-security-recommendations.md)

