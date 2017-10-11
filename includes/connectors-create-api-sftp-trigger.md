Dodajmy wyzwalacza.

1. Wprowadź *sftp* w polu wyszukiwania w Projektancie aplikacji logiki wybiorą **SFTP - podczas dodawania lub modyfikowania pliku** wyzwalacza   
   ![Obraz wyzwalacza SFTP 1](./media/connectors-create-api-sftp/trigger-1.png)  
2. **Podczas dodawania lub modyfikowania pliku** otwiera formantu  
   ![Wyzwalacz SFTP — obraz 2](./media/connectors-create-api-sftp/trigger-2.png)  
3. Wybierz **...**  znajdujący się po prawej stronie formantu. Spowoduje to otwarcie formant wyboru folderu  
   ![Obraz wyzwalacza SFTP 3](./media/connectors-create-api-sftp/action-1.png)  
4. Wybierz **SFTP** do folderu głównego jako folder do monitorowania nowe lub zmodyfikowane pliki. Powiadomienie folderu głównego jest teraz wyświetlany w **folderu** formantu.  
   ![Obraz wyzwalacza SFTP 4](./media/connectors-create-api-sftp/action-2.png)   

W tym momencie aplikację logiki została skonfigurowana z wyzwalaczy, które rozpocznie wykonywania innych wyzwalacze i akcje w przepływie pracy zmodyfikowanie lub utworzony w folderze SFTP określonego pliku. 

> [!NOTE]
> Dla aplikacji logiki działała musi ona zawierać przynajmniej jeden wyzwalacz i jedną akcję. Wykonaj kroki opisane w następnej sekcji, aby dodać akcję.  
> 
> 

