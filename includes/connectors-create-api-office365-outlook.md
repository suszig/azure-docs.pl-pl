#### <a name="prerequisites"></a>Wymagania wstępne
* Konto platformy Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)
* [Usługi Office 365](https://office365.com) konta  

Przed rozpoczęciem korzystania z konta usługi Office 365 w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem usługi Office 365. Można to zrobić łatwo w aplikacjach logiki w portalu Azure.  

Zezwolić aplikacji logiki do nawiązania połączenia konta usługi Office 365, wykonując następujące czynności:

1. Tworzenie aplikacji logiki. W Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na rozwijanej liście, a następnie wprowadź "office 365" w polu wyszukiwania. Wybierz jedną z akcji lub wyzwalaczy:  
    ![Krok tworzenia połączenia usługi Office 365](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  
2. Jeśli nie utworzono jeszcze wszystkie połączenia z usługą Office 365, zostanie wyświetlony monit o Zaloguj się przy użyciu poświadczeń konta usługi Office 365:  
    ![Krok tworzenia połączenia usługi Office 365](./media/connectors-create-api-office365-outlook/office365-signin.png)  
3. Wybierz **Zaloguj**, a następnie wprowadź nazwę użytkownika i hasło. Wybierz **Zaloguj**:  
    ![Krok tworzenia połączenia usługi Office 365](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)
   
    Te poświadczenia są używane do autoryzacji aplikację logiki, aby połączyć się i uzyskać dostęp do konta usługi Office 365. 
4. Należy zauważyć, że utworzono połączenie. Teraz kontynuować pozostałe kroki w aplikacji logiki:   
    ![Krok tworzenia połączenia usługi Office 365](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  

