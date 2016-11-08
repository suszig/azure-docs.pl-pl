
Pobrany projekt usługi mobilnej umożliwia uruchamianie nowej usługi mobilnej bezpośrednio na komputerze lokalnym lub w maszynie wirtualnej. Ułatwia to debugowanie kodu usługi przed opublikowaniem go na platformie Azure.

W tej sekcji opisano testowanie nowej aplikacji z usługą mobilną działającą lokalnie.

1. Przejdź do lokalizacji, w której zapisano skompresowane pliki projektu, rozwiń pliki na komputerze, a następnie w programie Visual Studio otwórz plik rozwiązania.
2. W Eksploratorze rozwiązań w programie Visual Studio kliknij prawym przyciskiem myszy projekt usługi, kliknij opcję **Ustaw jako projekt startowy**, a następnie naciśnij klawisz **F5**, aby skompilować projekt i uruchomić usługę mobilną lokalnie.
   
    ![](./media/mobile-services-dotnet-backend-test-local-service-dotnet/mobile-service-startup.png)
   
    Po pomyślnym uruchomieniu usługi mobilnej zostanie wyświetlona strona sieci Web.
3. Aby testować aplikację ze sklepu, kliknij prawym przyciskiem myszy projekt aplikacji klienta, kliknij opcję **Ustaw jako projekt startowy**, a następnie naciśnij klawisz **F5**, aby ponownie skompilować projekt i uruchomić aplikację.
   
    Spowoduje to uruchomienie aplikacji, która połączy się z lokalnym wystąpieniem usługi mobilnej.   
4. W aplikacji wpisz łatwy do rozpoznania tekst (np. *Ukończenie samouczka*) w polu tekstowym **Insert a TodoItem** (Wstaw zadanie do wykonania), a następnie kliknij pozycję **Save** (Zapisz).
   
    Spowoduje to wysłanie żądania POST do lokalnej usługi mobilnej. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez usługę mobilną, a dane są wyświetlane w drugiej kolumnie aplikacji.

<!--HONumber=Sep16_HO3-->


