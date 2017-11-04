## <a name="route-tables"></a>Tabele tras
Zasoby tabeli tras zawiera trasy używane do definiowania sposobu przepływa ruch w ramach infrastruktury platformy Azure. Umożliwia trasy zdefiniowane przez użytkownika (przez) Wyślij cały ruch z jednej podsieci do urządzenia wirtualnego, takich jak system wykrywania zapory lub nieautoryzowanego dostępu (ID). Możesz skojarzyć tabelę tras do podsieci. 

Tabele tras obejmują następujące właściwości.

| Właściwość | Opis | Przykładowe wartości |
| --- | --- | --- |
| **trasy** |Kolekcja użytkownika zdefiniowanych tras w tabeli tras |zobacz [trasy zdefiniowane przez użytkownika](#User-defined-routes) |
| **podsieci** |Kolekcja podsieci tabelę tras jest stosowany do |zobacz [podsieci](#Subnets) |

### <a name="user-defined-routes"></a>Trasy zdefiniowane przez użytkownika
Udr, aby określić wysyłania ruchu sieciowego, można utworzyć na podstawie jego adresu docelowego. Trasa można traktować jako definicji bramy domyślnej na podstawie docelowego adresu pakietów sieciowych.

Udr obejmują następujące właściwości. 

| Właściwość | Opis | Przykładowe wartości |
| --- | --- | --- |
| **addressPrefix** |Prefiks adresu lub pełny adres IP dla miejsca docelowego |192.168.1.0/24, 192.168.1.101 |
| **Typ następnego przeskoku** |Typ urządzenia, dane zostaną wysłane do |Internet VirtualAppliance, Brama sieci VPN |
| **adres IP następnego przeskoku** |Adres IP następnego przeskoku |192.168.1.4 |

Przykładowa tabela tras w formacie JSON:

    {
        "name": "UDR-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/routeTables",
        "location": "westus",
        "properties": {
            "provisioningState": "Succeeded",
            "routes": [
                {
                    "name": "RouteToFrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd/routes/RouteToFrontEnd",
                    "etag": "W/\"v\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "nextHopType": "VirtualAppliance",
                        "nextHopIpAddress": "192.168.0.4"
                    }
                }
            ],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd"
                }
            ]
        }
    }

### <a name="additional-resources"></a>Dodatkowe zasoby
* Uzyskaj więcej informacji [Udr](../articles/virtual-network/virtual-networks-udr-overview.md).
* Odczyt [dokumentacji interfejsu API REST](https://msdn.microsoft.com/library/azure/mt502549.aspx) dla tabel tras.
* Odczyt [dokumentacji interfejsu API REST](https://msdn.microsoft.com/library/azure/mt502539.aspx) dla użytkownika zdefiniowanych tras (Udr).

