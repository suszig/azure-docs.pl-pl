---
title: "Debugowanie usługi Azure Functions krawędzi IoT Azure przy użyciu programu Visual Studio Code | Dokumentacja firmy Microsoft"
description: "Debugowanie C# usługę Azure Functions Azure IoT krawędzi w kodzie VS"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/20/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4344a450d218a7424cd055cf086c1e4865c9af10
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Użyj programu Visual Studio kodu do debugowania usługi Azure Functions krawędzi IoT Azure

Ten artykuł zawiera szczegółowe instrukcje dotyczące używania [Visual Studio Code](https://code.visualstudio.com/) jako Narzędzia główne programowanie debugowania funkcji Azure IoT krawędzi.

## <a name="prerequisites"></a>Wymagania wstępne
Ten samouczek zakłada, że używasz komputera lub maszyny wirtualnej z systemem Windows lub Linux jako komputerze deweloperskim. Urządzenia IoT krawędzi może być inne fizyczne urządzenie lub urządzenia IoT krawędzi można symulować na komputerze deweloperskim.

Upewnij się, że masz następujące samouczki ukończone przed rozpoczęciem tej wskazówki.
- [Użyj programu Visual Studio Code do opracowywania i wdrażania usługi Azure Functions do krawędzi IoT Azure](how-to-vscode-develop-azure-function.md)

Po Zakończ poprzedni Samouczek, powinien mieć następujące elementy gotowy,
- Lokalnego rejestru Docker uruchomiony na komputerze deweloperskim. Zalecane jest korzystanie lokalnego rejestru Docker prototypu i celów testowych.
- `run.csx` Pliku kodem najnowszych funkcji filtru.
- Zaktualizowano `deployment.json` pliku modułu czujnik i moduł funkcji filtru.
- Uruchomiony na komputerze deweloperskim środowiska uruchomieniowego krawędzi.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Tworzenie modułu krawędzi IoT cel debugowania
1. Aby rozpocząć debugowanie, należy użyć **dockerfile.debug** Aby ponownie skompilować obraz docker i wykonaj ponowne wdrożenie rozwiązania krawędzi. W kodzie VS explorer kliknij folder Docker, aby go otworzyć. Następnie kliknij przycisk `linux-x64` folderu, kliknij prawym przyciskiem myszy **Dockerfile.debug**i kliknij przycisk **krawędzi IoT Tworzenie modułu Docker obrazu**.
3. W **wybierz Folder** okna, przejdź do **funkcji FilterFunction** projekt i kliknij przycisk **wybierz Folder jako EXE_DIR**.
4. W polu tekstowym wyskakujących w górnej części okna kodu programu VS wprowadź nazwę obrazu. Na przykład: `<your container registry address>/filterfunction:latest`. Jeśli są wdrażane w lokalnym rejestrze, powinien być `localhost:5000/filterfunction:latest`.
5. Wypchnij obrazu do repozytorium Docker. Użyj **krawędzi: Push krawędzi IoT modułu Docker obrazu** polecenia i wprowadź adres URL obrazu, w polu tekstowym wyskakujących w górnej części okna kodu programu VS. Użyj używany w ponad krok ten sam adres URL obrazu.
6. Można użyć ponownie `deployment.json` można wdrożyć ponownie. W poleceniu palety, wpisz i wybierz **krawędzi: Uruchom ponownie krawędzi** uzyskanie funkcji Filtr z wersji do debugowania.

## <a name="start-debugging-in-vs-code"></a>Rozpocznij debugowanie w kodzie VS
1. Przejdź do okna debugowania kodu programu VS. Naciśnij klawisz **F5** i wybierz **IoT Edge(.Net Core)**
2. W `launch.json`, przejdź do **debugowania funkcji krawędzi IoT (.NET Core)** sekcji, a następnie wypełnij `<container_name>`w obszarze `pipeArgs`. Powinien być `filterfunction` w tym samouczku.
3. Przejdź do run.csx. Dodaj punkt przerwania w funkcji.
4. Naciśnij klawisz **F5** ponownie. I wybierz można dołączyć do procesu.
5. W oknie debugowania kodu VS widać zmiennych w lewym panelu. 

> [!NOTE]
> Powyżej przykładzie przedstawiono sposób debugowania .net funkcja krawędzi IoT Core kontenerów. Jest on oparty na wersji do debugowania z `Dockerfile.debug`, w tym VSDBG (debuger wiersza polecenia platformy .NET Core) w obrazie kontenera podczas jego tworzenia. Firma Microsoft zaleca, możesz bezpośrednio użyć lub dostosować `Dockerfile` bez VSDBG dla funkcji krawędzi IoT gotowe do produkcji, po zakończeniu debugowania funkcji języka C#.

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku zostało utworzone funkcji platformy Azure i wdrożyć go do krawędzi IoT w celu debugowania i uruchomić debugowanie go w kodzie VS. Można kontynuować do jednej z następujących samouczków, aby dowiedzieć się więcej o innych scenariuszy podczas opracowywania Azure IoT krawędź w kodzie VS. 

> [!div class="nextstepaction"]
> [Tworzenie i wdrażanie modułu C# w kodzie VS](how-to-vscode-develop-csharp-module.md)

