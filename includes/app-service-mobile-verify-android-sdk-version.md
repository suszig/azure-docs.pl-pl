Z powodu bieżących prac wersji zestawu SDK systemu Android w programie Android Studio nie może być zgodna z wersją w kodzie. Zestaw SDK systemu Android, do którego odwołuje się ten samouczek jest wersja 23 r w czasie zapisywania. Numer wersji może zwiększyć pojawiają się nowe wersje zestawu SDK, a firma Microsoft zaleca używanie najnowszej dostępnej wersji.

Są dwa objawy niezgodność wersji:

- Podczas tworzenia lub Odbuduj projekt może zostać Gradle komunikaty, takie jak "**nie można znaleźć elementu docelowego Google Inc.:Google APIs:n**".
- Standard Android obiektów w kodzie, który powinien być rozpoznawany na podstawie `import` instrukcje generujących komunikaty o błędach.

Jeśli zostanie wyświetlona dowolna z tych, wersja zestawu SDK systemu Android zainstalowane w programie Android Studio nie może odpowiadać docelowego zestawu SDK z pobranego projektu. Aby sprawdzić wersję, wprowadź następujące zmiany:

1. W programie Android Studio, kliknij przycisk **narzędzia** > **Android** > **SDK Manager**. Jeśli nie zainstalowano najnowszej wersji zestawu SDK platformy, następnie kliknij, aby go zainstalować. Zanotuj numer wersji.
2. Na **Eksplorator projektów** , w obszarze **skryptów narzędzia Gradle**, otwórz plik **build.gradle (modeule: aplikacji)**. Upewnij się, że **compileSdkVersion** i **buildToolsVersion** są ustawione, aby zainstalować najnowszą wersję zestawu SDK. Tagi może wyglądać następująco:

             compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
3. W Eksploratorze projektu systemu Android Studio kliknij prawym przyciskiem myszy węzeł projektu, wybierz **właściwości**, a w kolumnie po lewej stronie wybierz **Android**. Upewnij się, że **docelowym kompilacji projektu** ustawiono do tej samej wersji zestawu SDK jako **targetSdkVersion**.

W programie Android Studio pliku manifestu już jest używany do określania docelowych zestawu SDK i minimalna wersja zestawu SDK, w przeciwieństwie do sprawę z Eclipse.
