1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. Na pasku dostępu kliknij pozycje **Nowy** i **Dane i magazyn**, a następnie kliknij pozycję **DocumentDB (NoSQL)**.
   
   ![Zrzut ekranu przedstawiający witrynę Azure Portal z wyróżnionymi poleceniami Więcej usług i DocumentDB (NoSQL)](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)  
3. W bloku **Nowe konto** określ odpowiednią konfigurację konta usługi DocumentDB.
   
    ![Zrzut ekranu bloku Nowe konto usługi DocumentDB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)
   
   * W polu **Identyfikator** wprowadź nazwę, aby zidentyfikować konto usługi DocumentDB.  Po zweryfikowaniu wartości pola **Identyfikator** w polu tym zostanie wyświetlony zielony znacznik wyboru.**** Wartość pola **Identyfikator** jest używana jako nazwa hosta w identyfikatorze URI. Pole **Identyfikator** może zawierać tylko małe litery, cyfry i znak „-” w liczbie od 3 do 50 znaków. Pamiętaj, że ciąg *documents.azure.com* jest dołączany do wybranej nazwy punktu końcowego, by utworzyć punkt końcowy konta usługi DocumentDB.
   * W polu **Interfejs API NoSQL** wybierz opcję **DocumentDB**.  
   * W polu **Subskrypcja** wybierz subskrypcję platformy Azure, która ma być używana dla konta usługi DocumentDB. Jeśli konto ma tylko jedną subskrypcję, jest wybierane jako domyślne.
   * W polu **Grupa zasobów** wybierz lub utwórz grupę zasobów dla konta usługi DocumentDB.  Domyślnie zostanie utworzona nowa grupa zasobów. Aby uzyskać więcej informacji, zobacz temat [Using the Azure portal to manage your Azure resources](../articles/azure-portal/resource-group-portal.md) (Korzystanie z witryny Azure Portal do zarządzania zasobami Azure).
   * Użyj opcji **Lokalizacja**, aby określić lokalizację geograficzną, w której będzie hostowane konto usługi DocumentDB. 
4. Po skonfigurowaniu opcji nowego konta usługi DocumentDB kliknij przycisk **Utwórz**. Aby sprawdzić stan wdrożenia, przejdź do centrum powiadomień.  
   
   ![Szybkie tworzenie baz danych — zrzut ekranu przedstawiający centrum powiadomień z informacją o trwającym tworzeniu konta usługi DocumentDB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-4.png)  
   
   ![Zrzut ekranu przedstawiający centrum powiadomień z informacją o pomyślnym utworzeniu i wdrożeniu w grupie zasobów konta usługi DocumentDB — powiadomienie kreatora bazy danych w trybie online](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-5.png)
5. Utworzone konto usługi DocumentDB jest gotowe do użycia z ustawieniami domyślnymi. Domyślna spójność konta usługi DocumentDB jest ustawiona na wartość **Sesja**.  Można ją dostosować, klikając pozycję **Domyślna spójność** w menu zasobów. Aby dowiedzieć się więcej na temat poziomów spójności oferowanych przez usługę DocumentDB, zobacz temat [Consistency levels in DocumentDB](../articles/documentdb/documentdb-consistency-levels.md) (Poziomy spójności w usłudze DocumentDB).
   
   ![Zrzut ekranu przedstawiający blok Grupa zasobów — rozpoczęcie tworzenia aplikacji](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-6.png)  
   
   ![Zrzut ekranu przedstawiający blok poziomu spójności — spójność sesji](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-7.png)  

[Porady: tworzenie konta usługi DocumentDB]: #Howto
[Następne kroki]: #NextSteps
[documentdb-manage]:../articles/documentdb/documentdb-manage.md


<!--HONumber=Nov16_HO2-->


