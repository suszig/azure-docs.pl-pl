### <a name="prerequisites"></a>Wymagania wstępne
* A [usługi Yammer](https://www.yammer.com/) konta 

Aby korzystać z konta usługi Yammer, w aplikacji logiki, musisz autoryzować aplikacji logiki do łączenia się z kontem usługi Yammer. Na szczęście można w tym z aplikacji logiki w portalu Azure. 

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem usługi Yammer:

1. Aby utworzyć połączenie z usługi Yammer, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *usługi Yammer* w polu wyszukiwania. Wybierz wyzwalacz lub akcja będzie chcesz użyć:  
   ![](./media/connectors-create-api-yammer/yammer-1.png)
2. Jeśli nie utworzono wszystkie połączenia z usługi Yammer przed będzie zostanie wyświetlony monit o podanie poświadczeń usługi Yammer. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych konta usługi Yammer:  
   ![](./media/connectors-create-api-yammer/yammer-2.png)
3. Podaj Yammer nazwę użytkownika i hasło, aby zezwolić aplikacji logiki:  
   ![](./media/connectors-create-api-yammer/yammer-3.png)   
4. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki:  
   ![](./media/connectors-create-api-yammer/yammer-4.png)   

