---
title: "Przy użyciu repozytorium Git w usłudze Azure Machine Learning Workbench projektu | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób korzystania z repozytorium Git w połączeniu z projektem Workbench Azure Machine Learning."
services: machine-learning
author: hning86
ms.author: haining
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/18/2017
ms.openlocfilehash: 0cd447a52964578dd2348a786dd57a45ea87516e
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2017
---
# <a name="using-git-repository-with-an-azure-machine-learning-workbench-project"></a>Przy użyciu repozytorium Git w usłudze Azure Machine Learning Workbench projektu
Ten dokument zawiera informacje na Azure Machine Learning Workbench używaniu Git do zapewnienia kontroli wersji i upewnij się, powtarzalności w eksperymencie analizy danych. Podawane są również instrukcje dotyczące sposobu kojarzenia projektu z chmurą repozytorium Git.

## <a name="introduction"></a>Wprowadzenie
Azure Machine Learning Workbench zaprojektowano z Integracja z usługą Git od podstaw w górę. Podczas tworzenia nowego projektu, folder projektu jest automatycznie "Git inicjowany" jako lokalnego repozytorium Git (repozytorium). W tym samym czasie, drugi ukryte lokalnego repozytorium Git tworzona jest również o gałąź o nazwie _AzureMLHistory / < project_GUID >_ do śledzenia zmian folderu projektu dla każdego wykonywania. 

Kojarzenie projektu uczenie Maszynowe Azure z repozytorium Git, hostowana w ramach projektu programu Visual Studio Team usługi (VSTS), umożliwia automatyczne kontroli wersji, zarówno lokalnie, jak i zdalnie. To skojarzenie umożliwia każdy z dostępem do zdalnego repozytorium Pobierz najnowsze kodu źródłowego do innego komputera (mobilnych).  

> [!NOTE]
> VSTS ma własną listy kontroli dostępu, która jest niezależna od Azure Machine Learning eksperymenty usługi. Dostęp użytkownika może się różnić od repozytorium Git do obszaru roboczego uczenia Maszynowego Azure lub projektu i może być konieczne zarządzanie. Jeśli chcesz udostępnić projektu uczenia Maszynowego Azure za pomocą elementu członkowskiego zespołu, w tym kod dostępu na poziomie, oprócz tylko udostępnić obszaru roboczego, musisz jawnie przyznać mu właściwy dostęp do repozytorium Git programu VSTS. 

Za pomocą narzędzia Git, istnieje również możliwość zarządzania kontroli wersji jawnie przy użyciu _wzorca_ gałąź lub przez tworzenie innych gałęzi w repozytorium. Można użyć lokalnego repozytorium Git, a jeśli udostępniane również może bezzwłocznie przekazywać do zdalnego repozytorium Git.

Ten diagram przedstawia relacje między repozytorium Git programu VSTS a projektu uczenie Maszynowe Azure:

![AML Git](media/using-git-ml-project/aml_git.png)

Aby rozpocząć korzystanie ze zdalnego repozytorium Git, wykonaj następujące instrukcje podstawowe.

> [!NOTE]
> Uczenie maszynowe Azure obsługuje obecnie tylko repozytoria Git na kontach usługi VSTS. W przyszłości planowane jest obsługa dla repozytoriów Git ogólne (na przykład GitHub i itp.).

## <a name="step-1-create-an-azure-ml-experimentation-account"></a>Krok 1. Tworzenie konta usługi Azure ML eksperymenty
Jeśli nie, już, tworzenie konta usługi Azure ML eksperymenty i zainstaluj aplikację Azure ML Workbench. Dowiedz się więcej w [zainstalować i utworzyć Szybki Start](quickstart-installation.md).

## <a name="step-2-create-a-team-project-or-use-an-existing-team-project"></a>Krok 2. Tworzenie projektu zespołowego lub użyj istniejącego projektu zespołowego
Z [portalu Azure](https://portal.azure.com/), Utwórz nową **projektu zespołowego**.
1. Kliknij pozycję**+**
2. Wyszukaj **"Projektu zespołowego"**
3. Wprowadź wymagane informacje.
    - Nazwa: Nazwa zespołu.
    - Kontrola wersji: **Git**
    - Subskrypcja: Jedną przy użyciu konta Azure Machine Learning eksperymenty.
    - Lokalizacja: Najlepiej pozostają w regionie, który znajduje się w pobliżu zasobów platformy Azure Machine Learning eksperymentu.
4. Kliknij przycisk **Utwórz**. 

![Tworzenie projektu zespołowego przy użyciu portalu Azure](media/using-git-ml-project/create_vsts_team.png)

Upewnij się, że możesz zalogować się przy użyciu tego samego konta usługi Azure Active Directory (AAD), którego używasz dostępu Azure Machine Learning Workbench. W przeciwnym razie system nie może uzyskać dostępu przy użyciu poświadczeń użytkownika usługi AAD, chyba że za pomocą wiersza polecenia Utwórz projekt uczenie Maszynowe Azure i podanie osobisty token dostępu do repozytorium Git. Więcej informacji na temat to później.

Po utworzeniu projektu zespołowego, można przystąpić do przeniesienia do następnego kroku.

Aby przejść bezpośrednio do projektu zespołowego właśnie utworzony, adres URL jest `https://<team_project_name>.visualstudio.com`.

## <a name="step-3-create-a-new-azure-ml-project-with-a-remote-git-repo"></a>Krok 3. Utwórz nowy projekt uczenie Maszynowe Azure ze zdalnego repozytorium Git
Uruchom usługi Azure ML Workbench i Utwórz nowy projekt. Adres URL repozytorium Git programu VSTS, który można pobrać z etapu 2 wypełnić pole tekstowe repozytorium Git. Zwykle wygląda następująco:`http://<vsts_account_name>.visualstudio.com/_git/<project_name>`

![Utwórz projekt uczenie Maszynowe Azure z repozytorium Git](media/using-git-ml-project/create_project_with_git_rep.png)

Można również utworzyć projekt za pomocą narzędzia wiersza polecenia. Istnieje możliwość udzielenia osobisty token dostępu. Uczenie Maszynowe Azure umożliwia ten token dostępu repozytorium Git w Twoim imieniu, zamiast polegania na Twoje poświadczenia usługi AAD:

```
# create a new project with a Git repo and personal access token.
$ az ml project create -a <experimentation account name> -n <project name> -g <resource group name> -w <workspace name> -r <Git repo URL> --vststoken <VSTS personal access token>
```
> [!IMPORTANT]
> Jeśli wybierzesz szablon projektu puste, jeśli repozytorium Git, możesz wybrać już jest OK _wzorca_ gałęzi. Po prostu klonów Azure ML _wzorca_ gałęzi lokalnie, a następnie dodaj `aml_config` folderu i inne pliki metadanych do folderu lokalnego projektu projektu. Ale jeśli wybierzesz innego szablonu projektu z repozytorium Git nie mogą już mieć _wzorca_ gałęzi, lub zostanie wyświetlony komunikat o błędzie. Alternatywą jest użycie `az ml project create` narzędzia wiersza polecenia, aby utworzyć projekt i podać `--force` przełącznika. Usuwa pliki na oryginalnym głównej gałęzi i Zamień nowe pliki w wybranego szablonu.

Teraz nowy projekt uczenie Maszynowe Azure jest tworzony z zdalnego integracji repozytorium Git, włączona i gotowa do go. Folder projektu jest zawsze Git zainicjowany jako lokalne repozytorium Git. I Git _zdalnego_ ustawiono zdalnego repozytorium Git programu VSTS, więc zatwierdzeń może zostać przeniesiony do zdalnego repozytorium Git.

## <a name="step-3a-associate-an-existing-azure-ml-project-with-a-vsts-git-repo"></a>Krok 3a. Skojarz istniejący projekt uczenie Maszynowe Azure z repozytorium Git programu VSTS
Opcjonalnie można również utworzyć projekt uczenie Maszynowe Azure bez repozytorium Git programu VSTS i polegać tylko na lokalne repozytorium Git dla migawki historii wykonywania. I repozytorium Git programu VSTS można skojarzyć później z tym istniejący projekt uczenie Maszynowe Azure za pomocą następującego polecenia:

```azurecli
# make sure you are in the project path so CLI has context of your current project
$ az ml project update --repo http://<vsts_account_name>.visualstudio.com/_git/<project_name>
```

> [!NOTE] 
> Można wykonać tylko operację aktualizacji repozytorium w projekcie uczenie Maszynowe Azure, która nie ma repozytorium Git skojarzonego z nim. I po repozytorium Git jest skojarzony, nie może zostać usunięta.

## <a name="step-4-capture-project-snapshot-in-git-repo"></a>Krok 4. Przechwytywanie migawek projektu w repozytorium Git
Teraz możesz wykonać kilka skrypt będzie uruchamiany w projekcie, należy między nimi pewne zmiany działa. Można to zrobić, albo z aplikacji pulpitu lub przy użyciu interfejsu wiersza polecenia `az ml experiment submit` polecenia. Aby uzyskać więcej informacji, możesz wykonać [samouczek klasyfikowania Iris](tutorial-classifying-iris-part-1.md). Przy każdym uruchomieniu, jeśli zmiany wprowadzone w plikach w folderze projektu migawki folderu cały projekt jest zadeklarowane i przypisany do zdalnego repozytorium Git w gałęzi o nazwie `AzureMLHistory/<Project_GUID>`. Można wyświetlać gałęzi i zatwierdzeń przechodząc do adresu URL repozytorium Git programu VSTS i znaleźć gałęzi. 

> [!NOTE] 
> Migawki tylko zostaje zatwierdzona przed wykonaniem skryptu. Obecnie przedprodukcyjnym danych wykonywania lub wykonywania komórki notesu nie spowoduje wyzwolenia migawki.

![Historia uruchomień gałęzi](media/using-git-ml-project/run_history_branch.png)

> [!IMPORTANT] 
> Najlepiej nie działa w gałęzi historii samodzielnie przy użyciu polecenia usługi Git. Dzięki temu może mess się Historia uruchomień. Użyj głównej gałęzi lub utworzyć inne gałęzie dla operacji Git.

## <a name="step-5-restore-a-previous-project-snapshot"></a>Krok 5. Przywróć poprzednią migawkę projektu 
Aby przywrócić folder całego projektu do stanu poprzedniego Historia uruchomień migawką stanu projektu, z Workbench uczenie Maszynowe Azure:
1. Polecenie **działa** w działaniu paska (ikona szkła godzinę).
2. Z **Uruchom listy** wyświetlić, kliknij pozycję Uruchom, aby przywrócić.
3. Z **Uruchom szczegółów** wyświetlić, wybierz polecenie **przywrócić**.

![Historia uruchomień gałęzi](media/using-git-ml-project/restore_project.png)

Alternatywnie można użyć następującego polecenia w oknie wiersza polecenia platformy Azure ML Workbench.

```azurecli
# discover the run I want to restore snapshot from:
$ az ml history list -o table

# restore the snapshot from a particular run
$ az ml project restore --run-id <run_id>
```

Przez wykonanie tego polecenia, możemy spowoduje zastąpienie folderu cały projekt migawką, gdy zostało rozpoczęte z określonym programem. Jednak pozostanie projektu na bieżącej gałęzi. Oznacza to, że będzie **utracić wszystkie zmiany** w bieżącym folderze projektu. Dlatego należy zachować szczególną ostrożność podczas uruchamiania tego polecenia. Aby zapisać zmiany przed wykonaniem operacji powyżej bieżącej gałęzi może chcesz Git. Aby uzyskać więcej informacji, zobacz przed.

## <a name="step-6-use-the-master-branch"></a>Krok 6. Użyj głównej gałęzi
Jednym ze sposobów uniknąć przypadkowego utraty aktualnym stanie projektu, jest można przekazać projektu do gałęzi głównej (lub działu utworzonego przez siebie) repozytorium Git. Git można korzystać bezpośrednio z wiersza polecenia (lub inne ulubionego Git klienta narzędzia wyboru) może działać w gałęzi głównej. Na przykład:

```sh
# check status to make sure you are on the master branch (or branch of your choice)
$ git status

# stage all changes
$ git add -A

# commit all changes locally on the master branch
$ git commit -m 'these are my updates so far'

# push changes into the remote VSTS Git repo master branch.
$ git push origin master
```

Teraz można bezpiecznie Przywracanie projektu do wcześniejszej migawki następującego kroku 5, wiedząc, że możesz zawsze wrócić do zatwierdzenia możesz po prostu dokonywane na głównym gałęzi.

## <a name="authentication"></a>Authentication
Jeśli właśnie polegać funkcji Historia uruchomień w uczenie Maszynowe Azure do wykonywania migawek projektu i ich przywracania, nie trzeba martwić się o uwierzytelnianiu repozytorium Git. Jest poświęcony na obsługę przez warstwę usługi eksperymenty.

Jednak użycie własnego narzędzia Git w celu zarządzania kontrolą wersji, należy poprawnie obsługiwać uwierzytelnianie względem zdalnego repozytorium Git na VSTS. Azure ml zdalnego repozytorium Git jest dodawana do lokalnego repozytorium jako Git zdalnego przy użyciu protokołu HTTPS. Oznacza to, w przypadku wysyłania Git polecenia do zdalnego (takich jak wypychania lub ściągania), należy podać nazwę użytkownika i hasło lub osobisty token dostępu. Postępuj zgodnie z [tych instrukcji](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) utworzyć osobisty token dostępu w repozytorium Git programu VSTS.

## <a name="next-steps"></a>Następne kroki
Używanie procesu nauki danych zespołu organizowanie struktury projektu, zobacz [struktury projektu z TDSP](how-to-use-tdsp-in-azure-ml.md)
