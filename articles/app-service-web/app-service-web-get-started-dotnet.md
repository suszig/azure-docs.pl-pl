---
title: "Tworzenie pierwszej aplikacji sieci Web platformy ASP.NET na platformie Azure w ciągu pięciu minut | Microsoft Docs"
description: "Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając prostą aplikację platformy ASP.NET."
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
ms.date: 03/27/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 26d460a699e31f6c19e3b282fa589ed07ce4a068
ms.openlocfilehash: b575e92d2140ecc0e0f30186302e7733e08d0415
ms.lasthandoff: 04/04/2017


---
# <a name="create-your-first-aspnet-web-app-in-azure-in-five-minutes"></a>Tworzenie pierwszej aplikacji sieci Web platformy ASP.NET na platformie Azure w ciągu pięciu minut

[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Ten przewodnik szybkiego startu pomaga w ciągu kilku minut wdrożyć pierwszą aplikację sieci Web platformy ASP.NET w [usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md). Po zakończeniu będziesz mieć prostą aplikację sieci Web uruchomioną w chmurze.

![Aplikacja sieci Web platformy ASP.NET w usłudze Azure App Service](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="before-you-begin"></a>Przed rozpoczęciem

Ten samouczek pokazuje sposób używania programu Visual Studio 2017 do tworzenia i wdrażania aplikacji sieci Web platformy ASP.NET na platformie Azure. Jeśli nie masz jeszcze zainstalowanego programu Visual Studio 2017, możesz pobrać program [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) i używać go **bezpłatnie**. Podczas instalacji programu Visual Studio upewnij się, że jest włączona opcja **Programowanie na platformie Azure**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-aspnet-web-app"></a>Tworzenie aplikacji sieci Web platformy ASP.NET

W programie Visual Studio utwórz nowy projekt za pomocą kombinacji `Ctrl` + `Shift` + `N`.

W oknie dialogowym **Nowy projekt** kliknij pozycje **Visual C# > Sieć Web > Aplikacja sieci Web ASP.NET (program .NET Framework)**.

Podaj nazwę aplikacji **myFirstAzureWebApp**, a następnie kliknij przycisk **OK**.
   
![Okno dialogowe Nowy projekt](./media/app-service-web-get-started-dotnet/new-project.png)

Na platformie Azure można wdrożyć dowolny typ aplikacji sieci Web platformy ASP.NET. Do celów tego samouczka wybierz szablon **MVC** i upewnij się, że uwierzytelnianie jest ustawione na **Bez uwierzytelniania**.
      
Kliknij przycisk **OK**.

![Okno dialogowe Nowy projekt ASP.NET](./media/app-service-web-get-started-dotnet/select-mvc-template.png)

## <a name="publish-to-azure"></a>Publikowanie na platformie Azure

W obszarze **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy projekt **myFirstAzureWebApp** i wybierz polecenie **Publikuj**.

![Publikowanie z Eksploratora rozwiązań](./media/app-service-web-get-started-dotnet/solution-explorer-publish.png)

Upewnij się, że jest zaznaczona usługa **Microsoft Azure App Service**, a następnie kliknij przycisk **Publikuj**.

![Publikowanie ze strony przeglądu projektu](./media/app-service-web-get-started-dotnet/publish-to-app-service.png)

Spowoduje to otwarcie okna dialogowego **Tworzenie usługi App Service**, które ułatwia tworzenie wszystkich zasobów platformy Azure potrzebnych do uruchomienia aplikacji sieci Web platformy ASP.NET na platformie Azure.

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

W oknie dialogowym **Tworzenie usługi App Service** kliknij pozycję **Dodaj konto**, a następnie zaloguj się do swojej subskrypcji platformy Azure. Jeśli zalogowano się już do konta Microsoft, upewnij się, że to konto zawiera Twoją subskrypcję platformy Azure. Jeśli użyte do logowania konto Microsoft nie ma subskrypcji platformy Azure, kliknij je, aby dodać prawidłowe konto.
   
![Logowanie do platformy Azure](./media/app-service-web-get-started-dotnet/sign-in-azure.png)

Po zalogowaniu możesz w tym oknie dialogowym utworzyć wszystkie zasoby potrzebne dla aplikacji sieci Web platformy Azure.

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Najpierw potrzebna będzie _grupa zasobów_. 

> [!NOTE] 
> Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure, takich jak aplikacje sieci Web, bazy danych i konta magazynu, oraz zarządzania nimi.
>
>

Obok pozycji **Grupa zasobów** kliknij przycisk **Nowa**.

Nadaj grupie zasobów nazwę **myResourceGroup**, a następnie kliknij przycisk **OK**.

## <a name="create-an-app-service-plan"></a>Tworzenie planu usługi App Service

Aplikacja sieci Web platformy Azure wymaga też _planu usługi App Service_. 

> [!NOTE]
> Plan usługi App Service reprezentuje kolekcję zasobów fizycznych służących do hostowania aplikacji. Wszystkie aplikacje przypisane do planu usługi App Service współdzielą zasoby przez niego zdefiniowane, co pozwala zmniejszyć koszty hostowania wielu aplikacji. 
>
> Plany usługi App Service definiują następujące elementy:
>
> - Region (Europa Północna, Wschodnie stany USA, Azja Południowo-Wschodnia)
> - Rozmiar wystąpienia (mały, średni, duży)
> - Wartość licznika skali (jedno, dwa lub trzy wystąpienia itp.) 
> - Jednostka magazynowa (Bezpłatna, Współdzielona, Podstawowa, Standardowa, Premium)
>
>

Obok pozycji **Plan usługi App Service** kliknij przycisk **Nowy**. 

W oknie dialogowym **Konfiguruj plan usługi App Service** skonfiguruj nowy plan usługi App Service przy użyciu następujących ustawień:

- **Plan usługi App Service**: wpisz wartość **myAppServicePlan**. 
- **Lokalizacja**: wybierz pozycję **Europa Zachodnia** lub inny odpowiedni region.
- **Rozmiar**: wybierz **Bezpłatna** lub inną odpowiednią [warstwę cenową](https://azure.microsoft.com/pricing/details/app-service/).

Kliknij przycisk **OK**.

![Tworzenie nowego planu usługi App Service](./media/app-service-web-get-started-dotnet/configure-app-service-plan.png)

## <a name="create-and-publish-the-web-app"></a>Tworzenie i publikowanie aplikacji sieci Web

Pozostało jedynie nadanie nazwy aplikacji sieci Web. W polu **Nazwa aplikacji sieci Web** wpisz unikatową nazwę aplikacji. Ta nazwa będzie używana jako część domyślnej nazwy DNS dla aplikacji (`<app_name>.azurewebsites.net`), więc musi być unikatowa dla wszystkich aplikacji na platformie Azure. Później możesz zamapować niestandardową nazwę domeny na aplikację, zanim udostępnisz ją użytkownikom.

Możesz również zaakceptować automatycznie wygenerowaną nazwę, która już jest unikatowa.

Kliknij przycisk **Utwórz**, aby rozpocząć tworzenie zasobów platformy Azure.

![Konfigurowanie nazwy aplikacji sieci Web](./media/app-service-web-get-started-dotnet/web-app-name.png)

Po zakończeniu tworzenia zasobów platformy Azure przez kreator automatycznie opublikuje on po raz pierwszy aplikację sieci Web platformy ASP.NET na platformie Azure, a następnie uruchomi opublikowaną aplikację sieci Web platformy Azure w domyślnej przeglądarce.

![Opublikowana aplikacja sieci Web platformy ASP.NET na platformie Azure](./media/app-service-web-get-started-dotnet/published-azure-web-app.png)

Adres URL używa określonej wcześniej nazwy aplikacji sieci Web w formacie `http://<app_name>.azurewebsites.net`. 

Gratulacje, Twoja pierwsza aplikacja sieci Web platformy ASP.NET działa na żywo w usłudze Azure App Service.

## <a name="update-the-app-and-redeploy"></a>Aktualizowanie aplikacji i ponowne wdrażanie

Ponowne wdrożenie aktualizacji na platformie Azure jest bardzo proste. Wprowadźmy aktualizację na stronie głównej.

Za pomocą **Eksploratora rozwiązań** otwórz plik **Views\Home\Index.cshtml**.

Znajdź tag HTML `<div class="jumbotron">` u góry i zastąp cały tag następującym kodem:

```HTML
<div class="jumbotron">
    <h1>ASP.NET in Azure!</h1>
    <p class="lead">This is a simple app that we’ve built that demonstrates how to deploy a .NET app to Azure App Service.</p>
</div>
```

Aby przeprowadzić ponowne wdrożenie na platformie Azure, kliknij prawym przyciskiem myszy swój projekt **myFirstAzureWebApp** w **Eksploratorze rozwiązań**, a następnie wybierz pozycję **Publikuj**.

Na stronie publikowania kliknij przycisk **Publikuj**.

Po zakończeniu operacji program Visual Studio uruchomi zaktualizowaną aplikację sieci Web na platformie Azure w przeglądarce.

![Zaktualizowana aplikacja sieci Web platformy ASP.NET na platformie Azure](./media/app-service-web-get-started-dotnet/updated-azure-web-app.png)

## <a name="manage-your-new-azure-web-app"></a>Zarządzanie nową aplikacją sieci Web platformy Azure

Przejdź do witryny Azure Portal, aby przyjrzeć się właśnie utworzonej aplikacji sieci Web. 

W tym celu zaloguj się do witryny [https://portal.azure.com](https://portal.azure.com).

W lewym menu kliknij pozycję **App Service**, a następnie kliknij nazwę swojej aplikacji sieci Web platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/app-service-web-get-started-dotnet/access-portal.png)

Jesteś teraz w _bloku_ aplikacji sieci Web (stronie portalu, która jest otwierana w poziomie). 

Domyślnie blok aplikacji sieci Web wyświetla stronę **Przegląd**. Ta strona udostępnia widok sposobu działania aplikacji. Tutaj możesz również wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie. Karty po lewej stronie bloku pokazują różne strony konfiguracji, które można otworzyć. 

![Blok usługi App Service w witrynie Azure Portal](./media/app-service-web-get-started-dotnet/web-app-blade.png)

Te karty w bloku pokazują wiele doskonałych funkcji, które możesz dodać do aplikacji sieci Web. Poniższa lista zawiera tylko kilka możliwości:

- Mapowanie niestandardowej nazwy DNS
- Tworzenie powiązania niestandardowego certyfikatu SSL
- Konfigurowanie ciągłego wdrażania
- Skalowanie w górę i w dół
- Dodawanie uwierzytelniania użytkownika

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Aby usunąć swoją pierwszą aplikację sieci Web platformy Azure, możesz kliknąć przycisk **Usuń** na stronie **Przegląd**. Jest jednak lepszy sposób, aby usunąć wszystkie elementy utworzone w ramach tego samouczka szybkiego startu. Na stronie **Przegląd** aplikacji sieci Web kliknij grupę zasobów, aby otworzyć jej blok. 

![Dostęp do grupy zasobów z bloku usługi App Service](./media/app-service-web-get-started-dotnet/access-resource-group.png)

W bloku grupy zasobów można zobaczyć zarówno plan usługi App Service, jak i aplikację usługi App Service, która została utworzona przez program Visual Studio. 

W górnej części bloku kliknij pozycję **Usuń**. 

<!--![Delete resource group in Azure portal](./media/app-service-web-get-started-dotnet/delete-resource-group.png)-->

W bloku potwierdzenia potwierdź operację, wpisując nazwę grupy zasobów **myResourceGroup** w polu tekstowym i klikając przycisk **Usuń**.

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z wstępnie utworzonymi [skryptami programu PowerShell aplikacji sieci Web](app-service-powershell-samples.md).

