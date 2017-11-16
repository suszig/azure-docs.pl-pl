---
title: "Zainstaluj krawędzi IoT Azure IoT Core | Dokumentacja firmy Microsoft"
description: "Zainstaluj środowisko uruchomieniowe Azure IoT Edge na urządzeniu Windows IoT Core"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: be2a80645d23e709d6c5cfb3978498bbe85eca34
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Zainstaluj środowisko uruchomieniowe IoT Edge w systemie Windows IoT Core - preview

Środowisko uruchomieniowe krawędzi IoT Azure może działać nawet na niewielki rozmiar urządzenia pojedynczy komputer tablicy (SBC), które są bardzo często spotykane w branży IoT. W tym artykule przedstawiono inicjowania obsługi środowiska uruchomieniowego na [MinnowBoard skarp (Turbot)] [ lnk-minnow] tablicy programowanie z systemem Windows IoT Core.

## <a name="install-the-runtime"></a>Zainstaluj środowisko uruchomieniowe

1. Zainstaluj [pulpitu nawigacyjnego systemu Windows 10 IoT Core] [ lnk-core] w systemie hosta.
1. Postępuj zgodnie z instrukcjami [skonfigurować urządzenie] [ lnk-board] do skonfigurowania tablicy z obrazem MinnowBoard skarp (Turbot) / MAX kompilacji 16299. 
1. Włącz urządzenie, następnie [logowania zdalnie za pomocą programu PowerShell][lnk-powershell].
1. W konsoli programu PowerShell [Zainstaluj wartości binarne Docker][lnk-docker-install].
1. Uruchom następujące polecenie w konsoli programu PowerShell, aby zainstalować środowisko uruchomieniowe krawędzi IoT i Sprawdź konfigurację:

   ```powershell
   Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
   ```

   Ten skrypt zawiera następujące informacje: 
   * 3,6 Python
   * Skrypt kontroli IoT krawędzi (iotedgectl.exe)

Może pojawić się informacyjną danych wyjściowych z narzędzia iotedgectl.exe na czerwono w oknie zdalnego programu PowerShell. To nie musi oznaczać błędy. 

## <a name="next-steps"></a>Następne kroki

Teraz, gdy masz na urządzenie z systemem środowiska uruchomieniowego krawędzi IoT Dowiedz się jak [wdrażanie i monitorowanie krawędzi IoT modułów na dużą skalę][lnk-deploy].

<!--Links-->
[lnk-minnow]: https://minnowboard.org/ 
[lnk-core]: https://docs.microsoft.com/windows/iot-core/connect-your-device/iotdashboard
[lnk-board]: https://developer.microsoft.com/windows/iot/Docs/GetStarted/mbm/sdcard/stable/getstartedstep2
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers