<properties
 pageTitle="Omówienie usługi Azure IoT Hub | Microsoft Azure"
 description="Omówienie usługi Azure IoT Hub: czym jest centrum IoT, łączność urządzeń, wzorce komunikacji w Internecie rzeczy oraz wzorzec komunikacji korzystającej z usług"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>


# Czym jest usługa Azure IoT Hub?

Witamy w usłudze Azure IoT Hub. W tym artykule omówiono usługę Azure IoT Hub oraz opisano, dlaczego należy używać tej usługi w przypadku implementowania rozwiązania Internetu rzeczy (IoT). Azure IoT Hub to w pełni zarządzana usługa, która umożliwia bezpieczną i niezawodną dwukierunkową komunikację między milionami urządzeń IoT i zapleczem rozwiązania. Usługa Azure IoT Hub:

- zapewnia niezawodne przesyłanie komunikatów z urządzeń do chmury i z chmury do urządzeń na dużą skalę;
- umożliwia bezpieczne komunikowanie się przy użyciu poświadczeń zabezpieczeń i kontroli dostępu dla poszczególnych urządzeń;
- udostępnia rozbudowane funkcje monitorowania zdarzeń zarządzania dotyczących łączności i tożsamości urządzeń;
- zawiera biblioteki urządzeń dla najbardziej popularnych języków i platform.

W artykule [Comparison of IoT Hub and Event Hubs][lnk-compare] (Porównanie usług IoT Hub i Event Hubs) opisano podstawowe różnice między tymi dwoma usługami oraz wyszczególniono zalety korzystania z usługi IoT Hub w rozwiązaniach IoT.

![Usługa Azure IoT Hub jako brama chmury w rozwiązaniu Internetu rzeczy][img-architecture]

> [AZURE.NOTE] Szczegółowe omówienie architektury IoT można znaleźć w temacie [Architektura referencyjna IoT platformy Microsoft Azure][lnk-refarch].

## Wyzwania dotyczące łączności z urządzeniami IoT

Usługa IoT Hub i biblioteki urządzeń pomagają sprostać wyzwaniom związanym z zapewnieniem niezawodnej i bezpiecznej łączności urządzeń z zapleczem. Urządzenia IoT:

- są często systemami osadzonymi bez osoby pełniącej rolę operatora;
- mogą znajdować się w lokalizacjach zdalnych, gdzie dostęp fizyczny jest kosztowny;
- mogą być dostępne tylko za pośrednictwem zaplecza rozwiązania;
- mogą mieć ograniczone zasoby w zakresie zasilania i przetwarzania;
- mogą korzystać z przerywanej, powolnej lub kosztownej łączności sieciowej;
- mogą wymagać używania zastrzeżonych, niestandardowych lub branżowych protokołów aplikacji;
- mogą być tworzone przy użyciu szerokiej gamy popularnych platform sprzętowych i programowych.

Oprócz powyższych wymagań każde rozwiązanie IoT musi również zapewniać skalowalność, bezpieczeństwo i niezawodność. Ustalony zbiór wymogów dotyczących łączności jest trudny i czasochłonny we wdrażaniu przy użyciu tradycyjnych technologii, takich jak kontenery sieci Web i brokery obsługujące komunikaty.

## Dlaczego warto korzystać z usługi Azure IoT Hub

Usługa Azure IoT Hub wychodzi naprzeciw wyzwaniom związanym z łącznością urządzeń, zapewniając następujące korzyści:

-   **Uwierzytelnianie poszczególnych urządzeń i bezpieczna łączność**. Każde urządzenie może być aprowizowane za pomocą własnego [klucza zabezpieczeń][lnk-devguide-security] w celu umożliwienia nawiązania połączenia z usługą IoT Hub. W [rejestrze tożsamości usługi IoT Hub][lnk-devguide-identityregistry] są przechowywane tożsamości i klucze urządzeń należących do rozwiązania. Zaplecze rozwiązania może dodawać poszczególne urządzenia do listy dozwolonych i niedozwolonych urządzeń, co pozwala na pełną kontrolę nad ich dostępem.

-   **Monitorowanie operacji dotyczących łączności urządzeń**. Dostępne są szczegółowe dzienniki zawierające operacje zarządzania tożsamościami urządzeń i zdarzenia dotyczące łączności urządzeń. Ta funkcja monitorowania pozwala zidentyfikować problemy z łącznością, takie jak próby nawiązania połączenia przez urządzenia przy użyciu nieprawidłowych poświadczeń lub zbyt częste wysyłanie komunikatów albo odrzucanie wszystkich komunikatów przesyłanych z chmury do urządzenia.

-   **Obszerny zestaw bibliotek urządzeń**. Dostępne są [zestawy SDK urządzeń Azure IoT][lnk-device-sdks], które obsługują różne języki i platformy, na przykład język C w przypadku wielu dystrybucji systemu Linux, systemu Windows czy systemów operacyjnych czasu rzeczywistego. Zestawy SDK urządzeń Azure IoT obsługują także języki zarządzane, takie jak C#, Java i JavaScript.

-   **Protokoły i możliwość rozszerzania infrastruktury IoT**. Usługa IoT Hub udostępnia publiczny protokół, który umożliwia urządzeniom natywne korzystanie z protokołów MQTT 3.1.1, HTTP 1.1 lub AMQP 1.0. Jest to przydatne, jeśli w danym rozwiązaniu nie można użyć bibliotek urządzeń. Dodatkowo można rozszerzyć usługę IoT Hub pod kątem obsługi niestandardowych protokołów, wykonując następujące działania:

    - Utworzenie bramy pola przy użyciu [zestawu SDK bramy Azure IoT][lnk-gateway-sdk], która umożliwia przekształcenie niestandardowego protokołu na jeden z trzech protokołów obsługiwanych przez usługę IoT Hub. 
    - Dostosowanie [bramy protokołu Azure IoT][protocol-gateway] — składnika typu open source działającego w chmurze.

-   **Skalowalność**. Usługa Azure IoT Hub może zarządzać milionami równocześnie połączonych urządzeń i obsługiwać miliony zdarzeń na sekundę.

Te ogólne korzyści dotyczą wielu wzorców komunikacyjnych. Obecnie usługa IoT Hub umożliwia implementowanie poniższych wzorców komunikacyjnych:

-   **Oparte na zdarzeniach przyjmowanie danych z urządzeń do chmury.** Usługa IoT Hub zapewnia niezawodne odbieranie pochodzących z urządzeń milionów zdarzeń na sekundę, które mogą być następnie przetwarzane na aktywnej ścieżce za pomocą aparatu procesora zdarzeń. Odebrane zdarzenia mogą być również przechowywane na nieaktywnej ścieżce w celu przeprowadzenia analizy. Dane zdarzeń są przechowywane przez maksymalnie siedem dni, co gwarantuje niezawodne przetwarzanie i pozwala absorbować szczytowe obciążenia.

-   **Niezawodne wysyłanie komunikatów (lub *poleceń*)** z chmury do urządzeń. Za pomocą usługi IoT Hub zaplecze rozwiązania może wysyłać komunikaty do poszczególnych urządzeń z gwarancją co najmniej jednokrotnego dostarczenia. Każdy komunikat ma przypisany czas wygaśnięcia. Do zaplecza mogą być wysyłane zarówno potwierdzenia dostarczania, jak i potwierdzenia wygaśnięcia. Te potwierdzenia zapewniają pełny wgląd w cykl życia komunikatu wysyłanego z chmury do urządzenia. Dzięki temu można zaimplementować logikę biznesową obejmującą operacje, które są wykonywane na urządzeniach.

-   **Przekazywanie plików i buforowanych danych czujników do chmury.** Urządzenia mogą przekazywać pliki do usługi Azure Storage przy użyciu identyfikatorów URI SAS automatycznie zarządzanych przez usługę IoT Hub. Gdy pliki pojawią się w chmurze, usługa IoT Hub może wygenerować powiadomienia, co pozwoli zapleczu na przetworzenie plików.

## Bramy

Brama rozwiązania IoT jest zwykle [bramą protokołu][lnk-gateway] wdrożoną w chmurze lub [bramą pola][lnk-field-gateway] wdrożoną lokalnie na urządzeniach. Brama protokołu dokonuje translacji protokołu, na przykład przekształca protokół MQTT na protokół AMQP. Brama pola może uruchamiać funkcje analityczne na obrzeżu rozwiązania, podejmować szybkie decyzje w celu zmniejszenia opóźnień, udostępniać usługi zarządzania urządzeniami oraz wymuszać stosowanie ograniczeń dotyczących zabezpieczeń i prywatności. Może również dokonywać translacji protokołów. Oba typy bram pełnią rolę pośredników między urządzeniami i usługą IoT Hub.

Działanie bramy pola jest inne niż w przypadku urządzenia zapewniającego prosty routing ruchu (takiego jak zapora lub translator adresów sieciowych) — zwykle odgrywa ona aktywną rolę w zarządzaniu dostępem i przepływem informacji w rozwiązaniu.

Rozwiązanie może zawierać zarówno bramę protokołu, jak i bramę pola.

## Jak działa usługa IoT Hub?

W usłudze Azure IoT Hub zaimplementowano wzorzec [komunikacji korzystającej z usług][lnk-service-assisted-pattern], aby zapewnić obsługę interakcji między urządzeniami a zapleczem rozwiązania. Celem komunikacji korzystającej z usług jest zapewnienie zaufanych ścieżek komunikacji dwukierunkowej między system sterującym, takim jak usługa IoT Hub, a specjalnymi urządzeniami, które są wdrożone w niezaufanych lokalizacjach fizycznych. W ramach tego wzorca określono następujące zasady:

- Bezpieczeństwo jest nadrzędne w stosunku do wszystkich innych funkcji.
- Urządzenia nie akceptują niechcianej komunikacji z sieci. Urządzenie ustanawia wszystkie połączenia i trasy tylko w oparciu o reguły ruchu wychodzącego. Aby urządzenie mogło odebrać polecenie z zaplecza, musi regularnie inicjować połączenie w celu sprawdzenia, czy istnieją oczekujące polecenia do przetworzenia.
- Urządzenia powinny łączyć się tylko ze znanymi, skojarzonymi usługami, takimi jak IoT Hub.
- Ścieżka komunikacji między urządzeniem i usługą lub między urządzeniem i bramą jest zabezpieczona na poziomie warstwy protokołu aplikacji.
- Autoryzacja i uwierzytelnianie na poziomie systemu są oparte na tożsamościach poszczególnych urządzeń. Dzięki temu uprawnienia i poświadczenia dostępu mogą być niemal natychmiast odwoływane.
- W przypadku urządzeń, które łączą się sporadycznie ze względu na problemy z zasilaniem lub łącznością, komunikacja dwukierunkowa jest zapewniana przez wstrzymywanie poleceń i powiadomień do momentu, aż urządzenie będzie mogło je odebrać. Usługa IoT Hub utrzymuje kolejki poleceń wysyłanych do określonych urządzeń.
- Dane ładunku aplikacji są zabezpieczane oddzielnie i przesyłane chronionymi kanałami do określonej usługi za pośrednictwem bram.

Wzorzec komunikacji korzystającej z usług jest używany na ogromną skalę w branży urządzeń przenośnych na potrzeby implementacji usług powiadomień wypychanych, takich jak [usługa powiadomień WNS][lnk-wns], [Google Cloud Messaging][lnk-google-messaging] i [Apple Push Notification Service][lnk-apple-push].

## Następne kroki

Aby dowiedzieć się, jak usługa Azure IoT Hub umożliwia oparte na standardach zarządzanie urządzeniami IoT pozwalające na zdalne konfigurowanie i aktualizowanie urządzeń oraz zarządzanie nimi, zobacz [Omówienie zarządzania urządzeniami w usłudze Azure IoT Hub][lnk-device-management].

W celu wdrożenia aplikacji klienckich na wielu różnych platformach sprzętowych i w różnych systemach operacyjnych można użyć zestawów SDK urządzeń IoT. Zestawy SDK urządzeń IoT zawierają biblioteki, które ułatwiają wysyłanie danych telemetrycznych do usługi IoT Hub i odbieranie poleceń wysyłanych z chmury do urządzeń. Zestawy SDK udostępniają możliwość wyboru różnych protokołów sieciowych służących do komunikowania się z usługą IoT Hub. Aby dowiedzieć się więcej, zobacz [i][lnk-device-sdks].

Aby rozpocząć pisanie kodu i uruchomić kilka przykładów, zobacz samouczek [Wprowadzenie do usługi IoT Hub][lnk-get-started].

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Service Assisted Communication (Komunikacja korzystająca z usług) — wpis na blogu autorstwa Clemensa Vastersa"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md



<!--HONumber=Oct16_HO1-->


