<properties
    pageTitle="Tworzenie aplikacji oprogramowania Cordova przy użyciu funkcji Azure App Service Mobile Apps | Microsoft Azure"
    description="Postępuj zgodnie z tym samouczkiem, aby rozpocząć korzystanie z zapleczy usług mobilnych Azure na potrzeby opracowywania aplikacji dla oprogramowania Apache Cordova."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova,javascript,mobile,client" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/11/2016"
    ms.author="glenga"/>


#Tworzenie aplikacji oprogramowania Apache Cordova

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## Omówienie

W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji mobilnej oprogramowania Apache Cordova przy użyciu zaplecza aplikacji mobilnej Azure.  Będziesz tworzyć nowe zaplecze aplikacji mobilnej oraz prostą aplikację oprogramowania Apache Cordova typu _Lista czynności do wykonania_, która przechowuje dane aplikacji na platformie Azure.

Wykonanie kroków tego samouczka jest wymagane w przypadku wszystkich innych samouczków z zakresu oprogramowania Apache Cordova dotyczących używania funkcji Mobile Apps w usłudze Azure App Service.

## Wymagania wstępne

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Komputer z programem [Visual Studio Community 2015] lub nowszym.
* [Visual Studio Tools for Apache Cordova].
* [Aktywne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

Można również pominąć program Visual Studio i używać bezpośrednio wiersza polecenia programu Apache Cordova.  Jest to przydatne w przypadku wykonywania kroków samouczka na komputerze Mac.  Kompilowanie aplikacji klienckich oprogramowania Apache Cordova za pomocą wiersza polecenia nie zostało opisane w tym samouczku.

## Tworzenie zaplecza nowej aplikacji mobilnej Azure

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Obejrzyj film przedstawiający podobne kroki](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## Konfigurowanie projektu serwera

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## Pobieranie i uruchamianie aplikacji oprogramowania Apache Cordova

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## Następne kroki

Teraz po ukończeniu tego samouczka szybkiego startu, przejdź do jednego z następujących samouczków:

* [Dodawanie uwierzytelniania] do aplikacji oprogramowania Apache Cordova.
* [Dodawanie powiadomień wypychanych] do aplikacji oprogramowania Apache Cordova.

Dowiedz się więcej na temat najważniejszych pojęć związanych z usługą Azure App Service.

* [Authentication]
* [Powiadomienia wypychane]

Dowiedz się, jak korzystać z zestawów SDK.

* [Zestaw Apache Cordova SDK]
* [Zestaw ASP.NET Server SDK]
* [Zestaw Node.js Server SDK]

<!-- Images. -->

<!-- URLs -->
[Azure Portal]: https://portal.azure.com/
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Dodawanie uwierzytelniania]: app-service-mobile-cordova-get-started-users.md
[Dodawanie powiadomień wypychanych]: app-service-mobile-cordova-get-started-push.md
[Authentication]: app-service-mobile-auth.md
[Powiadomienia wypychane]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Zestaw Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[Zestaw ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Zestaw Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md



<!--HONumber=Sep16_HO3-->


