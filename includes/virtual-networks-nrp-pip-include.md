## <a name="public-ip-address"></a>Publiczny adres IP
Zasób publicznego adresu IP zawiera albo zastrzeżonego lub dynamiczny internetowy adres IP. Mimo że można utworzyć publiczny adres IP, jako autonomiczny obiekt, należy skojarzyć je do innego obiektu użyć adresu. Możesz skojarzyć publicznego adresu IP do modułu równoważenia obciążenia, bramy aplikacji lub karty Sieciowej, aby zapewnić dostęp do Internetu dla tych zasobów.  

| Właściwość | Opis | Przykładowe wartości |
| --- | --- | --- |
| **publicIPAllocationMethod** |Określa, czy adres IP jest *statycznych* lub *dynamiczne*. |statyczna, dynamiczne |
| **idleTimeoutInMinutes** |Określa limit czasu bezczynności, wartość domyślna wynosi 4 minut. Jeśli w tej chwili nie zostanie odebrana nie więcej pakietów dla danej sesji, sesja zostanie zakończona. |dowolna wartość od 4 do 30 |
| **adres IP** |Adres IP przypisany do obiektu. Jest to właściwość tylko do odczytu. |104.42.233.77 |

### <a name="dns-settings"></a>Ustawienia DNS
Publiczne adresy IP mieć obiektu podrzędnego o nazwie **dnsSettings** zawierającą następujące właściwości:

| Właściwość | Opis | Przykładowe wartości |
| --- | --- | --- |
| **domainNameLabel** |Hosta o nazwie używany do rozpoznawania nazw. |WWW, ftp, vm1 |
| **Nazwa FQDN** |Pełna nazwa publicznego adresu IP. |www.westus.cloudapp.Azure.com |
| **reverseFqdn** |Pełna nazwa domeny jest rozpoznawana jako adres IP, który jest zarejestrowany w systemie DNS jako rekord PTR. |www.contoso.com. |

Przykładowe publicznego adresu IP w formacie JSON:

    {
       "name": "PIP01",
       "location": "North US",
       "tags": { "key": "value" },
       "properties": {
          "publicIPAllocationMethod": "Static",
          "idleTimeoutInMinutes": 4,
          "ipAddress": "104.42.233.77",
          "dnsSettings": {
             "domainNameLabel": "mylabel",
             "fqdn": "mylabel.westus.cloudapp.azure.com",
             "reverseFqdn": "contoso.com."
          }
       }
    } 

### <a name="additional-resources"></a>Dodatkowe zasoby
* Uzyskaj więcej informacji [publiczne adresy IP](../articles/virtual-network/virtual-networks-reserved-public-ip.md).
* Dowiedz się więcej o [wystąpienie poziomu publiczne adresy IP](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).
* Odczyt [dokumentacji interfejsu API REST](https://msdn.microsoft.com/library/azure/mt163638.aspx) publicznego adresu IP adresów.

