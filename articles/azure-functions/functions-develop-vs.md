---
title: "Tworzenie funkcji platformy Azure przy użyciu programu Visual Studio | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak opracowywać i testować funkcje platformy Azure przy użyciu narzędzi funkcji Azure dla programu Visual Studio 2017 r."
services: functions
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: glenga
ms.openlocfilehash: ed1d8298123597fe8330b54f89fd580095f21ec7
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="azure-functions-tools-for-visual-studio"></a>Środowisko Azure Functions Tools for Visual Studio  

Narzędzi funkcji platformy Azure dla programu Visual Studio 2017 to rozszerzenie dla programu Visual Studio, który pozwala tworzyć, testować i wdrażać funkcje C# na platformie Azure. Jeśli jest to usprawnić pierwszej funkcji platformy Azure, możesz dowiedzieć się więcej, zobacz [wprowadzenie do usługi Azure Functions](functions-overview.md).

Narzędzia funkcji Azure zapewnia następujące korzyści: 

* Edytowania, tworzenia i uruchamiania funkcji na komputerze deweloperskim lokalnego. 
* Publikowanie projektu usługi Azure Functions bezpośrednio na platformie Azure. 
* Użycie zadań Webjob atrybutów w celu zadeklarowania powiązania funkcji bezpośrednio w kodzie C# zamiast utrzymywania osobnych function.json dla powiązania definicje.
* Tworzenie i wdrażanie wstępnie skompilowanym funkcje C#. Wstępnie skompilowane funkcje umożliwiać wydajności zimnego lepiej niż C# opartych na skryptach funkcji. 
* Kod funkcji w języku C# przy jednoczesnym zachowaniu wszystkie zalety tworzenia Visual Studio. 

W tym temacie przedstawiono sposób użycia narzędzia funkcji Azure dla programu Visual Studio 2017 do opracowywania funkcji w języku C#. Możesz również Dowiedz się, jak opublikować projekt na platformie Azure jako zestaw .NET.

> [!IMPORTANT]
> Nie można mieszać lokalne działania projektowe z portalu Programowanie w tej samej aplikacji funkcji. Podczas publikowania z lokalnej projektu aplikacji funkcji procesu wdrażania zastąpią wszystkie funkcje, które utworzonych w portalu.

## <a name="prerequisites"></a>Wymagania wstępne

Narzędzi funkcji platformy Azure jest uwzględniona w obciążenie Azure programowanie [programu Visual Studio 2017 wersji 15.4](https://www.visualstudio.com/vs/), lub jego nowsza wersja. Upewnij się, że uwzględniasz **Azure programowanie** obciążenia w instalacji programu Visual Studio 2017:

![Instalowanie programu Visual Studio 2017 z obciążeniem Programowanie na platformie Azure](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

Tworzenie i wdrażanie funkcji, należy również:

* Aktywna subskrypcja platformy Azure. Jeśli nie masz subskrypcji platformy Azure, [wolnego konta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) są dostępne.

* Konto magazynu Azure. Aby utworzyć konto magazynu, zobacz [Utwórz konto magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account).

## <a name="create-an-azure-functions-project"></a>Tworzenie projektu usługi Azure Functions 

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]


## <a name="configure-the-project-for-local-development"></a>Konfigurowanie projektu dla rozwoju lokalnych

Podczas tworzenia nowego projektu przy użyciu szablonu usługi Azure Functions, otrzymasz pusty C# projekt, który zawiera następujące pliki:

* **Host.JSON**: umożliwia skonfigurowanie hostów funkcji. Te ustawienia dotyczą zarówno podczas uruchamiania lokalnie i na platformie Azure. Aby uzyskać więcej informacji, zobacz [odwołania host.json](functions-host-json.md).
    
* **Local.Settings.JSON**: przechowuje ustawienia używane podczas uruchamiania lokalnego funkcji. Te ustawienia nie są używane przez platformę Azure, są one używane przez [Azure funkcje podstawowe narzędzia](functions-run-local.md). Użyj tego pliku, aby określić ustawienia, takie jak parametry połączenia do innych usług platformy Azure. Dodaj nowy klucz do **wartości** tablicy dla każdego połączenia wymagany przez funkcje w projekcie. Aby uzyskać więcej informacji, zobacz [pliku ustawień lokalnych](functions-run-local.md#local-settings-file) w temacie Azure funkcje podstawowe narzędzia.

Środowisko uruchomieniowe Functions używa konta usługi Azure Storage wewnętrznie. W przypadku wyzwolenia wszystkich typów innych niż HTTP i elementów webhook, należy skonfigurować **Values.AzureWebJobsStorage** klucza do prawidłowego ciągu połączenia konta magazynu Azure.

[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

 Aby ustawić parametry połączenia konta magazynu:

1. W programie Visual Studio Otwórz **Eksplorator chmury**, rozwiń węzeł **konta magazynu** > **Twoje konto magazynu**, a następnie wybierz pozycję **właściwości**i skopiuj **parametry połączenia podstawowej** wartość.   

2. W projekcie, otwórz plik local.settings.json i ustaw wartość **AzureWebJobsStorage** skopiować klucza w parametrach połączenia.

3. Powtórz poprzedni krok, aby dodać unikatowy kluczy do **wartości** tablicy dla innych połączeń wymagany przez funkcje.  

## <a name="create-a-function"></a>Tworzenie funkcji

W przypadku funkcji wstępnie skompilowanym powiązania używane przez funkcję są definiowane przez stosowanie atrybutów w kodzie. Korzystając z narzędzi funkcji Azure można utworzyć funkcji z podanych szablonów, te atrybuty są stosowane dla Ciebie. 

1. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy węzeł projektu i wybierz polecenie **Dodaj** > **Nowy element**. Wybierz **funkcji platformy Azure**, wpisz **nazwa** klasy, a następnie kliknij przycisk **Dodaj**.

2. Wybierz wyzwalacz, ustaw właściwości powiązania, a następnie kliknij przycisk **Utwórz**. W poniższym przykładzie przedstawiono ustawienia wyzwolenia tworzenia magazynu kolejek funkcji. 

    ![](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)
    
    Klucz ciąg połączenia o nazwie **QueueStorage** zostanie podany, która jest zdefiniowana w pliku local.settings.json. 
 
3. Sprawdź, czy nowo dodanego klasy. Zobacz statycznego **Uruchom** — metoda, która ma atrybut **FunctionName** atrybutu. Ten atrybut wskazuje, że metoda jest punkt wejścia dla funkcji. 

    Na przykład następujące klasy C# reprezentuje podstawowych funkcji magazynu wyzwalane kolejki:

    ````csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    
    namespace FunctionApp1
    {
        public static class Function1
        {
            [FunctionName("QueueTriggerCSharp")]        
            public static void Run([QueueTrigger("myqueue-items", Connection = "QueueStorage")]string myQueueItem, TraceWriter log)
            {
                log.Info($"C# Queue trigger function processed: {myQueueItem}");
            }
        }
    } 
    ````
 
    Atrybut specyficzne dla powiązania jest stosowany do każdego parametru wiązania dostarczony do metody punktu wejścia. Ten atrybut ma informacje o powiązaniu jako parametry. W poprzednim przykładzie pierwszy parametr ma **QueueTrigger** atrybut, wskazujący funkcję kolejki wyzwolone. Nazwa kolejki i nazwa ustawienie parametrów połączenia są przekazywane jako parametry **QueueTrigger** atrybutu.

## <a name="testing-functions"></a>Testowanie funkcji

Podstawowe narzędzia usługi Azure Functions umożliwiają uruchamianie projektu usługi Azure Functions na lokalnym komputerze deweloperskim. Monit o zainstalowanie tych narzędzi pojawia się przy pierwszym uruchomieniu funkcji w programie Visual Studio.  

Aby przetestować funkcję, naciśnij klawisz F5. Po wyświetleniu monitu zaakceptuj żądanie programu Visual Studio dotyczące pobrania i zainstalowania podstawowych narzędzi usługi Azure Functions (CLI).  Może także być konieczne włączenie wyjątku zapory, aby umożliwić narzędziom obsługę żądań HTTP.

W projekcie jest uruchomiona można przetestować kodu jako może przetestować wdrożonej funkcji. Aby uzyskać więcej informacji, zobacz [strategii do testowania kodu w usługi Azure Functions](functions-test-a-function.md). Podczas uruchamiania w trybie debugowania, punkty przerwania są osiągane w programie Visual Studio, zgodnie z oczekiwaniami. 

Na przykład jak przetestować funkcję kolejki wyzwalane zobacz [samouczek Szybki Start — funkcja kolejki wyzwalane](functions-create-storage-queue-triggered-function.md#test-the-function).  

Aby dowiedzieć się więcej o korzystaniu z narzędzi podstawowych funkcji Azure, zobacz [kodu i przetestuj usługę Azure functions lokalnie](functions-run-local.md).

## <a name="publish-to-azure"></a>Publikowanie na platformie Azure

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

>[!NOTE]  
>Wszystkie ustawienia, które zostały dodane w local.settings.json można również dodać do aplikacji funkcji na platformie Azure. Te ustawienia nie są automatycznie dodawane. Wymagane ustawienia można dodać do aplikacji funkcji w jednym z następujących sposobów:
>
>* [Przy użyciu portalu Azure](functions-how-to-use-azure-function-app-settings.md#settings).
>* [Przy użyciu `--publish-local-settings` opcja publikowania w Azure funkcje podstawowe narzędzia](functions-run-local.md#publish).
>* [Przy użyciu interfejsu wiersza polecenia platformy Azure](/cli/azure/functionapp/config/appsettings#set). 

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat narzędzi funkcji Azure, zobacz sekcję Często zadawane pytania [2017 Visual Studio Tools dla usługi Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) wpis w blogu.

Aby dowiedzieć się więcej o podstawowych narzędzi funkcji platformy Azure, zobacz [kodu i przetestuj usługę Azure functions lokalnie](functions-run-local.md).  
Aby dowiedzieć się więcej o wdrażaniu funkcji jako biblioteki klas .NET, zobacz [dokumentacja dla deweloperów Azure funkcje C#](functions-dotnet-class-library.md). Ten temat zawiera również linki do przykłady tego, jak używać atrybutów, aby zadeklarować różnych typów powiązania obsługiwane przez usługi Azure Functions.    
