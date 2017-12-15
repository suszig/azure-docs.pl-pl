Teraz możesz użyć narzędzia Eksplorator danych w witrynie Azure Portal, aby utworzyć bazę danych i kolekcję. 

1. Kliknij przycisk **Eksplorator danych** > **Nowa kolekcja**. 
    
    Obszar **Dodaj kolekcję** jest wyświetlany po prawej stronie i konieczne może być przewinięcie w prawo w celu wyświetlenia go.

    ![Eksplorator danych w witrynie Azure Portal, blok Dodaj kolekcję](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. Na stronie **Dodaj kolekcję** wprowadź ustawienia dla nowej kolekcji.

    Ustawienie|Sugerowana wartość|Opis
    ---|---|---
    Identyfikator bazy danych|Zadania|Wprowadź *Zadania* jako nazwę nowej bazy danych. Nazwy baz danych muszą zawierać od 1 do 255 znaków i nie mogą zawierać znaków /, \\, #, ? ani mieć spacji na końcu.
    Identyfikator kolekcji|Items|Wprowadź *Elementy* jako nazwę nowej kolekcji. W przypadku identyfikatorów kolekcji obowiązują takie same wymagania dotyczące znaków, jak dla nazw baz danych.
    Pojemność magazynu| Stała (10 GB)|Zmień wartość na **Stała (10 GB)**. Ta wartość to pojemność magazynu bazy danych.
    Przepływność|400 RU|Zmień przepływność na 400 jednostek żądania na sekundę (RU/s). Pojemność magazynu musi być ustawiona na **Stałe (10 GB)**, aby możliwe było ustawienie przepływności na 400 RU/s. Jeśli chcesz zmniejszyć opóźnienie, możesz później przeskalować przepływność w górę. 
    Klucz partycji|/category|Wprowadź */category* jako klucz partycji. Klucz partycji służący do równomiernego dystrybuowania danych do każdej partycji w bazie danych. Więcej informacji na temat partycjonowania zawiera temat [Designing for partitioning](../articles/cosmos-db/partition-data.md#designing-for-partitioning) (Projektowanie pod kątem partycjonowania).

    Kliknij przycisk **OK**.

    W Eksploratorze danych zostanie wyświetlona nowa baza danych i kolekcja.

    ![Eksplorator danych w witrynie Azure Portal z widoczną nową bazą danych i kolekcją](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)