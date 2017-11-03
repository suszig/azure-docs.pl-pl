---
title: "Samouczek artykułu dla funkcji w wersji zapoznawczej usługi Azure Machine Learning - interfejsu wiersza polecenia | Dokumentacja firmy Microsoft"
description: "Ten samouczek przeszukiwania wszystkie kroki wymagane w celu zakończenia Iris klasyfikacji end-to-end z interfejsu wiersza polecenia."
services: machine-learning
author: ahgyger
ms.author: ahgyger, ritbhat
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 10/15/2017
ms.openlocfilehash: 453c774c97b77dd7829a50fa5e5668d06f817a1d
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2017
---
# <a name="tutorial-classifying-iris-using-the-command-line-interface"></a>Samouczek: Klasyfikowania Iris przy użyciu interfejsu wiersza polecenia
Usługa Azure Machine Learning (wersja zapoznawcza) stanowi zintegrowane, kompleksowe rozwiązanie do nauki o danych i do analiz zaawansowanych przeznaczone dla profesjonalnych analityków, którzy będą z niego korzystać w celu przygotowywania danych, opracowywania eksperymentów i wdrażania modeli na skalę chmury.

Z tego samouczka dowiesz się, za pomocą narzędzi interfejsu wiersza polecenia (CLI) w wersji zapoznawczej usługi Azure Machine Learning do: 
> [!div class="checklist"]
> * Konfigurowanie konta eksperymenty i tworzenie obszaru roboczego
> * Tworzenie projektu
> * Przedstawia eksperymentu do wielu elementów docelowych obliczeń
> * Podwyższ poziom i zarejestruj uczonego modelu
> * Wdrażanie usługi sieci web, aby oceniać nowe dane

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Wymagania wstępne
- Potrzebny jest dostęp do subskrypcji platformy Azure i uprawnienia do tworzenia zasobów w danej subskrypcji. 
- Należy zainstalować aplikację Azure maszyny Learing Workbench wykonując [zainstalować i utworzyć szybkiego startu](quickstart-installation.md). 

  >[!NOTE]
  >Musisz zainstalować usługi Azure Machine Learning Workbench lokalnie. Wystarczy wykonaj kroki opisane w sekcjach prawo zainstalować Azure Machine Learning Workbench od czasu utworzenia konta i Utwórz nowy projekt, który kroki zostaną wykonane przy użyciu wiersza polecenia w tym artykule.
 
## <a name="getting-started"></a>Wprowadzenie
Azure Machine Learning interfejsu wiersza polecenia (CLI) umożliwia wykonywanie wszystkich zadań wymaganych do pracy do analizy danych na całej trasie. Aby dostęp do narzędzi interfejsu wiersza polecenia w następujący sposób:

### <a name="option-1-launch-azure-ml-cli-from-azure-ml-workbench-log-in-dialog-box"></a>Opcja 1. Uruchamianie interfejsu wiersza polecenia Azure ML w Azure ML Workbench dziennika w oknie dialogowym
Podczas uruchamiania narzędzia Workbench uczenie Maszynowe Azure i zaloguj się po raz pierwszy, a jeśli nie masz już dostępu do konta eksperymenty, otrzymasz następujący ekran:

![Nie można odnaleźć konta](media/tutorial-iris-azure-cli/no_account_found.png)

Polecenie **okno wiersza polecenia** łącze w oknie dialogowym, aby uruchomić okno wiersza polecenia.

### <a name="option-2-launch-azure-ml-cli-from-azure-ml-workbench-app"></a>Opcja 2. Uruchamianie interfejsu wiersza polecenia Azure ML z aplikacji Azure ML Workbench
Jeśli masz już dostępu do konta eksperymenty, możesz zalogować się pomyślnie. A następnie możesz otworzyć okno wiersza polecenia, klikając **pliku** --> **Otwórz monitu polecenie** menu.

### <a name="option-3-enable-azure-ml-cli-in-an-arbitrary-command-line-window"></a>Opcja 3. Włącz interfejsu wiersza polecenia Azure ML dowolnego okno wiersza polecenia
Można również włączyć interfejsu wiersza polecenia Azure ML w dowolnym oknie wiersza polecenia. Po prostu Uruchom okno poleceń, a następnie wprowadź następujące polecenia:

```sh
# Windows Command Prompt
set PATH=%LOCALAPPDATA%\amlworkbench\Python;%LOCALAPPDATA%\amlworkbench\Python\Scripts;%PATH%

# Windows PowerShell
$env:Path = $env:LOCALAPPDATA+"\amlworkbench\Python;"+$env:LOCALAPPDATA+"\amlworkbench\Python\Scripts;"+$env:Path

# macOS Bash Shell
PATH=$HOME/Library/Caches/AmlWorkbench/Python/bin:$PATH
```
Aby zmiany były trwałe, można użyć `SETX` w systemie Windows. System macOS, można użyć `setenv`.

>[!TIP]
>Interfejsu wiersza polecenia Azure można włączyć w Twoje ulubione okno terminalu, ustawiając powyżej zmiennych środowiskowych.

## <a name="step-1-log-in-to-azure"></a>Krok 1. Zaloguj się do platformy Azure.
Pierwszym krokiem jest otwarcie interfejsu wiersza polecenia z poziomu aplikacji AMLWorkbench (Plik > Otwórz wiersz polecenia). Daje to gwarancję, używamy środowiska python w prawo i mamy dostępne polecenia ML interfejsu wiersza polecenia. 

Następnie należy ustawić kontekst prawo w interfejs wiersza polecenia dostępu i zarządzania zasobami Azure.
 
```azure-cli
# log in
$ az login

# list all subscriptions
$ az account list -o table

# set the current subscription
$ az account set -s <subscription id or name>
```

## <a name="step-2-create-a-new-azure-machine-learning-experimentation-account-and-workspace"></a>Krok 2. Utwórz nowe konto usługi Azure Machine Learning eksperymenty i obszaru roboczego
Firma Microsoft Rozpocznij od utworzenia nowego konta eksperymenty i nowy obszar roboczy. Zobacz [pojęcia dotyczące usługi Azure Machine Learning](overview-general-concepts.md) Aby uzyskać więcej informacji o kontach eksperymenty i obszarów roboczych.

> [!NOTE]
> Konta eksperymenty wymagają konta magazynu, który jest używany do przechowywania danych wyjściowych z przebiegów eksperymentu. Nazwa konta magazynu musi być globalnie unikatowa na platformie Azure, ponieważ jest skojarzony z nim adresem url. Jeśli nie określisz istniejącego konta magazynu, nazwę konta eksperymenty służy do tworzenia nowego konta magazynu. Upewnij się, że Użyj unikatowej nazwy lub będzie wyświetlany komunikat o błędzie, takich jak _"konto magazynu o nazwie \<storage_account_name > jest już zajęta."_ Alternatywnie można użyć `--storage` argument można podać istniejącego konta magazynu.

```azure-cli
# create a resource group 
$ az group create --name <resource group name> --location <supported Azure region>

# create a new experimentation account with a new storage account
$ az ml account experimentation create --name <experimentation account name> --resource-group <resource group name>

# create a new experimentation account with an existing storage account
$ az ml account experimentation create --name <experimentation account name>  --resource-group <resource group name> --storage <storage account Azure Resource ID>

# create a workspace in the experimentation account
az ml workspace create --name <workspace name> --account <experimentation account name> --resource-group <resource group name>
```

## <a name="step-2a-optional-share-a-workspace-with-co-worker"></a>Krok 2.a (opcjonalnie) udostępniać współpracownika obszaru roboczego
W tym miejscu możemy Poznaj jak udostępniać dostęp do obszaru roboczego współpracownika. Kroki w celu udostępnienia dostęp do konta eksperymenty lub projekt będzie taki sam. Jedynym sposobem uzyskania Identyfikatora zasobu Azure czy muszą zostać zaktualizowane.

```azure-cli
# find the workspace Azure Resource ID
$az ml workspace show --name <workspace name> --account <experimentation account name> --resource-group <resource group name>

# add Bob to this workspace as a new owner
$az role assignment create --assignee bob@contoso.com --role owner --scope <workspace Azure Resource ID>
```

> [!TIP]
> `bob@contoso.com`w poleceniu powyżej musi być prawidłową tożsamość usługi Azure AD w katalogu, gdzie należy bieżącej subskrypcji.

## <a name="step-3-create-a-new-project"></a>Krok 3. Tworzenie nowego projektu
Nasze następnym krokiem jest do utworzenia nowego projektu. Istnieje kilka sposobów na rozpoczęcie pracy z nowym projektem.

### <a name="create-a-new-blank-project"></a>Utwórz nowy projekt puste

```azure-cli
# create a new project
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path>
```

### <a name="create-a-new-project-with-a-default-project-template"></a>Utwórz nowy projekt z domyślny szablon projektu
Można utworzyć nowy projekt z szablonu domyślnego.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template
```

### <a name="create-a-new-project-associated-with-a-cloud-git-repository"></a>Tworzenie nowego projektu powiązanego z chmurą repozytorium Git
Można utworzyć nowy projekt skojarzonych z repozytorium Git programu VSTS (Visual Studio Team Service). Za każdym razem, gdy przesłaniu eksperyment, migawka folderu cały projekt jest przekazane do zdalnego repozytorium Git. Zobacz [repozytorium Git przy użyciu usługi Azure Machine Learning Workbench projektu](using-git-ml-project.md) więcej szczegółów.

> [!NOTE]
> Usługa Azure Machine Learning obsługuje tylko puste repozytoriów Git utworzone w VSTS.

```azure-cli
$ az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --repo <VSTS repo URL>
```
> [!TIP]
> Wystąpił błąd w przypadku uzyskiwania "adres url repozytorium może być nieprawidłowy lub użytkownik może nie mieć dostępu", można utworzyć tokenu zabezpieczającego w VSTS (w obszarze _zabezpieczeń_, _dodać osobiste tokeny dostępu_ menu) i używać `--vststoken`argumentów podczas tworzenia projektu. 

### <a name="sample_create"></a>Utwórz nowy projekt z próbki
W tym przykładzie utworzymy nowy projekt za pomocą przykładowy projekt jako szablon.

```azure-cli
# List the project samples, find the Classifying Iris sample
$ az ml project sample list

# Create a new project from the sample
az ml project create --name <project name> --workspace <workspace name> --account <experimentation account name> --resource-group <resource group name> --path <local folder path> --template-url https://github.com/MicrosoftDocs/MachineLearningSamples-Iris
```
Po utworzeniu projektu, użyj `cd` polecenie, aby wprowadzić katalogu projektu.

## <a name="step-4-run-the-training-experiment"></a>Krok 4 Uruchom eksperyment uczenia 
Poniższe kroki założono, że istnieje projekt z przykładowych Iris (zobacz [Utwórz nowy projekt z online próbki](#sample_create)).

### <a name="prepare-your-environment"></a>Przygotowywanie środowiska 
Dla przykładu Iris musimy zainstalować matplotlib.

```azure-cli
$ pip install matplotlib
```

###  <a name="submit-the-experiment"></a>Przedstawia eksperymentu

```azure-cli
# Execute the file
$ az ml experiment submit --run-configuration local iris_sklearn.py
```

### <a name="iterate-on-your-experiment-with-descending-regularization-rates"></a>Iterację eksperymentu z malejącej uregulowania szybkości
Z niektórych kreatywności jest proste, można opracować skrypt języka Python, który przesyła eksperymenty z różnych uregulowania stawki. (Może być konieczne edytować plik, aby wskazać ścieżkę projektu.)

```azure-cli
$ python run.py
```

## <a name="step-5-view-run-history"></a>Krok 5. Widok historii uruchamiania
Następujące polecenie wyświetla listę wszystkich poprzednich uruchamia wykonywane. 

```azure-cli
$ az ml history list -o table
```
Powyższe polecenia spowoduje wyświetlenie listy wszystkich uruchomień należących do tego projektu. Widać zbyt wymieniony dokładność i uregulowania metryki szybkości. Ta upewnij go łatwo zidentyfikować najlepszą Uruchom z listy.

## <a name="step-5a-view-attachment-created-by-a-given-run"></a>Krok załącznika widoku 5.a utworzone przez dany Uruchom 
Aby wyświetlić załącznik skojarzone z danym uruchomieniu, możemy użyć polecenia informacji historii wykonywania. Znajdź identyfikator uruchomienia w określonym przebiegu z powyższej listy.

```azure-cli
$ az ml history info --run <run id> --artifact driver_log
```

Aby pobrać artefaktów z działania, można użyć poniższych poleceń:

```azure-cli
# Stream a given attachment 
$ az ml history info --run <run id> --artifact <artifact location>
```

## <a name="step-6-promote-artifacts-of-a-run"></a>Krok 6. Podwyższ poziom artefakty Uruchom 
Jeden z przebiegi, które wprowadziliśmy ma lepszą AUC, dlatego chcemy go użyć do utworzenia oceniania usługi sieci web do wdrożenia w środowisku produkcyjnym. Aby to zrobić, najpierw musimy wspierania artefakty do elementu zawartości.

```azure-cli
$ az ml history promote --run <run id> --artifact-path outputs/model.pkl --name model.pkl
```

Spowoduje to utworzenie `assets` folderu w katalogu projektu z `model.pkl.link` pliku. Ten plik łącza jest używany do odwołania awansowana zasobów.

## <a name="step-7-download-the-files-to-be-operationalized"></a>Krok 7. Pobierz pliki, aby być operationalized
Teraz musisz pobrać awansowana modelu, więc ich używać do tworzenia naszej usługi sieci web prognozowania. 

```azure-cli
$ az ml asset download --link-file assets\pickle.link -d asset_download
```

## <a name="step-8-setup-your-model-management-environment"></a>Krok 8. Ustawienia środowiska zarządzania modelu 
Możemy utworzyć środowisko do wdrażania usług sieci web. Firma Microsoft można uruchomić usługi sieci web na komputerze lokalnym przy użyciu rozwiązania Docker. Albo wdrożyć go do klastra usługi ACS dla operacji wysokiej skali. 

```azure-cli
# Create new local operationalization environment
$ az ml env setup -l <supported Azure region> -n <env name>
# Once setup is complete, set your environment for current context
$ az ml env set -g <resource group name> -n <env name>
```

## <a name="step-9-create-a-model-management-account"></a>Krok 9. Utwórz konto zarządzania modelu 
Konto zarządzania model jest wymagany do wdrażania i śledzenia modeli w środowisku produkcyjnym. 

```azure-cli
$ az ml account modelmanagement create -n <model management account name> -g <resource group name> -l <supported Azure region>
```

## <a name="step-10-create-a-web-service"></a>Krok 10. Tworzenie usługi sieci Web
Następnie utworzymy usługi sieci web, która zwraca prognozowania, za pomocą modelu wdrożone. 

```azure-cli
$ az ml service create realtime -m asset_download/model.pkl -f score.py -r python –n <web service name>
```

## <a name="step-10-run-the-web-service"></a>Krok 10. Uruchom usługę sieci web
Przy użyciu identyfikatora usługi sieci web z danych wyjściowych w poprzednim kroku, możemy wywoływania usługi sieci web i przetestować go. 

```azure-cli
# Get web service usage infomration
$ az ml service usage realtime -i <web service id>

# Call the web service with the run command:
$ az ml service run realtime -i <web service id> -d <input data>
```

## <a name="deleting-all-the-resources"></a>Usuwanie wszystkich zasobów 
Załóżmy wykonania kroków tego samouczka, usuwając wszystkie zasoby, który został utworzony, chyba że chcesz kontynuować pracę na nim! 

Aby to zrobić, możemy po prostu usuń grupę zasobów, zawierający naszych zasobów. 

```azure-cli
az group delete --name <resource group name>
```

## <a name="next-steps"></a>Następne kroki
W tym samouczku wiesz już korzystać z funkcji wersji zapoznawczej usługi Azure Machine Learning do 
> [!div class="checklist"]
> * Konfigurowanie konta eksperymenty, tworzenie obszaru roboczego
> * Tworzenie projektów
> * Przedstawia eksperymenty do wielu obliczeń elementu docelowego.
> * Podwyższ poziom i zarejestruj uczonego modelu
> * Utwórz konto zarządzania modelu dla modelu zarządzania
> * Utwórz środowisko do wdrażania usługi sieci web
> * Wdrażanie usługi sieci web i wynik nowymi danymi