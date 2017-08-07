> [!NOTE]
> * Publiczny adres IP bramy sieci VPN ulegnie zmianie po migracji ze starej do nowej jednostki SKU.
> * Nie można migrować klasycznych bram sieci VPN do nowej jednostki SKU. Klasyczne bramy sieci VPN mogą korzystać jedynie ze starszych (starych) jednostek SKU.
> 

Nie można zmienić rozmiaru bram Azure VPN Gateway, jednocześnie zmieniając starą rodzinę jednostek SKU na nową lub odwrotnie. Jeśli bramy sieci VPN występują w modelu wdrażania Menedżera zasobów i używają starszej wersji jednostek SKU, można migrować do nowych jednostek SKU. Aby przeprowadzić migrację, należy usunąć istniejącą bramę sieci VPN dla sieci wirtualnej, a następnie utworzyć nową.

Przepływ pracy migracji:

1. Usuń wszystkie połączenia z bramą sieci wirtualnej.
2. Usuń starą bramę sieci VPN.
3. Utwórz nową bramę sieci VPN.
4. Zaktualizuj lokalne urządzenia sieci VPN do nowego adresu IP bramy sieci VPN (dla połączeń typu lokacja-lokacja).
5. Zaktualizuj wartość adresu IP bramy dla wszystkich bram sieci lokalnej połączeń między sieciami wirtualnymi, które będą łączyć się z tą bramą.
6. Pobierz nowe pakiety konfiguracji sieci VPN klienta dla klientów połączeń punkt-lokacja nawiązujących połączenie z siecią wirtualną przez tę bramę sieci VPN.
7. Odtwórz połączenia z bramą sieci wirtualnej.