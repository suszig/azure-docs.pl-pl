#### Tworzenie zestawu rekordów A z pojedynczym rekordem

Aby utworzyć zestaw rekordów, użyj polecenia `azure network dns record-set create`. Określ grupę zasobów, nazwę strefy, nazwę względną zestawu rekordów, typ rekordu i czas wygaśnięcia (TTL).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Po utworzeniu zestawu rekordów A dodaj do zestawu rekordów adres IPv4 przy użyciu polecenia `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### Tworzenie zestawu rekordów AAAA z pojedynczym rekordem

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### Tworzenie zestawu rekordów CNAME z pojedynczym rekordem

Rekordy CNAME mogą zawierać tylko jedną wartość ciągu.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### Tworzenie zestawu rekordów MX z pojedynczym rekordem

W tym przykładzie używamy nazwy zestawu rekordów „@”, aby utworzyć rekord MX w wierzchołku strefy (w tym przypadku „contoso.com”). Jest to popularny scenariusz w przypadku rekordów MX.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### Tworzenie zestawu rekordów NS z pojedynczym rekordem

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### Tworzenie zestawu rekordów SRV z pojedynczym rekordem

Jeśli tworzysz rekord SRV w katalogu głównym strefy, możesz określić w nazwie rekordu wartości „_usługa” i „_protokół”. Nie trzeba wpisywać w nazwie rekordu nazwy „@”.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### Tworzenie zestawu rekordów TXT z pojedynczym rekordem

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"



<!--HONumber=Jun16_HO2-->


