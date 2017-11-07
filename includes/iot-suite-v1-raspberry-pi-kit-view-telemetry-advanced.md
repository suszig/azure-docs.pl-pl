## <a name="view-the-telemetry"></a>Widok telemetrii

Pi malina teraz wysyła dane telemetryczne do zdalnego rozwiązanie monitorowania. Można wyświetlić dane telemetryczne na pulpicie nawigacyjnym rozwiązania. Można również wysyłać wiadomości do Twojej Pi malina z poziomu pulpitu nawigacyjnego rozwiązania.

- Przejdź do pulpitu nawigacyjnego rozwiązania.
- Wybierz urządzenie w **urządzenia do widoku** listy rozwijanej.
- Dane telemetryczne z Pi malina wyświetla na pulpicie nawigacyjnym.

![Wyświetl dane telemetryczne z Pi malina][img-telemetry-display]

## <a name="initiate-the-firmware-update"></a>Zainicjuj aktualizację oprogramowania układowego

Proces aktualizacji oprogramowania układowego pobiera i instaluje zaktualizowaną wersję aplikacji klienta urządzenia na malina Pi. Aby uzyskać więcej informacji na temat procesu aktualizacji oprogramowania układowego, zobacz opis wzorca aktualizacji oprogramowania układowego w [omówienie zarządzania urządzeniami z Centrum IoT][lnk-update-pattern].

Inicjuje proces aktualizacji oprogramowania układowego przez wywołanie metody na urządzeniu. Ta metoda jest asynchroniczne i zwraca natychmiast rozpoczyna się proces aktualizacji. Urządzenie używa właściwości zgłoszony do powiadamiania rozwiązania dotyczące postępu aktualizacji.

Wywołuje metody na Twoje Pi malina z poziomu pulpitu nawigacyjnego rozwiązania. Pi malina pierwszy raz łączy się zdalnego rozwiązanie monitorowania, wysyła informacje o metodach obsługiwanych. 

[img-telemetry-display]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced/telemetry.png
[lnk-update-pattern]: ../articles/iot-hub/iot-hub-device-management-overview.md
