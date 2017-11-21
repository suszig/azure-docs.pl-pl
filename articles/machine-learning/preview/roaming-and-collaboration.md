---
title: "Roaming i współpracy w Azure uczenia maszynowego Workbench | Dokumentacja firmy Microsoft"
description: "Listę znanych problemów i przewodnik dotyczący rozwiązywania problemów"
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/16/2017
ms.openlocfilehash: 50f48fb096cb907e050769a8a4159689eb25418c
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming i współpracy w konsoli usługi Azure Machine Learning Workbench
Ten dokument przeprowadzi Cię przez jak Azure Machine Learning Workbench może pomóc są przekazywane projektów między maszyny, a także Włącz współpracy z członków zespołu. 

Po utworzeniu projektu usługi Azure Machine Learning łączem zdalnego repozytorium Git (repozytorium) projektu metadanych i migawki są przechowywane w chmurze. Łącze chmury umożliwia dostęp do projektu z innego komputera (mobilne). Możesz również zapewnić dostęp do współpracowników, umożliwiając w ten sposób współpracy. 

## <a name="prerequisites"></a>Wymagania wstępne
Najpierw zainstaluj Azure Machine Learning Workbench z dostępem do konta eksperymenty. Postępuj zgodnie z [Przewodnik instalacji](quickstart-installation.md) więcej szczegółów.

Po drugie, dostęp do [Visual Studio Team System](https://www.visualstudio.com) i repozytorium projekt do tworzenia. Aby uzyskać szczegółowe informacje na temat narzędzia Git, odwołanie [przy użyciu repozytorium Git z projektem Workbench Azure Machine Learning](using-git-ml-project.md) artykułu.

## <a name="create-a-new-azure-machine-learning-project"></a>Utwórz nowy projekt usługi Azure Machine Learning
Uruchom usługi Azure Machine Learning Workbench i Utwórz nowy projekt (na przykład _iris_). Wypełnij **adres URL repozytorium GIT Visualstudio.com** pole tekstowe z prawidłowym adresem URL repozytorium Git programu VSTS. 

> [!IMPORTANT]
> Jeśli wybierzesz szablon projektu puste, jeśli repozytorium Git, możesz wybrać już jest OK _wzorca_ gałęzi. Po prostu klonów Azure ML _wzorca_ gałęzi lokalnie, a następnie dodaj `aml_config` folderu i inne pliki metadanych do folderu lokalnego projektu projektu. Ale jeśli wybierzesz innego szablonu projektu z repozytorium Git nie mogą już mieć _wzorca_ gałęzi, lub zostanie wyświetlony komunikat o błędzie. Alternatywą jest użycie `az ml project create` narzędzia wiersza polecenia, aby utworzyć projekt i podać `--force` przełącznika. Usuwa pliki na oryginalnym głównej gałęzi i Zamień nowe pliki w wybranego szablonu.

Po utworzeniu projektu, należy przesłać kilka działa na wszystkie skrypty w projekcie. Ta akcja zatwierdza stan projektu w gałęzi Historia uruchomień zdalnego repozytorium Git. 

> [!NOTE] 
> Tylko skrypt jest wykonywany wyzwalacz zatwierdzeń do gałęzi Historia uruchomień. Dane Przygotowywanie wykonywania uruchomień notesu nie wyzwalacza migawki projektu w gałęzi Historia uruchomień lub.

Jeśli ustawienia uwierzytelniania Git, możesz również jawnie działają w gałęzi głównej lub utworzyć nową gałąź. 

Na przykład: 
```
# check current repo status
$ git status

# stage all changes in the current repo
$ git add -A

# commit changes
$ git commit -m "my commit fixes this weird bug!"

# push to remote repo.
$ git push origin master
```

## <a name="roaming"></a>Mobilny
<a name="roaming"></a>

### <a name="open-azure-machine-learning-workbench-on-second-machine"></a>Otwórz Azure Machine Learning Workbench na drugim komputerze.
Gdy repozytorium Git programu VSTS jest połączony z projektem, będzie dostępny _iris_ projektu za pomocą dowolnego komputera, na którym jest zainstalowany Azure Machine Learning Workbench. 

Aby uzyskać dostęp do projektu iris na innym komputerze, należy zalogować się do aplikacji za pomocą tych samych poświadczeń, używane podczas tworzenia projektu. Ponadto należy przejść do tego samego konta eksperymenty i obszaru roboczego. _Iris_ projektu alfabetycznie znajduje się z innymi projektami w obszarze roboczym. 

### <a name="download-project-on-second-machine"></a>Pobieranie projektu na drugim komputerze.
Po otwarciu obszaru roboczego na sekundę maszyny, obok ikony _iris_ projektu różni się od ikona typowy folder. Ikona pobierania wskazuje, że zawartość projektu znajduje się w chmurze i muszą zostać pobrane na bieżącym komputerze. 

![Tworzenie projektu](./media/roaming-and-collaboration/downloadable-project.png)

Kliknięcie _iris_ rozpoczęcia projektu akcję pobierania. Automatycznie podczas po zakończeniu pobierania pliku projektu jest gotowy do uzyskiwać na drugim komputerze. 

W systemie Windows jest`C:\Users\<username>\Documents\AzureML`

Na macOS jest w tym miejscu:`/home/<username>/Documents/AzureML`

W przyszłym wydaniu planujemy rozszerzają funkcje umożliwiające wybranie folderu docelowego. 

> [!NOTE]
> W przypadku folderu w katalogu uczenie Maszynowe Azure ma dokładnie samą nazwę jak projekt, nie powiedzie się pobieranie. Trwa raz należy zmienić nazwę istniejącego folderu w celu obejścia tego problemu.


### <a name="work-on-the-downloaded-project"></a>Pobrany projektu 
Nowo pobranego projektu odzwierciedla stan projektu, począwszy od ostatniego uruchomienia w projekcie. Migawka stanu projektu jest automatycznie zatwierdzone do wykonywania historii gałęzi w repozytorium Git programu VSTS za każdym razem, gdy przesłać Uruchom. Używamy migawki skojarzone z najnowszą uruchomiony podczas tworzenia wystąpienia projektu na drugim komputerze. 
 

## <a name="collaboration"></a>Współpraca
Umożliwia współpracę z członków zespołu nad projektami połączone z repozytorium Git programu VSTS. Można przypisać uprawnienia dla użytkowników na konto eksperymenty, obszar roboczy i projektu. W tej chwili można wykonać polecenia usługi Azure Resource Manager przy użyciu wiersza polecenia platformy Azure. Można również użyć [portalu Azure](https://portal.azure.com). Zobacz [po sekcji](#portal).    

### <a name="using-command-line-to-add-users"></a>Aby dodać użytkowników przy użyciu wiersza polecenia
Umożliwia korzystanie z przykładu. Powiedz Alicja jest właścicielem th e_Iris_ projektu i chce korzystać z niego. 

Alicja kliknie **pliku** menu i wybiera **wiersza polecenia** element menu, aby uruchomić wiersz polecenia skonfigurowany tak, aby _iris_ projektu. Alicja jest następnie zdecydować, jakiego poziomu dostępu fo chce udzielić Roberta, wykonując następujące polecenia.  

```azurecli
# Find ARM ID of the experimnetation account
az ml account experimentation show --query "id"

# Add Bob to the Experimentation Account as a Contributor.
# Bob now has read/write access to all workspaces and projects under the Account by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <experimentation account ARM ID>

# Find ARM ID of the workspace
az ml workspace show --query "id"

# Add Bob to the workspace as an Owner.
# Bob now has read/write access to all projects under the Workspace by inheritance. And he can invite or remove others.
az role assignment create --assignee bob@contoso.com --role Owner --scope <workspace ARM ID>
```

Po przypisania roli bezpośrednio lub przez dziedziczenie, Roberta, który można zobaczyć projektu w liście projektu Workbench. Aplikacji może być konieczne ponowne uruchomienie komputera, aby zobaczyć projektu. Roberta, który może pobierać projekt zgodnie z opisem w [roamingu sekcji](#roaming) i współpracę z Alicji. 

Historia uruchomień dla wszystkich użytkowników współpracy nad projektem dba o tej samej zdalnego repozytorium Git. Dlatego po Alicja wykonuje Uruchom, Bob można wyświetlić Uruchom w sekcji Historia uruchomień projektu w aplikacji Workbench. Robert można także przywrócić projektu do stanu wszelkich Uruchom uruchomione przez Alicja działa w tym. 

Udostępnianie zdalnego repozytorium Git dla projektu umożliwia Alicja i Robert również współpracować nad gałęzi głównej. Jeśli to konieczne, mogą także tworzyć osobiste gałęzie i umożliwia współpracę żądań ściągnięcia Git i scalenia. 

### <a name="using-azure-portal-to-add-users"></a>Dodawanie użytkowników przy użyciu portalu Azure
<a name="portal"></a>

Azure Machine Learning eksperymenty kont, obszarami roboczymi i projekty są zasoby usługi Azure Resource Manager. Można użyć łącza kontroli dostępu w [portalu Azure](https://portal.azure.com) do przypisywania ról. 

Znajdź zasób, do którego chcesz dodać użytkowników do ze wszystkich zasobów wyświetlić. Polecenie kontroli dostępu (IAM) łącze na stronie. Dodawanie użytkowników 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

## <a name="sample-collaboration-workflow"></a>Przykładowy przepływ pracy współpracy
Aby zilustrować przepływu współpracy, Przejdźmy przykładem. Pracownicy firmy Contoso Alicja i Robert chcesz współpracować nad projektem nauki danych przy użyciu usługi Azure ML Workbench. Tożsamości należą do tej samej dzierżawie Contoso usługi Azure AD.

1. Alicja najpierw tworzy pustego repozytorium Git w projekcie usługi VSTS. Ten projekt programu VSTS powinny znajdować się w subskrypcji platformy Azure utworzona w ramach dzierżawy firmy Contoso w usłudze AAD. 

2. Alicja następnie tworzy konta eksperymenty uczenie Maszynowe Azure, obszar roboczy i projektu usługi Azure ML Workbench na swoim komputerze. Udostępnia ona adres URL repozytorium Git, podczas tworzenia projektu.

3. Alicja zostanie uruchomiony do projektu. Użytkownik tworzy niektóre skrypty i wykonuje kilka działa. Przy każdym uruchomieniu migawki folderu cały projekt automatycznie zostanie przypisany do gałęzi Historia uruchomień repozytorium Git programu VSTS utworzone przez Workbench jako zatwierdzenia.

4. Alicja jest teraz wszystkiego o pracy w toku. Chce przekazać swoje zmiany w lokalnej _wzorca_ gałęzi i wypchnięcia jej do repozytorium Git programu VSTS _wzorca_ gałęzi. Aby to zrobić, otwórz projekt użytkownik uruchamia okno wiersza polecenia z usługi Azure ML Workbench i problemy z następujących poleceń:
    
    ```sh
    # verify the Git remote is pointing to the VSTS Git repo
    $ git remote -v

    # verify that the current branch is master
    $ git branch

    # stage all changes
    $ git add -A

    # commit changes with a comment
    $ git commit -m "this is a good milestone"

    # push the commit to the master branch of the remote Git repo in VSTS
    $ git push
    ```

5. Alicja dodaje Roberta do obszaru roboczego jako współautor. Użytkownik można to zrobić w portalu Azure lub za pomocą `az role assignment` polecenia ilustrują powyżej. Przyznaje ona również Bob odczytu/zapisu dostępu do repozytorium Git programu VSTS.

6. Robert teraz loguje Workbench uczenia Maszynowego Azure, na jego komputerze. Widzi Alicja obszaru roboczego udostępnionych z nim i projektu wymienionych w tym obszarze roboczym. 

7. Robert kliknie nazwę projektu i projekt zostanie pobrana do komputera.
    
    a. Pliki z pobranego projektu to klony migawki najnowszej działają zarejestrowane w historii uruchamiania. Nie są one ostatniego zatwierdzenia dla gałęzi głównej.
    
    b. Folder lokalny projektu jest ustawiona na _wzorca_ gałęzi nieprzygotowanych zmian.

8. Bob może przeglądać uruchamia wykonywane przez Alicja i przywracanie migawki wszelkie poprzednie przebiegów.

9. Robert chce, aby pobrać najnowsze zmiany wypychana przez Alicja i rozpocząć pracę nad innej gałęzi. Dlatego on zostanie otwarte okno wiersza polecenia z usługi Azure ML Workbench i wykonuje następujące polecenia:

    ```sh
    # verify the Git remote is pointing to the VSTS Git repo
    $ git remote -v

    # verify that the current branch is master
    $ git branch

    # get the latest commit in VSTS Git master branch and overwrite current files
    $ git pull --force

    # create a new local branch named "bob" so Bob's work is done on the "bob" branch
    $ git checkout -b bob
    ```

10. Robert teraz modyfikuje projektu i przesłać nowe przebiegi. Zmiany są wykonywane na _bob_ gałęzi. I uruchamia Roberta stają się widoczne dla Alicji również.

11. Robert jest teraz gotowy do wypchnąć jego zmiany do zdalnego repozytorium Git. Aby uniknąć konfliktu z _wzorca_ gałęzi, w której działa Alicja, postanowił wypchnąć pracy do nowej gałęzi zdalnego o nazwie _bob_.

    ```sh
    # verify that the current branch is "bob" and it has unstaged changes
    $ git status
    
    # stage all changes
    $ git add -A

    # commit them with a comment
    $ git commit -m "I found a cool new trick."

    # create a new branch on the remote VSTS Git repo, and push changes
    $ git push origin bob
    ```

12. Robert można następnie opisz Alicji nowe lewy chłodnych w jego kod i tworzy żądanie ściągnięcia na zdalnego repozytorium Git ze _bob_ gałęzi do _wzorca_ gałęzi. I Alicja może scalić żądania ściągnięcia do _wzorca_ gałęzi.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat korzystania z usługi Azure ML Workbench Git: [repozytorium Git przy użyciu usługi Azure Machine Learning Workbench projektu](using-git-ml-project.md)