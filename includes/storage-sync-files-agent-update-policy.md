Aktualizacje agenta synchronizacji plików Azure zostaną wydane regularnie, aby dodać nowe funkcje i rozwiąż problemy, które zostały wykryte. Firma Microsoft zaleca, aby skonfigurować Microsoft Update, aby pobrać wszystkie aktualizacje agenta synchronizacji plików Azure, zgodnie z ich wydania. Firma Microsoft Dowiedz się, że niektóre organizacje mają być ścisłej kontroli i testowanie aktualizacji. 

W przypadku wdrożeń, korzystających z wcześniejszych wersji agenta synchronizacji plików Azure:

- Usługa synchronizacji magazynu honoruje poprzednich wersji głównej przez trzy miesiące po wydaniu nowej wersji głównej początkowej. Na przykład usługa synchronizacji magazynu będzie obsługiwać w wersji 1. \* do trzech miesięcy po wydaniu wersji 2. \*.
- Po upływie trzech miesięcy zablokować zarejestrowane serwery, która jest używana wygasła wersja synchronizowanie z grupami synchronizacji rozpoczyna się usługa synchronizacji magazynu.
- W ciągu trzech miesięcy do poprzednich wersji głównej wszystkie poprawki go tylko do bieżącej wersji głównej.

> [!Note]  
> Otrzymasz wyskakujące powiadomienie w witrynie Azure Portal, jeśli używasz usługi Azure File Sync w wersji, która wygaśnie w ciągu następnych trzech miesięcy.