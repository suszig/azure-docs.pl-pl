### <a name="prerequisites"></a>Wymagania wstępne
* Konto platformy Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)
* [Konta magazynu obiektów Blob Azure](../articles/storage/common/storage-create-storage-account.md) w tym nazwę konta magazynu i klucza dostępu. Te informacje jest wyświetlane we właściwościach konta magazynu w portalu Azure. Przeczytaj więcej na temat [usługi Azure Storage](../articles/storage/common/storage-introduction.md).

Przed rozpoczęciem korzystania z konta magazynu obiektów Blob Azure w aplikacji logiki, łączenia się z kontem magazynu obiektów Blob Azure. Można to zrobić łatwo w aplikacjach logiki w portalu Azure.  

Połączenie do swojego konta magazynu obiektów Blob Azure, wykonując następujące czynności:  

1. Tworzenie aplikacji logiki. W Projektancie aplikacji logiki dodać wyzwalacz, a następnie dodaj akcję. Wybierz **Pokaż Microsoft zarządzanych interfejsów API** na rozwijanej liście, a następnie wprowadź "blob" w polu wyszukiwania. Wybierz jedną z akcji:  
   
    ![Połączenia magazynu obiektów Blob platformy Azure — tworzenie krok](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  
2. Jeśli nie utworzono jeszcze żadnych połączenia z magazynem platformy Azure, zostanie wyświetlony monit o informacje dotyczące połączenia:   
   
    ![Połączenia magazynu obiektów Blob platformy Azure — tworzenie krok](./media/connectors-create-api-azureblobstorage/connection-details.png)  
3. Wprowadź szczegóły konta magazynu. Właściwości oznaczone gwiazdką są wymagane.
   
   | Właściwość | Szczegóły |
   | --- | --- |
   | Nazwa połączenia * |Wprowadź dowolną nazwę połączenia. |
   | Nazwa konta magazynu Azure * |Wprowadź nazwę konta magazynu. Nazwa konta magazynu jest wyświetlane we właściwościach magazynu w portalu Azure. |
   | Klucz dostępu do konta magazynu Azure * |Wprowadź klucz konta magazynu. Klawisze dostępu są wyświetlane we właściwościach magazynu w portalu Azure. |
   
    Te poświadczenia są używane do autoryzacji aplikację logiki, aby podłączyć się i uzyskać dostęp do danych. 
4. Wybierz pozycję **Utwórz**.
5. Należy zauważyć, że utworzono połączenie. Teraz kontynuować pozostałe kroki w aplikacji logiki: 
   
    ![Połączenia magazynu obiektów Blob platformy Azure — tworzenie krok](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  

