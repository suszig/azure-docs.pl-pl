---
title: "Tworzenie wizualizacji i rozwiązywanie problemów z zadania usługi analiza strumienia | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wizualizacji potok zadania usługi analiza strumienia samoobsługi Rozwiązywanie problemów przy użyciu funkcji diagram diagnostyki."
keywords: 
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: d87841cd-c59f-4a46-b46e-8b904fdc12e9
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 820b73a5dbf9bb108e189313cf6ee2b924ab04c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Tworzenie wizualizacji i rozwiązywanie problemów z zadania usługi analiza strumienia
W Stream Analytics podobnie jak w przypadku innych technologii chmurowych, rozwiązywanie problemów z jest czasami potrzebne do dlaczego zadania nie tworzy oczekiwane dane wyjściowe (lub żadnych danych wyjściowych istotnego dla badania). Dzięki koncepcji na uwadze Stream Analytics zapewnia możliwość do wizualizacji zadanie przesyłania strumieniowego. To jest również przydatne jako narzędzia do modelowania i rozwiązanie po stronie tych wymagające dokumentacji ich pracy.

W panelu wizualizacji dane wejściowe są widoczne, a także zapytania i następnie wszystkie dane wyjściowe skonfigurowane. Problemy z łącznością lub konfiguracją może okazać się bardziej i może być również przydatne wyświetlić graficzną reprezentację danej konfiguracji.

## <a name="using-the-diagnosis-diagram-tool"></a>Za pomocą narzędzia diagram diagnostyki
Aby uzyskać dostęp do tego wizualizatora, po prostu kliknij przycisk "Diagnostyki diagram" w obszarze "Ustawienia" w zadaniu Stream Analytics.

![Stream-Analytics-Troubleshoot-visualization-Diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Co wejściowa i wyjściowa jest zaznaczony kolorami wskazująca bieżący stan tego składnika, jak pokazano w następujący sposób.

![Stream-Analytics-Troubleshoot-visualization-Input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Gdy użytkownik chce przyjrzeć się pośrednie zapytania kroki, aby zrozumieć wzorce przepływu danych wewnątrz zadania, narzędzia wizualizacji udostępnia widok podział zapytania do jej kroków składnika i sekwencja przepływu. Kliknięcie każdego kroku zapytania zawiera odpowiedniej sekcji w zapytaniu edycji okienku zgodnie z opisami. 

![Stream-Analytics-Troubleshoot-visualization-Intermediate-Steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do usługi Azure Stream Analytics](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

