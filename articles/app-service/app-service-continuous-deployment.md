---
title: "Ciągłe wdrażanie w usłudze Azure App Service | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak włączyć ciągłe wdrażanie w usłudze Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: dariagrigoriu
ms.openlocfilehash: ef5924607868bcb3dc35e279539b78d5a0e17c1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="continuous-deployment-to-azure-app-service"></a>Ciągłe wdrażanie w usłudze Azure App Service
Ten samouczek pokazuje, jak skonfigurować ciągłe wdrażanie przepływ pracy dla Twojego [Azure Web Apps](app-service-web-overview.md). Usługi aplikacji integracji z BitBucket, GitHub, i [programu Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) umożliwia ciągłe wdrażanie przepływu pracy, gdzie Azure ściąga najnowsze aktualizacje z projektu opublikować na jednym z tych usług. Ciągłe wdrażanie to doskonałe rozwiązanie dla projektów, w których ma miejsce częste współtworzenie wielu treści.

Aby dowiedzieć się, jak skonfigurować ciągłe wdrażanie ręcznie z repozytorium chmury nie są wyświetlane w portalu Azure (takich jak [GitLab](https://gitlab.com/)), zobacz [Konfigurowanie ciągłego wdrażania przy użyciu ręczne](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="overview"></a>Włącz ciągłe wdrażanie
Aby włączyć ciągłe wdrażanie,

1. opublikuj zawartość swojej aplikacji w repozytorium, które będzie używane do ciągłego wdrażania.  
    Aby uzyskać więcej informacji dotyczących publikowania projektu w tych usługach, zobacz [Tworzenie repozytorium (GitHub)], [Tworzenie repozytorium (BitBucket)], i [Wprowadzenie do usług VSTS].
2. W bloku menu swojej aplikacji w [Azure Portal] kliknij pozycję **WDRAŻANIE APLIKACJI > Opcje wdrażania**. Kliknij pozycję **Wybierz źródło**, a następnie wybierz źródło wdrożenia.  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > Aby skonfigurować konto usług VSTS dla wdrożenia usługi App Service, zobacz ten [samouczek](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
   > 
   > 
3. Ukończ przepływ pracy autoryzacji.
4. W bloku **Źródło wdrożenia** wybierz projekt i gałąź, z której ma być wdrożony. Gdy skończysz, kliknij przycisk **OK**.
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > Podczas włączania ciągłego wdrażania za pomocą usługi GitHub lub BitBucket zostaną wyświetlone zarówno projekty publiczne, jak i prywatne.
   > 
   > 
   
    Usługa App Service tworzy skojarzenie z wybranym repozytorium, pobiera pliki z określonej gałęzi i utrzymuje klon repozytorium dla aplikacji usługi App Service. Po skonfigurowaniu ciągłego wdrażania usług VSTS z witryny Azure Portal funkcja integracji używa [aparatu wdrażania programu Kudu](https://github.com/projectkudu/kudu/wiki) usługi App Service, który już automatyzuje zadania kompilowania i wdrażania za pomocą każdego polecenia `git push`. Nie musisz oddzielnie konfigurować ciągłego wdrażanie w usługach VSTS. Po zakończeniu tego procesu blok aplikacji **Opcje wdrażania** pokaże aktywne wdrożenie, co oznacza powodzenie wdrożenia.
5. Aby sprawdzić, czy aplikacja została pomyślnie wdrożona, kliknij pozycję **Adres URL** u góry bloku aplikacji w witrynie Azure Portal.
6. Aby sprawdzić, czy ciągłe wdrażanie następuje z wybranego repozytorium, wypchnij zmianę do repozytorium. Aplikacja powinna zostać zaktualizowana zgodnie ze zmianami krótko po zakończeniu wypychania do repozytorium. Możesz sprawdzić, czy pobrała ona aktualizację w bloku **Opcje wdrażania** aplikacji.

## <a name="VSsolution"></a>Ciągłe wdrażanie za pomocą rozwiązania Visual Studio
Wypychanie rozwiązania Visual Studio do usługi Azure App Service jest tak proste jak wypchnięcie prostego pliku index.html. Proces wdrażania usługi App Service usprawnia wszystkie szczegóły, w tym przywracanie zależności NuGet i kompilowanie plików binarnych aplikacji. Możesz postępować zgodnie z najlepszymi praktykami kontroli źródła dla utrzymania kodu tylko w repozytorium Git i pozwolić usłudze App Service zająć się resztą.

Kroki wypychania rozwiązania Visual Studio do usługi App Service są takie same jak w [poprzedniej sekcji](#overview), przy założeniu, że rozwiązanie i repozytorium zostały skonfigurowane w następujący sposób:

* Użyj opcji kontroli źródła programu Visual Studio, aby wygenerować plik `.gitignore` taki jak poniższy obraz, lub ręcznie dodaj plik `.gitignore` w głównym repozytorium o zawartości podobnej do tego [przykładu pliku gitignore](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* Dodaj drzewo katalogów całego rozwiązania do repozytorium, przy czym plik sln powinien się znaleźć w katalogu głównym repozytorium.

Po skonfigurowaniu repozytorium zgodnie z opisem i skonfigurowaniu aplikacji na platformie Azure do ciągłego publikowania z jednego z repozytoriów online Git możesz lokalnie utworzyć aplikację platformy ASP.NET w programie Visual Studio i ciągle wdrażać kod, po prostu wypychając zmiany do repozytorium online Git.

## <a name="disableCD"></a>Wyłączanie ciągłego wdrażania
Aby wyłączyć ciągłe wdrażanie

1. W bloku menu swojej aplikacji w [Azure Portal] kliknij pozycję **WDRAŻANIE APLIKACJI > Opcje wdrażania**. Następnie kliknij pozycję **Rozłącz** w bloku **Opcje wdrażania**.
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. Po udzieleniu odpowiedzi **Tak** na komunikat potwierdzenia możesz powrócić do bloku aplikacji i kliknąć pozycję **WDRAŻANIE APLIKACJI > Opcje wdrażania**, jeśli chcesz skonfigurować publikowanie z innego źródła.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Jak badać typowe problemy z ciągłym wdrażaniem](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Jak korzystać z programu PowerShell dla platformy Azure]
* [Jak używać narzędzi wiersza polecenia platformy Azure dla systemów Mac i Linux]
* [Dokumentacja usługi Git]
* [Projekt Kudu](https://github.com/projectkudu/kudu/wiki)
* [Umożliwia automatyczne generowanie potoku CI/CD, aby wdrożyć aplikację ASP.NET 4 Azure](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Try App Service](https://azure.microsoft.com/try/app-service/) (Wypróbuj usługę App Service), w którym wyjaśniono, jak od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service. Bez kart kredytowych i bez zobowiązań.
> 
> 

[Azure Portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Jak korzystać z programu PowerShell dla platformy Azure]: /powershell/azureps-cmdlets-docs
[Jak używać narzędzi wiersza polecenia platformy Azure dla systemów Mac i Linux]:../cli-install-nodejs.md
[Dokumentacja usługi Git]: http://git-scm.com/documentation

[Tworzenie repozytorium (GitHub)]: https://help.github.com/articles/create-a-repo
[Tworzenie repozytorium (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Wprowadzenie do usług VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
