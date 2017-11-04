---
title: "Rozwiązywanie problemów z urządzeń w rozwiązaniu monitorowania zdalnego - Azure | Dokumentacja firmy Microsoft"
description: "Ten samouczek pokazuje, jak rozwiązywanie problemów i Korygowanie problemów z urządzeniami w zdalnym rozwiązanie monitorowania."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 09/16/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: ac284126c2c940caf97fb95bd46234de49b1f678
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="troubleshoot-and-remediate-device-issues"></a>Rozwiązywanie problemów i Korygowanie problemów z urządzeniami

Ten samouczek przedstawia sposób użycia **konserwacji** strony w rozwiązaniu Rozwiązywanie problemów i Korygowanie problemów z urządzeniami. Aby dodać tych funkcji, samouczku scenariusza aplikacji Contoso IoT.

Firma Contoso jest testowania nowego **prototypu** urządzenie w polu. Jako operatora firmy Contoso, zauważysz podczas testowania, który **prototypu** urządzenia jest nieoczekiwanie wyzwolenie alarmu temperatury, na pulpicie nawigacyjnym. Musi teraz Zbadaj zachowanie tym błędny **prototypu** urządzenia.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

>[!div class="checklist"]
> * Użyj **konserwacji** stronę, aby zbadać alarmu
> * Wywołanie metody urządzenia skorygowanie problemu

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, należy wdrożone wystąpienie zdalnego rozwiązanie monitorowania w ramach subskrypcji platformy Azure.

Jeśli jeszcze tego nie wdrożono rozwiązanie monitorowania zdalnego jeszcze, należy wykonać [wdrożyć zdalnego wstępnie skonfigurowane rozwiązanie monitorowania](iot-suite-remote-monitoring-deploy.md) samouczka.

## <a name="use-the-maintenance-dashboard"></a>Pulpit nawigacyjny obsługi

Na **pulpitu nawigacyjnego** strony można zauważyć, że istnieją alarmy nieoczekiwany temperatury pochodzące z regułą skojarzoną z **prototypu** urządzeń:

![Alarmy wyświetlane na pulpicie nawigacyjnym](media/iot-suite-remote-monitoring-maintain/dashboardalarm.png)

Aby zbadać problem, wybierz **Eksploruj Alarm** opcję obok alarmu:

![Eksploruj alarmu z poziomu pulpitu nawigacyjnego](media/iot-suite-remote-monitoring-maintain/dashboardexplorealarm.png)

Można teraz wyświetlić listę alarmy na **konserwacji** strony:

![Lista alarmy na stronie konserwacji](media/iot-suite-remote-monitoring-maintain/maintenancealarms.png)

Aby wyświetlić szczegóły alarmu, wybierz alarmowych w **alarmy** listy. Przedstawia widok szczegółów:

* Jeśli zostało wyzwolone alarmu
* Informacje dotyczące urządzeń skojarzonych z alarm stanu
* Dane telemetryczne z urządzeń skojarzonych z alarmu

![Szczegóły alarmu](media/iot-suite-remote-monitoring-maintain/maintenancealarmdetail.png)

Aby potwierdzić alarmu, wybierz **Alarm wystąpień** i wybierz polecenie **potwierdzenia**. Ta akcja umożliwia innych operatorom alarm jak już wspomniano, a następnie pracy.

![Potwierdzić alarmy](media/iot-suite-remote-monitoring-maintain/maintenanceacknowledge.png)

Na liście widać **prototypu** odpowiedzialny za wyzwalania alarm temperatury urządzenia:

![Utwórz listę urządzeń, co powoduje alarmu](media/iot-suite-remote-monitoring-maintain/maintenanceresponsibledevice.png)

## <a name="remediate-the-issue"></a>Rozwiązania problemu

Do rozwiązania problemu z **prototypu** urządzenia, należy wywołać **DecreaseTemperature** metody na urządzeniu.

Do działania na urządzeniu, wybierz go na liście urządzeń, a następnie wybierz pozycję **harmonogram**. **Aparat** model urządzenia określa trzy metody musi obsługiwać urządzenia:

![Wyświetl metody, które obsługuje urządzenia](media/iot-suite-remote-monitoring-maintain/maintenancemethods.png)

Wybierz **DecreaseTemperature** i ustaw nazwę zadania **DecreaseTemperature**. Następnie wybierz pozycję **Zastosuj**:

![Utwórz zadanie, aby zmniejszyć temperatury](media/iot-suite-remote-monitoring-maintain/maintenancecreatejob.png)

Aby śledzić stan zadania na **konserwacji** wybierz pozycję **stan systemu**. Użyj **stan systemu** Wyświetl, aby śledzić wszystkie zadania i wywołuje metodę w rozwiązaniu:

![Monitor zadania, aby zmniejszyć temperatury](media/iot-suite-remote-monitoring-maintain/maintenancerunningjob.png)

Aby wyświetlić szczegóły określonego zadania lub wywołanie metody, wybierz go na liście w **stan systemu** widoku:

![Wyświetlanie szczegółów zadania](media/iot-suite-remote-monitoring-maintain/maintenancejobdetail.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku będziemy wyświetlał jak do:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Użyj **konserwacji** stronę, aby zbadać alarmu
> * Wywołanie metody urządzenia skorygowanie problemu

Po zapoznaniu się Rozwiązywanie problemów dotyczących urządzenia, sugerowane następnym krokiem jest Dowiedz się, jak [przetestowany z urządzeniami symulowane](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->