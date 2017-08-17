Teraz możesz użyć Eksploratora danych, aby utworzyć tabelę i dodać dane do swojej bazy danych. 

1. W witrynie Azure Portal w menu nawigacyjnym kliknij pozycję **Eksplorator danych (wersja zapoznawcza)**. 
2. W bloku Eksplorator danych kliknij pozycję **Nowa tabela**, a następnie wypełnij stronę, korzystając z poniższych informacji.

    ![Eksplorator danych w witrynie Azure Portal](./media/cosmos-db-create-table/azure-cosmosdb-data-explorer.png)

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    Identyfikator tabeli|sample-table|Identyfikator nowej tabeli. W przypadku nazw tabel obowiązują takie same wymagania dotyczące znaków, jak dla identyfikatorów baz danych. Nazwy baz danych muszą zawierać od 1 do 255 znaków i nie mogą zawierać znaków `/ \ # ?` ani mieć spacji na końcu.
    Pojemność magazynu| 10 GB|Pozostaw wartość domyślną. To jest pojemność magazynu bazy danych.
    Przepływność|400 jednostek żądania|Pozostaw wartość domyślną. Później możesz skalować w górę [przepływność](../articles/cosmos-db/request-units.md), aby zmniejszyć opóźnienie.
    RU/m|Wyłączone|Użyj wartości domyślnej.

3. Po wypełnieniu formularza kliknij przycisk **OK**.