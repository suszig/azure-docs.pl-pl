## <a name="virtual-network"></a>Virtual Network
Zasoby sieci (VNET) i podsieci wirtualne pomoc w określeniu granicy zabezpieczeń dla obciążeń działających na platformie Azure. Kolekcja przestrzeni adresów, zdefiniowany jako bloków CIDR charakteryzuje się sieci wirtualnej. 

> [!NOTE]
> Administratorzy sieci znają notacji CIDR. Jeśli nie znasz CIDR, [Dowiedz się więcej o](http://whatismyipaddress.com/cidr).
> 
> 

![Sieć wirtualną z wieloma podsieciami](./media/resource-groups-networking/Figure4.png)

Sieci wirtualne obejmują następujące właściwości.

| Właściwość | Opis | Przykładowe wartości |
| --- | --- | --- |
| **Element addressSpace** |Kolekcja prefiksów adresów, które tworzą sieci wirtualnej w notacji CIDR |192.168.0.0/16 |
| **podsieci** |Kolekcja podsieci, które tworzą sieci wirtualnej |zobacz [podsieci](#Subnets) poniżej. |
| **adres IP** |Adres IP przypisany do obiektu. Jest to właściwość tylko do odczytu. |104.42.233.77 |

### <a name="subnets"></a>Podsieci
Podsieć jest zasobem podrzędnych sieci wirtualnej i pomaga zdefiniować segmenty przestrzeni adresów w obrębie blok CIDR, przy użyciu prefiksów adresów IP. Karty sieciowe mogą być dodawane do podsieci lub podłączone do maszyn wirtualnych, zapewniają łączność dla różnych obciążeń.

Podsieci obejmują następujące właściwości. 

| Właściwość | Opis | Przykładowe wartości |
| --- | --- | --- |
| **addressPrefix** |Prefiks pojedynczy adres, który tworzą podsieci w notacji CIDR |192.168.1.0/24 |
| **grupy networkSecurityGroup** |Grupa NSG stosowana do podsieci |zobacz [grupy NSG](#Network-Security-Group) |
| **Stan** |Tabela tras stosowane do podsieci |zobacz [przez](#Route-table) |
| **elementy Ipconfiguration** |Kolekcja obiektów configruation IP używane przez karty sieciowe podłączone do podsieci |zobacz [przez](#Route-table) |

Przykład sieci wirtualnej w formacie JSON:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Dodatkowe zasoby
* Uzyskaj więcej informacji [sieci wirtualnej](../articles/virtual-network/virtual-networks-overview.md).
* Odczyt [dokumentacji interfejsu API REST](https://msdn.microsoft.com/library/azure/mt163650.aspx) dla sieci wirtualnych.
* Odczyt [dokumentacji interfejsu API REST](https://msdn.microsoft.com/library/azure/mt163618.aspx) podsieci.

