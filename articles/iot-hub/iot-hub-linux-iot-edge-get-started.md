---
title: "Wprowadzenie do usługi Azure IoT krawędzi (Linux) | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie tworzenia bramy na komputerze z systemem Linux oraz o najważniejszych pojęciach dotyczących usługi Azure IoT Edge, takich jak moduły i pliki konfiguracji JSON."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fb65e3c34d2b2a14370792d8506c13c8c5fb522e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Eksplorowanie architektury usługi Azure IoT Edge w systemie Linux

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Jak uruchomić przykład

Skrypt **build.sh** generuje swoje dane wyjściowe w folderze **build** w lokalnej kopii repozytorium **iot-edge**. Te dane wyjściowe zawiera dwa moduły krawędzi IoT używane w tym przykładzie.

Skrypt kompilacji umieszcza plik **liblogger.so** w folderze **build/modules/logger/**, a plik **libhello\_world.so** w folderze **build/modules/hello_world/**. Użyj tych ścieżek dla **ścieżka modułu** wartości, jak pokazano w przykładzie pliku ustawień JSON.

Witaj\_world\_przykładowy proces przyjmuje ścieżkę do pliku konfiguracji JSON jako argument wiersza polecenia. Następujący przykład pliku JSON jest udostępniany w repozytorium zestawu SDK w pliku **samples/hello\_world/src/hello\_world\_lin.json**. Ten plik konfiguracyjny działa jest chyba że zmodyfikujesz skrypt kompilacji do umieszczenia krawędzi IoT modułów lub przykładowych plików wykonywalnych w innych niż domyślne lokalizacje.

> [!NOTE]
> Względne ścieżki modułów odnoszą się do bieżącego katalogu roboczego, z którego jest uruchamiany plik wykonywalny hello\_world\_sample, a nie do katalogu, w którym znajduje się ten plik wykonywalny. Przykładowy plik konfiguracji JSON domyślnie zapisuje plik „log.txt” w bieżącym katalogu roboczym.

```json
{
    "modules" :
    [
        {
            "name" : "logger",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args" : {"filename":"log.txt"}
        },
        {
            "name" : "hello_world",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/hello_world/libhello_world.so"
            }
            },
            "args" : null
        }
    ],
    "links":
    [
        {
            "source": "hello_world",
            "sink": "logger"
        }
    ]
}
```

1. Przejdź do **kompilacji** folderu w folderze głównym lokalną kopię **krawędzi iot** repozytorium.

1. Uruchom następujące polecenie:

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

