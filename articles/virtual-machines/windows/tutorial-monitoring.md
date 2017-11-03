---
title: Azure monitorowanie i aktualizowanie maszyn wirtualnych systemu Windows i | Dokumentacja firmy Microsoft
description: "Samouczek — monitorowania i aktualizowania maszyny wirtualnej systemu Windows przy użyciu programu Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: a37aed8b3321d3518ffd73e09f5bb21266a7e577
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-update-a-windows-virtual-machine-with-azure-powershell"></a>Monitorowanie i aktualizowanie maszyny wirtualnej systemu Windows przy użyciu programu Azure PowerShell

Azure monitorowanie używa agentów do zbierania danych rozruchu i wydajności z maszyn wirtualnych platformy Azure, przechowywania tych danych w magazynie Azure i stał się dostępny za pośrednictwem portalu, moduł Azure PowerShell i interfejsu wiersza polecenia Azure. Zarządzanie aktualizacjami umożliwia zarządzanie aktualizacje i poprawki dla maszyn wirtualnych systemu Windows Azure.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Włącz diagnostykę rozruchu na maszynie Wirtualnej
> * Wyświetlanie diagnostyki rozruchu
> * Wyświetlaj metryki hosta maszyny Wirtualnej
> * Zainstaluj rozszerzenie diagnostyki
> * Wyświetlaj metryki maszyny Wirtualnej
> * Utwórz alert
> * Zarządzanie aktualizacjami systemu Windows
> * Skonfiguruj zaawansowane monitorowanie

Dla tego samouczka jest wymagany moduł Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

Aby ukończyć przykład, w tym samouczku, musi mieć istniejącej maszyny wirtualnej. Jeśli to konieczne, to [przykładowym skrypcie](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) można utworzyć. Podczas pracy z samouczkiem, Zastąp grupy zasobów, nazwę maszyny Wirtualnej i lokalizację w razie potrzeby.

## <a name="view-boot-diagnostics"></a>Wyświetlanie diagnostyki rozruchu

Jak rozruchu maszyn wirtualnych systemu Windows, agenta diagnostyki rozruchu przechwytuje danych wyjściowych ekranu, który może służyć do rozwiązywania problemów z celem. Ta funkcja jest domyślnie włączona. Zrzuty ekranu przechwyconych są przechowywane w koncie magazynu platformy Azure, której tworzona jest również domyślnie. 

Można uzyskać danych diagnostycznych rozruchu z [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) polecenia. W poniższym przykładzie diagnostyki rozruchu są pobierane z katalogiem głównym * c:\* dysku. 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Wyświetlaj metryki hosta

Maszyny Wirtualnej systemu Windows ma dedykowanego hosta maszyny Wirtualnej na platformie Azure, która współdziała ona z. Metryki są automatycznie pobierane dla hosta i mogą być wyświetlane w portalu Azure.

1. W portalu Azure kliknij **grup zasobów**, wybierz pozycję **myResourceGroup**, a następnie wybierz **myVM** na liście zasobów.
2. Kliknij przycisk **metryki** w bloku maszyny Wirtualnej, a następnie wybierz jedno z hosta metryki w obszarze **dostępne metryki** aby zobaczyć, jak działa hosta maszyny Wirtualnej.

    ![Wyświetlaj metryki hosta](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Zainstaluj rozszerzenie diagnostyki

Metryki podstawowych hostów są dostępne, ale bardziej szczegółowe i metryki specyficzne dla maszyny Wirtualnej, można, należy zainstalować rozszerzenie diagnostycznych platformy Azure na maszynie Wirtualnej. Rozszerzenia diagnostyki Azure umożliwia dodatkowe funkcje monitorowania i dane diagnostyczne mają zostać pobrane z maszyny Wirtualnej. Możesz wyświetlić te metryki wydajności i tworzyć alerty oparte na sposób wykonywania maszyny Wirtualnej. Rozszerzenia diagnostycznych jest zainstalowany za pośrednictwem portalu Azure w następujący sposób:

1. W portalu Azure kliknij **grup zasobów**, wybierz pozycję **myResourceGroup**, a następnie wybierz **myVM** na liście zasobów.
2. Kliknij przycisk **ustawienia diagnostyki**. Lista wskazuje, że *diagnostyki rozruchu* są już włączone w poprzedniej sekcji. Kliknij pole wyboru dla *podstawowe metryki*.
3. Kliknij przycisk **Włącz monitorowanie na poziomie gościa** przycisku.

    ![Wyświetlaj metryki diagnostycznych](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Wyświetlaj metryki maszyny Wirtualnej

Metryki maszyny Wirtualnej można wyświetlać w taki sam sposób, aby wyświetlić hosta metryki maszyny Wirtualnej:

1. W portalu Azure kliknij **grup zasobów**, wybierz pozycję **myResourceGroup**, a następnie wybierz **myVM** na liście zasobów.
2. Aby zobaczyć, jak działa maszyny Wirtualnej, kliknij **metryki** w bloku maszyny Wirtualnej, a następnie wybierz jedno z metryki diagnostyki w obszarze **dostępne metryki**.

    ![Wyświetlaj metryki maszyny Wirtualnej](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Tworzenie alertów

Można tworzyć alertów w oparciu metryki dotyczące wydajności. Alerty można powiadomić o, gdy średniego użycia procesora CPU przekracza określonego progu lub wolnego miejsca na dysku spada poniżej pewnego, np. Alerty są wyświetlane w portalu Azure lub mogą być wysyłane za pośrednictwem poczty e-mail. W odpowiedzi na alerty generowane może także wyzwolić elementu runbook usługi Automatyzacja Azure lub usługi Azure Logic Apps.

Poniższy przykład tworzy alert dla średniego użycia procesora CPU.

1. W portalu Azure kliknij **grup zasobów**, wybierz pozycję **myResourceGroup**, a następnie wybierz **myVM** na liście zasobów.
2. Kliknij przycisk **reguły alertów** w bloku maszyny Wirtualnej, następnie kliknij polecenie **Dodaj alert metryki** w górnej części bloku alerty.
4. Podaj **nazwa** alertu, takie jak *myAlertRule*
5. Do wyzwolenia alertu, gdy procent użycia procesora CPU przekracza 1.0 przez pięć minut, pozostaw wszystkie inne wartości domyślne wybrane.
6. Opcjonalnie zaznacz pole wyboru, aby uzyskać *E-mail właściciele, współautorzy i czytelnicy* do wysyłania powiadomień e-mail. Domyślne działanie jest obecne powiadomienia w portalu.
7. Kliknij przycisk **OK**.

## <a name="manage-windows-updates"></a>Zarządzanie aktualizacjami systemu Windows

Zarządzanie aktualizacjami umożliwia zarządzanie aktualizacje i poprawki dla maszyn wirtualnych systemu Windows Azure.
Bezpośrednio z maszyny Wirtualnej, można szybko ocenić stan dostępne aktualizacje, zaplanowanie instalacji wymaganych aktualizacji i przejrzeć wyniki wdrożenia, aby sprawdzić, czy aktualizacje zostały pomyślnie zastosowane do maszyny Wirtualnej.

Aby uzyskać informacje o cenach, zobacz [ceny Automatyzacja zarządzania aktualizacjami](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management"></a>Włącz zarządzanie aktualizacjami

Włącz zarządzanie aktualizacji dla maszyny Wirtualnej:
 
1. Po lewej stronie ekranu, wybierz **maszyn wirtualnych**.
2. Z listy wybierz maszynę Wirtualną.
3. Na ekranie maszyny Wirtualnej w **operacji** kliknij **zarządzanie aktualizacjami**. **Włączyć zarządzanie aktualizacjami** ekranu zostanie otwarta.

Weryfikacja jest przeprowadzana w celu ustalenia, czy zarządzanie aktualizacjami jest włączony dla tej maszyny Wirtualnej. Sprawdzanie poprawności obejmuje sprawdza, czy obszar roboczy analizy dzienników i połączonego konta automatyzacji, a jeśli rozwiązanie znajduje się w obszarze roboczym.

Obszar roboczy analizy dzienników służy do zbierania danych, który jest generowany przez funkcji i usług, takich jak zarządzanie aktualizacjami. Obszar roboczy zawiera pojedynczej lokalizacji, aby przejrzeć i analizowania danych z wielu źródeł. Do wykonania dodatkowych czynności na maszynach wirtualnych, które wymagają aktualizacji, usługi Automatyzacja Azure pozwala na uruchamianie skryptów przed maszynami wirtualnymi, takie jak do pobrania i zastosowania aktualizacji.

Proces weryfikacji sprawdza również, jeśli maszyna wirtualna jest udostępniane z programu Microsoft Monitoring Agent (MMA) i hybrydowy proces roboczy. Ten agent jest używany do komunikowania się z maszyną Wirtualną i uzyskiwania informacji o stanie aktualizacji. 

Te wymagania wstępne nie są spełnione, zostanie wyświetlony transparent daje możliwość włączenia rozwiązania.

![Zaktualizuj transparent dołączyć konfiguracji zarządzania](./media/tutorial-monitoring/manageupdates-onboard-solution-banner.png)

Kliknij transparent, aby włączyć rozwiązanie. W przypadku dowolnej z następujących warunków wstępnych zostały brakować po weryfikacji, zostaną automatycznie dodane:

* [Zaloguj się Analytics](../../log-analytics/log-analytics-overview.md) obszaru roboczego
* [Automatyzacja](../../automation/automation-offering-get-started.md)
* A [hybrydowy proces roboczy elementu runbook](../../automation/automation-hybrid-runbook-worker.md) jest włączona na maszynie Wirtualnej

**Włączyć zarządzanie aktualizacjami** ekranu zostanie otwarta. Skonfiguruj ustawienia, a następnie kliknij przycisk **włączyć**.

![Włącz rozwiązanie do zarządzania aktualizacji](./media/tutorial-monitoring/manageupdates-update-enable.png)

Włączanie rozwiązania może potrwać do 15 minut, a w tym czasie nie powinna zamykać okna przeglądarki. Po włączeniu rozwiązania, informacje o brakujących aktualizacji na maszynie Wirtualnej przechodzi do analizy dzienników.
Może upłynąć od 30 minut do 6 godzin dla danych, które mają być dostępne dla analizy.

### <a name="view-update-assessment"></a>Wyświetlanie oceny aktualizacji

Po włączeniu rozwiązania **Update Management** zostanie wyświetlony ekran **Update Management**. Możesz wyświetlić listę brakujących aktualizacji na karcie **Brakujące aktualizacje**.

 ![Wyświetl stan aktualizacji](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>Planowanie wdrożenia aktualizacji

Aby zainstalować aktualizacje, zaplanuj wdrożenie zgodnie z harmonogramem wydawania i oknem obsługi.
Możesz wybrać typy aktualizacji, które mają zostać uwzględnione we wdrożeniu. Na przykład można uwzględnić krytyczny lub aktualizacji zabezpieczeń i Wyklucz pakiety zbiorcze aktualizacji.

Planowanie nowego wdrożenia aktualizacji dla maszyny Wirtualnej, klikając **harmonogram wdrożenia aktualizacji** w górnej części **zarządzanie aktualizacjami** ekranu. W **nowego wdrożenia aktualizacji** ekranu, podaj następujące informacje:

* **Nazwa** — wprowadź unikatową nazwę identyfikującą wdrożenie aktualizacji.
* **Klasyfikacja aktualizacji** — wybierz typy oprogramowania uwzględnione we wdrożeniu wdrożenia aktualizacji. Dostępne są następujące typy klasyfikacji:
  * Aktualizacje krytyczne
  * Aktualizacje zabezpieczeń
  * Pakiety zbiorcze aktualizacji
  * Pakiety funkcji
  * Dodatki Service Pack
  * Aktualizacje definicji
  * Narzędzia
  * Aktualizacje

* **Ustawienia harmonogramu** — możesz zaakceptować domyślną datę i godzinę (30 minut po bieżącej godzinie) lub określić inny czas.
  Możesz też określić, czy wdrożenie ma występować raz, czy zgodnie z ustawionym harmonogramem cyklicznym. Kliknij opcję Cyklicznie w obszarze Cykl, aby skonfigurować harmonogram cykliczny.

  ![Ekran ustawień harmonogramu aktualizacji](./media/tutorial-monitoring/manageupdates-schedule-win.png)

* **Okno konserwacji (w minutach)** — podaj okres, w którym ma zostać przeprowadzone wdrażanie aktualizacji.  Pozwala to zagwarantować, że zmiany będą wprowadzane w ramach zdefiniowanych okien obsługi.

Po ukończeniu konfigurowania harmonogramu kliknij przycisk **Utwórz**, aby wrócić do pulpitu nawigacyjnego stanu.
Zwróć uwagę, że **zaplanowane** tabeli przedstawiono harmonogram wdrożenia został utworzony.

> [!WARNING]
> Aktualizacje, które wymagają ponownego uruchomienia systemu maszyna wirtualna zostanie ponownie uruchomiony.

### <a name="view-results-of-an-update-deployment"></a>Wyświetlanie wyników wdrażania aktualizacji

Po rozpoczęciu zaplanowanego wdrażania stan tego wdrożenia można sprawdzić na karcie **Wdrożenia aktualizacji** na ekranie **Update Management**.
Jeśli wdrażanie trwa, wdrożenie ma stan **W toku**. Po pomyślnym ukończeniu wdrażania stan zmienia się na **Powodzenie**.
W przypadku błędu co najmniej jednej aktualizacji w ramach wdrożenia jest wyświetlany stan **Częściowe niepowodzenie**.
Kliknij ukończone wdrożenie aktualizacji, aby wyświetlić pulpit nawigacyjny tego wdrożenia.

   ![Pulpit nawigacyjny stan wdrożenia aktualizacji dla określonego wdrożenia](./media/tutorial-monitoring/manageupdates-view-results.png)

W **wyniki aktualizacji** kafelka znajduje się podsumowanie całkowita liczba aktualizacji i wyniki wdrożenia na maszynie Wirtualnej.
Tabela po prawej stronie zawiera szczegółowy podział każdej aktualizacji i wyniki instalacji, które mogą mieć jedną z następujących wartości:

* **Nie podjęto** — aktualizacja nie została zainstalowana w powodu niewystarczających czas dostępne w oparciu o czas trwania okna obsługi, które zostały zdefiniowane.
* **Pomyślnie** -aktualizacji zakończyło się pomyślnie.
* **Nie powiodło się** — aktualizacja nie powiodła się.

Kliknij pozycję **Wszystkie dzienniki**, aby wyświetlić wszystkie wpisy dziennika utworzone przez wdrożenie.

Kliknij przycisk **dane wyjściowe** Kafelek, aby zobaczyć strumień zadań elementu runbook odpowiedzialny za zarządzanie wdrożenia aktualizacji w celu maszyny Wirtualnej.

Kliknij pozycję **Błędy**, aby wyświetlić szczegółowe informacje o błędach związanych z wdrożeniem.

## <a name="advanced-monitoring"></a>Zaawansowane monitorowanie 

Możliwość bardziej zaawansowane monitorowanie maszyny wirtualnej za pomocą [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Jeśli nie zostało to jeszcze zrobione, należy zarejestrować się w celu [bezpłatnej wersji próbnej](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) programu Operations Management Suite.

Jeśli masz dostęp do portalu OMS klucz obszaru roboczego i identyfikator obszaru roboczego można znaleźć w bloku ustawień. Użyj [AzureRmVMExtension zestaw](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) polecenie, aby dodać rozszerzenie OMS do maszyny Wirtualnej. Zaktualizuj wartości zmiennej w poniżej przykładowy w celu uwzględnienia możesz OMS klucz obszaru roboczego i identyfikator obszaru roboczego  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

Po upływie kilku minut powinien zostać wyświetlony nowej maszyny Wirtualnej w obszarze roboczym pakietu OMS. 

![Blok OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Następne kroki
W tym samouczku został skonfigurowany i przejrzeć maszyn wirtualnych w Centrum zabezpieczeń Azure. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie sieci wirtualnej
> * Tworzenie grupy zasobów i maszyny Wirtualnej 
> * Włącz diagnostykę rozruchu na maszynie Wirtualnej
> * Wyświetlanie diagnostyki rozruchu
> * Wyświetlaj metryki hosta
> * Zainstaluj rozszerzenie diagnostyki
> * Wyświetlaj metryki maszyny Wirtualnej
> * Utwórz alert
> * Zarządzanie aktualizacjami systemu Windows
> * Skonfiguruj zaawansowane monitorowanie

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure.

> [!div class="nextstepaction"]
> [Zarządzanie zabezpieczeniami maszyn wirtualnych](./tutorial-azure-security.md)