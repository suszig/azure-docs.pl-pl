---
title: "Wydajność i skalę w funkcjach trwałe - Azure"
description: "Wprowadzenie do funkcji trwałe rozszerzenie dla usługi Azure Functions."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: cc4c643b8d0e8de1b5c38ca7bb1b0193d6b0f05b
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Wydajność i skalę w funkcjach trwałe (usługi Azure Functions)

Aby zoptymalizować wydajność i skalowalność, ważne jest, aby zrozumieć skalowania unikatowość tej [trwałe funkcji](durable-functions-overview.md).

Aby zrozumieć zachowania skali, należy zapoznać się szczegóły podstawowego dostawcy usługi Azure Storage przez funkcje trwałe.

## <a name="history-table"></a>Tabela historii

Historia jest tabela usługi Azure Storage, zawierający zdarzenia historii dla wszystkich wystąpień aranżacji. Jak uruchomić wystąpień, nowe wiersze zostaną dodane do tej tabeli. Klucz partycji tabeli pochodzi z Identyfikatora wystąpienia orchestration. Te wartości są losowe w większości przypadków, co zapewnia optymalne dystrybucji wewnętrzny partycji w usłudze Azure Storage.

## <a name="internal-queue-triggers"></a>Wyzwalacze kolejki wewnętrznej

Funkcje programu orchestrator i funkcje działania są oba wyzwalane przez wewnętrzne kolejki w aplikacji funkcji domyślne konto magazynu. Istnieją dwa typy kolejek w funkcjach trwałe: **kolejki kontroli** i **kolejki elementu roboczego**.

### <a name="the-work-item-queue"></a>Kolejki elementu roboczego

Brak jednego elementu roboczego kolejki na Centrum zadania w funkcje trwałe. To jest podstawowa kolejki i działa podobnie jak każdy inny `queueTrigger` kolejki w funkcji platformy Azure. Kolejka ta służy do wyzwalania bezstanowych *działania funkcji*. Gdy aplikacji funkcji trwałe skaluje się do wielu maszyn wirtualnych, wszystkie te maszyny wirtualne konkurować uzyskanie pracy z kolejki elementu roboczego.

### <a name="control-queues"></a>Formant kolejek

*Kolejki kontroli* jest bardziej złożone niż prostsze kolejki elementu roboczego. Służy do wyzwalania funkcje stanowe orchestrator. Ponieważ wystąpień funkcji programu orchestrator są stanowe pojedynczych, nie jest możliwość użycia modelu konkurujących konsumentów rozłożenie obciążenia na maszynach wirtualnych. Zamiast tego wiadomości programu orchestrator są równoważenia obciążenia wielu kolejek formantu. Więcej informacji na ten w kolejnych sekcjach.

Formant kolejki zawierają różne typy komunikatów cyklu życia aranżacji. Przykłady obejmują [komunikatów formantu orchestrator](durable-functions-instance-management.md), działania funkcji *odpowiedzi* komunikatów i komunikatów czasomierza.

## <a name="orchestrator-scale-out"></a>Orchestrator skalowalnego w poziomie

Działanie funkcji są bezstanowych i automatycznie skalować w poziomie przez dodanie maszyn wirtualnych. Funkcje programu orchestrator z drugiej strony są *partycjonowanej* przez jedną lub więcej kolejek formantu. Liczba kolejek kontroli jest stała i nie można zmienić po rozpoczęciu tworzenia obciążenia.

W trakcie skalowania do wielu wystąpień hosta — funkcja (zwykle na różnych maszyn wirtualnych), każde wystąpienie uzyskuje blokady na jednym z kolejek formantu. Ta blokada zapewnia, że wystąpienie aranżacji tylko działa na jednej maszynie Wirtualnej w czasie. Oznacza to, że jeśli koncentrator zadań jest skonfigurowany z trzech kolejek kontroli, wystąpień orchestration można równoważeniem obciążenia na maszynach wirtualnych trzech. Aby zwiększyć pojemność dla wykonania działania funkcji można dodać dodatkowe maszyny wirtualne.  Ale dodatkowe zasoby nie będą używane do uruchamiania funkcji programu orchestrator.

Na poniższym diagramie przedstawiono, jak hosta usługi Azure Functions współdziała z jednostek magazynu w środowisku skalowanej.

![Diagram skali](media/durable-functions-perf-and-scale/scale-diagram.png)

Jak widać, wszystkie maszyny wirtualne mogą konkurować wiadomości w kolejce elementu roboczego. Jednak tylko trzech maszyn wirtualnych można uzyskać komunikaty z kolejek kontroli i każdej maszyny Wirtualnej blokuje kolejki jeden formant.

Wystąpień aranżacji są dystrybuowane między wystąpieniami kolejki kontroli przez uruchomienie funkcji skrótu wewnętrznego dla identyfikatora wystąpienia aranżacji. Identyfikatory wystąpienia są generowane automatycznie i losowe domyślnie zapewniający równoważy wystąpień wszystkich kolejek dostępne formantu. Bieżący domyślny numer partycji kolejki obsługiwanych kontroli to **4**.

> [!NOTE]
> Nie jest obecnie można skonfigurować liczby partycji kolejki kontroli w funkcji platformy Azure. [Pracy do obsługi tej opcji konfiguracji jest śledzony](https://github.com/Azure/azure-functions-durable-extension/issues/73).

Ogólnie rzecz biorąc funkcje programu orchestrator powinny być lekkie i nie będą dużo mocy obliczeniowej. Z tego powodu nie jest konieczne tworzenie dużej liczby partycji kolejki sterowania można uzyskać doskonałe przepływności. Zamiast większość pracy duże odbywa się w funkcji bezstanowych działania, które może być skalowana w poziomie nieograniczonej.

## <a name="auto-scale"></a>Automatyczne skalowanie

Zgodnie z systemem w planie zużycie wszystkich funkcji Azure, trwałe funkcje obsługi automatycznego skalowania za pomocą [kontrolera skali usługi Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-scale#runtime-scaling). Kontroler skali monitoruje długość kolejki elementów pracy oraz wszystkich kolejek kontroli Dodawanie lub usuwanie odpowiednio wystąpień maszyn wirtualnych. Jeśli długości kolejki kontroli rosną wraz z upływem czasu, kontrolera skali będzie kontynuować dodawanie wystąpień maszyn wirtualnych, dopóki nie osiągnie liczba partycji kolejki formantu. Jeśli długości kolejki elementu roboczego rosną wraz z upływem czasu, kontrolera skali będzie kontynuować dodawanie wystąpień maszyn wirtualnych, dopóki nie może dopasować obciążenia, niezależnie od liczby partycji kolejki formantu.

## <a name="thread-usage"></a>Użycie wątku

Funkcje programu orchestrator są wykonywane w jednym wątku. Jest to wymagane, aby upewnić się, czy deterministyczne jest wykonanie funkcji programu orchestrator. Pamiętając o tym należy koniecznie nigdy nie Zachowaj orchestrator funkcji wątku niepotrzebnie zajęty z zadań, takich jak We/Wy (która jest zabroniona dla z różnych powodów), blokowania lub wirowania operacji. Pracę, co może wymagać we/wy, blokowanie, lub wiele wątków powinna zostać przeniesiona do działania funkcji.

Działanie funkcji ma te same zachowania jako regularne funkcji wyzwalanych przez kolejkę. Oznacza to, bezpiecznie można wykonać operacji We/Wy, wykonać operacje o znacznym wykorzystaniu procesora CPU i użycie wiele wątków. Ponieważ działania wyzwalaczy bezstanowych, ich za darmo skalować w poziomie do niepowiązanego liczbę maszyn wirtualnych.

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Zainstaluj rozszerzenie funkcji trwałe i przykłady](durable-functions-install.md)
