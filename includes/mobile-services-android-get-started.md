Ostatnim krokiem tego samouczka jest skompilowanie i uruchomienie nowej aplikacji.

### Załadowanie projektu do programu Android Studio i synchronizacja narzędzia Gradle
1. Przejdź do lokalizacji, w której zapisano skompresowane pliki projektu i wyodrębnij je na komputerze w katalogu projektów programu Android Studio.
2. Otwórz program Android Studio. Jeśli pracujesz nad projektem i jest on widoczny, zamknij projekt [File (Plik) => Close Project (Zamknij projekt)].
3. Wybierz opcję **Open an existing Android Studio project** (Otwórz istniejący projekt Android Studio), przejdź do lokalizacji projektu, a następnie kliknij przycisk **OK**. Spowoduje to załadowanie projektu i rozpoczęcie jego synchronizacji z narzędziem Gradle.
   
    ![](./media/mobile-services-android-get-started/android-studio-import-project.png)
4. Poczekaj, aż synchronizacja z narzędziem Gradle zostanie zakończona. Jeśli zostanie wyświetlony komunikat o błędzie dotyczący braku możliwości odnalezienia obiektu docelowego, będzie to spowodowane tym, że wersja używana w programie Android Studio nie jest zgodna z wersją próbki. Najłatwiejszym sposobem rozwiązania tego problemu jest kliknięcie linku **Install missing platform(s) and sync project** (Zainstaluj brakujące platformy i zsynchronizuj projekt) w komunikacie o błędzie. Mogą występować inne komunikaty o błędach dotyczące wersji, więc po prostu powtarzaj tę operację, aż błędy przestaną się pojawiać.
   
   * Istnieje inny sposób rozwiązania tego problemu, jeśli chcesz uruchamiać aplikację za pomocą „najnowszej i najlepszej” wersji systemu Android. Możesz zaktualizować właściwość **targetSdkVersion** w pliku *build.gradle* w katalogu *aplikacji* w celu dopasowania jej do wersji już zainstalowanej na komputerze, którą można sprawdzić, klikając ikonę **SDK Manager**. Następnie kliknij pozycję **Sync Project with Gradle Files** (Synchronizuj projekt z plikami narzędzia Gradle). Może zostać wyświetlony komunikat o błędzie dotyczący wersji narzędzi Build Tools. Ten problem można naprawić w taki sam sposób.

### Uruchamianie aplikacji
Aplikację można uruchomić przy użyciu emulatora lub rzeczywistego urządzenia.

1. Aby uruchomić aplikację na urządzeniu, podłącz je do komputera za pomocą kabla USB. Musisz [skonfigurować urządzenie do programowania](https://developer.android.com/training/basics/firstapp/running-app.html). Jeśli programowanie jest realizowane na urządzeniu z systemem Windows, musisz również pobrać i zainstalować sterownik USB.
2. Aby uruchomić aplikację w emulatorze systemu Android, musisz zdefiniować co najmniej jedno urządzenie wirtualne z systemem Android. Kliknij ikonę AVD Manager (Menedżer urządzeń wirtualnych z systemem Android), aby utworzyć te urządzenia i nimi zarządzać.
3. W menu **Run** (Uruchamianie) kliknij pozycję **Run** (Uruchom), aby uruchomić projekt. W wyświetlonym oknie dialogowym wybierz urządzenie lub emulator.
4. Po wyświetleniu aplikacji wpisz łatwy do rozpoznania tekst, na przykład *Ukończenie samouczka*, a następnie kliknij przycisk **Add** (Dodaj).
   
    ![](./media/mobile-services-android-get-started/mobile-quickstart-startup-android.png)
   
    Spowoduje to wysłanie żądania POST do nowej usługi mobilnej, która jest hostowana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez usługę mobilną, a dane są wyświetlane na liście.
   
   > [!NOTE]
   > Możesz przejrzeć kod uzyskujący dostęp do usługi mobilnej w celu wykonywania zapytań i wstawiania danych. Znajduje się on w pliku ToDoActivity.java.
   > 
   > 
5. W klasycznym portalu Azure kliknij kartę **Dane**, a następnie kliknij tabelę **TodoItems**.
   
    ![](./media/mobile-services-android-get-started/mobile-data-tab1.png)
   
    Pozwoli to przeglądać dane wstawiane przez aplikację do tabeli.
   
    ![](./media/mobile-services-android-get-started/mobile-data-browse.png)

<!--HONumber=Sep16_HO3-->


