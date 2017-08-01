---
title: "Tworzenie aplikacji internetowej w języku Python na platformie Azure | Microsoft Docs"
description: "Wdróż swoją pierwszą aplikację Hello World w języku Python w usłudze Azure App Service Web Apps w ciągu kilku minut."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 928ee2e5-6143-4c0c-8546-366f5a3d80ce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 8021f8641ff3f009104082093143ec8eb087279e
ms.openlocfilehash: 754c381cb242e0bdf7c56bd2a763d46acc80fbda
ms.contentlocale: pl-pl
ms.lasthandoff: 07/21/2017

---
# <a name="create-a-python-web-app-in-azure"></a>Tworzenie aplikacji sieci Web w języku Python na platformie Azure

Usługa [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) oferuje wysoce skalowalną i samonaprawialną usługę hostowaną w Internecie.  Ten podręcznik Szybki start przeprowadzi Cię przez tworzenie i wdrażanie aplikacji w języku Python w usłudze Azure Web Apps. Możesz utworzyć aplikację internetową przy użyciu [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), a usługa Git umożliwia wdrażanie przykładowego kodu języka Python w aplikacji internetowej.

![Przykładowa aplikacja działająca na platformie Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

Poniższe kroki możesz wykonać przy użyciu komputera z systemem Mac, Windows lub Linux. Po zainstalowaniu wymagań wstępnych wykonanie czynności trwa około pięciu minut.
## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

1. [Zainstaluj oprogramowanie Git](https://git-scm.com/)
1. [Zainstaluj język Python](https://www.python.org/downloads/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten temat będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="download-the-sample"></a>Pobierz przykład

W oknie terminala uruchom następujące polecenie, aby sklonować przykładowe repozytorium aplikacji na maszynę lokalną.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

To okno terminala umożliwia uruchamianie wszystkich poleceń z tego przewodnika Szybki start.

Przejdź do katalogu, który zawiera przykładowy kod.

```bash
cd Python-docs-hello-world
```

## <a name="run-the-app-locally"></a>Lokalne uruchamianie aplikacji

Zainstaluj wymagane pakiety za pomocą polecenia `pip`.

```bash
pip install -r requirements.txt
```

Uruchom aplikację lokalnie, otwierając okno terminala i korzystając z polecenia języka `Python` w celu uruchomienia wbudowanego serwera internetowego języka Python.

```bash
python main.py
```

Otwórz przeglądarkę internetową i przejdź do przykładowej aplikacji pod adresem http://localhost:5000.

Na stronie zostanie wyświetlony komunikat **Witaj, świecie** z przykładowej aplikacji.

![Przykładowa aplikacja działająca w środowisku lokalnym](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

W oknie terminalu naciśnij kombinację klawiszy **Ctrl + C**, aby zamknąć serwer sieci Web.

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Pusta strona aplikacji internetowej](media/app-service-web-get-started-python/app-service-web-service-created.png)

Na platformie Azure została utworzona nowa pusta aplikacja internetowa.

## <a name="configure-to-use-python"></a>Konfigurowanie do używania środowiska Python

Użyj polecenia [az webapp config set](/cli/azure/webapp/config#set) do skonfigurowania aplikacji internetowej korzystającej z języka Python w wersji `3.4`.

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


Podczas ustawiania wersji języka Python w ten sposób jest używany domyślny kontener udostępniany przez platformę. Aby korzystać z własnego kontenera, zapoznaj się z opisem polecenia [az webapp config container set](/cli/azure/webapp/config/container#set) w dokumentacji dotyczącej interfejsu wiersza polecenia.

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 18, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (18/18), 4.31 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '44e74fe7dd'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling python deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'main.py'
remote: Copying file: 'README.md'
remote: Copying file: 'requirements.txt'
remote: Copying file: 'virtualenv_proxy.py'
remote: Copying file: 'web.2.7.config'
remote: Copying file: 'web.3.4.config'
remote: Detected requirements.txt.  You can skip Python specific steps with a .skipPythonDeployment file.
remote: Detecting Python runtime from site configuration
remote: Detected python-3.4
remote: Creating python-3.4 virtual environment.
remote: .................................
remote: Pip install requirements.
remote: Successfully installed Flask click itsdangerous Jinja2 Werkzeug MarkupSafe
remote: Cleaning up...
remote: .
remote: Overwriting web.config with web.3.4.config
remote:         1 file(s) copied.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a>Przechodzenie do aplikacji

Przejdź do wdrożonej aplikacji za pomocą przeglądarki sieci Web.

```bash
http://<app_name>.azurewebsites.net
```

Przykładowy kod w języku Python jest uruchamiany w aplikacji internetowej usługi Azure App Service.

![Przykładowa aplikacja działająca na platformie Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

**Gratulacje!** Udało Ci się wdrożyć pierwszą własną aplikację w języku Python w usłudze App Service.

## <a name="update-and-redeploy-the-code"></a>Aktualizowanie i ponowne wdrażanie kodu

Za pomocą lokalnego edytora tekstów otwórz plik `main.py` w aplikacji w języku Python i wprowadź niewielką zmianę w tekście obok instrukcji `return`:

```python
return 'Hello, Azure!'
```

Zatwierdź zmiany w narzędziu Git, a następnie wypchnij zmiany kodu na platformę Azure.

```bash
git commit -am "updated output"
git push azure master
```

Po zakończeniu wdrożenia przejdź z powrotem do okna przeglądarki otwartego w kroku [przechodzenia do aplikacji](#browse-to-the-app), a następnie odśwież stronę.

![Zaktualizowana przykładowa aplikacja działająca na platformie Azure](media/app-service-web-get-started-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Zarządzanie nową aplikacją sieci Web platformy Azure

Przejdź do witryny <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, aby zarządzać utworzoną aplikacją internetową.

W menu po lewej stronie kliknij pozycję **App Services**, a następnie kliknij nazwę swojej aplikacji internetowej platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Zostanie wyświetlona strona Omówienie aplikacji internetowej. Tutaj możesz wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie. 

![Blok usługi App Service w witrynie Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Menu po lewej stronie zawiera różne strony służące do konfigurowania aplikacji. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Python z PostgreSQL](app-service-web-tutorial-docker-python-postgresql-app.md)

