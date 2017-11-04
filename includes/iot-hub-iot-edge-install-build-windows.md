## <a name="install-the-prerequisites"></a>Zainstaluj wymagania wstępne

1. Zainstaluj [programu Visual Studio 2015 lub 2017](https://www.visualstudio.com). Bezpłatna wersja Community można użyć, jeśli spełniają wymagania licencyjne. Należy uwzględnić Visual C++ i Menedżer pakietów NuGet.

1. Zainstaluj [git](http://www.git-scm.com) i upewnij się, że git.exe można uruchomić z wiersza polecenia.

1. Zainstaluj [CMake](https://cmake.org/download/) i upewnij się, że cmake.exe można uruchomić z wiersza polecenia. CMake wersji 3.7.2 lub nowszy jest zalecane. **.Msi** Instalator jest najłatwiejsza opcja w systemie Windows. Dodaj CMake ścieżkę do co najmniej bieżącego użytkownika, gdy Instalator wyświetli monit.

1. Zainstaluj [Python 2.7](https://www.python.org/downloads/release/python-27). Upewnij się, że dodawanie Python do programu `PATH` zmiennej środowiskowej. Przejdź do **Panelu sterowania** > **System i zabezpieczenia** > **systemu** > **Zaawansowane ustawienia systemu**  >  **Zmiennych środowiskowych**. Dodaj `C:\Python27` do ścieżki. 

1. W wierszu polecenia Uruchom następujące polecenie, aby Klonuj repozytorium GitHub krawędzi IoT Azure na komputerze lokalnym:

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-to-build-the-sample"></a>Jak skompilować przykład

Krawędź IoT można teraz tworzyć środowiska uruchomieniowego i przykłady, na komputerze lokalnym:

1. Otwórz **wiersz polecenia dla programu VS 2015 deweloperów** lub **wiersz polecenia dla programu VS 2017 deweloperów**w zależności od wersji.

1. Przejdź do folderu głównego w lokalnej kopii repozytorium **iot-edge**.

1. Uruchom skrypt kompilacji w następujący sposób:

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

Ten skrypt tworzy plik rozwiązania Visual Studio i tworzy rozwiązanie. Możesz znaleźć rozwiązania Visual Studio w **kompilacji** folderu w lokalnej kopii **krawędzi iot** repozytorium. Jeśli chcesz tworzenia i uruchamiania testów jednostkowych, dodać `--run-unittests` parametru. Aby tworzenie i Uruchamianie testów kompleksowe, dodać `--run-e2e-tests`.

> [!NOTE]
> Każdym uruchomieniu **build.cmd** skryptu, go usuwa, a następnie tworzy ponownie **kompilacji** folderu w folderze głównym lokalną kopię **krawędzi iot** repozytorium.