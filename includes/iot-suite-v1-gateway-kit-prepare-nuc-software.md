## <a name="build-iot-edge"></a>Tworzenie krawędzi IoT

Ten samouczek używa niestandardowe moduły krawędzi IoT do komunikowania się ze zdalnym wstępnie skonfigurowane rozwiązanie monitorowania. W związku z tym jest potrzebne do tworzenia krawędzi IoT modułów z kodu źródła niestandardowego. W poniższych sekcjach opisano sposób instalowania krawędzi IoT i utworzenie niestandardowego modułu krawędzi IoT.

### <a name="install-iot-edge"></a>Zainstaluj krawędzi IoT

W poniższych krokach opisano sposób instalowania oprogramowania wstępnie skompilowanym krawędzi IoT na Intel NUC:

1. Skonfiguruj repozytoria wymagany pakiet inteligentne, uruchamiając następujące polecenia na Intel NUC:

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    Wprowadź `y` gdy pojawi się monit o polecenie **obejmują ten kanał?**.

1. Aktualizacja Menedżera pakietów inteligentne, uruchamiając następujące polecenie:

    ```bash
    smart update
    ```

1. Zainstaluj pakiet Azure IoT krawędzi, uruchamiając następujące polecenie:

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. Weryfikowanie instalacji przez uruchomienie przykładu "Hello world". W tym przykładzie zapisuje wiadomość hello world pliku log.txT co pięć sekund. Poniższe polecenia są uruchamiane przykładu "Hello world":

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    Ignoruj wszystkie **nieprawidłowy argument** wiadomości po zatrzymaniu próbki.

    Aby wyświetlić zawartość pliku dziennika, użyj następującego polecenia:

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli zostanie wyświetlony błąd "nie zawiera util-linux deweloperów", spróbuj wykonać ponowny rozruch Intel NUC.
