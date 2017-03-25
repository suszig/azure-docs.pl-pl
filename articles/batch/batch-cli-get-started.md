---
title: "Wprowadzenie do interfejsu wiersza polecenia platformy Azure dla usługi Batch | Microsoft Docs"
description: "Skorzystaj z szybkiego wprowadzenia do poleceń usługi Batch w interfejsie wiersza polecenia platformy Azure, aby zarządzać zasobami usługi Azure Batch"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: fcd76587-1827-4bc8-a84d-bba1cd980d85
ms.service: batch
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 01/23/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 698c481e2eff5e0a3b893a0377d9f4cd2f052eb4
ms.lasthandoff: 03/21/2017


---
# <a name="manage-batch-resources-with-azure-cli"></a>Zarządzanie zasobami usługi Batch przy użyciu interfejsu wiersza polecenia platformy Azure

Interfejs wiersza polecenia platformy Azure obejmujący wiele platform (interfejs wiersza polecenia platformy Azure) umożliwia zarządzanie kontami i zasobami usługi Batch, np. pulami i zadaniami w powłokach poleceń systemów Linux, Mac i Windows. Za pomocą interfejsu wiersza polecenia platformy Azure można wykonywać oraz tworzyć skrypty dla wielu tych samych zadań, które wykonuje się za pomocą interfejsów API usługi Batch, witryny Azure Portal oraz poleceń cmdlet PowerShell usługi Batch.

Ten artykuł jest oparty na interfejsie wiersza polecenia platformy Azure w wersji 0.10.5.

## <a name="prerequisites"></a>Wymagania wstępne
* [Zainstalowanie interfejsu wiersza polecenia platformy Azure](../cli-install-nodejs.md)
* [Połączenie interfejsu wiersza polecenia platformy Azure z subskrypcją Azure](../xplat-cli-connect.md)
* Przełącz się w **tryb usługi Resource Manager**: `azure config mode arm`

> [!TIP]
> Zaleca się częstą aktualizację interfejsu wiersza polecenia platformy Azure, aby mieć możliwość korzystania z aktualizacji i rozszerzeń usługi.
> 
> 

## <a name="command-help"></a>Pomoc związana z poleceniami
Możesz wyświetlić tekst pomocy dla każdego polecenia w interfejsie wiersza polecenia platformy Azure, dodając `-h` jako jedyną opcję po poleceniu. Na przykład:

* Aby uzyskać pomoc dotyczącą polecenia `azure`, wprowadź: `azure -h`
* Aby uzyskać listę wszystkich poleceń usługi Batch w interfejsie wiersza polecenia, użyj: `azure batch -h`
* Aby uzyskać pomoc związaną z tworzeniem konta usługi Batch, wprowadź: `azure batch account create -h`

W razie wątpliwości użyj opcji wiersza polecenia `-h`, aby uzyskać pomoc dotyczącą jakiegokolwiek polecenia interfejsu wiersza polecenia platformy Azure.

## <a name="create-a-batch-account"></a>Tworzenie konta usługi Batch
Użycie:

    azure batch account create [options] <name>

Przykład:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Tworzy nowe konto usługi Batch z określonymi parametrami. Należy określić co najmniej lokalizację, grupę zasobów i nazwę konta. Jeśli nie masz jeszcze grupy zasobów, utwórz ją, uruchamiając polecenie `azure group create` i podając jeden z regionów świadczenia usługi Azure (np. „Zachodnie stany USA”) dla opcji `--location`. Na przykład:

    azure group create --name "resgroup001" --location "West US"

> [!NOTE]
> Nazwa konta usługi Batch musi być unikatowa w regionie usługi Azure, w którym konto zostanie utworzone. Może zawierać tylko małe znaki alfanumeryczne oraz mieć od 3 do 24 znaków. W nazwach kont usługi Batch nie możesz używać znaków specjalnych, takich jak `-` lub `_`.
> 
> 

### <a name="linked-storage-account-autostorage"></a>Połączone konto magazynu (autostorage)
Możesz (opcjonalnie) połączyć konto usługi Storage **ogólnego przeznaczenia** z kontem usługi Batch podczas jego tworzenia. Funkcja [pakietów aplikacji](batch-application-packages.md) usługi Batch korzysta z magazynu obiektów blob w ramach połączonego konta usługi Storage ogólnego przeznaczenia, podobnie jak biblioteka [.NET Batch File Conventions](batch-task-output.md). Te opcjonalne funkcje pomagają we wdrażaniu aplikacji uruchamianych przez zadania usługi Batch oraz utrwalaniu wytwarzanych przez nich danych.

Aby połączyć istniejące konto usługi Azure Storage z nowym kontem usługi Batch podczas jego tworzenia, określ opcję `--autostorage-account-id`. Ta opcja wymaga w pełni kwalifikowanego identyfikatora zasobu konta magazynu.

Najpierw podaj szczegóły dotyczące konta magazynu:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Następnie użyj wartości **Url** dla opcji `--autostorage-account-id`. Wartość Url rozpoczyna się od części „/subscriptions/” i zawiera identyfikator subskrypcji oraz ścieżkę zasobu prowadzącą do konta magazynu:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>Usuwanie konta usługi Batch
Użycie:

    azure batch account delete [options] <name>

Przykład:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Usuwa określone konto usługi Batch. Po wyświetleniu monitu potwierdź chęć usunięcia konta (usuwanie konta może potrwać trochę czasu).

## <a name="manage-account-access-keys"></a>Zarządzanie kluczami dostępu do konta
Potrzebujesz klucza dostępu, aby [utworzyć i zmodyfikować zasoby](#create-and-modify-batch-resources) na koncie usługi Batch.

### <a name="list-access-keys"></a>Lista kluczy dostępu
Użycie:

    azure batch account keys list [options] <name>

Przykład:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Wyświetla listę kluczy konta dla danego konta usługi Batch.

### <a name="generate-a-new-access-key"></a>Generowanie nowego klucza dostępu
Użycie:

    azure batch account keys renew [options] --<primary|secondary> <name>

Przykład:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Ponownie generuje określony klucz dostępu dla danego konta usługi Batch.

## <a name="create-and-modify-batch-resources"></a>Tworzenie i modyfikowanie zasobów usługi Batch
Możesz użyć interfejsu wiersza polecenia platformy Azure, aby tworzyć, odczytywać, aktualizować i usuwać zasoby usługi Batch (CRUD), np. pule, węzły obliczeniowe i zadania. Te operacje CRUD wymagają nazwy konta usługi Batch, klucza dostępu i punktu końcowego. Możesz je określić za pomocą opcji `-a`, `-k` i `-u` lub ustawić [zmienne środowiskowe](#credential-environment-variables), których interfejs wiersza polecenia będzie używać automatycznie (jeśli są wypełnione).

### <a name="credential-environment-variables"></a>Zmienne środowiskowe poświadczeń
Możesz ustawić zmienne środowiskowe `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY` i `AZURE_BATCH_ENDPOINT` zamiast określania opcji `-a`, `-k` i `-u` w wierszu polecenia dla każdego wykonywanego polecenia. Interfejs wiersza polecenia usługi Batch używa tych zmiennych (jeśli zostały ustawione), dlatego możesz pominąć opcje `-a`, `-k` i `-u`. W dalszej części tego artykułu założono użycie tych zmiennych środowiskowych.

> [!TIP]
> Wyświetl listę kluczy przy użyciu opcji `azure batch account keys list` i wyświetl punkt końcowy konta przy użyciu opcji `azure batch account show`.
> 
> 

### <a name="json-files"></a>Pliki JSON
Podczas tworzenia zasobów usługi Batch, np. puli i zadań, możesz określić plik JSON zawierający konfiguracje nowego zasobu, zamiast przekazywać parametry zasobu w opcjach wiersza polecenia. Na przykład:

`azure batch pool create my_batch_pool.json`

Możesz wykonać wiele operacji tworzenia zasobów, korzystając wyłącznie z opcji wiersza polecenia, niemniej niektóre funkcje wymagają pliku w formacie JSON zawierającego szczegółowe informacje o zasobie. Przykładowo musisz użyć pliku JSON, jeśli chcesz określić pliki zasobu dla zadania rozpoczęcia.

Aby znaleźć plik JSON wymagany do utworzenia zasobu, zapoznaj się z [dokumentacją interfejsu API REST usługi Batch][rest_api] w witrynie MSDN. Każdy temat „Dodawanie *typu zasobu*” zawiera przykładowy plik JSON do tworzenia zasobu, którego możesz użyć jako szablonu dla Twoich plików JSON. Na przykład plik JSON do tworzenia puli można znaleźć w temacie [Dodawanie puli do konta][rest_add_pool].

> [!NOTE]
> Jeśli określisz plik JSON podczas tworzenia zasobu, wszystkie inne parametry określone dla tego zasobu w wierszu polecenia zostaną zignorowane.
> 
> 

## <a name="create-a-pool"></a>Tworzenie puli
Użycie:

    azure batch pool create [options] [json-file]

Przykład (konfiguracja maszyny wirtualnej):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Przykład (konfiguracja usług Cloud Services):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Tworzy pulę węzłów obliczeniowych w usłudze Batch.

Jak wspomniano w [omówieniu funkcji Batch](batch-api-basics.md#pool), po wybraniu systemu operacyjnego dla węzłów puli dostępne są dwie opcje: **konfiguracja maszyny wirtualnej** i **konfiguracja usług Cloud Services**. Użyj opcji `--image-*`, aby utworzyć pule konfiguracji maszyny wirtualnej, lub opcji `--os-family`, aby utworzyć pule konfiguracji usług Cloud Services. Nie możesz określić jednocześnie opcji `--os-family` i `--image-*`.

Możesz określić [pakiety aplikacji](batch-application-packages.md) puli i wiersz polecenia dla [zadania rozpoczęcia](batch-api-basics.md#start-task). Niemniej aby określić pliki zasobów dla zadania rozpoczęcia, musisz użyć [pliku JSON](#json-files).

Usuń pulę z:

    azure batch pool delete [pool-id]

> [!TIP]
> Sprawdź [listę obrazów maszyn wirtualnych](batch-linux-nodes.md#list-of-virtual-machine-images) dla wartości odpowiednich dla opcji `--image-*`.
> 
> 

## <a name="create-a-job"></a>Tworzenie zadania
Użycie:

    azure batch job create [options] [json-file]

Przykład:

    azure batch job create --id "job001" --pool-id "pool001"

Dodaje zadanie do konta usługi Batch i określa pulę wykonywania zadania.

Usuń zadanie z:

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Wyświetlanie listy puli, zadań i innych zasobów
Każdy typ zasobu usługi Batch obsługuje polecenie `list`, które wysyła zapytania do konta usługi Batch i wyświetla listę zasobów tego typu. Przykładowo możesz wyświetlić listę puli na koncie i zadań w ramach zadania:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Efektywne wyświetlanie zasobów
Możesz przyspieszyć wysyłanie zapytań, określając opcje klauzuli **wybierz**, **filtr** i **rozwiń** dla operacji `list`. Użyj tych opcji, aby ograniczyć ilość danych zwracanych przez usługę Batch. Ponieważ całe filtrowanie odbywa się po stronie serwera, tylko dane, które Cię interesują, zostają przekazane podczas transmisji. Użyj tych klauzul, aby oszczędzić przepustowość (i czas) podczas wykonywania operacji związanych z wyświetlaniem listy.

Przykładowo następujące polecenie zwróci tylko pule, których identyfikatory rozpoczynają się od frazy „renderTask”:

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

Interfejs wiersza polecenia usługi Batch obsługuje wszystkie trzy rodzaje klauzul obsługiwane przez usługę Batch:

* `--select-clause [select-clause]` Zwracanie podzbioru właściwości dla każdej jednostki
* `--filter-clause [filter-clause]` Zwracanie tylko jednostek, które pasują do określonego wyrażenia OData
* `--expand-clause [expand-clause]` Uzyskiwanie informacji dotyczących jednostki w pojedynczym, podstawowym wywołaniu REST Klauzula expand obsługuje obecnie tylko właściwość `stats`.

Aby uzyskać informacje o trzech klauzulach i wykonywaniu zapytań listy przy ich użyciu, zobacz temat [Query the Azure Batch service efficiently](batch-efficient-list-queries.md) (Efektywne wysyłanie zapytań do usługi Azure Batch).

## <a name="application-package-management"></a>Zarządzanie pakietem aplikacji
Pakiety aplikacji zapewniają uproszczony sposób na wdrażanie aplikacji do węzłów obliczeniowych w pulach. Przy użyciu interfejsu wiersza polecenia platformy Azure można przekazywać pakiety aplikacji, zarządzać wersjami pakietów i usuwać pakiety.

Aby utworzyć nową aplikację i dodać wersję pakietu:

**Tworzenie** aplikacji:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Dodawanie** pakietu aplikacji:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Aktywacja** pakietu:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

Ustaw **wersję domyślną** aplikacji:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Wdrażanie pakietu aplikacji
Podczas tworzenia nowej puli możesz określić co najmniej jeden pakiet aplikacji dla wdrożenia. Jeśli określisz pakiet w czasie tworzenia puli, zostanie wdrożony w każdym węźle w przypadku dołączenia węzła do puli. Pakiety są też wdrażane, gdy węzeł zostaje uruchomiony ponownie lub odtworzony z obrazu.

Określ opcję `--app-package-ref` podczas tworzenia puli, aby wdrożyć pakiet aplikacji do węzłów dołączanych do puli. Opcja `--app-package-ref` akceptuje rozdzielaną średnikami listę identyfikatorów aplikacji do wdrożenia w węzłach obliczeniowych.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

W przypadku tworzenia puli za pomocą opcji wiersza polecenia nie można obecnie określić wersji pakietu aplikacji *do wdrożenia* w węzłach obliczeniowych, na przykład „1.10-beta3”. Dlatego przed utworzeniem puli (zobacz poprzednią sekcję) musisz określić domyślną wersję aplikacji przy użyciu opcji `azure batch application set [options] --default-version <version-id>`. Możesz jednak określić wersję pakietu dla puli, jeśli używasz [pliku JSON](#json-files) zamiast opcji wiersza polecenia podczas tworzenia puli.

Więcej informacji dotyczących pakietów aplikacji można znaleźć w temacie [Application deployment with Azure Batch application packages](batch-application-packages.md) (Wdrażanie aplikacji za pomocą pakietów aplikacji usługi Azure Batch).

> [!IMPORTANT]
> Najpierw [połącz konto usługi Azure Storage](#linked-storage-account-autostorage) z kontem usługi Batch, aby użyć pakietów aplikacji.
> 
> 

### <a name="update-a-pools-application-packages"></a>Aktualizowanie pakietów aplikacji puli
Aby zaktualizować aplikacje przypisane do istniejącej puli, wydaj polecenie `azure batch pool set` za pomocą opcji `--app-package-ref`:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Aby wdrożyć nowy pakiet aplikacji w węzłach obliczeniowych znajdujących się już w istniejącej puli, musisz ponownie uruchomić te węzły lub odtworzyć je z obrazu:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

> [!TIP]
> Aby uzyskać listę węzłów w puli wraz z ich identyfikatorami, użyj polecenia `azure batch node list`.
> 
> 

Skonfigurowanie domyślnej wersji aplikacji było wymagane przed wdrożeniem (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Wskazówki dotyczące rozwiązywania problemów
Ta sekcja ma na celu zapewnienie zasobów do użycia w przypadku rozwiązywania problemów związanych z interfejsem wiersza polecenia platformy Azure. Niekoniecznie rozwiąże ona wszystkie problemy, ale może pomóc w zawężeniu przyczyny i wskazać dalsze zasoby pomocy.

* Użyj polecenia `-h`, aby uzyskać **tekst pomocy** dla dowolnego polecenia interfejsu wiersza polecenia
* Użyj opcji `-v` i `-vv`, aby wyświetlić **pełne** dane wyjściowe polecenia; `-vv` jest „dodatkową” formą pełnych informacji i wyświetla faktyczne żądania i odpowiedzi REST. Te przełączniki są przydatne do wyświetlania pełnych danych wyjściowych błędu.
* Możesz wyświetlić **dane wyjściowe polecenia w formie pliku JSON** przy użyciu opcji `--json`. Przykładowo polecenie `azure batch pool show "pool001" --json` wyświetla właściwości puli&001; w formacie JSON. Następnie możesz skopiować i zmodyfikować te dane wyjściowe, aby użyć ich w pliku `--json-file` (zobacz sekcję [Pliki JSON](#json-files) wcześniej w tym artykule).
* [Forum usługi Batch w witrynie MSDN][batch_forum] jest doskonałym zasobem pomocy i jest uważnie monitorowane przez członków zespołu usługi Batch. Upewnij się, że zamieszczasz tam pytania w przypadku napotkania problemów lub w sytuacji, w której potrzebujesz pomocy z konkretną operacją.
* Nie każda operacja na zasobach usługi Batch jest obecnie obsługiwana przez interfejs wiersza polecenia platformy Azure. Przykładowo nie możesz obecnie określić *wersji* pakietu aplikacji dla puli. Możesz określić tylko identyfikator pakietu. W takich przypadkach konieczne będzie dostarczenie pliku `--json-file` dla polecenia zamiast używania opcji wiersza polecenia. Upewnij się, że jesteś na bieżąco, korzystając z najnowszej wersji interfejsu wiersza polecenia, aby pozyskiwać przyszłe ulepszenia.

## <a name="next-steps"></a>Następne kroki
* Zobacz temat [Application deployment with Azure Batch application packages](batch-application-packages.md) (Wdrażanie aplikacji za pomocą pakietów aplikacji w usłudze Azure Batch), aby zobaczyć, jak używać tej funkcji do zarządzania i wdrażania aplikacji wykonywanych w węzłach obliczeniowych usługi Batch.
* Więcej informacji na temat zmniejszenia liczby elementów oraz typu informacji zwracanych dla zapytań w usłudze Batch znajduje się w temacie [Query the Batch service efficiently](batch-efficient-list-queries.md) (Skuteczne wykonywanie zapytań w usłudze Batch).

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx

