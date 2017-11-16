---
title: "Jak dodać odwołanie do zestawu danych do środowiska Azure czas serii Insights | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób dodawania zestawem danych odwołania do środowiska Azure czas serii Insights. Dane referencyjne przydaje się do dołączenia do danych źródłowych w celu dostosowania wartości."
services: time-series-insights
ms.service: time-series-insights
author: venkatgct
ms.author: venkatja
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 7cdcefbd0daec3b7bab59680afa1470624583c74
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Tworzenie zestawu danych odwołania dla danego środowiska Insights serii czasu przy użyciu portalu Azure

W tym artykule opisano sposób dodawania zestawem danych odwołania do środowiska Azure czas serii Insights. Dane referencyjne przydaje się do dołączenia do danych źródłowych w celu dostosowania wartości.

Zestaw danych referencyjnych jest kolekcją elementów, które zostały rozszerzone o zdarzenia ze źródła zdarzenia użytkownika. Aparat transferu danych przychodzących usługi Time Series Insights łączy zdarzenie ze źródła zdarzenia z elementem w zestawie danych referencyjnych. To rozszerzone zdarzenie jest następnie dostępne dla zapytania. To połączenie jest oparte na kluczach zdefiniowanych w zestawie danych referencyjnych.

## <a name="add-a-reference-data-set"></a>Dodaj odwołanie do zestawu danych

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Znajdź istniejące środowisko Insights serii czasu. Kliknij przycisk **wszystkie zasoby** w menu po lewej stronie portalu Azure. Wybierz środowisko usługi Time Series Insights.

3. W obszarze **topologia środowiska** nagłówek, wybierz **zestawów danych referencyjnych**.

    ![Tworzenie zestawu danych referencyjnych usługi Time Series Insights](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. Wybierz **+ Dodaj** Aby dodać nowy zestaw danych odwołania.

5. Określ unikatową **Nazwa zestawu danych odwołania**.

    ![Tworzenie zestawu danych referencyjnych usługi Time Series Insights — szczegóły](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

6. Określ **nazwa klucza** puste pola i wybierz jego **typu**. Tę nazwę i typ są używane do pobrania prawidłowe właściwości ze zdarzeń w źródle zdarzeń w celu przyłączenia się do danych referencyjnych. 

   Na przykład, jeśli podasz nazwę klucza jako **DeviceId** i wpisać jako **ciąg**, następnie aparat wejściowych Insights serii czasu wyszukuje właściwości o nazwie **DeviceId** typu **Ciąg** w każdego zdarzenia przychodzące do wyszukiwania Konfigurowanie i dołączanie za pomocą funkcji. Na potrzeby połączenia ze zdarzeniem można określić więcej niż jeden klucz. W nazwie klucza jest uwzględniana wielkość liter.

7. Wybierz pozycję **Utwórz**.

## <a name="next-steps"></a>Następne kroki
* [Zarządzanie danymi referencyjnymi](time-series-insights-manage-reference-data-csharp.md) na drodze programowej.
* Aby uzyskać pełną dokumentację interfejsu API, zobacz dokument [Interfejs API danych referencyjnych](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api).
