<properties
    pageTitle="Wprowadzenie do zestawu SDK bramy usługi IoT Hub | Microsoft Azure"
    description="W tym przewodniku po zestawie SDK bramy usługi IoT Hub użyto systemu Linux, aby zilustrować kluczowe założenia, które należy rozumieć w przypadku używania zestawu SDK bramy usługi IoT Hub."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>



# Zestaw SDK bramy IoT (wersja beta) — wprowadzenie przy użyciu systemu Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## Jak skompilować przykład

Przed rozpoczęciem należy [skonfigurować środowisko programistyczne][lnk-setupdevbox] do pracy z zestawem SDK w systemie Linux.

1. Otwórz powłokę.
2. Przejdź do folderu głównego w lokalnej kopii repozytorium **azure-iot-gateway-sdk**.
3. Uruchom skrypt **tools/build.sh**. Ten skrypt używa narzędzia **cmake** do utworzenia folderu o nazwie **build** w folderze głównym lokalnej kopii repozytorium **azure-iot-gateway-sdk** i wygenerowania pliku reguł programu make. Skrypt następnie kompiluje rozwiązania i uruchamia testy.

> [AZURE.NOTE]  Za każdym razem, gdy zostanie uruchomiony skrypt **build.sh**, folder **build** jest usuwany i tworzony ponownie w folderze głównym lokalnej kopii repozytorium **azure-iot-gateway-sdk**.

## Jak uruchomić przykład

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
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
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

3. W powłoce przejdź do folderu **azure-iot-gateway-sdk**.
4. Uruchom następujące polecenie:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md



<!--HONumber=Oct16_HO1-->


