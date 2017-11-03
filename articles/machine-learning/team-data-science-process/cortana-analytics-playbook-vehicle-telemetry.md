---
title: "Przewidywanie vehicle kondycji i wspierającym zwyczaje - Azure | Dokumentacja firmy Microsoft"
description: "Korzystanie z możliwości Cortana Intelligence, aby uzyskać wgląd w czasie rzeczywistym oraz predykcyjnej na vehicle kondycji i wspierającym zwyczaje."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: af8b3d5bf891c93c30a05c5f02d86639a466dde5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Podręcznik dotyczący rozwiązań analizy telemetrii pojazdów
To **menu** łącza do rozdziały tego podręcznika dotyczącego. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Omówienie
Komputery Super został przeniesiony poza laboratorium i są teraz stoi w naszym garaż! Tych samochodów najnowocześniejsze zawierać większą liczbą czujników, zapewniając im możliwość śledzenie i monitorowanie miliony zdarzeń w ciągu sekundy. Oczekuje się, że 2020, większość tych samochodów będzie mieć został połączony z Internetem. Wyobraź sobie naciśnięcie przycisku w tym wiele danych, aby zapewnić większe bezpieczeństwo, niezawodność i pobudzenie lepsze środowisko! Firma Microsoft wprowadziła to marzy rzeczywistością z Cortana Intelligence.

Firmy Microsoft Cortana Intelligence jest pełni zarządzanych danych big data oraz pakiet zaawansowane metody analizy, który pozwala na przekształcanie danych do inteligentnego akcji. Chcemy przedstawiono szablon rozwiązania analizy Telemetrii Cortana analizy pojazdów. To rozwiązanie pokazano, jak przedstawicielstw samochodu handlowych, producentów samochodów i ubezpieczeń można używać funkcji Cortana Intelligence w celu uzyskania w czasie rzeczywistym i zwyczaje predykcyjnej rozeznanie vehicle kondycji i wspierającym. 

Rozwiązanie jest zaimplementowany jako [wzorzec architektury lambda](https://en.wikipedia.org/wiki/Lambda_architecture) przedstawiający potencjalnych platformy Cortana Intelligence pełnego w czasie rzeczywistym i przetwarzania wsadowego. Rozwiązanie: 

* udostępnia symulatora telematyki Vehicle
* korzysta z usługi Event Hubs służy do wprowadzania miliony zdarzeń telemetrii symulowane vehicle na platformie Azure 
* używa Stream Analytics, aby uzyskać wgląd w czasie rzeczywistym w kondycję vehicle
* utrzymuje dane do długoterminowego przechowywania dla bardziej zaawansowane funkcje analizy partii. 
* korzysta z uczenia maszynowego wykrywania anomalii w czasie rzeczywistym i partii przetwarzania, aby uzyskać wgląd predykcyjnej.
* wykorzystuje HDInsight do transformacji danych na dużą skalę i fabryki danych do obsługi aranżacji, planowania, zarządzanie zasobami i monitorowania w potoku przetwarzania wsadowego 
* daje to rozwiązanie sformatowanego pulpitu nawigacyjnego w czasie rzeczywistym danych i wizualizacji analizy predykcyjnej przy użyciu usługi Power BI

## <a name="architecture"></a>Architektura
![Diagram architektury rozwiązania](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*rysunek 1 — Architektura rozwiązania analizy Telemetrii Vehicle*

To rozwiązanie obejmuje następujące elementy **Cortana Intelligence składniki** i ich integracji kompleksowe ilustrację:

* **Centra zdarzeń** służy do wprowadzania miliony zdarzeń telemetrii vehicle na platformie Azure.
* **Analiza strumienia** do uzyskania wgląd w czasie rzeczywistym w kondycję vehicle i będzie się powtarzał tych danych do długoterminowego przechowywania dla bardziej zaawansowane funkcje analizy partii.
* **Machine Learning** wykrywania anomalii w czasie rzeczywistym i przetwarzanie wsadowe, aby uzyskać wgląd predykcyjnej.
* **HDInsight** jest wykorzystywana do przekształcania danych na dużą skalę
* **Fabryka danych** obsługuje aranżacji, Planowanie zarządzania zasobami i monitorowania w potoku przetwarzania wsadowego.
* **Power BI** daje to rozwiązanie sformatowanego pulpitu nawigacyjnego w czasie rzeczywistym danych i wizualizacji analizy predykcyjnej.

To rozwiązanie uzyskuje dostęp do dwóch różnych **źródeł danych**: 

* **Symulowane sygnały vehicle i informacji diagnostycznych**: symulatora telematyki vehicle emituje informacje diagnostyczne i sygnalizuje, że odpowiada do stanu mechanizm i kierowania wzorca w danym punkcie w czasie. 
* **Vehicle katalogu**: odwołanie do zestawu danych zawierającego VIN do mapowania modelu.

