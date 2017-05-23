Teraz możesz utworzyć kolekcję i dodać dane do bazy danych przy użyciu Eksploratora danych. 

1. W witrynie Azure Portal w menu nawigacyjnym kliknij pozycję **Eksplorator danych**. 
2. W bloku Eksplorator danych kliknij pozycję **Nowa kolekcja**, a następnie wypełnij stronę, korzystając z poniższych informacji.

    ![Eksplorator danych w witrynie Azure Portal](./media/cosmosdb-create-collection/azure-cosmosdb-data-explorer.png)

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    Identyfikator bazy danych|Items|Identyfikator nowej bazy danych. Nazwy baz danych muszą zawierać od 1 do 255 znaków i nie mogą zawierać znaków `/ \ # ?` ani mieć spacji na końcu.
    Identyfikator kolekcji|ToDoList|Identyfikator nowej kolekcji. W przypadku nazw kolekcji obowiązują takie same wymagania dotyczące znaków, jak dla identyfikatorów baz danych.
    Pojemność magazynu| Stała (10 GB)|Pozostaw wartość domyślną. To jest pojemność magazynu bazy danych.
    Przepływność|400 jednostek żądania|Pozostaw wartość domyślną. Później możesz skalować przepływność, aby zmniejszyć opóźnienie.
    Klucz partycji|/userid|Klucz partycji służący do równomiernego dystrybuowania danych do każdej partycji. Wybór odpowiedniego klucza partycji jest ważny w celu utworzenia wydajnej kolekcji. Więcej informacji zawiera temat [Designing for partitioning](../articles/cosmos-db/partition-data.md#designing-for-partitioning) (Projektowanie pod kątem partycjonowania).    



3. Po wypełnieniu formularza kliknij przycisk **OK**.