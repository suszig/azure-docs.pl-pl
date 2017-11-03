---
title: "Usuń hybrydowymi elementami roboczymi Runbook usługi Automatyzacja Azure | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera informacje na temat zarządzania wdrożonej Azure automatyzacji hybrydowe procesy robocze elementu Runbook, który służy do uruchamiania elementów runbook na komputerach w środowisku lokalnym centrum danych lub w chmurze."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte
ms.openlocfilehash: 68031ba2ce5c47870c3e60dc469dcda2971b4d87
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="remove-azure-automation-hybrid-runbook-workers"></a>Usuń hybrydowymi elementami roboczymi Runbook usługi Automatyzacja Azure

Można 

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
1. Wybierz **hybrydowego procesu roboczego grupy** kafelka w **hybrydowego procesu roboczego grupy** bloku, wybierz grupę, którą chcesz usunąć.  Po wybraniu określonej grupy, **grupy hybrydowych procesów roboczych** bloku właściwości jest wyświetlany.<br> ![Blok grupy hybrydowych procesów roboczych elementu Runbook](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
1. W bloku właściwości wybranej grupy, kliknij **usunąć**.  Zostanie wyświetlony komunikat prośbą o potwierdzenie tej akcji, wybierz **tak** czy na pewno chcesz kontynuować.<br> ![Okno dialogowe potwierdzenia usunięcia grupy](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Ten proces może potrwać kilka sekund. Możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu. 

## <a name="next-steps"></a>Następne kroki

* Przegląd [uruchamiania elementów runbook na hybrydowy proces roboczy elementu Runbook](automation-hrw-run-runbooks.md) Aby dowiedzieć się, jak skonfigurować elementy runbook do automatyzacji procesów w lokalnym centrum danych lub w innym środowisku chmury.
* Aby uzyskać informacje na temat instalowania systemu Windows hybrydowymi elementami roboczymi Runbook, zobacz [procesu roboczego Runbook hybrydowego Windows automatyzacji Azure](automation-windows-hrw-install.md).
* Aby uzyskać informacje na temat instalowania systemu Linux hybrydowymi elementami roboczymi Runbook, zobacz [procesu roboczego Runbook hybrydowego Linux automatyzacji Azure](automation-linux-hrw-install.md).