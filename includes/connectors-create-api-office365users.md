### <a name="prerequisites"></a>Wymagania wstępne
* [Użytkowników usługi Office 365](https://office365.com) konta  

Aby korzystać z konta użytkowników usługi Office 365, w aplikacji logiki, musisz autoryzować aplikację logiki, aby połączyć się konta użytkowników usługi Office 365. Na szczęście można w tym z aplikacji logiki w portalu Azure.  

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem użytkowników usługi Office 365:  

1. Aby utworzyć połączenie do użytkowników usługi Office 365, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *użytkowników usługi Office 365* w polu wyszukiwania. Wybierz wyzwalacz lub akcja będzie chcesz użyć:  
   ![Krok tworzenia połączenia użytkowników usługi Office 365](./media/connectors-create-api-office365users/office365users-1.png)  
2. Jeśli nie utworzono żadnych połączeń użytkowników usługi Office 365 przed będzie zostanie wyświetlony monit o podanie poświadczeń użytkowników usługi Office 365. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych konta użytkowników usługi Office 365:  
   ![Krok tworzenia połączenia użytkowników usługi Office 365](./media/connectors-create-api-office365users/office365users-2.png)  
3. Podaj użytkowników usługi Office 365, nazwę użytkownika i hasło, aby zezwolić aplikacji logiki:  
   ![Krok tworzenia połączenia użytkowników usługi Office 365](./media/connectors-create-api-office365users/office365users-3.png)  
4. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki:  
   ![Krok tworzenia połączenia użytkowników usługi Office 365](./media/connectors-create-api-office365users/office365users-4.png)  

