Agent synchronizacji plików Azure została zaktualizowana, regularne dodawanie nowych funkcji, a także do rozwiązywania problemów. Firma Microsoft zaleca się skonfigurowanie usługi Microsoft Update do pobierania aktualizacji dla agenta synchronizacji plików Azure, ponieważ są one dostępne. Rozumiemy, że niektóre organizacje mają być ścisłej kontroli i testowanie aktualizacji.

W przypadku wdrożeń, korzystających z wcześniejszych wersji agenta synchronizacji plików Azure:

- Po początkowej wydaniu nowej wersji głównej usługi synchronizacji magazynu honoruje wcześniejszej wersji przez trzy miesiące. Na przykład usługa synchronizacji magazynu obsługuje wersji 1. \* przez trzy miesiące po wydaniu wersji 2. \*.
- Po upływie trzech miesięcy, usługa synchronizacji magazynu blokuje zarejestrowane serwery wygasłą wersją z synchronizacji z grupami synchronizacji.
- Podczas trzech miesięcy, które jest honorowane wcześniejszej wersji wszystkie poprawki go tylko do bieżącej wersji głównej (nowy).

> [!Note]  
> Jeśli wersji synchronizacji plików Azure wygaśnie w ciągu trzech miesięcy, wiesz już za pośrednictwem wyskakujące powiadomienie w portalu Azure.
