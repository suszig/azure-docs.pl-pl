---
title: "Tworzenie aplikacji sieci Web Node.js w usłudze Azure App Service | Microsoft Docs"
description: "Dowiedz się, jak wdrożyć aplikację Node.js w aplikacji sieci Web w usłudze Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 85af23df-54af-4430-8d77-a1f97e2f5b10
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 4b8eb863a1fcb65c307870619ea9d8942c57a906


---
# <a name="create-a-nodejs-web-app-in-azure-app-service"></a>Tworzenie aplikacji sieci Web Node.js w usłudze Azure App Service
> [!div class="op_single_selector"]
> * [.Net](web-sites-dotnet-get-started.md)
> * [Node.js](web-sites-nodejs-develop-deploy-mac.md)
> * [Java](web-sites-java-get-started.md)
> * [PHP — Git](web-sites-php-mysql-deploy-use-git.md)
> * [PHP — FTP](web-sites-php-mysql-deploy-use-ftp.md)
> * [Python](web-sites-python-ptvs-django-mysql.md)
> 
> 

Ten samouczek przedstawia sposób tworzenia prostej aplikacji [Node.js](http://nodejs.org) i wdrażania jej do [aplikacji sieci Web](app-service-web-overview.md) w [usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md) za pomocą usługi [Git](http://git-scm.com). Instrukcje podane w tym samouczku można wykonać w dowolnym systemie operacyjnym, w którym można uruchomić środowisko Node.js.

Dowiesz się:

* Jak utworzyć aplikację sieci Web w usłudze Azure App Service przy użyciu Portalu Azure.
* Jak wdrożyć aplikację Node.js do aplikacji sieci Web, wypychając ją do repozytorium Git aplikacji sieci Web.

Ukończona aplikacja wyświetla krótki ciąg „Hello World” w przeglądarce.

![Przeglądarka wyświetlająca ciąg „Hello World”.][helloworld-completed]

Samouczki i przykładowy kod z bardziej złożonymi aplikacjami Node.js lub inne tematy dotyczące użycia środowiska Node.js na platformie Azure można znaleźć w [Centrum deweloperów Node.js](/develop/nodejs/).

> [!NOTE]
> Do wykonania kroków tego samouczka potrzebne jest konto platformy Microsoft Azure. Jeśli nie masz konta, możesz [aktywować korzyści dla subskrybentów programu Visual Studio](/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) lub [utworzyć konto bezpłatnej wersji próbnej](/en-us/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Wypróbuj usługę App Service](https://azure.microsoft.com/try/app-service/). W tej lokalizacji możesz od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service — bez karty kredytowej i bez zobowiązań.
> 
> 

## <a name="create-a-web-app-and-enable-git-publishing"></a>Tworzenie aplikacji sieci Web i umożliwianie publikowania w usłudze Git
Wykonaj poniższe czynności, aby utworzyć aplikację sieci Web w usłudze Azure App Service i umożliwić publikowanie w usłudze Git. 

[Git](http://git-scm.com/) jest systemem kontroli w wersji rozproszonej, którego możesz użyć do wdrożenia witryny sieci Web platformy Azure. Utworzony kod dla aplikacji sieci Web zostanie zapisany w lokalnym repozytorium Git i wdrożony na platformie Azure przez wypchnięcie do repozytorium zdalnego. Ta metoda wdrażania jest funkcją aplikacji sieci Web w usłudze App Service.  

1. Zaloguj się do [Portalu Azure](https://portal.azure.com).
2. Kliknij ikonę **+ NOWY** znajdującą się po lewej stronie w górnej części Portalu Azure.
3. Kliknij pozycję **Sieci Web i mobilność**, a następnie kliknij pozycję **Aplikacja sieci Web**.
   
    ![][portal-quick-create]
4. Wprowadź nazwę aplikacji sieci Web w polu **Aplikacja sieci Web**.
   
    Ta nazwa musi być unikatowa w domenie azurewebsites.net, ponieważ adres URL aplikacji sieci Web będzie miał format {nazwa}.azurewebsites.net. Jeśli wprowadzona nazwa nie jest unikatowa, w polu tekstowym jest wyświetlany czerwony wykrzyknik.
5. Wybierz pozycję **Subskrypcja**.
6. Wybierz **grupę zasobów** lub utwórz nową.
   
    Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
7. Wybierz pozycję **Plan usługi App Service/Lokalizacja** lub utwórz nowy plan.
   
    Aby uzyskać więcej informacji na temat planów usługi App Service, zobacz [Omówienie planów usługi Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
8. Kliknij przycisk **Utwórz**.
   
    ![][portal-quick-create2]
   
    Platforma Azure szybko (zazwyczaj trwa to nie dłużej niż minutę) kończy tworzenie nowej aplikacji sieci Web.
9. Kliknij pozycję **Aplikacje sieci Web > {nazwa nowej aplikacji sieci Web}**.
   
    ![](./media/web-sites-nodejs-develop-deploy-mac/gotowebapp.png)
10. W bloku **Aplikacja sieci Web** kliknij część **Wdrożenie**.
    
    ![][deployment-part]
11. W bloku **Ciągłe wdrażanie** kliknij pozycję **Wybierz źródło**.
12. Kliknij pozycję **Lokalne repozytorium Git**, a następnie kliknij pozycję **OK**.
    
    ![][setup-git-publishing]
13. Skonfiguruj poświadczenia wdrożenia, jeśli jeszcze nie zostały skonfigurowane.
    
    a. W bloku Aplikacja sieci Web kliknij pozycje **Ustawienia > Poświadczenia wdrożenia**.
    
    ![][deployment-credentials]
    
    b. Utwórz nazwę użytkownika i hasło. 
    
    ![](./media/web-sites-nodejs-develop-deploy-mac/setdeploycreds.png)
14. W bloku Aplikacja sieci Web kliknij pozycję **Ustawienia**, a następnie kliknij pozycję **Właściwości**.
    
    Aby opublikować aplikację, należy wypchnąć ją do zdalnego repozytorium Git. Adres URL repozytorium jest wyświetlany w obszarze **Adres URL usługi GIT**. Ten adres zostanie wykorzystany później w samouczku.
    
    ![][git-url]

## <a name="build-and-test-your-application-locally"></a>Kompilowanie i testowanie aplikacji lokalnie
W tej sekcji utworzysz plik **server.js** zawierający nieco zmodyfikowaną wersję przykładu „Hello World” z witryny [nodejs.org]. Ten kod dodaje port process.env.PORT do nasłuchiwania podczas uruchamiania w aplikacji sieci Web platformy Azure.

1. Utwórz katalog o nazwie *helloworld*.
2. Użyj edytora tekstów, aby utworzyć nowy plik o nazwie **server.js** w katalogu *helloworld*.
3. Skopiuj następujący kod do pliku **server.js**, a następnie zapisz plik:
   
        var http = require('http')
        var port = process.env.PORT || 1337;
        http.createServer(function(req, res) {
          res.writeHead(200, { 'Content-Type': 'text/plain' });
          res.end('Hello World\n');
        }).listen(port);
4. Otwórz okno wiersza polecenia i uruchom aplikację sieci Web lokalnie przy użyciu poniższego polecenia.
   
        node server.js
5. Otwórz przeglądarkę sieci Web i przejdź do http://localhost:1337. 
   
    Zostanie wyświetlona strona sieci Web z ciągiem „Hello World” pokazana na poniższym zrzucie ekranu.
   
    ![Przeglądarka wyświetlająca ciąg „Hello World”.][helloworld-localhost]

## <a name="publish-your-application"></a>Publikowanie aplikacji
1. Zainstaluj system Git, jeśli jeszcze nie został zainstalowana.
   
    Aby uzyskać instrukcje dotyczące instalacji dla określonej platformy, przejdź na [stronę pobierania systemu Git](http://git-scm.com/download).
2. W wierszu polecenia przejdź do katalogu **helloworld**, a następnie wprowadź poniższe polecenie w celu zainicjowania lokalnego repozytorium Git.
   
        git init
3. Dodaj pliki do repozytorium przy użyciu następujących poleceń:
   
        git add .
        git commit -m "initial commit"
4. Dodaj zdalny element systemu Git do wypychania aktualizacji do utworzonej aplikacji sieci Web przy użyciu następującego polecenia:
   
        git remote add azure [URL for remote repository]
5. Wypchnij wprowadzone zmiany na platformę Azure przy użyciu następującego polecenia:
   
        git push azure master
   
    Zostanie wyświetlony monit o podanie utworzonego wcześniej hasła. Dane wyjściowe będą podobne do poniższego przykładu.
   
        Counting objects: 3, done.
        Delta compression using up to 8 threads.
        Compressing objects: 100% (2/2), done.
        Writing objects: 100% (3/3), 374 bytes, done.
        Total 3 (delta 0), reused 0 (delta 0)
        remote: New deployment received.
        remote: Updating branch 'master'.
        remote: Preparing deployment for commit id '5ebbe250c9'.
        remote: Preparing files for deployment.
        remote: Deploying Web.config to enable Node.js activation.
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
         * [new branch]      master -> master
6. Aby wyświetlić aplikację, kliknij przycisk **Przeglądaj** w części **Aplikacja sieci Web** w Portalu Azure.
   
    ![Przycisk Przeglądaj](./media/web-sites-nodejs-develop-deploy-mac/browsebutton.png)
   
    ![Aplikacja Hello World na platformie Azure](./media/web-sites-nodejs-develop-deploy-mac/helloworldazure.png)

## <a name="publish-changes-to-your-application"></a>Publikowanie zmian w aplikacji
1. Otwórz plik **server.js** w edytorze tekstów, a następnie zmień ciąg „Hello World” na „Hello Azure”. 
2. Zapisz plik.
3. W wierszu polecenia przejdź do katalogu **helloworld**, a następnie uruchom poniższe polecenia:
   
        git add .
        git commit -m "changing to hello azure"
        git push azure master
   
    Zostanie ponownie wyświetlony monit o podanie hasła.
4. Odśwież okno przeglądarki, w którym jest wyświetlana strona wskazywana przez adres URL aplikacji sieci Web.
   
    ![Strona sieci Web z wyświetlonym ciągiem „Hello Azure”][helloworld-completed]

## <a name="roll-back-a-deployment"></a>Wycofywanie wdrożenia
W bloku **Aplikacja sieci Web** możesz kliknąć pozycje **Ustawienia > Ciągłe wdrażanie**, aby wyświetlić historię wdrożenia w bloku **Wdrożenia**. Jeśli musisz przywrócić wcześniejsze wdrożenie, możesz wybrać je, a następnie kliknąć pozycję **Wdróż ponownie** w bloku **Szczegóły wdrożenia**.

## <a name="next-steps"></a>Następne kroki
Aplikacja Node.js została wdrożona do aplikacji sieci Web w usłudze Azure App Service. Aby dowiedzieć się więcej na temat sposobu uruchamiania aplikacji Node.js przez usługę App Service, zobacz [Aplikacje Web Apps w usłudze Azure App Service: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx) i [Określanie wersji środowiska Node.js w aplikacji Azure](../nodejs-specify-node-version-azure-apps.md).

Środowisko Node.js zapewnia rozbudowany ekosystem modułów, które mogą być używane przez aplikacje. Aby dowiedzieć się, jak usługa Web Apps współpracuje z modułami, zobacz [Użycie modułów Node.js z aplikacjami platformy Azure](../nodejs-use-node-modules-azure-apps.md).

Jeśli wystąpią problemy z aplikacją po wdrożeniu jej na platformie Azure, zobacz [Debugowanie aplikacji Node.js w usłudze Azure App Service](web-sites-nodejs-debug.md), aby uzyskać informacje na temat diagnozowania problemu.

W tym artykule do utworzenia aplikacji sieci Web użyto Portalu Azure. Można również użyć [interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md) lub [programu Azure PowerShell](/powershell/azureps-cmdlets-docs) w celu wykonania tych samych operacji.

Informacje dotyczące sposobu tworzenia aplikacji Node.js na platformie Azure można znaleźć w [Centrum deweloperów Node.js](/develop/nodejs/).

[helloworld-completed]: ./media/web-sites-nodejs-develop-deploy-mac/helloazure.png
[helloworld-localhost]: ./media/web-sites-nodejs-develop-deploy-mac/helloworldlocal.png
[portal-quick-create]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website.png
[portal-quick-create2]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website2.png
[setup-git-publishing]: ./media/web-sites-nodejs-develop-deploy-mac/setup_git_publishing.png
[go-to-dashboard]: ./media/web-sites-nodejs-develop-deploy-mac/go_to_dashboard.png
[deployment-part]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-part.png
[deployment-credentials]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-credentials.png
[git-url]: ./media/web-sites-nodejs-develop-deploy-mac/git-url.png



<!--HONumber=Jan17_HO3-->


