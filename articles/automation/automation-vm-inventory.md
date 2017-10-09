---
title: "Zarządzanie maszyną wirtualną platformy Azure z wykorzystaniem zbierania spisu | Microsoft Docs"
description: "Zarządzanie maszyną wirtualną z wykorzystaniem zbierania spisu"
services: automation
keywords: "spis, automatyzacja, zmiana, śledzenie"
author: jennyhunter-msft
ms.author: jehunte
ms.date: 09/13/2017
ms.topic: hero-article
manager: carmonm
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 5291d112c2cdf157543fe301d5a5c80f3fe561ae
ms.contentlocale: pl-pl
ms.lasthandoff: 09/25/2017

---

# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Zarządzanie maszyną wirtualną platformy Azure z wykorzystaniem zbierania spisu

Śledzenie z użyciem spisu można włączyć dla maszyny wirtualnej platformy Azure na stronie zasobów tej maszyny. Ta metoda zapewnia interfejs użytkownika oparty na przeglądarce przeznaczony do instalowania i konfigurowania zbierania spisu.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.
Jeśli nie masz maszyny wirtualnej platformy Azure, utwórz [maszynę wirtualną](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal) przed rozpoczęciem.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Włączanie zbierania spisu na stronie zasobów maszyny wirtualnej

1. Na ekranie po lewej stronie wybierz pozycję **Maszyny wirtualne**.
1. Z listy wybierz maszynę wirtualną.
1. Wybierz pozycję **Spis (wersja zapoznawcza)** z menu zasobów poniżej obszaru **Operacje**.
1. W górnej części strony pojawi się transparent informujący o tym, że rozwiązanie nie jest włączone. Kliknij transparent, aby włączyć rozwiązanie.
1. Wybierz obszar roboczy usługi Log Analytics, aby zapisać w nim dzienniki danych. Jeśli w tym regionie nie ma dostępnych dla Ciebie obszarów roboczych, pojawi się zachęta do utworzenia domyślnego obszaru roboczego i konta usługi Automation. Kliknij pozycję **Włącz**, aby rozpocząć dołączanie swojego komputera.

   ![Widok opcji dołączania](./media/automation-vm-inventory/inventory-onboarding-options.png)  

1. Pojawi się pasek stanu z informacją o tym, że trwa włączanie rozwiązania. Ten proces może potrwać do 15 minut. W tym czasie możesz zamknąć blok lub zachować blok otwarty i wówczas po włączeniu rozwiązania pojawi się w nim odpowiednia informacja. Stan wdrożenia możesz monitorować w okienku powiadomień.

   ![Widok rozwiązania spisu natychmiast po dołączeniu](./media/automation-vm-inventory/inventory-onboarded.png)

1. Po zakończeniu wdrażania zniknie pasek stanu. W tym czasie system nadal zbiera dane spisu, a dane mogą być jeszcze niewidoczne. Zebranie pełnych danych może zająć 24 godziny.

## <a name="configure-your-inventory-settings"></a>Konfigurowanie ustawień spisu

Domyślnie na potrzeby zbierania skonfigurowane są pozycje Oprogramowanie, Usługi systemu Windows oraz Demony systemu Linux. Aby zbierać spis dla Rejestru systemu Windows i plików, skonfiguruj ustawienia zbierania spisu.

1. W widoku **Spis (wersja zapoznawcza)** wybierz przycisk **Edytuj ustawienia** w górnej części strony.
2. Aby dodać nowe ustawienie zbierania, przejdź do kategorii ustawienia, które chcesz dodać, używając kart **Rejestr systemu Windows**, **Pliki systemu Windows** oraz **Pliki systemu Linux**. Kliknij pozycję **Dodaj** w górnej części strony.
3. Aby wyświetlić szczegóły i opisy każdej właściwości ustawienia, odwiedź [stronę dokumentacji spisu](https://aka.ms/configinventorydocs).

## <a name="disconnecting-your-virtual-machine-from-management"></a>Odłączanie maszyny wirtualnej od zarządzania

Aby usunąć maszynę z zarządzania spisem:

1. W menu po lewej stronie w witrynie Azure Portal kliknij pozycję **Log Analytics**, a następnie wybierz kliknięciem obszar roboczy, który był używany podczas dołączania danej maszyny wirtualnej.
1. Na stronie Log Analytics wybierz pozycję **Maszyny wirtualne** w kategorii **Źródła danych obszaru roboczego** w menu zasobów. 
1. Wybierz maszynę wirtualną, którą chcesz odłączyć od listy. Dla tej maszyny wirtualnej w kolumnie **Połączenie OMS** pojawi się zielony znacznik wyboru obok tekstu „Ten obszar roboczy”. Kliknij pozycję **Rozłącz** w górnej części następnej strony i pozycję **Tak** w oknie dialogowym potwierdzenia, aby odłączyć maszynę od zarządzania.

## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się więcej o zarządzaniu zmianami w plikach i ustawieniach rejestru na maszynach wirtualnych, zobacz temat [Śledzenie zmian](../log-analytics/log-analytics-change-tracking.md).
* Aby dowiedzieć się więcej o zarządzaniu aktualizacjami systemu Windows i aktualizacjami pakietów na maszynach wirtualnych, zobacz temat [Zarządzanie aktualizacjami](../operations-management-suite/oms-solution-update-management.md)

