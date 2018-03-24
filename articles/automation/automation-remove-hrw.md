---
title: Usuwanie hybrydowych procesów roboczych elementu runbook usługi Azure Automation
description: Ten artykuł zawiera informacje na temat zarządzania wdrożonej Azure automatyzacji hybrydowe procesy robocze elementu Runbook, który służy do uruchamiania elementów runbook na komputerach w środowisku lokalnym centrum danych lub w chmurze.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 0b8f951bbd9712e3d20c3286ef3571e287499c68
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="remove-azure-automation-hybrid-runbook-workers"></a>Usuwanie hybrydowych procesów roboczych elementu runbook usługi Azure Automation

Funkcja hybrydowy proces roboczy elementu Runbook usługi Automatyzacja Azure umożliwia uruchamianie elementów runbook, bezpośrednio na komputerze hostującym rolę i względem zasobów w środowisku, aby zarządzać tymi zasobów lokalnych. W tym artykule przeprowadza użytkownika przez kroki, aby usunąć hybrydowy proces roboczy z komputera lokalnego.

## <a name="removing-hybrid-runbook-worker"></a>Usuwanie hybrydowy proces roboczy elementu Runbook

Co najmniej jeden hybrydowymi elementami roboczymi Runbook można usunąć z grupy lub usunąć grupy, w zależności od wymagań.  Aby usunąć hybrydowy proces roboczy elementu Runbook z komputera lokalnego, wykonaj następujące kroki.

1. W portalu Azure przejdź do swojego konta automatyzacji.  
2. Z **ustawienia** bloku, wybierz opcję **klucze** i zanotuj wartości dla pola **adres URL** i **podstawowy klucz dostępu**.  Te informacje są niezbędne do następnego kroku.
3. Otwórz sesję programu PowerShell w trybie administratora i uruchom następujące polecenie: `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Użyj **-Verbose** przełączać szczegółowy dziennik proces usuwania.

> [!NOTE]
> Nie powoduje usunięcia programu Microsoft Monitoring Agent z komputera, tylko funkcje i konfiguracji roli hybrydowy proces roboczy elementu Runbook.  

## <a name="remove-hybrid-worker-groups"></a>Usuń grupy hybrydowego procesu roboczego

Aby usunąć grupę, należy najpierw usunąć hybrydowy proces roboczy elementu Runbook z każdego komputera, który jest członkiem grupy, korzystając z procedury przedstawionej wcześniej, a następnie wykonaj następujące kroki, aby usunąć grupę.  

1. Otwórz konto usługi Automatyzacja w portalu Azure.
1. W obszarze **automatyzacji procesu** wybierz **hybrydowego procesu roboczego grupy**. Wybierz grupę, którą chcesz usunąć.  Po wybraniu określonej grupy, **grupy hybrydowych procesów roboczych** bloku właściwości jest wyświetlany.<br> ![Blok grupy hybrydowych procesów roboczych elementu Runbook](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
2. W bloku właściwości wybranej grupy, kliknij **usunąć**.  Zostanie wyświetlony komunikat prośbą o potwierdzenie tej akcji, wybierz **tak** czy na pewno chcesz kontynuować.<br> ![Okno dialogowe potwierdzenia usunięcia grupy](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Ten proces może potrwać kilka sekund. Możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu. 

## <a name="next-steps"></a>Następne kroki

* Przegląd [uruchamiania elementów runbook na hybrydowy proces roboczy elementu Runbook](automation-hrw-run-runbooks.md) Aby dowiedzieć się, jak skonfigurować elementy runbook do automatyzacji procesów w lokalnym centrum danych lub w innym środowisku chmury.
* Aby uzyskać informacje na temat instalowania systemu Windows hybrydowymi elementami roboczymi Runbook, zobacz [procesu roboczego Runbook hybrydowego Windows automatyzacji Azure](automation-windows-hrw-install.md).
* Aby uzyskać informacje na temat instalowania systemu Linux hybrydowymi elementami roboczymi Runbook, zobacz [procesu roboczego Runbook hybrydowego Linux automatyzacji Azure](automation-linux-hrw-install.md).