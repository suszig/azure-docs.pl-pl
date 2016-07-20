1.  Zaloguj się w trybie online do [portalu Microsoft Azure](https://portal.azure.com/).
2.  Na pasku dostępu kliknij przycisk **Nowe**, kliknij opcję **Dane i magazyn**, a następnie kliknij opcję **Azure DocumentDB**.

    ![Zrzut ekranu przedstawiający portal Azure podczas tworzenia bazy danych z wyróżnieniem przycisku Nowe, opcji Dane i magazyn w bloku Utwórz oraz opcji Azure DocumentDB w bloku Dane i magazyn](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)  

3. W bloku **Nowe konto usługi DocumentDB** określ odpowiednią konfigurację dla konta usługi DocumentDB.

    ![Zrzut ekranu bloku Nowe konto usługi DocumentDB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)


    - W polu **Identyfikator** wprowadź nazwę, aby zidentyfikować konto usługi DocumentDB.  Po zweryfikowaniu wartości pola **Identyfikator** w polu tym zostanie wyświetlony zielony znacznik wyboru.**** Wartość pola **Identyfikator** jest używana jako nazwa hosta w identyfikatorze URI. Pole **Identyfikator** może zawierać tylko małe litery, cyfry i znak „-” w liczbie od 3 do 50 znaków. Pamiętaj, że ciąg *documents.azure.com* jest dołączany do wybranej nazwy punktu końcowego, tworząc razem punkt końcowy konta usługi DocumentDB.

    - W polu **Subskrypcja** wybierz subskrypcję platformy Azure, która ma być używana dla konta usługi DocumentDB. Jeśli konto ma tylko jedną subskrypcję, jest wybierane jako domyślne.

    - W polu **Grupa zasobów** wybierz lub utwórz grupę zasobów dla konta usługi DocumentDB.  Domyślnie zostanie wybrana istniejąca grupa zasobów w ramach subskrypcji Azure.  Możesz jednak zdecydować się na utworzenie nowej grupy zasobów, do której ma być dodane konto usługi DocumentDB. Więcej informacji znajduje się w temacie [Zarządzania zasobami platformy Azure przy użyciu portalu Azure](resource-group-portal.md).

    - Użyj opcji **Lokalizacja**, aby określić lokalizację geograficzną, w której będzie hostowane konto usługi DocumentDB.   

4.  Po skonfigurowaniu opcji nowego konta usługi DocumentDB kliknij przycisk **Utwórz**.  Utworzenie konta usługi DocumentDB może potrwać kilka minut.  Stan operacji można sprawdzić, monitorując postęp na tablicy startowej.  
    ![Zrzut ekranu przedstawiający kafelek tworzenia na tablicy startowej — kreator bazy danych w trybie online](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-3.png)  

    Możesz też monitorować postęp w centrum powiadomień.  

    ![Szybkie tworzenie baz danych — zrzut ekranu przedstawiający centrum powiadomień z informacją o trwającym tworzeniu konta usługi DocumentDB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-4.png)  

    ![Zrzut ekranu przedstawiający centrum powiadomień z informacją o pomyślnym utworzeniu i wdrożeniu w grupie zasobów konta usługi DocumentDB — powiadomienie kreatora bazy danych w trybie online](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-5.png)

5.  Po utworzeniu konta usługi DocumentDB jest ono gotowe do użycia z ustawieniami domyślnymi w portalu online. Należy pamiętać, że domyślna spójność konta usługi DocumentDB jest ustawiona na wartość **Sesja**.  Możesz dostosować domyślne ustawienie spójności, klikając ikonę **Ustawienia** na górnym pasku poleceń, a następnie klikając pozycję **Domyślna spójność** w obszarze **Funkcja**, w bloku **Wszystkie ustawienia**.

    ![Zrzut ekranu przedstawiający blok Grupa zasobów — rozpoczęcie tworzenia aplikacji](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-6.png)  

    ![Zrzut ekranu przedstawiający blok poziomu spójności — spójność sesji](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-7.png)  

[Porady: tworzenie konta usługi DocumentDB]: #Howto
[Następne kroki]: #NextSteps
[documentdb-manage]:../articles/documentdb/documentdb-manage.md



<!--HONumber=Jun16_HO2-->


