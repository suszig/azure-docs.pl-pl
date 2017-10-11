### <a name="prerequisites"></a>Wymagania wstępne
* A [SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) konta  

Zanim użyjesz konto SMTP w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem SMTP. Na szczęście można w tym z aplikacji logiki w portalu Azure.  

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem SMTP:  

1. Aby utworzyć połączenie SMTP, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *SMTP* w polu wyszukiwania. Wybierz wyzwalacz lub akcja będzie chcesz użyć:  
   ![](./media/connectors-create-api-smtp/smtp-1.png)  
2. Jeśli nie utworzono żadnych połączeń SMTP przed będzie zostanie wyświetlony monit o podanie poświadczeń SMTP. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych konta SMTP:  
   ![](./media/connectors-create-api-smtp/smtp-2.png)  
3. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki:  
   ![](./media/connectors-create-api-smtp/smtp-3.png)  

