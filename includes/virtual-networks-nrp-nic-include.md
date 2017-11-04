## <a name="nic"></a>KARTA SIECIOWA
Karta sieciowa zasobu interfejsu sieciowego zapewnia łączność sieciową z istniejącą podsieć w zasobie sieci wirtualnej. Mimo że można tworzyć karty Sieciowej jako obiekt autonomiczny, należy do powiązania go z innym obiektem, aby rzeczywiście zapewniają łączność. Karta sieciowa może służyć do nawiązania maszyny Wirtualnej podsieci, publiczny adres IP lub równoważenia obciążenia.  

| Właściwość | Opis | Przykładowe wartości |
| --- | --- | --- |
| **maszyny wirtualnej** |Maszyna wirtualna karta sieciowa jest skojarzony. |/Subscriptions/{GUID}/../microsoft.COMPUTE/virtualMachines/vm1 |
| **macAddress** |Adres MAC karty sieciowej |dowolna wartość od 4 do 30 |
| **grupy networkSecurityGroup** |Grupy NSG skojarzonej z kartą sieciową |/Subscriptions/{GUID}/../microsoft.Network/networkSecurityGroups/myNSG1 |
| **dnsSettings** |Ustawienia DNS dla karty Sieciowej |zobacz [PIP](#Public-IP-address) |

Karty sieciowej lub karty Sieciowej, reprezentuje interfejs sieciowy, który może być skojarzony z maszyną wirtualną (VM). Maszyna wirtualna może mieć jeden lub więcej kart sieciowych.

![Karty Sieciowej na jednej maszynie Wirtualnej](./media/resource-groups-networking/Figure3.png)

### <a name="ip-configurations"></a>Konfiguracje adresów IP
Karty sieciowe mają obiektu podrzędnego o nazwie **Ipconfiguration** zawierającą następujące właściwości:

| Właściwość | Opis | Przykładowe wartości |
| --- | --- | --- |
| **podsieci** |Podsieć karty Sieciowej jest onnected do. |/Subscriptions/{GUID}/../microsoft.Network/virtualNetworks/myvnet1/Subnets/mysub1 |
| **elementu privateIPAddress** |Adres IP dla karty Sieciowej w podsieci |10.0.0.8 |
| **privateIPAllocationMethod** |Metoda alokacji IP |Dynamiczne lub statyczne |
| **enableIPForwarding** |Określa, czy karta sieciowa może służyć do routingu |wartość PRAWDA lub FAŁSZ |
| **podstawowy** |Określa, czy karta sieciowa jest podstawowym karty Sieciowej dla maszyny Wirtualnej |wartość PRAWDA lub FAŁSZ |
| **publicznego adresu IP** |PIP skojarzona z kartą Sieciową |zobacz [ustawienia DNS](#DNS-settings) |
| **Loadbalancerbackendaddresspool** |Pule adresów zakończenia karty interfejsu Sieciowego jest skojarzony z kopii | |
| **Dodatkowe** |Liczba przychodzących reguł NAT modułu równoważenia obciążenia, karta sieciowa jest skojarzony z | |

Przykładowe publicznego adresu IP w formacie JSON:

    {
        "name": "lb-nic1-be",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkInterfaces",
        "location": "eastus",
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "ipConfigurations": [
                {
                    "name": "NIC-config",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/NIC-config",
                    "etag": "W/\"0027f1a2-3ac8-49de-b5d5-fd46550500b1\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "privateIPAddress": "10.0.0.4",
                        "privateIPAllocationMethod": "Dynamic",
                        "subnet": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet"
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1"
                            }
                        ]
                    }
                }
            ],
            "dnsSettings": { ... },
            "macAddress": "00-0D-3A-10-F1-29",
            "enableIPForwarding": false,
            "primary": true,
            "virtualMachine": {
                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Compute/virtualMachines/web1"
            }
        }
    }

### <a name="additional-resources"></a>Dodatkowe zasoby
* Odczyt [dokumentacji interfejsu API REST](https://msdn.microsoft.com/library/azure/mt163579.aspx) dla kart sieciowych.

