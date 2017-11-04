### <a name="prerequisites"></a>Wymagania wstępne
* [FTP](https://wikipedia.org/wiki/File_Transfer_Protocol) konta  

Aby korzystać z konta użytkownika FTP, w aplikacji logiki, musisz autoryzować aplikacji logiki do łączenia się z kontem FTP. Na szczęście można w tym z aplikacji logiki w portalu Azure.  

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem FTP:  

1. Aby utworzyć połączenie FTP, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *FTP* w polu wyszukiwania. Wybierz wyzwalacz lub akcja będzie chcesz użyć:  
   ![Krok tworzenia połączeń FTP](./media/connectors-create-api-ftp/ftp-1.png)  
2. Jeśli nie utworzono żadnych połączeń FTP przed będzie zostanie wyświetlony monit o podanie poświadczeń FTP. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych konta FTP:  
   ![Krok tworzenia połączeń FTP](./media/connectors-create-api-ftp/ftp-2.png)  
3. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki:  
   ![Krok tworzenia połączeń FTP](./media/connectors-create-api-ftp/ftp-3.png)  

