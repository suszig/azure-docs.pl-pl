1.  Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2.  Na pasku przechodzenia kliknij kolejno pozycje **Nowy**, **Dane i magazyn** oraz **DocumentDB (NoSQL)**.

    ![Zrzut ekranu przedstawiający witrynę Azure Portal, ze szczególnym uwzględnieniem poleceń Więcej usług i DocumentDB (NoSQL)](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)  

3. W bloku **Nowe konto** określ odpowiednią konfigurację dla konta usługi DocumentDB.

    ![Zrzut ekranu bloku Nowe konto usługi DocumentDB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)


    - W polu **Identyfikator** wprowadź nazwę, aby zidentyfikować konto usługi DocumentDB.  Po zweryfikowaniu wartości pola **Identyfikator** w polu tym zostanie wyświetlony zielony znacznik wyboru.**** Wartość pola **Identyfikator** jest używana jako nazwa hosta w identyfikatorze URI. Pole **Identyfikator** może zawierać tylko małe litery, cyfry i znak „-” w liczbie od 3 do 50 znaków. Pamiętaj, że ciąg *documents.azure.com* jest dołączany do wybranej nazwy punktu końcowego, tworząc razem punkt końcowy konta usługi DocumentDB.

    - W polu **Subskrypcja** wybierz subskrypcję platformy Azure, która ma być używana dla konta usługi DocumentDB. Jeśli konto ma tylko jedną subskrypcję, jest wybierane jako domyślne.

    - W polu **Grupa zasobów** wybierz lub utwórz grupę zasobów dla konta usługi DocumentDB.  Domyślnie zostanie utworzona nowa grupa zasobów. Więcej informacji znajduje się w temacie [Using the Azure portal to manage your Azure resources](../articles/azure-portal/resource-group-portal.md) (Zarządzanie zasobami platformy Azure przy użyciu witryny Azure Portal).

    - Użyj opcji **Lokalizacja**, aby określić lokalizację geograficzną, w której będzie hostowane konto usługi DocumentDB. 
    
    - Aby zapewnić sobie wygodny dostęp do swojego konta i zasobów, które zostaną utworzone w przyszłości, zaznacz pole **Przypnij do pulpitu nawigacyjnego**.  

4.  Po skonfigurowaniu opcji nowego konta usługi DocumentDB kliknij przycisk **Utwórz**. Stan wdrożenia można sprawdzić, monitorując postęp na tablicy startowej.  
    ![Zrzut ekranu przedstawiający kafelek tworzenia na tablicy startowej — kreator bazy danych w trybie online](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-3.png)  

    Możesz też monitorować postęp w centrum powiadomień.  

    ![Szybkie tworzenie baz danych — zrzut ekranu przedstawiający centrum powiadomień z informacją o trwającym tworzeniu konta usługi DocumentDB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-4.png)  

    ![Zrzut ekranu przedstawiający centrum powiadomień z informacją o pomyślnym utworzeniu i wdrożeniu w grupie zasobów konta usługi DocumentDB — powiadomienie kreatora bazy danych w trybie online](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-5.png)

5.  Po utworzeniu konta usługi DocumentDB jest ono gotowe do użycia z ustawieniami domyślnymi. Należy pamiętać, że domyślna spójność konta usługi DocumentDB jest ustawiona na wartość **Sesja**.  Można ją dostosować, klikając pozycję **Domyślna spójność** w menu zasobów. Aby dowiedzieć się więcej na temat poziomów spójności oferowanych przez usługę DocumentDB, zobacz [Consistency levels in DocumentDB](../articles/azure-portal/resource-group-portal.md) (Poziomy spójności w usłudze DocumentDB).

    ![Zrzut ekranu przedstawiający blok Grupa zasobów — rozpoczęcie tworzenia aplikacji](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-6.png)  

    ![Zrzut ekranu przedstawiający blok poziomu spójności — spójność sesji](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-7.png)  

[Porady: tworzenie konta usługi DocumentDB]: #Howto
[Następne kroki]: #NextSteps
[documentdb-manage]:../articles/documentdb/documentdb-manage.md



<!--HONumber=sep16_HO1-->


