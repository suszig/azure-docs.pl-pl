### <a name="prerequisites"></a>Wymagania wstępne
* A [pole](http://box.com) konta  

Zanim użyjesz swojego konta pole w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem pole. Na szczęście można w tym z aplikacji logiki w portalu Azure.  

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem pola:  

1. Aby utworzyć połączenie do pola, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *pole* w polu wyszukiwania. Wybierz wyzwalacz lub akcja będzie chcesz użyć:  
   ![krok tworzenia połączenia pole](./media/connectors-create-api-box/box-1.png)  
2. Jeśli nie utworzono wszystkie połączenia z polu przed będzie zostanie wyświetlony monit o podanie poświadczeń pole. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do konta pola danych:  
   ![krok tworzenia połączenia pole](./media/connectors-create-api-box/box-2.png)  
3. Podaj pole nazwę użytkownika i hasło, aby zezwolić aplikacji logiki:  
   ![krok tworzenia połączenia pole](./media/connectors-create-api-box/box-3.png)  
4. Zezwalaj firmie Microsoft w celu nawiązania połączenia pola:  
   ![krok tworzenia połączenia pole](./media/connectors-create-api-box/box-4.png)  
5. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki:  
   ![krok tworzenia połączenia pole](./media/connectors-create-api-box/box-5.png)  

