---
title: "Dołączenia aktualizacji i rozwiązań zmian do automatyzacji Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dołączyć aktualizacji i rozwiązań zmian do automatyzacji Azure."
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: edae1156-2dc7-4dab-9e5c-bf253d3971d0
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2017
ms.author: eamono
ms.openlocfilehash: 25ad5e04c96aab9dddd39cdb4d33bd8411817e8e
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/22/2017
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Dołączenia aktualizacji i rozwiązań zmian do usługi Automatyzacja Azure

Z tego samouczka, dowiesz się, jak automatycznie dołączyć rozwiązań aktualizacji, śledzenia zmian i magazynu dla maszyn wirtualnych do usługi Automatyzacja Azure:

> [!div class="checklist"]
> * Dołączyć maszyny Wirtualnej platformy Azure
> * Włącz rozwiązania
> * Instalowanie i aktualizowanie modułów
> * Zaimportuj element runbook dołączania
> * Uruchomić element runbook

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka wymagane są następujące elementy:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Konto automatyzacji](automation-offering-get-started.md) do zarządzania maszynami.
* A [maszyny wirtualnej](../virtual-machines/windows/quick-create-portal.md) dołączyć.

## <a name="onboard-an-azure-vm"></a>Dołączyć maszyny Wirtualnej platformy Azure

Aby dołączyć maszyn wirtualnych Azure automatycznie, istniejącej maszyny Wirtualnej, musi być został załadowany z śledzenia zmian lub rozwiązania do zarządzania aktualizacji. W tym kroku, możesz dołączyć maszynę wirtualną z zarządzania aktualizacjami i śledzenie zmian.

### <a name="enable-change-tracking-and-inventory"></a>Włączanie śledzenia zmian i magazynu

Śledzenie zmian i spisu rozwiązanie zapewnia możliwość [śledzenia zmian](automation-vm-change-tracking.md) i [spisu](automation-vm-inventory.md) na maszynach wirtualnych. W tym kroku należy włączyć rozwiązania na maszynie wirtualnej.

1. Wybierz z menu po lewej stronie **kont automatyzacji**, a następnie wybierz konto Automatyzacja na liście.
1. Wybierz **spisu** w obszarze **zarządzania konfiguracją**.
1. Wybierz istniejący obszar roboczy analizy dzienników lub utworzyć nowy. Kliknij przycisk **włączyć** przycisku.

![Dodaj rozwiązanie aktualizacji](media/automation-onboard-solutions/inventory-onboard.png)

Po zakończeniu śledzenia i spisu rozwiązania dołączania powiadomienia o zmianie, kliknij **zarządzania aktualizacjami** w obszarze **zarządzania konfiguracją**.

### <a name="enable-update-management"></a>Włącz zarządzanie aktualizacjami

Rozwiązanie do zarządzania aktualizacji umożliwia zarządzanie aktualizacje i poprawki dla maszyn wirtualnych systemu Windows Azure. Można ocenić stan dostępne aktualizacje, harmonogram instalacji wymaganych aktualizacji i przegląd wyników wdrożenia Sprawdź aktualizacje zostały pomyślnie zastosowane do maszyny Wirtualnej. W tym kroku zostanie włączone rozwiązania dla maszyny Wirtualnej.

1. Twoje konto usługi Automatyzacja, zaznacz **zarządzanie aktualizacjami** w obszarze **zarządzania AKTUALIZACJAMI**.
1. Obszar roboczy analizy dzienników, które zostały wybrane jest tym samym obszarze roboczym w poprzednim kroku. Kliknij przycisk **włączyć** dołączyć rozwiązania do zarządzania aktualizacji.

![Dodaj rozwiązanie aktualizacji](media/automation-onboard-solutions/update-onboard.png)

Podczas instalowania aktualizacji rozwiązania do zarządzania, jest wyświetlany transparent niebieski. Po włączeniu rozwiązania wybierz **śledzenie zmian** w obszarze **zarządzania konfiguracją** przejść do następnego kroku.

### <a name="select-azure-vm-to-be-managed"></a>Wybierz maszynę Wirtualną Azure mają być zarządzane

Teraz, gdy rozwiązania są włączone, aby rozpocząć korzystanie z tych rozwiązań można dodać maszyny Wirtualnej platformy Azure.

1. Z Twojego konta automatyzacji na **śledzenie zmian** wybierz pozycję **+ Dodaj maszyny Wirtualnej Azure** można dodać maszyny wirtualnej.

1. Wybierz maszyny Wirtualnej z listy i wybierz **włączyć**. Ta akcja umożliwia śledzenia zmian i rozwiązanie magazynu dla maszyny wirtualnej.

   ![Włącz rozwiązanie aktualizacji dla maszyny wirtualnej](media/automation-onboard-solutions/enable-change-tracking.png)

1. Po powiadomień dołączania maszyny Wirtualnej zakończeniu z Twojego konta automatyzacji wybierz opcję **zarządzanie aktualizacjami** w obszarze **zarządzania AKTUALIZACJAMI**.

1. Wybierz **+ Dodaj maszyny Wirtualnej Azure** można dodać maszyny wirtualnej.

1. Wybierz maszyny Wirtualnej z listy i wybierz **włączyć**. Ta akcja umożliwia rozwiązanie do zarządzania aktualizacji dla maszyny wirtualnej.

   ![Włącz rozwiązanie aktualizacji dla maszyny wirtualnej](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> Aby nie czekać inne rozwiązanie, aby ukończyć, podczas włączania kolejne rozwiązanie zostanie wyświetlony komunikat z informacją: *instalacji inne rozwiązanie jest w toku na tym serwerze lub inną maszynę wirtualną. Po zakończeniu instalacji jest włączony przycisk włączania i mogą żądać instalacji rozwiązania na tej maszynie wirtualnej.*

## <a name="install-and-update-modules"></a>Instalowanie i aktualizowanie modułów

Jest wymagany do aktualizacji do najnowszej modułów Azure i zaimportować `AzureRM.OperationalInsights` pomyślnie zautomatyzować przy dołączaniu rozwiązania.

## <a name="update-azure-modules"></a>Zaktualizuj Azure modułów

Twoje konto usługi Automatyzacja, zaznacz **modułów** w obszarze **UDOSTĘPNIONE zasoby**. Wybierz **aktualizacji modułów Azure** aktualizacji modułów Azure do najnowszej wersji. Wybierz **tak** na monit o aktualizację wszystkich istniejących modułów Azure do najnowszej wersji.

![Moduły aktualizacji](media/automation-onboard-solutions/update-modules.png)

### <a name="install-azurermoperationalinsights-module"></a>Zainstaluj moduł AzureRM.OperationalInsights

Z **modułów** wybierz pozycję **galerii przeglądania** otwarcie Galerii modułu. Wyszukaj AzureRM.OperationalInsights i zaimportować tego modułu do konta automatyzacji.

![Zaimportuj moduł OperationalInsights](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>Zaimportuj element runbook dołączania

1. Twoje konto usługi Automatyzacja, zaznacz na **Runbook** w obszarze **AUTOMATYZACJI procesu**.
1. Wybierz **galerii przeglądania**.
1. Wyszukaj **aktualizacji i śledzenie zmian**, kliknij element runbook i wybierz **importu** na **Wyświetl źródło** strony. Wybierz **OK** można zaimportować elementu runbook do konta automatyzacji.

  ![Importowanie elementu runbook dołączania](media/automation-onboard-solutions/import-from-gallery.png)

1. Na **Runbook** wybierz pozycję **Edytuj**, a następnie wybierz pozycję **publikowania**. Na **publikowania elementu Runbook** okno dialogowe, wybierz opcję **tak** opublikować elementu runbook.

## <a name="start-the-runbook"></a>Uruchomić element runbook

Musi mieć został załadowany, śledzenie zmian albo zaktualizuj rozwiązań do maszyny Wirtualnej platformy Azure, aby można było uruchomić ten element runbook. Wymaga istniejącej maszyny wirtualnej i grupy zasobów z został załadowany rozwiązania dla parametrów.

1. Otwórz element runbook MultipleSolution Włącz.

   ![Wiele elementów runbook rozwiązania](media/automation-onboard-solutions/runbook-overview.png)

1. Kliknij przycisk start, a następnie wprowadź następujące wartości dla parametrów.

   * **VMNAME** -pozostaw to pole puste. Nazwa istniejącej maszyny Wirtualnej do dołączenia do aktualizacji lub zmień śledzenie rozwiązania. Tę wartość należy pozostawić pusty, wszystkie maszyny wirtualne w grupie zasobów są został załadowany.
   * **VMRESOURCEGROUP** — Nazwa grupy zasobów dla maszyn wirtualnych je dołączyć.
   * **Identyfikator SUBSKRYPCJI** -pozostaw to pole puste. Identyfikator subskrypcji nowej maszyny Wirtualnej został załadowany. Jeśli pole pozostanie puste, subskrypcja obszaru roboczego jest używany. Jeśli podano identyfikator innej subskrypcji, konto Uruchom jako dla tego konta automatyzacji należy dołączyć jako współautora dla tej subskrypcji również.
   * **ALREADYONBOARDEDVM** -Nazwa maszyny wirtualnej, która została ręcznie został załadowany do śledzenia zmian lub aktualizacji rozwiązania.
   * **ALREADYONBOARDEDVMRESOURCEGROUP** — Nazwa grupy zasobów, do której należy maszyna wirtualna.
   * **SOLUTIONTYPE** — wprowadź **aktualizacje** lub **śledzenia zmian**

   ![Włącz MultipleSolution parametrów elementu runbook](media/automation-onboard-solutions/runbook-parameters.png)

1. Wybierz **OK** można uruchomić zadania elementu runbook.
1. Monitoruj postęp oraz wszelkie błędy na stronie zadania elementu runbook.

## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dołączyć maszynę wirtualną platformy Azure ręcznie.
> * Instalowanie i aktualizowanie wymagane moduły Azure.
> * Zaimportuj element runbook tego onboards maszynach wirtualnych platformy Azure.
> * Element runbook tego onboards maszynach wirtualnych platformy Azure automatycznie uruchomiony.

Wykonaj to łącze, aby dowiedzieć się więcej na temat planowania elementów runbook.

> [!div class="nextstepaction"]
> [Planowanie elementów runbook](automation-schedules.md).
