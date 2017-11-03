---
title: "Wprowadzenie do usługi Azure IoT krawędzi (system Windows) | Dokumentacja firmy Microsoft"
description: "Jak utworzyć bramę brzegową IoT Azure na komputerze z systemem Windows i Poznaj podstawowe pojęcia dotyczące usługi Azure IoT krawędzi takie jak moduły i pliki konfiguracyjne w formacie JSON."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 9aff3724-5e4e-40ec-b95a-d00df4f590c5
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/29/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 262963250921ceb6f1a2fed0f68c36a9f052b47b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>Eksploruj architektura krawędzi IoT Azure w systemie Windows

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="run-the-sample"></a>Uruchom próbki

**Build.cmd** skrypt generuje dane wyjściowe w **kompilacji** folderu w lokalnej kopii **krawędzi iot** repozytorium. Te dane wyjściowe zawiera wiele plików, ale w tym przykładzie koncentruje się na trzech:
- Dwa moduły krawędzi IoT: **kompilacji\\modułów\\rejestratora\\debugowania\\logger.dll** i **kompilacji\\modułów\\hello_world\\ Debugowanie\\hello\_world.dll**. 
- Plik wykonywalny: **kompilacji\\przykłady\\hello\_world\\debugowania\\hello\_world\_sample.exe**. Ten proces trwa pliku konfiguracji JSON jako argument wiersza polecenia.

Czwarty używanej w tym przykładzie plik nie znajduje się w folderze kompilacji, ale został uwzględniony podczas sklonować repozytorium krawędzi iot:
- Plik konfiguracji JSON: **przykłady\\hello\_world\\src\\hello\_world\_win.json**. Ten plik zawiera ścieżki do dwa moduły. Deklaruje również, gdzie logger.dll zapisuje dane wyjściowe do lokalizacji. Wartość domyślna to **log.txt** w bieżącym katalogu roboczym. 

   >[!NOTE]
   >Jeśli Przenieś modułów próbki lub Dodaj do testowania aktualizacji **module.path** wartości w pliku konfiguracji do dopasowania. Ścieżki modułu są względem katalogu gdzie **hello\_world\_sample.exe** znajduje się. 

Aby uruchomić przykład, wykonaj następujące kroki:

1. Przejdź do **kompilacji** folderu w folderze głównym lokalną kopię **krawędzi iot** repozytorium.

1. Uruchom następujące polecenie:

   ```cmd
   samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
   ```

1. Następujące dane wyjściowe oznacza, że próbki działa prawidłowo:

   ```cmd
   gateway successfully created from JSON
   gateway shall run until ENTER is pressed
   ```
  
1. Naciśnij klawisz **Enter** klawisz, aby zatrzymać proces.


[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
