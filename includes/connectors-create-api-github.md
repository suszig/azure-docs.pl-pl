### <a name="prerequisites"></a>Wymagania wstępne
* A [GitHub](http://GitHub.com) konta 

Zanim użyjesz konto GitHub w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem usługi GitHub. Na szczęście można w tym z aplikacji logiki w portalu Azure. 

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem usługi GitHub:

1. Aby utworzyć połączenie do usługi GitHub, w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej wprowadź *GitHub* w polu wyszukiwania. Wybierz wyzwalacz lub akcja będzie chcesz użyć:  
   ![](./media/connectors-create-api-github/github-1.png)
2. Jeśli nie utworzono wszystkie połączenia z usługi GitHub przed będzie zostanie wyświetlony monit o podanie poświadczeń użytkownika usługi GitHub. Te poświadczenia będą używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych na koncie usługi GitHub:  
   ![](./media/connectors-create-api-github/github-2.png)
3. Podaj nazwę użytkownika usługi GitHub i hasło, aby zezwolić aplikacji logiki:  
   ![](./media/connectors-create-api-github/github-3.png)   
4. Potwierdzenie zamiaru:  
   ![](./media/connectors-create-api-github/github-4.png)   
5. Należy zauważyć, że utworzono połączenie w portalu. Można teraz kontynuować tworzenie aplikacji logiki i korzystanie z usługi GitHub w niej:   
   ![](./media/connectors-create-api-github/github-5.png)   

