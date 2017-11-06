---
title: Monitorowanie i aktualizowanie maszyn wirtualnych systemu Linux na platformie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak monitorować diagnostyki rozruchu i metryk wydajności i zarządzać nimi pakietu aktualizacji na maszynie wirtualnej systemu Linux na platformie Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 70c17d9a8f7bf6d9106efcb56eee7cd996460c18
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-monitor-and-update-a-linux-virtual-machine-in-azure"></a>Jak monitorować i aktualizować maszyny wirtualnej systemu Linux na platformie Azure

Aby zapewnić, że maszyn wirtualnych (VM) na platformie Azure działają poprawnie, można sprawdzić diagnostyki rozruchu, metryki wydajności i zarządzania aktualizacjami pakietu. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Włącz diagnostykę rozruchu na maszynie Wirtualnej
> * Wyświetlanie diagnostyki rozruchu
> * Wyświetlaj metryki hosta
> * Włącz diagnostykę rozszerzenie na maszynie Wirtualnej
> * Wyświetlaj metryki maszyny Wirtualnej
> * Tworzenie alertów w oparciu metryki diagnostycznych
> * Zarządzanie aktualizacjami pakietu
> * Skonfiguruj zaawansowane monitorowanie


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>Tworzenie maszyny wirtualnej

Aby wyświetlić dane diagnostyczne i metryki w akcji, należy maszyny Wirtualnej. Najpierw utwórz nową grupę zasobów o [Tworzenie grupy az](/cli/azure/group#create). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroupMonitor* w *eastus* lokalizacji.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Teraz Utwórz maszynę Wirtualną z [tworzenia maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm#az_vm_create). Poniższy przykład tworzy Maszynę wirtualną o nazwie *myVM*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Włącz diagnostykę rozruchu

Jak rozruchu maszyn wirtualnych systemu Linux, rozszerzenia diagnostyki rozruchu przechwytuje dane wyjściowe rozruchu i zapisuje go w magazynie Azure. Te dane mogą służyć do rozwiązywania problemów rozruchu maszyny Wirtualnej. Diagnostyki rozruchu nie są automatycznie włączone, podczas tworzenia maszyny Wirtualnej systemu Linux przy użyciu wiersza polecenia platformy Azure.

Przed włączeniem diagnostyki rozruchu, konta magazynu musi być utworzony do przechowywania dzienników rozruchu. Konta magazynu musi mieć globalnie unikatowej nazwy należeć do zakresu od 3 do 24 znaków i musi zawierać tylko cyfry i małe litery. Utwórz konto magazynu z [Tworzenie konta magazynu az](/cli/azure/storage/account#create) polecenia. W tym przykładzie losowy ciąg służy do tworzenia nazwy konta magazynu unikatowy. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Podczas włączania diagnostyki rozruchu, wymagany jest identyfikator URI do kontenera magazynu obiektów blob. Polecenie wysyła zapytanie do konta magazynu, aby powrócić do tego identyfikatora URI. Wartość identyfikatora URI jest przechowywana w nazwach zmiennych *bloburi*, która jest używana w następnym kroku.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Teraz Włącz diagnostyki rozruchu z [włączyć diagnostyki rozruchu az wirtualna](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_enable). `--storage` Wartość jest obiektu blob identyfikatora URI zebrane w poprzednim kroku.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Wyświetlanie diagnostyki rozruchu

Po włączeniu diagnostyki rozruchu zawsze zatrzymać i uruchomić maszynę Wirtualną, informacje na temat procesu rozruchu są zapisywane w pliku dziennika. W tym przykładzie najpierw cofnąć maszyny Wirtualnej z [deallocate wirtualna az](/cli/azure/vm#deallocate) polecenia w następujący sposób:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Teraz uruchom maszynę Wirtualną z [uruchomienia maszyny wirtualnej az]( /cli/azure/vm#stop) polecenia w następujący sposób:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Można uzyskać danych diagnostyki rozruchu dla *myVM* z [az maszyny wirtualnej diagnostyki rozruchu get rozruchu — dziennik](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_get_boot_log) polecenia w następujący sposób:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Wyświetlaj metryki hosta

Maszynę wirtualną systemu Linux zawiera dedykowany hosta na platformie Azure, która współdziała ona z. Metryki są automatycznie pobierane dla hosta i mogą być wyświetlane w portalu Azure w następujący sposób:

1. W portalu Azure kliknij **grup zasobów**, wybierz pozycję **myResourceGroupMonitor**, a następnie wybierz **myVM** na liście zasobów.
1. Aby zobaczyć, jak działa hosta maszyny Wirtualnej, kliknij **metryki** w bloku maszyny Wirtualnej, następnie wybierz jedno z *[Host]* metryki w obszarze **dostępne metryki**.

    ![Wyświetlaj metryki hosta](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Zainstaluj rozszerzenie diagnostyki

> [!IMPORTANT]
> Ten dokument zawiera opis wersji 2.3 rozszerzenia diagnostyczne systemu Linux, która została wycofana. Wersja 2.3 będzie obsługiwana do 30 czerwca 2018.
>
> Zamiast tego można włączyć w wersji 3.0 rozszerzenia diagnostyczne systemu Linux. Aby uzyskać więcej informacji, zobacz [dokumentacji](./diagnostic-extension.md).

Metryki podstawowych hostów są dostępne, ale bardziej szczegółowe i metryki specyficzne dla maszyny Wirtualnej, można, należy zainstalować rozszerzenie diagnostycznych platformy Azure na maszynie Wirtualnej. Rozszerzenia diagnostyki Azure umożliwia dodatkowe funkcje monitorowania i dane diagnostyczne mają zostać pobrane z maszyny Wirtualnej. Możesz wyświetlić te metryki wydajności i tworzyć alerty oparte na sposób wykonywania maszyny Wirtualnej. Rozszerzenia diagnostycznych jest zainstalowany za pośrednictwem portalu Azure w następujący sposób:

1. W portalu Azure kliknij **grup zasobów**, wybierz pozycję **myResourceGroup**, a następnie wybierz **myVM** na liście zasobów.
1. Kliknij przycisk **ustawienia diagnostyki**. Lista wskazuje, że *diagnostyki rozruchu* są już włączone w poprzedniej sekcji. Kliknij pole wyboru dla *podstawowe metryki*.
1. W *konta magazynu* sekcji, wyszukaj i wybierz *mydiagdata [1234]* konto utworzone w poprzedniej sekcji.
1. Kliknij przycisk **Zapisz**.

    ![Wyświetlaj metryki diagnostycznych](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Wyświetlaj metryki maszyny Wirtualnej

Metryki maszyny Wirtualnej można wyświetlać w taki sam sposób, aby wyświetlić hosta metryki maszyny Wirtualnej:

1. W portalu Azure kliknij **grup zasobów**, wybierz pozycję **myResourceGroup**, a następnie wybierz **myVM** na liście zasobów.
1. Aby zobaczyć, jak działa maszyny Wirtualnej, kliknij **metryki** w bloku maszyny Wirtualnej, a następnie wybierz jedno z metryki diagnostyki w obszarze **dostępne metryki**.

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

## <a name="manage-package-updates"></a>Zarządzanie aktualizacjami pakietu

Przy użyciu zarządzania aktualizacjami, można zarządzać pakietu aktualizacje i poprawki dla maszyn wirtualnych systemu Linux platformy Azure. Bezpośrednio z maszyny Wirtualnej, można szybko ocenić stan dostępne aktualizacje, zaplanowanie instalacji wymaganych aktualizacji i przejrzeć wyniki wdrożenia, aby sprawdzić, czy aktualizacje zostały pomyślnie zastosowane do maszyny Wirtualnej.

Aby uzyskać informacje o cenach, zobacz [ceny Automatyzacja zarządzania aktualizacjami](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management-preview"></a>Włącz zarządzanie aktualizacjami (wersja zapoznawcza)

Włącz zarządzanie aktualizacji dla maszyny Wirtualnej

1. Po lewej stronie ekranu, wybierz **maszyn wirtualnych**.
1. Z listy wybierz maszynę Wirtualną.
1. Na ekranie maszyny Wirtualnej w **operacji** kliknij **zarządzanie aktualizacjami**. **Zarządzania aktualizacjami włączyć** ekranu zostanie otwarta.

Weryfikacja jest przeprowadzana w celu ustalenia, czy zarządzanie aktualizacjami jest włączony dla tej maszyny Wirtualnej. Sprawdzanie poprawności obejmuje sprawdza, czy obszar roboczy analizy dzienników i połączonego konta automatyzacji, a jeśli rozwiązanie znajduje się w obszarze roboczym.

Obszar roboczy analizy dzienników służy do zbierania danych, który jest generowany przez funkcji i usług, takich jak zarządzanie aktualizacjami. Obszar roboczy zawiera pojedynczej lokalizacji, aby przejrzeć i analizowania danych z wielu źródeł. Do wykonania dodatkowych czynności na maszynach wirtualnych, które wymagają aktualizacji, usługi Automatyzacja Azure pozwala na uruchamianie skryptów przed maszynami wirtualnymi, takie jak do pobrania i zastosowania aktualizacji.

Proces weryfikacji sprawdza również, jeśli maszyna wirtualna jest udostępniane z programu Microsoft Monitoring Agent (MMA) i hybrydowy proces roboczy. Ten agent jest używany do komunikowania się z maszyną Wirtualną i uzyskiwania informacji o stanie aktualizacji. 

Te wymagania wstępne nie są spełnione, zostanie wyświetlony transparent daje możliwość włączenia rozwiązania.

![Zaktualizuj transparent dołączyć konfiguracji zarządzania](./media/tutorial-monitoring/manage-updates-onboard-solution-banner.png)

Kliknij transparent, aby włączyć rozwiązanie. W przypadku dowolnej z następujących warunków wstępnych zostały brakować po weryfikacji, zostaną automatycznie dodane:

* [Zaloguj się Analytics](../../log-analytics/log-analytics-overview.md) obszaru roboczego
* [Automatyzacja](../../automation/automation-offering-get-started.md)
* A [hybrydowy proces roboczy elementu runbook](../../automation/automation-hybrid-runbook-worker.md) jest włączona na maszynie Wirtualnej

**Włączyć zarządzanie aktualizacjami** ekranu zostanie otwarta. Skonfiguruj ustawienia, a następnie kliknij przycisk **włączyć**.

![Włącz rozwiązanie do zarządzania aktualizacji](./media/tutorial-monitoring/manage-updates-update-enable.png)

Włączanie rozwiązania może potrwać do 15 minut, a w tym czasie nie powinna zamykać okna przeglądarki. Po włączeniu rozwiązania, informacje o brakujących aktualizacji przez Menedżera pakietu na maszynie Wirtualnej przechodzi do analizy dzienników.
Może upłynąć od 30 minut do 6 godzin dla danych, które mają być dostępne dla analizy.

### <a name="view-update-assessment"></a>Wyświetlanie oceny aktualizacji

Po **zarządzanie aktualizacjami** rozwiązania jest włączona, **zarządzanie aktualizacjami** pojawi się ekran. Możesz wyświetlić listę brakujących aktualizacji na karcie **Brakujące aktualizacje**.

![Wyświetl stan aktualizacji](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Planowanie wdrożenia aktualizacji

Do instalowania aktualizacji, należy zaplanować wdrożenie znajdujący się okna harmonogramu i obsługa wersji.

Planowanie nowego wdrożenia aktualizacji dla maszyny Wirtualnej, klikając **harmonogram wdrożenia aktualizacji** w górnej części **zarządzanie aktualizacjami** ekranu. W **nowego wdrożenia aktualizacji** ekranu, podaj następujące informacje:

* **Nazwa** — wprowadź unikatową nazwę identyfikującą wdrożenie aktualizacji.
* **Aktualizacje, które mają zostać wykluczone** — wybierz, aby wprowadzić nazwy pakietów, które mają zostać wykluczone z aktualizacji.
* **Ustawienia harmonogramu** — możesz zaakceptować domyślną datę i godzinę (30 minut po bieżącej godzinie) lub określić inny czas. Możesz też określić, czy wdrożenie ma występować raz, czy zgodnie z ustawionym harmonogramem cyklicznym. Kliknij opcję Cyklicznie w obszarze Cykl, aby skonfigurować harmonogram cykliczny.

  ![Ekran ustawień harmonogramu aktualizacji](./media/tutorial-monitoring/manage-updates-schedule-linux.png)

* **Okno konserwacji (w minutach)** — podaj okres, w którym ma zostać przeprowadzone wdrażanie aktualizacji.  Dzięki temu, upewnij się, że zmiany są wykonywane w ramach Twojej zdefiniowanymi oknami obsługi. 

Po ukończeniu konfigurowania harmonogramu kliknij przycisk **Utwórz**, aby wrócić do pulpitu nawigacyjnego stanu.
Zwróć uwagę, że **zaplanowane** tabeli przedstawiono harmonogram wdrożenia został utworzony.

> [!WARNING]
> Maszyna wirtualna zostanie ponownie uruchomiony po zainstalowaniu aktualizacji, jeśli jest wystarczająco dużo czasu, w oknie obsługi.

Zarządzanie aktualizacjami stosuje istniejącego Menedżera pakietów na maszynie Wirtualnej w celu zainstalowania pakietów.

### <a name="view-results-of-an-update-deployment"></a>Wyświetlanie wyników wdrażania aktualizacji

Po rozpoczęciu zaplanowanego wdrażania stan tego wdrożenia można sprawdzić na karcie **Wdrożenia aktualizacji** na ekranie **Update Management**.
Jeśli wdrażanie trwa, wdrożenie ma stan **W toku**. Po pomyślnym ukończeniu wdrażania stan zmienia się na **Powodzenie**.
W przypadku awarii z co najmniej jednej aktualizacji we wdrożeniu, stan jest **niepowodzenie**.
Kliknij ukończone wdrożenie aktualizacji, aby wyświetlić pulpit nawigacyjny tego wdrożenia.

![Pulpit nawigacyjny stan wdrożenia aktualizacji dla określonego wdrożenia](./media/tutorial-monitoring/manage-updates-view-results.png)

W **wyniki aktualizacji** kafelka znajduje się podsumowanie całkowita liczba aktualizacji i wyniki wdrożenia na maszynie Wirtualnej.
Tabela po prawej stronie zawiera szczegółowy podział każdej aktualizacji i wyniki instalacji, które mogą mieć jedną z następujących wartości:

* **Nie podjęto** — aktualizacja nie została zainstalowana w powodu niewystarczających czas dostępne w oparciu o czas trwania okna obsługi, które zostały zdefiniowane.
* **Pomyślnie** -aktualizacja została pomyślnie pobrana i zainstalowana na maszynie Wirtualnej
* **Nie powiodło się** — nie można pobrać lub zainstalować na maszynie Wirtualnej aktualizacji.

Kliknij pozycję **Wszystkie dzienniki**, aby wyświetlić wszystkie wpisy dziennika utworzone przez wdrożenie.

Kliknij przycisk **dane wyjściowe** Kafelek, aby zobaczyć strumień zadań elementu runbook odpowiedzialny za zarządzanie wdrożenia aktualizacji w celu maszyny Wirtualnej.

Kliknij pozycję **Błędy**, aby wyświetlić szczegółowe informacje o błędach związanych z wdrożeniem.

## <a name="advanced-monitoring"></a>Zaawansowane monitorowanie 

Możliwość bardziej zaawansowane monitorowanie maszyny wirtualnej za pomocą [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Jeśli nie zostało to jeszcze zrobione, należy zarejestrować się w celu [bezpłatnej wersji próbnej](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) programu Operations Management Suite.

Jeśli masz dostęp do portalu OMS klucz obszaru roboczego i identyfikator obszaru roboczego można znaleźć w bloku ustawień. Zastąp < klucz obszaru roboczego > i < identyfikator obszaru roboczego > wartościami dla z Twojej OMS można użyć obszaru roboczego, a następnie możesz **zestaw rozszerzenia maszyny wirtualnej az** można dodać rozszerzenia OMS do maszyny Wirtualnej:

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

W bloku dziennika wyszukiwania portalu OMS, powinna zostać wyświetlona *myVM* takich jak co przedstawiono na poniższej ilustracji:

![Blok OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku został skonfigurowany, przejrzeć i zarządzane aktualizacje dla maszyny Wirtualnej. W tym samouczku omówiono:

> [!div class="checklist"]
> * Włącz diagnostykę rozruchu na maszynie Wirtualnej
> * Wyświetlanie diagnostyki rozruchu
> * Wyświetlaj metryki hosta
> * Włącz diagnostykę rozszerzenie na maszynie Wirtualnej
> * Wyświetlaj metryki maszyny Wirtualnej
> * Tworzenie alertów w oparciu metryki diagnostycznych
> * Zarządzanie aktualizacjami pakietu
> * Skonfiguruj zaawansowane monitorowanie

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure.

> [!div class="nextstepaction"]
> [Zarządzanie zabezpieczeniami maszyn wirtualnych](./tutorial-azure-security.md)
