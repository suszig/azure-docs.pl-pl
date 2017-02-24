Magazyn jest ograniczony przez ilość miejsca na dysku albo stały limit *maksymalnej liczby* indeksów lub dokumentów w zależności od tego, która wartość zostanie osiągnięta jako pierwsza.

| Zasób | Bezpłatna | Podstawowa | S1 | S2 | S3 | S3 (wysoka gęstość) |
| --- | --- | --- | --- | --- | --- | --- |
| Umowa dotycząca poziomu usług (SLA) |Nie<sup>1</sup> |Tak |Tak |Tak |Tak |Tak |
| Magazyn na partycję |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Partycje na usługę |Nie dotyczy |1 |12 |12 |12 |3<sup>2</sup> |
| Rozmiar partycji |Nie dotyczy |2 GB |25 GB |100 GB |200 GB |200 GB |
| Repliki |Nie dotyczy |3 |12 |12 |12 |12 |
| Maksymalna liczba indeksów |3 |5 |50 |200 |200 |1000 na partycję lub 3000 na usługę |
| Maksymalna liczba indeksatorów |3 |5 |50 |200 |200 |Brak obsługi indeksatorów |
| Maksymalna liczba źródeł danych |3 |5 |50 |200 |200 |Brak obsługi indeksatorów |
| Maksymalna liczba dokumentów |10 000 |1 mln |15 mln na partycję lub 180 mln na usługę |60 mln na partycję lub 720 mln na usługę |120 mln na partycję lub 1,4 mld na usługę |1 mln na indeks lub 200 mln na partycję |
| Szacowana liczba zapytań na sekundę |Nie dotyczy |Około&3; na replikę |Około&15; na replikę |Około&60; na replikę |Około&60; na replikę |Ponad&60; na replikę |

<sup>1</sup> Bezpłatne jednostki SKU i jednostki SKU w wersji zapoznawczej nie są objęte umowami dotyczącymi poziomu usług (SLA, service level agreement). Umowy SLA są wymuszane, gdy jednostka SKU stanie się ogólnie dostępna.

<sup>2</sup> Usługa S3 (wysoka gęstość) ma stały limit 3 partycji, który jest niższy niż limit partycji dla usługi S3. Niższy limit partycji wynika z tego, że liczba indeksów w przypadku usługi S3 (wysoka gęstość) jest znacznie wyższa. Biorąc pod uwagę, że istnieją limity usługi zarówno dla zasobów obliczeniowych (magazyn i przetwarzanie), jak i zawartości (indeksy i dokumenty), limit zawartości jest osiągany jako pierwszy.


<!--HONumber=Feb17_HO2-->


