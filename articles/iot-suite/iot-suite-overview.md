---
title: "Omówienie Pakietu IoT Microsoft Azure | Microsoft Docs"
description: "Omówienie dotyczące wstępnie skonfigurowanych rozwiązań Internetu rzeczy udostępnianych w Pakiecie IoT Azure umożliwiających zbieranie, analizowanie i przechowywanie danych, przeprowadzanie wizualizacji oraz integrację z innymi systemami."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 7adde91586f5fbbffd0aeaf0efb0810cc891ac0b
ms.openlocfilehash: ecae2cb9c0cdc78226c100cd287b840b6b2a6bb8
ms.lasthandoff: 03/02/2017


---
# <a name="overview-of-azure-iot-suite"></a>Przegląd Pakietu IoT Azure
Usługi Internetu rzeczy (IoT) platformy Azure oferują szeroką gamę możliwości. Są to usługi klasy korporacyjnej, które pozwalają wykonywać następujące operacje:

* Zbieranie danych z urządzeń
* Analizowanie strumieni danych w ruchu
* Przechowywanie dużych zestawów danych i tworzenie dotyczących ich zapytań
* Wizualizowanie danych w czasie rzeczywistym i danych historycznych
* Integrowanie z systemami zaplecza biura
* Zarządzanie urządzeniami

Pakiet IoT Azure udostępnia te możliwości w postaci *wstępnie skonfigurowanych rozwiązań*, w których połączono szereg usług platformy Azure z rozszerzeniami niestandardowymi. Te wstępnie skonfigurowane rozwiązania stanowią podstawowe implementacje typowych wzorców rozwiązań IoT i pomagają skrócić czas dostarczania własnych rozwiązań IoT. Rozwiązania te można dostosować i rozszerzyć pod kątem własnych wymagań przy użyciu [zestawów SDK IoT][lnk-sdks]. Można ich także użyć jako przykładów lub szablonów podczas tworzenia nowych rozwiązań IoT.

Poniższy klip wideo zawiera wprowadzenie do Pakietu IoT Azure:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON309/player]
> 
> 

## <a name="azure-iot-services-in-azure-iot-suite"></a>Usługi Azure IoT w Pakiecie IoT Azure
Wstępnie skonfigurowane rozwiązania zwykle korzystają z następujących usług:

* Podstawowym elementem Pakietu IoT Azure jest usługa [Azure IoT Hub][lnk-iot-hub]. Umożliwia ona dwukierunkowe przesyłanie komunikatów między urządzeniami a chmurą i działa jako brama chmury i innych kluczowych usług Pakietu IoT. Usługa ta pozwala odbierać komunikaty z urządzeń na dużą skalę i wysyłać polecenia do urządzeń. Usługa ta umożliwia także [zarządzanie urządzeniami][lnk-device-management]. Na przykład możesz skonfigurować jedno lub większą liczbę urządzeń połączonych z tą usługą, przeprowadzić ich ponowny rozruch lub je zresetować do ustawień fabrycznych.
* Usługa [Azure Stream Analytics][lnk-asa] umożliwia analizowanie danych w ruchu. Pakiet IoT korzysta z niej w celu przetwarzania przychodzących danych telemetrycznych, przeprowadzania agregacji danych i wykrywania zdarzeń. Wstępnie skonfigurowane rozwiązania używają również analizy strumienia do przetwarzania komunikatów informacyjnych, które zawierają metadane lub odpowiedzi urządzeń na wysyłane polecenia. Usługa Stream Analytics przetwarza komunikaty pochodzące z urządzeń i przekazuje je do innych usług.
* Usługi [Azure Storage][lnk-azure-storage] i [Azure DocumentDB][lnk-document-db] umożliwiają przechowywanie danych. Wstępnie skonfigurowane rozwiązania korzystają z magazynu obiektów blob do przechowywania danych telemetrycznych i udostępniania ich w celu analizy. Usługa DocumentDB służy do przechowywania metadanych urządzeń, a także umożliwia zarządzanie urządzeniami w ramach rozwiązań.
* Usługi [Azure Web Apps][lnk-web-apps] i [Microsoft Power BI][lnk-power-bi] pozwalają na przeprowadzanie wizualizacji danych. Elastyczność zapewniana przez usługę Power BI umożliwia szybkie tworzenie własnych interaktywnych pulpitów nawigacyjnych korzystających z danych Pakietu IoT.

Omówienie architektury typowego rozwiązania IoT można znaleźć w artykule [Platforma Microsoft Azure i Internet rzeczy][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Wstępnie skonfigurowane rozwiązania
Pakiet IoT zawiera wstępnie skonfigurowane rozwiązania, które pozwalają na szybkie rozpoczęcie pracy i zapoznanie się z typowymi scenariuszami IoT obejmującymi na przykład *zdalne monitorowanie* i *konserwację predykcyjną*. Rozwiązania te można wdrożyć w swojej subskrypcji platformy Azure, a następnie uruchomić kompletny, uniwersalny scenariusz IoT.

## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz już pojęcie o możliwościach Pakietu IoT i jego głównych składnikach, możesz dowiedzieć się więcej o wstępnie skonfigurowanych rozwiązaniach Pakietu IoT. Aby uzyskać więcej informacji na ten temat, zobacz [Co to są wstępnie skonfigurowane rozwiązania Pakietu IoT Azure?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md

