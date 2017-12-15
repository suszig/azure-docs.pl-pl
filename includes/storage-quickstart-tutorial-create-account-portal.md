## <a name="create-a-storage-account-using-the-azure-portal"></a>Tworzenie konta magazynu w witrynie Azure Portal

Najpierw utwórz nowe konto magazynu ogólnego przeznaczenia na potrzeby tego przewodnika Szybki Start. 

1. Przejdź do witryny [Azure Portal](https://portal.azure.com) i zaloguj się przy użyciu konta platformy Azure. 
2. W menu Centrum wybierz kolejno pozycje **Nowy** > **Magazyn** > **Konto magazynu — obiekt blob, plik, tabela, kolejka**. 
3. Wprowadź unikatową nazwę konta magazynu. Podczas określania nazwy konta magazynu należy pamiętać o następujących regułach:
    - Nazwa musi mieć długość od 3 do 24 znaków.
    - Nazwa może zawierać tylko cyfry i małe litery.
4. Upewnij się, że są ustawione następujące wartości domyślne: 
    - Właściwość **Model wdrażania** jest ustawiona na wartość **Usługa Resource Manager**.
    - Właściwość **Rodzaj konta** jest ustawiona na wartość **Zastosowania ogólne**.
    - Właściwość **Wydajność** jest ustawiona na wartość **Standardowa**.
    - Właściwość **Replikacja** jest ustawiona na wartość **Magazyn lokalnie nadmiarowy (LRS)**.
5. Wybierz subskrypcję. 
6. Dla elementu **Grupa zasobów** utwórz nową grupę zasobów i nadaj jej unikatową nazwę. 
7. Wybierz **Lokalizację**, która ma być używana dla tego konta magazynu.
8. Zaznacz pozycję **Przypnij do pulpitu nawigacyjnego** i kliknij przycisk **Utwórz**, aby utworzyć konto magazynu. 

Po utworzeniu konto magazynu zostaje przypięte do pulpitu nawigacyjnego. Kliknij je, aby je otworzyć. W obszarze **Ustawienia** kliknij pozycję **Klucze dostępu**. Wybierz klucz podstawowy i skopiuj skojarzone **parametry połączenia** do schowka, a następnie wklej je w edytorze tekstu do późniejszego użytku.
