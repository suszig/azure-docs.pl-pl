---
title: "Dostosowywanie zdalnego rozwiązanie monitorowania - Azure | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera informacje o jak można pobrać kodu źródłowego dla zdalnego wstępnie skonfigurowane rozwiązanie monitorowania."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/17/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: f5d38091b59110859d4376a5cd16a19f24dad65b
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="customize-the-remote-monitoring-preconfigured-solution"></a>Dostosowywanie zdalnego wstępnie skonfigurowane rozwiązanie monitorowania

Ten artykuł zawiera informacje dotyczące sposobu można uzyskać dostępu do kodu źródłowego i Dostosuj ustawienia monitorowania zdalnego wstępnie skonfigurowane rozwiązanie. Artykuł opisuje:

* Repozytoriów GitHub zawierające kod źródłowy i zasoby dla mikrousług, które tworzą wstępnie skonfigurowane rozwiązanie.
* Typowe scenariusze dostosowywania, takie jak dodanie nowego typu urządzenia.

Poniższe wideo przedstawia Omówienie opcji dostosowywania zdalnego wstępnie skonfigurowane rozwiązanie monitorowania:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/How-to-customize-the-Remote-Monitoring-Preconfigured-Solution-for-Azure-IoT/Player]

## <a name="project-overview"></a>Przegląd projektu

### <a name="implementations"></a>Implementacje

Rozwiązanie monitorowania zdalnego ma implementacje .NET i Java. Zarówno implementacje zapewniają podobne funkcje i korzystać z tej samej podstawowej usług Azure. Można znaleźć tutaj najwyższego poziomu repozytoriów GitHub:

* [Rozwiązanie platformy .NET](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Rozwiązanie Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

### <a name="microservices"></a>Mikrousługi

Jeśli interesuje Cię określoną funkcję rozwiązania, możesz uzyskać dostęp repozytoriów GitHub dla każdego poszczególnych mikrousługi. Każdy mikrousługi implementuje inną część funkcjonalności rozwiązania. Aby uzyskać więcej informacji na temat ogólnej architektury, zobacz [monitorowania zdalnego wstępnie architektury rozwiązania](iot-suite-remote-monitoring-sample-walkthrough.md).

Ta tabela zawiera podsumowanie bieżącego dostępność każdego mikrousługi dla każdego języka:

<!-- please add links for each of the repos in the table, you can find them here https://github.com/Azure/azure-iot-pcs-team/wiki/Repositories-->

| Mikrousługi      | Opis | Java | .NET |
| ----------------- | ----------- | ---- | ---- |
| Interfejs użytkownika sieci Web            | Aplikacja sieci Web dla zdalnego rozwiązanie monitorowania. Implementuje interfejs użytkownika za pomocą React.js framework. | [N/A(React.js)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) | [N/A(React.js)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) |
| IoT Hub Manager   | Obsługuje komunikację z Centrum IoT.        | [Available](https://github.com/Azure/iothub-manager-java) | [Available](https://github.com/Azure/iothub-manager-dotnet)   |
| Authentication    |  Zarządza integracji Azure Active Directory.  | Jeszcze niedostępne | [Available](https://github.com/Azure/pcs-auth-dotnet)   |
| Symulacja urządzenia | Zarządza puli symulowane urządzeń. | Jeszcze niedostępne | [Available](https://github.com/Azure/device-simulation-dotnet)   |
| Telemetria         | Udostępnia telemetrii urządzenia do interfejsu użytkownika. | [Available](https://github.com/Azure/device-telemetry-java) | [Available](https://github.com/Azure/device-telemetry-dotnet)   |
| Agent telemetrii   | Analizuje dane telemetryczne strumienia, przechowuje komunikaty z Centrum IoT Azure i generuje alerty, zgodnie ze zdefiniowanymi regułami.  | [Available](https://github.com/Azure/telemetry-agent-java) | [Available](https://github.com/Azure/telemetry-agent-dotnet)   |
| Konfiguracja interfejsu użytkownika         | Zarządza danymi konfiguracji w interfejsie użytkownika. | [Available](https://github.com/azure/pcs-ui-config-java) | [Available](https://github.com/azure/pcs-ui-config-dotnet)   |
| Karta magazynu   |  Zarządza interakcji z usługą magazynu.   | [Available](https://github.com/azure/pcs-storage-adapter-java) | [Available](https://github.com/azure/pcs-storage-adapter-dotnet)   |
| Zwrotny serwer proxy     | Udostępnia zasoby prywatny w sposób zarządzane za pośrednictwem unikatowy punktu końcowego. | Jeszcze niedostępne | [Available](https://github.com/Azure/reverse-proxy-dotnet)   |

Rozwiązanie Java aktualnie używa uwierzytelniania platformy .NET, symulacji i mikrousług zwrotnego serwera proxy. Te mikrousług zostanie zastąpione przez wersje oprogramowania Java, jak tylko staną się dostępne.

## <a name="presentation-and-visualization"></a>Prezentacja i wizualizacji

W poniższych sekcjach opisano opcje, aby dostosować Warstwa prezentacji i wizualizacje w zdalnym rozwiązanie monitorowania:

### <a name="change-the-logo-in-the-ui"></a>Zmiana logo w interfejsie użytkownika

Domyślnym wdrożeniu używa nazwy firmy Contoso i logo w interfejsie użytkownika. Aby zmienić te elementy interfejsu użytkownika, aby wyświetlić nazwę firmy, a logo:

1. Klonowanie repozytorium interfejsu użytkownika sieci Web, użyj następującego polecenia:

    ```cmd/sh
    git clone https://github.com/Azure/pcs-remote-monitoring-webui.git
    ```

1. Aby zmienić nazwę firmy, należy otworzyć `src/common/lang.js` plik w edytorze tekstów.

1. Znajdź następujący wiersz w pliku:

    ```js
    CONTOSO: 'Contoso',
    ```

1. Zastąp `Contoso` o nazwie firmy. Na przykład:

    ```js
    CONTOSO: 'YourCo',
    ```

1. Zapisz plik.

1. Aby zaktualizować logo, Dodaj nowy plik SVG `assets/icons` folderu. Istniejące logo jest `assets/icons/Contoso.svg` pliku.

1. Otwórz `src/components/layout/leftNav/leftNav.js` plik w edytorze tekstów.

1. Znajdź następujący wiersz w pliku:

    ```js
    import ContosoIcon from '../../../assets/icons/Contoso.svg';
    ```

1. Zastąp `Contoso.svg` o nazwie plik z logo. Na przykład:

    ```js
    import ContosoIcon from '../../../assets/icons/YourCo.svg';
    ```

1. Znajdź następujący wiersz w pliku:

    ```js
    alt="ContosoIcon"
    ```

1. Zastąp `ContosoIcon` z Twojej `alt` tekstu. Na przykład:

    ```js
    alt="YourCoIcon"
    ```

1. Zapisz plik.

1. Aby przetestować zmiany, można uruchomić zaktualizowanego `webui` na komputerze lokalnym. Aby dowiedzieć się, jak skompilować i uruchomić `webui` rozwiązania lokalnie, zobacz [uruchomienie kompilacji i testowania lokalnie](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/README.md#build-run-and-test-locally) w `webui` plik readme repozytorium GitHub.

1. Aby wdrożyć zmiany, zobacz [Podręcznik dewelopera](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide).

<!--

### Add a new KPI to the Dashboard page

The following steps describe how to add a new KPI to display on the **Dashboard** page. The new KPI shows information about the number of alarms with specific status values as a pie chart:

1. Step 1

1. Step 2
-->

### <a name="customize-the-map"></a>Dostosowywanie mapy

Zobacz [mapy Dostosuj](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) strony w witrynie GitHub szczegółowe składników mapy w rozwiązaniu.

<!--
### Customize the telemetry chart

See the [Customize telemetry chart](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of the telemetry chart components in the solution.

### Connect an external visualization tool

See the [Connect an external visualization tool](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to connect an external visualization tool.

### Duplicate an existing control

To duplicate an existing UI element such as a chart or alert, see the [Duplicate a control](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub.

-->

### <a name="other-customization-options"></a>Inne opcje dostosowywania

Do modyfikowania Warstwa prezentacji i wizualizacje w zdalnym rozwiązanie monitorowania, należy edytować kod. Odpowiednich repozytoriów GitHub są:

* [UIConfig (.NET)](https://github.com/Azure/pcs-ui-config-dotnet/)
* [UIConfig (Java)](https://github.com/Azure/pcs-ui-config-java/)
* [WebUI zdalnego monitorowania komputerów z platformy Azure](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="device-connectivity-and-streaming"></a>Łączność urządzeń i przesyłania strumieniowego

W poniższych sekcjach opisano opcje, aby dostosować łączność urządzeń i przesyłania strumieniowego warstwie zdalnego rozwiązanie monitorowania. [Modele urządzeń](https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models) opisano typy urządzeń i telemetrii w rozwiązaniu. Możesz użyć modele urządzeń dla urządzeń fizycznych i symulowane.

Na przykład implementacji urządzenia fizycznego zobacz [Podłącz urządzenie do zdalnego wstępnie skonfigurowane rozwiązanie monitorowania](iot-suite-connecting-devices-node.md).

Jeśli używasz _urządzenie fizyczne_, musisz podać aplikacji klienckiej z modelem urządzenia, który zawiera specyfikację metadane i dane telemetryczne urządzenia.

W poniższych sekcjach omówiono przy użyciu urządzeń z symulowanego urządzenia:

### <a name="add-a-telemetry-type"></a>Dodaj typ telemetrii

Typy urządzeń w rozwiązaniu pokaz Contoso Określ dane telemetryczne, która wysyła każdego typu urządzenia. Aby określić typy dodatkowe dane telemetryczne, urządzenie może wysyłać dane telemetryczne definicje jak metadanych do rozwiązania. Jeśli używasz tego formatu, pulpit nawigacyjny wykorzystuje telemetrii urządzenia, a dostępne metody dynamicznie, a nie ma potrzeby modyfikowania interfejsu użytkownika. Alternatywnie można zmodyfikować definicję typu urządzenia w rozwiązaniu.

Aby dowiedzieć się, jak dodać telemetrii niestandardowej w _symulator urządzeń_ mikrousługi, zobacz [przetestowany z urządzeniami symulowane](iot-suite-remote-monitoring-test.md).

### <a name="add-a-device-type"></a>Dodaj typ urządzenia

Rozwiązanie pokaz Contoso definiuje niektóre przykładowe typy urządzeń. Rozwiązanie umożliwia definiowanie typów niestandardowych urządzenia zgodnie z wymaganiami określonej aplikacji. Na przykład firma może używać przemysłowych bramy jako głównej urządzenia podłączone do rozwiązania.

Aby utworzyć dokładną reprezentację urządzenia, należy zmodyfikować aplikację, która działa na urządzeniu, aby dopasować wymagania dotyczące urządzeń.

Aby dowiedzieć się, jak dodać nowego typu urządzenia w _symulator urządzeń_ mikrousługi, zobacz [przetestowany z urządzeniami symulowane](iot-suite-remote-monitoring-test.md).

### <a name="define-custom-methods-for-simulated-devices"></a>Definiowanie niestandardowych metod symulowane urządzeń

Aby dowiedzieć się, jak zdefiniować niestandardowych metod symulowane urządzeń w zdalnym rozwiązanie monitorowania, zobacz [modele urządzeń](https://github.com/Azure/device-simulation-dotnet/wiki/%5BAPI-Specifications%5D-Device-Models) w repozytorium GitHub.

<!--
#### Using the simulator service

TODO: add steps for the simulator microservice here
-->

#### <a name="using-a-physical-device"></a>Za pomocą urządzenia fizycznego

Aby zaimplementować metody i zadań na urządzeniach fizycznych, zobacz następujące artykuły z Centrum IoT:

* [Informacje o funkcji i wywołanie metody bezpośrednio z Centrum IoT](../iot-hub/iot-hub-devguide-direct-methods.md).
* [Planowanie zadań na wielu urządzeniach](../iot-hub/iot-hub-devguide-jobs.md).

### <a name="other-customization-options"></a>Inne opcje dostosowywania

Aby zmodyfikować dodatkowe urządzenia łączności i przesyłania strumieniowego warstwie zdalnego rozwiązanie monitorowania, można edytować kodu. Odpowiednich repozytoriów GitHub są:

* [Telemetrii urządzenia (.NET)](https://github.com/Azure/device-telemetry-dotnet)
* [Telemetrii urządzenia (Java)](https://github.com/Azure/device-telemetry-java)
* [Agent telemetrii (.NET)](https://github.com/Azure/telemetry-agent-dotnet)
* [Agent telemetrii (Java)](https://github.com/Azure/telemetry-agent-java)

## <a name="data-processing-and-analytics"></a>Przetwarzanie danych i analiza

<!--
The following sections describe options to customize the data processing and analytics layer in the remote monitoring solution:

### Rules and actions

See the [Customize rules and actions](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to customize the rules and actions in solution.


### Other customization options
-->

Aby zmodyfikować przetwarzania danych i warstwy analityka w zdalnym rozwiązanie monitorowania, należy edytować kod. Odpowiednich repozytoriów GitHub są:

* [Agent telemetrii (.NET)](https://github.com/Azure/telemetry-agent-dotnet)
* [Agent telemetrii (Java)](https://github.com/Azure/telemetry-agent-java)

## <a name="infrastructure"></a>Infrastruktura

<!--
The following sections describe options for customizing the infrastructure services in the remote monitoring solution:

### Change storage

The default storage service for the remote monitoring solution is Cosmos DB. See the [Customize storage service](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to change the storage service the solution uses.

### Change log storage

The default storage service for logs is Cosmos DB. See the [Customize log storage service](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to change the storage service the solution uses for logging.

### Other customization options
-->

Aby zmodyfikować infrastruktury w zdalnym rozwiązanie monitorowania, należy edytować kod. Odpowiednich repozytoriów GitHub są:

* [IoTHub Manager (.NET)](https://github.com/Azure/iothub-manager-dotnet)
* [Menedżer Centrum IoTHub (Java)](https://github.com/Azure/iothub-manager-java)
* [Karta magazynu (.NET)](https://github.com/Azure/pcs-storage-adapter-dotnet)
* [Karta magazynu (Java)](https://github.com/Azure/pcs-storage-adapter-java)

## <a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono o zasoby dostępne do dostosowywania wstępnie skonfigurowanych rozwiązań.

Aby uzyskać więcej informacji pojęciach dotyczących zdalnego wstępnie skonfigurowane rozwiązanie monitorowania, zobacz [zdalne monitorowanie architektury](iot-suite-remote-monitoring-sample-walkthrough.md)

Aby uzyskać więcej informacji dotyczących dostosowywania zdalnego rozwiązanie monitorowania zobacz:

* [Przewodnik informacyjny dla deweloperów](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Przewodnik po rozwiązywaniu problemów dla deweloperów](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->