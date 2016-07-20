
1. Odwiedź [portal Azure]. Kliknij kolejno opcje **Przeglądaj wszystkie** > **Aplikacje mobilne** > nowo utworzoną wewnętrzną bazę danych. W ustawieniach aplikacji mobilnej kliknij kolejno opcje **Szybki start** > **Cordova**. W obszarze **Konfigurowanie aplikacji klienta** wybierz opcję **Utwórz nową aplikację**, a następnie kliknij przycisk **Pobierz**. Spowoduje to pobranie kompletnego projektu systemu Cordova dla aplikacji wstępnie skonfigurowanej do połączenia z wewnętrzną bazą danych.

2. Rozpakuj pobrany plik ZIP do katalogu na dysku twardym.

3. Otwórz projekt za pomocą środowiska **Visual Studio**.  Kliknij opcję **Otwórz** > **Projekt/Rozwiązanie...**.

4. Znajdź plik _sitename_.sln i kliknij opcję **Otwórz**.

5. Emulatorem domyślnym jest **Ripple — Nexus (Galaxy)**.  Kliknij strzałkę listy rozwijanej obok emulatora i wybierz opcję **Emulator systemu Google Android**.

6. Kliknij opcję **Emulator systemu Google Android**.  Projekt zostanie skompilowany, a następnie uruchomiony.  Ponieważ emulator systemu Google Android żąda dostępu do sieci, może pojawić się ostrzeżenie zabezpieczeń sieciowych.  Po pewnym czasie emulator systemu Google Android zostanie wyświetlony i aplikacja zostanie uruchomiona.

7. W aplikacji wpisz łatwy do rozpoznania tekst, np. _Ukończ samouczek_, a następnie kliknij przycisk Dodaj. To spowoduje wysłanie żądania POST do wdrożonej wcześniej wewnętrznej bazy danych Azure. Wewnętrzna baza danych wstawia dane z żądania do tabeli SQL TodoItem i zwraca informacje na temat przechowywanych od niedawna elementów do aplikacji mobilnej. W aplikacji mobilnej dane te są wyświetlane na liście.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)

[portal Azure]: https://portal.azure.com/



<!--HONumber=Jun16_HO2-->


