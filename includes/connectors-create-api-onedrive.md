#### <a name="prerequisites"></a>Wymagania wstępne
* Konto platformy Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)
* A [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) konta 

Zanim użyjesz kontem usługi OneDrive w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem usługi OneDrive.  Można to zrobić łatwo w aplikacjach logiki w portalu Azure. 

Autoryzuj aplikację logiki nawiązywania połączenia z kontem usługi OneDrive, wykonując następujące czynności:

1. Tworzenie aplikacji logiki. W Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na rozwijanej liście, a następnie wprowadź "onedrive" w polu wyszukiwania. Wybierz jedną z akcji lub wyzwalaczy:  
   ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Jeśli nie utworzono jeszcze żadnych połączeń do usługi OneDrive, zostanie wyświetlony monit o Zaloguj się przy użyciu poświadczeń usługi OneDrive:  
   ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Wybierz **Zaloguj**, a następnie wprowadź nazwę użytkownika i hasło. Wybierz **Zaloguj**:  
   ![](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Te poświadczenia są używane do autoryzacji aplikację logiki, aby połączyć się i uzyskać dostęp do danych na koncie usługi OneDrive. 
4. Wybierz **tak** do autoryzacji aplikację logiki, aby użyć konta usługi OneDrive:  
   ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Należy zauważyć, że utworzono połączenie. Teraz kontynuować pozostałe kroki w aplikacji logiki:  
   ![](./media/connectors-create-api-onedrive/onedrive-5.png)

