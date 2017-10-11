### <a name="prerequisites"></a>Wymagania wstępne
* Konto w serwisie Twitter 

Aby korzystać z konta w usłudze Twitter, w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem usługi Twitter. Na szczęście można w tym z aplikacji logiki w portalu Azure. 

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem usługi Twitter:

1. Aby utworzyć połączenie do usługi Twitter, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *Twitter* w polu wyszukiwania. Wybierz wyzwalacz lub akcja będzie chcesz użyć:  
   ![Obraz łączenia z serwisem Twitter 0](./media/connectors-create-api-twitter/twitter-0.png)
2. Jeśli nie utworzono wszystkie połączenia z serwisem Twitter przed będzie zostanie wyświetlony monit o podanie poświadczeń użytkownika usługi Twitter. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych konta usługi Twitter:  
   ![Obraz łączenia z serwisem Twitter 1](./media/connectors-create-api-twitter/twitter-1.png)  
3. Podaj Twitter, nazwę użytkownika i hasło, aby zezwolić aplikacji logiki:  
   ![Obraz łączenia z serwisem Twitter 2](./media/connectors-create-api-twitter/twitter-2.png)  
4. Upewnij się, uprawnienia użytkownika:  
   ![Obraz łączenia z serwisem Twitter 3](./media/connectors-create-api-twitter/twitter-3.png)  
5. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki:  
   ![Obraz łączenia z serwisem Twitter 4](./media/connectors-create-api-twitter/twitter-4.png)

