---
title: "Dodawanie zestawu danych referencyjnych do środowiska usługi Azure Time Series Insights | Microsoft Docs"
description: "W tym samouczku omówiono dodawanie zestawu danych referencyjnych do środowiska usługi Time Series Insights"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: venkatja
ms.openlocfilehash: 574f3c5b3a889733f47d729c795ec39009f2ef4a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-ibiza-portal"></a>Tworzenie zestawu danych referencyjnych dla środowiska usługi Time Series Insights przy użyciu portalu Ibiza

Zestaw danych referencyjnych jest kolekcją elementów, które zostały rozszerzone o zdarzenia ze źródła zdarzenia użytkownika. Aparat transferu danych przychodzących usługi Time Series Insights łączy zdarzenie ze źródła zdarzenia z elementem w zestawie danych referencyjnych. To rozszerzone zdarzenie jest następnie dostępne dla zapytania. To połączenie jest oparte na kluczach zdefiniowanych w zestawie danych referencyjnych.

## <a name="steps-to-add-a-reference-data-set-to-your-environment"></a>Kroki mające na celu dodanie zestawu danych referencyjnych do środowiska

1. Zaloguj się w [portalu Ibiza](https://portal.azure.com).
2. W menu po lewej stronie w portalu Ibiza kliknij pozycję „Wszystkie zasoby”.
3. Wybierz środowisko usługi Time Series Insights.

    ![Tworzenie zestawu danych referencyjnych usługi Time Series Insights](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. Wybierz opcję „Zestawy danych referencyjnych”, kliknij przycisk „+ Dodaj”.

    ![Tworzenie zestawu danych referencyjnych usługi Time Series Insights — szczegóły](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. Określ nazwę zestawu danych referencyjnych.
6. Określ nazwę klucza i jego typ. Ta nazwa i typ są używane do pobierania odpowiedniej właściwości ze zdarzenia w źródle zdarzenia. Na przykład, jeśli jako nazwę klucza podasz „DeviceId”, a typ określisz jako „String”, aparat transferu danych przychodzących usługi Time Series Insights będzie szukać w przychodzącym zdarzeniu właściwości o nazwie „DeviceId” i typie „String”. Na potrzeby połączenia ze zdarzeniem można określić więcej niż jeden klucz. W nazwie właściwości rozróżniana jest wielkość liter.

     ![Tworzenie zestawu danych referencyjnych usługi Time Series Insights — szczegóły](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. Kliknij przycisk „Utwórz”.

## <a name="next-steps"></a>Następne kroki

* [Zarządzanie danymi referencyjnymi](time-series-insights-manage-reference-data-csharp.md) na drodze programowej.
* Aby uzyskać pełną dokumentację interfejsu API, zobacz dokument [Interfejs API danych referencyjnych](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api).