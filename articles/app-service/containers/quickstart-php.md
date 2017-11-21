---
title: "Tworzenie aplikacji sieci web PHP i wdrożyć w usłudze App Service w systemie Linux | Dokumentacja firmy Microsoft"
description: "Wdrażanie Twojego pierwszego PHP Hello World w usłudze App Service w systemie Linux w minutach."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/30/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 34da73e5feca1782f3e107fd30d268df4f9883d8
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
---
# <a name="create-a-php-web-app-in-app-service-on-linux"></a>Tworzenie aplikacji sieci web PHP w usłudze App Service w systemie Linux

[Usługa aplikacji w systemie Linux](app-service-linux-intro.md) oferuje wysoce skalowalną, własnym poprawiania usługi hosta sieci web przy użyciu systemu operacyjnego Linux. Ten samouczek Szybki Start pokazano, jak wdrożyć aplikację PHP w usłudze Azure App Service w systemie Linux. Tworzenie aplikacji sieci web z wbudowanych obrazu przy użyciu [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) powłoki usługi w chmurze i wdrażanie kodu PHP w aplikacji sieci web przy użyciu narzędzia Git.

! [Przykładowej aplikacji działających na platformie Azure]] (media/quickstart-php/hello-world-in-browser.png)

Poniższe kroki możesz wykonać przy użyciu komputera z systemem Mac, Windows lub Linux.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten przewodnik Szybki Start:

* [Zainstaluj oprogramowanie Git](https://git-scm.com/)
* [Zainstaluj środowisko PHP](https://php.net)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

W oknie terminalu, uruchom następujące polecenia w klonowania przykładowej aplikacji na komputerze lokalnym i przejdź do katalogu zawierającego przykładowy kod.

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
cd php-docs-hello-world
```

## <a name="run-the-app-locally"></a>Lokalne uruchamianie aplikacji

Uruchom aplikację lokalnie, otwierając okno terminala i korzystając z polecenia `php` w celu uruchomienia wbudowanego serwera internetowego środowiska PHP.

```bash
php -S localhost:8080
```

Otwórz przeglądarkę sieci web i przejdź do przykładowej aplikacji w `http://localhost:8080`.

Na stronie zostanie wyświetlony komunikat **Hello World!** z przykładowej aplikacji.

![Przykładowa aplikacja działająca w środowisku lokalnym](media/quickstart-php/localhost-hello-world-in-browser.png)

W oknie terminalu naciśnij kombinację klawiszy **Ctrl + C**, aby zamknąć serwer sieci Web.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-php-no-h.md)] 

Przejdź do witryny, aby wyświetlić aplikację sieci web nowo utworzony przy użyciu wbudowanych obrazu. Zastąp  _&lt;Nazwa aplikacji >_ nazwą aplikacji sieci web.

```bash
http://<app_name>.azurewebsites.net
```

![Pusta strona aplikacji internetowej](media/quickstart-php/app-service-web-service-created.png)

[!INCLUDE [Push to Azure](../../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 2, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 352 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '25f18051e9'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.php'
remote: Ignoring: .git
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-to-the-app"></a>Przechodzenie do aplikacji

Przejdź do wdrożonej aplikacji za pomocą przeglądarki sieci Web.

```bash
http://<app_name>.azurewebsites.net
```

W przykładowym kodzie PHP jest uruchomiony w aplikacji sieci web przy użyciu wbudowanych obrazu.

![Przykładowa aplikacja działająca na platformie Azure](media/quickstart-php/hello-world-in-browser.png)

**Gratulacje!** Pierwszą aplikację PHP została wdrożona w usłudze App Service w systemie Linux.

## <a name="update-locally-and-redeploy-the-code"></a>Lokalne aktualizowanie i ponowne wdrażanie kodu

W katalogu lokalnym, otwórz `index.php` pliku w aplikacji PHP i dalej, aby wprowadzić niewielkie zmiany na tekst w ciągu `echo`:

```php
echo "Hello Azure!";
```

Zatwierdź zmiany w narzędziu Git, a następnie wypchnij zmiany kodu na platformę Azure.

```bash
git commit -am "updated output"
git push azure master
```

Po zakończeniu wdrożenia przejdź z powrotem do okna przeglądarki otwartego w kroku **przechodzenia do aplikacji**, a następnie odśwież stronę.

![Zaktualizowana przykładowa aplikacja działająca na platformie Azure](media/quickstart-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Zarządzanie nową aplikacją sieci Web platformy Azure

Przejdź do witryny <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, aby zarządzać utworzoną aplikacją internetową.

W menu po lewej stronie kliknij pozycję **App Services**, a następnie kliknij nazwę swojej aplikacji internetowej platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/quickstart-php/php-docs-hello-world-app-service-list.png)

Zostanie wyświetlona strona Omówienie aplikacji internetowej. Tutaj możesz wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie.

![Strona usługi App Service w witrynie Azure Portal](media/quickstart-php/php-docs-hello-world-app-service-detail.png)

Menu po lewej stronie zawiera różne strony służące do konfigurowania aplikacji. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Środowisko PHP z bazą danych MySQL](tutorial-php-mysql-app.md)
