---
title: "Strumienia Azure dzienników diagnostycznych do analizy dzienników | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak strumienia Azure dzienników diagnostycznych do obszaru roboczego analizy dzienników."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: johnkem
ms.openlocfilehash: 9440bd7f872914887c1f6e50f08a3c273536fcf8
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics"></a>Strumienia Azure dzienników diagnostycznych do analizy dzienników
**[Azure dzienników diagnostycznych](monitoring-overview-of-diagnostic-logs.md)**  przesyłane strumieniowo w najbliższym czasie rzeczywistym za pomocą portalu, poleceń cmdlet programu PowerShell lub interfejsu wiersza polecenia Azure Analiza dzienników Azure.

## <a name="what-you-can-do-with-diagnostics-logs-in-log-analytics"></a>Co można zrobić w diagnostyce loguje analizy dzienników

Analiza dzienników Azure jest narzędziem elastyczne dziennik wyszukiwania i analiza, które pozwala uzyskiwać wgląd w danych pierwotnych dziennika wygenerowanych z zasobów platformy Azure. Niektóre funkcje obejmują:

* **Dziennik wyszukiwania** -zapisu zaawansowane zapytania na danych dziennika, dzienniki korelowanie z różnych źródeł, a nawet Generowanie wykresów, które można przypiąć do pulpitu nawigacyjnego platformy Azure.
* **Alerty** -Wykryj, kiedy jednego lub wielu zdarzeń pasuje do określonego zapytania i stają się powiadomienia za pomocą poczty e-mail lub elementu webhook wywołania.
* **Rozwiązania** -użyć wbudowanych widoki i pulpity nawigacyjne, które zapewniają błyskawiczny wgląd w dane dziennika.
* **Zaawansowane analizy** — zastosować uczenia maszynowego i wzorca zgodnego algorytmy umożliwiające identyfikowanie możliwe problemy ujawnionych w dziennikach.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics"></a>Przesyłania strumieniowego dzienników diagnostycznych do analizy dzienników
Można włączyć przesyłania strumieniowego dzienników diagnostycznych programowo, w portalu lub przy użyciu [interfejsów API REST Monitor Azure](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings). W obu przypadkach należy utworzyć ustawienie diagnostyczne, w którym można określić obszaru roboczego analizy dzienników i kategorie dziennika i metryki, aby wysłać do tego obszaru roboczego. Diagnostyka **kategorii dziennika** jest typem dziennika, która może zapewnić zasobu.

Obszar roboczy analizy dzienników nie musi znajdować się w tej samej subskrypcji co zasób emitowanie dzienniki, dopóki użytkownik, który konfiguruje ustawienia ma odpowiedni dostęp RBAC do obu subskrypcji.

## <a name="stream-diagnostic-logs-using-the-portal"></a>Strumieniowe przesyłanie dzienników diagnostycznych przy użyciu portalu
1. W portalu przejdź do monitora Azure i kliknij na **ustawień diagnostycznych**

    ![Monitorowanie sekcji Azure Monitor](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-blade.png)

2. Opcjonalnie Filtruj listę według grupy zasobów lub typ zasobu, a następnie kliknij na zasobie, dla której chcesz skonfigurować ustawienie diagnostyczne.

3. Jeśli nie istnieją żadne ustawienia zasobu wybrany, zostanie wyświetlony monit, aby utworzyć ustawienie. Kliknij pozycję "Włącz diagnostykę."

   ![Dodaj ustawienie diagnostyczne - żadnych istniejących ustawień](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-none.png)

   W przypadku istniejących ustawień na zasobie, zostanie wyświetlona lista ustawień już skonfigurowana dla tego zasobu. Kliknij przycisk "Dodaj ustawienie diagnostyczne".

   ![Dodaj ustawienie diagnostyczne — istniejące ustawienia](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-multiple.png)

3. Nadaj nazwę ustawienia i pole wyboru dla **wysyłać do analizy dzienników**, następnie wybierz obszar roboczy analizy dzienników.
   
   ![Dodaj ustawienie diagnostyczne — istniejące ustawienia](media/monitoring-stream-diagnostic-logs-to-event-hubs/diagnostic-settings-configure.png)

4. Kliknij pozycję **Zapisz**.

Po kilku chwilach nowe ustawienie jest wyświetlane na liście ustawień dla tego zasobu, a dzienników diagnostycznych są przesyłane strumieniowo do tego obszaru roboczego zaraz po wygenerowaniu nowych danych zdarzenia. Należy pamiętać, że mogą być do piętnastu minut między gdy zdarzenie jest emitowany i po wyświetleniu w analizy dzienników.

### <a name="via-powershell-cmdlets"></a>Za pomocą poleceń cmdlet programu PowerShell
Przesyłania strumieniowego za pośrednictwem [poleceń cmdlet programu PowerShell Azure](insights-powershell-samples.md), można użyć `Set-AzureRmDiagnosticSetting` polecenia cmdlet z następującymi parametrami:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Należy pamiętać, że właściwość workspaceID przyjmuje identyfikator zasobu pełnej Azure obszaru roboczego nie obszaru roboczego identyfikator/klucz wyświetlane w portalu usługi Analiza dzienników.

### <a name="via-azure-cli"></a>Za pomocą interfejsu wiersza polecenia platformy Azure
Do przesyłania strumieniowego za pośrednictwem [interfejsu wiersza polecenia Azure](insights-cli-samples.md), można użyć `insights diagnostic set` polecenia w następujący sposób:

```azurecli
azure insights diagnostic set --resourceId <resourceID> --workspaceId <workspace resource ID> --categories <list of categories> --enabled true
```

Należy pamiętać, że właściwość workspaceId przyjmuje identyfikator zasobu pełnej Azure obszaru roboczego nie obszaru roboczego identyfikator/klucz wyświetlane w portalu usługi Analiza dzienników.

## <a name="how-do-i-query-the-data-in-log-analytics"></a>Jak zbadać danych analizy dzienników?

W bloku dziennika wyszukiwania w portalu lub analityka zaawansowane środowisko w ramach analizy dzienników jako część rozwiązania do zarządzania dziennika w tabeli AzureDiagnostics można badać dzienników diagnostycznych. Dostępne są także [kilka rozwiązań dla zasobów Azure](../log-analytics/log-analytics-add-solutions.md) można zainstalować, aby uzyskać błyskawiczny wgląd w dane dziennika są wysyłane do analizy dzienników.


## <a name="next-steps"></a>Następne kroki
* [Więcej informacji na temat dzienników diagnostycznych platformy Azure](monitoring-overview-of-diagnostic-logs.md)
