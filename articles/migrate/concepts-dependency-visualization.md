---
title: "Wizualizacja zależności w migracji Azure | Dokumentacja firmy Microsoft"
description: "Zawiera omówienie obliczeń oceny w usłudze Azure migracji."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 10bc249aa70852dce71c96e46319925cececdd8b
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="dependency-visualization"></a>Wizualizacja zależności

[Migracji Azure](migrate-overview.md) usług ocenia grup lokalnych maszyn do migracji do usługi Azure. Aby zgrupować maszyny, można użyć wizualizacji zależności. Ten artykuł zawiera informacje o tej funkcji.


## <a name="overview"></a>Omówienie

Wizualizacja zależności w Azure migracji służy do tworzenia grup dla migracji oceny o większej pewności. Za pomocą wizualizacji zależności można wyświetlić zależności sieci przez określone maszyny lub dla grupy komputerów. Jest to przydatne w celu zapewnienia, że żadne funkcje lub zostały utracone (lub zapomnienia hasła maszyny) w procesie migracji, gdy aplikacje i obciążenia uruchomione na wielu komputerach.  

## <a name="how-does-it-work"></a>Jak to działa?

Azure używa migracji [mapy usługi](../operations-management-suite/operations-management-suite-service-map.md) rozwiązania [analizy dzienników](../log-analytics/log-analytics-overview.md) dla zależności wizualizacji.
- Podczas tworzenia projektu migracji Azure obszaru roboczego analizy dzienników OMS jest tworzony w ramach subskrypcji.
- Nazwa obszaru roboczego jest nazwą, określ dla projektu migracji, i jest poprzedzony prefiksem **migracji-**i opcjonalnie sufiks z numerem. 
- Przejdź do obszaru roboczego analizy dzienników z **Essentials** sekcji projektu **omówienie** strony.
- Obszar roboczy utworzony zostanie oznaczony przy użyciu klucza **MigrateProject**i wartość **Nazwa projektu**. Te umożliwia wyszukiwanie w portalu Azure.  

    ![Obszar roboczy usługi Log Analytics](./media/concepts-dependency-visualization/oms-workspace.png)

Aby korzystać z wizualizacji zależności, musisz pobrać i zainstalować agentów na każdej maszynie lokalnej, które mają być analizowane.  

## <a name="do-i-need-to-pay-for-it"></a>Należy dokonać jego?

Tak. Obszar roboczy analizy dzienników jest domyślnie tworzona, ale nie jest używana, chyba że w Azure migracji za pomocą wizualizacji zależności. Jeśli jest używany wizualizacji zależności (lub Użyj obszaru roboczego poza Azure migracji), są naliczane opłaty za użycie obszaru roboczego.  [Dowiedz się więcej](https://azure.microsoft.com/pricing/details/insight-analytics/) o cenach rozwiązania mapy usługi. 

## <a name="how-do-i-manage-the-workspace"></a>Jak zarządzać obszaru roboczego?

Można użyć obszaru roboczego analizy dzienników poza Azure migracji. Nie zostanie usunięta po usunięciu projektu migracji, w której został utworzony. Jeśli nie są już potrzebne obszaru roboczego [go usunąć](../log-analytics/log-analytics-manage-access.md) ręcznie.

Nie należy usuwać obszar roboczy utworzony przez Azure migracji, o ile nie usuniesz migracji projektu. Jeśli to zrobisz, zależności nie działają zgodnie z oczekiwaniami.

## <a name="next-steps"></a>Następne kroki

[Grupy maszyn przy użyciu zależności maszyny](how-to-create-group-machine-dependencies.md)