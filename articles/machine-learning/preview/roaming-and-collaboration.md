---
title: "Roaming i współpracy w Azure uczenia maszynowego Workbench | Dokumentacja firmy Microsoft"
description: "Listę znanych problemów i przewodnik dotyczący rozwiązywania problemów"
services: machine-learning
author: svankam
ms.author: svankam
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/05/2017
ms.openlocfilehash: 156dd1b7f928df22b3feb9e7a13396d3b53a91d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="roaming-and-collaboration-in-azure-machine-learning-workbench"></a>Roaming i współpracy w konsoli usługi Azure Machine Learning Workbench
Ten dokument przeprowadzi Cię przez jak Azure Machine Learning Workbench może pomóc są przekazywane projektów między maszyny, a także Włącz współpracy z członków zespołu. 

Po utworzeniu projektu usługi Azure Machine Learning łączem zdalnego repozytorium Git (repozytorium) projektu metadanych i migawki są przechowywane w chmurze. Łącze chmury umożliwia dostęp do projektu z innego komputera (mobilne). Możesz również zapewnić dostęp do współpracowników, umożliwiając w ten sposób współpracy. 

## <a name="prerequisites"></a>Wymagania wstępne
Najpierw zainstaluj Azure Machine Learning Workbench z dostępem do konta eksperymenty. Postępuj zgodnie z [Przewodnik instalacji](quickstart-installation.md) więcej szczegółów.

Po drugie, dostęp do [Visual Studio Team System](https://www.visualstudio.com) i repozytorium projekt do tworzenia. Aby uzyskać szczegółowe informacje na temat narzędzia Git, odwołanie [przy użyciu repozytorium Git z projektem Workbench Azure Machine Learning](using-git-ml-project.md) artykułu.

## <a name="create-a-new-azure-machine-learning-project"></a>Utwórz nowy projekt usługi Azure Machine Learning
Uruchom usługi Azure Machine Learning Workbench i Utwórz nowy projekt (na przykład _iris_). Wypełnij **adres URL repozytorium GIT Visualstudio.com** pole tekstowe z prawidłowym adresem URL repozytorium Git programu VSTS. 
>[!IMPORTANT]
>Tworzenie projektu nie powiedzie się, jeśli nie masz dostęp do odczytu/zapisu w repozytorium Git i repozytorium Git nie jest pusta, tj. zawiera ona już gałęzi głównej.

Po utworzeniu projektu, należy przesłać kilka działa na wszystkie skrypty w projekcie. Ta akcja zatwierdza stan projektu w gałęzi Historia uruchomień zdalnego repozytorium Git. 

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

>Należy pamiętać, nawiązały istnieje folder w katalogu uczenie Maszynowe Azure ma dokładnie samą nazwę jak projekt, pobierania kończy się niepowodzeniem. Trwa raz należy zmienić nazwę istniejącego folderu w celu obejścia tego problemu.


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

# Add Bob to the Experimentation Account as a Reader.
# Bob now has read access to all workspaces and projects under the Account by inheritance.
az role assignment create --assignee bob@contoso.com --role Reader --scope <experimentation account ARM ID>

# Find ARM ID of the workspace
az ml workspace show --query "id"

# Add Bob to the workspace as a Contributor.
# Bob now has read/write access to all projects under the Workspace by inheritance.
az role assignment create --assignee bob@contoso.com --role Contributor --scope <workspace ARM ID>

# find ARM ID of the project 
az ml project show --query "id"

# Add Bob to the Project as an Owner.
# Bob now has read/write access to the Project, and can add others too.
az role assignment create --assignee bob@contoso.com --role Owner --scope <project ARM ID>
```

Po przypisania roli bezpośrednio lub przez dziedziczenie, Roberta, który można zobaczyć projektu w liście projektu Workbench. Aplikacji może być konieczne ponowne uruchomienie komputera, aby zobaczyć projektu. Roberta, który może pobierać projekt zgodnie z opisem w [roamingu sekcji](#roaming) i współpracę z Alicji. 

Historia uruchomień dla wszystkich użytkowników współpracy nad projektem dba o tej samej zdalnego repozytorium Git. Dlatego po Alicja wykonuje Uruchom, Bob można wyświetlić Uruchom w sekcji Historia uruchomień projektu w aplikacji Workbench. Robert można także przywrócić projektu do stanu wszelkich Uruchom uruchomione przez Alicja działa w tym. 

Udostępnianie zdalnego repozytorium Git dla projektu umożliwia Alicja i Robert również współpracować nad gałęzi głównej. Jeśli to konieczne, mogą także tworzyć osobiste gałęzie i umożliwia współpracę żądań ściągnięcia Git i scalenia. 

### <a name="using-azure-portal-to-add-users"></a>Dodawanie użytkowników przy użyciu portalu Azure
<a name="portal"></a>

Azure Machine Learning eksperymenty kont, obszarami roboczymi i projekty są zasoby usługi Azure Resource Manager. Można użyć łącza kontroli dostępu w [portalu Azure](https://portal.azure.com) do przypisywania ról. 

Znajdź zasób, do którego chcesz dodać użytkowników do ze wszystkich zasobów wyświetlić. Polecenie kontroli dostępu (IAM) łącze na stronie. Dodawanie użytkowników 

<img src="./media/roaming-and-collaboration/iam.png" width="320px">

