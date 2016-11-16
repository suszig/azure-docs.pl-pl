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
ms.date: 08/25/2016
ms.author: andbuc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 23176a9251a90a985a5d2fbce23ceeb9d0925234


---
# <a name="azure-iot-gateway-sdk-beta-get-started-using-linux"></a>Zestaw SDK bramy usługi Azure IoT (wersja beta) — wprowadzenie przy użyciu systemu Linux
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
   
    Skrypt kompilacji umieszcza plik **liblogger_hl.so** w folderze **build/modules/logger/**, a plik **libhello_world_hl.so** w folderze **build/modules/hello_world/**. Użyj tych ścieżek dla wartości **module path**, jak pokazano poniżej w pliku ustawień JSON.
2. Plik **hello_world_lin.json** w folderze **samples/hello_world/src** to przykładowy plik ustawień JSON dla systemu Linux, którego można użyć do uruchomienia przykładu. W przykładowych ustawieniach JSON przedstawionych poniżej przyjęto założenie, że przykład zostanie uruchomiony z katalogu głównego lokalnej kopii repozytorium **azure-iot-gateway-sdk**.
3. Dla modułu **logger_hl** w sekcji **args** ustaw wartość elementu **filename** na ścieżkę do pliku i nazwę pliku, który będzie zawierać dane dziennika.
   
    Jest to przykładowy plik ustawień JSON dla systemu Linux, który będzie zapisywał dane w pliku **log.txt** w folderze, z poziomu którego zostanie uruchomiony przykład.
   
    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "loading args": {
            "module path" : "./build/modules/logger/liblogger_hl.so"
          },
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "loading args": {
            "module path" : "./build/modules/hello_world/libhello_world_hl.so"
          },
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```
4. W powłoce przejdź do folderu **azure-iot-gateway-sdk**.
5. Uruchom następujące polecenie:
   
   ```
   ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
   ``` 

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Nov16_HO2-->


