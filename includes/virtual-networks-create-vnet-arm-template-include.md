## Pobieranie szablonu ARM i zapoznawanie się z nim
Z witryny github można pobrać istniejący szablon ARM służący do utworzenia sieci wirtualnej z dwoma podsieciami, wprowadzić dowolne zmiany, a następnie użyć go ponownie. W tym celu wykonaj poniższe kroki.

1. Przejdź do [strony przykładowego szablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Kliknij opcję **azuredeploy.json**, a następnie kliknij opcję **RAW**.
3. Zapisz plik do folderu lokalnego na komputerze.
4. Jeśli znasz szablony ARM, przejdź do kroku 7.
5. Otwórz zapisany plik i przyjrzyj się zawartości sekcji **parameters** w wierszu 5. Parametry szablonu ARM zawierają symbole zastępcze wartości, które mogą zostać wypełnione podczas wdrażania.
   
   | Parametr | Opis |
   | --- | --- |
   | **location** |Region platformy Azure, w której zostanie utworzona sieć wirtualna |
   | **vnetName** |Nazwa nowej sieci wirtualnej |
   | **addressPrefix** |Przestrzeń adresowa sieci wirtualnej w formacie CIDR |
   | **subnet1Name** |Nazwa pierwszej sieci wirtualnej |
   | **subnet1Prefix** |Blok CIDR pierwszej podsieci |
   | **subnet2Name** |Nazwa drugiej sieci wirtualnej |
   | **subnet2Prefix** |Blok CIDR drugiej podsieci |
   
   > [!IMPORTANT]
   > Szablony ARM przechowywane w witrynie github mogą ulec zmianie. Przed użyciem szablonu należy go sprawdzić.
   > 
   > 
6. Sprawdź zawartość w obszarze **resources**, w szczególności następujące pozycje:
   
   * **type**. Typ zasobu tworzonego przez szablon. W tym przypadku jest to **Microsoft.Network/virtualNetworks** reprezentujący sieć wirtualną.
   * **name**. Nazwa zasobu. Zwróć uwagę na pozycję **[parameters('vnetName')]**, co oznacza nazwę podaną przez użytkownika jako dane wejściowe lub zawartą w pliku parametrów podczas wdrażania.
   * **properties**. Lista właściwości zasobu. Ten szablon używa właściwości przestrzeni adresowej i podsieci podczas tworzenia sieci wirtualnej.
7. Przejdź z powrotem do [strony przykładowego szablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Kliknij opcję **azuredeploy-paremeters.json**, a następnie kliknij opcję **RAW**.
9. Zapisz plik do folderu lokalnego na komputerze.
10. Otwórz zapisany plik i dokonaj edycji wartości parametrów. Do wdrożenia opisanej w scenariuszu sieci wirtualnej użyj poniższych wartości.
    
        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
11. Zapisz plik.

<!--HONumber=Sep16_HO3-->


