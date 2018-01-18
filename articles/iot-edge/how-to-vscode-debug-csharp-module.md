---
title: "Debugowanie modułu C# z krawędzią IoT Azure przy użyciu programu Visual Studio Code | Dokumentacja firmy Microsoft"
description: "Debugowanie modułu C# przy użyciu Azure IoT krawędź w kodzie VS"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 1ab67cd8aaf59cde3157fcb877ce13f10cb432bb
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="use-visual-studio-code-to-debug-c-module-with-azure-iot-edge"></a>Za pomocą programu Visual Studio Code do debugowania C# modułu krawędzi IoT Azure
Ten artykuł zawiera szczegółowe instrukcje dotyczące używania [Visual Studio Code](https://code.visualstudio.com/) jako Narzędzia główne programowanie debugowania moduły IoT krawędzi.

## <a name="prerequisites"></a>Wymagania wstępne
Ten samouczek zakłada, że używasz komputera lub maszyny wirtualnej z systemem Windows lub Linux jako komputerze deweloperskim. Urządzenia IoT krawędzi może być inne fizyczne urządzenie lub urządzenia IoT krawędzi można symulować na komputerze deweloperskim.

Upewnij się, że masz następujące samouczki ukończone przed rozpoczęciem tej wskazówki.
- [Użyj programu Visual Studio Code do opracowywania C# modułu krawędzi IoT Azure](how-to-vscode-develop-csharp-module.md)

Po Zakończ poprzedni Samouczek, powinien mieć następujące elementy gotowy,
- Lokalnego rejestru Docker uruchomiony na komputerze deweloperskim. Zalecane jest korzystanie lokalnego rejestru Docker prototypu i celów testowych.
- `Program.cs` Pliku z najnowszą kod modułu filtru.
- Zaktualizowano `deployment.json` pliku modułu czujnik i modułu filtru.
- Uruchomiony na komputerze deweloperskim środowiska uruchomieniowego krawędzi.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Tworzenie modułu krawędzi IoT cel debugowania
1. Aby rozpocząć debugowanie, należy użyć **dockerfile.debug** Aby ponownie skompilować obraz docker i wykonaj ponowne wdrożenie rozwiązania krawędzi. W kodzie VS explorer kliknij folder Docker, aby go otworzyć. Następnie kliknij przycisk `linux-x64` folderu, kliknij prawym przyciskiem myszy **Dockerfile.debug**i kliknij przycisk **krawędzi IoT Tworzenie modułu Docker obrazu**.

    ![Utwórz obraz do debugowania](./media/how-to-debug-csharp-module/build-debug-image.png)

3. W **wybierz Folder** okna, przejdź do albo wprowadź `./bin/Debug/netcoreapp2.0/publish`. Kliknij przycisk **wybierz Folder jako EXE_DIR**.
4. W polu tekstowym wyskakujących w górnej części okna kodu programu VS wprowadź nazwę obrazu. Na przykład: `<your container registry address>/filtermodule:latest`. Jeśli są wdrażane w lokalnym rejestrze, powinien być `localhost:5000/filtermodule:latest`.
5. Wypchnij obrazu do repozytorium Docker. Użyj **krawędzi: Push krawędzi IoT modułu Docker obrazu** polecenia i wprowadź adres URL obrazu, w polu tekstowym wyskakujących w górnej części okna kodu programu VS. Użyj używany w ponad krok ten sam adres URL obrazu.
6. Można użyć ponownie `deployment.json` można wdrożyć ponownie. W poleceniu palety, wpisz i wybierz **krawędzi: Uruchom ponownie krawędzi** uzyskać modułu filtru uruchomiony z wersji do debugowania.

## <a name="start-debugging-in-vs-code"></a>Rozpocznij debugowanie w kodzie VS
1. Przejdź do okna debugowania kodu programu VS. Naciśnij klawisz **F5** i wybierz **IoT Edge(.Net Core)**

    ![Naciśnij klawisz F5](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. W `launch.json`, przejdź do **debugowania IoT krawędzi niestandardowego modułu (.NET Core)** sekcji, a następnie wypełnij `<container_name>`w obszarze `pipeArgs`. Powinien być `filtermodule` w tym samouczku.

    ![Modyfikowanie pipeArgs](./media/how-to-debug-csharp-module/f5-debug-option.png)

3. Przejdź do pliku Program.cs. Dodaj punkt przerwania w `method static async Task<MessageResponse> FilterModule(Message message, object userContext)`.
4. Naciśnij klawisz **F5** ponownie. I wybierz można dołączyć do procesu. W tym samouczku powinna być nazwa procesu`FilterModule.dll`

    ![Dołączanie procesu](./media/how-to-debug-csharp-module/attach-process.png)

5. W oknie debugowania kodu VS widać zmiennych w lewym panelu. 

> [!NOTE]
> Powyżej przykładzie przedstawiono sposób debugowania .net krawędzi IoT Core modułów do kontenerów. Jest on oparty na wersji do debugowania z `Dockerfile.debug`, w tym VSDBG (debuger wiersza polecenia platformy .NET Core) w obrazie kontenera podczas jego tworzenia. Firma Microsoft zaleca, możesz bezpośrednio użyć lub dostosować `Dockerfile` bez VSDBG dla modułów krawędzi IoT gotowe do produkcji, po zakończeniu debugowania modułów C#.

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku utworzony moduł krawędzi IoT i wdrożyć go w celu debugowania i uruchomić debugowanie go w kodzie VS. Można kontynuować do jednej z następujących samouczków, aby dowiedzieć się więcej o innych scenariuszy podczas opracowywania Azure IoT krawędź w kodzie VS. 

> [!div class="nextstepaction"]
> [Tworzenie i wdrażanie modułu C# w kodzie VS](how-to-vscode-develop-csharp-module.md)
