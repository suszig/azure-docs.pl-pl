1. W nowym oknie zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. W menu po lewej stronie kliknij pozycję **Nowy**, kliknij pozycję **Bazy danych**, a następnie kliknij pozycję **Azure Cosmos DB**.
   
   ![Zrzut ekranu przedstawiający witrynę Azure Portal z wyróżnionymi poleceniami Więcej usług i Azure Cosmos DB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. W bloku **Nowe konto** określ odpowiednią konfigurację konta usługi Azure Cosmos DB. 

    Usługa Azure Cosmos DB umożliwia wybór jednego z czterech modeli programowania: Gremlin (graf), MongoDB, SQL (DocumentDB) oraz Tabela (klucz-wartość). 
    
    W tym przewodniku Szybki start będziemy programować przy użyciu interfejsu API usługi DocumentDB, dlatego wybierz pozycję **SQL (DocumentDB)** podczas wypełniania formularza. Jednak jeśli masz dane grafu dla aplikacji mediów społecznościowych, dane typu klucz/wartość (tabela) lub dane zmigrowane z aplikacji MongoDB, weź pod uwagę, że usługa Azure Cosmos DB może zapewnić globalnie rozproszoną platformę usługi bazy danych o wysokiej dostępności dla wszystkich Twoich aplikacji o znaczeniu krytycznym.

    Wypełnij blok Nowe konto, używając informacji przedstawionych na zrzucie ekranu jako wskazówki. Podczas konfigurowania konta wybierzesz unikatowe wartości, więc będą one nieco inne niż wartości przedstawione na zrzucie ekranu. 
 
    ![Zrzut ekranu bloku Nowa usługa Azure Cosmos DB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    ID|*Unikatowa wartość*|Wybrana unikatowa nazwa służąca do identyfikacji konta usługi Azure Cosmos DB. Adres domeny *documents.azure.com* jest dołączany do podanego identyfikatora w celu utworzenia identyfikatora URI, dlatego użyty identyfikator powinien być unikatowy, ale rozpoznawalny. Identyfikator może zawierać tylko małe litery, cyfry oraz znak „-” i musi się składać z 3–50 znaków.
    Interfejs API|SQL (DocumentDB)|W dalszej części artykułu będziemy programować przy użyciu interfejsu [API usługi DocumentDB](../articles/documentdb/documentdb-introduction.md).|
    Subskrypcja|*Twoja subskrypcja*|Subskrypcja platformy Azure, która ma być używana dla konta usługi Azure Cosmos DB. 
    Grupa zasobów|*Taka sama wartość jak identyfikator*|Nazwa nowej grupy zasobów dla Twojego konta. Dla uproszczenia można użyć takiej samej nazwy jak identyfikator. 
    Lokalizacja|*Region najbliżej Twoich użytkowników*|Lokalizacja geograficzna, w której będzie hostowane Twoje konto usługi Azure Cosmos DB. Wybierz lokalizację znajdującą się najbliżej Twoich użytkowników, aby zapewnić im najszybszy dostęp do danych.
4. Kliknij przycisk **Utwórz**, aby utworzyć konto.
5. Na górnym pasku narzędzi kliknij pozycję **Powiadomienia**, aby monitorować proces wdrażania.

    ![Powiadomienie Wdrażanie rozpoczęte](./media/documentdb-create-dbaccount/azure-documentdb-nosql-notification.png)

6.  Po zakończeniu wdrażania otwórz nowe konto przy użyciu kafelka Wszystkie zasoby. 

    ![Konto usługi DocumentDB na kafelku Wszystkie zasoby](./media/documentdb-create-dbaccount/azure-documentdb-all-resources.png)