### <a name="prerequisites"></a>Wymagania wstępne
* A [Salesforce](https://salesforce.com) konta  

Aby korzystać z konta usług Salesforce, w aplikacji logiki, musisz autoryzować aplikacji logiki do łączenia się z kontem usługi Salesforce. Na szczęście można w tym z aplikacji logiki w portalu Azure.  

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem Salesforce:  

1. Aby utworzyć połączenie usługi Salesforce, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *Salesforce* w polu wyszukiwania. Wybierz wyzwalacz lub akcja będzie chcesz użyć:  
   ![Obraz połączenia usługi SalesForce 1](./media/connectors-create-api-salesforce/salesforce-1.png)  
2. Jeśli nie utworzono wszystkie połączenia z Salesforce przed będzie zostanie wyświetlony monit o podanie poświadczeń usługi Salesforce. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do konta usług Salesforce danych:  
   ![Obraz połączenia usługi SalesForce 2](./media/connectors-create-api-salesforce/salesforce-2.png)  
3. Dostarcza usług Salesforce, nazwę użytkownika i hasło, aby zezwolić aplikacji logiki:  
   ![Obraz połączenia usługi SalesForce 3](./media/connectors-create-api-salesforce/salesforce-3.png)  
4. Zezwalaj firmie Microsoft w celu nawiązania połączenia usługi Salesforce:  
   ![Obraz połączenia usługi SalesForce 4](./media/connectors-create-api-salesforce/salesforce-4.png)  
5. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki:  
   ![Obraz połączenia usługi SalesForce 5](./media/connectors-create-api-salesforce/salesforce-5.png)  

