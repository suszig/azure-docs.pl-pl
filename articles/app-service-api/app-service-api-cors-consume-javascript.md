---
title: "Obsługa mechanizmu CORS w usłudze App Service | Microsoft Docs"
description: "Dowiedz się, jak korzystać z obsługi mechanizmu CORS w usłudze Azure App Service."
services: app-service\api
documentationcenter: .net
author: tdykstra
manager: wpickett
editor: 
ms.assetid: 4f980a97-b9f5-4d1d-87ab-82b60bb96e1c
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/27/2016
ms.author: rachelap
translationtype: Human Translation
ms.sourcegitcommit: 4edd2696c9a5709ded6e2a3e352090775335f0d2
ms.openlocfilehash: be48f1f0b9c3c749c13fd89f63d41fbaff6a62ee


---
# <a name="consume-an-api-app-from-javascript-using-cors"></a>Korzystanie z aplikacji interfejsu API z poziomu języka JavaScript przy użyciu mechanizmu CORS
Usługa App Service udostępnia wbudowaną obsługę [współużytkowania zasobów między źródłami (CORS, Cross-Origin Resource Sharing)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), co pozwala klientom języka JavaScript wykonywać międzydomenowe wywołania interfejsów API hostowanych w Aplikacjach interfejsu API. Usługa App Service umożliwia skonfigurowanie dostępu do interfejsu API za pomocą mechanizmu CORS bez pisania żadnego kodu interfejsu API.

Ten artykuł zawiera dwie sekcje:

* W sekcji [Konfigurowanie mechanizmu CORS](#corsconfig) ogólnie opisano konfigurowanie mechanizmu CORS w przypadku dowolnej aplikacji interfejsu API, aplikacji sieci Web lub aplikacji mobilnej. Te informacje dotyczą wszystkich platform obsługiwanych przez usługę App Service, w tym .NET, Node.js i Java. 
* Sekcja [Kolejne kroki samouczka ułatwiającego rozpoczęcie pracy z platformą .NET](#tutorialstart) stanowi początek samouczka demonstrującego obsługę mechanizmu CORS. Te informacje rozszerzają zagadnienia omówione w [pierwszym samouczku zawierającym wprowadzenie do usługi API Apps](app-service-api-dotnet-get-started.md). 

## <a name="a-idcorsconfiga-how-to-configure-cors-in-azure-app-service"></a><a id="corsconfig"></a> Konfigurowanie mechanizmu CORS w usłudze Azure App Service
Mechanizm CORS można skonfigurować w witrynie Azure Portal lub za pomocą narzędzi usługi [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

#### <a name="configure-cors-in-the-azure-portal"></a>Konfigurowanie mechanizmu CORS w portalu Azure
1. Otwórz witrynę [Azure Portal](https://portal.azure.com/) w przeglądarce.
2. Kliknij pozycję **App Services**, a następnie kliknij nazwę aplikacji interfejsu API.
   
    ![Wybieranie aplikacji interfejsu API w portalu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. W bloku **Ustawienia**, który pojawi się na prawo od bloku **Aplikacja interfejsu API**, znajdź sekcję **API**, a następnie kliknij pozycję **CORS**.
   
   ![Wybieranie pozycji CORS w bloku Ustawienia](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. W polu tekstowym wprowadź adres lub adresy URL, z których mają być dozwolone wywołania języka JavaScript.

    Jeśli na przykład aplikację języka JavaScript wdrożono w aplikacji sieci Web o nazwie ToDoListAngular, wpisz adres „https://todolistangular.azurewebsites.net”. Alternatywnie można wprowadzić gwiazdkę (*), aby określić, że wszystkie domeny pochodzenia są akceptowane.


1. Kliknij pozycję **Zapisz**.
   
   ![Klikanie pozycji Zapisz.](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   Po kliknięciu pozycji **Zapisz** aplikacja interfejsu API będzie akceptować wywołania języka JavaScript pochodzące z określonych adresów URL.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Konfigurowanie mechanizmu CORS za pomocą narzędzi usługi Azure Resource Manager
Mechanizm CORS dla aplikacji interfejsu API można również skonfigurować za pomocą [szablonów usługi Azure Resource Manager](../resource-group-authoring-templates.md) i narzędzi wiersza polecenia, takich jak [program Azure PowerShell](/powershell/azureps-cmdlets-docs) i [interfejs wiersza polecenia Azure](../xplat-cli-install.md). 

Aby zobaczyć przykładowy szablon usługi Azure Resource Manager, który umożliwia ustawienie właściwości CORS, otwórz [plik azuredeploy.json znajdujący się w repozytorium przykładowej aplikacji opisanej w tym samouczku ](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Znajdź sekcję szablonu, który wygląda następująco:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a name="a-idtutorialstarta-continuing-the-net-getting-started-tutorial"></a><a id="tutorialstart"></a> Kolejne kroki samouczka ułatwiającego rozpoczęcie pracy z platformą .NET
Jeśli korzystasz z serii szkoleń wprowadzających do Aplikacji interfejsu API dotyczących platform Node.js lub Java, to w tym momencie kończysz tę serię. Sekcja [Następne kroki](#next-steps) zawiera propozycje materiałów, które umożliwiają kontynuowanie poznawania usługi API Apps.

W dalszej części tego artykułu, która stanowi kontynuację serii wprowadzającej do platformy .NET, założono, że udało Ci się ukończyć [pierwszy samouczek](app-service-api-dotnet-get-started.md).

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>Wdrażanie projektu ToDoListAngular w nowej aplikacji sieci Web
W [pierwszym samouczku](app-service-api-dotnet-get-started.md) utworzono aplikacje interfejsu API warstwy środkowej i warstwy danych. Bieżący samouczek obejmuje utworzenie jednostronicowej aplikacji sieci Web, która wywołuje aplikację interfejsu API warstwy środkowej. Aby aplikacja jednostronicowa działała, musisz włączyć mechanizm CORS w aplikacji interfejsu API warstwy środkowej. 

[Przykładowa aplikacja ToDoList](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) zawiera projekt ToDoListAngular, który jest prostym klientem AngularJS wywołującym projekt interfejsu API sieci Web warstwy środkowej ToDoListAPI. Kod języka JavaScript zawarty w pliku *app/scripts/todoListSvc.js* wywołuje funkcje interfejsu API przy użyciu dostawcy usługi HTTP AngularJS. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 

            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Tworzenie nowej aplikacji sieci Web dla projektu ToDoListAngular
Procedura obejmująca tworzenie nowej aplikacji sieci Web usługi App Service i wdrażanie w niej projektu jest podobna do instrukcji dotyczących [tworzenia i wdrażania aplikacji interfejsu API zawartych w pierwszym samouczku tej serii](app-service-api-dotnet-get-started.md#createapiapp). Jedyna różnica polega na tym, że typ aplikacji to **aplikacja sieci Web**, a nie **aplikacja interfejsu API**.  Aby zapoznać się ze zrzutami ekranu okien dialogowych, zobacz 

1. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy projekt ToDoListAngular, a następnie kliknij polecenie **Opublikuj**.
2. Na karcie **Profil** kreatora **Publikowanie w sieci Web** kliknij pozycję **Microsoft Azure App Service**.
3. W oknie dialogowym **App Service** kliknij pozycję **Nowy**.
4. Na karcie **Hosting** okna dialogowego **Tworzenie usługi App Service** wpisz **nazwę aplikacji sieci Web** unikatową w domenie *azurewebsites.net*. 
5. Wybierz **subskrypcję** platformy Azure, której chcesz używać.
6. Z listy rozwijanej **Grupa zasobów** wybierz wcześniej utworzoną grupę zasobów.
7. Z listy rozwijanej **Plan usługi App Service** wybierz wcześniej utworzony plan. 
8. Kliknij przycisk **Utwórz**.
   
    Program Visual Studio utworzy aplikację sieci Web wraz z odpowiednim profilem publikowania i przejdzie do kroku **Połączenie** kreatora **Publikowanie w sieci Web**.
   
    W tym momencie nie klikaj pozycji **Opublikuj**. W następnej sekcji zostanie skonfigurowana nowa aplikacja sieci Web wywołująca aplikację interfejsu API warstwy środkowej, w której działa usługa App Service. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>Konfigurowanie adresu URL warstwy środkowej w ustawieniach aplikacji sieci Web
1. Otwórz witrynę [Azure Portal](https://portal.azure.com/) i przejdź do bloku **Aplikacja sieci Web** dla aplikacji sieci Web utworzonej w celu hostowania projektu TodoListAngular (frontonu).
2. Kliknij kolejno pozycje **Ustawienia > Ustawienia aplikacji**.
3. W sekcji **Ustawienia aplikacji** dodaj następujący klucz i wartość:
   
   | Klucz | Wartość | Przykład |
   | --- | --- | --- |
   | toDoListAPIURL |https://{nazwa aplikacji interfejsu API warstwy środkowej}.azurewebsites.net |https://todolistapi0121.azurewebsites.net |
4. Kliknij pozycję **Zapisz**.
   
    Gdy kod działa na platformie Azure, ta wartość zastępuje adres URL localhost, który został określony w pliku *Web.config*. 
   
    Kod, który służy do pobierania wartości ustawienia, znajduje się w pliku *index.cshtml*:
   
        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>
   
    Kod w pliku *todoListSvc.js* używa tego ustawienia:
   
        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>Wdrażanie projektu sieci Web ToDoListAngular w nowej aplikacji sieci Web
* W programie Visual Studio w kroku **Połączenie** kreatora **Publikowanie w sieci Web** kliknij pozycję **Opublikuj**.
  
   Program Visual Studio wdroży projekt ToDoListAngular w nowej aplikacji sieci Web i otworzy w przeglądarce adres URL aplikacji sieci Web. 

### <a name="test-the-application-without-cors-enabled"></a>Testowanie aplikacji bez włączonego mechanizmu CORS
1. W obszarze narzędzi dla deweloperów w przeglądarce otwórz okno konsoli.
2. W oknie przeglądarki, w którym zostanie wyświetlony interfejs użytkownika klienta AngularJS, kliknij link **listy zadań do wykonania**.
   
    Próba wywołania aplikacji interfejsu API warstwy środkowej przez kod języka JavaScript kończy się niepowodzeniem, ponieważ fronton jest uruchomiony w innej domenie niż wewnętrzna baza danych. W oknie konsoli narzędzi dla deweloperów w przeglądarce pojawi się komunikat o błędzie między źródłami.
   
    ![Komunikat o błędzie między źródłami](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>Konfigurowanie mechanizmu CORS dla aplikacji interfejsu API warstwy środkowej
Ta sekcja dotyczy konfiguracji ustawienia mechanizmu CORS na platformie Azure na potrzeby aplikacji interfejsu API warstwy środkowej ToDoListAPI. To ustawienie umożliwia aplikacji interfejsu API warstwy środkowej odbieranie wywołań języka JavaScript z aplikacji sieci Web utworzonej dla projektu ToDoListAngular.

1. Otwórz [portal Azure](https://portal.azure.com/) w przeglądarce.
2. Kliknij pozycję **App Services**, a następnie kliknij aplikację interfejsu API (warstwy środkowej) ToDoListAPI.
   
    ![Wybieranie aplikacji interfejsu API w portalu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. W bloku **Ustawienia**, który pojawi się na prawo od bloku **Aplikacja interfejsu API**, znajdź sekcję **API**, a następnie kliknij pozycję **CORS**.
   
   ![Wybieranie mechanizmu CORS w portalu](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. W polu tekstowym wpisz adres URL aplikacji sieci Web ToDoListAngular (frontonu). Jeśli na przykład projekt ToDoListAngular wdrożono w aplikacji sieci Web o nazwie todolistangular0121, musisz zezwolić na wywołania pochodzące z adresu URL `https://todolistangular0121.azurewebsites.net`.
   
   Alternatywnie można wprowadzić gwiazdkę (*), aby określić, że wszystkie domeny pochodzenia są akceptowane.
5. Kliknij pozycję **Zapisz**.
   
   ![Klikanie pozycji Zapisz.](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   Po kliknięciu pozycji **Zapisz** aplikacja interfejsu API będzie akceptować wywołania języka JavaScript pochodzące z określonego adresu URL. Na tym zrzucie ekranu widać, że skonfigurowano akceptowanie wywołań klienta języka JavaScript pochodzących z aplikacji sieci Web ToDoListAngular przez aplikację interfejsu API ToDoListAPI0223.

### <a name="test-the-application-with-cors-enabled"></a>Testowanie aplikacji z włączonym mechanizmem CORS
* Otwórz w przeglądarce adres URL HTTPS aplikacji sieci Web. 
  
    W tej konfiguracji aplikacja pozwala wyświetlać, dodawać, edytować i usuwać elementy zadań do wykonania. 
  
    ![Strona z listą zadań do wykonania w przykładowej aplikacji](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>Porównanie mechanizmu CORS usługi App Service i mechanizmu CORS interfejsu API sieci Web
W projekcie interfejsu API sieci Web można zainstalować pakiet NuGet [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/), aby móc za pomocą kodu określać domeny, z których mogą pochodzić wywołania języka JavaScript akceptowane przez interfejs API.

Obsługa mechanizmu CORS interfejsu API sieci Web jest bardziej elastyczna niż obsługa mechanizmu CORS usługi App Service. Na przykład za pomocą kodu można określić różne dozwolone źródła dla różnych metod akcji, natomiast mechanizm CORS usługi App Service pozwala skonfigurować tylko jeden zestaw dozwolonych źródeł dla wszystkich metod aplikacji interfejsu API.

> [!NOTE]
> W jednej aplikacji interfejsu API nie należy jednocześnie używać mechanizmu CORS interfejsu API sieci Web i mechanizmu CORS usługi App Service. Mechanizm CORS usługi App Service ma wyższy priorytet, co sprawia, że użycie mechanizmu CORS interfejsu API sieci Web nie odniesie żadnego skutku. Jeśli na przykład w kodzie interfejsu API sieci Web włączysz wszystkie domeny pochodzenia, ale tylko jedną domenę pochodzenia w usłudze App Service, to aplikacja interfejsu API platformy Azure będzie akceptować tylko wywołania z domeny określonej w ustawieniach platformy Azure.
> 
> 

### <a name="how-to-enable-cors-in-web-api-code"></a>Włączanie mechanizmu CORS w kodzie interfejsu API sieci Web
Poniższe instrukcje stanowią podsumowanie procesu włączania obsługi mechanizmu CORS interfejsu API sieci Web. Aby uzyskać więcej informacji, zobacz [Enabling Cross-Origin Requests in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api) (Włączanie żądań Cross-Origin w interfejsie API sieci Web 2 platformy ASP.NET).

1. W projekcie interfejsu API sieci Web zainstaluj pakiet NuGet [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/).
2. Dodaj wiersz `config.EnableCors()` w kodzie metody **Register** klasy **WebApiConfig**, jak pokazano w poniższym przykładzie. 
   
        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
   
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
   
                // Web API routes
                config.MapHttpAttributeRoutes();
   
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }
3. W kontrolerze interfejsu API sieci Web dodaj instrukcję `using` dla przestrzeni nazw `System.Web.Http.Cors` i dodaj atrybut `EnableCors` do klasy kontrolera lub poszczególnych metod akcji. W poniższym przykładzie obsługa mechanizmu CORS dotyczy całego kontrolera.
   
        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController

## <a name="using-azure-api-management-with-api-apps"></a>Używanie usługi Azure API Management z usługą API Apps
Jeśli używasz usługi Azure API Management razem z aplikacją interfejsu API, skonfiguruj mechanizm CORS w usłudze API Management zamiast w aplikacji interfejsu API. Więcej informacji zawierają następujące zasoby:

* [Omówienie usługi Azure API Management (klip wideo, informacje dotyczące mechanizmu CORS są prezentowane od momentu 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [Zasady usługi API Management obejmujące różne domeny](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)

## <a name="troubleshooting"></a>Rozwiązywanie problemów
W przypadku napotkania problemów podczas pracy z tym samouczkiem skorzystaj z poniższych sugestii dotyczących ich rozwiązywania.

* Upewnij się, że używasz najnowszej wersji [zestawu Azure SDK dla platformy .NET dla programu Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).
* Upewnij się, że wprowadzono adres `https` w ustawieniach mechanizmu CORS i sprawdź, czy adres `https` jest używany do uruchomienia aplikacji frontonu sieci Web.
* Upewnij się, że ustawienia mechanizmu CORS skonfigurowano w aplikacji interfejsu API warstwy środkowej, a nie w aplikacji frontonu sieci Web.
* Jeśli konfigurujesz mechanizm CORS zarówno w kodzie aplikacji, jak i usłudze Azure App Service, pamiętaj o tym, że ustawienia mechanizmu CORS Usługi aplikacji zastąpią instrukcje zawarte w kodzie aplikacji. 

Aby dowiedzieć się więcej o funkcjach programu Visual Studio, które ułatwiają rozwiązywanie problemów, zobacz [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) (Rozwiązywanie problemów z aplikacjami usługi Azure App Service w programie Visual Studio).

## <a name="next-steps"></a>Następne kroki
W tym artykule pokazano, jak włączyć obsługę mechanizmu CORS usługi App Service w celu umożliwienia wywołania funkcji API z innej domeny przez kod języka JavaScript klienta. Aby dowiedzieć się więcej o aplikacjach interfejsu API, zapoznaj się z [wprowadzeniem do uwierzytelniania w usłudze App Service](../app-service/app-service-authentication-overview.md), a następnie przejdź do samouczka dotyczącego [uwierzytelniania użytkowników w Aplikacjach interfejsu API](app-service-api-dotnet-user-principal-auth.md).




<!--HONumber=Dec16_HO1-->


