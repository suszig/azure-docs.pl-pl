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
ms.date: 11/30/2017
ms.author: eamono
ms.openlocfilehash: 9ae5e9ebcbcf3af61318eac98defc6b249417f41
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/02/2017
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Dołączenia aktualizacji i rozwiązań zmian do usługi Automatyzacja Azure

Z tego samouczka, dowiesz się, jak automatycznie dołączyć rozwiązań aktualizacji, śledzenia zmian i magazynu dla maszyn wirtualnych do usługi Automatyzacja Azure:

> [!div class="checklist"]
> * Dołączyć maszynę wirtualną platformy Azure ręcznie.
> * Instalowanie i aktualizowanie wymagane moduły Azure.
> * Zaimportuj element runbook tego onboards maszynach wirtualnych platformy Azure.
> * Element runbook tego onboards maszynach wirtualnych platformy Azure automatycznie uruchomiony.

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka wymagane są następujące elementy.
+ Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
+ [Konto automatyzacji](automation-offering-get-started.md) do zarządzania maszynami.

## <a name="onboard-an-azure-virtual-machine-manually"></a>Dołączyć maszynę wirtualną platformy Azure ręcznie

1.  Otwórz konta automatyzacji.
2.  Kliknij na stronie spisu.
![Zintegrowanego rozwiązania magazynu](media/automation-onboard-solutions/inventory-onboard.png)
3.  Wybierz istniejący obszar roboczy analizy dzienników lub utworzyć nowy. Kliknij przycisk Włącz.
4.  Po zakończeniu wykonywania śledzenia i spisu rozwiązania dołączania powiadomienia o zmianie, kliknij na stronie Zarządzanie aktualizacjami.
![Dodaj rozwiązanie aktualizacji](media/automation-onboard-solutions/update-onboard.png)
4.  Kliknij przycisk Włącz tego onboards rozwiązanie aktualizacji.
5.  Po zakończeniu wykonywania powiadomienie o aktualizacji rozwiązania dołączania, kliknij na stronie śledzenia zmian.
![Dołączyć śledzenie zmian.](media/automation-onboard-solutions/change-tracking-onboard-vm.png)
6.  Kliknij przycisk Dodaj maszyny Wirtualnej platformy Azure.
![Wybierz maszynę Wirtualną do śledzenia zmian](media/automation-onboard-solutions/enable-change-tracking.png)
7.  Wybierz maszyny Wirtualnej platformy Azure i kliknij przycisk Włącz, aby dołączyć do rozwiązania śledzenia i spisu zmiany.
8.  Po zakończeniu wykonywania powiadomienia dołączania maszyny Wirtualnej, kliknij na stronie Zarządzanie aktualizacjami.
![Dołączyć maszyny Wirtualnej do zarządzania aktualizacjami](media/automation-onboard-solutions/update-onboard-vm.png)
9.  Kliknij przycisk Dodaj maszyny Wirtualnej platformy Azure.
![Wybierz maszynę Wirtualną do zarządzania aktualizacjami](media/automation-onboard-solutions/enable-update.png)
10.  Wybierz maszyny Wirtualnej platformy Azure i kliknij przycisk Włącz, aby rozpocząć korzystanie z rozwiązania do zarządzania aktualizacji.

## <a name="install-and-update-required-azure-modules"></a>Instalowanie i aktualizowanie wymagane moduły Azure

Jest wymagany do aktualizacji do najnowszej modułów Azure i zaimportować AzureRM.OperationalInsights pomyślnie zautomatyzować przy dołączaniu rozwiązania.
1.  Kliknij na stronie moduły.
![Moduły aktualizacji](media/automation-onboard-solutions/update-modules.png)
2.  Kliknij przycisk modułów Azure aktualizacji, która aktualizuje do najnowszej wersji. Poczekaj na zakończenie aktualizacji.
3.  Kliknij przycisk Przeglądaj galerii otwarcie Galerii modułu.
![Zaimportuj moduł OperationalInsights](media/automation-onboard-solutions/import-operational-insights-module.png)
4.  Wyszukaj AzureRM.OperationalInsights i zaimportować tego modułu do konta automatyzacji.

## <a name="import-a-runbook-that-onboards-solutions-to-azure-vms"></a>Zaimportuj element runbook tego rozwiązania onboards na maszynach wirtualnych platformy Azure

1.  Kliknij na stronie elementy Runbook w kategorii "AUTOMATYZACJI procesu".
2.  Kliknij przycisk "Przeglądaj Galeria".
3.  Wyszukaj "aktualizacji i śledzenie zmian" i zaimportuj element runbook do konta automatyzacji.
![Importowanie elementu runbook dołączania](media/automation-onboard-solutions/import-from-gallery.png)
4.  Kliknij "Edytuj", aby wyświetlić źródło elementu Runbook, a następnie kliknij przycisk "Publikuj".
![Importowanie elementu runbook dołączania](media/automation-onboard-solutions/publish-runbook.png)

## <a name="start-the-runbook-that-onboards-azure-vms-automatically"></a>Uruchomić element runbook, że onboards maszynach wirtualnych platformy Azure automatycznie

Musi mieć został załadowany, śledzenie zmian albo zaktualizuj rozwiązań do maszyny Wirtualnej platformy Azure, aby można było uruchomić ten element runbook. Wymaga istniejącej maszyny wirtualnej i grupy zasobów z został załadowany rozwiązania dla parametrów.
1.  Otwórz element runbook MultipleSolution Włącz.
![Wiele elementów runbook rozwiązania](media/automation-onboard-solutions/runbook-overview.png)
2.  Kliknij przycisk start, a następnie wprowadź następujące wartości dla parametrów.
    *   VMNAME. Nazwa istniejącej maszyny Wirtualnej do dołączenia do aktualizacji lub zmień śledzenie rozwiązania. Pozostaw to pole puste, a wszystkie maszyny wirtualne w grupie zasobów są został załadowany.
    *   VMRESOURCEGROUP. Nazwa maszyny Wirtualnej jest członkiem grupy zasobów.
    *   IDENTYFIKATOR SUBSKRYPCJI. Identyfikator subskrypcji znajduje się nowej maszyny Wirtualnej do dołączenia. Pozostaw to pole puste i subskrypcji obszaru roboczego jest używany. Jeśli podano identyfikator innej subskrypcji, konto Uruchom jako dla tego konta automatyzacji należy dołączyć jako współautora dla tej subskrypcji również.
    *   ALREADYONBOARDEDVM. Nazwa maszyny wirtualnej, która została ręcznie został załadowany do śledzenia zmian lub aktualizacji rozwiązania.
    *   ALREADYONBOARDEDVMRESOURCEGROUP. Nazwa maszyny Wirtualnej jest członkiem grupy zasobów.
    *   SOLUTIONTYPE. Wprowadź "Aktualizacje" lub "Śledzenia zmian"
    
    ![Wiele parametrów elementu runbook rozwiązania](media/automation-onboard-solutions/runbook-parameters.png)
3.  Kliknij przycisk OK, aby uruchomić zadanie elementu runbook.
4.  Monitoruj postęp oraz wszelkie błędy na stronie zadania elementu runbook.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz [planowania elementów runbook](automation-schedules.md).