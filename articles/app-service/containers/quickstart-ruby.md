---
title: "Tworzenie aplikacji Ruby i wdrożyć w usłudze App Service w systemie Linux | Dokumentacja firmy Microsoft"
description: "Informacje dotyczące sposobu tworzenia dopisków fonetycznych aplikacji z usługi aplikacji w systemie Linux."
keywords: "Usługa aplikacji Azure, linux, oss, ruby"
services: app-service
documentationcenter: 
author: SyntaxC4
manager: cfowler
editor: 
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/10/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: a54ef1ae40ba6ea9ad604a29c67e41228c0d5946
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2017
---
# <a name="create-a-ruby-app-in-app-service-on-linux"></a>Tworzenie aplikacji dopisków fonetycznych w usłudze App Service w systemie Linux

[Usługa aplikacji w systemie Linux](app-service-linux-intro.md) oferuje wysoce skalowalną, własnym poprawiania usługi hosta sieci web. Ta opcja szybkiego startu pokazuje, jak utworzyć podstawowy Ruby szyny aplikacji można następnie wdrożyć go na Azure jako aplikacji sieci Web w systemie Linux.

![Witaj świecie](./media/quickstart-ruby/hello-world-updated.png)

## <a name="prerequisites"></a>Wymagania wstępne

* <a href="https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller" target="_blank">Zainstaluj Ruby 2.4.1 lub nowszej</a>
* <a href="https://git-scm.com/" target="_blank">Zainstaluj oprogramowanie Git</a>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

W oknie terminalu, uruchom następujące polecenie, aby klonowanie repozytorium przykładowej aplikacji na komputerze lokalnym:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

## <a name="run-the-application-locally"></a>Uruchamianie aplikacji lokalnie

Serwer szyny są uruchamiane w kolejności dla aplikacji do pracy. Zmień na *hello world* katalogu i `rails server` polecenie uruchamia serwer.

```bash
cd hello-world\bin
rails server
```

Przy użyciu przeglądarki sieci web, przejdź do `http://localhost:3000` Aby przetestować aplikację lokalnie.

![Witaj świecie](./media/quickstart-ruby/hello-world.png)

## <a name="modify-app-to-display-welcome-message"></a>Modyfikowanie aplikacji, aby wyświetlić komunikat powitalny

Modyfikowanie aplikacji, dlatego wyświetla komunikat powitalny. Najpierw należy skonfigurować trasy, modyfikując *~/workspace/ruby-docs-hello-world/config/routes.rb* pliku, aby uwzględnić trasa o nazwie `hello`.

  ```ruby
  Rails.application.routes.draw do
      #For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
      root 'application#hello'
  end
  ```

Umożliwia zmianę kontrolera aplikacji, tak aby zwracało komunikatu jako HTML do przeglądarki. 

Otwórz *~/workspace/hello-world/app/controllers/application_controller.rb* do edycji. Modyfikowanie `ApplicationController` klasy do przeszukania, takich jak w poniższym przykładzie kodu:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

Aplikacja jest teraz skonfigurowany. Przy użyciu przeglądarki sieci web, przejdź do `http://localhost:3000` o potwierdzenie strony docelowej głównego.

![Witaj świecie skonfigurowane](./media/quickstart-ruby/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

## <a name="create-a-ruby-web-app-on-azure"></a>Tworzenie aplikacji sieci web dopisków fonetycznych na platformie Azure

Grupa zasobów jest wymagana do zawiera zasoby potrzebne dla aplikacji sieci web. Aby utworzyć grupę zasobów, użyj [Tworzenie grupy az]() polecenia.

```azurecli-interactive
az group create --location westeurope --name myResourceGroup
```

Użyj [Tworzenie planu usług aplikacji az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) polecenie, aby utworzyć plan usługi app service dla aplikacji sieci web.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

Następnie wystawiania [tworzenie aplikacji sieci Web az](https://docs.microsoft.com/cli/azure/webapp) polecenie, aby utworzyć aplikacji sieci web, który używa usługi nowo utworzony plan. Należy zauważyć, że środowisko wykonawcze ma ustawioną wartość `ruby|2.3`. Nie zapomnij Zastąp `<app name>` przy użyciu unikatowej nazwy aplikacji.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> \
--runtime "ruby|2.3" --deployment-local-git
```

Dane wyjściowe polecenia ujawnia informacje o nowo utworzonej aplikacji sieci web, a także adres URL wdrożenia. Powinien wyglądać podobnie do poniższego przykładu. Skopiuj adres URL do późniejszego użycia, w tym samouczku.

```bash
https://<deployment user name>@<app name>.scm.azurewebsites.net/<app name>.git
```

Po utworzeniu aplikacji sieci web **omówienie** strona jest dostępny do wyświetlenia. Przejdź do niego. Następująca strona powitalny jest wyświetlane:

![Strony powitalnej](./media/quickstart-ruby/splash-page.png)


## <a name="deploy-your-application"></a>Wdrażanie aplikacji

Uruchom następujące polecenia w celu wdrożenia lokalnej aplikacji do witryny sieci Web platformy Azure:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Upewnij się, że operacje zdalnego wdrażania raportu Powodzenie. Polecenia tworzy dane wyjściowe podobne do następującego tekstu:

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

Po zakończeniu wdrożenia należy ponownie uruchomić aplikacji sieci web do wdrożenia, które zostały wprowadzone za pomocą [ponowne uruchomienie aplikacji sieci Web az](https://docs.microsoft.com/cli/azure/webapp#az_webapp_restart) polecenia, jak pokazano poniżej:

```azurecli-interactive
az webapp restart --name <app name> --resource-group myResourceGroup
```

Przejdź do witryny i sprawdź wyniki.

```bash
http://<app name>.azurewebsites.net
```

![zaktualizowano aplikację sieci web](./media/quickstart-ruby/hello-world-updated.png)

> [!NOTE]
> Podczas ponownego uruchamiania aplikacji próby przeglądania wyników witryny w kod stanu HTTP `Error 503 Server unavailable`. Może upłynąć kilka minut w pełni ponowne uruchomienie.
>

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Następne kroki

[Usługa aplikacji Azure w systemie Linux — często zadawane pytania](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)
