---
title: Odłączanie konta usługi Azure Automation od usługi Log Analytics
description: Ten artykuł zawiera omówienie sposobu odłączania Twoje konto usługi Automatyzacja Azure z obszaru roboczego analizy dzienników.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 2e6f887a7684ae995e9b7da84dace523843ff555
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Sposobu odłączania Twoje konto usługi Automatyzacja z obszaru roboczego analizy dzienników

Automatyzacja Azure umożliwia integrację z analizy dzienników umożliwia nie tylko monitorowanie zadań elementu runbook dla wszystkich kont automatyzacji, ale jest również wymagany podczas importowania następujące rozwiązania, które są zależne od analizy dzienników:

* [Zarządzanie aktualizacjami](../operations-management-suite/oms-solution-update-management.md)
* [Śledzenie zmian](../log-analytics/log-analytics-change-tracking.md)
* [Uruchamiania/zatrzymywania maszyn wirtualnych w godzinach](automation-solution-vm-management.md)
 
Jeśli zdecydujesz się, że chcesz zintegrować Twoje konto usługi Automatyzacja z analizy dzienników nie będzie można odłączyć konta bezpośrednio z portalu Azure.  Zanim będziesz kontynuować, należy najpierw usunąć rozwiązania wspomniano wcześniej, w przeciwnym razie ten proces nie będzie mógł kontynuować.  Przejrzyj temat dla określonego rozwiązania, które zostały zaimportowane, aby zrozumieć kroki wymagane w celu usunięcia go.  

Po usunięciu tych rozwiązań, które można wykonać następujące kroki, aby odłączyć Twoje konto usługi Automatyzacja.

## <a name="unlink-workspace"></a>Rozłącz obszaru roboczego

1. Korzystając z portalu Azure Otwórz Twoje konto usługi Automatyzacja, a na automatyzację konta wybierz stronę **odłączyć obszaru roboczego** w sekcji **powiązane zasoby** po lewej stronie.<br><br> ![Odłącz obszar roboczy](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. Na stronie Rozłącz obszaru roboczego kliknij **odłączyć obszaru roboczego**.<br><br> ![Strona obszaru roboczego odłączyć](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).<br><br>  Zostanie wyświetlony monit sprawdzający, czy chcesz kontynuować.<br><br>
3. Gdy usługi Automatyzacja Azure usiłuje odłączyć konto obszaru roboczego analizy dzienników, możesz śledzić postępy w obszarze **powiadomienia** z menu.

Użycie rozwiązania zarządzania aktualizacjami, opcjonalnie możesz usunąć następujące elementy, które nie są już potrzebne po usunięciu rozwiązania.

* Harmonogramy aktualizacji.  Będzie mieć nazwy są zgodne z wdrożenia aktualizacji, który utworzono)

* Hybrydowe utworzone dla rozwiązania grupy procesu roboczego.  Każda będzie miała nazwę podobnie do machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Jeśli używasz uruchamiania/zatrzymywania maszyn wirtualnych podczas rozwiązania poza godzinami szczytu, opcjonalnie może chcesz usunąć następujące elementy, które nie są już potrzebne po usunięciu rozwiązania.

* Uruchamianie i zatrzymywanie harmonogramy runbook maszyny Wirtualnej 
* Uruchamianie i zatrzymywanie elementów runbook maszyny Wirtualnej
* Zmienne   

## <a name="next-steps"></a>Kolejne kroki

Aby zmienić konfigurację konta automatyzacji do integracji z analizy dzienników OMS, zobacz [przekazuj strumienie zadania i stan zadania z automatyzacji analizy dzienników (OMS)](automation-manage-send-joblogs-log-analytics.md). 