---
title: "Tworzenie niestandardowych łączniki z interfejsów API sieci Web - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Tworzenie niestandardowych łączniki z interfejsów API sieci Web"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 22bf335718721d29933557142b436e75778f8c86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-custom-connectors-from-web-apis"></a>Tworzenie niestandardowych łączniki z interfejsów API sieci Web

Aby utworzyć niestandardowy łącznik, który można użyć usługi Azure Logic Apps, Flow firmy Microsoft lub PowerApps firmy Microsoft, należy najpierw utworzyć interfejs API sieci Web, hosta o Azure Web Apps, uwierzytelniania za pomocą usługi Azure Active Directory i Zarejestruj się jako łącznika z usługą Logic Apps przepływu, lub Rozwiązanie PowerApps. W tym samouczku przedstawiono sposób wykonywania tych zadań tworzenie aplikacji interfejsu API sieci Web ASP.NET. Wzorce pokazujące, można tworzyć struktury kodu dla Twojego łącznika wyzwalacze i akcje, można znaleźć [Tworzenie niestandardowych interfejsów API, który można wywołać z przepływów pracy aplikacji logiki](../logic-apps/logic-apps-create-api-app.md).

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2013 lub nowszym](https://www.visualstudio.com/vs/). W tym samouczku korzysta z programu Visual Studio 2015.

* Kod dla interfejsu API sieci Web. Jeśli nie masz żadnych, spróbuj w tym samouczku: [wprowadzenie do korzystania z programu ASP.NET Web API 2 (C#)](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, można uruchomić z poziomu [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/). W przeciwnym razie zarejestrować się w celu [subskrypcji płatność za rzeczywiste użycie](https://azure.microsoft.com/pricing/purchase-options/).

## <a name="create-and-deploy-an-aspnet-web-app-to-azure"></a>Tworzenie i wdrażanie aplikacji sieci web platformy ASP.NET na platformie Azure

Ten samouczek tworzenia aplikacji sieci web programu Visual C# ASP.NET. 

1. Otwórz program Visual Studio, a następnie wybierz pozycję **pliku** > **nowy projekt**.

   1. Rozwiń węzeł **zainstalowana**, przejdź do **szablony** > **Visual C#** > **Web**i wybierz **ASP Aplikacja sieci web .NET**.

   2. Podaj nazwę projektu, lokalizację i nazwę rozwiązanie dla aplikacji, a następnie wybierz **OK**.

   Na przykład:

   ![Tworzenie aplikacji sieci web programu Visual C# ASP.NET](./media/custom-connector-build-web-api-app-tutorial/visual-studio-new-project-aspnet-web-app.png)

2. W **nowej aplikacji sieci Web ASP.NET** wybierz opcję **interfejsu API sieci Web** szablonu. Jeśli jeszcze nie jest wybrana, wybierz **Hostuj w chmurze**. Wybierz **Zmień uwierzytelnianie**.

   ![Wybierz szablon "Interfejsu API sieci Web", "Host w chmurze", "Zmienianie uwierzytelniania"](./media/custom-connector-build-web-api-app-tutorial/visual-studio-web-api-template.png)

3. Wybierz **bez uwierzytelniania**i wybierz polecenie **OK**. Można skonfigurować uwierzytelniania później.

   ![Wybierz opcję "Bez uwierzytelniania"](./media/custom-connector-build-web-api-app-tutorial/visual-studio-change-authentication.png)

4. Gdy **nowej aplikacji sieci Web ASP.NET** pojawi się, wybierz **OK**. 

5. W **Tworzenie usługi App Service** polu Przejrzyj hostingu ustawienia opisane w tabeli, wprowadzić zmiany i wybierz polecenie **Utwórz**. 

   [Planu usługi aplikacji](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) reprezentuje kolekcję zasobów fizycznych używany do hostowania aplikacji w Twojej subskrypcji platformy Azure. Dowiedz się więcej o [usługi aplikacji](../app-service/app-service-value-prop-what-is.md).

   ![Tworzenie usługi App Service](./media/custom-connector-build-web-api-app-tutorial/visual-studio-create-app-service.png)

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | Azure pracy lub konto służbowe lub osobiste konto Microsoft | *Twoje konto użytkownika* | Wybierz konto użytkownika. | 
   | **Nazwa aplikacji sieci Web** | *niestandardowe —-api Nazwa aplikacji sieci web* lub domyślna nazwa | Wprowadź nazwę dla aplikacji interfejsu API sieci Web, która jest używana w adresie URL aplikacji, na przykład: http://*-api Nazwa aplikacji sieci web*. | 
   | **Subskrypcja** | *Nazwa w przypadku subskrypcji platformy Azure* | Wybierz subskrypcję platformy Azure, której chcesz użyć. | 
   | **Grupa zasobów** | *Azure-resource grupy name* | Wybierz istniejącą grupę zasobów platformy Azure, lub jeśli nie jest jeszcze, Utwórz grupę zasobów. <p>**Uwaga**: grupy zasobów platformy Azure są uporządkowane zasobów platformy Azure w ramach subskrypcji platformy Azure. | 
   | **Plan usługi aplikacji** | *Aplikacja usługi planu nazwa-* | Wybierz istniejący plan usługi aplikacji lub jeśli nie jest jeszcze utworzenie planu. | 
   |||| 

   W przypadku utworzenia planu usługi App Service, określ następujące ustawienia:

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Lokalizacja** | *region wdrożenia* | Wybierz region do wdrażania aplikacji. | 
   | **Rozmiar** | *Usługi plan rozmiaru aplikacji* | Wybierz rozmiar plan określa kosztów i mocy obliczeniowej zasobów dla planu obsługi. | 
   |||| 

   Aby skonfigurować inne zasoby wymagane przez aplikację, wybierz **Eksploruj dodatkowych usług Azure**.

   | Ustawienie | Sugerowana wartość | Opis | 
   | ------- | --------------- | ----------- | 
   | **Typ zasobu** | *Azure-resource-type* | Wybierz i skonfiguruj dodatkowe zasoby wymagane przez aplikację. | 
   |||| 

6. Po Visual Studio wdroży projekt, Skompiluj kod aplikacji.

## <a name="create-an-openapi-swagger-file-that-describes-your-web-api"></a>Utwórz plik OpenAPI (Swagger), który opisuje interfejs API sieci Web

Aby połączyć aplikację interfejsu API sieci Web do aplikacji logiki, należy [OpenAPI (dawniej Swagger) pliku](http://swagger.io/) opisujący Twój interfejs API operacji. Można napisać własny OpenAPI definicji interfejsu API z [edytora online programu Swagger](http://editor.swagger.io/), ale w tym samouczku korzysta z narzędzi typu open source o nazwie [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle/blob/master/README.md).

1. Jeśli nie jest jeszcze zainstaluj pakiet Swashbuckle Nuget w projekcie programu Visual Studio.

   1. W programie Visual Studio, wybierz **narzędzia** > **Menedżera pakietów NuGet** > 
    **Konsola Menedżera pakietów**.

   2. W **Konsola Menedżera pakietów**, przejdź do katalogu projektu aplikacji, jeśli nie masz istnieje już (Uruchom `Set-Location "project-path"`), i uruchom ten aplet polecenia programu PowerShell: 
   
      `Install-Package Swashbuckle`

      Na przykład:

      ![Konsola Menedżera pakietów, zainstaluj pakiet Swashbuckle](./media/custom-connector-build-web-api-app-tutorial/visual-studio-package-manager-install-swashbuckle.png)

   > [!TIP]
   > Po uruchomieniu aplikacji po zainstalowaniu pakietu Swashbuckle Swashbuckle generuje plik OpenAPI pod tym adresem URL: 
   >
   > http://*{your-web-api-app-root-URL}*/swagger/docs/v1
   > 
   > Pakiet Swashbuckle generuje również interfejs użytkownika pod tym adresem URL: 
   > 
   > http://*{your-web-api-app-root-URL}*  /swagger

3. Gdy wszystko będzie gotowe, publikowanie aplikacji interfejsu API sieci Web na platformie Azure. Aby opublikować w programie Visual Studio, kliknij prawym przyciskiem myszy projekt sieci web w Eksploratorze rozwiązań, wybierz polecenie **publikowania...** i postępuj zgodnie z monitami.

   > [!IMPORTANT]
   > Zduplikowanych identyfikatorów operacji należy nieprawidłowy dokument OpenAPI. Jeśli używasz przykładowy szablon języka C#, szablon *powtarza identyfikator tej operacji dwukrotnie*:`Values_Get` 
   > 
   > Aby rozwiązać ten problem, zmień jedno wystąpienie do `Value_Get` i ponownie opublikować.

4. Pobierz dokument OpenAPI, przechodząc do tej lokalizacji: 

   http://*{your-web-api-app-root-URL}*/swagger/docs/v1

   Możesz również pobrać [przykładowy dokument OpenAPI](https://pwrappssamples.blob.core.windows.net/samples/webAPI.json) z tego samouczka. 
   Upewnij się, że usunięcie komentarzy, które począwszy od "/ /", aby korzystać z dokumentu.

5. Zapisywanie zawartości w formacie JSON. Oparte na przeglądarce, należy skopiować i wkleić tekst do pusty plik tekstowy.

## <a name="next-steps"></a>Następne kroki

* [Konfigurowanie uwierzytelniania dla łączników niestandardowych](../logic-apps/custom-connector-azure-active-directory-authentication.md)











