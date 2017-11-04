---
title: "Jak dodać źródła zdarzenia Centrum zdarzeń do środowiska Azure czas serii Insights | Dokumentacja firmy Microsoft"
description: "W tym samouczku uwzględniono również sposób dodawania źródła zdarzenia podłączoną do Centrum zdarzeń w danym środowisku Insights serii czasu"
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
ms.openlocfilehash: f6a993b3858cfb94dd9795f5e55f15fa6ec7dcb2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-add-an-event-hub-event-source"></a>Jak dodać źródła zdarzenia Centrum zdarzeń

W tym samouczku uwzględniono również sposób przy użyciu portalu Azure Dodaj źródło zdarzenia, które odczytuje z Centrum zdarzeń w danym środowisku Insights serii czasu.

## <a name="prerequisites"></a>Wymagania wstępne

Utworzono Centrum zdarzeń i są zapisywania zdarzeń. Aby uzyskać więcej informacji dotyczących usługi Event Hubs, zobacz <https://azure.microsoft.com/services/event-hubs/>

> [Grupy konsumentów] Każdego źródła zdarzeń Insights serii czasu musi mieć własną dedykowanej grupy klientów, które nie są współużytkowane z innym klientom. Jeśli wielu czytników korzystanie ze zdarzeń z tej samej grupy konsumentów, wszystkich czytelników prawdopodobnie błędy. Należy pamiętać, że jest również maksymalnie 20 grup odbiorców, na Centrum zdarzeń. Aby uzyskać więcej informacji, zobacz [Event Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md).

## <a name="choose-an-import-option"></a>Wybierz opcję importu

Ustawienia dla źródła zdarzenia można wprowadzić ręcznie lub Centrum zdarzeń można wybrać z centrów zdarzeń, które są dostępne dla Ciebie.
W **opcję importu** selektora, wybierz jedną z następujących opcji:

* Podaj ustawienia Centrum zdarzeń ręcznie
* Użyj Centrum zdarzeń z dostępnych subskrypcji

### <a name="select-an-available-event-hub"></a>Wybierz dostępny Centrum zdarzeń

W poniższej tabeli opisano poszczególne opcje na karcie nowe źródło zdarzeń z jego opisem podczas wybierania dostępne Centrum zdarzeń jako źródło zdarzenia:

| NAZWA WŁAŚCIWOŚCI | OPIS |
| --- | --- |
| Nazwa źródła zdarzeń | Nazwa źródła zdarzenia. Ta nazwa musi być unikatowa w danym środowisku Insights serii czasu.
| Element źródłowy | Wybierz **Centrum zdarzeń** można utworzyć źródła zdarzeń Centrum zdarzeń.
| Identyfikator subskrypcji | Wybierz subskrypcję, w której utworzono to Centrum zdarzeń.
| Przestrzeń nazw magistrali usług | Wybierz obszar nazw usługi Service Bus, który zawiera Centrum zdarzeń.
| Nazwa Centrum zdarzeń | Wybierz nazwę Centrum zdarzeń.
| Nazwa zasad Centrum zdarzeń | Wybierz zasady dostępu współdzielonego, można utworzyć na karcie Konfigurowanie Centrum zdarzeń. Wszystkie zasady dostępu współdzielonego ma nazwę uprawnienia ustawić i klucze dostępu. Zasady dostępu współdzielonego dla źródła zdarzeń *musi* ma **odczytu** uprawnienia.
| Grupy konsumentów Centrum zdarzeń | Grupy odbiorców, które mają być odczytywane zdarzenia z Centrum zdarzeń. Zdecydowanie zaleca się używania dedykowanej grupy klientów dla źródła zdarzeń.

### <a name="provide-event-hub-settings-manually"></a>Podaj ustawienia Centrum zdarzeń ręcznie

W poniższej tabeli opisano każdej właściwości w karcie nowe źródło zdarzeń z jej opis, wprowadzając ustawienia ręcznie:

| NAZWA WŁAŚCIWOŚCI | OPIS |
| --- | --- |
| Nazwa źródła zdarzeń | Nazwa źródła zdarzenia. Ta nazwa musi być unikatowa w danym środowisku Insights serii czasu.
| Element źródłowy | Wybierz **Centrum zdarzeń** można utworzyć źródła zdarzeń Centrum zdarzeń.
| Identyfikator subskrypcji | Subskrypcja, w której utworzono to Centrum zdarzeń.
| Grupa zasobów | Subskrypcja, w której utworzono to Centrum zdarzeń.
| Przestrzeń nazw magistrali usług | Przestrzeń nazw magistrali usług to kontener dla zestawu jednostek do obsługi komunikatów. Podczas tworzenia nowego Centrum zdarzeń utworzonego również przestrzeni nazw usługi Service Bus.
| Nazwa Centrum zdarzeń | Nazwa Centrum zdarzeń. Podczas tworzenia Centrum zdarzeń należy też nadać mu nazwę
| Nazwa zasad Centrum zdarzeń | Zasady dostępu współdzielonego, można utworzyć na karcie Konfigurowanie Centrum zdarzeń. Wszystkie zasady dostępu współdzielonego ma nazwę uprawnienia ustawić i klucze dostępu. Zasady dostępu współdzielonego dla źródła zdarzeń *musi* ma **odczytu** uprawnienia.
| Klucz zasad Centrum zdarzeń | Klucz dostępu współużytkowanego używany do uwierzytelniania dostępu do przestrzeni nazw usługi Service Bus. Wpisz tutaj klucz podstawowy lub pomocniczy.
| Grupy konsumentów Centrum zdarzeń | Grupy odbiorców, które mają być odczytywane zdarzenia z Centrum zdarzeń. Zdecydowanie zaleca się używania dedykowanej grupy klientów dla źródła zdarzeń.

## <a name="next-steps"></a>Następne kroki

1. Dodaj zasady dostępu do danych w środowisku [danych Definiowanie zasad dostępu](time-series-insights-data-access.md)
1. Dostęp do środowiska w [portalu Insights serii czasu](https://insights.timeseries.azure.com)