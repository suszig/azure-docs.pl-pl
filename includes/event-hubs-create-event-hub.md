## Tworzenie centrum zdarzeń

1. Zaloguj się do [klasycznego portalu Azure][] i kliknij opcję **NOWE** u dołu ekranu.

2. Kliknij kolejno opcje **App Services**, **Service Bus**, **Centrum zdarzeń**, a następnie kliknij opcję **Szybkie tworzenie**.

    ![][1]

3. Wpisz nazwę centrum zdarzeń, wybierz odpowiedni region, a następnie kliknij opcję **Create a new Event Hub** (Utwórz nowe centrum zdarzeń).

    ![][2]

4. Jeśli istniejąca przestrzeń nazw nie została jawnie wybrana w danym regionie, portal tworzy przestrzeń nazw dla użytkownika (zwykle jest to ***nazwa centrum zdarzeń*-ns**). Kliknij przestrzeń nazw (w tym przykładzie **eventhub ns**).

    ![][3]

5. W dolnej części strony kliknij pozycję **Informacje o połączeniu**. Kliknij przycisk kopiowania (widoczny na rysunku poniżej), aby skopiować parametry połączenia **RootManageSharedAccessKey** do schowka. Zapisz te parametry połączenia, aby użyć ich w dalszej części samouczka.

    ![][4]

Utworzono centrum zdarzeń i uzyskano parametry połączenia potrzebne do wysyłania i odbierania zdarzeń.

[1]: ./media/event-hubs-create-event-hub/create-event-hub1.png
[2]: ./media/event-hubs-create-event-hub/create-event-hub2.png
[3]: ./media/event-hubs-create-event-hub/create-event-hub3.png
[4]: ./media/event-hubs-create-event-hub/create-conn-str1.png

[klasycznego portalu Azure]: https://manage.windowsazure.com/


<!--HONumber=sep16_HO1-->


