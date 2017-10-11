## <a name="azure-dns"></a>System DNS platformy Azure
Usługa DNS platformy Azure to Usługa hostingu domen DNS, rozpoznawania nazw przy użyciu infrastruktury Microsoft Azure.

| Właściwość | Opis | Przykładowa wartość |
| --- | --- | --- |
| **DNSzones** |Informacji o strefie domeny do rekordów hosta DNS dla określonej domeny |/ subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com " |

### <a name="dns-record-sets"></a>Zestawy rekordów DNS
Strefy DNS mieć obiektu podrzędnego o nazwie zestaw rekordów. Zestawy rekordów jest kolekcją rekordów hosta przez typ strefy DNS. Typy rekordów są A, AAAA, CNAME, MX, NS, SOA, SRV i TXT.

| Właściwość | Opis | Wartość przykładowa |
| --- | --- | --- |
| A |Typ rekordu IPv4 |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/A/www |
| AAAA |Typ rekordu IPv6 |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord |
| CNAME |Nazwa kanoniczna typu rekordu <sup>1</sup> |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/CNAME/www |
| MX |Typ rekordu poczty |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/MX/mail |
| NS |Typ rekordu nazwy serwera |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/NS/ |
| SOA |Początek typ rekordu urząd <sup>2</sup> |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SOA |
| SRV |Typ rekordu usługi |/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SRV |

<sup>1</sup> zezwala na tylko jedną wartość na zestawie rekordów.

<sup>2</sup> zezwala na tylko jeden typ rekordów SOA dla stref DNS. 

Przykład strefy DNS w formacie Json:

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "newZoneName": {
          "type": "String",
          "metadata": {
              "description": "The name of the DNS zone to be created."
          }
        },
        "newRecordName": {
          "type": "String",
          "defaultValue": "www",
          "metadata": {
              "description": "The name of the DNS record to be created.  The name is relative to the zone, not the FQDN."
          }
        }
      },
      "resources": 
      [
        {
          "type": "microsoft.network/dnszones",
          "name": "[parameters('newZoneName')]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": {
          }
        },
        {
          "type": "microsoft.network/dnszones/a",
          "name": "[concat(parameters('newZoneName'), concat('/', parameters('newRecordName')))]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": 
          {
            "TTL": 3600,
            "ARecords": 
            [
                {
                    "ipv4Address": "1.2.3.4"
                },
                {
                    "ipv4Address": "1.2.3.5"
                }
            ]
          },
          "dependsOn": [
            "[concat('Microsoft.Network/dnszones/', parameters('newZoneName'))]"
          ]
        }
          ]
    }

## <a name="additional-resources"></a>Dodatkowe zasoby
Odczyt [dokumentacja interfejsu API REST dla stref DNS ](https://msdn.microsoft.com/library/azure/mt130626.aspx) Aby uzyskać więcej informacji.

Odczyt [dokumentacja interfejsu API REST dla zestawów rekordów DNS](https://msdn.microsoft.com/library/azure/mt130627.aspx) Aby uzyskać więcej informacji.

