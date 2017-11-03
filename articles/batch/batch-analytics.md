---
title: "Partia zadań Azure Analytics | Dokumentacja firmy Microsoft"
description: "Odwołanie do analizy partii zadań Azure."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 7d634e1bb595a84b8af339e5bc5a483a7849e7f7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="batch-analytics"></a>Analizach wsadowych
Tematy w analizach wsadowych zawierają informacje dotyczące zdarzenia i alerty dostępne dla zasobów usługi partii.

Zobacz [rejestrowania diagnostycznego partii zadań Azure](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) Aby uzyskać więcej informacji o włączaniu i korzystanie z dzienników diagnostycznych partii.

## <a name="diagnostic-logs"></a>Dzienniki diagnostyczne

Usługa partia zadań Azure emituje następujące zdarzenia dziennika diagnostycznego przez cały okres istnienia niektórych zasobów partii.

**Usługa dziennika zdarzeń**
* [Tworzenie puli](batch-pool-create-event.md)
* [Start usuwania puli](batch-pool-delete-start-event.md)
* [Usuwanie puli ukończone](batch-pool-delete-complete-event.md)
* [Początkowy rozmiar puli](batch-pool-resize-start-event.md)
* [Zmiana rozmiaru puli ukończone](batch-pool-resize-complete-event.md)
* [Rozpoczęcia zadania](batch-task-start-event.md)
* [Zadania ukończone](batch-task-complete-event.md)
* [Niepowodzenie zadania](batch-task-fail-event.md)