---
title: "Wykrywanie problemów z urządzeniami w zdalnym rozwiązanie monitorowania - Azure | Dokumentacja firmy Microsoft"
description: "W tym samouczku przedstawiono sposób automatycznego wykrywania problemów z urządzeniami oparte na wartościach progowych w zdalnym rozwiązanie monitorowania przy użyciu reguł i akcje."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 02/22/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 9d9fbefd81fed506bcc025fa0f44315ec831cf0d
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="detect-issues-using-threshold-based-rules"></a>Wykrywanie problemów przy użyciu reguły progu

Ten samouczek pokazuje możliwości aparatu reguł w zdalnym rozwiązanie monitorowania. Aby dodać tych funkcji, samouczku scenariusza aplikacji Contoso IoT.

Firma Contoso ma regułę, która generuje alert krytyczny, gdy wykorzystanie zgłoszone przez **Chłodnica** urządzenia przekracza 250 PSI. Jako operator chcesz zidentyfikować **Chłodnica** urządzeń, które mogą mieć problemy czujników, wyszukując początkowej wartości szczytowe wykorzystania. Aby zidentyfikować te urządzenia, należy utworzyć regułę, aby generować ostrzeżenia, gdy wykorzystanie przekracza 150 PSI.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

>[!div class="checklist"]
> * Wyświetlanie reguł w rozwiązaniu
> * Utwórz nową regułę
> * Edytuj istniejącą regułę
> * Usuwanie reguły

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, należy wdrożone wystąpienie zdalnego rozwiązanie monitorowania w ramach subskrypcji platformy Azure.

Jeśli jeszcze tego nie wdrożono rozwiązanie monitorowania zdalnego jeszcze, należy wykonać [wdrożyć zdalnego wstępnie skonfigurowane rozwiązanie monitorowania](iot-suite-remote-monitoring-deploy.md) samouczka.

## <a name="view-the-rules-in-your-solution"></a>Wyświetlanie reguł w rozwiązaniu

**Reguł i akcje** w rozwiązaniu zostanie wyświetlona lista wszystkich istniejących zasad:

![Strona reguł i akcji](media/iot-suite-remote-monitoring-automate/rulesactions.png)

Aby wyświetlić tylko reguły, które dotyczą **Chłodnica** urządzeń, zastosuj filtr:

![Lista reguł filtrowania](media/iot-suite-remote-monitoring-automate/rulesactionsfilter.png)

Możesz wyświetlić więcej informacji na temat regułę i go edytować, po wybraniu na liście:

![Wyświetlanie szczegółów reguły](media/iot-suite-remote-monitoring-automate/rulesactionsdetail.png)

Aby wyłączyć, włączanie lub usuwanie co najmniej jednej reguły, wybierz wiele reguł na liście:

![Wybierz wiele reguł](media/iot-suite-remote-monitoring-automate/rulesactionsmultiselect.png)

## <a name="create-a-new-rule"></a>Utwórz nową regułę

Aby dodać nową regułę, która generuje ostrzeżenie podczas wykorzystania w **Chłodnica** urządzenia przekracza 150 PSI, wybierz **nową regułę**:

![Utwórz regułę](media/iot-suite-remote-monitoring-automate/rulesactionsnewrule.png)

Aby utworzyć regułę, należy użyć następujących wartości:

| Ustawienie          | Wartość                                 |
| ---------------- | ------------------------------------- |
| Name (Nazwa)             | Ostrzeżenie Chłodnica                       |
| Element źródłowy           | **Chłodniach** grupy urządzeń             |
| Pole wyzwalacza    | pressure                              |
| Operator wyzwalacza | Więcej niż                          |
| Wartość wyzwalacza    | 150                                   |
| Poziom ważności   | Ostrzeżenie                               |
| Opis      | Wykorzystanie Chłodnica przekracza 150 PSI |

Aby zapisać nową regułę, wybierz **Zastosuj**.

Można wyświetlić, gdy reguła jest wyzwalana na **reguł i akcje** strony lub na **pulpitu nawigacyjnego** strony.

## <a name="edit-an-existing-rule"></a>Edytuj istniejącą regułę

Aby wprowadzić zmianę do istniejącej reguły, wybierz je w listy zasad. Następnie w **szczegóły** wybierz panel **trybu edycji**.

![Edytuj regułę](media/iot-suite-remote-monitoring-automate/rulesactionsedit.png)

## <a name="disable-a-rule"></a>Wyłączanie reguły

Aby tymczasowo wyłączyć regułę, można ją wyłączyć na liście reguł. Wybierz regułę tak, aby wyłączyć, a następnie wybierz pozycję **wyłączyć**. **Stan** reguły na liście zmieni się, wskazując reguły jest obecnie wyłączony. Można ponownie włączyć regułę, że wcześniej wyłączone w tym samym trybie.

![Wyłączanie reguły](media/iot-suite-remote-monitoring-automate/rulesactionsdisable.png)

Można włączyć i wyłączyć wielu reguł jednocześnie w przypadku wybrania wielu reguł na liście.

## <a name="delete-a-rule"></a>Usuwanie reguły

Aby trwale usunąć regułę, wybierz regułę na liście reguł, a następnie wybierz **usunąć**.

W przypadku wybrania wielu reguł na liście, możesz usunąć wiele reguł w tym samym czasie.

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku przedstawiono należy jak do:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Wyświetlanie reguł w rozwiązaniu
> * Utwórz nową regułę
> * Edytuj istniejącą regułę
> * Usuwanie reguły

Teraz, kiedy znasz sposobu wykrywania problemów przy użyciu reguł oparte na wartościach progowych, Sugerowane następne kroki są Aby dowiedzieć się, jak:

* [Konfigurowanie urządzeń oraz zarządzanie nimi](./iot-suite-remote-monitoring-manage.md).
* [Rozwiązywanie problemów i Korygowanie problemów z urządzeniami](./iot-suite-remote-monitoring-maintain.md).
* [Testowanie rozwiązania z urządzeniami symulowane](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->