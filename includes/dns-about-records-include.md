## Informacje o rekordach
Każdy rekord DNS ma nazwę i typ. Rekordy są pogrupowane w różne typy według danych, które zawierają. Najczęściej spotykanym typem jest rekord „A”, który mapuje nazwę na adres IPv4. Innym typem jest rekord „MX”, który mapuje nazwę na serwer poczty e-mail.

Usługa DNS platformy Azure obsługuje wszystkie popularne typy rekordów DNS, takie jak A, AAAA, CNAME, MX, NS, PTR, SOA, SRV i TXT. Należy pamiętać, że:

* Zestawy rekordów SOA są tworzone automatycznie w każdej strefie i nie mogą być tworzone oddzielnie.
* Rekordy SPF powinny być tworzone przy użyciu typu rekordu TXT. Więcej informacji znajduje się na [tej stronie](http://tools.ietf.org/html/rfc7208#section-3.1).

W usłudze DNS platformy Azure rekordy są określane przy użyciu nazw względnych. „W pełni kwalifikowana” nazwa domeny (FQDN) zawiera nazwę strefy, której nie zawiera nazwa „względna”. Na przykład względna nazwa rekordu „www” w strefie „contoso.com” daje w pełni kwalifikowaną nazwę rekordu www.contoso.com.

## Informacje o zestawach rekordów
Czasami trzeba utworzyć więcej niż jeden rekord DNS określonego typu o danej nazwie. Na przykład załóżmy, że witryna sieci Web „www.contoso.com” jest hostowana pod dwoma różnymi adresami IP. Witryna sieci Web wymaga dwóch różnych rekordów A, po jednym dla każdego adresu IP. Oto przykład zestawu rekordów:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Usługa DNS platformy Azure zarządza rekordami DNS za pomocą zestawów rekordów. Zestaw rekordów jest kolekcją rekordów DNS w strefie, które mają taką samą nazwę i są tego samego typu. Większość zestawów rekordów zawiera jeden rekord, ale sytuacje, w których zestaw rekordów zawiera więcej niż jeden rekord, również często mają miejsce.

Zestawy rekordów SOA i CNAME stanowią wyjątki. Standardy usługi DNS nie zezwalają na występowanie wielu rekordów tych typów o takiej samej nazwie.

Czas wygaśnięcia (TTL) określa, jak długo każdy rekord jest buforowany przez klientów przed ponownym uruchomieniem zapytania. W tym przykładzie czas wygaśnięcia wynosi 3600 sekund lub 1 godzinę. Czas wygaśnięcia jest określany dla zestawu rekordów, a nie dla każdego rekordu, więc dla wszystkich rekordów w danym zestawie rekordów jest używana taka sama wartość.

#### Zestawy rekordów z użyciem symboli wieloznacznych
Usługa DNS platformy Azure obsługuje [rekordy z użyciem symboli wieloznacznych](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Są one zwracane dla dowolnego zapytania o zgodnej nazwie (chyba że istnieje lepsze dopasowanie pochodzące z zestawu rekordów bez symboli wieloznacznych). Zestawy rekordów z użyciem symboli wieloznacznych są obsługiwane dla wszystkich typów rekordów z wyjątkiem NS i SOA.  

Aby utworzyć zestaw rekordów z użyciem symboli wieloznacznych, użyj nazwy zestawu rekordów „\*”. Można też użyć nazwy z etykietą „\*”, np. „\*.foo”.

#### Zestawy rekordów CNAME
Zestawy rekordów CNAME nie mogą współistnieć z innymi zestawami rekordów o tej samej nazwie. Na przykład nie można utworzyć jednocześnie zestawu rekordów CNAME o nazwie względnej „www” oraz rekordu A o nazwie względnej „www”. Ponieważ wierzchołek strefy (nazwa = „@”) zawsze zawiera zestawy rekordów NS i SOA, które zostały utworzone podczas tworzenia strefy, nie można utworzyć zestawu rekordów CNAME w wierzchołku strefy. Te ograniczenia wynikają ze standardów dotyczących serwerów DNS i nie stanowią ograniczeń usługi DNS platformy Azure.

<!--HONumber=Sep16_HO4-->


