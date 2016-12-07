
1. Odwiedź witrynę [Azure Portal]. Kliknij kolejno opcje **Przeglądaj wszystkie** > **Aplikacje mobilne** > nowo utworzoną wewnętrzną bazę danych. W ustawieniach aplikacji mobilnej kliknij kolejno opcje **Szybki start** > **Cordova**. W obszarze **Konfigurowanie aplikacji klienta** wybierz opcję **Utwórz nową aplikację**, a następnie kliknij przycisk **Pobierz**. Spowoduje to pobranie kompletnego projektu systemu Cordova dla aplikacji wstępnie skonfigurowanej do połączenia z wewnętrzną bazą danych.
2. Rozpakuj pobrany plik ZIP do katalogu na dysku twardym, przejdź do pliku rozwiązań (sln) i otwórz go za pomocą programu Visual Studio.
3. W programie Visual Studio z listy rozwijanej znajdującej się obok strzałki rozpoczęcia wybierz platformę rozwiązania (Android, iOS lub Windows), a następnie wybierz urządzenie określonego wdrożenia lub emulator, klikając przycisk listy rozwijanej na zielonej strzałce. Należy pamiętać, że można użyć domyślnej platformy Android i emulatora Ripple. Samouczki bardziej zaawansowane będą wymagać, aby wybrać obsługiwane urządzenie lub emulator. 
4. Naciśnij klawisz F5 lub kliknij zieloną strzałkę, aby skompilować i uruchomić aplikację systemu Cordova. Jeśli w emulatorze widzisz okno dialogowe zabezpieczeń żądające dostępu do sieci, zaakceptuj je.   
5. Po uruchomieniu aplikacji na urządzeniu lub w emulatorze wpisz znaczący tekst w obszarze **Wprowadź nowy tekst**, taki jak *Ukończ samouczek*, a następnie kliknij przycisk **Dodaj**.  
   To spowoduje wysłanie żądania POST do wdrożonego wcześniej zaplecza platformy Azure. Zaplecze wstawia dane z żądania do tabeli TodoItem bazy danych SQL Database i zwraca do aplikacji mobilnej informacje na temat przechowywanych od niedawna elementów. W aplikacji mobilnej dane te są wyświetlane na liście.
   
    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
6. Powtórz poprzednie trzy kroki dla każdej platformy urządzenia, która ma być obsługiwana.

[Azure Portal]: https://portal.azure.com/


<!--HONumber=Nov16_HO2-->


