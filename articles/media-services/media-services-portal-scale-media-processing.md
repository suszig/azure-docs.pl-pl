---
title: "Skalowania przetwarzania nośnika przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Ten samouczek przedstawia kroki nośnika skalowania przetwarzania przy użyciu portalu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: 46ca29d3e66701f2abcb185791089e94761984e8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="change-the-reserved-unit-type"></a>Zmiana typu jednostki zarezerwowanej
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Omówienie

Konto usługi Media Services jest skojarzone z typem jednostki zarezerwowanej określającym szybkość, z jaką są przetwarzane zadania przetwarzania multimediów. Można wybrać jeden z następujących typów jednostki zarezerwowanej: **S1**, **S2** lub **S3**. Na przykład to samo zadanie kodowania jest wykonywane szybciej przy użyciu typu jednostki zarezerwowanej **S2** niż w przypadku użycia typu **S1**.

Oprócz określenia typu jednostki zarezerwowanej można określić aprowizację swojego konta przy użyciu **jednostek zarezerwowanych** (RU, Reserved Unit). Liczba zainicjowanych jednostek zarezerwowanych określa liczbę zadań multimedialnych, które mogą być przetwarzane jednocześnie w ramach danego konta.

>[!NOTE]
>Jednostki zarezerwowane przekształcają wszystkie operacje przetwarzania multimediów do postaci równoległej, uwzględniając zadania Indeksowania za pomocą usługi Azure Media Indexer. Jednak w przeciwieństwie do kodowania zadania indeksowania nie są przetwarzane szybciej przy użyciu szybszych jednostek zarezerwowanych.

> [!IMPORTANT]
> Upewnij się przejrzeć [omówienie](media-services-scale-media-processing-overview.md) tematu, aby uzyskać więcej informacji na temat skalowania przetwarzania tematu nośnika.
> 
> 

## <a name="scale-media-processing"></a>Skalowanie przetwarzania multimediów
Aby zmienić typ jednostki zarezerwowane i liczbę jednostek zarezerwowanego, wykonaj następujące czynności:

1. W witrynie [Azure Portal](https://portal.azure.com/) wybierz swoje konto usługi Azure Media Services.
2. W **ustawienia** wybierz **jednostki zarezerwowane multimediów**.
   
    Aby zmienić liczbę jednostek zarezerwowanego dla typu wybranej jednostki zarezerwowane, użyj **jednostki obsługiwanej nośnika** suwaka.
   
    Aby zmienić **typ jednostki zarezerwowane**, naciśnij klawisz S1, S2 lub S3.
   
    ![Strona procesorów](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. Kliknij przycisk ZAPISZ, aby zapisać zmiany.
   
    Nowe zastrzeżone jednostki są przydzielane po naciśnięciu przycisku ZAPISZ.

## <a name="next-steps"></a>Następne kroki
Przejrzyj ścieżki szkoleniowe dotyczące usługi Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

