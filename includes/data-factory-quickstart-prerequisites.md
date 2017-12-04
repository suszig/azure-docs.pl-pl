## <a name="prerequisites"></a>Wymagania wstępne

### <a name="azure-subscription"></a>Subskrypcja platformy Azure
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

### <a name="azure-roles"></a>Role platformy Azure
Aby utworzyć wystąpienia usługi Data Factory, konto użytkownika używane do logowania się na platformie Azure musi być członkiem roli **współautora** lub **właściciela** albo **administratorem** subskrypcji platformy Azure. W witrynie Azure Portal kliknij swoją **nazwę użytkownika** w prawym górnym rogu i wybierz pozycję **Uprawnienia**, aby wyświetlić uprawnienia, które masz w subskrypcji. Jeśli masz dostęp do wielu subskrypcji, wybierz odpowiednią subskrypcję. Aby uzyskać przykładowe instrukcje dotyczące dodawania użytkownika do roli, zobacz artykuł [Dodawanie ról](../articles/billing/billing-add-change-azure-subscription-administrator.md).

### <a name="azure-storage-account"></a>Konto usługi Azure Storage
W tym samouczku Szybki start używasz konta usługi Azure Storage ogólnego przeznaczenia (w szczególności usługi Blob Storage) jako **źródłowego** i **docelowego** magazynu danych. Jeśli nie masz konta usługi Azure Storage ogólnego przeznaczenia, zobacz [Tworzenie konta magazynu](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account), aby uzyskać informacje o jego tworzeniu. 

#### <a name="get-storage-account-name-and-account-key"></a>Pobieranie nazwy konta i klucza konta magazynu
W tym samouczku Szybki start używasz nazwy i klucza konta magazynu platformy Azure. Poniższa procedura obejmuje kroki pobierania nazwy i konta klucza magazynu. 

1. Otwórz przeglądarkę internetową i przejdź do witryny [Azure Portal](https://portal.azure.com). Zaloguj się za pomocą nazwy użytkownika i hasła platformy Azure. 
2. Kliknij pozycję **Więcej usług >** w menu po lewej stronie, odfiltruj przy użyciu słowa kluczowego **Storage** i wybierz pozycję **Konta usługi Storage**.

    ![Wyszukiwanie konta magazynu](media/data-factory-quickstart-prerequisites/search-storage-account.png)
3. Na liście kont magazynu odfiltruj konto magazynu (w razie potrzeby), a następnie wybierz **swoje konto magazynu**. 
4. Na stronie **Konto magazynu** wybierz pozycję **Klucze dostępu**.

    ![Pobieranie nazwy i klucza konta magazynu](media/data-factory-quickstart-prerequisites/storage-account-name-key.png)
5. Skopiuj wartości pól **Nazwa konta magazynu** i **klucz1** do schowka. Wklej je do Notatnika lub innego edytora i zapisz plik. Będziesz ich używać w dalszej części tego samouczka Szybki start.   

#### <a name="create-input-folder-and-files"></a>Tworzenie plików i folderu wejściowego
W tej sekcji utworzysz kontener obiektów blob o nazwie **adftutorial** w usłudze Azure Blob Storage. Następnie utwórz folder o nazwie **input** w kontenerze i przekaż przykładowy plik do folderu input. 

1. Na stronie **Konto magazynu** przełącz się do widoku **Przegląd**, a następnie kliknij pozycję **Obiekty blob**. 

    ![Wybieranie opcji Obiekty blob](media/data-factory-quickstart-prerequisites/select-blobs.png)
2. Na stronie **Usługa Blob** kliknij pozycję **+ Kontener** na pasku narzędzi. 

    ![Przycisk dodawania kontenera](media/data-factory-quickstart-prerequisites/add-container-button.png)    
3. W oknie dialogowym **Nowy kontener** wprowadź jako nazwę **adftutorial**, a następnie kliknij przycisk **OK**. 

    ![Wprowadzanie nazwy kontenera](media/data-factory-quickstart-prerequisites/new-container-dialog.png)
4. Kliknij pozycję **adftutorial** na liście kontenerów. 

    ![Wybieranie kontenera](media/data-factory-quickstart-prerequisites/seelct-adftutorial-container.png)
1. Na stronie **Kontener** kliknij pozycję **Przekaż** na pasku narzędzi.  

    ![Przycisk Przekaż](media/data-factory-quickstart-prerequisites/upload-toolbar-button.png)
6. Na stronie **Przekaż obiekt blob** kliknij pozycję **Zaawansowane**.

    ![Klikanie linku Zaawansowane](media/data-factory-quickstart-prerequisites/upload-blob-advanced.png)
7. Uruchom program **Notatnik** i utwórz plik o nazwie **emp.txt** z następującą zawartością: zapisz go w folderze **c:\ADFv2QuickStartPSH**: (utwórz folder **ADFv2QuickStartPSH**, jeśli jeszcze nie istnieje).
    
    ```
    John, Doe
    Jane, Doe
    ```    
8. W witrynie Azure Portal na stronie **Przekazywanie obiektu blob** wyszukaj i wybierz plik **emp.txt** dla pola **Pliki**. 
9. Wprowadź wartość **input** jako wartość pola **Przekaż do folderu**. 

    ![Ustawienia przekazywania obiektu blob](media/data-factory-quickstart-prerequisites/upload-blob-settings.png)    
10. Upewnij się, że wybrano folder **input** i plik **emp.txt**, a następnie kliknij przycisk **Przekaż**.
11. Na liście powinien pojawić się plik **emp.txt** i stan przekazywania. 
12. Zamknij stronę **Przekazywanie obiektu blob**, klikając przycisk **X** w rogu strony. 

    ![Zamykanie strony przekazywania obiektu blob](media/data-factory-quickstart-prerequisites/close-upload-blob.png)
1. Zachowaj otwartą stronę **kontenera**. Będzie ona używana do weryfikowania danych wyjściowych na końcu tego samouczka Szybki start.