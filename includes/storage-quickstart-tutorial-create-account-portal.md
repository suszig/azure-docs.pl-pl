## <a name="create-a-storage-account-by-using-the-azure-portal"></a>Utwórz konto magazynu przy użyciu portalu Azure

Najpierw utwórz nowe konto magazynu ogólnego przeznaczenia na potrzeby tego przewodnika Szybki Start. 

1. Przejdź do [portalu Azure](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM) i zaloguj się przy użyciu konta platformy Azure. 
2. Wprowadź unikatową nazwę konta magazynu. Podczas określania nazwy konta magazynu należy pamiętać o następujących regułach:
    - Nazwa musi być 3 do 24 znaków.
    - Nazwa może zawierać tylko cyfry i małe litery.
3. Upewnij się, że są ustawione następujące wartości domyślne: 
    - **Model wdrażania** ustawiono **Resource Manager**.
    - Właściwość **Rodzaj konta** jest ustawiona na wartość **Zastosowania ogólne**.
    - Właściwość **Wydajność** jest ustawiona na wartość **Standardowa**.
    - Właściwość **Replikacja** jest ustawiona na wartość **Magazyn lokalnie nadmiarowy (LRS)**.
4. Wybierz subskrypcję. 
5. Dla elementu **Grupa zasobów** utwórz nową grupę zasobów i nadaj jej unikatową nazwę. 
6. Wybierz lokalizację dla konta magazynu.
7. Wybierz **Przypnij do pulpitu nawigacyjnego** i wybierz **Utwórz** można utworzyć konta magazynu. 

Po utworzeniu konta magazynu jest przypięta do pulpitu nawigacyjnego. Wybierz, aby go otworzyć. W obszarze **ustawienia**, wybierz pozycję **klucze dostępu**. Wybierz klucz podstawowy i skopiuj parametry połączenia skojarzone do Schowka. Następnie wklej parametry do edytora tekstu do późniejszego użycia.
