## <a name="view-the-telemetry"></a>Widok telemetrii

Pi malina teraz wysyła dane telemetryczne do zdalnego rozwiązanie monitorowania. Można wyświetlić dane telemetryczne na pulpicie nawigacyjnym rozwiązania. Można również wysyłać wiadomości do Twojej Pi malina z poziomu pulpitu nawigacyjnego rozwiązania.

- Przejdź do pulpitu nawigacyjnego rozwiązania.
- Wybierz urządzenie w **urządzenia do widoku** listy rozwijanej.
- Dane telemetryczne z Pi malina wyświetla na pulpicie nawigacyjnym.

![Wyświetl dane telemetryczne z Pi malina][img-telemetry-display]

## <a name="act-on-the-device"></a>Działania na urządzeniu

Na pulpicie nawigacyjnym rozwiązania można wywoływać metod w Twojej Pi malina. Gdy Pi malina łączy się zdalnego rozwiązanie monitorowania, wysyła informacje o metodach obsługiwanych.

- Na pulpicie nawigacyjnym rozwiązania kliknij **urządzeń** do odwiedzenia **urządzeń** strony. Wybierz użytkownika malinowe Pi w **listę urządzeń**. Następnie wybierz pozycję **metody**:

    ![Lista urządzeń na pulpicie nawigacyjnym][img-list-devices]

- Na **wywołania metody** wybierz pozycję **LightBlink** w **metody** listy rozwijanej.

- Wybierz **InvokeMethod**. Symulator wyświetla komunikat w konsoli za pośrednictwem malina Pi. Aplikacja na Pi malina wysyła potwierdzenie z powrotem do pulpitu nawigacyjnego rozwiązania:

    ![Pokaż historię — metoda][img-method-history]

- Możesz przełączyć LED włączać i wyłączać za pomocą **ChangeLightStatus** metody z **LightStatusValue** ustawioną **1** dla na lub **0** dla off.

> [!WARNING]
> Pozostawienie zdalnego monitorowania działającej na koncie Azure są rozliczane dla przy uruchomieniu. Aby uzyskać więcej informacji na temat zmniejszenie zużycia podczas wykonywania zdalnego rozwiązanie monitorowania, zobacz [Konfigurowanie pakiet IoT Azure wstępnie rozwiązań dla celów demonstracyjnych][lnk-demo-config]. Usuwanie wstępnie skonfigurowane rozwiązanie z konta platformy Azure po zakończeniu korzystania z niego.


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md