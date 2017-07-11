---
title: Tworzenie aplikacji internetowej ASP.NET na platformie Azure | Microsoft Docs
description: "Dowiedz się, jak uruchamiać aplikacje internetowe w usłudze Azure App Service, wdrażając przykładową aplikację internetową ASP.NET."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 2b447bcc5930550af3996cb40925ab59d203dc7c
ms.contentlocale: pl-pl
ms.lasthandoff: 06/20/2017

---
<a id="create-an-aspnet-web-app-in-azure" class="xliff"></a>

# Tworzenie aplikacji sieci Web platformy ASP.NET na platformie Azure

Usługa [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) oferuje wysoce skalowalną i samonaprawialną usługę hostowaną w Internecie.  Ten przewodnik Szybki start zawiera informacje dotyczące sposobu wdrożenia pierwszej aplikacji internetowej ASP.NET w usłudze Azure App Service. Po zakończeniu będzie istnieć grupa zasobów składająca się z planu usługi App Service i aplikacji internetowej platformy Azure wraz z wdrożoną aplikacją internetową.

![Aplikacja sieci Web platformy ASP.NET w usłudze Azure App Service](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne

W celu ukończenia tego samouczka:

* Zainstaluj program [Visual Studio 2017](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) z następującymi pakietami roboczymi:
    - **Tworzenie aplikacji na platformie ASP.NET i tworzenie aplikacji internetowych**
    - **Tworzenie aplikacji na platformie Azure**

    ![Tworzenie aplikacji na platformie ASP.NET i tworzenie aplikacji internetowych oraz tworzenie aplikacji na platformie Azure (w ramach Internetu i chmury)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-an-aspnet-web-app" class="xliff"></a>

## Tworzenie aplikacji sieci Web platformy ASP.NET

W programie Visual Studio utwórz nowy projekt, wybierając pozycję **Plik > Nowy > Projekt**. 

W oknie dialogowym **Nowy projekt** kliknij pozycję **Visual C# > Sieć Web > Aplikacja sieci Web ASP.NET (program .NET Framework)**.

Nadaj aplikacji nazwę _myFirstAzureWebApp_, a następnie kliknij przycisk **OK**.
   
![Okno dialogowe Nowy projekt](./media/app-service-web-get-started-dotnet/new-project.png)

Na platformie Azure można wdrożyć dowolny typ aplikacji sieci Web platformy ASP.NET. Do celów tego przewodnika Szybki start wybierz szablon **MVC** i upewnij się, że uwierzytelnianie jest ustawione na wartość **Bez uwierzytelniania**.
      
Kliknij przycisk **OK**.

![Okno dialogowe Nowy projekt ASP.NET](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

Z menu wybierz pozycję **Debuguj > Uruchom bez debugowania**, aby lokalnie uruchomić aplikację internetową.

![Uruchamianie aplikacji lokalnie](./media/app-service-web-get-started-dotnet/local-web-app.png)

<a id="publish-to-azure" class="xliff"></a>

## Publikowanie na platformie Azure

W obszarze **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy projekt **myFirstAzureWebApp**, a następnie wybierz polecenie **Publikuj**.

![Publikowanie z Eksploratora rozwiązań](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

Upewnij się, że pozycja **Microsoft Azure App Service** jest zaznaczona, a następnie kliknij przycisk **Publikuj**.

![Publikowanie ze strony przeglądu projektu](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Spowoduje to otwarcie okna dialogowego **Tworzenie usługi App Service**, które ułatwia tworzenie wszystkich zasobów platformy Azure niezbędnych do uruchomienia aplikacji internetowej ASP.NET na platformie Azure.

<a id="sign-in-to-azure" class="xliff"></a>

## Logowanie do platformy Azure

W oknie dialogowym **Tworzenie usługi App Service** wybierz pozycję **Dodaj konto**, a następnie zaloguj się do swojej subskrypcji platformy Azure. Jeśli przeprowadzono już logowanie, upewnij się, że konto zawiera Twoją subskrypcję platformy Azure. Możesz wybrać konto logowania, aby dodać odpowiednie konto.

> [!NOTE]
> Jeśli przeprowadzono już logowanie, nie wybieraj jeszcze pozycji **Utwórz**.
>
>
   
![Logowanie do platformy Azure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

Po zalogowaniu się możesz w tym oknie dialogowym utworzyć wszystkie zasoby wymagane dla aplikacji internetowej platformy Azure.

<a id="create-a-resource-group" class="xliff"></a>

## Tworzenie grupy zasobów

[!INCLUDE [resource group intro text](../../includes/resource-group.md)]

Obok pozycji **Grupa zasobów** wybierz pozycję **Nowa**.

Nadaj grupie zasobów nazwę **myResourceGroup**, a następnie kliknij przycisk **OK**.

<a id="create-an-app-service-plan" class="xliff"></a>

## Tworzenie planu usługi App Service

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Obok pozycji **Plan usługi App Service** wybierz pozycję **Nowy**. 

W oknie dialogowym **Konfiguruj plan usługi App Service** użyj ustawień znajdujących się w tabeli poniżej zrzutu ekranu.

![Tworzenie planu usługi App Service](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

| Ustawienie | Sugerowana wartość | Opis |
|-|-|-|
|Plan usługi App Service| myAppServicePlan | Nazwa planu usługi App Service. |
| Lokalizacja | Europa Zachodnia | Centrum danych, w którym hostowana jest aplikacja internetowa. |
| Rozmiar | Bezpłatna | [Warstwa cenowa](https://azure.microsoft.com/pricing/details/app-service/) określa funkcje hostowania. |

Kliknij przycisk **OK**.

<a id="create-and-publish-the-web-app" class="xliff"></a>

## Tworzenie i publikowanie aplikacji sieci Web

W polu **Nazwa aplikacji sieci Web** wpisz unikatową nazwę aplikacji (prawidłowe znaki to `a-z`, `0-9` i `-`). Adres URL aplikacji internetowej to `http://<app_name>.azurewebsites.net`, gdzie `<app_name>` to nazwa aplikacji internetowej. 

Możesz zaakceptować automatycznie wygenerowaną nazwę, która jest unikatowa.

Wybierz pozycję **Utwórz**, aby rozpocząć tworzenie zasobów platformy Azure.

![Konfigurowanie nazwy aplikacji sieci Web](./media/app-service-web-get-started-dotnet/web-app-name.png)

Po zakończeniu działania kreatora aplikacja internetowa ASP.NET zostanie opublikowana na platformie Azure, a następnie uruchomiona w domyślnej przeglądarce.

![Opublikowana aplikacja sieci Web platformy ASP.NET na platformie Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

Adres URL używa określonej wcześniej nazwy aplikacji sieci Web w formacie `http://<app_name>.azurewebsites.net`. 

Gratulacje, Twoja aplikacja internetowa ASP.NET działa w usłudze Azure App Service.

<a id="update-the-app-and-redeploy" class="xliff"></a>

## Aktualizowanie aplikacji i ponowne wdrażanie

Za pomocą **Eksploratora rozwiązań** otwórz plik _Views\Home\Index.cshtml_.

Znajdź tag HTML `<div class="jumbotron">` u góry i zastąp cały element następującym kodem:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Aby przeprowadzić ponowne wdrożenie na platformie Azure, kliknij prawym przyciskiem myszy projekt **myFirstAzureWebApp** w **Eksploratorze rozwiązań**, a następnie wybierz polecenie **Publikuj**.

Na stronie publikowania wybierz pozycję **Publikuj**.

Po zakończeniu publikowania program Visual Studio otworzy w przeglądarce adres URL aplikacji internetowej.

![Zaktualizowana aplikacja sieci Web platformy ASP.NET na platformie Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

<a id="manage-the-azure-web-app" class="xliff"></a>

## Zarządzanie aplikacją internetową platformy Azure

Przejdź do witryny <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, aby zarządzać aplikacją internetową.

W lewym menu wybierz pozycję **App Services**, a następnie wybierz nazwę swojej aplikacji sieci Web platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/app-service-web-get-started-dotnet/access-portal.png)

Zostanie wyświetlona strona Omówienie aplikacji internetowej. Tutaj możesz wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie. 

![Blok usługi App Service w witrynie Azure Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

Menu po lewej stronie zawiera różne strony służące do konfigurowania aplikacji. 

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

<a id="next-steps" class="xliff"></a>

## Następne kroki

> [!div class="nextstepaction"]
> [Platforma .ASP.NET z usługą SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)

