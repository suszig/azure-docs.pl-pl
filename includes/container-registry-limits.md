| Zasób | Podstawowa | Standardowa | Premium |
|---|---|---|---|---|
| Magazyn | 10 giB | 100 giB| 500 giB |
| ReadOps na minutę<sup>1, 2</sup> | 1 KB | 300k | 10 000 k |
| WriteOps na minutę<sup>1, 3</sup> | 100 | 500 | 2k |
| Pobierz MB/s przepustowości<sup>1</sup> | 30 | 60 | 100 |
| Przekaż MB/s przepustowości<sup>1</sup> | 10 | 20 | 50 |
| elementów webhook | 2 | 10 | 100 |
| Replikacja geograficzna | Nie dotyczy | Nie dotyczy | [Obsługiwane *(wersja zapoznawcza)*](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *ReadOps*, *WriteOps*, i *przepustowości* są szacowane wartości minimalnej. ACR dokłada starań zwiększyć wydajność, ponieważ wymaga użycia.

<sup>2</sup> [ściągania docker](https://docs.docker.com/registry/spec/api/#pulling-an-image) przekłada się wiele operacji odczytu na podstawie liczby warstw w obrazu, a także pobierania manifestu.

<sup>3</sup> [wypychania docker](https://docs.docker.com/registry/spec/api/#pushing-an-image) tłumaczy wiele operacji zapisu, na podstawie liczby warstw, które muszą zostać przeniesiony. A `docker push` obejmuje *ReadOps* można pobrać manifestu dla istniejącego obrazu.