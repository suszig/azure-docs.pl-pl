
1. Odwiedź [portal Azure] przy użyciu komputera Mac. Kliknij kolejno opcje **Przeglądaj wszystkie** > **Aplikacje mobilne** > nowo utworzoną wewnętrzną bazę danych. W ustawieniach aplikacji mobilnej kliknij kolejno opcje **Szybki start** > **iOS (Objective-C)**. Jeśli preferujesz język Swift, zamiast tego kliknij kolejno opcje **Szybki start** > **iOS (Swift)**. W obszarze **Pobierz i uruchom projekt w systemie iOS** kliknij opcję **Pobierz**. Spowoduje to pobranie kompletnego projektu systemu Xcode dla aplikacji wstępnie skonfigurowanej do połączenia z wewnętrzną bazą danych. Otwórz projekt za pomocą środowiska Xcode.
2. Naciśnij przycisk **Uruchom**, aby skompilować projekt i uruchomić aplikację w symulatorze systemu iOS.
3. W aplikacji wpisz łatwy do rozpoznania tekst (np. *Ukończenie samouczka*), a następnie kliknij ikonę plusa (**+**). To spowoduje wysłanie żądania POST do wdrożonej wcześniej wewnętrznej bazy danych Azure. Wewnętrzna baza danych wstawia dane z żądania do tabeli SQL TodoItem i zwraca informacje na temat przechowywanych od niedawna elementów do aplikacji mobilnej. W aplikacji mobilnej dane te są wyświetlane na liście. 
   
    ![](./media/mobile-services-ios-run-app/mobile-quickstart-startup-ios.png)

[portal Azure]: https://portal.azure.com/



<!--HONumber=Jun16_HO2-->


