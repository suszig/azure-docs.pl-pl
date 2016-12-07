> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)
> 
> 

Ten artykuł zawiera szczegółowy przewodnik dotyczący [przykładowego kodu Witaj, świecie][lnk-helloworld-sample] w celu zilustrowania podstawowych składników architektury [zestawu SDK usługi Azure Gateway][lnk-gateway-sdk]. W przykładzie użyto zestawu SDK usługi IoT Hub Gateway do utworzenia prostej bramy, która co pięć sekund rejestruje w pliku komunikat „witaj, świecie”.

Przewodnik składa się z następujących elementów:

* **Pojęcia**: omówienie składników wchodzących w skład dowolnej bramy utworzonej za pomocą zestawu SDK usługi IoT Gateway.  
* **Przykładowa architektura Witaj, świecie**: opisuje sposób stosowania pojęć do przykładu Witaj, świecie oraz wzajemne dopasowanie składników.
* **Jak utworzyć przykład**: czynności wymagane do utworzenia przykładu.
* **Jak uruchomić przykład**: czynności wymagane do uruchomienia przykładu. 
* **Typowe dane wyjściowe**: przykładowe dane wyjściowe, których należy spodziewać się po uruchomieniu przykładu.
* **Fragmenty kodu**: kolekcja fragmentów kodu pokazujących, w jaki sposób przykład Witaj, świecie implementuje kluczowe składniki bramy.

## <a name="azure-iot-gateway-sdk-concepts"></a>Pojęcia dotyczące zestawu SDK usługi Azure IoT Gateway
Przed wypróbowaniem przykładowego kodu lub utworzeniem własnej bramy pola za pomocą zestawu SDK usługi IoT Gateway musisz zrozumieć kluczowe pojęcia, które stanowią podstawę architektury zestawu SDK.

### <a name="modules"></a>Moduły
Brama powstaje w wyniku utworzenia i złożenia *modułów* za pomocą zestawu SDK usługi Azure IoT Gateway. Moduły wymieniają między sobą dane za pomocą *komunikatów*. Moduł odbiera komunikat, wykonuje na nim jakąś akcję, opcjonalnie przekształca go w nowy komunikat, a następnie publikuje go w celu przetworzenia przez inne moduły. Niektóre moduły mogą jedynie tworzyć nowe komunikaty i nigdy nie przetwarzają komunikatów przychodzących. Łańcuch modułów tworzy potok przetwarzania danych, w którym każdy moduł wykonuje przekształcenie danych w jednym punkcie tego potoku.

![Łańcuch modułów w bramie skompilowanej przy użyciu zestawu Azure IoT Gateway SDK][1]

Na zestaw SDK składają się następujące elementy:

* Wstępnie napisane moduły, które wykonują typowe funkcje bramy.
* Interfejsy, za pomocą których deweloperzy mogą pisać moduły niestandardowe.
* Infrastruktura niezbędna do wdrożenia i uruchomienia zestawu modułów.

Zestaw SDK zawiera warstwę abstrakcji umożliwiającą tworzenie bram, które można uruchamiać w różnych systemach operacyjnych i na różnych platformach.

![Warstwa abstrakcji zestawu SDK usługi Azure IoT Gateway][2]

### <a name="messages"></a>Komunikaty
Wyobrażenie modułów przekazujących sobie nawzajem komunikaty ułatwia zrozumienie sposobu działania bramy, jednak nie odzwierciedla precyzyjnie tego procesu. Moduły komunikują się ze sobą przy użyciu brokera, publikując komunikaty do brokera (wzorzec komunikatów magistrali, pubsub lub inny), a następnie pozwalając brokerowi przekazać komunikat do modułów, które są do niego podłączone.

Moduł publikuje komunikaty do brokera przy użyciu funkcji **Broker_Publish**. Broker dostarcza komunikaty do modułu za pomocą funkcji wywołania zwrotnego. Komunikat składa się z zestawu właściwości klucz/wartość oraz zawartości przekazywanej w postaci bloku pamięci.

![Rola brokera w zestawie Azure IoT Gateway SDK][3]

### <a name="message-routing-and-filtering"></a>Routing i filtrowanie komunikatów
Istnieją dwa sposoby kierowania komunikatów do odpowiednich modułów. Zestaw linków może być przekazywany do brokera, aby broker znał źródło i ujście każdego modułu, lub moduł może filtrować właściwości komunikatu. Moduł powinien podejmować działania dotyczące komunikatu tylko wtedy, gdy komunikat jest przeznaczony dla niego. Linki i filtrowanie komunikatów efektywnie tworzą potok komunikatów.

## <a name="hello-world-sample-architecture"></a>Przykładowa architektura Witaj, świecie
Przykład Witaj, świecie ilustruje pojęcia opisane w poprzedniej sekcji. Przykład Witaj, świecie implementuje bramę, która zawiera potok składający się z dwóch modułów:

* Moduł *witaj, świecie* co pięć sekund tworzy komunikat i przekazuje go do modułu rejestratora.
* Moduł *rejestratora* zapisuje odebrane komunikaty w pliku.

![Architektura przykładowej aplikacji Hello World skompilowanej przy użyciu zestawu Azure IoT Gateway SDK][4]

Zgodnie z opisem w poprzedniej sekcji, moduł Witaj, świecie nie przekazuje komunikatów co pięć sekund bezpośrednio do modułu rejestratora. Zamiast tego publikuje komunikaty do brokera co pięć sekund.

Moduł rejestratora odbiera komunikaty od broker i podejmuje działania dotyczące tych komunikatów, zapisując treść komunikatu do pliku.

Moduł rejestratora używa jedynie komunikatów z brokera, natomiast nigdy nie publikuje do brokera nowych komunikatów.

![Kierowanie komunikatów między modułami przez brokera w zestawie Azure IoT Gateway SDK][5]

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

<!--HONumber=Nov16_HO3-->


