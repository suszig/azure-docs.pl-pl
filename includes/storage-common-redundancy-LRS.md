Magazyn lokalnie nadmiarowy (LRS) ma na celu zapewnienie co najmniej 99.999999999% (11 na 9) trwałość obiektów w danym roku przez replikowanie danych w ramach jednostki skalowania magazynu, który znajduje się w centrum danych w regionie, w którym utworzono konto magazynu. Żądanie zapisu pomyślnie zwraca tylko wtedy, gdy został zapisany do wszystkich replik. Te repliki każdego znajdują się w oddzielnych domen błędów i zaktualizuj domen w jedną jednostkę skalowania magazynu.

Jednostki skalowania magazynu to zbiór stojakami węzłów magazynu. Domeny błędów (FD) to grupa węzłów, które reprezentują fizyczną jednostkę awarii i mogą być uważane za węzłach należących do tej samej fizycznej stojaku. Domeny uaktualnienia (UD) to grupa węzłów, które są ze sobą uaktualniony podczas procesu uaktualniania usługi (wdrażanie). Repliki są rozkładane między UDs i FDs w jedną jednostkę skalowania magazynu, aby upewnić się, czy dane są dostępne, nawet w przypadku awarii sprzętu ma wpływ na jednym stojaku lub po uaktualnieniu węzłów podczas wdrażania.

Magazyn LRS jest opcją najniższy koszt i oferuje co najmniej trwałości w porównaniu do innych opcji. W przypadku awarii poziomu Centrum danych (fire zalewania itp.) wszystkie repliki może być utracone lub ich nieodwracalnej utraty. Aby zmniejszyć to zagrożenie, z magazynu geograficznie nadmiarowego magazynu (GRS) jest zalecane dla większości aplikacji.

Magazyn lokalnie nadmiarowy nadal może być wskazane w niektórych scenariuszach:

* Zapewnia najwyższy maksymalna przepustowość opcji replikacji magazynu Azure.
* Jeśli aplikacja przechowuje dane, które można łatwo odtworzyć, można wybrać opcję LRS.
* Niektóre aplikacje są ograniczone do replikowanie danych tylko w obrębie kraju ze względu na wymagania ładu danych. Region sparowanego może być w innym kraju. Aby uzyskać więcej informacji na pary regionu, zobacz [regiony platformy Azure](https://azure.microsoft.com/regions/).
