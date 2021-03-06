1. W nowym oknie zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. W menu po lewej stronie kliknij pozycję **Utwórz zasób**, kliknij pozycję **Bazy danych**, a następnie w obszarze **Azure Cosmos DB** kliknij pozycję **Utwórz**.
   
   ![Zrzut ekranu przedstawiający witrynę Azure Portal z wyróżnionymi poleceniami Więcej usług i Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. W okienku **Nowe konto** określ odpowiednią konfigurację konta usługi Azure Cosmos DB. 

    Usługa Azure Cosmos DB umożliwia wybór jednego z czterech modeli programowania: Gremlin (wykres), MongoDB, SQL oraz Tabela (klucz-wartość). 
       
    W tym przewodniku Szybki start będziemy programować przy użyciu interfejsu API usługi MongoDB, dlatego wybierz pozycję **MongoDB** podczas wypełniania formularza. Jeśli jednak masz dane grafu dla aplikacji mediów społecznościowych, dane dokumentu z aplikacji wykazu lub dane typu klucz/wartość (tabela), weź pod uwagę, że usługa Azure Cosmos DB może zapewnić globalnie rozproszoną platformę usługi bazy danych o wysokiej dostępności dla wszystkich Twoich aplikacji o znaczeniu krytycznym.

    Wypełnij okienko **Nowe konto**, używając informacji przedstawionych w tabeli jako wskazówki.
 
    ![Zrzut ekranu okienka Nowa usługa Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-2.png)
   
    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    ID|*Unikatowa wartość*|Wybrana unikatowa wartość służąca do identyfikacji konta usługi Azure Cosmos DB. Adres domeny *documents.azure.com* jest dołączany do podanego identyfikatora w celu utworzenia identyfikatora URI, dlatego użyty identyfikator powinien być unikatowy, ale rozpoznawalny. Identyfikator może zawierać tylko małe litery, cyfry oraz znak „-” i musi się składać z 3–50 znaków.
    Interfejs API|MongoDB|Interfejs API określa typ konta do utworzenia. Usługa Azure Cosmos DB oferuje pięć interfejsów API odpowiadających potrzebom aplikacji: SQL (baza danych dokumentów), Gremlin (baza danych wykresów), MongoDB (baza danych dokumentów), tabela platformy Azure i Cassandra. Każdy z tych interfejsów wymaga obecnie oddzielnego konta. <br><br>Wybierz pozycję **MongoDB**, ponieważ w tym podręczniku Szybki start tworzysz bazę danych dokumentów. Zapytania dotyczące tej bazy danych można tworzyć przy użyciu usługi MongoDB.<br><br>[Dowiedz się więcej o interfejsie API usługi MongoDB](../articles/cosmos-db/mongodb-introduction.md)|
    Subskrypcja|*Twoja subskrypcja*|Subskrypcja platformy Azure, która ma być używana dla usługi Azure Cosmos DB. 
    Grupa zasobów|*Taka sama wartość jak identyfikator*|Nazwa nowej grupy zasobów dla Twojego konta. Dla uproszczenia można użyć takiej samej nazwy jak identyfikator. 
    Lokalizacja|*Region najbliżej Twoich użytkowników*|Lokalizacja geograficzna, w której będzie hostowane Twoje konto usługi Azure Cosmos DB. Wybierz lokalizację znajdującą się najbliżej Twoich użytkowników, aby zapewnić im najszybszy dostęp do danych.

4. Kliknij przycisk **Utwórz**, aby utworzyć konto.
5. Na pasku narzędzi kliknij pozycję **Powiadomienia**, aby monitorować proces wdrażania.

    ![Powiadomienie Wdrażanie rozpoczęte](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-nosql-notification.png)

6.  Po zakończeniu wdrażania otwórz nowe konto przy użyciu kafelka Wszystkie zasoby. 

    ![Konto usługi Azure Cosmos DB na kafelku Wszystkie zasoby](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-all-resources.png)
