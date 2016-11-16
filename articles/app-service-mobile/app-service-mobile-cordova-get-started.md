---
title: "Tworzenie aplikacji oprogramowania Cordova przy użyciu funkcji Azure App Service Mobile Apps | Microsoft Docs"
description: "Postępuj zgodnie z tym samouczkiem, aby rozpocząć korzystanie z zapleczy usług mobilnych Azure na potrzeby opracowywania aplikacji dla oprogramowania Apache Cordova."
services: app-service\mobile
documentationcenter: javascript
author: adrianhall
manager: erikre
editor: 
tags: 
keywords: cordova,javascript,mobile,client
ms.assetid: 0b08fc12-0a80-42d3-9cc1-9b3f8d3e3a3f
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: adrianha
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e8a51979c5444d1e1b454d8f434b421f9cc17c24


---
# <a name="create-an-apache-cordova-app"></a>Tworzenie aplikacji oprogramowania Apache Cordova
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Omówienie
W tym samouczku przedstawiono sposób dodawania usługi zaplecza opartej na chmurze do aplikacji mobilnej oprogramowania Apache Cordova przy użyciu zaplecza aplikacji mobilnej Azure.  Będziesz tworzyć nowe zaplecze aplikacji mobilnej oraz prostą aplikację oprogramowania Apache Cordova typu *Lista czynności do wykonania*, która przechowuje dane aplikacji na platformie Azure.

Wykonanie kroków tego samouczka jest wymagane w przypadku wszystkich innych samouczków z zakresu oprogramowania Apache Cordova dotyczących używania funkcji Mobile Apps w usłudze Azure App Service.

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Komputer z programem [Visual Studio Community 2015] lub nowszym.
* [Visual Studio Tools for Apache Cordova].
* [Aktywne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

Można również pominąć program Visual Studio i używać bezpośrednio wiersza polecenia programu Apache Cordova.  Jest to przydatne w przypadku wykonywania kroków samouczka na komputerze Mac.  Kompilowanie aplikacji klienckich oprogramowania Apache Cordova za pomocą wiersza polecenia nie zostało opisane w tym samouczku.

## <a name="create-a-new-azure-mobile-app-backend"></a>Tworzenie zaplecza nowej aplikacji mobilnej Azure
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Obejrzyj wideo przedstawiające podobne kroki](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Konfigurowanie projektu serwera
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Pobieranie i uruchamianie aplikacji oprogramowania Apache Cordova
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Następne kroki
Teraz po ukończeniu tego samouczka szybkiego startu, przejdź do jednego z następujących samouczków:

* [Dodawanie uwierzytelniania] do aplikacji oprogramowania Apache Cordova.
* [Dodawanie powiadomień wypychanych] do aplikacji oprogramowania Apache Cordova.

Dowiedz się więcej na temat najważniejszych pojęć związanych z usługą Azure App Service.

* [Uwierzytelnianie]
* [Powiadomienia wypychane]

Dowiedz się, jak korzystać z zestawów SDK.

* [Zestaw Apache Cordova SDK]
* [Zestaw ASP.NET Server SDK]
* [Zestaw Node.js Server SDK]

<!-- Images. -->

<!-- URLs -->
[Witryna Azure Portal]: https://portal.azure.com/
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Dodawanie uwierzytelniania]: app-service-mobile-cordova-get-started-users.md
[Dodawanie powiadomień wypychanych]: app-service-mobile-cordova-get-started-push.md
[Uwierzytelnianie]: app-service-mobile-auth.md
[Powiadomienia wypychane]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Zestaw Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[Zestaw ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Zestaw Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md



<!--HONumber=Nov16_HO2-->


