---
title: "Jak dodać źródła zdarzenia Centrum IoT w środowisku Azure czas serii Insights | Dokumentacja firmy Microsoft"
description: "W tym samouczku uwzględniono również sposób dodawania źródła zdarzenia połączoną z Centrum IoT w danym środowisku Insights serii czasu"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: 3b98728ced81256d05b1bed2c92fc66c5ca61b98
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-add-an-iot-hub-event-source"></a>Jak dodać źródła zdarzenia Centrum IoT

W tym samouczku uwzględniono również sposób przy użyciu portalu Azure Dodaj źródło zdarzenia, które odczytuje z Centrum IoT w danym środowisku Insights serii czasu.

## <a name="prerequisites"></a>Wymagania wstępne

Utworzono Centrum IoT i są zapisywania zdarzeń. Aby uzyskać więcej informacji o centra IoT, zobacz <https://azure.microsoft.com/services/iot-hub/>

> [Grupy konsumentów] Każdego źródła zdarzeń Insights serii czasu musi mieć własną dedykowanej grupy klientów, które nie są współużytkowane z innym klientom. Jeśli wielu czytników korzystanie ze zdarzeń z tej samej grupy konsumentów, wszystkich czytelników prawdopodobnie błędy. Aby uzyskać więcej informacji, zobacz [Centrum IoT — przewodnik dewelopera](../iot-hub/iot-hub-devguide.md).

## <a name="choose-an-import-option"></a>Wybierz opcję importu

Ustawienia dla źródła zdarzenia można wprowadzić ręcznie lub Centrum IoT można wybierać z centra IoT, które są dostępne dla Ciebie.
W **opcję importu** selektora, wybierz jedną z następujących opcji:

* Podaj ustawienia Centrum IoT ręcznie
* Użyj Centrum IoT z dostępnych subskrypcji

### <a name="select-an-available-iot-hub"></a>Wybierz dostępny Centrum IoT

W poniższej tabeli opisano poszczególne opcje na karcie nowe źródło zdarzeń z jego opisem podczas wybierania dostępne Centrum IoT jako źródło zdarzenia:

| NAZWA WŁAŚCIWOŚCI | OPIS |
| --- | --- |
| Nazwa źródła zdarzeń | Nazwa źródła zdarzenia. Ta nazwa musi być unikatowa w danym środowisku Insights serii czasu.
| Element źródłowy | Wybierz **Centrum IoT** można utworzyć źródła zdarzeń Centrum IoT.
| Identyfikator subskrypcji | Wybierz subskrypcję, w której utworzono to Centrum IoT.
| Nazwa centrum IoT | Wybierz nazwę Centrum IoT.
| Nazwa zasad Centrum IoT | Wybierz zasady dostępu współdzielonego, który można znaleźć na karcie Ustawienia Centrum IoT. Wszystkie zasady dostępu współdzielonego ma nazwę uprawnienia ustawić i klucze dostępu. Zasady dostępu współdzielonego dla źródła zdarzeń *musi* ma **usługa połączyć** uprawnienia.
| Grupy odbiorców Centrum IoT | Grupy odbiorców, które mają być odczytywane zdarzenia z Centrum IoT. Zdecydowanie zaleca się używania dedykowanej grupy klientów dla źródła zdarzeń.

### <a name="provide-iot-hub-settings-manually"></a>Podaj ustawienia Centrum IoT ręcznie

W poniższej tabeli opisano każdej właściwości w karcie nowe źródło zdarzeń z jej opis, wprowadzając ustawienia ręcznie:

| NAZWA WŁAŚCIWOŚCI | OPIS |
| --- | --- |
| Nazwa źródła zdarzeń | Nazwa źródła zdarzenia. Ta nazwa musi być unikatowa w danym środowisku Insights serii czasu.
| Element źródłowy | Wybierz **Centrum IoT** można utworzyć źródła zdarzeń Centrum IoT.
| Identyfikator subskrypcji | Subskrypcja, w której utworzono to Centrum IoT.
| Grupa zasobów | Subskrypcja, w której utworzono to Centrum IoT.
| Nazwa centrum IoT | Nazwa centrum IoT. Podczas tworzenia Centrum IoT należy też nadać mu nazwę
| Nazwa zasad Centrum IoT | Zasady dostępu współdzielonego, można utworzyć na karcie Ustawienia Centrum IoT. Wszystkie zasady dostępu współdzielonego ma nazwę uprawnienia ustawić i klucze dostępu. Zasady dostępu współdzielonego dla źródła zdarzeń *musi* ma **usługa połączyć** uprawnienia.
| Klucz zasad Centrum IoT | Klucz dostępu współużytkowanego używany do uwierzytelniania dostępu do przestrzeni nazw usługi Service Bus. Wpisz tutaj klucz podstawowy lub pomocniczy.
| Grupy odbiorców Centrum IoT | Grupy odbiorców, które mają być odczytywane zdarzenia z Centrum IoT. Zdecydowanie zaleca się używania dedykowanej grupy klientów dla źródła zdarzeń.

## <a name="next-steps"></a>Następne kroki

1. Dodaj zasady dostępu do danych w środowisku [danych Definiowanie zasad dostępu](time-series-insights-data-access.md)
1. Dostęp do środowiska w [portalu Insights serii czasu](https://insights.timeseries.azure.com)