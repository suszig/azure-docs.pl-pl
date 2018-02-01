---
title: "Tworzenie pierwszej funkcji na platformie Azure przy użyciu programu Visual Studio | Microsoft Docs"
description: "Tworzenie prostej funkcji wyzwalanej przez protokół HTTP i publikowanie jej na platformie Azure przy użyciu narzędzi usługi Azure Functions dla programu Visual Studio."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "azure functions, funkcje, przetwarzanie zdarzeń, obliczenia, architektura bez serwera"
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/17/2018
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: e8a43febdb2958f58ecb8d82f9f42b39c591522d
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="create-your-first-function-using-visual-studio"></a>Tworzenie pierwszej funkcji przy użyciu programu Visual Studio

Usługa Azure Functions umożliwia wykonywanie kodu w środowisku [bezserwerowym](https://azure.microsoft.com/overview/serverless-computing/) bez konieczności uprzedniego tworzenia maszyny wirtualnej lub publikowania aplikacji internetowej.

W tym artykule przedstawiono użycie narzędzi programu Visual Studio 2017 dla usługi Azure Functions w celu lokalnego utworzenia i przetestowania funkcji „hello world”. Kod funkcji zostanie następnie opublikowany na platformie Azure. Te narzędzia są dostępne jako część obciążenia projektowania na platformie Azure w programie Visual Studio 2017.

![Kod usługi Azure Functions w projekcie programu Visual Studio](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

Ten temat zawiera [wideo](#watch-the-video) demonstrujące te same podstawowe kroki.

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

* Zainstaluj program [Visual Studio 2017 w wersji 15.4](https://www.visualstudio.com/vs/) lub nowszej zawierający obciążenie **Programowanie na platformie Azure**.

    ![Instalowanie programu Visual Studio 2017 z obciążeniem Programowanie na platformie Azure](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

* Upewnij się, że usługa Azure Functions i narzędzia usługi WebJobs zostały zaktualizowane do najnowszej wersji. Można to sprawdzić w obszarze **Aktualizacje** > **Visual Studio Marketplace** w oknie **Rozszerzenia i aktualizacje**.
    
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-a-function-app-project"></a>Tworzenie projektu aplikacji funkcji

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Program Visual Studio tworzy projekt, a w nim klasę zawierającą standardowy kod dla wybranego typu funkcji. Atrybut **FunctionName** metody ustawia nazwę funkcji. Atrybut **HttpTrigger** określa, że funkcja jest wyzwalana przez żądanie HTTP. Standardowy kod wysyła odpowiedź HTTP zawierającą wartość z treści żądania lub ciągu zapytania. Do funkcji można dodać powiązania danych wejściowych i wyjściowych przez zastosowanie odpowiednich atrybutów do metody. Aby uzyskać więcej informacji, zobacz sekcję [Triggers and bindings](functions-dotnet-class-library.md#triggers-and-bindings) (Wyzwalacze i powiązania) [dokumentacji usługi Azure Functions dla deweloperów w C#](functions-dotnet-class-library.md).

![Plik kodu funkcji](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

Po utworzeniu projektu funkcji i funkcji wyzwalanej przez protokół HTTP można je przetestować na komputerze lokalnym.

## <a name="test-the-function-locally"></a>Lokalne testowanie funkcji

Podstawowe narzędzia usługi Azure Functions umożliwiają uruchamianie projektu usługi Azure Functions na lokalnym komputerze deweloperskim. Monit o zainstalowanie tych narzędzi pojawia się przy pierwszym uruchomieniu funkcji w programie Visual Studio.  

1. Aby przetestować funkcję, naciśnij klawisz F5. Po wyświetleniu monitu zaakceptuj żądanie programu Visual Studio dotyczące pobrania i zainstalowania podstawowych narzędzi usługi Azure Functions (CLI). Może także być konieczne włączenie wyjątku zapory, aby umożliwić narzędziom obsługę żądań HTTP.

2. Skopiuj adres URL funkcji z danych wyjściowych środowiska uruchomieniowego usługi Azure Functions.  

    ![Lokalne środowisko uruchomieniowe platformy Azure](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. Wklej adres URL żądania HTTP w pasku adresu przeglądarki. Dołącz ciąg zapytania `?name=<yourname>` do tego adresu URL i wykonaj żądanie. Na poniższym obrazie przedstawiono wyświetloną w przeglądarce odpowiedź na lokalne żądanie GET zwróconą przez funkcję: 

    ![Odpowiedź hosta localhost funkcji wyświetlona w przeglądarce](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. Aby zatrzymać debugowanie, kliknij przycisk **Zatrzymaj** na pasku narzędzi programu Visual Studio.

Gdy będziesz mieć pewność, że funkcja działa poprawnie na komputerze lokalnym, możesz opublikować projekt na platformie Azure.

## <a name="publish-the-project-to-azure"></a>Publikowanie projektu na platformie Azure

Aby opublikować projekt, musisz mieć aplikację funkcji w swojej subskrypcji platformy Azure. Aplikację funkcji możesz utworzyć bezpośrednio w programie Visual Studio.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Testowanie funkcji na platformie Azure

1. Skopiuj podstawowy adres URL aplikacji funkcji ze strony profilu publikowania. Część `localhost:port` adresu URL używaną podczas lokalnego testowania funkcji zastąp nowym podstawowym adresem URL. Tak jak poprzednio dołącz ciąg zapytania `?name=<yourname>` do tego adresu URL i wykonaj żądanie.

    Adres URL, który wywołuje funkcję wyzwalaną przez protokół HTTP, powinien mieć następujący format:

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. Wklej nowy adres URL żądania HTTP na pasku adresu przeglądarki. Na poniższym obrazie przedstawiono wyświetloną w przeglądarce odpowiedź na zdalne żądanie GET zwróconą przez funkcję: 

    ![Odpowiedź funkcji wyświetlona w przeglądarce](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)

## <a name="watch-the-video"></a>Obejrzyj film

> [!VIDEO https://www.youtube-nocookie.com/embed/DrhG-Rdm80k]

## <a name="next-steps"></a>Następne kroki

W programie Visual Studio utworzono aplikację funkcji C# z prostą funkcją wyzwalaną przez protokół HTTP. 

+ Aby dowiedzieć się, jak skonfigurować projekt w celu obsługi innych typów wyzwalaczy i powiązań, zobacz sekcję [Configure the project for local development (Konfigurowanie projektu na potrzeby lokalnego projektowania)](functions-develop-vs.md#configure-the-project-for-local-development) w temacie [Azure Functions Tools for Visual Studio (Narzędzia usługi Azure Functions dla programu Visual Studio)](functions-develop-vs.md).
+ Aby dowiedzieć się więcej na temat lokalnego testowania i debugowania przy użyciu podstawowych narzędzi usługi Azure Functions, zobacz [Code and test Azure Functions locally (Kodowanie i lokalne testowanie usługi Azure Functions)](functions-run-local.md). 
+ Aby dowiedzieć się więcej o projektowaniu funkcji jako bibliotek klasy .NET, zobacz [Korzystanie z bibliotek klasy .NET w usłudze Azure Functions](functions-dotnet-class-library.md). 

