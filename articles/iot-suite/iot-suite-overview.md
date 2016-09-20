<properties
    pageTitle="Omówienie Pakietu IoT Microsoft Azure | Microsoft Azure"
    description="Omówienie dotyczące wstępnie skonfigurowanych rozwiązań Internetu rzeczy udostępnianych w Pakiecie IoT Azure umożliwiających zbieranie, analizowanie i przechowywanie danych, przeprowadzanie wizualizacji oraz integrację z innymi systemami."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/09/2016"
     ms.author="dobett"/>

# Co to jest Pakiet IoT Azure?

Usługi Internetu rzeczy (IoT) platformy Azure oferują szeroką gamę możliwości. Są to usługi klasy korporacyjnej, które pozwalają wykonywać następujące operacje:

- Zbieranie danych z urządzeń
- Analizowanie strumieni danych w ruchu
- Przechowywanie dużych zestawów danych i tworzenie dotyczących ich zapytań
- Wizualizowanie danych w czasie rzeczywistym i danych historycznych
- Integrowanie z systemami zaplecza biura

Pakiet IoT Azure udostępnia te możliwości w postaci *wstępnie skonfigurowanych rozwiązań*, w których połączono szereg usług platformy Azure z rozszerzeniami niestandardowymi. Te wstępnie skonfigurowane rozwiązania stanowią podstawowe implementacje typowych wzorców rozwiązań IoT i pomagają skrócić czas dostarczania własnych rozwiązań IoT. Rozwiązania te można dostosować i rozszerzyć pod kątem własnych wymagań przy użyciu [zestawów SDK IoT][lnk-sdks]. Można ich także użyć jako przykładów lub szablonów podczas tworzenia nowych rozwiązań IoT.

Poniższy klip wideo zawiera wprowadzenie do Pakietu IoT Azure:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## Usługi Azure IoT w Pakiecie IoT Azure

Wstępnie skonfigurowane rozwiązania zwykle korzystają z następujących usług:

- Podstawowym elementem Pakietu IoT Azure jest usługa [Azure IoT Hub][lnk-iot-hub]. Umożliwia ona dwukierunkowe przesyłanie komunikatów między urządzeniami a chmurą i działa jako brama chmury i innych kluczowych usług Pakietu IoT. Usługa ta pozwala odbierać komunikaty z urządzeń na dużą skalę i wysyłać polecenia do urządzeń.

- Usługa [Azure Stream Analytics][lnk-asa] umożliwia analizowanie danych w ruchu. Pakiet IoT korzysta z niej w celu przetwarzania przychodzących danych telemetrycznych, przeprowadzania agregacji danych i wykrywania zdarzeń. Wstępnie skonfigurowane rozwiązania używają również analizy strumienia do przetwarzania komunikatów informacyjnych, które zawierają metadane lub odpowiedzi urządzeń na wysyłane polecenia. Usługa Stream Analytics przetwarza komunikaty pochodzące z urządzeń i przekazuje je do innych usług.

- Usługi [Azure Storage][lnk-azure-storage] i [Azure DocumentDB][lnk-document-db] umożliwiają przechowywanie danych. Wstępnie skonfigurowane rozwiązania korzystają z magazynu obiektów blob do przechowywania danych telemetrycznych i udostępniania ich w celu analizy. Usługa DocumentDB służy do przechowywania metadanych urządzeń, a także umożliwia zarządzanie urządzeniami w ramach rozwiązań.

- Usługi [Azure Web Apps][lnk-web-apps] i [Microsoft Power BI][lnk-power-bi] pozwalają na przeprowadzanie wizualizacji danych. Elastyczność zapewniana przez usługę Power BI umożliwia szybkie tworzenie własnych interaktywnych pulpitów nawigacyjnych korzystających z danych Pakietu IoT.

Omówienie architektury typowego rozwiązania IoT można znaleźć w artykule [Platforma Azure i Internet rzeczy][iot-suite-what-is-azure-iot]

## Wstępnie skonfigurowane rozwiązania

Pakiet IoT zawiera wstępnie skonfigurowane rozwiązania, które pozwalają na szybkie rozpoczęcie pracy i zapoznanie się z typowymi scenariuszami IoT (których realizację umożliwia Pakiet IoT Azure) obejmującymi na przykład *zdalne monitorowanie* i *konserwację predykcyjną*. Rozwiązania te można wdrożyć w swojej subskrypcji platformy Azure, a następnie uruchomić kompletny, uniwersalny scenariusz IoT.

## Następne kroki

Teraz, gdy masz już pojęcie o możliwościach Pakietu IoT i jego głównych składnikach, możesz dowiedzieć się więcej o wstępnie skonfigurowanych rozwiązaniach Pakietu IoT — zobacz artykuł [Co to są wstępnie skonfigurowane rozwiązania Pakietu IoT Azure?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md



<!--HONumber=sep16_HO1-->


