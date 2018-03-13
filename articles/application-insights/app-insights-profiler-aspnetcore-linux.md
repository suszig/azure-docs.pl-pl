---
title: Profil aplikacji sieci web platformy ASP.NET core Azure Linux z Application Insights profilera | Dokumentacja firmy Microsoft
description: "Omówienie koncepcji i samouczek krok po kroku dotyczące sposobu włączania go"
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: mbullwin
ms.openlocfilehash: 7f3a0a1a22e14f12b86474dd5b1985029074444e
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="profile-aspnet-core-azure-linux-web-apps-with-application-insights-profiler"></a>Profil platformy ASP.NET Core aplikacji sieci Web dla systemu Linux platformy Azure za pomocą Application Insights profilera

Ta funkcja jest obecnie w wersji zapoznawczej

Sprawdzić, ile jest czas w każdej metodzie aplikacji sieci web na żywo przy użyciu [usługi Application Insights](app-insights-overview.md). Profiler jest teraz dostępna dla aplikacji sieci web platformy ASP.NET core z hostowanej w systemie Linux na usługi aplikacji. Ten przewodnik zawiera instrukcje krok po kroku dotyczące sposobu można zbierać ślady profilera dla aplikacji sieci web programu ASP.NET core Linux.

Po ukończeniu tego przewodnika, aplikacja będzie zbierać ślady profilera, podobnie jak na poniższym zrzucie ekranu. W tym przykładzie śledzenia profilera wskazuje, że żądania sieci web określonego przebiega powoli, ponieważ większość czas na oczekiwania. Ikona zabezpieczenia umieszczona ścieżkę aktywną w kodzie spowolnieniu aplikacji. W tym przykładzie pokazano `About` metody w `HomeController` spowolnieniu aplikacji sieci web, ponieważ został on wywoływania `Thread.Sleep`.

![Ślady profilera](./media/app-insights-profiler-aspnetcore-linux/profiler-traces.png)

## <a name="pre-requisites"></a>Wymagania wstępne
Poniższe instrukcje stosowane do wszystkich środowisk deweloperskich systemu Windows, Linux i komputerów Mac:

* Zainstaluj [.NET core SDK 2.1.2 lub nowszy](https://www.microsoft.com/net/download/windows/build)
* Zainstaluj system Git, postępując zgodnie z instrukcjami w [wprowadzenie — instalowanie Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## <a name="setup-project-locally"></a>Instalacja projektu lokalnie

1. Otwórz wiersz polecenia na komputerze. Poniższe instrukcje działa dla wszystkich środowisk deweloperskich systemu Windows, Linux i komputerów Mac.

2. Utwórz platformy ASP.NET core aplikacji sieci web MVC
    ```
    dotnet new mvc -n LinuxProfilerTest
    ```
3. Zmień katalog, w wierszu polecenia w folderze głównym projektu

4. Dodaj pakiet nuget służący do zbierania ślady profilera.
    ```
    dotnet add package Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```
5. Dodaj wiersz kodu, aby losowo kilka sekund HomeController.cs

    ```csharp
        using System.Threading;
        ...

        public IActionResult About()
            {
                Random r = new Random();
                int delay = r.Next(5000, 10000);
                Thread.Sleep(delay);
                return View();
            }
    ```
6. Zapisz i zatwierdź zmiany w lokalnym repozytorium

    ```
        git init
        git add .
        git commit -m "first commit"
    ```

## <a name="create-azure-app-service-for-hosting-your-project"></a>Tworzenie usługi aplikacji Azure do hostowania projektu
1. Utwórz środowisko usługi aplikacji w systemie Linux

    ![Tworzenie usługi aplikacji w systemie Linux](./media/app-insights-profiler-aspnetcore-linux/create-linux-appservice.png)

2. Utwórz poświadczenia wdrażania. Zanotuj swoje hasło, ponieważ będzie on potrzebny później podczas wdrażania aplikacji.

    ![Utwórz poświadczenia wdrażania](./media/app-insights-profiler-aspnetcore-linux/create-deployment-credentials.png)

3. Wybierz opcję wdrożenia. Konfigurowanie lokalnego repozytorium Git w aplikacji sieci web, zgodnie z instrukcjami w portalu Azure. Repozytorium Git zostaną utworzone automatycznie.

    ![Instalator repozytorium Git](./media/app-insights-profiler-aspnetcore-linux/setup-git-repo.png)

Dostępne są dodatkowe opcje wdrażania [tutaj](https://docs.microsoft.com/azure/app-service/containers/choose-deployment-type)

## <a name="deploy-your-project"></a>Wdrażanie projektu

1. Z wiersza polecenia przejdź do folderu głównego projektu. Dodaj zdalnego repozytorium Git, aby wskazywał na usługi aplikacji:

    ```
    git remote add azure https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
    ```
    * Użyj 'username' z kroku "Utwórz poświadczenia wdrażania".
    * Użyj "Nazwa aplikacji" z kroku "Tworzenie usługi app service".

2. Wdrażanie projektu przez naciśnięcie zmiany na platformie Azure

    ```
    git push azure master
    ```
Zostaną wyświetlone informacje podobne do następujących:

    ```
    Counting objects: 9, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (8/8), done.
    Writing objects: 100% (9/9), 1.78 KiB | 911.00 KiB/s, done.
    Total 9 (delta 3), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id 'd7369a99d7'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling ASP.NET Core Web Application deployment.
    remote: ......
    remote:   Restoring packages for /home/site/repository/EventPipeExampleLinux.csproj...
    remote: .
    remote:   Installing Newtonsoft.Json 10.0.3.
    remote:   Installing Microsoft.ApplicationInsights.Profiler.Core 1.1.0-LKG
    …

    ```

## <a name="add-application-insights-to-monitor-your-web-apps"></a>Dodawanie usługi Application Insights do monitorowania aplikacji sieci web
1. [Tworzenie zasobu usługi Application Insights](./app-insights-create-new-resource.md)
2. Skopiuj iKey zasobu usługi Application Insights i skonfigurować następujące ustawienia w usługach aplikacji

    ```
    APPINSIGHTS_INSTRUMENTATIONKEY: [YOUR_APPINSIGHTS_KEY]
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES: Microsoft.ApplicationInsights.Profiler.AspNetCore
    ```

    ![Ustawienia aplikacji](./media/app-insights-profiler-aspnetcore-linux/set-appsettings.png)

    Zmienianie ustawień aplikacji zostanie automatycznie uruchomiony ponownie lokacji. Gdy są używane nowe ustawienia, profilera rozpocznie działanie natychmiast 2 minut. następnie zostanie on uruchomiony przez 2 minuty co godzinę.

3. Generowanie niektórych ruch do witryny sieci Web. Można odświeżać lokacji ```About``` dla strony kilka razy.

4. Poczekaj 2 do 5 minut, więc zdarzenia może być agregowany do usługi Application Insights.

5. Przejdź do okienka wydajności usługi Application Insights w portalu Azure. Zostanie wyświetlone ślady profilera dostępne w prawym dolnym rogu.

    ![Wyświetl dane śledzenia](./media/app-insights-profiler-aspnetcore-linux/view-traces.png)

## <a name="report-issues-to-project-github-repository"></a>Raport problemów z repozytorium Github projektu
Jeśli masz problemy lub sugestie, zgłoś do repozytorium github: [ApplicationInsights-profilera-AspNetCore: problemów](https://github.com/Microsoft/ApplicationInsights-Profiler-AspNetCore/issues)
