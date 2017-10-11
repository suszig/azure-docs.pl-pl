## <a name="configure-the-nodejs-simulated-device"></a>Konfigurowanie środowiska Node.js symulowane urządzenie
1. Na pulpicie nawigacyjnym monitorowania zdalnego kliknij **+ Dodaj urządzenie** , a następnie dodaj *urządzeń niestandardowych*. Zanotuj Centrum IoT nazwy hosta, identyfikator urządzenia i klucz urządzenia. Należy je później w tym samouczku podczas przygotowywania remote_monitoring.js aplikację klienta dla urządzeń.
2. Upewnij się, że wersji środowiska Node.js 0.12.x lub nowszy jest zainstalowany na komputerze deweloperskim. Uruchom `node --version` w wierszu polecenia lub w powłoce, aby sprawdzić wersję. Aby uzyskać informacje dotyczące instalowania programu Node.js w systemie Linux przy użyciu Menedżera pakietów, zobacz [instalowanie Node.js za pomocą Menedżera pakietów][node-linux].
3. Po zainstalowaniu programu Node.js w klonowania najnowszą wersję [azure iot-sdk węzłami] [ lnk-github-repo] repozytorium na komputerze deweloperskim. Zawsze używaj **wzorca** gałęzi do najnowszej wersji biblioteki i przykłady.
4. Z lokalną kopię [azure iot-sdk węzłami] [ lnk-github-repo] repozytorium, pliki kopii dwa z folderu węzła/urządzenia/próbek do pustego folderu na komputerze deweloperskim:
   
   * Packages.JSON
   * remote_monitoring.js
5. Otwórz plik remote_monitoring.js i poszukaj następujących definicji zmiennej:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Zastąp **[parametry połączenia Centrum IoT urządzenia]** z parametrów połączenia urządzenia. Użyj wartości dla nazwy hosta Centrum IoT, identyfikator urządzenia i klucz urządzenia, który należy zanotować w kroku 1. Ciąg połączenia urządzenia ma następujący format:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Jeśli nazwa hosta z Centrum IoT jest **contoso** i identyfikator urządzenia jest **mydevice**, ciąg połączenia prawdopodobnie następujący fragment kodu:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Zapisz plik. Uruchom następujące polecenia w powłoce lub wiersza polecenia w folderze, który zawiera te pliki, aby zainstalować wymagane pakiety, a następnie uruchom przykładową aplikację:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Obserwować dynamiczne telemetrii w akcji
Pulpitu nawigacyjnego przedstawia dane telemetryczne temperatury i wilgotności z istniejących urządzeń symulowanego:

![Domyślny pulpit nawigacyjny][image1]

W przypadku wybrania symulowane urządzenie Node.js, który działał w poprzedniej sekcji, zobacz temperatury, wilgotności i dane telemetryczne temperatury zewnętrznych:

![Dodaj zewnętrzne temperatury do pulpitu nawigacyjnego][image2]

Rozwiązanie monitorowania zdalnego automatycznie wykrywa typ telemetrii dodatkowe temperatury zewnętrznych i dodaje go do wykresu w pulpicie nawigacyjnym.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png