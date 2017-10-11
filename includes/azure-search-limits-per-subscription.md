Możesz utworzyć wiele usług w ramach subskrypcji, każdy z nich zainicjowane w określonej warstwy, ograniczona tylko liczby dozwolone w poszczególnych warstwach usług. Na przykład można utworzyć maksymalnie 12 usług w warstwie podstawowa i innym 12 usług w warstwie S1 w ramach tej samej subskrypcji. Aby uzyskać więcej informacji na temat warstw, zobacz [wybrać SKU lub warstwy usługi Azure Search](../articles/search/search-sku-tier.md).

Ograniczenia usługi maksymalną można uruchamiany na żądanie. Jeśli potrzebujesz większej liczby usług w ramach tej samej subskrypcji, skontaktuj się z pomocą techniczną platformy Azure.

| Zasób | Bezpłatna | Podstawowa | S1 | S2 | S3 | S3 HD <sup>1</sup> |
| --- | --- | --- | --- | --- | --- | --- |
| Maksymalna usług |1 |12 |12 |6 |6 |6 |
| Maksymalną skalę w SU <sup>2</sup> |N/D <sup>3</sup> |3 SU <sup>4</sup> |36 SU |36 SU |36 SU |36 SU |

<sup>1</sup> S3 HD nie obsługuje [indeksatory](../articles/search/search-indexer-overview.md) w tej chwili. 

<sup>2</sup> jednostek wyszukiwania (SU) są rozliczeń jednostki, przydzielone jako *repliki* lub *partycji*. Oba zasoby są wymagane dla magazynu, indeksowanie i operacje zapytań. Aby dowiedzieć się więcej na temat sposobu jednostek wyszukiwania są obliczane, a także wykres prawidłowe kombinacje, które pozostanie w granicach maksymalnej, zobacz [skalować poziomy zasobów dla obciążeń zapytań i indeksu](../articles/search/search-capacity-planning.md). 

<sup>3</sup> wolne opiera się na udostępnionych zasobów używanych przez wielu subskrybentów. W tej warstwie Brak zasobów dedykowanych dla poszczególnych subskrybentów. Z tego powodu maksymalną skalę jest oznaczona jako nie ma zastosowania.

<sup>4</sup> basic ma jedną partycję stałym. W tej warstwie SUs dodatkowe służą do przydzielania więcej replik dla obciążeń zwiększona zapytania.

