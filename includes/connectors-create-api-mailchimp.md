### <a name="prerequisites"></a>Wymagania wstępne
* A [MailChimp](https://www.MailChimp.com/) konta 

Zanim użyjesz swojego konta MailChimp w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem MailChimp. Na szczęście można w tym z aplikacji logiki w portalu Azure. 

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem MailChimp:

1. Aby utworzyć połączenie MailChimp, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *MailChimp* w polu wyszukiwania. Wybierz wyzwalacz lub akcja będzie chcesz użyć:  
   ![MailChimp w kroku 1](./media/connectors-create-api-mailchimp/mailchimp-1.png)
2. Jeśli nie utworzono żadnych połączeń MailChimp przed będzie zostanie wyświetlony monit o podanie poświadczeń MailChimp. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych konta MailChimp:  
   ![MailChimp krok 2](./media/connectors-create-api-mailchimp/mailchimp-2.png)
3. Podaj MailChimp nazwę użytkownika i hasło, aby zezwolić aplikacji logiki:  
   ![MailChimp w kroku 3](./media/connectors-create-api-mailchimp/mailchimp-3.png)   
4. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki:  
   ![MailChimp w kroku 4](./media/connectors-create-api-mailchimp/mailchimp-4.png)

