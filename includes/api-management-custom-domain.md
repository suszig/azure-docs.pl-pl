## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>Sposób odpowiadania przez serwer Proxy APIM z certyfikatów SSL w uzgadnianie protokołu TLS

### <a name="clients-calling-with-sni-header"></a>Wywoływanie z nagłówkiem SNI klientów
Jeśli klient ma jednego lub wielu domen niestandardowych skonfigurowane dla serwera Proxy, APIM może odpowiadać na żądania HTTPS z niestandardowej domeny (na przykład contoso.com), a także domyślnej domeny (na przykład apim usług name.azure-api.net). Na podstawie informacji w nagłówku oznaczenia nazwy serwera (SNI), APIM odpowiada odpowiedni certyfikat.

### <a name="clients-calling-without-sni-header"></a>Wywołanie bez nagłówka SNI klientów
Jeśli klient korzysta z klienta, który nie wysyła [SNI](https://tools.ietf.org/html/rfc6066#section-3) nagłówka, tworzy APIM odpowiedzi opartych na logice następujące:

* Jeśli usługa ma tylko jedną domenę niestandardowe skonfigurowane dla serwera Proxy, certyfikat domyślny jest certyfikat, który został wystawiony dla domeny niestandardowej serwera Proxy.
* Jeśli usługa została skonfigurowana dla serwera Proxy wielu domen niestandardowych (obsługiwane tylko w **Premium** warstwy), klienta można określić, który certyfikat powinien być certyfikat domyślny. Aby ustawić domyślnego certyfikatu, [defaultSslBinding](https://docs.microsoft.com/rest/api/apimanagement/apimanagementservice/createorupdate#definitions_hostnameconfiguration) właściwość powinna być ustawiona na true ("defaultSslBinding": "prawda"). Jeśli klient nie ustawiać właściwości, certyfikat domyślny jest certyfikat wystawiony dla domyślnej domeny serwera Proxy hostowanej w lokalizacji *.azure api.net.

## <a name="support-for-putpost-request-with-large-payload"></a>Obsługa żądania PUT/POST o dużych ładunku

Serwer APIM Proxy obsługuje żądania z duży ładunek podczas korzystania z certyfikatów klienta przy użyciu protokołu HTTPS (na przykład ładunku > od 40 KB). Aby zapobiec zamrażanie żądania serwera, klientów można ustawić właściwości ["negotiateClientCertificate": "prawda"](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/CreateOrUpdate#definitions_hostnameconfiguration) na nazwę hosta serwera Proxy. Jeśli właściwość jest ustawiona na wartość true, klient certyfikatu jest wymagane w czasie połączenia SSL/TLS, przed wszelkie wymiany żądania HTTP. Ponieważ to ustawienie ma zastosowanie w **nazwę hosta serwera Proxy** wszystkie żądania połączenia poziomu, poproś o certyfikat klienta. Klientów można skonfigurować maksymalnie 20 domen niestandardowych dla serwera Proxy (obsługiwane tylko w **Premium** warstwy) i obejście tego ograniczenia.

