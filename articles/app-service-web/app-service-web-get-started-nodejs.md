---
title: Tworzenie aplikacji internetowej Node.js na platformie Azure | Microsoft Docs
description: "Wdróż swoją pierwszą aplikację Hello World w środowisku Node.js w usłudze Azure App Service Web Apps w ciągu kilku minut."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/05/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: f7479260c7c2e10f242b6d8e77170d4abe8634ac
ms.openlocfilehash: 665f55f79eb6e09bce1b5130efc01c76ccbc9109
ms.contentlocale: pl-pl
ms.lasthandoff: 06/21/2017

---
<a id="create-a-nodejs-web-app-in-azure" class="xliff"></a>

# Tworzenie aplikacji internetowej Node.js na platformie Azure

Usługa [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) oferuje wysoce skalowalną i samonaprawialną usługę hostowaną w Internecie.  Ten samouczek Szybki start przedstawia sposób wdrażania aplikacji Node.js w usłudze Azure Web Apps. Aplikację internetową możesz utworzyć przy użyciu [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), a usługa Git umożliwia wdrażanie przykładowego kodu w języku Node.js w aplikacji internetowej.

![Przykładowa aplikacja działająca na platformie Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

Poniższe kroki możesz wykonać przy użyciu komputera z systemem Mac, Windows lub Linux. Po zainstalowaniu wymagań wstępnych wykonanie czynności trwa około pięciu minut.

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne

Aby ukończyć ten przewodnik Szybki Start:

* [Zainstaluj oprogramowanie Git](https://git-scm.com/)
* [Zainstaluj środowisko Node.js i menedżer NPM](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten temat będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

<a id="download-the-sample" class="xliff"></a>

## Pobierz przykład

W oknie terminala uruchom następujące polecenie, aby sklonować przykładowe repozytorium aplikacji na maszynę lokalną.

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

To okno terminala umożliwia uruchamianie wszystkich poleceń z tego przewodnika Szybki start.

Przejdź do katalogu, który zawiera przykładowy kod.

```bash
cd nodejs-docs-hello-world
```

<a id="run-the-app-locally" class="xliff"></a>

## Lokalne uruchamianie aplikacji

Uruchom aplikację lokalnie, otwierając okno terminala i korzystając ze skryptu `npm start` w celu uruchomienia wbudowanego serwera HTTP środowiska Node.js.

```bash
npm start
```

Otwórz przeglądarkę internetową i przejdź do przykładowej aplikacji pod adresem http://localhost:1337.

Na stronie zostanie wyświetlony komunikat **Hello World** z przykładowej aplikacji.

![Przykładowa aplikacja działająca w środowisku lokalnym](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

W oknie terminalu naciśnij kombinację klawiszy **Ctrl + C**, aby zamknąć serwer sieci Web.

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Pusta strona aplikacji internetowej](media/app-service-web-get-started-php/app-service-web-service-created.png)

Na platformie Azure została utworzona nowa pusta aplikacja internetowa.

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 23, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (23/23), 3.71 KiB | 0 bytes/s, done.
Total 23 (delta 8), reused 7 (delta 1)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'bf114df591'.
remote: Generating deployment script.
remote: Generating deployment script for node.js Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling node.js deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.js'
remote: Copying file: 'package.json'
remote: Copying file: 'process.json'
remote: Deleting file: 'hostingstart.html'
remote: Ignoring: .git
remote: Using start-up script index.js from package.json.
remote: Node.js versions available on the platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file to choose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

<a id="browse-to-the-app" class="xliff"></a>

## Przechodzenie do aplikacji

Przejdź do wdrożonej aplikacji za pomocą przeglądarki sieci Web.

```bash
http://<app_name>.azurewebsites.net
```

Przykładowy kod w języku Node.js jest uruchamiany w aplikacji internetowej usługi Azure App Service.

![Przykładowa aplikacja działająca na platformie Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

**Gratulacje!** Udało Ci się wdrożyć pierwszą własną aplikację w języku PHP w usłudze App Service.

<a id="update-and-redeploy-the-code" class="xliff"></a>

## Aktualizowanie i ponowne wdrażanie kodu

Za pomocą edytora tekstów otwórz plik `index.js` w aplikacji Node.js i wprowadź niewielką zmianę w tekście w wywołaniu `response.end`:

```nodejs
response.end("Hello Azure!");
```

Zatwierdź zmiany w narzędziu Git, a następnie wypchnij zmiany kodu na platformę Azure.

```bash
git commit -am "updated output"
git push azure master
```

Po zakończeniu wdrożenia przejdź z powrotem do okna przeglądarki otwartego w kroku **przechodzenia do aplikacji**, a następnie kliknij przycisk Odśwież.

![Zaktualizowana przykładowa aplikacja działająca na platformie Azure](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

<a id="manage-your-new-azure-web-app" class="xliff"></a>

## Zarządzanie nową aplikacją sieci Web platformy Azure

Przejdź do witryny <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, aby zarządzać utworzoną aplikacją internetową.

W menu po lewej stronie kliknij pozycję **App Services**, a następnie kliknij nazwę swojej aplikacji internetowej platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Zostanie wyświetlona strona Omówienie aplikacji internetowej. Tutaj możesz wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie. 

![Blok usługi App Service w witrynie Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Menu po lewej stronie zawiera różne strony służące do konfigurowania aplikacji. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a id="next-steps" class="xliff"></a>

## Następne kroki

> [!div class="nextstepaction"]
> [Środowisko Node.js z bazą danych MongoDB](app-service-web-tutorial-nodejs-mongodb-app.md)

