
Ostatnim krokiem tego samouczka jest skompilowanie i uruchomienie nowej aplikacji.

1. Przejdź do lokalizacji, w której zapisano skompresowane pliki projektu, rozwiń pliki na komputerze, a następnie w programie Visual Studio otwórz plik rozwiązania.
2. Naciśnij klawisz **F5**, aby ponownie skompilować projekt i uruchomić aplikację.
3. W aplikacji wpisz łatwy do rozpoznania tekst (np. *Ukończenie samouczka*) w polu tekstowym **Insert a TodoItem** (Wstaw zadanie do wykonania), a następnie kliknij pozycję **Save** (Zapisz).
   
    Spowoduje to wysłanie żądania POST do nowej usługi mobilnej, która jest hostowana na platformie Azure. Dane z żądania zostaną wstawione do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez usługę mobilną, a dane są wyświetlane w drugiej kolumnie aplikacji.
4. (Opcjonalnie) W uniwersalnym rozwiązaniu systemu Windows zmień domyślny projekt startowy na inną aplikację i ponownie uruchom aplikację.
   
    Sprawdź, czy dane zapisane w poprzednim kroku są ładowane z usługi mobilnej po uruchomieniu aplikacji.
5. W [klasycznym portalu Azure](https://manage.windowsazure.com/) kliknij kartę **Dane**, a następnie kliknij tabelę **TodoItems**.
   
    Pozwoli to przeglądać dane wstawiane przez aplikację do tabeli.
   
    ![](./media/mobile-services-javascript-backend-run-app/mobile-data-browse.png)

<!--HONumber=Sep16_HO3-->


