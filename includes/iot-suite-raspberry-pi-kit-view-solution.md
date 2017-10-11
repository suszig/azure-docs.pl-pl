## <a name="view-the-solution-dashboard"></a>Wyświetlanie pulpitu nawigacyjnego

Pulpit nawigacyjny pozwala zarządzać wdrożonym rozwiązaniem. Można na przykład wyświetlić dane telemetryczne, Dodaj urządzenia i wywołania metody.

1. Jeśli aprowizacja została ukończona, a na kafelku wstępnie skonfigurowanego rozwiązania jest wyświetlany stan **Gotowe**, wybierz pozycję **Uruchom**, aby otworzyć portal rozwiązania do monitorowania zdalnego na nowej karcie.

    ![Uruchamianie wstępnie skonfigurowanego rozwiązania][img-launch-solution]

1. Domyślnie w portalu rozwiązania jest wyświetlany *pulpit nawigacyjny*. Korzystając z menu po lewej stronie, można przejść do innych obszarów portalu rozwiązania.

    ![Pulpit nawigacyjny wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego][img-menu]

## <a name="add-a-device"></a>Dodawanie urządzenia

Aby urządzenie mogło nawiązać połączenie ze wstępnie skonfigurowanym rozwiązaniem, musi ono zidentyfikować się względem usługi IoT Hub za pomocą prawidłowych poświadczeń. Poświadczenia urządzenia możesz pobrać z pulpitu nawigacyjnego rozwiązania. W dalszej części tego samouczka podasz te poświadczenia urządzenia w Twojej aplikacji klienckiej.

Jeśli jeszcze tego nie zrobiono, należy dodać niestandardowe urządzenie do zdalnego rozwiązanie monitorowania. Wykonaj następujące czynności na pulpicie nawigacyjnym rozwiązania:

1. W lewym dolnym rogu pulpitu nawigacyjnego kliknij pozycję **Dodaj urządzenie**.

   ![Dodawanie urządzenia][1]

1. W panelu **Urządzenie niestandardowe** kliknij pozycję **Dodaj nowe**.

   ![Dodawanie urządzenia niestandardowego][2]

1. Wybierz pozycję **Pozwól mi zdefiniować własny identyfikator urządzenia**. Wprowadź identyfikator urządzenia, takie jak **rasppi**, kliknij przycisk **Sprawdź identyfikator** można zweryfikować już nie użyto nazwy rozwiązania, a następnie kliknij przycisk **Utwórz** do obsługi administracyjnej urządzeniu.

   ![Dodawanie identyfikatora urządzenia][3]

1. Zanotuj poświadczenia urządzenia (**identyfikator urządzenia**, **nazwy hosta Centrum IoT**, i **klucza urządzenia**). Aplikacja kliencka na Pi malina wymaga tych wartości, aby nawiązać połączenie zdalne rozwiązanie monitorowania. Następnie kliknij przycisk **Gotowe**.

    ![Wyświetlanie poświadczeń urządzenia][4]

1. Wybierz urządzenie na liście urządzeń na pulpicie nawigacyjnym rozwiązania. Następnie w panelu **Szczegóły urządzenia** kliknij pozycję **Włącz urządzenie**. Stan Twojego urządzenia to teraz **Uruchomione**. Rozwiązanie do monitorowania zdalnego może teraz odbierać dane telemetryczne z Twojego urządzenia i wywoływać metody na tym urządzeniu.

[img-launch-solution]: media/iot-suite-raspberry-pi-kit-view-solution/launch.png
[img-menu]: media/iot-suite-raspberry-pi-kit-view-solution/menu.png
[1]: media/iot-suite-raspberry-pi-kit-view-solution/suite0.png
[2]: media/iot-suite-raspberry-pi-kit-view-solution/suite1.png
[3]: media/iot-suite-raspberry-pi-kit-view-solution/suite2.png
[4]: media/iot-suite-raspberry-pi-kit-view-solution/suite3.png