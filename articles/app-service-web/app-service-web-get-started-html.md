---
title: "Wdrażanie pierwszej aplikacji sieci Web na platformie Azure w ciągu pięciu minut (interfejs wiersza polecenia 2.0 w wersji zapoznawczej) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając przykładową aplikację. Szybko rozpocznij rzeczywiste tworzenie aplikacji i od razu zobacz wyniki."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 05e61d2fc751c4239aef4b10ad897765c59fe928
ms.openlocfilehash: 42f3f17366a37e10d746fe177a93a2432dbdcca2


---
# <a name="deploy-your-first-web-app-to-azure-in-five-minutes-cli-20-preview"></a>Wdrażanie pierwszej aplikacji sieci Web na platformie Azure w ciągu pięciu minut (interfejs wiersza polecenia 2.0 w wersji zapoznawczej)

> [!div class="op_single_selector"]
> * [Pierwsza witryna HTML](app-service-web-get-started-html.md)
> * [Pierwsza aplikacja platformy .NET](app-service-web-get-started-dotnet.md)
> * [Pierwsza aplikacja PHP](app-service-web-get-started-php.md)
> * [Pierwsza aplikacja Node.js](app-service-web-get-started-nodejs.md)
> * [Pierwsza aplikacja w języku Python](app-service-web-get-started-python.md)
> * [Pierwsza aplikacja w języku Java](app-service-web-get-started-java.md)
> 
> 

Ten samouczek ułatwia wdrażanie prostej aplikacji sieci Web korzystającej z kodu HTML i CSS w [usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md).
Za pomocą usługi App Service można tworzyć aplikacje sieci Web, [zaplecza aplikacji mobilnych](/documentation/learning-paths/appservice-mobileapps/) oraz [aplikacje interfejsu API](../app-service-api/app-service-api-apps-why-best-platform.md).

Wykonasz następujące zadania: 

* utworzysz aplikację sieci Web w usłudze Azure App Service;
* wdrożysz w niej kod HTML i CSS;
* sprawdzisz działanie stron w środowisku produkcyjnym;
* zaktualizujesz zawartość w taki sam sposób, w jaki [wypycha się zatwierdzenia Git](https://git-scm.com/docs/git-push).

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>Wersje interfejsu wiersza polecenia umożliwiające wykonanie zadania

Zadanie można wykonać przy użyciu jednej z następujących wersji interfejsu wiersza polecenia:

- [Interfejs wiersza polecenia platformy Azure w wersji 1.0](app-service-web-get-started-html-cli-nodejs.md) — nasz interfejs wiersza polecenia dla klasycznego modelu wdrażania i modelu wdrażania na potrzeby zarządzania zasobami
- [Interfejs wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza)](app-service-web-get-started-html.md) — nasz interfejs wiersza polecenia nowej generacji dla modelu wdrażania na potrzeby zarządzania zasobami

## <a name="prerequisites"></a>Wymagania wstępne
* [Git](http://www.git-scm.com/downloads).
* [Interfejs wiersza polecenia platformy Azure w wersji 2.0 (wersja zapoznawcza)](/cli/azure/install-az-cli2).
* Konto platformy Microsoft Azure. Jeśli nie masz konta, możesz [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Usługę [App Service](http://go.microsoft.com/fwlink/?LinkId=523751) możesz wypróbować, nie mając konta platformy Azure. Utwórz aplikację startową i testuj ją nawet przez godzinę — bez kart kredytowych i bez zobowiązań.
> 
> 

## <a name="deploy-a-simple-html-site"></a>Wdrożenie prostej witryny HTML
1. Otwórz nowy wiersz polecenia systemu Windows, okno programu PowerShell, powłokę systemu Linux lub terminal systemu OS X. Uruchom polecenia `git --version` i `azure --version`, aby upewnić się, że oprogramowanie Git i interfejs wiersza polecenia platformy Azure są zainstalowane na komputerze.
   
    ![Testowanie instalacji narzędzi interfejsu wiersza polecenia dla pierwszej aplikacji sieci Web na platformie Azure](./media/app-service-web-get-started-languages/1-test-tools-2.0.png)
   
    Jeśli narzędzia nie zostały zainstalowane, pobierz je przy użyciu z linków w sekcji [Wymagania wstępne](#Prerequisites).
2. Zaloguj się do platformy Azure w następujący sposób:
   
        az login
   
    Postępuj zgodnie z instrukcjami w komunikacie pomocy, aby kontynuować proces logowania.
   
    ![Logowanie do platformy Azure w celu utworzenia pierwszej aplikacji sieci Web](./media/app-service-web-get-started-languages/3-azure-login-2.0.png)

3. Ustaw użytkownika wdrożenia dla usługi App Service. Kod zostanie później wdrożony przy użyciu tych poświadczeń.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Utwórz nową [grupę zasobów](../azure-resource-manager/resource-group-overview.md). Aby skorzystać z pierwszego samouczka dotyczącego usługi App Service, nie musisz wiedzieć, co to jest.

        az group create --location "<location>" --name my-first-app-group

    Aby sprawdzić, jakich wartości można użyć dla lokalizacji `<location>`, użyj polecenia interfejsu wiersza polecenia `az appservice list-locations`.

3. Utwórz nowy, BEZPŁATNY [plan usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). W przypadku pierwszego samouczka usługi App Service nie będzie naliczana opłata za aplikacje sieci Web w ramach tego planu.

        az appservice plan create --name my-free-appservice-plan --resource-group my-first-app-group --sku FREE

4. Utwórz nową aplikację sieci Web o unikatowej nazwie wprowadzonej w tagu `<app_name>`.

        az appservice web create --name <app_name> --resource-group my-first-app-group --plan my-free-appservice-plan

4. Następnie pobierz przykładową witrynę HTML, którą chcesz wdrożyć. Przejdź do katalogu roboczego (`CD`) i sklonuj przykładową aplikację w następujący sposób:
   
        cd <working_directory>
        git clone https://github.com/Azure-Samples/app-service-web-html-get-started.git

5. Przejdź do repozytorium przykładowej aplikacji. 
   
        cd app-service-web-html-get-started
5. Skonfiguruj lokalne wdrożenie narzędzia Git dla aplikacji sieci Web usługi App Service za pomocą następującego polecenia:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-first-app-group

    Otrzymasz dane wyjściowe JSON podobne do następujących, co oznacza, że skonfigurowano zdalne repozytorium Git:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Dodaj adres URL w formacie JSON jako zdalny obiekt narzędzia Git lokalnego repozytorium Git (nazywanego dla uproszczenia `azure`).

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Wdróż przykładowy kod do nowej aplikacji platformy Azure, tak jak wypycha się dowolny kod przy użyciu narzędzia Git. Po wyświetleniu monitu należy użyć hasła, które zostało wcześniej skonfigurowane.
   
        git push azure master
   
    ![Wypychanie kodu do pierwszej aplikacji sieci Web na platformie Azure](./media/app-service-web-get-started/5-push-code.png)
   
    Jeśli użyto jednego ze środowisk języka, wyświetlone dane wyjściowe będą się różnić. Dzieje się tak, ponieważ polecenie `git push` nie tylko umieszcza kod na platformie Azure, ale wyzwala również zadania wdrażania w aparacie wdrażania. Jeśli w katalogu głównym (repozytorium) projektu znajduje się plik package.json (Node.js) lub requirements.txt (Python) albo w projekcie ASP.NET znajduje się plik packages.config, wymagane pakiety zostaną przywrócone przez skrypty wdrażania. Możesz również [włączyć rozszerzenie Composer](web-sites-php-mysql-deploy-use-git.md#composer), aby automatycznie przetwarzać pliki composer.json w aplikacji w języku PHP.

Gratulacje! Udało Ci się wdrożyć aplikację w usłudze Azure App Service.

## <a name="see-your-app-running-live"></a>Wyświetlanie działającej aplikacji
Aby wyświetlić aplikację działającą na platformie Azure, uruchom to polecenie w dowolnym katalogu repozytorium:

    azure site browse

## <a name="make-updates-to-your-app"></a>Aktualizowanie aplikacji
Teraz możesz wypychać zmiany z katalogu głównego projektu (repozytorium) przy użyciu narzędzia Git, aby w dowolnej chwili aktualizować działającą witrynę. Procedura jest taka sama, jak podczas wdrażania kodu po raz pierwszy. Na przykład za każdym razem, gdy chcesz wypchnąć nową zmianę przetestowaną lokalnie, uruchom następujące polecenia w katalogu głównym (repozytorium) projektu:

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Następne kroki
Znajdź czynności tworzenia i wdrażania aplikacji w preferowanym środowisku języka:

* [.NET](web-sites-dotnet-get-started.md)
* [PHP](app-service-web-php-get-started.md)
* [Node.js](app-service-web-nodejs-get-started.md)
* [Python](web-sites-python-ptvs-django-mysql.md)
* [Java](web-sites-java-get-started.md)

Możesz także wykonać inne zadania związane ze swoją pierwszą aplikacją sieci Web . Na przykład:

* Wypróbuj [inne sposoby wdrożenia kodu na platformie Azure](web-sites-deploy.md). Na przykład w celu wdrożenia jednego z naszych repozytoriów GitHub wystarczy wybrać pozycję **GitHub** zamiast **Lokalne repozytorium Git** w obszarze **Opcje wdrażania**.
* Przenieś swoją aplikację Azure na wyższy poziom. Uwierzytelniaj użytkowników. Skaluj ją zależnie od potrzeb. Skonfiguruj alerty dotyczące wydajności. Wszystkie te czynności możesz wykonać za pomocą kilku kliknięć. Zobacz [Dodawanie funkcji do pierwszej aplikacji sieci Web](app-service-web-get-started-2.md).




<!--HONumber=Jan17_HO1-->


