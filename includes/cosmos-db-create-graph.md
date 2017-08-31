Teraz możesz użyć narzędzia Eksplorator danych w witrynie Azure Portal, aby utworzyć bazę danych grafów. 

1. W witrynie Azure Portal w menu nawigacyjnym po lewej kliknij pozycję **Eksplorator danych (wersja zapoznawcza)**. 
2. W bloku **Eksplorator danych (wersja zapoznawcza)** kliknij pozycję **Nowy graf**, a następnie wypełnij stronę, korzystając z poniższych informacji.

    ![Eksplorator danych w witrynie Azure Portal](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    Identyfikator bazy danych|sample-database|Identyfikator nowej bazy danych. Nazwy baz danych muszą zawierać od 1 do 255 znaków i nie mogą zawierać znaków `/ \ # ?` ani mieć spacji na końcu.
    Identyfikator grafu|sample-graph|Identyfikator nowego grafu. W przypadku nazw grafów obowiązują takie same wymagania dotyczące znaków, jak dla identyfikatorów baz danych.
    Pojemność magazynu| 10 GB|Pozostaw wartość domyślną. To jest pojemność magazynu bazy danych.
    Przepływność|400 jednostek żądania|Pozostaw wartość domyślną. Później możesz skalować przepływność, aby zmniejszyć opóźnienie.
    Klucz partycji|/userid|Klucz partycji służący do równomiernego dystrybuowania danych do każdej partycji. Wybór odpowiedniego klucza partycji jest ważny w celu utworzenia wydajnego grafu. Więcej informacji zawiera temat [Designing for partitioning](../articles/cosmos-db/partition-data.md#designing-for-partitioning) (Projektowanie pod kątem partycjonowania).

3. Po wypełnieniu formularza kliknij przycisk **OK**.