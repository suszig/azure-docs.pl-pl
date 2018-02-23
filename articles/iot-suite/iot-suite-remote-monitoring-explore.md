---
title: "Wprowadzenie do zdalnego rozwiązanie monitorowania - Azure | Dokumentacja firmy Microsoft"
description: "W tym samouczku używana symulowane scenariusze wprowadzenie zdalnego wstępnie skonfigurowane rozwiązanie monitorowania. Te scenariusze są tworzone podczas wdrażania zdalnego wstępnie skonfigurowane rozwiązanie monitorowania po raz pierwszy."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 02/15/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 5bc01c7e70e6837989c6bebd843ba1f41c8c5aeb
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="explore-the-capabilities-of-the-remote-monitoring-preconfigured-solution"></a>Poznaj możliwości zdalnego wstępnie skonfigurowane rozwiązanie monitorowania

Ten samouczek pokazuje kluczowych możliwości zdalnego rozwiązanie monitorowania. Aby dodać tych funkcji, samouczka ilustrację typowych scenariuszy klienta przy użyciu symulowane aplikacji IoT firmy o nazwie Contoso.

Samouczek ułatwia zrozumienie typowych scenariuszach IoT zdalnego rozwiązanie monitorowania udostępnia poza pole.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

>[!div class="checklist"]
> * Wizualizuj i urządzenia na pulpicie nawigacyjnym filtrować
> * Odpowiadanie na alarmu
> * Zaktualizuj oprogramowanie układowe urządzenia
> * Organizowanie zasobów
> * Zatrzymywanie i uruchamianie symulowanego urządzenia

Poniższe wideo przedstawia przewodnik zdalnego rozwiązanie monitorowania:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Part-28-An-introduction-to-Azure-IoT-through-the-new-Remote-Monitoring-Preconfigured-Solution/Player]

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka należy wdrożone wystąpienie zdalnego rozwiązanie monitorowania w ramach subskrypcji platformy Azure.

Jeśli jeszcze tego nie wdrożono rozwiązanie monitorowania zdalnego jeszcze, należy wykonać [wdrożyć zdalnego wstępnie skonfigurowane rozwiązanie monitorowania](iot-suite-remote-monitoring-deploy.md) samouczka.

## <a name="the-contoso-sample-iot-deployment"></a>Wdrożenie IoT przykładowej firmy Contoso

Umożliwia wdrożenie IoT przykładowej firmy Contoso zrozumieć podstawowe scenariusze zdalnego monitorowania rozwiązanie zapewnia poza pole. Te scenariusze są oparte na rzeczywistych wdrożeń IoT. Prawdopodobnie wybierzesz opcję Dostosuj zdalnego rozwiązanie monitorowania do własnych wymagań, ale przykładowej firmy Contoso pomaga Poznaj podstawy.

> [!NOTE]
> Jeśli użyto interfejsu wiersza polecenia do wdrożenia wstępnie skonfigurowanego rozwiązania, plik `deployment-{your deployment name}-output.json` zawiera informacje dotyczące wdrażania, takie jak adres URL wdrożonej przykładu.

Przykładowe firmy Contoso inicjuje zestaw symulowanego urządzenia i reguł, które działają na nich. Po zrozumieniu podstawowe scenariusze można kontynuować eksploracji więcej funkcji rozwiązania w [wykonaj zaawansowanego monitorowania urządzeń przy użyciu zdalnego rozwiązanie monitorowania](iot-suite-remote-monitoring-monitor.md).

Contoso to firma, która zarządza szerokiej gamy zasobów w różnych środowiskach. Contoso jest planowane używanie możliwości aplikacji opartych na chmurze IoT zdalnie monitorować i zarządzać wielu zasobów z scentralizowane aplikacji. Poniższe sekcje zawierają Podsumowanie konfiguracji początkowej próbki Contoso:

> [!NOTE]
> Pokaz Contoso jest tylko jednym ze sposobów udostępniania symulowanego urządzenia i tworzenie reguł. Inne opcje inicjowania obsługi administracyjnej obejmują tworzenie własnych urządzeń niestandardowych. Aby dowiedzieć się więcej o sposobie tworzenia własnych urządzeń i reguł, zobacz [zarządzanie i konfigurowanie urządzeń](iot-suite-remote-monitoring-manage.md) i [wykrywania problemów przy użyciu reguły progu](iot-suite-remote-monitoring-automate.md).

### <a name="contoso-devices"></a>Urządzenia firmy Contoso

Firma Contoso używa różnych typów urządzeń inteligentnych. Te urządzenia spełnienia różnych ról dla firmy, wysyłanie różnych strumieni danych telemetrycznych. Ponadto każdego typu urządzenia ma właściwości różnych urządzeń i obsługiwane metody.

W poniższej tabeli przedstawiono podsumowanie typów elastycznie urządzeń:

| Typ urządzenia        | Telemetria                                  | Właściwości                                  | Tagi                    | Metody                                                                                      |
| ------------------ | ------------------------------------------ | ------------------------------------------- | ----------------------- | -------------------------------------------------------------------------------------------- |
| Chłodnica            | Wykorzystanie temperatury i wilgotności,            | Typ, wersja oprogramowania układowego modelu               | Lokalizacja, Floor, firmy | Ponowny rozruch, oprogramowanie układowe aktualizacji wersji awaryjnego zawór wzrostu wykorzystania                          |
| Tworzenie prototypów urządzenia | Lokalizacja geograficzna temperatury, wykorzystania,        | Typ, wersja oprogramowania układowego modelu               | Lokalizacja, tryb          | Ponowny rozruch, aktualizacji oprogramowania układowego, Przenieś urządzenia, Zatrzymaj urządzenia, wydanie temperatury, wzrost temperatury |
| Aparat             | Zbiornik paliwa poziomu, chłodziwa czujnika, wibrację | Typ, wersja oprogramowania układowego modelu               | Lokalizacja, Floor, firmy | Uruchom ponownie, aktualizacji oprogramowania układowego pusty zbiornika, zbiornika wypełnienia                                              |
| Ciężarówka              | Lokalizacja geograficzna, szybkość ładunku temperatury     | Typ, wersja oprogramowania układowego modelu               | Lokalizacja, obciążenia          | Niższe temperatury ładunku, Zwiększ ładunku temperatury, aktualizacji oprogramowania układowego                         |
| Krótka           | FLOOR, wibrację, temperatury              | Typ, wersja oprogramowania układowego, Model, lokalizacja geograficzna | Lokalizacja, firmy        | Zatrzymaj krótka, krótka rozpoczęcia aktualizacji oprogramowania układowego                                               |

> [!NOTE]
> Wersja demonstracyjna Contoso przykładowe obsługuje dwa urządzenia według typu. Dla każdego typu jeden działa prawidłowo w granicach zdefiniowana jako normalne przez firmę Contoso i ten, który ma określonego rodzaju nieprawidłowego działania. W następnej sekcji informacje o regułach, które Contoso definiuje dla urządzeń. Reguły te należy zdefiniować granice poprawne zachowanie.

### <a name="contoso-rules"></a>Zasady firmy Contoso

Operatory w firmie Contoso znać progi określające, czy urządzenie działa poprawnie. Na przykład Chłodnica nie działa poprawnie, jeśli wykorzystania, który zgłasza jest większy niż 250 PSI. W poniższej tabeli przedstawiono zasady oparte na wartościach progowych, który definiuje Contoso dla każdego typu urządzenia:

| Nazwa reguły | Opis | Próg | Ważność | Dotyczy urządzeń |
| --------- | ----------- | --------- | -------- | ---------------- |
| Zbyt duże wykorzystanie Chłodnica | Alerty, jeśli chłodniach wyższe niż wykorzystania normalne poziomy dostępu   |P > 250 psi       | Krytyczne | Chłodniach            |
| Tworzenie prototypów urządzenia temp zbyt wysoka  | Alerty, jeśli urządzenia prototypowania wyższe niż temperatury normalne poziomy dostępu  |T>80&deg; F |Krytyczne | Tworzenie prototypów urządzeń |
| Zbiornik aparat jest pusty  | Alerty, jeśli aparat zbiornika przechodzi pusty                     | F < galonach 5 | Info     | Aparaty             |
| Wyższa niż temperatury normalne ładunku | Alerty, jeśli jego ciężarówka ładunku temperatury jest wyższy niż normalne                 | T<45&deg; F |Ostrzeżenie  | Pojazdów              |
| Krótka wibrację zatrzymana      | Alerty, jeśli krótka całkowicie zatrzymuje (na podstawie wibrację poziomu)                     | V < 0,1 mm |Ostrzeżenie  | Windy           |

### <a name="operate-the-contoso-sample-deployment"></a>Działanie wdrażania przykładowej firmy Contoso

Ma teraz widoczna początkowej konfiguracji w przykładowej firmy Contoso. W poniższych sekcjach opisano trzy scenariusze w przykładowym Contoso ilustrujące jak operator może używać wstępnie skonfigurowanych rozwiązań.

## <a name="respond-to-a-pressure-alarm"></a>Odpowiadanie na alarm wykorzystania

W tym scenariuszu przedstawiono sposób identyfikowania i reagowanie na alarmu, który zostanie wywołany przez urządzenie Chłodnica. Chłodnica znajduje się w Redmond, tworzenia 43, piętro 2.

Uprawnienia operatora można są wyświetlane na pulpicie nawigacyjnym alarmu dotyczące wykorzystania z Chłodnica. Można przesuwać i powiększania na mapie, aby wyświetlić więcej szczegółów.

1. Na **pulpitu nawigacyjnego** strony w **alarmy systemu** siatki, zostanie wyświetlony **zbyt duże wykorzystanie Chłodnica** alarm. Chłodnica jest wyróżnione na mapie:

    ![Pulpit nawigacyjny zawiera alarmu wykorzystania i urządzenia na mapie](media/iot-suite-remote-monitoring-explore/dashboardalarm.png)

1. Aby wyświetlić szczegóły urządzenia i dane telemetryczne, kliknij przycisk wyróżnione Chłodnica na mapie. Dane telemetryczne przedstawia kolekcji wykorzystania:

    ![Wybierz urządzenie, na mapie, aby wyświetlić szczegóły](media/iot-suite-remote-monitoring-explore/dashboarddetail.png)

1. Zamknij **szczegółów urządzenia**.

1. Aby przejść do **konserwacji** wybierz pozycję **konserwacji** w menu nawigacji.

Na **konserwacji** strony, można wyświetlić szczegóły reguły, która wyzwoliła Chłodnica alarm wykorzystania.

1. Na liście powiadomień pokazuje, ile razy wyzwolił alarmu, potwierdzenia i alarmy otwarte i zamknięte:

    ![Strona konserwacji listą alarmów, które ma być wywołany](media/iot-suite-remote-monitoring-explore/maintenancealarmlist.png)

1. Alarm pierwszy na liście jest ostatnim zadaniem. Kliknij przycisk **wykorzystania Chłodnica** alarmu, aby wyświetlić skojarzone urządzenia i danych telemetrycznych. Dane telemetryczne przedstawia końcówka wykorzystania Chłodnica:

    ![Strona konserwacji przedstawia dane telemetryczne dla wybranych alarmu](media/iot-suite-remote-monitoring-explore/maintenancetelemetry.png)

Możesz teraz zidentyfikować problem, która wyzwoliła alarm i skojarzone urządzenia. Jako operator następne kroki są potwierdzić alarm i łagodzenia problem.

1. Aby wskazać, że teraz pracujesz nad alarmu, zmień **Alarm stan** do **Acknowledged**:

    ![Wybierz i potwierdzić alarmu](media/iot-suite-remote-monitoring-explore/maintenanceacknowledge.png)

1. Do działania na chłodnica, zaznacz go, a następnie wybierz pozycję **harmonogram**. Wybierz **EmergencyValveRelease**, Dodaj nazwę zadania **ChillerPressureRelease**i wybierz polecenie **Zastosuj**. Tych ustawień Utwórz zadanie, które wykonuje natychmiast:

    ![Wybierz urządzenie i Zaplanuj akcji](media/iot-suite-remote-monitoring-explore/maintenanceschedule.png)

1. Aby wyświetlić stan zadania, wróć do **konserwacji** i Wyświetl listę zadań w **zadania** widoku. Można zauważyć, że zadanie zostało uruchomione zwolnienia zawór nacisku Chłodnica:

    ![Stan zadania w widoku zadania](media/iot-suite-remote-monitoring-explore/maintenancerunningjob.png)

Na koniec Potwierdź wartości danych telemetrycznych z Chłodnica na normalne.

1. Aby wyświetlić siatki alarmów, przejdź do **pulpitu nawigacyjnego** strony.

1. Aby wyświetlić dane telemetryczne urządzenie, wybierz urządzenie, dla oryginalnego alarmu na mapie i upewnij się, że jest na normalne.

1. Aby zamknąć zdarzenie, przejdź do **konserwacji** wybierz alarm, a wartość stanu **zamknięte**:

    ![Wybierz i zamknij alarmu](media/iot-suite-remote-monitoring-explore/maintenanceclose.png)

## <a name="update-device-firmware"></a>Zaktualizuj oprogramowanie układowe urządzenia

Contoso jest testowania nowego typu urządzenia, w tym polu. W ramach cyklu testowania musisz upewnij się, że poprawnie pracy aktualizacji oprogramowania układowego tego urządzenia. Poniższe kroki pokazują, jak używać zdalnego rozwiązanie monitorowania aktualizacji oprogramowania układowego na wielu urządzeniach.

Aby wykonywać zadania zarządzania niezbędne urządzenia, należy użyć **urządzeń** strony. Rozpocznij od filtrowania dla wszystkich urządzeń prototypowania:

1. Przejdź do **urządzeń** strony. Wybierz **urządzeń Prototypowania** Filtruj w **filtry** listy rozwijanej:

    ![Użyj filtru na stronie urządzenia](media/iot-suite-remote-monitoring-explore/devicesprototypingfilter.png)

    > [!TIP]
    > Kliknij przycisk **Zarządzaj** do zarządzania dostępnych filtrów.

1. Wybierz jedno z urządzeń prototypowania:

    ![Wybierz urządzenie, na stronie urządzenia](media/iot-suite-remote-monitoring-explore/devicesselect.png)

1. Kliknij przycisk **harmonogram** przycisk, a następnie wybierz pozycję **aktualizacji oprogramowania układowego**. Wprowadź wartości w polach **Nazwa zadania** i **URI oprogramowania układowego**. Wybierz **Zastosuj** zaplanowane zadanie, które chcesz teraz uruchomić:

    ![Planowanie aktualizacji oprogramowania układowego na urządzeniu](media/iot-suite-remote-monitoring-explore/devicesschedulefirmware.png)

    > [!NOTE]
    > Z symulowanego urządzenia można użyć dowolnego adresu URL jako, takich jak **URI oprogramowania układowego** wartość. Symulowane urządzeń nie dostępu do adresu URL.

1. Należy pamiętać, ile urządzeń ma wpływ na zadania i wybierz polecenie **Zastosuj**:

    ![Prześlij zadanie aktualizacji oprogramowania układowego](media/iot-suite-remote-monitoring-explore/devicessubmitupdate.png)

Można użyć **konserwacji** stronę, aby śledzić zadania, jak to działa.

1. Aby wyświetlić listę zadań, przejdź do **konserwacji** i kliknij przycisk **zadania**.

1. Znajdź zdarzenie związane z zadania, który został utworzony. Upewnij się, że proces aktualizacji oprogramowania układowego zostało poprawnie zainicjowane.

Można utworzyć filtr, aby sprawdzić, wersja oprogramowania układowego poprawnie aktualizowany.

1. Aby utworzyć filtr, przejdź do **urządzeń** i wybrać opcję **Zarządzanie filtrami**:

    ![Zarządzanie filtrami urządzenia](media/iot-suite-remote-monitoring-explore/devicesmanagefilters.png)

1. Utworzyć filtr, który zawiera tylko urządzenia z nowej wersji oprogramowania układowego:

    ![Utwórz filtr urządzenia](media/iot-suite-remote-monitoring-explore/devicescreatefilter.png)

1. Wróć do **urządzeń** stronie i sprawdź, czy urządzenie jest nowa wersja oprogramowania układowego.

## <a name="organize-your-assets"></a>Organizowanie zasobów

Firma Contoso ma dwa różne zespoły dla działania usługi pola:

* Zespół budynku inteligentnych zarządza chłodniach, windy i aparatów.
* Zespół inteligentne Vehicle zarządza pojazdów i prototypowania urządzeń.

Aby ułatwić jako operatora w celu organizowania i Zarządzaj swoimi urządzeniami, chcesz oznaczyć je o nazwie zespołu odpowiednie.

Można utworzyć nazwy tagów do użycia z urządzeń.

1. Aby wyświetlić wszystkich urządzeń, przejdź do **urządzeń** strony i wybierz polecenie **wszystkie urządzenia** filtru:

    ![Pokaż wszystkie urządzenia](media/iot-suite-remote-monitoring-explore/devicesalldevices.png)

1. Wybierz **pojazdów** i **Prototypowania** urządzeń. Następnie wybierz pozycję **Tag**:

    ![Wybierz urządzenia prototypu i ciężarówka](media/iot-suite-remote-monitoring-explore/devicesmultiselect.png)

1. Wybierz **Tag** , a następnie utwórz nowy znacznik tekst o nazwie **FieldService** z wartością **ConnectedVehicle**. Wybierz nazwę dla zadania. Następnie kliknij przycisk **Zastosuj**:

    ![Dodaj tag prototypu i ciężarówka urządzeń](media/iot-suite-remote-monitoring-explore/devicesaddtag.png)

1. Wybierz **Chłodnica**, **krótka**, i **aparat** urządzeń. Następnie wybierz pozycję **Tag**:

    ![Wybierz urządzenia chłodnica, aparatu i krótka](media/iot-suite-remote-monitoring-explore/devicesmultiselect2.png)

1. Wybierz **Tag** , a następnie utwórz nowy znacznik tekst o nazwie **FieldService** z wartością **SmartBuilding**. Wybierz nazwę dla zadania. Następnie kliknij przycisk **zapisać**:

    ![Dodaj tag chłodnica, aparatu i krótka urządzeń](media/iot-suite-remote-monitoring-explore/devicesaddtag2.png)

Można użyć wartości tagów, aby utworzyć filtry.

1. Na **urządzeń** wybierz pozycję **Zarządzanie filtrami**:

    ![Zarządzanie filtrami urządzenia](media/iot-suite-remote-monitoring-explore/devicesmanagefilters.png)

1. Utwórz nowy filtr, który używa nazwy tagu **FieldService** i wartość **SmartBuilding**. Zapisz filtr jako **budynku inteligentnych**.

1. Utwórz nowy filtr, który używa nazwy tagu **FieldService** i wartość **ConnectedVehicle**. Zapisz filtr jako **połączone Vehicle**.

Contoso operator można teraz kwerendy urządzeń według zespół operacyjny bez konieczności zmian na urządzeniach.

## <a name="stop-simulated-devices"></a>Zatrzymaj symulowanego urządzenia

W menu ustawień służy do zatrzymania symulowanego urządzenia. Pozwala to zmniejszyć koszty testowania i eksploracja rozwiązania. Aby uruchomić lub zatrzymać symulowanego urządzenia:

1. Wybierz **ustawienia** ikony.

1. Następnie przełącz **systemem** lub wyłącz:

    ![Menu ustawień](media/iot-suite-remote-monitoring-explore/settings.png)

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku przedstawiono do:

>[!div class="checklist"]
> * Wizualizuj i urządzenia na pulpicie nawigacyjnym filtrować
> * Odpowiadanie na alarmu
> * Zaktualizuj oprogramowanie układowe urządzenia
> * Organizowanie zasobów
> * Zatrzymywanie i uruchamianie symulowanego urządzenia

Teraz, gdy rozwiązanie monitorowania zdalnego zostały zbadane, sugerowane następnych krokach zostaną Dowiedz się więcej o zaawansowanych funkcji zdalnego rozwiązanie monitorowania:

* [Monitorowanie urządzeń](./iot-suite-remote-monitoring-monitor.md).
* [Zarządzaj swoimi urządzeniami](./iot-suite-remote-monitoring-manage.md).
* [Automatyzowanie rozwiązania przy użyciu reguł](./iot-suite-remote-monitoring-automate.md).
* [Obsługa rozwiązania](./iot-suite-remote-monitoring-maintain.md).
