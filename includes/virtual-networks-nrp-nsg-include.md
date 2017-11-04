## <a name="network-security-group"></a>Grupy zabezpieczeń sieci
Zasób NSG umożliwia tworzenie granic zabezpieczeń dla obciążeń, implementując zezwalania i odmowy reguły. Takie zasady może odnosić się do maszyny Wirtualnej karty Sieciowej i podsieci.

| Właściwość | Opis | Przykładowe wartości |
| --- | --- | --- |
| **podsieci** |Listę identyfikatorów podsieci, grupa NSG jest stosowana do. |/Subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/TestRG/Providers/Microsoft.Network/virtualNetworks/TestVNet/Subnets/FrontEnd |
| **securityRules** |Lista reguł zabezpieczeń, które tworzą grupy NSG |Zobacz [reguły zabezpieczeń](#Security-rule) poniżej |
| **defaultSecurityRules** |Lista domyślnych reguł zabezpieczeń w każdej grupy NSG |Zobacz [domyślne reguły zabezpieczeń](#Default-security-rules) poniżej |

* **Reguła zabezpieczeń** -grupa NSG może mieć zdefiniowano wiele reguł zabezpieczeń. Każda reguła może akceptować lub odrzucać różnego rodzaju ruchu.

### <a name="security-rule"></a>Reguły zabezpieczeń
Reguła zabezpieczeń jest zasobem podrzędnych NSG, zawierający poniższe właściwości.

| Właściwość | Opis | Przykładowe wartości |
| --- | --- | --- |
| **Opis elementu** |Opis reguły |Zezwalaj na ruch przychodzący dla wszystkich maszyn wirtualnych w podsieci X |
| **Protokół** |Protokół odpowiadający regule |TCP, UDP lub * |
| **sourcePortRange** |Zakres portów źródłowych odpowiadający regule |80, 100-200, * |
| **destinationPortRange** |Zakres portów docelowych odpowiadający regule |80, 100-200, * |
| **sourceAddressPrefix** |Prefiks adresu źródłowego odpowiadający regule |10.10.10.1, 10.10.10.0/24, sieć wirtualną |
| **destinationAddressPrefix** |Prefiks adresu docelowego odpowiadający regule |10.10.10.1, 10.10.10.0/24, sieć wirtualną |
| **Kierunek** |Kierunek ruchu odpowiadający regule |ruch przychodzący lub wychodzący |
| **priorytet** |Priorytet reguły. Reguły są sprawdzane według ważności, gdy reguła ma zastosowanie, żadne inne reguły są sprawdzane pod kątem dopasowania. |10, 100, 65000 |
| **dostęp** |Typ dostępu do zastosowania, jeśli reguła jest dopasowana |zezwolenie lub zablokowanie |

Przykład grupy NSG w formacie JSON:

    {
        "name": "NSG-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkSecurityGroups",
        "location": "westus",
        "tags": {
            "displayName": "NSG - Front End"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "securityRules": [
                {
                    "name": "rdp-rule",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/rdp-rule",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "description": "Allow RDP",
                        "protocol": "Tcp",
                        "sourcePortRange": "*",
                        "destinationPortRange": "3389",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "access": "Allow",
                        "priority": 100,
                        "direction": "Inbound"
                    }
                }
            ],
            "defaultSecurityRules": [
                { [...],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                }
            ]
        }
    }

### <a name="default-security-rules"></a>Domyślne reguły zabezpieczeń

Domyślne reguły zabezpieczeń mają takie same właściwości dostępne w zasadach zabezpieczeń. Istnieją one w celu zapewnienia podstawowej łączności między zasoby, które mają zastosowanych do nich grup NSG. Upewnij się, że wiesz, jakiego [domyślne reguły zabezpieczeń](../articles/virtual-network/virtual-networks-nsg.md#default-rules) istnieje.

### <a name="additional-resources"></a>Dodatkowe zasoby
* Uzyskaj więcej informacji [grup NSG](../articles/virtual-network/virtual-networks-nsg.md).
* Odczyt [dokumentacji interfejsu API REST](https://msdn.microsoft.com/library/azure/mt163615.aspx) dla grup NSG.
* Odczyt [dokumentacji interfejsu API REST](https://msdn.microsoft.com/library/azure/mt163580.aspx) dla reguł zabezpieczeń.
