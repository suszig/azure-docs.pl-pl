### <a name="prerequisites"></a>Wymagania wstępne
* [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol) konta  

Przed użyciem protokołu SFTP konta w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem SFTP. Na szczęście można w tym z aplikacji logiki w portalu Azure.  

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem protokołu SFTP:  

1. Aby utworzyć połączenie z użyciem protokołu SFTP, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *SFTP* w polu wyszukiwania. Wybierz **SFTP - podczas dodawania lub modyfikowania pliku** wyzwalacz:  
   ![Obraz połączenia w trybie online SFTP 1](./media/connectors-create-api-sftp/sftp-1.png)  
2. Jeśli nie utworzono wszystkie połączenia z użyciem protokołu SFTP przed będzie zostanie wyświetlony monit o podanie poświadczeń SFTP. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych konta protokołu SFTP:  
   ![Obraz połączenia w trybie online SFTP 2](./media/connectors-create-api-sftp/sftp-2.png)  
3. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki:   
   ![Obraz połączenia w trybie online SFTP 3](./media/connectors-create-api-sftp/sftp-3.png) 

