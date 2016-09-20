- Sieci wirtualne mogą znajdować się w tym samym regionie lub w różnych regionach (lokalizacjach) świadczenia usługi Azure.

- Usługa w chmurze ani punkt końcowy z równoważeniem obciążenia NIE MOGĄ rozciągać się na sieci wirtualne, nawet jeśli są one ze sobą połączone.

- Łączenie wielu sieci wirtualnych Azure nie wymaga żadnych lokalnych bram VPN, chyba że jest wymagana łączność między różnymi lokalizacjami organizacji.

- Połączenie między sieciami wirtualnymi obsługuje sieci wirtualne. Nie obsługuje połączeń maszyn wirtualnych ani usług w chmurze, które NIE należą do sieci wirtualnej.

- Połączenie między sieciami wirtualnymi wymaga bram sieci VPN Azure z sieciami typu VPN RouteBased (nazywanymi wcześniej sieciami obsługującymi routing dynamiczny). 

- Połączenie sieci wirtualnej może być wykorzystywane równolegle z sieciami VPN obejmującymi wiele lokalizacji z maksymalnie 10 (w przypadku bram domyślnych/standardowych) lub 30 (w przypadku bram wysokiej wydajności) tunelami VPN bramy VPN sieci wirtualnej łączącej z innymi sieciami wirtualnymi lub z innymi witrynami lokalnymi.

- Przestrzenie adresów sieci wirtualnych i lokalnych witryn sieci obejmujących lokalizacje w obrębie organizacji nie mogą się nakładać. Nakładające się przestrzenie adresowe spowodują, że utworzenie połączenia między sieciami wirtualnymi się nie powiedzie.

- Nadmiarowe tunele między dwiema sieciami wirtualnymi nie są obsługiwane.

- Wszystkie tunele VPN sieci wirtualnej współdzielą dostępną przepustowość bramy VPN Azure i są wspólnie objęte umową SLA dotyczącą danej bramy na platformie Azure.

- Ruch sieciowy w ramach połączenia działa między sieciami wirtualnymi w obrębie sieci Microsoft Network, a nie Internetu.

- Ruch sieciowy w ramach połączenia między sieciami wirtualnymi w tym samym regionie jest bezpłatny w obu kierunkach. Opłaty są pobierane za ruch wychodzący w ramach połączenia sieci wirtualnych na przestrzeni różnych regionów, przy czym cenę za wychodzący transfer danych wewnątrz sieci wirtualnej określa się na podstawie regionów źródłowych. Szczegółowe informacje można znaleźć na stronie poświęconej [cenom](https://azure.microsoft.com/pricing/details/vpn-gateway/).


<!--HONumber=sep16_HO1-->


