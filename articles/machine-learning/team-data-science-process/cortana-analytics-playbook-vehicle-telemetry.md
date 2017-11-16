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
ms.openlocfilehash: bec5066a2e1ba0e4e5e81c4e1be28ed8eb93ceed
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Vehicle podręcznikowym rozwiązania analizy Telemetrii
Tego łącza menu rozdziałów, w tym podręcznika dotyczącego: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Omówienie
Superkomputerów został przeniesiony poza laboratorium i są teraz stoi w garaży. Tych samochodów najnowocześniejsze zawiera wiele czujników, które pozwalają im na śledzenie i monitorowanie miliony zdarzeń w ciągu sekundy. 2020 większość tych pojazdów zostanie połączony z Internetem. Naciskając pozycję w tym wiele danych może zapewnić większe bezpieczeństwo, niezawodność i pobudzenie lepsze środowisko. Firma Microsoft udostępnia to marzy rzeczywistością z Cortana Intelligence.

Cortana Intelligence jest pełni zarządzanych danych big data oraz pakiet zaawansowane metody analizy, który służy do przekształcania danych inteligentnego akcji. Szablon rozwiązania analizy Telemetrii Cortana analizy Vehicle pokazano, jak przedstawicielstw samochodu handlowych, producentów samochodów i ubezpieczeń można uzyskać w czasie rzeczywistym i zwyczaje predykcyjnej rozeznanie vehicle kondycji i wspierającym. 

Rozwiązanie jest zaimplementowany jako [wzorzec architektury lambda](https://en.wikipedia.org/wiki/Lambda_architecture), który wskazuje pełny potencjalnych platformy Cortana Intelligence w czasie rzeczywistym i przetwarzania wsadowego.

## <a name="architecture"></a>Architektura
Na tym diagramie przedstawiono architekturę rozwiązania analizy Telemetrii Vehicle:

![Diagram architektury rozwiązania](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


To rozwiązanie obejmuje następujące składniki Cortana Intelligence i ich integracji end-to-end ilustrację:

* **Usługa Azure Event Hubs** wysyła strumień miliony zdarzeń telemetrii vehicle na platformie Azure.
* **Usługa Azure Stream Analytics** zapewnia wgląd w czasie rzeczywistym w kondycję vehicle i będzie się powtarzał tych danych do długoterminowego przechowywania dla bardziej zaawansowane funkcje analizy partii.
* **Usługa Azure Machine Learning** wykrywa anomalii w czasie rzeczywistym i używa przetwarzania wsadowego do wgląd predykcyjnej.
* **Usługa Azure HDInsight** przekształcenia danych na dużą skalę.
* **Fabryka danych Azure** obsługuje aranżacji, planowania, zarządzanie zasobami i monitorowania w potoku przetwarzania wsadowego.
* **Power BI** daje to rozwiązanie sformatowanego pulpitu nawigacyjnego w czasie rzeczywistym danych i wizualizacji analizy predykcyjnej.

To rozwiązanie uzyskuje dostęp do dwóch różnych źródeł danych: 

* **Symulowane sygnały vehicle i informacji diagnostycznych**: symulatora telematyki vehicle emituje informacje diagnostyczne i sygnalizuje, że odpowiada do stanu mechanizm i kierowania wzorca w danym punkcie w czasie. 
* **Vehicle katalogu**: ten zestaw danych odwołania mapuje VINs modeli.

