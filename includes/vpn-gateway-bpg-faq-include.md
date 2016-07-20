### Czy protokół BGP jest obsługiwany na wszystkich jednostkach SKU bramy sieci VPN platformy Azure?

Nie, protokół BGP jest obsługiwany przez **standardową** i **wysokowydajną** bramę sieci VPN. **Podstawowa** jednostka SKU nie jest obsługiwana.

### Czy można użyć protokołu BGP z bramą sieci VPN platformy Azure opartej na zasadach?

Nie, protokół BGP jest obsługiwany tylko przez bramy sieci VPN opartych na trasach.

### Czy można użyć prywatnych numerów ASN (numery systemu autonomicznego)?

Tak, można użyć własnych publicznych lub prywatnych numerów ASN dla sieci lokalnych i sieci wirtualnych platformy Azure.

### Czy można użyć tego samego numeru ASN zarówno dla lokalnych sieci VPN, jak sieci wirtualnych platformy Azure?

Nie, należy przypisać różne numery ASN sieciom lokalnym i sieciom wirtualnym platformy Azure, jeśli są łączone za pomocą protokołu BGP. Bramy sieci VPN platformy Azure mają przypisany domyślny numer ASN 65515, niezależnie od tego, czy protokół BGP jest włączony dla łączności między różnymi lokalizacjami firmy. Można zastąpić to ustawienie domyślne, przypisując inny numer ASN podczas tworzenia bramy sieci VPN, lub zmienić numer ASN po utworzeniu bramy. Konieczne będzie przypisanie lokalnych numerów ASN do odpowiednich bram sieci lokalnej platformy Azure.



### Jakie prefiksy adresów będą anonsowane do użytkownika przez bramy sieci VPN platformy Azure?

Brama sieci VPN będzie anonsować następujące trasy na urządzeniach lokalnych działających według protokołu BGP:

- Prefiksy adresów sieci wirtualnej użytkownika
- Prefiksy adresów dla każdej bramy sieci lokalnej podłączonej do bramy sieci VPN platformy Azure
- Trasy zostały uzyskane na podstawie innych sesji równorzędnych protokołu BGP podłączonych do bramy sieci VPN platformy Azure **z wyjątkiem trasy domyślnej lub tras nakładających się dla dowolnego prefiksu sieci wirtualnej**.

### Czy można użyć protokołu BGP do połączeń między sieciami wirtualnymi użytkownika?

Tak, protokołu BGP można użyć do połączeń zarówno między różnymi lokalizacjami, jak i połączeń między sieciami wirtualnymi.

### Czy można mieszać połączenia BGP z połączeniami protokołów innych niż BGP dla bram sieci VPN użytkownika platformy Azure?

Tak, dla tej samej bramy sieci VPN platformy Azure można łączyć zarówno połączenia protokołu BGP, jak i połączenia innych protokołów.

### Czy brama sieci VPN platformy Azure obsługuje routing tranzytowy protokołu BGP?

Tak, routing tranzytowy protokołu BGP jest obsługiwany, ale bramy sieci VPN platformy Azure **nie** anonsują tras domyślnych do innych elementów równorzędnych protokołu BGP. Aby włączyć routing tranzytowy dla wielu bram sieci VPN platformy Azure, należy włączyć protokół BGP dla wszystkich pośrednich połączeń między sieciami wirtualnymi.

### Czy można mieć więcej niż jeden tunel między bramą sieci VPN platformy Azure i siecią lokalną użytkownika?

Tak, można utworzyć więcej niż jeden tunel VPN S2S między bramą sieci VPN platformy Azure i siecią lokalną. Należy pamiętać, że wszystkie te tunele będą uwzględniane w łącznej liczbie tuneli dla bram sieci VPN platformy Azure. Na przykład dwa nadmiarowe tunele między bramą sieci VPN platformy Azure i jedną z sieci lokalnych będą wymagały 2 tuneli spoza łącznego limitu przydziału dla bramy sieci VPN użytkownika platformy Azure (10 dla standardowej i 30 dla wysokowydajnej).

### Czy można mieć wiele tuneli między dwiema sieciami wirtualnymi platformy Azure z protokołem BGP?

Nie, nadmiarowe tunele między dwiema sieciami wirtualnymi nie są obsługiwane.

### Czy można użyć protokołu BGP dla sieci VPN S2S w konfiguracji współistnienia sieci ExpressRoute i sieci VPN S2S?

Nie w tej chwili.

### Jakiego adresu używa brama sieci VPN platformy Azure dla adresu IP elementu równorzędnego protokołu BGP?

Brama sieci VPN platformy Azure przydziela pojedynczy adres IP z zakresu podsieci bramy zdefiniowanego dla sieci wirtualnej. Domyślnie jest to przedostatni adres zakresu. Na przykład jeśli podsiecią bramy jest 10.12.255.0.0/27 z zakresu od 10.42.255.0.0 do 10.42.255.31, adresem IP elementu równorzędnego protokołu BGP dla bramy sieci VPN platformy Azure będzie 10.12.255.30. Te informacje można znaleźć wśród informacji dotyczących bramy sieci VPN platformy Azure.

### Jakie są wymagania dotyczące adresów IP elementów równorzędnych protokołu BGP na urządzeniu sieci VPN użytkownika?

Adres lokalnego elementu równorzędnego protokołu BGP **NIE MOŻE** być taki sam jak publiczny adres IP urządzenia sieci VPN użytkownika. Na urządzeniu sieci VPN należy użyć innego adresu IP dla adresu IP elementu równorzędnego protokołu BGP. Może to być adres przypisany do interfejsu sprzężenia zwrotnego na urządzeniu. Ten adres należy określić w odpowiedniej bramie sieci lokalnej reprezentującej daną lokalizację.

### Co należy określić jako prefiksy adresów użytkownika dla bramy sieci lokalnej w przypadku korzystania z protokołu BGP?

Brama sieci lokalnej platformy Azure określa początkowe prefiksy adresów dla sieci lokalnej. W przypadku protokołu BGP należy przydzielić prefiks hosta (prefiks /32) adresu IP elementu równorzędnego protokołu BGP jako przestrzeń adresów dla danej sieci lokalnej. Jeśli adresem IP elementu równorzędnego protokołu BGP jest 10.52.255.254, należy określić adres „10.52.255.254/32” jako przestrzeń adresów sieci lokalnej dla bramy sieci lokalnej reprezentującej tę sieć lokalną. Należy tak postąpić, aby brama sieci VPN platformy Azure ustanawiała sesję protokołu BGP przez tunel sieci VPN S2S.

### Co należy dodać do lokalnego urządzenia sieci VPN dla sesji połączenia równorzędnego protokołu BGP?

Na urządzeniu sieci VPN należy dodać trasę hosta dla adresu IP elementu równorzędnego protokołu BGP platformy Azure wskazującego tunel sieci VPN S2S protokołu IPsec. Na przykład jeśli adresem IP elementu równorzędnego sieci VPN platformy Azure jest „10.12.255.30”, należy dodać trasę hosta dla adresu „10.12.255.30” z interfejsem następnego skoku pasującym do interfejsu tunelu protokołu IPsec urządzenia sieci VPN użytkownika.


<!--HONumber=Jun16_HO2-->


