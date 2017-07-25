---
title: "Tworzenie pierwszej funkcji na platformie Azure przy użyciu programu Visual Studio | Microsoft Docs"
description: "Tworzenie prostej funkcji wyzwalanej przez protokół HTTP i publikowanie jej na platformie Azure przy użyciu narzędzi usługi Azure Functions dla programu Visual Studio."
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: "azure functions, funkcje, przetwarzanie zdarzeń, obliczenia, architektura bez serwera"
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/05/2017
ms.author: rachelap, glenga
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: be7a9979ba7e6aa26c60b24bcc892ca35af3c1fc
ms.contentlocale: pl-pl
ms.lasthandoff: 07/19/2017

---
# <a name="create-your-first-function-using-visual-studio"></a>Tworzenie pierwszej funkcji przy użyciu programu Visual Studio

Usługa Azure Functions umożliwia wykonywanie kodu w środowisku bezserwerowym bez konieczności uprzedniego tworzenia maszyny wirtualnej lub publikowania aplikacji sieci Web.

> [!IMPORTANT]
> Do wykonania czynności w tym temacie użyto wersji zapoznawczej programu Visual Studio. Przed rozpoczęciem upewnij się, że zainstalowano [program Visual Studio w wersji zapoznawczej 2017 15.3](https://www.visualstudio.com/vs/preview/).

W tym temacie przedstawiono użycie narzędzi usługi Azure Functions dla programu Visual Studio 2017 w celu utworzenia i przetestowania lokalnej funkcji „hello world”. Kod funkcji zostanie następnie opublikowany na platformie Azure.

![Kod usługi Azure Functions w projekcie programu Visual Studio](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka niezbędne jest zainstalowanie następujących składników:

* [Program Visual Studio 2017 (wersja zapoznawcza) w wersji 15.3](https://www.visualstudio.com/vs/preview/) zawierający obciążenie **Programowanie na platformie Azure**.

    ![Instalowanie programu Visual Studio 2017 z obciążeniem Programowanie na platformie Azure](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-azure-functions-tools-for-visual-studio-2017"></a>Instalowanie narzędzi usługi Azure Functions dla programu Visual Studio 2017

Przed rozpoczęciem pracy musisz pobrać i zainstalować narzędzia usługi Azure Functions dla programu Visual Studio 2017. Narzędzi tych można używać tylko z programem Visual Studio 2017 (wersja zapoznawcza) w wersji 15.3 lub nowszej. Jeśli zainstalowano już narzędzia usługi Azure Functions, można pominąć tę sekcję.

[!INCLUDE [Install the Azure Functions Tools for Visual Studio](../../includes/functions-install-vstools.md)]   

## <a name="create-an-azure-functions-project-in-visual-studio"></a>Tworzenie projektu usługi Azure Functions w programie Visual Studio

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Po utworzeniu projektu można utworzyć pierwszą funkcję.

## <a name="create-the-function"></a>Tworzenie funkcji

W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy węzeł projektu i wybierz polecenie **Dodaj** > **Nowy element**. Wybierz pozycję **Funkcja platformy Azure** i kliknij pozycję **Dodaj**.

Wybierz pozycję **HttpTrigger**, wpisz **nazwę funkcji**, wybierz opcję **Anonimowe** w polu **Prawa dostępu** i kliknij pozycję **Utwórz**. Do utworzonej funkcji można uzyskać dostęp przez żądanie HTTP z dowolnego klienta. 

![Tworzenie nowej funkcji platformy Azure](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

Po utworzeniu funkcji wyzwalanej przez protokół HTTP można ją przetestować na komputerze lokalnym.

## <a name="test-the-function-locally"></a>Lokalne testowanie funkcji

[!INCLUDE [Test the function locally](../../includes/functions-vstools-test.md)]

Skopiuj adres URL funkcji z danych wyjściowych środowiska uruchomieniowego usługi Azure Functions.  

![Lokalne środowisko uruchomieniowe platformy Azure](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

 Wklej adres URL żądania HTTP w pasku adresu przeglądarki. Dołącz ciąg zapytania `&name=<yourname>` do tego adresu URL i wykonaj żądanie. Na poniższym obrazie przedstawiono wyświetloną w przeglądarce odpowiedź na lokalne żądanie GET zwróconą przez funkcję: 

![Odpowiedź hosta localhost funkcji wyświetlona w przeglądarce](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

Aby zatrzymać debugowanie, kliknij przycisk **Zatrzymaj** na pasku narzędzi programu Visual Studio.

Gdy będziesz mieć pewność, że funkcja działa poprawnie na komputerze lokalnym, możesz opublikować projekt na platformie Azure.

## <a name="publish-the-project-to-azure"></a>Publikowanie projektu na platformie Azure

Aby opublikować projekt, musisz mieć aplikację funkcji w swojej subskrypcji platformy Azure. Aplikację funkcji możesz utworzyć bezpośrednio w programie Visual Studio.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Testowanie funkcji na platformie Azure

Skopiuj podstawowy adres URL aplikacji funkcji ze strony profilu publikowania. Część `localhost:port` adresu URL używaną podczas lokalnego testowania funkcji zastąp nowym podstawowym adresem URL. Tak jak poprzednio dołącz ciąg zapytania `&name=<yourname>` do tego adresu URL i wykonaj żądanie.

Adres URL, który wywołuje funkcję wyzwalaną przez protokół HTTP, wygląda następująco:

    http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

Wklej nowy adres URL żądania HTTP na pasku adresu przeglądarki. Na poniższym obrazie przedstawiono wyświetloną w przeglądarce odpowiedź na zdalne żądanie GET zwróconą przez funkcję: 

![Odpowiedź funkcji wyświetlona w przeglądarce](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a>Następne kroki

W programie Visual Studio utworzono aplikację funkcji C# z prostą funkcją wyzwalaną przez protokół HTTP. 

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

Aby dowiedzieć się więcej na temat lokalnego testowania i debugowania przy użyciu podstawowych narzędzi usługi Azure Functions, zobacz [Code and test Azure Functions locally (Kodowanie i lokalne testowanie usługi Azure Functions)](functions-run-local.md). Aby dowiedzieć się więcej o projektowaniu funkcji jako bibliotek klasy .NET, zobacz [Korzystanie z bibliotek klasy .NET w usłudze Azure Functions](functions-dotnet-class-library.md). 


