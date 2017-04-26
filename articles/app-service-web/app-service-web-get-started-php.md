---
title: "Tworzenie aplikacji w języku PHP w usłudze Web App | Microsoft Docs"
description: "Wdróż swoją pierwszą aplikację Hello World w języku PHP w usłudze App Service Web App w ciągi kilku minut."
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
ms.topic: hero-article
ms.date: 03/31/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: d5126f3b9fa92ff95eaa8bc06554c49f9836bab9
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-php-application-on-web-app"></a>Tworzenie aplikacji w języku PHP w usłudze Web App

Ten samouczek szybkiego startu poprowadzi Cię przez tworzenie i wdrażanie aplikacji w języku PHP na platformie Azure. Uruchomimy aplikację za pomocą opartej na systemie Linux usługi Azure App Service oraz utworzymy i skonfigurujemy w niej nową aplikację sieci Web przy użyciu interfejsu wiersza polecenia platformy Azure. Następnie użyjemy narzędzia Git do wdrożenia aplikacji w języku PHP na platformie Azure.

![hello-world-in-browser](media/app-service-web-get-started-php/hello-world-in-browser.png)

Poniższe kroki możesz wykonać przy użyciu komputera z systemem Mac, Windows lub Linux. Wykonanie wszystkich poniższych czynności powinno zająć tylko około 5 minut.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed uruchomieniem tego przykładu zainstaluj lokalnie następujące wymagania wstępne:

1. [Pobierz i zainstaluj narzędzie Git](https://git-scm.com/)
1. [Pobierz i zainstaluj środowisko PHP](https://php.net)
1. Pobierz i zainstaluj [interfejs wiersza polecenia platformy Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

Sklonuj repozytorium przykładowej aplikacji Hello World na komputer lokalny.

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
```

> [!TIP]
> Alternatywnie możesz [pobrać przykład](https://github.com/Azure-Samples/php-docs-hello-world/archive/master.zip) jako plik zip i wyodrębnić go.

Przejdź do katalogu, który zawiera przykładowy kod.

```bash
cd php-docs-hello-world
```

## <a name="run-the-app-locally"></a>Lokalne uruchamianie aplikacji

Uruchom aplikację lokalnie, otwierając okno terminalu i korzystając z wiersza polecenia `php` w celu uruchomienia przez przykład wbudowanego serwera sieci Web PHP.

```bash
php -S localhost:8080
```

Otwórz przeglądarkę sieci Web i przejdź do przykładu.

```bash
http://localhost:8080
```

Na stronie zostanie wyświetlony komunikat **Witaj, świecie** z przykładowej aplikacji.

![localhost-hello-world-in-browser](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

W oknie terminalu naciśnij kombinację klawiszy **Ctrl + C**, aby zamknąć serwer sieci Web.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Teraz użyjemy interfejsu wiersza polecenia platformy Azure 2.0 w oknie terminalu do utworzenia zasobów wymaganych do hostowania tej aplikacji w języku PHP na platformie Azure. Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```azurecli
az login
```

## <a name="configure-a-deployment-user"></a>Konfigurowanie użytkownika wdrożenia

Dla protokołu FTP i lokalnego narzędzia Git należy mieć użytkownika wdrożenia skonfigurowanego na serwerze w celu uwierzytelnienia wdrożenia. Tworzenie użytkownika wdrożenia to konfiguracja jednorazowa, przy czym należy zanotować nazwę użytkownika i hasło, ponieważ zostaną one użyte w kroku poniżej.

> [!NOTE]
> Użytkownik wdrożenia jest wymagany do wdrożenia protokołu FTP i lokalnego narzędzia Git w aplikacji sieci Web.
> Wartości `username` i `password` dotyczą poziomu konta i jako takie różnią się od poświadczeń subskrypcji platformy Azure. **Te poświadczenia wystarczy utworzyć tylko raz**.
>

Użyj polecenia [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set), aby utworzyć swoje poświadczenia na poziomie konta.

```azurecli
az appservice web deployment user set --user-name <username> --password <password>
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure, takich jak aplikacje sieci Web, bazy danych i konta magazynu, oraz zarządzania nimi.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-an-azure-app-service"></a>Tworzenie usługi Azure App Service

Utwórz plan usługi App Service opartej na systemie Linux za pomocą polecenia [az appservice plan create](/cli/azure/appservice/plan#create).

> [!NOTE]
> Plan usługi App Service reprezentuje kolekcję zasobów fizycznych służących do hostowania aplikacji. Wszystkie aplikacje przypisane do planu usługi App Service współdzielą zasoby przez niego zdefiniowane, ograniczając koszt hostowania wielu aplikacji.
>
> Plany usługi App Service definiują następujące elementy:
> * Region (Europa Północna, Wschodnie stany USA, Azja Południowo-Wschodnia)
> * Rozmiar wystąpienia (mały, średni, duży)
> * Wartość licznika skali (jedno, dwa lub trzy wystąpienia itp.)
> * Jednostka magazynowa (Bezpłatna, Współdzielona, Podstawowa, Standardowa, Premium)
>

Poniższy przykład tworzy plan usługi App Service dla procesów roboczych systemu Linux o nazwie `quickStartPlan` przy użyciu warstwy cenowej **Standardowa**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku S1 --is-linux
```

Po utworzeniu planu usługi App Service interfejs wiersza polecenia platformy Azure wyświetli informacje podobne do następujących.

```json
{
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
    "kind": "linux",
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

Teraz, gdy został utworzony plan usługi App Service, utwórz aplikację sieci Web w ramach planu usługi App Service `quickStartPlan`. Aplikacja sieci Web zapewnia miejsce hostowania do wdrożenia kodu oraz udostępnia adres URL w celu wyświetlania wdrożonej aplikacji. Użyj polecenia [az appservice web create](/cli/azure/appservice/web#create), aby utworzyć aplikację sieci Web.

W poniższym poleceniu wstaw własną unikatową nazwę aplikacji tam, gdzie zobaczysz symbol zastępczy <app_name>. Nazwa <app_name> będzie używana jako domyślna witryna DNS aplikacji sieci Web, więc nazwa ta musi być unikatowa wśród wszystkich aplikacji na platformie Azure. Później możesz zamapować dowolny niestandardowy wpis DNS na aplikację sieci Web, zanim udostępnisz ją użytkownikom.

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

![app-service-web-service-created](media/app-service-web-get-started-php/app-service-web-service-created.png)

Utworzyliśmy pustą nową aplikację sieci Web na platformie Azure. Skonfigurujmy teraz aplikację sieci Web do używania środowiska PHP i przeprowadźmy wdrożenie aplikacji w nim.

## <a name="configure-to-use-php"></a>Konfigurowanie do używania środowiska PHP

Użyj polecenia [az appservice web config update](/cli/azure/app-service/web/config#update), aby skonfigurować aplikację sieci Web do używania środowiska PHP w wersji `7.0.x`.

> [!TIP]
> Ustawienie wersji środowiska PHP w ten sposób powoduje używanie domyślnego kontenera dostarczonego przez platformę. Jeśli chcesz użyć własnego kontenera, zapoznaj się z dokumentacją interfejsu wiersza polecenia dla polecenia [az appservice web config container update](https://docs.microsoft.com/cli/azure/appservice/web/config/container#update).

```azurecli
az appservice web config update --linux-fx-version "PHP|7.0" --name <app_name> --resource-group myResourceGroup
```

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

Wypchnij na zdalną platformę Azure w celu wdrożenia aplikacji. Zostanie wyświetlony monit o podanie hasła określonego wcześniej w ramach tworzenia użytkownika wdrożenia.

```azurecli
git push azure master
```

Podczas wdrażania usługa Azure App Service przekaże postęp za pomocą narzędzi Git.

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

Tym razem strona, która wyświetla komunikat Witaj, świecie, jest uruchomiona przy użyciu kodu języka PHP działającego jako aplikacja sieci Web usługi Azure App Service.



## <a name="updating-and-deploying-the-code"></a>Aktualizowanie i wdrażanie kodu

Za pomocą lokalnego edytora tekstów otwórz plik `index.php` w aplikacji w języku PHP i wprowadź niewielką zmianę w tekście ciągu obok polecenia `echo`:

```php
echo "Hello Azure!";
```

Zatwierdź zmiany w narzędziu Git, a następnie wypchnij zmiany kodu na platformę Azure.

```bash
git commit -am "updated output"
git push azure master
```

Po zakończeniu wdrożenia przejdź z powrotem do okna przeglądarki otwartego w kroku przechodzenia do aplikacji, a następnie kliknij przycisk Odśwież.

![hello-world-in-browser](media/app-service-web-get-started-php/hello-world-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Zarządzanie nową aplikacją sieci Web platformy Azure

Przejdź do witryny Azure Portal, aby przyjrzeć się właśnie utworzonej aplikacji sieci Web.

W tym celu zaloguj się do witryny [https://portal.azure.com](https://portal.azure.com).

W lewym menu kliknij pozycję **App Services**, a następnie kliknij nazwę swojej aplikacji sieci Web platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

Jesteś teraz w _bloku_ aplikacji sieci Web (stronie portalu, która jest otwierana w poziomie).

Domyślnie blok aplikacji sieci Web wyświetla stronę **Przegląd**. Ta strona udostępnia widok sposobu działania aplikacji. Tutaj możesz również wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie. Karty po lewej stronie bloku pokazują różne strony konfiguracji, które można otworzyć.

![Blok usługi App Service w witrynie Azure Portal](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

Te karty w bloku pokazują wiele doskonałych funkcji, które możesz dodać do aplikacji sieci Web. Poniższa lista zawiera tylko kilka możliwości:

* Mapowanie niestandardowej nazwy DNS
* Tworzenie powiązania niestandardowego certyfikatu SSL
* Konfigurowanie ciągłego wdrażania
* Skalowanie w górę i w dół
* Dodawanie uwierzytelniania użytkownika

**Gratulacje!** Udało Ci się wdrożyć pierwszą własną aplikację w języku PHP w usłudze App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z wcześniej utworzonymi [skryptami interfejsu wiersza polecenia aplikacji sieci Web](app-service-cli-samples.md).
