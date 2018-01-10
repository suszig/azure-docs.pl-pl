---
title: "Wdrażanie lokalnej usługi Git w usłudze Azure App Service"
description: "Dowiedz się, jak włączyć lokalne wdrożenie systemu Git w usłudze Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 19681564ef48649d4d44da1d0a2a65044f494ce4
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2018
---
# <a name="local-git-deployment-to-azure-app-service"></a>Wdrażanie lokalnej usługi Git w usłudze Azure App Service
W tym samouczku przedstawiono sposób wdrażania aplikacji [Azure Web Apps](app-service-web-overview.md) z repozytorium Git na komputerze lokalnym. Usługi aplikacji obsługuje tej metody z **Git lokalnego** opcji wdrażania w [Azure Portal].  
Wiele poleceń Git opisane w tym artykule są wykonywane automatycznie podczas tworzenia aplikacji usługi App Service przy użyciu [interfejsu wiersza polecenia platformy Azure] zgodnie z opisem [tutaj](app-service-web-get-started-dotnet.md).

## <a name="prerequisites"></a>Wymagania wstępne
Do ukończenia tego samouczka niezbędne są następujące elementy:

* Git. Możesz pobrać binarne instalacji [tutaj](http://www.git-scm.com/downloads).  
* Podstawową wiedzę na temat narzędzia Git.
* Konto platformy Microsoft Azure. Jeśli nie masz konta, możesz [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

> [!NOTE]
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do [Wypróbuj usługę App Service](https://azure.microsoft.com/try/app-service/), gdzie możesz od razu utworzyć krótkotrwałą wersję początkową aplikacji w usłudze App Service. Bez kart kredytowych i bez zobowiązań.  
> 
> 

## <a name="Step1"></a>Krok 1: Tworzenie lokalnego repozytorium
Wykonaj następujące zadania w celu utworzenia nowego repozytorium Git.

1. Uruchom narzędzie wiersza polecenia, takich jak **GitBash** (system Windows) lub **Bash** (powłoka systemu Unix). OS X w systemach dostęp można uzyskać wiersza polecenia, za pomocą **Terminal** aplikacji.
2. Przejdź do katalogu, gdzie znajduje się będzie zawartość do wdrożenia.
3. Aby zainicjować nowego repozytorium Git, użyj następującego polecenia:

```bash  
git init
```

## <a name="Step2"></a>Krok 2: Zatwierdź zawartości
Usługa aplikacji obsługuje aplikacje utworzone w różnych językach programowania. 

1. Jeśli repozytorium zawiera już Pomiń zawartości tego punktu, a następnie przejdź do punktu 2 poniżej. Jeśli repozytorium nie zawiera zawartości po prostu umieścić plik statyczny .html w następujący sposób: 
   
   * Za pomocą edytora tekstu, Utwórz nowy plik o nazwie **index.html** w katalogu głównym repozytorium Git
   * Dodaj poniższy tekst jako zawartość dla index.html plik i zapisać go: *Git Witaj!*
2. W wierszu polecenia Sprawdź, czy w katalogu głównym repozytorium Git. Następnie użyj następującego polecenia, aby dodać pliki do repozytorium:

```bash  
git add -A
```
3. Następnie Zatwierdź zmiany do repozytorium przy użyciu następującego polecenia:

```bash  
git commit -m "Hello Azure App Service"
```  

## <a name="Step3"></a>Krok 3: Włącz repozytorium aplikacji usługi aplikacji
Wykonaj poniższe kroki, aby włączyć repozytorium Git aplikację usługi aplikacji.

1. Zaloguj się do [Azure Portal].
2. W bloku aplikację usługi aplikacji, kliknij przycisk **Ustawienia > źródło wdrożenia**. Kliknij przycisk **wybierz źródło**, następnie kliknij przycisk **lokalnego repozytorium Git**, a następnie kliknij przycisk **OK**.  
   
    ![Lokalne repozytorium Git](./media/app-service-deploy-local-git/local_git_selection.png)
3. Jeśli jest to pierwszy ustawienia czasu zapasowej repozytorium na platformie Azure, musisz utworzyć poświadczenia logowania dla niego. Użyje ich do zalogowania się do platformy Azure zmiany repozytorium i wypychania z lokalnym repozytorium Git. W bloku aplikacji kliknij **wdrożenia > poświadczenia wdrożenia**, skonfiguruj wdrożenia, nazwę użytkownika i hasło. Gdy wszystko będzie gotowe, kliknij przycisk **zapisać**.
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Krok 4: Wdrażanie projektu
Wykonaj następujące kroki, aby publikowanie aplikacji w usłudze App Service przy użyciu usługi Git lokalnego.

1. W bloku aplikacji w portalu Azure, kliknij **Ustawienia > właściwości** dla **adres URL Git**.
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    **Adres URL Git** jest zdalnego odwołania do wdrożenia z lokalnym repozytorium. W poniższych krokach użyjesz tego adresu URL.
2. Przy użyciu wiersza polecenia, sprawdź, czy w katalogu lokalnym repozytorium Git.
3. Użyj `git remote` można dodać odwołania zdalnego na liście **adres URL Git** z kroku 1. Polecenie będzie wyglądać podobnie do poniższej:
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > **Zdalnego** polecenie dodaje nazwany odwołanie do repozytorium zdalnego. W tym przykładzie powoduje utworzenie odwołania do repozytorium aplikacji sieci web o nazwie "azure".
   > 
   > 
4. Wypychania zawartości do usługi App Service przy użyciu nowego **azure** zdalnego utworzony.

```bash  
git push azure master
```
    You will be prompted for the password you created earlier when you reset your deployment credentials in the Azure Portal. Enter the password (note that Gitbash does not echo asterisks to the console as you type your password). 
5. Wróć do aplikacji w portalu Azure. Wpis dziennika z najnowszych wypychania powinien być wyświetlany w **wdrożeń** bloku. 
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. Kliknij przycisk **Przeglądaj** na górze bloku aplikacji, aby sprawdzić zawartość została wdrożona. 

## <a name="Step5"></a>Rozwiązywanie problemów
Poniżej znajdują się błędy lub problemy najczęściej występujące podczas przy użyciu narzędzia Git, aby opublikować aplikację usługi aplikacji na platformie Azure:

- - -
**Objaw**: nie można dostępu [siteURL]: nie można nawiązać połączenia z [scmAddress]

**Przyczyna**: ten błąd może wystąpić, jeśli aplikacja nie jest uruchomiona.

**Rozdzielczość**: uruchomić aplikację w portalu Azure. Wdrożenie systemu Git nie będzie działać, chyba że aplikacja jest uruchomiona. 

- - -
**Objaw**: nie można rozpoznać nazwy hosta "hosta"

**Przyczyna**: ten błąd może wystąpić, jeśli adres wprowadzonej podczas tworzenia zdalnego "azure" jest niepoprawne.

**Rozdzielczość**: Użyj `git remote -v` polecenie, aby wyświetlić listę wszystkich element zdalny, wraz z powiązanego adresu URL. Sprawdź, czy adres URL "platformy azure" zdalnego jest poprawna. W razie potrzeby usuń i ponownie utwórz ten zdalnego przy użyciu poprawny adres URL.

- - -
**Objaw**: nie odwołania do wspólnego i brak określone; żadne czynności. Być może należy określić gałąź, takich jak 'master'.

**Przyczyna**: ten błąd może wystąpić, jeśli nie określisz gałąź podczas wykonywania git operacji wypychania, a nie ustawiono wartość push.default używana przez Git.

**Rozdzielczość**: wykonaj wypychania ponownie wykonać operację, określając gałęzi głównej. Na przykład:

```bash  
git push azure master
```
- - -
**Objaw**: nie pasuje do żadnego src refspec [nazwa_gałęzi].

**Przyczyna**: ten błąd może wystąpić przy próbie wypychania do gałęzi innej niż główny "Azure" zdalnego.

**Rozdzielczość**: wykonaj wypychania ponownie wykonać operację, określając gałęzi głównej. Na przykład:

```bash  
git push azure master
```
- - -
**Objaw**: RPC nie powiodło się; spowodować = 22, kod HTTP = 502.

**Przyczyna**: ten błąd może wystąpić przy próbie push repozytorium git dużych za pośrednictwem protokołu HTTPS.

**Rozdzielczość**: Zmień konfigurację git na komputer lokalny, aby powiększyć postBuffer

```bash  
git config --global http.postBuffer 524288000
```
- - -
**Objaw**: Błąd — zmiany do repozytorium zdalnego, ale aplikacja sieci web nie zostały zaktualizowane.

**Przyczyna**: ten błąd może wystąpić w przypadku wdrażania aplikacji Node.js zawierający plik package.json, który określa dodatkowe wymagane moduły.

**Rozdzielczość**: dodatkowych komunikatów zawierających 'npm błąd!' powinny być rejestrowane przed ten błąd i może zapewnić dodatkowy kontekst błędu. Znane są następujące przyczyny tego błędu i odpowiednie "npm błąd!' Komunikat:

* **Plik package.json źle sformułowane**: błąd npm! Nie można odczytać zależności.
* **Natywny moduł, który nie ma binarne dystrybucji dla systemu Windows**:
  
  * Błąd npm! \`cmd "/ c" "gyp węzła Odbuduj"\` nie powiodło się z 1
    
      LUB
  * Błąd npm! [modulename@version] preinstalować: \`upewnij || gmake\`

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Dokumentacja usługi Git](http://git-scm.com/documentation)
* [Dokumentacja Kudu projektu](https://github.com/projectkudu/kudu/wiki)
* [Ciągłe wdrażanie w usłudze Azure App Service](app-service-continuous-deployment.md)
* [Jak korzystać z programu PowerShell dla platformy Azure](/powershell/azure/overview)
* [Sposób użycia interfejsu wiersza polecenia platformy Azure](../cli-install-nodejs.md)

[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[interfejsu wiersza polecenia platformy Azure]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
