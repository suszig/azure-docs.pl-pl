---
title: "Tworzenie aplikacji w języku Node.js w usłudze Web App platformy Azure | Microsoft Docs"
description: "Wdróż swoją pierwszą aplikację Hello World w języku Node.js w usłudze App Service Web App w ciągu kilku minut."
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: ced6f54603120d8832ee417b02b6673f80a99613
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017

---
# <a name="create-a-nodejs-application-on-web-app"></a>Tworzenie aplikacji w języku Node.js w usłudze Web App

Ten samouczek szybkiego startu poprowadzi Cię przez tworzenie i wdrażanie aplikacji w języku Node.js na platformie Azure. Uruchomimy aplikację za pomocą [planu usługi Azure App Service](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) oraz utworzymy i skonfigurujemy w niej nową aplikację sieci Web przy użyciu interfejsu wiersza polecenia platformy Azure. Następnie użyjemy narzędzia Git do wdrożenia aplikacji w języku Node.js na platformie Azure.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

Poniższe kroki możesz wykonać przy użyciu komputera z systemem Mac, Windows lub Linux. Wykonanie wszystkich poniższych czynności powinno zająć tylko około 5 minut.

## <a name="prerequisites"></a>Wymagania wstępne

Przed utworzeniem tego przykładu pobierz i zainstaluj poniższe:

* [Git](https://git-scm.com/)
* [Node.js i NPM](https://nodejs.org/)
* [Interfejs wiersza polecenia platformy Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

Sklonuj repozytorium przykładowej aplikacji Hello World na komputer lokalny.

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

Przejdź do katalogu, który zawiera przykładowy kod.

```bash
cd nodejs-docs-hello-world
```

## <a name="run-the-app-locally"></a>Lokalne uruchamianie aplikacji

Uruchom aplikację lokalnie, otwierając okno terminalu i korzystając ze skryptu `npm start` w celu uruchomienia przez przykład wbudowanego serwera HTTP Node.js.

```bash
npm start
```

Otwórz przeglądarkę sieci Web i przejdź do przykładu.

```bash
http://localhost:1337
```

Na stronie zostanie wyświetlony komunikat **Witaj, świecie** z przykładowej aplikacji.

![localhost-hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

W oknie terminalu naciśnij kombinację klawiszy **Ctrl + C**, aby zamknąć serwer sieci Web.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Teraz użyjemy interfejsu wiersza polecenia platformy Azure 2.0 w oknie terminalu do utworzenia zasobów wymaganych do hostowania tej aplikacji w języku Node.js na platformie Azure. Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```azurecli
az login
```

<!-- ## Configure a Deployment User -->
[!INCLUDE [login-to-azure](../../includes/configure-deployment-user.md)]

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure, takich jak aplikacje sieci Web, bazy danych i konta magazynu, oraz zarządzania nimi.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-an-azure-app-service-plan"></a>Tworzenie planu usługi Azure App Service

Utwórz „BEZPŁATNY” [plan usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) za pomocą polecenia [az appservice plan create](/cli/azure/appservice/plan#create).

<!--
 An App Service plan represents the collection of physical resources used to ..
-->
[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Poniższy przykład tworzy plan usługi App Service o nazwie `quickStartPlan` przy użyciu warstwy cenowej **Bezpłatna**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

Po utworzeniu planu usługi App Service interfejs wiersza polecenia platformy Azure wyświetli informacje podobne do następujących:

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "location": "West Europe",
    "sku": {
    "capacity": 1,
    "family": "S",
    "name": "S1",
    "tier": "Standard"
    },
    "status": "Ready",
    "type": "Microsoft.Web/serverfarms"
}
```

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web

Teraz, gdy został utworzony plan usługi App Service, utwórz [aplikację sieci Web](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) w ramach planu usługi App Service `quickStartPlan`. Aplikacja sieci Web zapewnia miejsce hostowania do wdrożenia kodu oraz udostępnia adres URL w celu wyświetlania wdrożonej aplikacji. Użyj polecenia [az appservice web create](/cli/azure/appservice/web#create), aby utworzyć aplikację sieci Web.

W poniższym poleceniu wstaw własną unikatową nazwę aplikacji w miejsce symbolu zastępczego `<app_name>`. Aplikacja `<app_name>` jest używana w witrynie DNS domyślnej dla aplikacji sieci Web. Jeśli nazwa `<app_name>` nie jest unikatowa, zostanie wyświetlony przyjazny komunikat o błędzie „Witryna sieci Web o nazwie <app_name> już istnieje”.

<!-- removed per https://github.com/Microsoft/azure-docs-pr/issues/11878
You can later map any custom DNS entry to the web app before you expose it to your users.
-->

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Po utworzeniu aplikacji sieci Web interfejs wiersza polecenia platformy Azure wyświetli informacje podobne do następujących.

```json
{
    "clientAffinityEnabled": true,
    "defaultHostName": "<app_name>.azurewebsites.net",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>",
    "isDefaultContainer": null,
    "kind": "app",
    "location": "West Europe",
    "name": "<app_name>",
    "repositorySiteName": "<app_name>",
    "reserved": true,
    "resourceGroup": "myResourceGroup",
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "state": "Running",
    "type": "Microsoft.Web/sites",
}
```

Przejdź do witryny, aby zobaczyć nowo utworzoną aplikację sieci Web.

```bash
http://<app_name>.azurewebsites.net
```

![app-service-web-service-created](media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)

Utworzyliśmy pustą nową aplikację sieci Web na platformie Azure.

## <a name="configure-local-git-deployment"></a>Konfigurowanie lokalnego wdrożenia narzędzia Git

Wdrożenie w aplikacji sieci Web można przeprowadzić na wiele różnych sposobów, w tym z użyciem protokołu FTP, lokalnego narzędzia Git i usługi GitHub oraz usług Visual Studio Team Services i Bitbucket.

Użyj polecenia [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git), aby skonfigurować dostęp lokalnego narzędzia Git do aplikacji sieci Web.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Skopiuj dane wyjściowe z terminalu, ponieważ zostaną one użyte w następnym kroku.

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

## <a name="push-to-azure-from-git"></a>Wypychanie z narzędzia Git na platformę Azure

Dodaj zdalną platformę Azure do lokalnego repozytorium Git.

```bash
git remote add azure <paste-previous-command-output-here>
```

Wypchnij na zdalną platformę Azure w celu wdrożenia aplikacji. Zostanie wyświetlony monit o podanie hasła określonego wcześniej podczas tworzenia użytkownika wdrożenia. Upewnij się, że zostało wprowadzone hasło utworzone w [konfiguracji użytkownika wdrożenia](#configure-a-deployment-user), a nie hasło, którego używasz do logowania się w witrynie Azure Portal.

```bash
git push azure master
```

Podczas wdrażania usługa Azure App Service przekaże postęp za pomocą narzędzi Git.

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

## <a name="browse-to-the-app"></a>Przechodzenie do aplikacji

Przejdź do wdrożonej aplikacji za pomocą przeglądarki sieci Web.

```bash
http://<app_name>.azurewebsites.net
```

Tym razem strona, która wyświetla komunikat Witaj, świecie, jest uruchomiona przy użyciu kodu języka Node.js działającego jako aplikacja sieci Web usługi Azure App Service.

## <a name="updating-and-deploying-the-code"></a>Aktualizowanie i wdrażanie kodu

Za pomocą lokalnego edytora tekstów otwórz plik `index.js` w aplikacji Node.js i wprowadź niewielką zmianę w tekście w wywołaniu `response.end`:

```nodejs
response.end("Hello Azure!");
```

Zatwierdź zmiany w narzędziu Git, a następnie wypchnij zmiany kodu na platformę Azure.

```bash
git commit -am "updated output"
git push azure master
```

Po zakończeniu wdrożenia przejdź z powrotem do okna przeglądarki otwartego w kroku **przechodzenia do aplikacji**, a następnie kliknij przycisk Odśwież.

![hello-world-in-browser](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Zarządzanie nową aplikacją sieci Web platformy Azure

Przejdź do witryny Azure Portal, aby przyjrzeć się właśnie utworzonej aplikacji sieci Web.

W tym celu zaloguj się do witryny [https://portal.azure.com](https://portal.azure.com).

W lewym menu kliknij pozycję **App Services**, a następnie kliknij nazwę swojej aplikacji sieci Web platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Jesteś teraz w _bloku_ aplikacji sieci Web (stronie portalu, która jest otwierana w poziomie).

Domyślnie blok aplikacji sieci Web wyświetla stronę **Przegląd**. Ta strona udostępnia widok sposobu działania aplikacji. Tutaj możesz również wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie. Karty po lewej stronie bloku pokazują różne strony konfiguracji, które można otworzyć.

![Blok usługi App Service w witrynie Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Te karty w bloku pokazują wiele doskonałych funkcji, które możesz dodać do aplikacji sieci Web. Poniższa lista zawiera tylko kilka możliwości:

* Mapowanie niestandardowej nazwy DNS
* Tworzenie powiązania niestandardowego certyfikatu SSL
* Konfigurowanie ciągłego wdrażania
* Skalowanie w górę i w dół
* Dodawanie uwierzytelniania użytkownika

**Gratulacje!** Udało Ci się wdrożyć pierwszą własną aplikację w języku Node.js w usłudze App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

> [!div class="nextstepaction"]
> [Zapoznaj się z przykładowymi skryptami interfejsu wiersza polecenia w usługach Web Apps](app-service-cli-samples.md)

