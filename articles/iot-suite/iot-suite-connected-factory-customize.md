---
title: "Dostosowywanie rozwiązania połączonych fabryki - Azure | Dokumentacja firmy Microsoft"
description: "Opis sposobu dostosowywania zachowanie połączonych fabryki wstępnie skonfigurowane rozwiązanie."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2017
ms.author: dobett
ms.openlocfilehash: 09b146740413e74e3030bf3a6cb660a3cfabd239
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Dostosuj sposób rozwiązania połączonych fabryki wyświetlania danych z serwerów OPC UA

Rozwiązanie połączonych fabryki agreguje i wyświetla dane z serwerów OPC UA nawiązaniu połączenia z rozwiązaniem. Można wybrać i wysyłać polecenia do serwerów OPC UA w rozwiązaniu. Aby uzyskać więcej informacji na temat OPC UA, zobacz [Connected factory FAQ (Połączona fabryka — często zadawane pytania)](iot-suite-faq-cf.md).

Przykładami zagregowane dane w rozwiązaniu ogólną wydajność sprzętu (OEE) oraz kluczowych wskaźników wydajności (KPI) widoczne na pulpicie nawigacyjnym poziomie fabryki, wiersza i stacji. Poniższy zrzut ekranu przedstawia wartości OEE i KPI **zestawu** stacji na **wiersza produkcyjnym 1**w **we Wrocławiu** fabryki:

![Przykładowe wartości OEE i wskaźnika KPI w rozwiązaniu][img-oee-kpi]

Rozwiązanie umożliwia wyświetlenie szczegółowych informacji z elementów określonych danych z serwerów OPC UA, nazywany *stacji*. Poniższy zrzut ekranu przedstawia powierzchni liczba wyprodukowanych elementów z określonym stacji:

![Liczba elementów wyprodukowanych wykresy][img-manufactured-items]

Kliknięcie wykresy można eksplorować danych za pomocą czasu serii Insights (TSI):

![Eksploruj dane przy użyciu czasu serii Insights][img-tsi]

W tym artykule opisano:

- Jak danych ma zostać udostępnione różne widoki w rozwiązaniu.
- Jak można dostosować sposób rozwiązania wyświetla dane.

## <a name="data-sources"></a>Źródła danych

Rozwiązanie połączonych fabryki wyświetla dane z serwerów OPC UA nawiązaniu połączenia z rozwiązaniem. Domyślna instalacja obejmuje kilka serwerów OPC UA systemem symulacji fabryki. Można dodać serwery OPC UA który [łączenie się za pośrednictwem bramy] [ lnk-connect-cf] do rozwiązania.

Możesz przejść elementów danych, które połączony serwer OPC Agent użytkownika może wysłać do rozwiązania na pulpicie nawigacyjnym:

1. Przejdź do **wybierz serwer OPC UA** widoku:

    ![Przejdź do wybierz Widok serwera OPC UA][img-select-server]

1. Wybierz serwer i kliknij przycisk **Connect**. Kliknij przycisk **Kontynuuj** gdy pojawi się ostrzeżenie o zabezpieczeniach.

    > [!NOTE]
    > To ostrzeżenie występuje raz dla każdego serwera i tylko ustanawia relację zaufania między pulpit nawigacyjny rozwiązania i serwer.

1. Możesz teraz przeglądać elementów danych, które serwer może wysłać do rozwiązania. Elementy, które są wysyłane do rozwiązania mają zielony znacznik wyboru:

    ![Opublikowane elementy][img-published]

1. Jeśli jesteś *administratora* w rozwiązaniu, możesz opublikować element danych, aby była ona dostępna w rozwiązaniu fabryka połączenia. Jako Administrator możesz zmienić wartości elementów danych i wywołanie metody Server OPC UA.

## <a name="map-the-data"></a>Mapowania danych

Rozwiązanie połączonych fabryki mapuje i agreguje elementy opublikowanych danych z serwera OPC UA do różnych widoków w rozwiązaniu. Podczas obsługi administracyjnej rozwiązania rozwiązania połączonych fabryki wdraża się do konta platformy Azure. Plik JSON w rozwiązaniu Visual Studio połączone fabryki przechowuje informacje o mapowaniu. Można wyświetlać i modyfikować tego pliku konfiguracji JSON w fabryce połączonych rozwiązania Visual Studio. Po wprowadzeniu zmiany można ponownie wdrożyć rozwiązanie.

Możesz użyć pliku konfiguracji do:

- Edytowanie istniejącego fabryki symulowane, linii produkcyjnych i stacji.
- Mapowanie danych rzeczywistych serwerów OPC UA, których połączenia z rozwiązaniem.

Klonowanie kopię połączonych fabryki rozwiązania Visual Studio, użyj następującego polecenia git:

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

Plik **ContosoTopologyDescription.json** definiuje mapowanie z elementów danych serwera OPC UA widoków na pulpicie nawigacyjnym rozwiązania fabryka połączenia. Możesz znaleźć tego pliku konfiguracji w **Contoso\Topology** folderu w **aplikacji sieci Web** projektu w rozwiązaniu Visual Studio.

Zawartość pliku JSON jest zorganizowana jako hierarchię fabryki, wiersza produkcyjnym i węzły stacji. Ta hierarchia definiuje hierarchii nawigacji na pulpicie nawigacyjnym fabryka połączenia. Wartości w każdym węźle hierarchii określają informacje wyświetlane na pulpicie nawigacyjnym. Na przykład plik JSON zawiera następujące wartości dla fabryki we Wrocławiu:

```json
"Guid": "73B534AE-7C7E-4877-B826-F1C0EA339F65",
"Name": "Munich",
"Description": "Braking system",
"Location": {
    "City": "Munich",
    "Country": "Germany",
    "Latitude": 48.13641,
    "Longitude": 11.57754
},
"Image": "munich.jpg"
```

Nazwa, opis oraz lokalizację są wyświetlane w tym widoku na pulpicie nawigacyjnym:

![Dane we Wrocławiu na pulpicie nawigacyjnym][img-munich]

Każdej fabryki wiersza produkcyjnym i stacji mają właściwość obrazu. Te pliki JPEG w można znaleźć **Content\img** folderu w **aplikacji sieci Web** projektu. Te pliki obrazów wyświetlenia na pulpicie nawigacyjnym fabryka połączenia.

Każda stacja obejmuje kilka szczegółowe właściwości, które definiują mapowanie OPC UA elementów danych. Te właściwości są opisane w poniższych sekcjach:

### <a name="opcuri"></a>OpcUri

**OpcUri** OPC identyfikator URI aplikacji Agent użytkownika, który unikatowo identyfikuje serwer OPC UA jest wartość. Na przykład **OpcUri** wartość dla stacji zestawu wiersza produkcyjnym 1 w we Wrocławiu wygląda następująco: **urn: scada2194:ua:munich:productionline0:assemblystation**.

Identyfikatory URI połączonych serwerów OPC UA można wyświetlić na pulpicie nawigacyjnym rozwiązania:

![Wyświetl OPC UA serwer identyfikatorów URI][img-server-uris]

### <a name="simulation"></a>Symulacja

Informacje zawarte w **symulacji** węzeł jest specyficzne dla symulacji OPC UA uruchamiana na serwerach OPC UA, które są udostępniane domyślnie. Nie służy do rzeczywistego serwera OPC UA.

### <a name="kpi1-and-kpi2"></a>Kpi1 i Kpi2

Te węzły opisano, jak dane ze stacji przyczynia się do dwóch wartości wskaźnika KPI w pulpicie nawigacyjnym. We wdrożeniu domyślne wartości tych wskaźników KPI są jednostki na godzinę i kWh na godzinę. Rozwiązanie oblicza vales wskaźnik KPI na poziomie stacji i agregowanie ich na poziomach fabryki i wiersza produkcyjnym.

Każdy wskaźnik KPI ma minimum, maksimum i wartości docelowej. Każda wartość wskaźnika KPI można również zdefiniować akcje alertu dla rozwiązania fabryki podłączonej do wykonania. Poniższy fragment kodu przedstawia definicje kluczowy wskaźnik wydajności dla stacji zestawu wiersza produkcyjnym 1 w we Wrocławiu:

```json
"Kpi1": {
  "Minimum": 150,
  "Target": 300,
  "Maximum": 600
},
"Kpi2": {
  "Minimum": 50,
  "Target": 100,
  "Maximum": 200,
  "MinimumAlertActions": [
    {
      "Type": "None"
    }
  ]
}
```

Poniższy zrzut ekranu przedstawia dane wskaźnik KPI na pulpicie nawigacyjnym.

![Informacje dotyczące KPI na pulpicie nawigacyjnym][lnk-kpi]

### <a name="opcnodes"></a>OpcNodes

**OpcNodes** węzłów zidentyfikować elementy opublikowanych danych z serwera OPC UA i określ sposób przetwarzania danych.

**ID. węzła** wartość identyfikuje określonych OPC UA ID. węzła z serwera OPC UA. Pierwszy węzeł w stacji zestawu dla wiersza produkcyjnym 1 w we Wrocławiu ma wartość **ns = 2; i = 385**. A **ID. węzła** wartość określa, że element danych do odczytu z serwera OPC UA i **SymbolicName** zapewnia przyjazna nazwa do użycia na pulpicie nawigacyjnym dla tych danych.

W poniższej tabeli przedstawiono podsumowanie innych wartości skojarzonych z każdym węźle:

| Wartość | Opis |
| ----- | ----------- |
| Trafność  | Wskaźnik KPI i OEE wartości tych danych przyczynia się do. |
| Kod operacji     | Jak dane są agregowane. |
| Jednostki      | Jednostki do użycia na pulpicie nawigacyjnym.  |
| Widoczne    | Określa, czy wyświetlaj tej wartości na pulpicie nawigacyjnym. Niektóre wartości są używane w obliczeniach, ale nie wyświetlany.  |
| Maksymalna    | Maksymalna wartość wyzwalania alertu na pulpicie nawigacyjnym. |
| MaximumAlertActions | Akcja do wykonania w odpowiedzi na alert. Na przykład wysyłać polecenia do stacji. |
| ConstValue | Stała wartość używana w obliczeniach. |

## <a name="deploy-the-changes"></a>Wdrażanie zmiany

Po zakończeniu wprowadzania zmian do **ContosoTopologyDescription.json** pliku, należy ponownie wdrożyć rozwiązanie fabryki podłączonej do konta platformy Azure.

**Azure iot — połączony fabryka** repozytorium zawiera **build.ps1** skrypt programu PowerShell, można użyć, aby ponownie skompilować i wdrożyć rozwiązanie.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o rozwiązaniu połączonych fabryki wstępnie przeczytaj następujące artykuły:

* [Przewodnik po wstępnie skonfigurowanym rozwiązaniu połączonej fabryki][lnk-rm-walkthrough]
* [Wdrażanie bramy dla połączonych fabryki][lnk-connect-cf]
* [Uprawnienia w witrynie azureiotsuite.com][lnk-permissions]
* [Często zadawane pytania dotyczące połączonej fabryki](iot-suite-faq-cf.md)
* [FAQ][lnk-faq]


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-faq]: iot-suite-v1-faq.md