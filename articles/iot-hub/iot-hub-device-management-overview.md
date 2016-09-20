<properties
 pageTitle="Omówienie zarządzania urządzeniami | Microsoft Azure"
 description="Omówienie zarządzania urządzeniami w usłudze Azure IoT Hub: bliźniaki urządzeń, zapytania urządzeń, zadania urządzeń"
 services="iot-hub"
 documentationCenter=""
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="juanpere"/>

# Omówienie zarządzania urządzeniami w usłudze Azure IoT Hub (wersja zapoznawcza)

Zarządzanie urządzeniami w usłudze Azure IoT Hub umożliwia oparte na standardach zarządzanie urządzeniami IoT pozwalające na zdalne konfigurowanie i aktualizowanie urządzeń oraz zarządzanie nimi.

Istnieją trzy główne pojęcia związane z zarządzaniem urządzeniami w usłudze Azure IoT:

1.  **Bliźniak urządzenia:** reprezentacja urządzenia fizycznego w usłudze IoT Hub.

2.  **Zapytania urządzeń**: umożliwiają znajdowanie bliźniaków urządzeń oraz generowanie zagregowanych informacji na temat wielu bliźniaków urządzeń. Można na przykład uruchomić zapytanie w celu wyszukania wszystkich bliźniaków urządzeń z wersją 1.0 oprogramowania układowego.

3.  **Zadania urządzeń**: akcja do wykonania na co najmniej jednym urządzeniu fizycznym, na przykład aktualizacja oprogramowania układowego, ponowne uruchomienie i reset do ustawień fabrycznych.

## Bliźniak urządzenia

Bliźniak urządzenia to reprezentacja urządzenia fizycznego w usłudze Azure IoT. Obiekt **Microsoft.Azure.Devices.Device** służy do reprezentowania bliźniaka urządzenia.

![][img-twin]

Bliźniak urządzenia zawiera następujące składniki:

1.  **Pola urządzenia:** Pola urządzenia to wstępnie zdefiniowane właściwości używane zarówno do obsługi komunikatów, jak i zarządzania urządzeniami usługi IoT Hub. Pomagają one usłudze IoT Hub w identyfikacji i nawiązywaniu połączeń z urządzeniami fizycznymi. Pola urządzenia nie są synchronizowane z urządzeniem i są przechowywane wyłącznie w bliźniaku urządzenia. Pola urządzenia obejmują identyfikator i dane uwierzytelniania urządzenia.

2.  **Właściwości urządzenia:** Właściwości urządzenia to wstępnie zdefiniowany słownik właściwości opisujący urządzenie fizyczne. Urządzenie fizyczne to wzorzec każdej właściwości urządzenia i autorytatywny magazyn każdej odpowiadającej wartości. Ostatecznie spójna reprezentacja tych właściwości jest przechowywana w bliźniaku urządzenia w chmurze. Spójność i aktualność zależą od ustawień synchronizacji opisanych w temacie [Tutorial: how to use the device twin][lnk-tutorial-twin] (Samouczek: jak używać bliźniaka urządzenia). Przykładowe właściwości urządzeń to wersja oprogramowania układowego, poziom naładowania baterii i nazwa producenta.

3.  **Właściwości usługi:** Właściwości usługi to pary **&lt;klucz,wartość&gt;** dodawane przez dewelopera do słownika właściwości usługi. Te właściwości rozszerzają model danych bliźniaka urządzenia, pozwalając na lepsze opisanie urządzenia. Właściwości usługi nie są synchronizowane z urządzeniem i są przechowywane wyłącznie w bliźniaku urządzenia w chmurze. Przykładem właściwości usługi jest właściwość **&lt;DataNastepnegoSerwisowania, 12.11.2017&gt;** umożliwiająca wyszukiwanie urządzeń według daty następnego serwisowania.

4.  **Tagi:** Tagi to podzbiór właściwości usługi, arbitralne ciągi, a nie właściwości słownika. Mogą służyć do dodawania adnotacji do bliźniaków urządzeń lub organizowania ich w grupy. Tagi nie są synchronizowane z urządzeniem i są przechowywane wyłącznie w bliźniaku urządzenia. Jeśli na przykład bliźniak urządzenia odpowiada fizycznej ciężarówce, można dodać tag dla każdego typu towaru w ciężarówce — **jabłka**, **pomarańcze** i **banany**.

## Zapytania urządzeń

W poprzedniej sekcji opisano różne składniki bliźniaka urządzenia. Teraz wyjaśnimy, jak odszukać bliźniaki urządzeń w rejestrze urządzeń IoT Hub na podstawie właściwości urządzenia, właściwości usługi lub tagów. Przykład okoliczności użycia zapytania to wyszukiwanie urządzeń wymagających aktualizacji. Można wysłać zapytanie o wszystkie urządzenia mające określoną wersję oprogramowania układowego i przekazać wynik do określonej akcji (nazywanej w usłudze IoT Hub zadaniem urządzenia, opisano je w kolejnej sekcji).

Zapytania można tworzyć przy użyciu tagów i właściwości:

-   Aby wysłać zapytanie o bliźniaki urządzeń przy użyciu tagów, należy przekazać tablicę ciągów, a zapytanie zwróci zbiór urządzeń oznaczonych wszystkimi tymi ciągami.

-   Aby wysłać zapytanie o bliźniaki urządzeń przy użyciu właściwości usługi lub urządzenia, należy użyć wyrażenia zapytania JSON. W poniższym przykładzie pokazano, jak wysłać zapytanie o wszystkie urządzenia z właściwością urządzenia o kluczu **FirmwareVersion** i wartości **1.0**. Widać, że opcja **type** (typ) właściwości ma wartość **device** (urządzenie), co wskazuje zapytanie na podstawie właściwości urządzenia, a nie właściwości usługi:

  ```
  {                           
      "filter": {                  
        "property": {                
          "name": "FirmwareVersion",   
          "type": "device"             
        },                           
        "value": "1.0",              
        "comparisonOperator": "eq",  
        "type": "comparison"         
      },                           
      "project": null,             
      "aggregate": null,           
      "sort": null                 
  }
  ```

## Zadania urządzeń

Kolejne pojęcie związane z zarządzaniem urządzeniami to zadania urządzeń umożliwiające koordynację wieloetapowych aranżacji na wielu urządzeniach.

Istnieje sześć typów zadań urządzeń udostępnianych obecnie przez zarządzanie urządzeniami usługi Azure IoT Hub (dodatkowe zadania zostaną dodane zgodnie z potrzebami klientów):

- **Aktualizacja oprogramowania układowego**: aktualizuje oprogramowanie układowe (lub obraz systemu operacyjnego) na urządzeniu fizycznym.
- **Ponowne uruchomienie**: ponowne uruchomienie urządzenia fizycznego.
- **Reset do ustawień fabrycznych**: przywraca oprogramowanie układowe (lub obraz systemu operacyjnego) urządzenia fizycznego do zapewnionego fabrycznie obrazu zapasowego zapisanego na urządzeniu.
- **Aktualizacja konfiguracji**: konfiguruje agenta klienta IoT Hub uruchomionego na urządzeniu fizycznym.
- **Odczyt właściwości urządzenia**: pobiera najnowszą wartość właściwości urządzenia na urządzeniu fizycznym.
- **Zapis właściwości urządzenia** zmienia właściwość urządzenia na urządzeniu fizycznym.

Szczegółowe informacje na temat korzystania z każdego z tych zadań zawiera [dokumentacja interfejsu API języka C\# i node.js][lnk-apidocs].

Zadanie może działać na wielu urządzeniach. Po uruchomieniu zadania tworzone jest skojarzone zadanie podrzędne dla każdego z tych urządzeń. Zadanie podrzędne działa na jednym urządzeniu. Każde zadanie podrzędne zawiera wskaźnik do swojego zadania nadrzędnego. Zadanie nadrzędne jest jedynie kontenerem dla zadań podrzędnych. Nie implementuje żadnej logiki rozróżniającej między typami urządzeń (np. aktualizacja urządzenia Intel Edison i aktualizacja urządzenia Raspberry Pi). Na poniższym diagramie przedstawiono relację między zadaniem nadrzędnym, jego elementami podrzędnymi i skojarzonymi urządzeniami fizycznymi.

![][img-jobs]

Aby sprawdzić stan uruchomionych zadań, można wysłać zapytanie o historię zadań. Pewne przykładowe zapytania można znaleźć w [naszej bibliotece zapytań][lnk-query-samples].

## Implementowanie urządzeń

Teraz, gdy omówiliśmy już pojęcia po stronie usługi, omówmy tworzenie zarządzanych urządzeń fizycznych. Biblioteka kliencka zarządzania urządzeniami usługi Azure IoT Hub umożliwia zarządzanie urządzeniami IoT przy użyciu usługi Azure IoT Hub. „Zarządzanie” obejmuje akcje takie jak ponowne uruchamianie, resetowanie do ustawień fabrycznych i aktualizacja oprogramowania układowego.  Obecnie zapewniamy niezależną od platformy bibliotekę języka C, ale w najbliższym czasie dodamy obsługę innych języków.  

Biblioteka kliencka zarządzania urządzeniami ma dwa główne obowiązki związane z zarządzaniem urządzeniami:

- Synchronizowanie właściwości urządzenia fizycznego z odpowiadającym mu bliźniakiem urządzenia w usłudze IoT Hub
- Obsługa zadań urządzenia przesłanych przez usługę IoT Hub do urządzenia

Aby dowiedzieć się więcej o tych obowiązkach i implementowaniu na urządzeniach fizycznych, zobacz [Introducing the Azure IoT Hub device management client library for C][lnk-library-c] (Wprowadzenie do biblioteki klienckiej zarządzania urządzeniami usługi Azure IoT Hub dla języka C).

## Następne kroki

W celu wdrożenia aplikacji klienckich na wielu różnych platformach sprzętowych i w różnych systemach operacyjnych można użyć zestawów SDK urządzeń IoT. Zestawy SDK urządzeń IoT zawierają biblioteki, które ułatwiają wysyłanie danych telemetrycznych do usługi IoT Hub i odbieranie poleceń wysyłanych z chmury do urządzeń. Zestawy SDK udostępniają możliwość wyboru wielu protokołów sieciowych służących do komunikowania się z usługą IoT Hub. Aby dowiedzieć się więcej, zobacz [i][lnk-device-sdks].

Aby kontynuować zapoznawanie się z funkcjami zarządzania urządzeniami usługi Azure IoT Hub, zobacz samouczek [Zarządzanie urządzeniami w usłudze Azure IoT Hub][lnk-get-started].

<!-- Images and links -->
[img-twin]: media/iot-hub-device-management-overview/image1.png
[img-jobs]: media/iot-hub-device-management-overview/image2.png
[img-client]: media/iot-hub-device-management-overview/image3.png

[lnk-lwm2m]: http://technical.openmobilealliance.org/Technical/technical-information/release-program/current-releases/oma-lightweightm2m-v1-0
[lnk-library-c]: iot-hub-device-management-library.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-tutorial-twin]: iot-hub-device-management-device-twin.md
[lnk-apidocs]: http://azure.github.io/azure-iot-sdks/
[lnk-query-samples]: https://github.com/Azure/azure-iot-sdks/blob/dmpreview/doc/get_started/dm_queries/query-samples.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks



<!--HONumber=sep16_HO1-->


