---
title: "Wprowadzenie do zestawu SDK bramy usługi IoT Hub | Microsoft Docs"
description: "W tym przewodniku po zestawie SDK bramy usługi Azure IoT użyto systemu Linux, aby zilustrować kluczowe założenia, które należy rozumieć w przypadku używania zestawu SDK bramy usługi Azure IoT."
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
ms.date: 11/23/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: a76320718f0cefa015728cb79df944e0d34bbf74
ms.openlocfilehash: cbb909adc2d29f9b80a4c97d06176fe74b64a75a


---
# <a name="azure-iot-gateway-sdk---get-started-using-linux"></a>Zestaw SDK bramy usługi Azure IoT — wprowadzenie przy użyciu systemu Linux
[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Jak skompilować przykład
Przed rozpoczęciem należy [skonfigurować środowisko programistyczne][lnk-setupdevbox] do pracy z zestawem SDK w systemie Linux.

1. Otwórz powłokę.
2. Przejdź do folderu głównego w lokalnej kopii repozytorium **azure-iot-gateway-sdk**.
3. Uruchom skrypt **tools/build.sh**. Ten skrypt używa narzędzia **cmake** do utworzenia folderu o nazwie **build** w folderze głównym lokalnej kopii repozytorium **azure-iot-gateway-sdk** i wygenerowania pliku reguł programu make. Skrypt następnie kompiluje rozwiązania i uruchamia testy.

> [!NOTE]
> Za każdym razem, gdy zostanie uruchomiony skrypt **build.sh**, folder **build** jest usuwany i tworzony ponownie w folderze głównym lokalnej kopii repozytorium **azure-iot-gateway-sdk**.
> 
> 

## <a name="how-to-run-the-sample"></a>Jak uruchomić przykład
1. Skrypt **build.sh** generuje swoje dane wyjściowe w folderze **build** w lokalnej kopii repozytorium **azure-iot-gateway-sdk**. Obejmuje to dwa moduły używane w tym przykładzie.
   
    Skrypt kompilacji umieszcza plik **liblogger.so** w folderze **build/modules/logger/**, a plik **libhello_world.so** w folderze **build/modules/hello_world/**. Użyj tych ścieżek dla wartości **module path**, jak pokazano poniżej w pliku ustawień JSON.
2. Proces hello_world_sample przyjmuje ścieżkę do pliku konfiguracji JSON jako argument w wierszu polecenia. Przykładowy plik JSON został udostępniony w repozytorium: **azure-iot-gateway-sdk/samples/hello_world/src/hello_world_win.json**, a także skopiowany poniżej. Będzie działał bez wprowadzania zmian, o ile nie zmodyfikowano skryptu kompilacji w celu umieszczenia modułów lub przykładowych plików wykonywalnych w lokalizacjach innych niż domyślne.

   > [!NOTE]
   > Względne ścieżki modułów odnoszą się do bieżącego katalogu roboczego, z którego jest uruchamiany plik wykonywalny hello_world_sample, a nie do katalogu, w którym znajduje się ten plik wykonywalny. Przykładowy plik konfiguracji JSON domyślnie zapisuje plik „log.txt” w bieżącym katalogu roboczym.
   
    ```
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
3. Przejdź do folderu **azure-iot-gateway-sdk**.
4. Uruchom następujące polecenie:
   
   ```
   ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
   ``` 

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Nov16_HO4-->


