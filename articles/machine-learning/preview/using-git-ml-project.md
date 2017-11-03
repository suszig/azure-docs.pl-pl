---
title: "Przy użyciu repozytorium Git w usłudze Azure Machine Learning Workbench projektu | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób korzystania z repozytorium Git w połączeniu z projektem Workbench Azure Machine Learning."
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: hning86
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 59b07c9834904e01256b75344ba2e6892e56438c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="using-git-repository-with-an-azure-machine-learning-workbench-project"></a>Przy użyciu repozytorium Git w usłudze Azure Machine Learning Workbench projektu
Ten dokument zawiera informacje na Azure Machine Learning Workbench używaniu Git do zapewnienia powtarzalności w eksperymencie analizy danych. Podawane są również instrukcje dotyczące sposobu kojarzenia projektu z chmurą repozytorium Git.

## <a name="introduction"></a>Wprowadzenie
Azure Machine Learning Workbench zaprojektowano z Integracja z usługą Git od podstaw w górę. Podczas tworzenia nowego projektu, folder projektu jest automatycznie "Git inicjowany" jako lokalnego repozytorium Git (repozytorium) podczas drugiego ukryte lokalnego repozytorium Git tworzona jest również o gałąź o nazwie _AzureMLHistory / < project_GUID >_ do Śledzenie pracy projektu folderu dla każdego wykonania. 

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


> [!TIP]
> Upewnij się, że logowanie przy użyciu konta usługi Azure Active Directory (AAD) używane do dostępu Azure Machine Learning Workbench. W przeciwnym razie nowego projektu zespołowego może zakończenia w pionie w obszarze nieprawidłowy identyfikator dzierżawy i uczenie maszynowe Azure nie może odnaleźć. W takim przypadku będą musiały korzystać z interfejsu wiersza polecenia i podaj token programu VSTS.

Po utworzeniu projektu zespołowego, można przystąpić do przeniesienia do następnego kroku.

Aby przejść bezpośrednio do projektu zespołowego właśnie utworzony, adres URL jest `https://<team_project_name>.visualstudio.com`.

> [!NOTE]
> Obecnie uczenia maszynowego Azure obsługuje tylko puste repozytoriów Git z nie gałęzi głównej. Przy użyciu interfejsu wiersza polecenia, można użyć argumentu--force, najpierw usunąć głównej gałęzi. 

## <a name="step-3-create-a-new-azure-ml-project-with-a-remote-git-repo"></a>Krok 3. Utwórz nowy projekt uczenie Maszynowe Azure ze zdalnego repozytorium Git
Uruchom usługi Azure ML Workbench i Utwórz nowy projekt. Adres URL repozytorium Git programu VSTS, który można pobrać z etapu 2 wypełnić pole tekstowe repozytorium Git. Zwykle wygląda następująco: http://<vsts_account_name>.visualstudio.com/_git/<project_name>

![Utwórz projekt uczenie Maszynowe Azure z repozytorium Git](media/using-git-ml-project/create_project_with_git_rep.png)

Teraz nowy projekt uczenie Maszynowe Azure jest tworzony z zdalnego integracji repozytorium Git, włączona i gotowa do go. Folder projektu jest zawsze Git zainicjowany jako lokalne repozytorium Git. I Git _zdalnego_ ustawiono zdalnego repozytorium Git programu VSTS, więc zatwierdzeń może zostać przeniesiony do zdalnego repozytorium Git.

## <a name="step-3a-associate-an-existing-azure-ml-project-with-a-vsts-git-repo"></a>Krok 3.a Powiąż istniejący projekt uczenie Maszynowe Azure z repozytorium Git programu VSTS
Opcjonalnie można również utworzyć projekt uczenie Maszynowe Azure bez repozytorium Git programu VSTS i polegać tylko na lokalne repozytorium Git dla migawki historii wykonywania. I repozytorium Git programu VSTS można skojarzyć później z tym istniejący projekt uczenie Maszynowe Azure za pomocą następującego polecenia:

```azurecli
# make sure you are in the project path so CLI has context of your current project
az ml project update --repo http://<vsts_account_name>.visualstudio.com/_git/<project_name
```

## <a name="step-4-capture-project-snapshot-in-git-repo"></a>Krok 4. Przechwytywanie migawek projektu w repozytorium Git
Teraz możesz wykonać kilka działa w projekcie, należy między nimi pewne zmiany działa. Można to zrobić, albo z aplikacji pulpitu lub przy użyciu interfejsu wiersza polecenia `az ml experiment submit` polecenia. Aby uzyskać więcej informacji, możesz wykonać [samouczek klasyfikowania Iris](tutorial-classifying-iris-part-1.md). Przy każdym uruchomieniu Jeśli zmiany wprowadzone w plikach w folderze projektu migawki folderu cały projekt jest zadeklarowane i przypisany do zdalnego repozytorium Git. Gałęzie i zatwierdzeń można wyświetlić, przechodząc do adresu URL repozytorium Git programu VSTS.

![Historia uruchomień gałęzi](media/using-git-ml-project/run_history_branch.png)

## <a name="step-5-restore-a-previous-project-snapshot"></a>Krok 5. Przywróć poprzednią migawkę projektu 
Aby przywrócić folder całego projektu do stanu poprzedniego Historia uruchomień migawką stanu projektu, z AML Workbench.
1. Polecenie **działa** w działaniu paska (ikona szkła godzinę).
2. Z **Uruchom listy** wyświetlić, kliknij pozycję Uruchom, aby przywrócić.
3. Z **Uruchom szczegółów** wyświetlić, wybierz polecenie **przywrócić**.

![Historia uruchomień gałęzi](media/using-git-ml-project/restore_project.png)

Alternatywnie można użyć następującego polecenia w oknie wiersza polecenia platformy Azure ML Workbench.

```azurecli
# discover the run I want to restore snapshot from:
az ml history list -o table

# restore the snapshot from a particular run
az ml project restore --run-id <run_id>
```

Przez wykonanie tego polecenia, możemy spowoduje zastąpienie folderu cały projekt migawką, gdy zostało rozpoczęte z określonym programem. Oznacza to, że będzie **utracić wszystkie zmiany** w bieżącym folderze projektu. Dlatego należy zachować szczególną ostrożność podczas uruchamiania tego polecenia.

## <a name="step-6-use-the-master-branch"></a>Krok 6. Użyj głównej gałęzi
Jest jednym ze sposobów uniknąć przypadkowej utraty aktualnego stanu projektu, można przekazać projektu do głównej gałęzi repozytorium Git. Git można korzystać bezpośrednio z wiersza polecenia (lub inne ulubionego Git klienta narzędzia wyboru) może działać w gałęzi głównej. Na przykład:

```
# make sure you are on the master branch
git checkout master

# stage all changes
git add -A

# commit all changes locally on the master branch
git commit -m 'this is my updates so far'

# push changes into the remote VSTS Git repo master branch.
git push origin master
```

Teraz można bezpiecznie Przywracanie projektu do wcześniejszej migawki następującego kroku 5, wiedząc, że możesz zawsze wrócić do zatwierdzenia możesz po prostu dokonywane na głównym gałęzi.

## <a name="authentication"></a>Authentication
Jeśli właśnie polegać funkcji Historia uruchomień w uczenie Maszynowe Azure do wykonywania migawek projektu i ich przywracania, nie trzeba martwić się o uwierzytelnianiu repozytorium Git. Jest poświęcony na obsługę przez warstwę usługi eksperymenty.

Jednak użycie własnego narzędzia Git w celu zarządzania kontrolą wersji, należy poprawnie obsługiwać uwierzytelnianie względem zdalnego repozytorium Git na VSTS. Oznacza to należy skonfigurować uwierzytelnianie z repozytorium Git na komputerze lokalnym, zanim można wydać polecenia usługi Git przed tym zdalnego repozytorium Git. 

Najprostszym sposobem, w tym celu jest utworzyć parę kluczy SSH i przekazać publicznej części klucza do ustawienia zabezpieczeń repozytorium Git.

### <a name="generate-ssh-key"></a>Generowanie klucza SSH 
Pierwszy umożliwia generowanie pary kluczy SSH na tym komputerze.

#### <a name="on-windows"></a>W systemie Windows:
Uruchamianie aplikacji komputerowej Git graficznego interfejsu użytkownika w systemie Windows, a w obszarze _pomocy_ menu, kliknij polecenie _Pokaż klucza SSH_.

![SSH klucza](media/using-git-ml-project/git_gui.png)

SSH, skopiuj do Schowka.

#### <a name="on-macos"></a>Na macOS:
Szybkie kroki z powłoki poleceń:
```
# generate the SSH key
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# start the SSH agent in the background
$ eval "$(ssh-agent -s)"

# add newly generated SSH key to the SSH agent
$ ssh-add -K ~/.ssh/id_rsa

# display the public key so you can copy it.
$ more ~/.ssh/id_rsa.pub
```
Więcej szczegółów kroki można znaleźć w [ten artykuł GitHub](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).

### <a name="upload-public-key-to-git-repo"></a>Przekazać klucza publicznego do repozytorium Git
Przejdź do strony głównej konta programu Visual Studio: https://<vsts_account_name>.visualstudio.com i zaloguj się, następnie kliknij przycisk zabezpieczeń w obszarze awatara.

Następnie dodaj klucz publiczny SSH, Wklej klucz publiczny SSH, uzyskasz z poprzedniego kroku i nadaj mu nazwę. Można dodać wiele kluczy w tym miejscu.

Teraz można wydać polecenia usługi Git lokalnie przed repozytorium Usuń wymagane żadne dodatkowe jawne uwierzytelniania.

### <a name="read-more"></a>Dowiedz się więcej
Wykonaj tych dwóch artykułach (albo podejście może działać) Aby uzyskać więcej informacji o sposobie włączania uwierzytelniania lokalnych do zdalnego repozytorium Git w VSTS.
- [Użyj uwierzytelniania klucza SSH](https://www.visualstudio.com/en-us/docs/git/use-ssh-keys-to-authenticate)
- [Użyj Menedżera poświadczeń Git](https://www.visualstudio.com/en-us/docs/git/set-up-credential-managers)


## <a name="next-steps"></a>Następne kroki
Używanie procesu nauki danych zespołu organizowanie struktury projektu, zobacz [struktury projektu z TDSP](how-to-use-tdsp-in-azure-ml.md)
