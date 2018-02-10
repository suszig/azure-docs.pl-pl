> [!div class="op_single_selector"]
> * [C w systemie Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C w systemie Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js (ogólny)](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> * [Node.js na urządzeniu Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-node.md)
> * [C na urządzeniu Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-c.md)

W tym samouczku zaimplementowaniem **Chłodnica** urządzenia, które wysyła następujące dane telemetryczne do monitorowania zdalnego [wstępnie skonfigurowane rozwiązanie](../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md):

* Temperatura
* wykorzystania
* Wilgotność

Dla uproszczenia kodu generuje przykładowe wartości telemetrii **Chłodnica**. Można rozszerzyć próbki przez połączenie czujników rzeczywistego urządzenia i wysyła rzeczywistych danych telemetrycznych.

Urządzenie próbki również:

* Wysyła metadane do rozwiązania do opisywania jego możliwości.
* Odpowiada na działania wywoływane z **urządzeń** strony w rozwiązaniu.
* Wyślij odpowiada na zmiany w konfiguracji z **urządzeń** strony w rozwiązaniu.

Do wykonania kroków tego samouczka jest potrzebne aktywne konto platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](http://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Przed rozpoczęciem

Przed przystąpieniem do pisania jakiegokolwiek kodu dla urządzenia musisz przeprowadzić aprowizację wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego i nowego niestandardowego urządzenia w ramach tego rozwiązania.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Aprowizowanie wstępnie skonfigurowanego rozwiązania do monitorowania zdalnego

**Chłodnica** urządzenia, możesz utworzyć w tym samouczku wysyłania danych do wystąpienia [monitorowania zdalnego](../articles/iot-suite/iot-suite-remote-monitoring-explore.md) wstępnie skonfigurowane rozwiązanie. Jeśli nie zostało już wstępnie skonfigurowane zdalnego wstępnie skonfigurowane rozwiązanie monitorowania w konta platformy Azure, zobacz [wdrożyć zdalnego wstępnie skonfigurowane rozwiązanie monitorowania](../articles/iot-suite/iot-suite-remote-monitoring-deploy.md)

Po zakończeniu procesu aprowizowania rozwiązania do monitorowania zdalnego kliknij pozycję **Uruchom**, aby otworzyć pulpit nawigacyjny rozwiązania w przeglądarce.

![Pulpit nawigacyjny rozwiązania](media/iot-suite-selector-connecting/dashboard.png)

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Aprowizowanie urządzenia w ramach rozwiązania do monitorowania zdalnego

> [!NOTE]
> Jeśli już przeprowadzono aprowizację urządzenia w ramach rozwiązania, możesz pominąć ten krok. Należy ciąg połączenia urządzenia, który można pobrać z portalu Azure, podczas tworzenia aplikacji klienckiej.

Aby urządzenie mogło nawiązać połączenie ze wstępnie skonfigurowanym rozwiązaniem, musi ono zidentyfikować się względem usługi IoT Hub za pomocą prawidłowych poświadczeń. Użytkownik ma możliwość zapisania rozwiązania parametry połączenia urządzenia, które zawiera poświadczenia podczas dodawania urządzenia. Ciąg połączenia urządzenia możesz uwzględnić w dalszej części tego samouczka aplikacji klienta.

Aby dodać urządzenie do zdalnego rozwiązanie monitorowania, wykonaj następujące czynności na **urządzeń** strony w rozwiązaniu:

1. Wybierz **+ nowe urządzenie**, a następnie wybierz pozycję **fizycznych** jako **typu urządzenia**:

    ![Zainicjuj obsługę urządzenia fizycznego](media/iot-suite-selector-connecting/devicesprovision.png)

1. Wprowadź **fizyczna Chłodnica** jako identyfikator urządzenia. Wybierz **klucza symetrycznego** i **automatycznego generowania kluczy** opcje:

    ![Wybierz opcje urządzenia](media/iot-suite-selector-connecting/devicesoptions.png)

1. Wybierz **zastosować**. Następnie zanotuj **identyfikator urządzenia**, **klucza podstawowego**, i **klucza podstawowego połączenia ciąg** wartości:

    ![Pobieranie poświadczeń](media/iot-suite-selector-connecting/credentials.png)

Aby zlokalizować poświadczeń, których urządzenie musi używać do łączenia się wstępnie skonfigurowane rozwiązanie, przejdź do portalu Azure w przeglądarce. Zaloguj się do subskrypcji.

Możesz mieć teraz udostępniane w monitorowania zdalnego urządzenia fizycznego wstępnie skonfigurowane rozwiązanie. W poniższych sekcjach można zaimplementować aplikacji klienckiej, który używa poświadczeń urządzenia do nawiązania połączenia rozwiązania.

Aplikacja kliencka implementuje wbudowane **Chłodnica** model urządzenia. Model urządzenia wstępnie skonfigurowane rozwiązanie określa następujące informacje o urządzeniu:

* Właściwości urządzenia raporty dla rozwiązania. Na przykład **Chłodnica** urządzenie raportuje informacje o jego oprogramowania układowego i lokalizacji.
* Typy danych telemetrycznych urządzenie wysyła do rozwiązania. Na przykład **Chłodnica** temperatury i wilgotności oraz wartości wykorzystania wysłania przez urządzenie.
* Metody można zaplanować z rozwiązania do uruchomienia na urządzeniu. Na przykład **Chłodnica** urządzenia musi implementować **ponowny rozruch**, **FirmwareUpdate**, **EmergencyValveRelease**, i  **IncreasePressure** metody.