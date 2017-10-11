### <a name="prerequisites"></a>Wymagania wstępne
* A [Dropbox](https://www.Dropbox.com/) konta 

Zanim użyjesz swojego konta Dropbox w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem usługi Dropbox. Na szczęście można w tym z aplikacji logiki w portalu Azure. 

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem usługi Dropbox:

1. Aby utworzyć połączenie usługi Dropbox, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *Dropbox* w polu wyszukiwania. Wybierz wyzwalacz lub akcja będzie chcesz użyć:  
   ![Krok Dropbox 1](./media/connectors-create-api-dropbox/dropbox-1.png)
2. Jeśli nie utworzono wszystkie połączenia z usługi Dropbox, będzie zostanie wyświetlony monit o podanie poświadczeń usługi Dropbox. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych konta Dropbox:  
   ![Krok Dropbox 2](./media/connectors-create-api-dropbox/dropbox-2.png)
3. Podaj Dropbox nazwę użytkownika i hasło, aby zezwolić aplikacji logiki:  
   ![Dropbox krok 3](./media/connectors-create-api-dropbox/dropbox-3.png)   
4. Autoryzuj aplikację logiki, aby użyć konta Dropbox:  
   ![Dropbox krok 4](./media/connectors-create-api-dropbox/dropbox-4.png)
5. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki:  
   ![Dropbox krok 5](./media/connectors-create-api-dropbox/dropbox-5.png)   

