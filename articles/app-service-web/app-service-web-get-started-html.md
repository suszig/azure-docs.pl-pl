---
title: "Tworzenie pierwszej statycznej aplikacji sieci Web w języku HTML na platformie Azure w ciągu pięciu minut | Microsoft Docs"
description: "Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając przykładową aplikację."
services: app-service\web
documentationcenter: 
author: rick-anderson
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/08/2017
ms.author: riande
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 895906e1ab4bc50093ed3b18f043c3dd515ca054
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017


---
# <a name="create-a-static-html-web-app-in-azure-in-five-minutes"></a>Tworzenie pierwszej statycznej aplikacji sieci Web w języku HTML na platformie Azure w ciągu pięciu minut

Ten samouczek Szybki start przedstawia sposób tworzenia i wdrażania podstawowej witryny HTML+CSS na platformie Azure. Uruchomisz aplikację za pomocą [planu usługi Azure App Service](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) oraz utworzysz w niej aplikację sieci Web przy użyciu [interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli). Do wdrożenia aplikacji na platformie Azure użyj usługi Git. Po zainstalowaniu wymagań wstępnych zakończenie tego samouczka trwa około pięciu minut.

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

## <a name="prerequisites"></a>Wymagania wstępne

Przed utworzeniem tego przykładu pobierz i zainstaluj następujące składniki:

- [Git](https://git-scm.com/)
- [Interfejs wiersza polecenia platformy Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

W oknie terminalu sklonuj repozytorium przykładowej aplikacji na komputer lokalny:

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

## <a name="view-the-html"></a>Wyświetlanie kodu HTML

Przejdź do katalogu, który zawiera przykładowy kod HTML. Otwórz plik *index.html* w swojej przeglądarce.

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [login-to-azure](../../includes/login-to-azure.md)] 
[!INCLUDE [configure-deployment-user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [app-service-web-quickstart1](../../includes/app-service-web-quickstart1.md)] 

Utwórz [aplikację sieci Web](app-service-web-overview.md) w planie usługi App Service `quickStartPlan`. Aplikacja sieci Web udostępnia miejsce na hosting Twojego kodu i zawiera adres URL do wyświetlania wdrożonej aplikacji.

[!INCLUDE [app-service-web-quickstart2](../../includes/app-service-web-quickstart2.md)] 

Strona działa jako aplikacja sieci Web usługi Azure App Service:

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

## <a name="update-and-redeploy-the-app"></a>Aktualizowanie i ponowne wdrażanie aplikacji

Otwórz plik *index.html*. Wprowadź zmiany w znacznikach. Na przykład zmień `Hello world!` na `Hello Azure!`

Zatwierdź zmiany w narzędziu Git, a następnie wypchnij zmiany kodu na platformę Azure.

```bash
git commit -am "updated HTML"
git push azure master
```

Po zakończeniu wdrożenia odśwież przeglądarkę, aby zobaczyć zmiany.

[!INCLUDE [manage-azure-web-app](../../includes/manage-azure-web-app.md)]


[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Następne kroki

- Zapoznaj się z przykładowymi [skryptami interfejsu wiersza polecenia w usługach Web Apps](app-service-cli-samples.md).
- Dowiedz się, jak [zmapować niestandardową nazwę domeny](app-service-web-tutorial-custom-domain.md), taką jak contoso.com, na [aplikację usługi App Service](app-service-web-tutorial-custom-domain.md).
