Z powodu bieżących prac wersji zestawu SDK systemu Android w programie Android Studio nie może być zgodna z wersją w kodzie. Zestaw SDK systemu Android, do którego odwołuje się ten samouczek jest wersja 26, r w czasie zapisywania. Numer wersji może zwiększyć pojawiają się nowe wersje zestawu SDK, a firma Microsoft zaleca używanie najnowszej dostępnej wersji.

Są dwa objawy niezgodność wersji:

- Podczas tworzenia lub Odbuduj projekt może zostać komunikaty o błędach narzędzia Gradle jak `Gradle sync failed: Failed to find target with hash string 'android-XX'`.
- Standard Android obiektów w kodzie, który powinien być rozpoznawany na podstawie `import` instrukcje generujących komunikaty o błędach.

Jeśli zostanie wyświetlona dowolna z tych, wersja zestawu SDK systemu Android zainstalowane w programie Android Studio nie może odpowiadać docelowego zestawu SDK z pobranego projektu. Aby sprawdzić wersję, wprowadź następujące zmiany:

1. W programie Android Studio, kliknij przycisk **narzędzia** > **Android** > **SDK Manager**. Jeśli nie zainstalowano najnowszej wersji zestawu SDK platformy, następnie kliknij, aby go zainstalować. Zanotuj numer wersji.

2. Na **Eksplorator projektów** , w obszarze **skryptów narzędzia Gradle**, otwórz plik **build.gradle (moduł: aplikacji)**. Upewnij się, że **compileSdkVersion** i **targetSdkVersion** są ustawione, aby zainstalować najnowszą wersję zestawu SDK. `build.gradle` Może wyglądać następująco:

    ```gradle
    android {
        compileSdkVersion 26
        defaultConfig {
            targetSdkVersion 26
        }
    }
    ```
