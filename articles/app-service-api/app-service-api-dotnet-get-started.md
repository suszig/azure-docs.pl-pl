---
title: "Wprowadzenie do usługi API Apps i programu ASP.NET w usłudze App Service | Microsoft Docs"
description: "Dowiedz się, jak za pomocą programu Visual Studio 2015 utworzyć i wdrożyć aplikację interfejsu API platformy ASP.NET w usłudze Azure App Service oraz korzystać z tej aplikacji."
services: app-service\api
documentationcenter: .net
author: tdykstra
manager: wpickett
editor: 
ms.assetid: ddc028b2-cde0-4567-a6ee-32cb264a830a
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: hero-article
ms.date: 09/20/2016
ms.author: rachelap
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: d72cf46c4a93d805ac9586696a6b41d81af733fd


---
# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Wprowadzenie do usługi API Apps, platformy ASP.NET i programu Swagger w usłudze Azure App Service
[!INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Jest to pierwszy z serii samouczków demonstrujących korzystanie z funkcji Azure App Service, która pomaga tworzyć i hostować interfejsy API RESTful.  Ten samouczek obejmuje obsługę metadanych interfejsu API w formacie struktury Swagger.

Dowiesz się:

* Jak tworzyć i wdrażać [aplikacje interfejsu API](app-service-api-apps-why-best-platform.md) w usłudze Azure App Service za pomocą narzędzi wbudowanych w program Visual Studio 2015.
* Jak automatyzować odnajdywanie interfejsów API przy użyciu pakietu Swashbuckle NuGet w celu dynamicznego generowania metadanych interfejsu API struktury Swagger.
* Jak używać metadanych interfejsu API struktury Swagger w celu automatycznego generowania kodu klienta dla aplikacji interfejsu API.

## <a name="sample-application-overview"></a>Omówienie przykładowej aplikacji
W tym samouczku będziemy pracować z przykładową aplikacją zawierającą prostą listę zadań do wykonania. Aplikacja obejmuje fronton aplikacji jednostronicowej, warstwę środkową interfejsu API sieci Web platformy ASP.NET oraz warstwę danych interfejsu API sieci Web platformy ASP.NET.

![Diagram przedstawiający przykładową aplikację usługi API Apps](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Oto zrzut ekranu przedstawiający fronton [AngularJS](https://angularjs.org/).

![Lista zadań do wykonania dla przykładowej aplikacji usługi API Apps](./media/app-service-api-dotnet-get-started/todospa.png)

Rozwiązanie programu Visual Studio zawiera trzy projekty:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** — fronton: aplikacja jednostronicowa AngularJS, która wywołuje warstwę środkową.
* **ToDoListAPI** — warstwa środkowa: projekt interfejsu API sieci Web platformy ASP.NET, który wywołuje warstwę danych w celu wykonywania operacji CRUD na zadaniach do wykonania.
* **ToDoListDataAPI** — warstwa danych: projekt interfejsu API sieci Web platformy ASP.NET, który wykonuje operacje CRUD na zadaniach do wykonania.

Architektury trójwarstwowej użyto jedynie do celów pokazowych — przy użyciu usługi API Apps można zaimplementować wiele konfiguracji. Kod w poszczególnych warstwach ma na celu pokazanie użycia funkcji usługi API Apps i jest maksymalnie uproszczony. Na przykład jako mechanizmu stanu trwałego danych warstwy danych użyto pamięci serwera, a nie bazy danych.

Po wykonaniu czynności opisanych w tym samouczku dwa projekty interfejsu API sieci Web będą działały w chmurze w aplikacjach interfejsu API w usłudze App Service.

Następny samouczek z tej serii dotyczy wdrożenia frontonu aplikacji jednostronicowej w chmurze.

## <a name="prerequisites"></a>Wymagania wstępne
* Interfejs API sieci Web platformy ASP.NET — w instrukcjach samouczka założono, że masz podstawową wiedzę z zakresu korzystania z [interfejsu API sieci Web 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) platformy ASP.NET w programie Visual Studio.
* Konto platformy Azure — możesz [utworzyć konto bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
  
    Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Wypróbuj usługę App Service](http://go.microsoft.com/fwlink/?LinkId=523751). W tej lokalizacji możesz od razu utworzyć początkową aplikację o krótkim okresie istnienia w usłudze App Service — **bez karty kredytowej** i bez zobowiązań.
* Program Visual Studio 2015 z [zestawem Azure SDK dla platformy .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) — jeśli nie masz jeszcze programu Visual Studio 2015, zostanie on automatycznie zainstalowany podczas instalacji tego zestawu SDK.
  
  * W programie Visual Studio kliknij menu Pomoc -> Informacje o programie Microsoft Visual Studio i upewnij się, że zainstalowano „Narzędzia usługi Azure App Service 2.9.1” lub nowsze.
    
    ![Wersja narzędzi usługi Azure App Service](./media/app-service-api-dotnet-get-started/apiversion.png)
    
    > [!NOTE]
    > Zależnie od liczby składników zależnych zestawu SDK, które znajdują się już na komputerze, instalowanie zestawu SDK może trwać od kilku minut do pół godziny lub dłużej.
    > 
    > 

## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji
1. Pobierz repozytorium [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list).
   
    Możesz kliknąć przycisk **Download ZIP** (Pobierz plik ZIP) lub sklonować repozytorium na komputerze lokalnym.
2. Otwórz rozwiązanie ToDoList w programie Visual Studio w wersji 2015 lub 2013.
   
   1. Należy zaufać każdemu rozwiązaniu.
         ![Ostrzeżenie o zabezpieczeniach](./media/app-service-api-dotnet-get-started/securitywarning.png)
3. Skompiluj rozwiązanie (Ctrl + Shift + B) w celu przywrócenia pakietów NuGet.
   
    Możesz uruchomić aplikację lokalnie, jeśli chcesz sprawdzić jej działanie przed wdrożeniem. Upewnij się, że projektem startowym jest ToDoListDataAPI, a następnie uruchom rozwiązanie. Należy oczekiwać wyświetlenia błędu HTTP 403 w przeglądarce.

## <a name="use-swagger-api-metadata-and-ui"></a>Korzystanie z interfejsu użytkownika oraz metadanych interfejsu API programu Swagger
Usługa Azure App Service zapewnia wbudowaną obsługę metadanych interfejsu API programu [Swagger](http://swagger.io/) 2.0. W każdej aplikacji interfejsu API można określić końcowy adres URL, z którego są zwracane metadane w formacie JSON programu Swagger dla interfejsu API. Metadanych zwróconych z tego punktu końcowego można użyć do wygenerowania kodu klienta.

W projekcie interfejsu API sieci Web platformy ASP.NET można dynamicznie generować metadane programu Swagger za pomocą pakietu [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet. Pakiet Swashbuckle NuGet jest już zainstalowany w pobranych projektach ToDoListDataAPI i ToDoListAPI.

W tej części samouczka przyjrzymy się wygenerowanym metadanym programu Swagger 2.0 i spróbujemy przetestować interfejs użytkownika oparty na metadanych programu Swagger.

1. Ustaw projekt ToDoListDataAPI (a **nie** projekt ToDoListAPI) jako projekt startowy.
   
    ![Ustawianie ToDoDataAPI jako projektu startowego](./media/app-service-api-dotnet-get-started/startupproject.png)
2. Naciśnij klawisz F5 lub kliknij kolejno pozycje **Debuguj > Rozpocznij debugowanie**, aby uruchomić projekt w trybie debugowania.
   
    W przeglądarce zostanie wyświetlona strona z informacją o błędzie HTTP 403.
3. Na pasku adresu przeglądarki dodaj element `swagger/docs/v1` na końcu adresu i naciśnij klawisz Return. (Adres URL to `http://localhost:45914/swagger/docs/v1`).
   
    Jest to domyślny adres URL używany przez pakiet Swashbuckle do zwracania metadanych JSON programu Swagger 2.0 dla interfejsu API.
   
    Jeśli używasz przeglądarki Internet Explorer, pojawi się monit o pobranie pliku *v1.json*.
   
    ![Pobieranie metadanych JSON w programie Internet Explorer](./media/app-service-api-dotnet-get-started/iev1json.png)
   
    Jeśli używasz programu Chrome, Firefox lub Microsoft Edge, metadane JSON zostaną wyświetlone w oknie przeglądarki. Obsługa danych JSON wygląda inaczej w różnych przeglądarkach, dlatego zawartość okna widocznego w Twojej przeglądarce może różnić się nieco od przykładowej ilustracji.
   
    ![Metadane JSON w programie Chrome](./media/app-service-api-dotnet-get-started/chromev1json.png)
   
    Poniższy przykład zawiera pierwszą sekcję metadanych programu Swagger dla interfejsu API oraz definicję metody Get. Te metadane stanowią podstawę interfejsu użytkownika programu Swagger używanego w kolejnych krokach. Zostaną też wykorzystane w dalszej części tego samouczka do automatycznego generowania kodu klienta.
   
        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },
4. Zamknij przeglądarkę i zatrzymaj debugowanie w programie Visual Studio.
5. W projekcie ToDoListDataAPI w **Eksploratorze rozwiązań** otwórz plik *App_Start\SwaggerConfig.cs*, a następnie przewiń w dół do wiersza 174 i usuń znaczniki komentarza z następującego kodu.
   
        /*
            })
        .EnableSwaggerUi(c =>
            {
        */
   
    Plik *SwaggerConfig.cs* jest tworzony podczas instalowania pakietu Swashbuckle w projekcie. Dostępne są różne sposoby konfigurowania pakietu Swashbuckle za pomocą tego pliku.
   
    Kod zawarty między usuniętymi znacznikami komentarza powoduje włączenie interfejsu użytkownika programu Swagger używanego w kolejnych krokach. Podczas tworzenia projektu interfejsu API sieci Web za pomocą szablonu projektu aplikacji interfejsu API ten kod jest domyślnie opatrzony znakami komentarza ze względów bezpieczeństwa.
6. Uruchom ponownie projekt.
7. Na pasku adresu przeglądarki dodaj element `swagger` na końcu adresu i naciśnij klawisz Return. (Adres URL to `http://localhost:45914/swagger`).
8. Gdy pojawi się strona interfejsu użytkownika programu Swagger, kliknij pozycję **ToDoList**, aby wyświetlić dostępne metody.
   
    ![Dostępne metody interfejsu użytkownika programu Swagger](./media/app-service-api-dotnet-get-started/methods.png)
9. Kliknij pierwszy przycisk **Get** na liście.
10. W sekcji **Parameters** (Parametry) wpisz gwiazdkę jako wartość parametru `owner`, a następnie kliknij pozycję **Try it out** (Wypróbuj to).
    
    Po dodaniu uwierzytelniania w kolejnych samouczkach warstwa środkowa będzie udostępniać rzeczywisty identyfikator użytkownika warstwie danych. Na razie aplikacja działa bez włączonego uwierzytelniania i dla wszystkich zadań identyfikator właściciela jest określony za pomocą gwiazdki.
    
    ![Próba wyświetlenia interfejsu użytkownika programu Swagger](./media/app-service-api-dotnet-get-started/gettryitout1.png)
    
    Interfejs użytkownika programu Swagger wywołuje metodę Get rozwiązania ToDoList i wyświetla kod odpowiedzi oraz dane wynikowe JSON.
    
    ![Wyniki próby wyświetlenia interfejsu użytkownika programu Swagger](./media/app-service-api-dotnet-get-started/gettryitout.png)
11. Kliknij przycisk **Post**, a następnie kliknij pole w obszarze **schematu modelu**.
    
    Kliknięcie schematu modelu powoduje wstępne wypełnienie pola wejściowego, w którym można określić wartość parametru metody Post. (Jeśli nie uda się tego zrobić w programie Internet Explorer, użyj innej przeglądarki albo ręcznie wprowadź wartość parametru w następnym kroku).  
    
    ![Klikanie przycisku Post podczas próby wyświetlenia interfejsu użytkownika programu Swagger](./media/app-service-api-dotnet-get-started/post.png)
12. Zmień dane JSON w polu wejściowym parametru `todo`, aby wyglądały one podobnie jak w poniższym przykładzie. Możesz również wpisać własny tekst:
    
        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }
13. Kliknij pozycję **Try it out** (Wypróbuj to).
    
    Interfejs API ToDoList zwraca kod odpowiedzi HTTP 204, co oznacza powodzenie.
14. Kliknij pierwszy przycisk **Get**, a następnie kliknij przycisk **Try it out** (Wypróbuj to) w tej części strony.
    
    Odpowiedź metody Get zawiera teraz nowe zadanie do wykonania.
15. Opcjonalnie: wypróbuj również metody Put, Delete i Get by ID.
16. Zamknij przeglądarkę i zatrzymaj debugowanie w programie Visual Studio.

Pakiet Swashbuckle współdziała z dowolnym projektem interfejsu API sieci Web platformy ASP.NET. Jeśli chcesz dodać generowanie metadanych programu Swagger do istniejącego projektu, po prostu zainstaluj pakiet Swashbuckle.

> [!NOTE]
> Metadane programu Swagger zawierają unikatowy identyfikator dla każdej operacji interfejsu API. Domyślnie pakiet Swashbuckle może generować zduplikowane identyfikatory operacji programu Swagger dla metod kontrolera interfejsu API sieci Web. Dzieje się tak, jeśli kontroler ma skonfigurowane przeciążone metody HTTP, takie jak `Get()` i `Get(id)`. Aby uzyskać informacje na temat obsługi przeciążeń, zobacz artykuł [Customize Swashbuckle-generated API definitions](app-service-api-dotnet-swashbuckle-customize.md) (Dostosowywanie definicji interfejsu API wygenerowanych przez pakiet Swashbuckle). Jeśli tworzysz projekt interfejsu API sieci Web w programie Visual Studio przy użyciu szablonu aplikacji interfejsu API platformy Azure, kod służący do generowania unikatowych identyfikatorów operacji jest automatycznie dodawany do pliku *SwaggerConfig.cs*.  
> 
> 

## <a name="a-idcreateapiappa-create-an-api-app-in-azure-and-deploy-code-to-it"></a><a id="createapiapp"></a> Tworzenie aplikacji interfejsu API na platformie Azure i wdrażanie w niej kodu
W tej sekcji użyjemy narzędzi platformy Azure zintegrowanych z kreatorem **Publikowanie w sieci Web** programu Visual Studio w celu utworzenia nowej usługi aplikacji interfejsu API na platformie Azure. Następnie wdrożymy projekt ToDoListDataAPI w nowej aplikacji interfejsu API i wywołamy interfejs API, uruchamiając interfejs użytkownika programu Swagger.

1. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy projekt ToDoListDataAPI, a następnie kliknij polecenie **Opublikuj**.
   
    ![Klikanie polecenia Opublikuj w programie Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)
2. W kroku **Profil** kreatora **Publikowanie w sieci Web** kliknij pozycję **Usługa Microsoft Azure App Service**.
   
   ![Klikanie pozycji Usługa Azure App Service w kreatorze Publikowanie w sieci Web](./media/app-service-api-dotnet-get-started/selectappservice.png)
3. Zaloguj się do konta platformy Azure, jeśli jeszcze tego nie zrobiono, lub odśwież swoje poświadczenia, jeśli wygasły.
4. W oknie dialogowym App Service wybierz **subskrypcję** platformy Azure, której chcesz używać, a następnie kliknij pozycję **Nowy**.
   
    ![Klikanie pozycji Nowy w oknie dialogowym App Service](./media/app-service-api-dotnet-get-started/clicknew.png)
   
    Zostanie wyświetlona karta **Hosting** okna dialogowego **Tworzenie aplikacji usługi App Service**.
   
    Ponieważ jest wdrażany projekt interfejsu API sieci Web z zainstalowanym pakietem Swashbuckle, program Visual Studio zakłada, że chcesz utworzyć aplikację interfejsu API. Z tego względu na pasku tytułu jest wyświetlany napis **Nazwa aplikacji interfejsu API**, a na liście rozwijanej **Zmień typ** jest widoczna pozycja **Aplikacja interfejsu API**.
   
    ![Typ aplikacji w oknie dialogowym usługi App Service](./media/app-service-api-dotnet-get-started/apptype.png)
5. W polu **Nazwa aplikacji interfejsu API** wprowadź nazwę, która jest unikatowa w domenie *azurewebsites.net*. Możesz zaakceptować domyślną nazwę sugerowaną przez program Visual Studio.
   
    Jeśli wprowadzona nazwa została już użyta przez inną osobę, z prawej strony będzie widoczny czerwony wykrzyknik.
   
    Adres URL aplikacji interfejsu API to `{API app name}.azurewebsites.net`.
6. Na liście **Grupa zasobów** kliknij pozycję **Nowa**, a następnie wprowadź ciąg „ToDoListGroup” lub inną wybraną nazwę.
   
    Grupa zasobów to kolekcja zasobów platformy Azure, takich jak aplikacje interfejsu API, bazy danych i maszyny wirtualne.    Do celów tego samouczka najlepiej utworzyć nową grupę zasobów, ponieważ ułatwi to usunięcie w jednym kroku wszystkich zasobów platformy Azure utworzonych na potrzeby samouczka.
   
    To pole pozwala wybrać istniejącą [grupę zasobów](../azure-resource-manager/resource-group-overview.md) lub utworzyć nową przez wpisanie nazwy, która różni się od wszystkich istniejących nazw grup zasobów w subskrypcji użytkownika.
7. Kliknij przycisk **Nowy** obok listy rozwijanej **Plan usługi App Service**.
   
    Na poniższym zrzucie ekranu przedstawiono przykładowe wartości pól **Nazwa aplikacji interfejsu API**, **Subskrypcja** i **Grupa zasobów**. Wartości wyświetlane na Twoim komputerze będą inne.
   
    ![Okno dialogowe Tworzenie usługi App Service](./media/app-service-api-dotnet-get-started/createas.png)
   
    Poniższe kroki umożliwiają utworzenie planu usługi App Service dla nowej grupy zasobów. Plan usługi App Service określa zasoby obliczeniowe, których używa aplikacja interfejsu API. Jeśli na przykład wybierzesz warstwę Bezpłatna, aplikacja interfejsu API będzie działać na udostępnionych maszynach wirtualnych. W przypadku niektórych warstw płatnych będzie ona działać na specjalnych maszynach wirtualnych. Aby uzyskać informacje o planach usługi App Service, zobacz temat [Omówienie planów usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
8. W oknie dialogowym **Konfigurowanie planu usługi App Service** wprowadź ciąg „ToDoListPlan” lub inną wybraną nazwę.
9. Z listy rozwijanej **Lokalizacja** wybierz najbliższą lokalizację.
   
    To ustawienie służy do określania, w którym centrum danych Azure zostanie uruchomiona aplikacja. Wybierz bliską lokalizację, aby zminimalizować [opóźnienie](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).
10. Na liście rozwijanej **Rozmiar** kliknij pozycję **Bezpłatna**.
    
    Do celów tego samouczka warstwa cenowa Bezpłatna zapewni wystarczającą wydajność.
11. W oknie dialogowym **Konfigurowanie planu usługi App Service** kliknij przycisk **OK**.
    
    ![Klikanie przycisku OK w oknie dialogowym Konfigurowanie planu usługi App Service](./media/app-service-api-dotnet-get-started/configasp.png)
12. W oknie dialogowym **Tworzenie usługi App Service** kliknij przycisk **Utwórz**.
    
    ![Klikanie przycisku Utwórz w oknie dialogowym Tworzenie usługi App Service](./media/app-service-api-dotnet-get-started/clickcreate.png)
    
    Program Visual Studio utworzy aplikację interfejsu API i profil publikowania, który zawiera wszystkie wymagane ustawienia usługi API Apps. Następnie zostanie otwarty kreator **Publikowanie w sieci Web**, który zostanie użyty do wdrożenia projektu.
    
    W kreatorze **Publikowanie w sieci Web** zostanie wyświetlona karta **Połączenie** (jak pokazano na poniższej ilustracji).
    
    Ustawienia **Serwer** i **Nazwa lokacji** na karcie **Połączenie** wskazują aplikację interfejsu API. Pola **Nazwa użytkownika** i **Hasło** zawierają poświadczenia wdrażania utworzone przez platformę Azure. Po ukończeniu wdrożenia program Visual Studio otworzy w przeglądarce stronę o adresie zawartym w polu **Docelowy adres URL** (przechowanie informacji o adresie jest jedyną funkcją pola **Docelowy adres URL**).  
13. Kliknij przycisk **Dalej**.
    
    ![Klikanie przycisku Dalej na karcie Połączenie kreatora Publikowanie w sieci Web](./media/app-service-api-dotnet-get-started/connnext.png)
    
    Następna karta to **Ustawienia** (pokazana poniżej). W tym miejscu możesz zmienić konfigurację kompilacji, aby wdrożyć kompilację debugowania dla [zdalnego debugowania](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug). Na tej karcie jest również wyświetlonych kilka **opcji publikowania pliku**:
    
    * Usunięcie dodatkowych plików w lokalizacji docelowej
    * Wstępna kompilacja podczas publikowania
    * Wykluczenie plików z folderu App_Data
    
    Czynności wykonywane w ramach tego samouczka nie wymagają użycia żadnej z tych opcji. Aby uzyskać szczegółowe wyjaśnienie działania tych opcji, zobacz artykuł [How to: Deploy a Web Project Using One-Click Publish in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (Porady: wdrażanie projektu sieci Web przy użyciu publikowania jednym kliknięciem w programie Visual Studio).
14. Kliknij przycisk **Dalej**.
    
    ![Klikanie przycisku Dalej na karcie Ustawienia kreatora Publikowanie w sieci Web](./media/app-service-api-dotnet-get-started/settingsnext.png)
    
    Następna karta to **Podgląd** (pokazana poniżej). W tej lokalizacji możesz zobaczyć pliki, które zostaną skopiowane z projektu do aplikacji interfejsu API. Podczas wdrażania projektu w aplikacji interfejsu API, która została już wcześniej wdrożona, kopiowane są tylko zmienione pliki. Jeśli chcesz wyświetlić listę plików, które zostaną skopiowane, możesz kliknąć przycisk **Uruchom podgląd**.
15. Kliknij przycisk **Opublikuj**.
    
    ![Klikanie przycisku Opublikuj na karcie Podgląd kreatora Publikowanie w sieci Web](./media/app-service-api-dotnet-get-started/clickpublish.png)
    
    Program Visual Studio wdroży projekt ToDoListDataAPI w nowej aplikacji interfejsu API. W oknie **Dane wyjściowe** pojawi się informacja o pomyślnym wdrożeniu, a w oknie przeglądarki zostanie wyświetlona strona o adresie URL usługi interfejsu API.
    
    ![Okno danych wyjściowych z informacją o pomyślnym wdrożeniu](./media/app-service-api-dotnet-get-started/deploymentoutput.png)
    
    ![Pomyślnie utworzona strona nowej usługi interfejsu API](./media/app-service-api-dotnet-get-started/appcreated.png)
16. Dodaj element „swagger” do adresu URL na pasku adresu przeglądarki, a następnie naciśnij klawisz Enter. (Adres URL to `http://{apiappname}.azurewebsites.net/swagger`).
    
    W przeglądarce pojawi się ten sam interfejs użytkownika programu Swagger, który był wyświetlany poprzednio, ale teraz działa on w chmurze. Po próbnym wywołaniu metody Get zobaczysz, że działanie aplikacji znowu obejmuje dwa domyślne zadania do wykonania. Zmiany wprowadzone wcześniej zostały zapisane w pamięci na komputerze lokalnym.
17. Otwórz [portal Azure](https://portal.azure.com/).
    
    Portal Azure to interfejs sieci Web, który służy do zarządzania zasobami platformy Azure, takimi jak aplikacje interfejsu API.
18. Kliknij przycisk **Więcej usług > App Services**.
    
    ![Przeglądanie usług App Services](./media/app-service-api-dotnet-get-started/browseas.png)
19. W bloku **App Services** znajdź swoją nową aplikację interfejsu API i kliknij ją. (W portalu Azure okna, które są wyświetlane z prawej strony, noszą nazwę *bloków*).
    
    ![Blok App Services](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)
    
    Zostaną otwarte dwa bloki. W pierwszym z nich jest widoczne omówienie aplikacji interfejsu API, a drugi zawiera długą listę ustawień, które można wyświetlać i zmieniać.
20. W bloku **Ustawienia** znajdź sekcję **API** i kliknij pozycję **Definicja interfejsu API**.
    
    ![Definicja interfejsu API w bloku ustawień](./media/app-service-api-dotnet-get-started/apidefinsettings.png)
    
    Blok **Definicja interfejsu API** umożliwia określenie adresu URL, z którego będą zwracane metadane programu Swagger 2.0 w formacie JSON. Gdy program Visual Studio tworzy aplikację interfejsu API, ustawia domyślną wartość adresu URL definicji interfejsu API dla metadanych wygenerowanych przez pakiet Swashbuckle, które były wyświetlane wcześniej. Ten adres URL składa się z podstawowego adresu URL aplikacji interfejsu API oraz elementu `/swagger/docs/v1`.
    
    ![Adres URL definicji interfejsu API](./media/app-service-api-dotnet-get-started/apidefurl.png)
    
    Po wybraniu aplikacji interfejsu API w celu wygenerowania dla niej kodu klienta program Visual Studio pobiera metadane z tego adresu URL.

## <a name="a-idcodegena-generate-client-code-for-the-data-tier"></a><a id="codegen"></a> Generowanie kodu klienta dla warstwy danych
Automatyczne generowanie kodu to jedna z zalet integracji programu Swagger z aplikacjami interfejsu API platformy Azure. Wygenerowane klasy klienta ułatwiają pisanie kodu, który wywołuje aplikację interfejsu API.

W projekcie ToDoListAPI kod klienta został już wygenerowany, ale w poniższych krokach zostanie on usunięty i wygenerowany ponownie. Ma to na celu zapoznanie się z procedurą generowania kodu.

1. W **Eksploratorze rozwiązań** programu Visual Studio usuń folder *ToDoListDataAPI* w projekcie ToDoListAPI. **Ostrzeżenie: usuń tylko folder, a nie projekt ToDoListDataAPI.**
   
    ![Usuwanie wygenerowanego kodu klienta](./media/app-service-api-dotnet-get-started/deletecodegen.png)
   
    Ten folder został utworzony przy użyciu procedury generowania kodu, która zostanie teraz przeprowadzona ponownie.
2. Kliknij prawym przyciskiem myszy projekt ToDoListAPI, a następnie kliknij kolejno polecenia **Dodaj > Klient interfejsu API REST**.
   
    ![Dodawanie klienta interfejsu API REST w programie Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)
3. W oknie dialogowym **Add REST API Client** (Dodawanie klienta interfejsu API REST) kliknij pozycję **Swagger URL** (Adres URL programu Swagger), a następnie kliknij pozycję **Select Azure Asset** (Wybierz element zawartości platformy Azure).
   
    ![Wybieranie elementu zawartości platformy Azure](./media/app-service-api-dotnet-get-started/codegenbrowse.png)
4. W oknie dialogowym **App Service** rozwiń grupę zasobów używaną na potrzeby tego samouczka i wybierz aplikację interfejsu API, a następnie kliknij przycisk **OK**.
   
    ![Wybieranie aplikacji interfejsu API w celu generowania kodu](./media/app-service-api-dotnet-get-started/codegenselect.png)
   
    Warto zauważyć, że po powrocie do okna dialogowego **Dodawanie klienta interfejsu API REST** pole tekstowe zostało wypełnione wartością adresu URL definicji interfejsu API, która była widoczna wcześniej w portalu.
   
    ![Adres URL definicji interfejsu API](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)
   
   > [!TIP]
   > Alternatywna metoda uzyskania metadanych podczas generowania kodu polega na bezpośrednim wprowadzeniu adresu URL zamiast korzystania z okna dialogowego przeglądania. Jeśli natomiast chcesz wygenerować kod klienta przed wdrożeniem aplikacji na platformie Azure, możesz uruchomić lokalnie projekt interfejsu API sieci Web, przejść do adresu URL, pod którym znajduje się plik danych JSON programu Swagger, zapisać ten plik, a następnie użyć opcji **Wybierz istniejący plik metadanych struktury Swagger**.
   > 
   > 
5. W oknie dialogowym **Dodawanie klienta interfejsu API REST** kliknij przycisk **OK**.
   
    Program Visual Studio utworzy folder o takiej samej nazwie jak nazwa aplikacji interfejsu API i wygeneruje klasy klienta.
   
    ![Pliki kodu dla wygenerowanego klienta](./media/app-service-api-dotnet-get-started/codegenfiles.png)
6. W projekcie ToDoListAPI otwórz plik *Controllers\ToDoListController.cs*, aby wyświetlić kod w wierszu 40, który wywołuje interfejs API za pomocą wygenerowanego klienta.
   
    W następującym fragmencie kodu pokazano tworzenie wystąpienia obiektu klienta i wywoływanie metody Get.
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }
   
        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }
   
    Parametr konstruktora pobiera końcowy adres URL z ustawienia aplikacji `toDoListDataAPIURL`. W pliku Web.config tej wartości przypisano adres URL lokalnego serwera IIS Express projektu interfejsu API, aby umożliwić lokalne uruchamianie aplikacji. W przypadku pominięcia parametru konstruktora domyślny punkt końcowy jest adresem URL użytym do wygenerowania kodu.
7. Wygenerowana klasa klienta będzie mieć inną nazwę, zależną od nazwy aplikacji interfejsu API. Zmień kod w pliku *Controllers\ToDoListController.cs*, tak aby nazwa typu była zgodna z danymi wygenerowanymi w projekcie. Jeśli na przykład aplikacja interfejsu API nosi nazwę ToDoListDataAPI071316, w kodzie:
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

wprowadź następujące zmiany:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>Tworzenie aplikacji interfejsu API w celu hostowania warstwy środkowej
W jednej z poprzednich części samouczka [utworzyliśmy aplikację interfejsu API warstwy danych i wdrożyliśmy w niej kod](#createapiapp).  Teraz wykonamy tę samą procedurę dla aplikacji interfejsu API warstwy środkowej.

1. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy projekt ToDoListAPI warstwy środkowej (a nie projekt ToDoListDataAPI warstwy danych), a następnie kliknij polecenie **Opublikuj**.
   
    ![Klikanie polecenia Opublikuj w programie Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)
2. Na karcie **Profil** kreatora **Publikowanie w sieci Web** kliknij pozycję **Microsoft Azure App Service**.
3. W oknie dialogowym **App Service** kliknij pozycję **Nowy**.
4. Na karcie **Hosting** okna dialogowego **Tworzenie usługi App Service** zaakceptuj domyślną wartość pola **Nazwa aplikacji interfejsu API** lub wpisz nazwę unikatową w domenie *azurewebsites.net*.
5. Wybierz **subskrypcję** platformy Azure, której używasz.
6. Z listy rozwijanej **Grupa zasobów** wybierz wcześniej utworzoną grupę zasobów.
7. Z listy rozwijanej **Plan usługi App Service** wybierz wcześniej utworzony plan. Domyślnie zostanie użyta ta wartość.
8. Kliknij przycisk **Utwórz**.
   
    Program Visual Studio utworzy aplikację interfejsu API wraz z odpowiednim profilem publikowania i przejdzie do kroku **Połączenie** kreatora **Publikowanie w sieci Web**.
9. W kroku **Połączenie** kreatora **Publikowanie w sieci Web** kliknij pozycję **Opublikuj**.
   
   Program Visual Studio wdroży projekt ToDoListAPI w nowej aplikacji interfejsu API i otworzy w przeglądarce adres URL tej aplikacji. Pojawi się informacja o pomyślnym utworzeniu strony.

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>Konfigurowanie wywoływania warstwy danych przez warstwę środkową
Jeśli teraz wywołasz aplikację interfejsu API warstwy środkowej, spróbuje ona wywołać warstwę danych przy użyciu adresu URL localhost, który wciąż jest skonfigurowany w pliku Web.config. W tej sekcji wprowadzimy adres URL aplikacji interfejsu API warstwy danych do ustawień środowiska aplikacji interfejsu API warstwy środkowej. Po pobraniu ustawienia adresu URL warstwy danych przez kod aplikacji interfejsu API warstwy środkowej ustawienie środowiska zastąpi wartość określoną w pliku Web.config.

1. Otwórz [portal Azure](https://portal.azure.com/) i przejdź do bloku **Aplikacja interfejsu API** dla aplikacji interfejsu API utworzonej w celu hostowania projektu TodoListAPI (warstwy środkowej).
2. W bloku **Ustawienia** aplikacji interfejsu API kliknij pozycję **Ustawienia aplikacji**.
3. W bloku **Ustawienia aplikacji** aplikacji interfejsu API przewiń w dół do sekcji **Ustawienia aplikacji** i dodaj następujący klucz oraz wartość. Wartość będzie adresem URL pierwszej aplikacji interfejsu API, która została opublikowana w ramach tego samouczka.
   
   | **Klucz** | toDoListDataAPIURL |
   | --- | --- |
   | **Wartość** |https://{nazwa aplikacji interfejsu API warstwy danych}.azurewebsites.net |
   | **Przykład** |https://todolistdataapi.azurewebsites.net |
4. Kliknij pozycję **Zapisz**.
   
    ![Klikanie pozycji Zapisz w ustawieniach aplikacji](./media/app-service-api-dotnet-get-started/asinportal.png)
   
    Gdy kod działa na platformie Azure, ta wartość zastępuje adres URL localhost określony w pliku Web.config.

## <a name="test"></a>Testowanie
1. W oknie przeglądarki otwórz adres URL nowej aplikacji interfejsu API warstwy środkowej utworzonej przed chwilą dla projektu ToDoListAPI. Aby przejść do tej lokalizacji, możesz kliknąć adres URL w głównym bloku aplikacji interfejsu API w portalu.
2. Dodaj element „swagger” do adresu URL na pasku adresu przeglądarki, a następnie naciśnij klawisz Enter. (Adres URL to `http://{apiappname}.azurewebsites.net/swagger`).
   
    W przeglądarce pojawi się ten sam interfejs użytkownika programu Swagger, który był wyświetlany w projekcie ToDoListDataAPI, ale teraz metoda Get nie wymaga pola `owner`, ponieważ aplikacja interfejsu API warstwy środkowej automatycznie wysyła tę wartość do aplikacji interfejsu API warstwy danych. (Podczas pracy z samouczkami dotyczącymi uwierzytelniania skonfigurujemy wysyłanie rzeczywistych identyfikatorów użytkowników dla parametru `owner` przez warstwę środkową. Obecnie te dane są zakodowane na stałe za pomocą gwiazdki).
3. Wykonaj próbne wywołania metody Get i innych metod, aby sprawdzić, czy wywołanie aplikacji interfejsu API warstwy danych przez aplikację interfejsu API warstwy środkowej przebiega pomyślnie.
   
    ![Metoda Get interfejsu użytkownika programu Swagger](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Rozwiązywanie problemów
W przypadku napotkania problemów podczas pracy z tym samouczkiem skorzystaj z poniższych sugestii dotyczących ich rozwiązywania:

* Upewnij się, że używasz najnowszej wersji [zestawu Azure SDK dla platformy .NET](http://go.microsoft.com/fwlink/?linkid=518003).
* Nazwy używanych projektów są podobne (ToDoListAPI i ToDoListDataAPI). Jeśli wykonanie opisanych instrukcji nie przynosi oczekiwanych rezultatów, upewnij się, że został otwarty odpowiedni projekt.
* Jeśli korzystasz z komputera działającego w sieci firmowej i próbujesz przeprowadzić wdrożenie w usłudze Azure App Service przez zaporę, upewnij się, że porty 443 i 8172 są otwarte dla narzędzia Web Deploy. W przypadku braku możliwości otwarcia tych portów można użyć innych metod wdrażania.  Zobacz temat [Deploy your app to Azure App Service](../app-service-web/web-sites-deploy.md) (Wdrażanie aplikacji w usłudze Azure App Service).
* Komunikaty o błędach „Nazwy tras muszą być unikatowe” mogą wystąpić, jeśli w aplikacji interfejsu API przypadkowo wdrożono niewłaściwy projekt, a następnie wdrożono właściwy projekt. Aby rozwiązać ten problem, wdróż ponownie właściwy projekt w aplikacji interfejsu API, a następnie na karcie **Ustawienia** kreatora **Publikowanie w sieci Web** zaznacz opcję **Usuń dodatkowe pliki w miejscu docelowym**.

Po uruchomieniu aplikacji interfejsu API platformy ASP.NET w usłudze Azure App Service warto dowiedzieć się więcej na temat funkcji programu Visual Studio ułatwiających rozwiązywanie problemów. Aby uzyskać informacje na temat rejestrowania, zdalnego debugowania i innych funkcji, zobacz temat [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) (Rozwiązywanie problemów z aplikacjami Azure App Service w programie Visual Studio).

## <a name="next-steps"></a>Następne kroki
W tym samouczku omówiono wdrażanie istniejących projektów interfejsu API sieci Web w aplikacjach interfejsu API, generowanie kodu klienta dla aplikacji interfejsu API oraz używanie aplikacji interfejsu API przez klientów platformy .NET. W następnym samouczku z tej serii przedstawiono [korzystanie z aplikacji interfejsu API z poziomu klientów języka JavaScript przy użyciu mechanizmu CORS](app-service-api-cors-consume-javascript.md).

Aby uzyskać więcej informacji na temat generowania kodu klienta, przejdź do repozytorium [Azure/AutoRest](https://github.com/azure/autorest) w witrynie GitHub.com. Aby uzyskać pomoc dotyczącą problemów z używaniem wygenerowanego klienta, zapoznaj się z opisem [problemu w repozytorium AutoRest](https://github.com/azure/autorest/issues).

Jeśli chcesz tworzyć nowe projekty aplikacji interfejsu API zupełnie od początku, użyj szablonu **aplikacji interfejsu API na platformie Azure**.

![Szablon aplikacji interfejsu API w programie Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

Użycie szablonu projektu **aplikacji interfejsu API na platformie Azure** jest równoważne wybraniu **pustego** szablonu platformy ASP.NET 4.5.2, dodaniu obsługi interfejsu API sieci Web za pomocą pola wyboru i zainstalowaniu pakietu Swashbuckle NuGet. Ponadto do szablonu dodawany jest kod konfiguracji pakietu Swashbuckle, który zapobiega tworzeniu zduplikowanych identyfikatorów operacji programu Swagger. Po utworzeniu projektu aplikacji interfejsu API możesz go wdrożyć w aplikacji interfejsu API w sposób opisany w tym samouczku.




<!--HONumber=Dec16_HO1-->


