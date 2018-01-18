---
title: "Wdrażanie lokalnej usługi Git w usłudze Azure App Service"
description: "Dowiedz się, jak włączyć lokalne wdrożenie systemu Git w usłudze Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: cfowler
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 7667f94ac4e7edd4e94d6605adefea469102a0c4
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="local-git-deployment-to-azure-app-service"></a>Wdrażanie lokalnej usługi Git w usłudze Azure App Service

W tym samouczku przedstawiono sposób wdrażania aplikacji [Azure Web Apps](app-service-web-overview.md) z repozytorium Git na komputerze lokalnym. Usługi aplikacji obsługuje takie podejście z **lokalnego Git** opcji wdrażania w [portalu Azure].

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka niezbędne są następujące elementy:

* Git. Możesz pobrać binarne instalacji [tutaj](http://www.git-scm.com/downloads).
* Podstawową wiedzę na temat narzędzia Git.
* Konto platformy Microsoft Azure. Jeśli nie masz konta, możesz [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

> [!NOTE]
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do [Wypróbuj usługę App Service](https://azure.microsoft.com/try/app-service/), gdzie możesz od razu utworzyć krótkotrwałą wersję początkową aplikacji w usłudze App Service. Bez kart kredytowych i bez zobowiązań.
>

## <a name="Step1"></a>Krok 1: Tworzenie lokalnego repozytorium

Wykonaj następujące zadania w celu utworzenia nowego repozytorium Git.

1. Uruchom narzędzie wiersza polecenia, takich jak **Git Bash** (system Windows) lub **Bash** (powłoka systemu Unix). W systemach OS X, można uzyskać dostępu do wiersza polecenia za pomocą **Terminal** aplikacji.
1. Przejdź do katalogu, gdzie znajduje się będzie zawartość do wdrożenia.
1. Aby zainicjować nowego repozytorium Git, użyj następującego polecenia:

  ```bash
  git init
  ```

## <a name="Step2"></a>Krok 2: Zatwierdź zawartości

Usługa aplikacji obsługuje aplikacje utworzone w różnych językach programowania.

1. Jeśli repozytorium nie obejmuje zawartości, Dodaj plik statyczny .html lub pominąć ten krok:
   * Za pomocą edytora tekstu, Utwórz nowy plik o nazwie **index.html** w katalogu głównym repozytorium Git
   * Dodaj poniższy tekst jako zawartość dla index.html plik i zapisać go: *Git Witaj!*
1. Z poziomu wiersza polecenia Sprawdź, czy w katalogu głównym repozytorium Git. Następnie użyj następującego polecenia, aby dodać pliki do repozytorium:

    ```bash
    git add -A
    ```
    
1. Następnie Zatwierdź zmiany do repozytorium przy użyciu następującego polecenia:

    ```bash
    git commit -m "Hello Azure App Service"
    ```

## <a name="Step3"></a>Krok 3: Włącz repozytorium aplikacji usługi aplikacji

Wykonaj poniższe kroki, aby włączyć repozytorium Git aplikację usługi aplikacji.

1. Zaloguj się do witryny [portalu Azure].
1. W widoku aplikację usługi aplikacji, kliknij przycisk **Ustawienia > źródło wdrożenia**. Kliknij przycisk **wybierz źródło**, następnie kliknij przycisk **lokalnego repozytorium Git**, a następnie kliknij przycisk **OK**.

    ![Lokalne repozytorium Git](./media/app-service-deploy-local-git/local_git_selection.png)

1. Jeśli jest to pierwszy ustawienia czasu zapasowej repozytorium na platformie Azure, musisz utworzyć poświadczenia logowania dla niego. Użyj ich do zalogowania się do platformy Azure zmiany repozytorium i wypychania z lokalnym repozytorium Git. Z widoku aplikacji sieci Web, kliknij przycisk **Ustawienia > poświadczenia wdrożenia**, następnie należy skonfigurować wdrożenie nazwę użytkownika i hasło. Gdy wszystko będzie gotowe, kliknij przycisk **zapisać**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Krok 4: Wdrażanie projektu

Wykonaj następujące kroki, aby publikowanie aplikacji w usłudze App Service przy użyciu usługi Git lokalnego.

1. W widoku aplikacji sieci Web w portalu Azure kliknij **Ustawienia > właściwości** dla **adres URL Git**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **Adres URL Git** jest zdalnego odwołania do wdrożenia z lokalnym repozytorium. Za pomocą tego adresu URL w następnych krokach.
1. Korzystając z wiersza polecenia, sprawdź, czy w katalogu lokalnym repozytorium Git.
1. Użyj `git remote` można dodać odwołania zdalnego na liście **adres URL Git** z kroku 1. Polecenie wygląda podobnie do następującego:

    ```
    git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git
    ```

   > [!NOTE]
   > **Zdalnego** polecenie dodaje nazwany odwołanie do repozytorium zdalnego. W tym przykładzie powoduje utworzenie odwołania do repozytorium aplikacji sieci web o nazwie "azure".
   >

1. Wypychania zawartości do usługi App Service przy użyciu nowego **azure** utworzone zdalnego.

    ```bash
    git push azure master
    ```

    Zostanie wyświetlony monit o podanie hasła utworzonego wcześniej po zresetowaniu poświadczenia wdrażania w portalu Azure. Wprowadź hasło (należy pamiętać, że Gitbash nie są wyświetlane gwiazdki w konsoli podczas wpisywania hasła). 
1. Wróć do aplikacji w portalu Azure. Wpis dziennika z najnowszych wypychania powinien być wyświetlany w **wdrożeń** widoku.

    ![](./media/app-service-deploy-local-git/deployment_history.png)

1. Kliknij przycisk **Przeglądaj** na górze strony aplikacji sieci web, aby sprawdzić zawartość została wdrożona.

## <a name="Step5"></a>Rozwiązywanie problemów

Poniżej znajdują się błędy lub problemy najczęściej występujące podczas przy użyciu narzędzia Git, aby opublikować aplikację usługi aplikacji na platformie Azure:

---
**Objaw**:`Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Przyczyna**: ten błąd może wystąpić, jeśli aplikacja nie jest uruchomiona.

**Rozdzielczość**: uruchomić aplikację w portalu Azure. Wdrażanie Git jest niedostępna, jeśli aplikacja sieci Web została zatrzymana.

---
**Objaw**:`Couldn't resolve host 'hostname'`

**Przyczyna**: ten błąd może wystąpić, jeśli adres wprowadzonej podczas tworzenia zdalnego "azure" jest niepoprawne.

**Rozdzielczość**: Użyj `git remote -v` polecenie, aby wyświetlić listę wszystkich element zdalny, wraz z powiązanego adresu URL. Sprawdź, czy adres URL "platformy azure" zdalnego jest poprawna. W razie potrzeby usuń i ponownie utwórz ten zdalnego przy użyciu poprawny adres URL.

---
**Objaw**:`No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Przyczyna**: ten błąd może wystąpić, jeśli nie określisz gałąź podczas `git push`, lub jeśli nie ustawiono `push.default` wartość w `.gitconfig`.

**Rozdzielczość**: wykonaj wypychania ponownie wykonać operację, określając gałęzi głównej. Na przykład:

```bash
git push azure master
```

---
**Objaw**:`src refspec [branchname] does not match any.`

**Przyczyna**: ten błąd może wystąpić przy próbie wypychania do gałęzi innej niż główny "Azure" zdalnego.

**Rozdzielczość**: wykonaj wypychania ponownie wykonać operację, określając gałęzi głównej. Na przykład:

```bash
git push azure master
```

---
**Objaw**:`RPC failed; result=22, HTTP code = 5xx.`

**Przyczyna**: ten błąd może wystąpić przy próbie push repozytorium git dużych za pośrednictwem protokołu HTTPS.

**Rozdzielczość**: Zmień konfigurację git na komputer lokalny, aby powiększyć postBuffer

```bash
git config --global http.postBuffer 524288000
```

---
**Objaw**:`Error - Changes committed to remote repository but your web app not updated.`

**Przyczyna**: ten błąd może wystąpić w przypadku wdrażania aplikacji Node.js zawierający plik package.json, który określa dodatkowe wymagane moduły.

**Rozdzielczość**: dodatkowych komunikatów zawierających 'npm błąd!' powinny być rejestrowane przed ten błąd i może zapewnić dodatkowy kontekst błędu. Znane są następujące przyczyny tego błędu i odpowiednie "npm błąd!' Komunikat:

* **Plik package.json źle sformułowane**: błąd npm! Nie można odczytać zależności.
* **Natywny moduł, który nie ma binarne dystrybucji dla systemu Windows**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      LUB
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dokumentacja usługi Git](http://git-scm.com/documentation)
* [Dokumentacja Kudu projektu](https://github.com/projectkudu/kudu/wiki)
* [Ciągłe wdrażanie w usłudze Azure App Service](app-service-continuous-deployment.md)
* [Jak korzystać z programu PowerShell dla platformy Azure](/powershell/azure/overview)
* [Sposób użycia interfejsu wiersza polecenia platformy Azure](../cli-install-nodejs.md)

[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[portalu Azure]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure Command-Line Interface]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
