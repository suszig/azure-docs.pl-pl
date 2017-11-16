1. W nowym oknie przeglądarki, zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Kliknij przycisk **nowe** > **baz danych** > **rozwiązania Cosmos Azure DB**.
   
   ![Okienko Bazy danych w witrynie Azure Portal](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-1.png)

3. W **nowe konto** wprowadź ustawienia dla nowego konta bazy danych Azure rozwiązania Cosmos. 
 
    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    ID|*Wprowadź unikatową nazwę*|Wprowadź unikatową nazwę pozwalającą zidentyfikować to konto bazy danych Azure rozwiązania Cosmos. Ponieważ *documents.azure.com* jest dołączany do Identyfikatora umożliwiają tworzenie punktu kontaktu, używają unikalny, ale do zidentyfikowania identyfikatora.<br><br>Identyfikator może zawierać tylko małe litery, cyfry i znaki łącznika (-) oraz musi zawierać od 3 do 50 znaków.
    Interfejs API|Cassandra|Interfejs API Określa typ konta, aby utworzyć. Azure DB rozwiązania Cosmos zawiera pięć interfejsy API w celu odpowiada potrzebom aplikacji: SQL (baza danych dokumentu), Gremlin (wykres bazy danych) bazy danych MongoDB (baza danych dokumentu), tabel Azure i Cassandra, każdy wymagających obecnie oddzielne konto. <br><br>Wybierz **Cassandra** ponieważ w tym szybkiego startu tworzysz kolumny całej bazy danych, która jest zapytań przy użyciu składni CQL.<br><br>Jeśli Cassandra (kolumny WWW) nie jest wyświetlana na liście, a następnie należy [dotyczą join](https://aka.ms/cosmosdb-cassandra-signup) Cassandra interfejsu API programu Podgląd.<br><br> [Dowiedz się więcej o interfejsie API Cassandra](../articles/cosmos-db/cassandra-introduction.md)|
    Subskrypcja|*Twoja subskrypcja*|Wybierz subskrypcję Azure, która ma być używany dla tego konta bazy danych Azure rozwiązania Cosmos. 
    Grupa zasobów|*Wprowadzić taką samą nazwę unikatową, co zostało opisane powyżej w identyfikatorze*|Wprowadź nazwę nowej grupy zasobów dla Twojego konta. Dla uproszczenia można użyć takiej samej nazwy jak identyfikator. 
    Lokalizacja|*Wybierz region najbliższy użytkowników*|Wybierz lokalizację geograficzną, w której będzie hostowane konto bazy danych Azure rozwiązania Cosmos. Użyj lokalizacji, która jest najbardziej zbliżony do użytkowników, aby uzyskać instrukcje na temat najszybszy dostęp do danych.
    Przypnij do pulpitu nawigacyjnego | Wybierz pozycję | Zaznacz to pole, dzięki czemu nowe konto bazy danych zostanie dodane do pulpitu nawigacyjnego portalu, by mieć łatwy dostęp.

    Następnie kliknij pozycję **Utwórz**.

    ![Blok nowego konta usługi Azure Cosmos DB](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-2.png)

4. Tworzenie konta zajmuje kilka minut. Podczas tworzenia konta Wyświetla pulpitu nawigacyjnego portalu **wdrażania rozwiązania Cosmos bazy danych Azure** kafelka.

    ![Kafelek powiadomienia portalu Azure](./media/cosmos-db-create-dbaccount-cassandra/deploying-cosmos-db.png)

    Po utworzeniu konta **Gratulacje! Utworzono konto bazy danych Azure rozwiązania Cosmos** zostanie wyświetlona strona. 

