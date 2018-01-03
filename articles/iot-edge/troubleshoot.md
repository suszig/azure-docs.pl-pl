---
title: "Rozwiązywanie problemów z Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie typowych problemów i Dowiedz się umiejętności dotyczące rozwiązywania problemów Edge IoT Azure"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 3f61f0bf8234e747ae38146d1a5ea030e3163fa3
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Najczęściej występujących problemów i rozwiązań Edge IoT Azure

Występują problemy z systemem Azure IoT krawędzi w danym środowisku, należy użyć w tym artykule przedstawiono wskazówki do rozwiązywania problemów i rozwiązania. 

## <a name="standard-diagnostic-steps"></a>Standardowe czynności diagnostycznych 

W przypadku wystąpienia problemu, Dowiedz się więcej o stan urządzenia IoT krawędzi, przeglądając dzienniki kontenera i komunikaty, które przejdą do i z urządzenia. Użyj polecenia i narzędzia w tej sekcji, aby zebrać informacje. 

* Sprawdź dzienniki kontenery docker opcję wykrywania problemów. Rozpoczynać się od wdrożonego kontenerów, a następnie przyjrzeć się kontenerów, które składają się na krawędzi IoT środowiska uruchomieniowego: krawędź Agent i krawędzi koncentratora. Dzienniki krawędź Agent zwykle zawierają informacje o cyklu życia każdego kontenera. Dzienniki Centrum krawędzi zawierają informacje o obsługi wiadomości i routing. 

   ```cmd
   docker logs <container name>
   ```

* Wyświetlanie komunikatów pośrednictwa Centrum krawędzi i zbierać szczegółowych informacji o aktualizacji właściwości urządzenia z dziennikami verbose z kontenerów środowiska wykonawczego.

   ```cmd
   iotedgectl setup --runtime-log-level DEBUG
   ```

* Jeśli występują problemy z łącznością, należy sprawdzić zmiennych środowiskowych urządzenia krawędzi, tak jak parametry połączenia urządzenia:

   ```cmd
   docker exec edgeAgent printenv
   ```

Można również sprawdzić komunikaty wysyłane między centrum IoT i urządzeniami IoT krawędzi. Te komunikaty można wyświetlać przy użyciu [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) rozszerzenia dla programu Visual Studio Code. Aby uzyskać więcej pomocy, zobacz [przydatne narzędzie podczas opracowywania Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

Po zbadaniu dzienniki i komunikaty, aby uzyskać informacje, można też spróbować uruchomić środowiska uruchomieniowego krawędzi IoT Azure:

   ```cmd
   iotedgectl restart
   ```

## <a name="edge-agent-stops-after-about-a-minute"></a>Krawędź Agent przestaje po około minuty

Krawędź Agent uruchamia jest uruchomiony pomyślnie około minuty, a następnie zatrzymuje. Dzienniki wskazują, że Agent krawędzi próbuje nawiązać połączenia z Centrum IoT za pośrednictwem protokołu AMQP, i następnie próbę połączenia za pomocą protokołu AMQP za pośrednictwem protokołu websocket około 30 sekund. Jeśli się nie powiedzie, zamyka agenta krawędzi. 

Przykład krawędź Agent dzienniki:

```
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Przyczyna główna
Konfiguracji sieci w sieci hosta uniemożliwia agenta krawędzi nawiązywania połączenia z siecią. Agent próbuje najpierw nawiązywać połączeń za pośrednictwem protokołu AMQP (port: 5671). W przypadku niepowodzenia podejmuje websocket (port 443).

Środowisko uruchomieniowe krawędzi IoT konfiguruje sieci dla poszczególnych modułów do komunikacji w. W systemie Linux ta sieć jest siecią mostek. W systemie Windows używa translatora adresów sieciowych. Ten problem występuje częściej na urządzeniach z systemem Windows przy użyciu kontenerów systemu Windows, które korzystają z sieci translatora adresów Sieciowych. 

### <a name="resolution"></a>Rozwiązanie
Upewnij się, że istnieje trasa do Internetu dla adresów IP przypisanych do tej sieci Mostek/translatora adresów Sieciowych. Konfiguracja sieci VPN, na hoście przesłania czasami sieci IoT krawędzi. 

## <a name="edge-hub-fails-to-start"></a>Krawędź Centrum nie można uruchomić

Koncentrator krawędzi kończy się niepowodzeniem, aby uruchomić i odbitek następujące wiadomości w dziennikach: 

```
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Przyczyna główna
Niektóre inny proces na komputerze hosta jest związana portu 443. Koncentrator krawędzi mapuje porty 5671 i 443 dla korzystać w scenariuszach bramy. To mapowanie portu kończy się niepowodzeniem, jeśli inny proces jest już powiązany tego portu. 

### <a name="resolution"></a>Rozwiązanie
Znajdź i zatrzymać proces, który jest przy użyciu portu 443. Ten proces jest zwykle serwer sieci web.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>Krawędź Agent nie ma dostępu do modułu obrazu (403)
Kontener nie powiedzie się, a w dziennikach krawędź Agent znajdują się błędu 403. 

### <a name="root-cause"></a>Przyczyna główna
Krawędź Agent nie ma uprawnień dostępu modułu obrazu. 

### <a name="resolution"></a>Rozwiązanie
Spróbuj uruchomić `iotedgectl login` polecenie ponownie.

## <a name="next-steps"></a>Kolejne kroki
Twoja opinia znaleźć błąd krawędzi IoT platformy? Wykonaj [przesłać problemu](https://github.com/Azure/iot-edge/issues) tak, aby móc dalej poprawy. 
