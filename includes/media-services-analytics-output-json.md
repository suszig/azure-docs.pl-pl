To zadanie tworzy dane wyjściowe JSON zawierający metadane dotyczące wykrytego i śledzonych kroje. Metadanych zawiera współrzędne wskazującą lokalizację kroje, a także numer identyfikacyjny krój, wskazując śledzenia tej osoby. Numery identyfikatorów krój są podatne na zresetować w sytuacji, gdy czołowego kroju zostanie utracony lub pokrywający się w ramce spowodować, że niektóre osoby pobierania przypisanych wiele identyfikatorów.

Dane wyjściowe JSON obejmuje następujące elementy:

### <a name="root-json-elements"></a>Elementy JSON głównego

| Element | Opis |
| --- | --- |
| wersja |Odnosi się do wersji interfejsu API wideo. |
| skali czasu |"Impulsów" na sekundę wideo. |
| Przesunięcie |Jest to przesunięcie czasu sygnatur czasowych. W wersji 1.0 interfejsów API, wideo ta będzie zawsze równa 0. W przyszłości scenariusze, które firma Microsoft obsługuje, ta wartość może zmienić. |
| szerokość, wysoki |Szerokość i wysoki ramki wyjście wideo w pikselach.|
| szybkość klatek |Klatek na sekundę wideo. |
| [fragmenty](#fragments-json-elements) |Metadane fragmentaryczne jest się w różnych segmentach fragmenty. Każdy fragmentu zawiera rozpoczęcia, czas trwania, liczba interwałów i zdarzenia. |

### <a name="fragments-json-elements"></a>Fragmenty elementów JSON

|Element|Opis|
|---|---|
| rozpoczynanie |Czas rozpoczęcia pierwsze zdarzenie parametrem "ticks". |
| Czas trwania |Długość fragmentu w parametrem "ticks". |
| indeks | (Dotyczy tylko z usługi Azure Media Redactor) definiuje indeks bieżącego zdarzenia ramki. |
| interval |Interwał każdego wpisu zdarzeń w obrębie fragmentu, w parametrem "ticks". |
| zdarzenia |Każde zdarzenie zawiera kroje wykryte i śledzone w tym czas trwania. Jest tablicą zdarzeń. Zewnętrzne tablicy reprezentuje jeden interwał czasu. Wewnętrzny tablicy składa się z 0 lub więcej zdarzeń, które wystąpiły w danym momencie. [] Nawiasu pusty oznacza, że nie wykryto żadnych kroje. |
| id |Identyfikator krój, który jest śledzona. Ta liczba może spowodować niezamierzoną zmianę, jeśli krój staje się niewykryte. Danej osoby powinny mieć ten sam identyfikator w ogólnej wideo, ale to nie można zagwarantować ze względu na ograniczenia w algorytmie wykrywania (okluzji itp.). |
| x, y |Lewy górny X i Y współrzędne kroju obwiedni w skali znormalizowane 0,0 do 1,0. <br/>-X i Y współrzędne są względem poziomą zawsze, więc jeśli pionowo wideo (lub dołu, w przypadku systemu iOS), musisz odpowiednio Przestaw współrzędne. |
| szerokość, wysokość |Szerokość i wysokość powierzchni obwiedni w skali znormalizowane 0,0 do 1,0. |
| facesDetected |To znajduje się na końcu wyniki JSON i zawiera podsumowanie liczby kroje wykrytych przez algorytm podczas wideo. Ponieważ identyfikatory można zresetować przypadkowo Jeśli krój staje się niewykryte (np. kroju wychodzeniu ekranu, wygląda optymalizacji), ta liczba nie zawsze równa true liczba powierzchni wideo. |
