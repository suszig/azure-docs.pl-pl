---
title: Ustawienia elementu Runbook | Dokumentacja firmy Microsoft
description: "W tym artykule opisano ustawienia konfiguracji dla elementu runbook w automatyzacji Azure oraz sposobu zmiany ich za pomocą portalu zarządzania Azure i programu Windows PowerShell."
services: automation
documentationcenter: 
author: georgewallace
manager: stevenka
editor: tysonn
ms.assetid: a726f20c-a952-48b8-88ee-36d76aa3ac61
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: bwren
ms.openlocfilehash: 8d626465bcfdb4efa9cf85c41a2ee679e30eadaf
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="runbook-settings"></a>Ustawienia elementu Runbook
Każdy element runbook automatyzacji Azure ma wiele ustawień, które ułatwiają jego identyfikację i zmianę jego zachowania rejestrowania. Każde z tych ustawień opisano poniżej; dołączono instrukcji na temat sposobu ich modyfikacji.

## <a name="settings"></a>Ustawienia
### <a name="name-and-description"></a>Nazwa i opis
Nie można zmienić nazwy elementu runbook, po jego utworzeniu. Opis jest opcjonalny i może zawierać maksymalnie 512 znaków.

### <a name="tags"></a>Tagi
Znaczniki umożliwiają przypisywanie unikatowych słów i wyrażeń w celu ułatwienia identyfikacji elementu runbook. Na przykład gdy przesłać elementu runbook [galerii programu PowerShell](https://www.powershellgallery.com/), określ określonego znaczniki, aby zidentyfikować elementu runbook powinien być wyświetlany w kategorii. Można określić wiele znaczników elementu runbook, oddzielając je przecinkami.

### <a name="logging"></a>Rejestrowanie
Domyślnie rekordy pełne i postępu nie są zapisywane w historii zadań. Można zmienić ustawienia określonego elementu runbook, aby zapisywać te rekordy. Aby uzyskać więcej informacji o tych rekordów, zobacz [Runbook dane wyjściowe i komunikaty](automation-runbook-output-and-messages.md).

## <a name="changing-runbook-settings"></a>Zmiana ustawień elementu runbook

### <a name="changing-runbook-settings-with-the-azure-portal"></a>Zmiana ustawień elementu runbook w portalu Azure
Możesz zmienić ustawienia dla elementu runbook w portalu Azure z **ustawienia** bloku dla elementu runbook.

1. W portalu Azure wybierz **automatyzacji** , a następnie kliknij nazwę konta automatyzacji.
2. Wybierz **elementów Runbook** kartę.
3. Kliknij nazwę elementu runbook i użytkownik jest kierowany do bloku ustawienia dla elementu runbook. W tym miejscu można określić lub Zmień tagi, opis elementu runbook, konfigurowanie ustawień śledzenia i rejestrowania, a także dostęp do narzędzia obsługi, aby pomóc w rozwiązywaniu problemów.     

### <a name="changing-runbook-settings-with-windows-powershell"></a>Zmiana ustawień elementu runbook przy użyciu programu Windows PowerShell
Można użyć [AzureRmAutomationRunbook zestaw](https://msdn.microsoft.com/library/mt603786.aspx) polecenia cmdlet, aby zmienić ustawienia dla elementu runbook. Jeśli chcesz określić wiele znaczników można albo udostępnić tablica lub jeden ciąg rozdzielany przecinkami wartości do parametru tagów. Możesz uzyskać bieżące znaczniki z [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).

Następujące przykładowe polecenia pokazują, jak można ustawić właściwości dla elementu runbook. W tym przykładzie dodaje trzy znaczniki do istniejących tagów i określa, że powinny być rejestrowane rekordów pełnych.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a>Następne kroki
* Aby dowiedzieć się, jak utworzyć i pobrać dane wyjściowe i komunikaty o błędach z elementów runbook, zobacz [Runbook dane wyjściowe i komunikaty](automation-runbook-output-and-messages.md) 
* Zrozumienie, jak można dodać elementu runbook, który już został opracowany przez społeczność lub inne źródło lub utworzyć własne można znaleźć elementu runbook [Tworzenie lub importowanie elementu Runbook](automation-creating-importing-runbook.md) 

