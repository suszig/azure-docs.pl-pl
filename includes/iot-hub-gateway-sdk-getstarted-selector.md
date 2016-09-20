> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

Ten artykuł zawiera szczegółowy przewodnik dotyczący [przykładowego kodu Witaj, świecie][lnk-helloworld-sample] w celu zilustrowania podstawowych składników architektury [zestawu SDK usługi Azure Gateway][lnk-gateway-sdk]. W przykładzie użyto zestawu SDK usługi Gateway do utworzenia prostej bramy, która co pięć sekund rejestruje w pliku komunikat „witaj, świecie”.

Przewodnik składa się z następujących elementów:

- **Pojęcia**: omówienie składników wchodzących w skład dowolnej bramy utworzonej za pomocą zestawu SDK usługi Gateway.  
- **Przykładowa architektura Witaj, świecie**: opisuje sposób stosowania pojęć do przykładu Witaj, świecie oraz wzajemne dopasowanie składników.
- **Jak utworzyć przykład**: czynności wymagane do utworzenia przykładu.
- **Jak uruchomić przykład**: czynności wymagane do uruchomienia przykładu. 
- **Typowe dane wyjściowe**: przykładowe dane wyjściowe, których należy spodziewać się po uruchomieniu przykładu.
- **Fragmenty kodu**: kolekcja fragmentów kodu pokazujących, w jaki sposób przykład Witaj, świecie implementuje kluczowe składniki bramy.

## Pojęcia zestawu SDK usługi Gateway

Przed wypróbowaniem przykładowego kodu lub utworzeniem własnej bramy pola za pomocą zestawu SDK usługi Gateway musisz zrozumieć kluczowe pojęcia, które stanowią podstawę architektury zestawu SDK.

### Moduły

Brama powstaje w wyniku utworzenia i złożenia *modułów* za pomocą zestawu SDK usługi Azure IoT Gateway. Moduły wymieniają między sobą dane za pomocą *komunikatów*. Moduł odbiera komunikat, wykonuje na nim jakąś akcję, opcjonalnie przekształca go w nowy komunikat, a następnie publikuje go w celu przetworzenia przez inne moduły. Niektóre moduły mogą jedynie tworzyć nowe komunikaty i nigdy nie przetwarzają komunikatów przychodzących. Łańcuch modułów tworzy potok przetwarzania danych, w którym każdy moduł wykonuje przekształcenie danych w jednym punkcie tego potoku.

![][1]
 
Na zestaw SDK składają się następujące elementy:

- Wstępnie napisane moduły, które wykonują typowe funkcje bramy.
- Interfejsy, za pomocą których deweloperzy mogą pisać moduły niestandardowe.
- Infrastruktura niezbędna do wdrożenia i uruchomienia zestawu modułów.

Zestaw SDK zawiera warstwę abstrakcji umożliwiającą tworzenie bram, które można uruchamiać w różnych systemach operacyjnych i na różnych platformach.

![][2]

### Komunikaty

Wyobrażenie modułów przekazujących sobie nawzajem komunikaty ułatwia zrozumienie sposobu działania bramy, jednak nie odzwierciedla precyzyjnie tego procesu. Moduły komunikują się ze sobą za pomocą magistrali komunikatów, publikują komunikaty w magistrali, a magistrala emituje komunikaty do wszystkich modułów podłączonych do magistrali.

Moduł publikuje komunikaty w magistrali wiadomości, używając funkcji **MessageBus_Publish**. Magistrala komunikatów dostarcza komunikaty do modułu za pomocą funkcji wywołania zwrotnego. Komunikat składa się z zestawu właściwości klucz/wartość oraz zawartości przekazywanej w postaci bloku pamięci.

![][3]

Każdy moduł jest odpowiedzialny za filtrowanie komunikatów, ponieważ magistrala komunikatów używa mechanizmu emisji, aby dostarczyć każdy komunikat do każdego podłączonego do niej modułu. Moduł powinien działać tylko w przypadku komunikatów, które są dla niego przeznaczone. Filtrowanie komunikatów skutecznie tworzy potok komunikatów. Zazwyczaj moduł filtruje odebrane komunikaty, identyfikując te, które powinien przetwarzać, przy użyciu właściwości komunikatów.

## Przykładowa architektura Witaj, świecie

Przykład Witaj, świecie ilustruje pojęcia opisane w poprzedniej sekcji. Przykład Witaj, świecie implementuje bramę, która zawiera potok składający się z dwóch modułów:

-   Moduł *witaj, świecie* co pięć sekund tworzy komunikat i przekazuje go do modułu rejestratora.
-   Moduł *rejestratora* zapisuje odebrane komunikaty w pliku.

![][4]

Zgodnie z opisem w poprzedniej sekcji, moduł Witaj, świecie nie przekazuje komunikatów co pięć sekund bezpośrednio do modułu rejestratora. Zamiast tego co pięć sekund publikuje komunikat w magistrali komunikatów.

Moduł rejestratora odbiera komunikaty z magistrali komunikatów oraz sprawdza ich właściwości w filtrze. Jeśli moduł rejestratora stwierdza, że powinien przetworzyć komunikat, zapisuje jego zawartość w pliku.

Moduł rejestratora jedynie używa komunikatów z magistrali, natomiast nigdy nie publikuje w magistrali nowych komunikatów.

![][5]

Powyższy rysunek przedstawia architekturę przykładu Witaj, świecie i ścieżki względne do plików źródłowych, które implementują różne części przykładu w [repozytorium][lnk-gateway-sdk]. Wypróbuj kod samodzielnie lub użyj poniższych fragmentów kodu jako wskazówki.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk


<!--HONumber=sep16_HO1-->


