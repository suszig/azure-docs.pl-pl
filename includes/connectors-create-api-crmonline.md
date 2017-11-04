#### <a name="prerequisites"></a>Wymagania wstępne
* Konto platformy Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)
* A [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) konta 

Przed użyciem Dynamics konta w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem CRM Online. Można to zrobić łatwo w aplikacjach logiki w portalu Azure. 

Zezwolić aplikacji logiki do łączenia się z kontem CRM Online, wykonując następujące czynności:

1. Tworzenie aplikacji logiki. W Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na rozwijanej liście, a następnie wprowadź "dynamics" w polu wyszukiwania. Wybierz jedną z akcji lub wyzwalaczy:  
   ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Jeśli nie utworzono jeszcze żadnych połączeń Dynamics, zostanie wyświetlony monit o Zaloguj się przy użyciu poświadczeń Dynamics:  
   ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Wybierz **Zaloguj**, a następnie wprowadź nazwę użytkownika i hasło. Wybierz **Zaloguj**. 
   
    Te poświadczenia są używane do autoryzacji aplikację logiki, aby połączyć się i uzyskać dostęp do danych na koncie Dynamics. 
4. Należy zauważyć, że utworzono połączenie. Teraz kontynuować pozostałe kroki w aplikacji logiki:  
   ![](./media/connectors-create-api-crmonline/dynamics-properties.png)

