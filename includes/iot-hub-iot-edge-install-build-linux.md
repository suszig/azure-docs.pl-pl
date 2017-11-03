## <a name="install-the-prerequisites"></a>Zainstaluj wymagania wstępne

Kroki opisane w tym samouczku przyjęto założenie, że używasz Ubuntu Linux.

Aby zainstalować wstępnie wymagane pakiety, Otwórz powłokę i uruchom następujące polecenia:

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake pkg-config libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

W powłoce, uruchom następujące polecenie, można sklonować repozytorium GitHub krawędzi IoT Azure na komputerze lokalnym:

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-to-build-the-sample"></a>Jak skompilować przykład

Krawędź IoT można teraz tworzyć środowiska uruchomieniowego i przykłady, na komputerze lokalnym:

1. Otwórz powłokę.

1. Przejdź do folderu głównego w lokalnej kopii repozytorium **iot-edge**.

1. Uruchom skrypt kompilacji w następujący sposób:

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

Ten skrypt używa narzędzia **cmake** do utworzenia folderu o nazwie **build** w folderze głównym lokalnej kopii repozytorium **iot-edge** i wygenerowania pliku reguł programu make. Skrypt następnie kompiluje rozwiązanie, pomijając testy jednostkowe i testy kompleksowe. Jeśli chcesz tworzenia i uruchamiania testów jednostkowych, dodać `--run-unittests` parametru. Aby tworzenie i Uruchamianie testów kompleksowe, dodać `--run-e2e-tests`.

> [!NOTE]
> Za każdym razem, gdy zostanie uruchomiony skrypt **build.sh**, folder **build** jest usuwany i tworzony ponownie w folderze głównym lokalnej kopii repozytorium **iot-edge**.