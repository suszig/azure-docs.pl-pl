---
title: "Wprowadzenie do zestawu SDK bramy usługi Azure IoT (Linux) | Microsoft Docs"
description: "Informacje o sposobie tworzenia bramy na komputerze z systemem Linux oraz o najważniejszych pojęciach dotyczących zestawu SDK bramy usługi Azure IoT, takich jak moduły i pliki konfiguracji JSON."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 856ffeeeb8f9d8296ba972a9e070686171f7fde8
ms.lasthandoff: 03/31/2017


---
# <a name="explore-the-iot-gateway-sdk-architecture-on-linux"></a>Eksplorowanie architektury zestawu SDK bramy IoT w systemie Linux

[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Jak skompilować przykład

Przed rozpoczęciem należy [skonfigurować środowisko programistyczne][lnk-setupdevbox] do pracy z zestawem SDK w systemie Linux.

1. Otwórz powłokę.
1. Przejdź do folderu głównego w lokalnej kopii repozytorium **azure-iot-gateway-sdk**.
1. Uruchom skrypt **tools/build.sh**. Ten skrypt używa narzędzia **cmake** do utworzenia folderu o nazwie **build** w folderze głównym lokalnej kopii repozytorium **azure-iot-gateway-sdk** i wygenerowania pliku reguł programu make. Skrypt następnie kompiluje rozwiązanie, pomijając testy jednostkowe i testy kompleksowe. Dodaj parametr **--run-unittests**, jeśli chcesz skompilować rozwiązanie i uruchomić testy jednostkowe. Dodaj parametr **--run-e2e-tests**, jeśli chcesz skompilować rozwiązanie i uruchomić testy kompleksowe.

> [!NOTE]
> Za każdym razem, gdy zostanie uruchomiony skrypt **build.sh**, folder **build** jest usuwany i tworzony ponownie w folderze głównym lokalnej kopii repozytorium **azure-iot-gateway-sdk**.

## <a name="how-to-run-the-sample"></a>Jak uruchomić przykład

1. Skrypt **build.sh** generuje swoje dane wyjściowe w folderze **build** w lokalnej kopii repozytorium **azure-iot-gateway-sdk**. Dane wyjściowe obejmują dwa moduły używane w tym przykładzie.

    Skrypt kompilacji umieszcza plik **liblogger.so** w folderze **build/modules/logger/**, a plik **libhello\_world.so** w folderze **build/modules/hello_world/**. Użyj tych ścieżek dla wartości **module path**, jak pokazano poniżej w przykładowym pliku ustawień JSON.
1. Proces hello\_world\_sample przyjmuje ścieżkę do pliku konfiguracji JSON jako argument w wierszu polecenia. Następujący przykład pliku JSON jest udostępniany w repozytorium zestawu SDK w pliku **samples/hello\_world/src/hello\_world\_lin.json**. Ten plik konfiguracji będzie działać bez wprowadzania zmian, o ile nie zmodyfikowano skryptu kompilacji w celu umieszczenia modułów lub przykładowych plików wykonywalnych w lokalizacjach innych niż domyślne.

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
1. Przejdź do folderu **build**.
1. Uruchom następujące polecenie:

   `./samples/hello_world/hello_world_sample ./../samples/hello_world/src/hello_world_lin.json`

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md

