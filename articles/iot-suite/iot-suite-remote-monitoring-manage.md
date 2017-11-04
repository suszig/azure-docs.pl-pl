---
title: "Zarządzanie urządzeniami w zdalnym rozwiązanie monitorowania - Azure | Dokumentacja firmy Microsoft"
description: "W tym samouczku przedstawiono sposób zarządzania urządzenia podłączone do zdalnego rozwiązanie monitorowania."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 09/06/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: e0b16a30bfd3b7ed711bcb1cc955d4eccf09fac2
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="manage-and-configure-your-devices"></a>Konfigurowanie urządzeń oraz zarządzanie nimi

Ten samouczek Pokazuje urządzenie możliwości zarządzania zdalnego rozwiązanie monitorowania. Aby dodać tych funkcji, samouczku scenariusza aplikacji Contoso IoT.

Firma Contoso ma uporządkowane nowe maszyny do rozwiń jedno ze swoich urządzeń do zwiększenia danych wyjściowych. Podczas oczekiwania dla nowych maszyn mają być dostarczane chcesz uruchomić symulacji, aby sprawdzić zachowanie rozwiązania. Jako operator chcesz zarządzania i skonfigurować urządzenia w rozwiązaniu do monitorowania zdalnego.

Oferuje rozszerzalny sposób zarządzania i konfigurowanie urządzeń, rozwiązanie monitorowania zdalnego korzysta z Centrum IoT funkcji, takich jak [zadania](../iot-hub/iot-hub-devguide-jobs.md) i [bezpośrednie metody](../iot-hub/iot-hub-devguide-direct-methods.md). Aby dowiedzieć się, jak implementuje metody dewelopera urządzenia na urządzeniu fizycznym, zobacz [dostosować zdalnego wstępnie skonfigurowane rozwiązanie monitorowania](iot-suite-remote-monitoring-customize.md).

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

>[!div class="checklist"]
> * Zapewnienie symulowane urządzenie.
> * Przetestuj symulowane urządzenie.
> * Wywołanie metody urządzenia z rozwiązania.
> * Zmień konfigurację urządzenia.

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, należy wdrożone wystąpienie zdalnego rozwiązanie monitorowania w ramach subskrypcji platformy Azure.

Jeśli jeszcze tego nie wdrożono rozwiązanie monitorowania zdalnego jeszcze, należy wykonać [wdrożyć zdalnego wstępnie skonfigurowane rozwiązanie monitorowania](iot-suite-remote-monitoring-deploy.md) samouczka.

## <a name="provision-a-simulated-device"></a>Zainicjuj obsługę symulowane urządzenie

Przejdź do **urządzeń** strony w rozwiązaniu, a następnie wybierz pozycję **udostępniania**. W **udostępniania** panelu, wybierz polecenie **symulowane**:

![Zainicjuj obsługę symulowane urządzenie](media/iot-suite-remote-monitoring-manage/devicesprovision.png)

Pozostaw liczbę urządzeń do świadczenia ustawioną **1**. Wybierz **aparat** jako **model urządzenia**, a następnie wybierz pozycję **Zastosuj** Aby utworzyć symulowane urządzenie:

![Zainicjuj obsługę aparatu symulowane urządzenie](media/iot-suite-remote-monitoring-manage/devicesprovisionengine.png)

Aby dowiedzieć się, jak udostępnić *fizycznych* urządzenia, zobacz [Podłącz urządzenie do zdalnego wstępnie skonfigurowane rozwiązanie monitorowania](iot-suite-connecting-devices-node.md).

## <a name="test-the-simulated-device"></a>Testowanie symulowane urządzenie

Aby wyświetlić szczegóły nowego symulowane urządzenie, wybierz go z listy urządzeń na **urządzeń** strony. Wyświetla informacje o urządzeniu **szczegółów urządzenia** panelu:

![Nowe urządzenie symulowane aparat widoku](media/iot-suite-remote-monitoring-manage/devicesviewnew.png)

W **szczegółów urządzenia**, Sprawdź nowe urządzenie wysyła dane telemetryczne. Aby wyświetlić strumieni różne dane telemetryczne z urządzenia, wybierz nazwę telemetrii takich jak **wibrację**:

![Wybierz strumień telemetrii, aby wyświetlić](media/iot-suite-remote-monitoring-manage/devicesvibration.png)

**Szczegółów urządzenia** panelu zawiera inne informacje o urządzeniu, takie jak wartości tagów, obsługuje metod i właściwości zgłoszonych przez urządzenia.

Aby wyświetlić szczegółowe diagnostyki, przewiń w dół do widoku **diagnostyki**.

## <a name="act-on-a-device"></a>Działania na urządzeniu

Do działania na urządzeniu, wybierz go na liście urządzeń, a następnie wybierz pozycję **harmonogram**. **Aparat** model urządzenia określa cztery metody musi obsługiwać urządzenia:

![Metody aparatu](media/iot-suite-remote-monitoring-manage/devicesmethods.png)

Wybierz **Uruchom ponownie**, ustaw nazwę zadania **RestartEngine**, a następnie wybierz pozycję **Zastosuj**:

![Zaplanuj metodę ponownego uruchomienia](media/iot-suite-remote-monitoring-manage/devicesrestartengine.png)

Aby śledzić stan zadania na **konserwacji** wybierz pozycję **stan systemu**:

![Monitor zadania harmonogramów](media/iot-suite-remote-monitoring-manage/maintenancerestart.png)

### <a name="methods-in-other-devices"></a>Metody w innych urządzeń

Poznaj typy różnych symulowane urządzenie, zobacz, czy inne typy urządzeń obsługuje różne metody. W przypadku wdrożenia z urządzeń fizycznych model urządzenia określa metody, które powinny obsługiwać urządzenia. Zazwyczaj developer urządzenia jest odpowiedzialny za tworzenie kodu, który sprawia, że urządzenie działa w odpowiedzi na wywołanie metody.

Aby zaplanować metody do uruchamiania na wielu urządzeniach, można wybrać wiele urządzeń na liście na **urządzeń** strony. **Harmonogram** panel zawiera typy metody wspólne dla wszystkich wybranych urządzeń.

## <a name="reconfigure-a-device"></a>Ponownie skonfiguruj urządzenie

Aby zmienić konfigurację urządzenia, wybierz ją na liście urządzeń **urządzeń** strony, a następnie wybierz pozycję **ponownie skonfigurować**. Panelu ponownej konfiguracji wyświetlane dla wybranego urządzenia, które można zmienić wartości właściwości:

![Ponownie skonfiguruj urządzenie](media/iot-suite-remote-monitoring-manage/devicesreconfigure.png)

Aby wprowadzić zmiany, Dodaj nazwę zadania, zaktualizować wartości właściwości i wybierz **Zastosuj**:

![Zaktualizuj wartość właściwości urządzenia](media/iot-suite-remote-monitoring-manage/devicesreconfigurephysical.png)

Aby śledzić stan zadania na **konserwacji** wybierz pozycję **stan systemu**.

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono należy jak do:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Zapewnienie symulowane urządzenie.
> * Przetestuj symulowane urządzenie.
> * Wywołanie metody urządzenia z rozwiązania.
> * Zmień konfigurację urządzenia.

Teraz, kiedy znasz sposobu zarządzania urządzeniami, Sugerowane następne kroki są Aby dowiedzieć się, jak:

* [Rozwiązywanie problemów i Korygowanie problemów z urządzeniami](iot-suite-remote-monitoring-maintain.md).
* [Testowanie rozwiązania z urządzeniami symulowane](iot-suite-remote-monitoring-test.md).
* [Podłącz urządzenie do zdalnego wstępnie skonfigurowane rozwiązanie monitorowania](iot-suite-connecting-devices-node.md).

<!-- Next tutorials in the sequence -->