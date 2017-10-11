System nazw domen (DNS) jest używana do lokalizowania rzeczy w Internecie. Na przykład podczas wprowadzania adresu w przeglądarce, lub kliknij łącze na stronie sieci web, używa DNS, tłumaczenie domeny na adres IP. Adres IP jest sortowania jak ulicę, ale nie jest bardzo człowieka przyjazny. Na przykład jest znacznie łatwiejsze do zapamiętania nazwę DNS, takich jak **contoso.com** niż do zapamiętania adresu IP, takie jak 192.168.1.88 lub 2001:0:4137:1f67:24a2:3888:9cce:fea3.

DNS system jest oparty na *rekordów*. Rejestruje skojarzenia określony *nazwa*, takich jak **contoso.com**, adres IP lub nazwa DNS innej. W przypadku aplikacji, takich jak przeglądarki sieci web, wyszukiwania nazw w systemie DNS, znajduje rekord i używa niezależnie od wskazuje jako adres. Wartość, która wskazuje jest adresem IP, wartości będą używać przeglądarki. Wskazuje na inną nazwę DNS, aplikacja ma robić rozpoznawania. Ostatecznie rozpoznawanie nazw zakończy adresu IP.

Podczas tworzenia witryny sieci Web platformy Azure, nazwa DNS jest automatycznie przypisany do lokacji. Ta nazwa ma postać  **&lt;yoursitename&gt;. azurewebsites.net**. Podczas dodawania witryny sieci Web jako punkt końcowy usługi Azure Traffic Manager, następnie jest dostępna za pośrednictwem witryny sieci Web  **&lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domeny.

> [!NOTE]
> Gdy witryny sieci Web jest skonfigurowany jako punkt końcowy Menedżera ruchu, użyje **. trafficmanager.net** adresów podczas tworzenia rekordów DNS.
> 
> Można używać tylko rekordy CNAME z Menedżera ruchu
> 
> 

Są również wiele typów rekordów, każda z własnych funkcji i ograniczeń, ale dla witryn sieci Web skonfigurowana do jako punkty końcowe usługi Traffic Manager, tylko Dbamy o jeden; *CNAME* rekordów.

### <a name="cname-or-alias-record"></a>Rekord CNAME lub Alias
Rekord CNAME mapuje *określonych* nazwy DNS, takich jak **mail.contoso.com** lub **www.contoso.com**, do innej nazwy domeny (canonical). W przypadku za pomocą Menedżera ruchu Azure witryn sieci Web, nazwa kanoniczna domeny jest  **&lt;moja_aplikacja >. trafficmanager.net** nazwę domeny profilu usługi Traffic Manager. Po utworzeniu nazwy CNAME tworzy alias dla  **&lt;moja_aplikacja >. trafficmanager.net** nazwy domeny. Wpis CNAME rozwiąże adres IP Twojego  **&lt;moja_aplikacja >. trafficmanager.net** automatycznie, nazwa domeny, jeśli zmieni adres IP witryny sieci Web, nie trzeba podejmować żadnych działań.

Po odebraniu ruchu na Menedżera ruchu następnie przekierowuje ruch do witryny sieci Web przy użyciu metody, które nie jest skonfigurowana do równoważenia obciążenia. To jest całkowicie niewidoczne dla odwiedzający witrynę sieci Web. Użytkownik widzi tylko nazwy domeny niestandardowej w przeglądarce.

> [!NOTE]
> Niektóre rejestratorów domeny umożliwiają tylko mapy poddomen, gdy przy użyciu rekordu CNAME, takich jak **www.contoso.com**, a nie głównego nazwy, takie jak **contoso.com**. Aby uzyskać więcej informacji na rekordy CNAME, zobacz w dokumentacji dostarczonej przez rejestratora, <a href="http://en.wikipedia.org/wiki/CNAME_record">wpis Wikipedia rekordu CNAME</a>, lub <a href="http://tools.ietf.org/html/rfc1035">nazwy domen IETF - wdrażania i specyfikację</a> dokumentu.
> 
> 

