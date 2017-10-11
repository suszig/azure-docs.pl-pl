### <a name="prerequisites"></a>Wymagania wstępne
* A [SendGrid](https://www.SendGrid.com/) konta 

Zanim użyjesz swojego konta włączenie w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem SendGrid. Na szczęście można w tym z aplikacji logiki w portalu Azure. 

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem SendGrid:

1. Aby utworzyć połączenie SendGrid, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *SendGrid* w polu wyszukiwania. Wybierz wyzwalacz lub akcja będzie chcesz użyć:  
   ![SendGrid krok 1](./media/connectors-create-api-sendgrid/sendgrid-1.png)
2. Jeśli nie utworzono żadnych połączeń SendGrid przed będzie zostanie wyświetlony monit o podanie poświadczeń SendGrid. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych konta SendGrid:  
   ![SendGrid krok 2](./media/connectors-create-api-sendgrid/sendgrid-2.png)
3. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki:  
   ![SendGrid krok 3](./media/connectors-create-api-sendgrid/sendgrid-3.png)   

