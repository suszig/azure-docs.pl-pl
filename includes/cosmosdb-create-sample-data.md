Teraz można dodać dane do nowej kolekcji za pomocą Eksploratora danych.

1. W Eksploratorze danych nowa baza danych jest wyświetlana w okienku Kolekcje. Rozwiń bazę danych **Items**, rozwiń kolekcję **ToDoList**, kliknij pozycję **Dokumenty**, a następnie kliknij pozycję **Nowe dokumenty**. 

   ![Tworzenie nowych dokumentów w Eksploratorze danych w witrynie Azure Portal](./media/cosmosdb-create-sample-data/azure-cosmosdb-data-explorer-emulator-new-document.png)
  
2. Teraz dodaj do kolekcji kilka nowych dokumentów o następującej strukturze. Dla poszczególnych dokumentów wstaw unikatowe identyfikatory i zmień odpowiednio inne właściwości. Nowe dokumenty mogą mieć dowolną strukturę, ponieważ usługa Azure Cosmos DB nie wymusza żadnego schematu danych.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries."
     }
     ```

     Teraz można użyć zapytań w Eksploratorze danych w celu pobrania danych. Domyślnie Eksplorator danych używa instrukcji SELECT * FROM c do pobrania wszystkich dokumentów w kolekcji, ale można zmienić tę instrukcję na SELECT * FROM c ORDER BY c.name ASC, aby zwrócić wszystkie dokumenty rosnąco w kolejności alfabetycznej według właściwości name. 
 
     Eksplorator danych umożliwia również tworzenie procedur składowanych, funkcji zdefiniowanych przez użytkownika i wyzwalaczy w celu wykonania logiki biznesowej po stronie serwera. Eksplorator danych uwidacznia wszystkie wbudowane programowe procedury dostępu do danych dostępne w interfejsach API, ale umożliwia łatwy dostęp do danych za pośrednictwem witryny Azure Portal.