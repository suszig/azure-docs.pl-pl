---
title: "Utwórz regułę niestandardową pakietu IoT Azure | Dokumentacja firmy Microsoft"
description: "Jak utworzyć niestandardową regułę w pakiet IoT wstępnie skonfigurowane rozwiązanie."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 9bf2a13035de141766fd935966ce18459dccdaab
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="create-a-custom-rule-in-the-remote-monitoring-preconfigured-solution"></a>Utwórz regułę niestandardową w zdalnym wstępnie skonfigurowane rozwiązanie monitorowania

## <a name="introduction"></a>Wprowadzenie

W przypadku wstępnie skonfigurowanych rozwiązań, można skonfigurować [wartość reguły, które są wyzwalane w razie telemetrii urządzenia osiągnie określony próg][lnk-builtin-rule]. [Użyj dynamicznych danych telemetrycznych z monitorowania zdalnego wstępnie skonfigurowane rozwiązanie] [ lnk-dynamic-telemetry] opisuje sposób dodawania wartości niestandardowych telemetrii, takich jak *ExternalTemperature* do rozwiązania. W tym artykule przedstawiono sposób tworzenia reguły niestandardowe dla typów dynamicznych telemetrii w rozwiązaniu.

W tym samouczku używana proste Node.js symulowane urządzenie można wygenerować dynamiczne telemetrię, aby wysłać do zaplecza wstępnie skonfigurowanego rozwiązania. Następnie dodaj niestandardowe reguły w **RemoteMonitoring** rozwiązania Visual Studio i wdrażanie tego dostosowane zaplecza do subskrypcji platformy Azure.

Do ukończenia tego samouczka niezbędne są następujące elementy:

* Aktywna subskrypcja platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure][lnk_free_trial].
* [Node.js] [ lnk-node] wersji 0.12.x lub nowszej, aby utworzyć symulowane urządzenie.
* Visual Studio 2015 lub Visual Studio 2017 zmodyfikować wstępnie skonfigurowane rozwiązanie ponownie kończyć się nowych reguł.

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

Zanotuj nazwy rozwiązania, którą wybrano dla danego wdrożenia. Należy to nazwa rozwiązania w dalszej części tego samouczka.

[!INCLUDE [iot-suite-v1-send-external-temperature](../../includes/iot-suite-v1-send-external-temperature.md)]

Po upewnieniu się, że jest wysyłany, można zatrzymać aplikacji konsoli Node.js **ExternalTemperature** telemetrię, aby wstępnie skonfigurowanych rozwiązań. Nie zamykaj okna konsoli ponieważ ponownie uruchom tę aplikację konsoli Node.js po dodaniu niestandardowej reguły do rozwiązania.

## <a name="rule-storage-locations"></a>Lokalizacje magazynu reguły

Informacje o regułach jest utrwalona w dwóch miejscach:

* **DeviceRulesNormalizedTable** tabeli — znormalizowane odwołanie do reguły zdefiniowane przez portal rozwiązania są przechowywane w tej tabeli. Reguły urządzenia są wyświetlane w portalu rozwiązania, wysyła zapytanie tej tabeli definicji reguły.
* **DeviceRules** obiektu blob — ten obiekt blob przechowuje wszystkie reguły zdefiniowane dla wszystkich zarejestrowanych urządzeń i jest zdefiniowany jako danych wejściowych do zadania usługi analiza strumienia Azure odwołanie.
 
Podczas aktualizacji istniejącej reguły lub zdefiniuj nowe reguły w portalu rozwiązania, tabelę i obiektów blob są aktualizowane zgodnie ze zmianami. Definicję reguły wyświetlana w portalu jest dostarczany z tabeli magazynu, a Definicja reguły odwołuje się zadania usługi analiza strumienia pochodzi z obiektu blob. 

## <a name="update-the-remotemonitoring-visual-studio-solution"></a>Aktualizacji rozwiązania RemoteMonitoring Visual Studio

Poniższe kroki pokazują sposób modyfikowania RemoteMonitoring rozwiązanie programu Visual Studio do uwzględnienia nową regułę, która używa **ExternalTemperature** danych telemetrycznych wysłanych z symulowane urządzenie:

1. Jeśli nie zostało to jeszcze zrobione, klonowanie **azure iot — zdalnego monitorowania** repozytorium do odpowiedniej lokalizacji na komputerze lokalnym za pomocą następujących poleceń Git:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. W programie Visual Studio Otwórz plik RemoteMonitoring.sln z lokalną kopię **azure iot — zdalnego monitorowania** repozytorium.

3. Otwórz plik Infrastructure\Models\DeviceRuleBlobEntity.cs i Dodaj **ExternalTemperature** właściwości w następujący sposób:

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. W tym samym pliku Dodaj **ExternalTemperatureRuleOutput** właściwości w następujący sposób:

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. Otwórz plik Infrastructure\Models\DeviceRuleDataFields.cs i Dodaj następujący **ExternalTemperature** właściwości po istniejącej **wilgotności** właściwości:

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. W tym samym pliku, należy zaktualizować **_availableDataFields** metodę w celu uwzględnienia **ExternalTemperature** w następujący sposób:

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. Otwórz plik Infrastructure\Repository\DeviceRulesRepository.cs i zmodyfikować **BuildBlobEntityListFromTableRows** metody w następujący sposób:

    ```csharp
    else if (rule.DataField == DeviceRuleDataFields.Humidity)
    {
        entity.Humidity = rule.Threshold;
        entity.HumidityRuleOutput = rule.RuleOutput;
    }
    else if (rule.DataField == DeviceRuleDataFields.ExternalTemperature)
    {
      entity.ExternalTemperature = rule.Threshold;
      entity.ExternalTemperatureRuleOutput = rule.RuleOutput;
    }
    ```

## <a name="rebuild-and-redeploy-the-solution"></a>Ponowne skompilowanie i wdrożenie rozwiązania.

Teraz można wdrażać zaktualizowane rozwiązanie z subskrypcją platformy Azure.

1. Otwórz wiersz polecenia z podwyższonym poziomem uprawnień i przejdź do katalogu głównego kopii lokalnej repozytorium azure iot — zdalnego monitorowania.

2. Aby wdrożyć rozwiązanie zaktualizowane, uruchom następujące polecenie podstawiając **{Nazwa wdrożenia}** o nazwie wdrożenia wstępnie skonfigurowane rozwiązanie zanotowany wcześniej:

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-the-stream-analytics-job"></a>Aktualizacja zadania usługi analiza strumienia

Po zakończeniu wdrożenia należy zaktualizować zadania usługi analiza strumienia do używania nowych definicji reguły.

1. W portalu Azure przejdź do grupy zasobów zawiera zasoby wstępnie skonfigurowanych rozwiązań. Ta grupa zasobów ma taką samą nazwę, określone dla rozwiązania podczas wdrażania.

2. Przejdź do {Nazwa wdrożenia}-zadania usługi analiza strumienia reguły. 

3. Kliknij przycisk **zatrzymać** zatrzymania zadania usługi analiza strumienia uruchamianie. (Należy poczekać na Zatrzymaj, aby można było edytować zapytanie zadania przesyłania strumieniowego).

4. Kliknij przycisk **zapytania**. Edytuj zapytanie, aby uwzględnić **wybierz** instrukcji dla **ExternalTemperature**. Poniższy przykład przedstawia pełną zapytania z nowym **wybierz** instrukcji:

    ```
    WITH AlarmsData AS 
    (
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Temperature' as ReadingType,
         Stream.Temperature as Reading,
         Ref.Temperature as Threshold,
         Ref.TemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Humidity' as ReadingType,
         Stream.Humidity as Reading,
         Ref.Humidity as Threshold,
         Ref.HumidityRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'ExternalTemperature' as ReadingType,
         Stream.ExternalTemperature as Reading,
         Ref.ExternalTemperature as Threshold,
         Ref.ExternalTemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.ExternalTemperature IS NOT null AND Stream.ExternalTemperature > Ref.ExternalTemperature
    )
     
    SELECT *
    INTO DeviceRulesMonitoring
    FROM AlarmsData
     
    SELECT *
    INTO DeviceRulesHub
    FROM AlarmsData
    ```

5. Kliknij przycisk **zapisać** zmienić zaktualizowane reguły zapytania.

6. Kliknij przycisk **Start** można uruchomić zadania Stream Analytics, ponowne uruchomienie.

## <a name="add-your-new-rule-in-the-dashboard"></a>Dodaj nową regułę na pulpicie nawigacyjnym

Można teraz dodawać **ExternalTemperature** regułę do urządzenia, na pulpicie nawigacyjnym rozwiązania.

1. Przejdź do portalu rozwiązania.

2. Przejdź do **urządzeń** panelu.

3. Zlokalizuj urządzeń niestandardowych utworzony wysyłanej **ExternalTemperature** telemetrii i na **szczegóły urządzenia** panelu, kliknij przycisk **Dodaj regułę**.

4. Wybierz **ExternalTemperature** w **pola danych**.

5. Ustaw **próg** do 56. Następnie kliknij przycisk **Zapisz i Wyświetl reguły**.

6. Wróć do pulpitu nawigacyjnego, aby wyświetlić historię alarm.

7. W oknie konsoli po lewej, Otwórz, uruchomić aplikację konsoli Node.js, aby rozpocząć wysyłanie **ExternalTemperature** danych telemetrycznych.

8. Zwróć uwagę, że **historii Alarm** tabeli przedstawiono nowe alarmy po wyzwoleniu nowej reguły.
 
## <a name="additional-information"></a>Dodatkowe informacje

Zmiana operator  **>**  jest bardziej złożony i wykracza poza kroki opisane w tym samouczku. Chociaż można zmienić zadanie usługi Stream Analytics, aby użyć niezależnie od operatora Ci się podoba, odzwierciedlające operatora w portalu rozwiązania jest bardziej złożonych zadań. 

## <a name="next-steps"></a>Następne kroki
Teraz, przedstawiono sposób tworzenia reguł niestandardowych, można dowiedzieć się więcej o wstępnie skonfigurowanych rozwiązań:

- [Łączenie aplikacji logiki do rozwiązania Azure IoT pakiet monitorowania zdalnego wstępnie][lnk-logic-app]
- [Urządzenia informacji metadanych do monitorowania zdalnego wstępnie skonfigurowane rozwiązanie][lnk-devinfo].

[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-v1-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-v1-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-v1-logic-apps-tutorial.md