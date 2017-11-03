---
title: "Zainstaluj rozszerzenie funkcji trwałe i przykłady - Azure"
description: "Dowiedz się, jak zainstalować rozszerzenie funkcji trwałe dla usługi Azure Functions do rozwoju portalu lub tworzenia Visual Studio."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: debadde78d937bcd4ec1df665aacfd1887fbcd02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Zainstaluj rozszerzenie funkcji trwałe i przykłady (usługi Azure Functions)

[Trwałe funkcji](durable-functions-overview.md) rozszerzenie dla usługi Azure Functions znajduje się w pakiecie NuGet [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). W tym artykule przedstawiono sposób instalowania pakietu i zestaw przykładów dla następujących środowisk deweloperskich:

* Visual Studio 2017 r (zalecane) 
* Azure Portal

## <a name="visual-studio-2017"></a>Visual Studio 2017

Program Visual Studio udostępnia obecnie najlepsze środowisko dla opracowywania aplikacji korzystających z funkcji trwałe.  Funkcji mogą być uruchamiane lokalnie, a także mogą być publikowane na platformie Azure. Pusty projekt lub zestaw funkcji przykładowe można uruchomić.

### <a name="prerequisites"></a>Wymagania wstępne

* Zainstaluj [najnowszej wersji programu Visual Studio](https://www.visualstudio.com/downloads/) (wersja 15.3 lub nowsza). Zawierają narzędzia Azure w opcjach instalacji.

### <a name="start-with-sample-functions"></a>Uruchom funkcje próbki

1. Pobierz [pliku .zip Przykładowa aplikacja dla programu Visual Studio](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). Nie musisz dodać odwołanie NuGet, ponieważ już ma przykładowy projekt.
2. Zainstaluj i uruchom [emulatora magazynu Azure](https://docs.microsoft.com/azure/storage/storage-use-emulator) w wersji 5.2 lub nowszej. Alternatywnie możesz zaktualizować *local.appsettings.json* pliku z rzeczywistych parametrów połączenia usługi Azure Storage.
3. Otwórz projekt w programie Visual Studio 2017 r. 
4. Aby uzyskać instrukcje na temat uruchamiania próbki, rozpoczynać się od [funkcji łańcucha — Witaj próbki sekwencji](durable-functions-sequence.md). Próbki można uruchomić lokalnie lub opublikowane na platformie Azure.

### <a name="start-with-an-empty-project"></a>Rozpoczynać pusty projekt
 
Postępuj zgodnie z instrukcjami sam jak w przypadku począwszy próbki, ale należy wykonać poniższe kroki zamiast pobierania *.zip* pliku:

1. Utwórz projekt aplikacji funkcji.
2. Dodaj następujące odwołanie pakietu NuGet, aby Twoje *.csproj* pliku:

   ```xml
   <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.DurableTask" Version="1.0.0-beta" />
   ```

## <a name="azure-portal"></a>Azure Portal

Jeśli wolisz, można użyć portalu Azure dla rozwoju trwałe funkcji.

### <a name="create-an-orchestrator-function"></a>Tworzenie funkcji programu orchestrator

1. Utwórz nową aplikację funkcji w [functions.azure.com](https://functions.azure.com/signin).
2. Konfigurowanie aplikacji funkcji [Użyj wersji 2.0 runtime](functions-versions.md).
3. Utwórz nową funkcję, a następnie wybierz **trwałe funkcje programu Orchestrator - C#** szablonu.
4. W obszarze **nie zainstalowano rozszerzenia**, kliknij przycisk **zainstalować** do pobrania rozszerzenie z NuGet.org.

### <a name="copy-sample-code-to-the-function-app"></a>Skopiuj przykładowy kod do aplikacji — funkcja

1. Pobierz [DFSampleApp.zip](https://github.com/Azure/azure-functions-durable-extension/raw/master/docfx/files/DFSampleApp.zip) pliku.
2. Przykładowy plik do rozpakowania `D:\home\site\wwwroot` w aplikacji funkcji przy użyciu Kudu lub FTP.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Uruchamianie funkcji łańcucha próbki](durable-functions-sequence.md)
