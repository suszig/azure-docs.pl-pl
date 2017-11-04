---
title: Zmienne systemowe w fabryce danych Azure | Dokumentacja firmy Microsoft
description: "W tym artykule opisano zmienne systemowe obsługiwane przez usługi fabryka danych Azure. Te zmienne można użyć w wyrażeniach podczas definiowania jednostek fabryki danych."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: shlo
ms.openlocfilehash: 274c003e0697ba08d010c3bf13724461a4b624ee
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2017
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Fabryka danych Azure są obsługiwane zmienne systemu
W tym artykule opisano zmienne systemowe obsługiwane przez usługi fabryka danych Azure. Te zmienne można użyć w wyrażeniach podczas definiowania jednostek fabryki danych. 

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [funkcje i zmienne w fabryce danych w wersji 1](v1/data-factory-functions-variables.md).


## <a name="pipeline-scope"></a>Zakres potoku:

| Nazwa zmiennej | Opis |
| --- | --- |
| @pipeline(). DataFactory |Nazwa fabryki danych potoku Uruchom działa w ramach | 
| @pipeline(). Potoku |Nazwa potoku |
| @pipeline(). Przebiegu | Identyfikator określonego procesu, uruchom | 
| @pipeline(). TriggerType | Typ wyzwalacza, który wywołał potoku (ręczne, harmonogram) | 
| @pipeline(). Identyfikator_wyzwalacza| Identyfikator wyzwalacza, który wywołuje potoku |
| @pipeline(). Nazwa_wyzwalacza| Nazwa wyzwalacza, który wywołuje potoku |
| @pipeline(). TriggerTime| Godzinę wyzwalacz, który wywołał potoku. Czas wyzwalacza jest bieżącą godzinę wypalane nie zaplanowanym terminie. Na przykład `13:20:08.0149599Z` jest zwracana zamiast`13:20:00.00Z` |

## <a name="trigger-scope"></a>Zakres wyzwalacz:

| Nazwa zmiennej | Opis |
| --- | --- |
| Wyzwalanie .scheduledTime) |Czasem wyzwalacz został zaplanowanego wywoływanie potoku uruchamiania. Na przykład dla wyzwalacza generowane co 5 minut, ta zmienna zwróci `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z`, `2017-06-01T22:29:00Z` odpowiednio.|
| Wyzwalanie .startTime) |Godzinę wyzwalacza **faktycznie** uruchamiany wywoływanie potoku uruchamiania. Na przykład dla wyzwalacza generowane co 5 minut, ta zmienna może zwrócić przypominać `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z`, `2017-06-01T22:29:00.9935483Z` odpowiednio.|

## <a name="next-steps"></a>Następne kroki
Aby uzyskać informacje o używaniu tych zmiennych w wyrażeniach, zobacz [język wyrażeń & funkcje](control-flow-expression-language-functions.md). 
