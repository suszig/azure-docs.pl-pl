---
title: "Debugowanie modułu C# z krawędzią IoT Azure przy użyciu programu Visual Studio Code | Dokumentacja firmy Microsoft"
description: "Debugowanie modułu C# przy użyciu krawędź IoT Azure w programie Visual Studio Code."
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5ed517cf8d70cd279a55b79ad448709116cf511b
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="use-visual-studio-code-to-debug-a-c-module-with-azure-iot-edge"></a>Debugowanie modułu C# z krawędzią IoT Azure przy użyciu programu Visual Studio Code
Ten artykuł zawiera szczegółowe instrukcje dotyczące używania [Visual Studio Code](https://code.visualstudio.com/) jako Narzędzia główne programowanie debugowania moduły Azure IoT krawędzi.

## <a name="prerequisites"></a>Wymagania wstępne
Ten samouczek zakłada, że używasz komputera lub maszyny wirtualnej z systemem Windows lub Linux jako komputerze deweloperskim. Urządzenie brzegowe IoT może być inne urządzenie fizyczne lub urządzenia IoT krawędzi można symulować na komputerze deweloperskim.

Przed rozpoczęciem tej wskazówki Ukończ samouczek następujące:
- [Użyj programu Visual Studio Code do opracowywania C# modułu krawędzi IoT Azure](how-to-vscode-develop-csharp-module.md)

Po zakończeniu samouczka poprzedniego powinny mieć gotowy następujące elementy:
- Lokalnego rejestru Docker uruchomiony na komputerze deweloperskim. Jest to prototypowania i testowania.
- `Program.cs` Pliku z najnowszą kod modułu filtru.
- Zaktualizowano `deployment.json` pliku moduły czujników i filtru.
- Krawędź IoT środowiska uruchomieniowego uruchomioną na komputerze deweloperskim.

## <a name="build-your-iot-edge-module-for-debugging"></a>Tworzenie modułu krawędzi IoT do debugowania
1. Aby rozpocząć debugowanie, użyj **dockerfile.debug** Aby ponownie skompilować obraz Docker i wykonaj ponowne wdrożenie rozwiązania IoT krawędzi. W Eksploratorze kodu programu Visual Studio wybierz folder Docker, aby go otworzyć. Następnie wybierz **linux x64** folderu, kliknij prawym przyciskiem myszy **Dockerfile.debug**i wybierz **krawędzi IoT Tworzenie modułu Docker obrazu**.

    ![Zrzut ekranu przedstawiający Eksplorator kodu VS](./media/how-to-debug-csharp-module/build-debug-image.png)

3. W **wybierz Folder** okna, przejdź do albo wprowadź **./bin/Debug/netcoreapp2.0/publish**. Następnie wybierz **wybierz Folder jako EXE_DIR**.
4. W polu tekstowym wyskakujących w górnej części okna kodu programu VS wprowadź nazwę obrazu. Na przykład: `<your container registry address>/filtermodule:latest`. Jeśli są wdrażane w lokalnym rejestrze, należy go: `localhost:5000/filtermodule:latest`.
5. Wypchnij obrazu do repozytorium Docker. Użyj **krawędzi: Push krawędzi IoT modułu Docker obrazu** polecenia i wprowadź adres URL obrazu, w polu tekstowym wyskakujących w górnej części okna kodu programu VS. Użyj tego samego adresu URL obraz używany w poprzednim kroku.
6. Można użyć ponownie `deployment.json` można wdrożyć ponownie. W palecie polecenia, wpisz i wybierz **krawędzi: Uruchom ponownie krawędzi** uzyskać modułu filtru uruchomiony z wersji do debugowania.

## <a name="start-debugging-in-vs-code"></a>Rozpocznij debugowanie w kodzie VS
1. Przejdź do okna debugowania kodu programu VS. Naciśnij klawisz **F5**i wybierz **IoT Edge(.NET Core)**.

    ![Zrzut ekranu kodzie VS debugowania okna](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. W `launch.json`, przejdź do **debugowania IoT krawędzi niestandardowego modułu (.NET Core)** sekcji. W obszarze **pipeArgs**, wypełnij `<container_name>`. Powinien być `filtermodule` w tym samouczku.

    ![Zrzut ekranu przedstawiający VS kodu launch.json](./media/how-to-debug-csharp-module/add-container-name.png)

3. Przejdź do **Program.cs**. Dodaj punkt przerwania w `method static async Task<MessageResponse> FilterModule(Message message, object userContext)`.
4. Naciśnij klawisz **F5** ponownie i wybierz można dołączyć do procesu. W tym samouczku powinna być nazwa procesu `FilterModule.dll`.

    ![Zrzut ekranu kodzie VS debugowania okna](./media/how-to-debug-csharp-module/attach-process.png)

5. W oknie debugowania kodu VS widać zmiennych w lewym panelu. 

> [!NOTE]
> Poprzednim przykładzie pokazano, jak można debugować modułów krawędzi IoT Core .NET do kontenerów. Jest on oparty na wersji do debugowania z `Dockerfile.debug`, w tym VSDBG (debuger wiersza polecenia platformy .NET Core) w obrazie kontenera podczas jego tworzenia. Po zakończeniu debugowania modułów C#, zaleca się bezpośrednio użyć lub dostosować `Dockerfile` bez VSDBG dla modułów krawędzi IoT gotowe do produkcji.

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku utworzony moduł krawędzi IoT i wdrożyć ją do debugowania. Uruchomiono jej debugowanie w kodzie VS. Aby dowiedzieć się o innych scenariuszy podczas projektowania usługi Azure IoT krawędź w kodzie VS, zobacz: 

> [!div class="nextstepaction"]
> [Tworzenie i wdrażanie modułu C# w kodzie VS](how-to-vscode-develop-csharp-module.md)
