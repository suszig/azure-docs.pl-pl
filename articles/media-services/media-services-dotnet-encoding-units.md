---
title: Skalowanie przetwarzania przez dodanie jednostek kodowania - Azure media |  Dokumentacja firmy Microsoft
description: "Dowiedz się, jak sposób dodawania kodowania jednostki z platformą .NET"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;milangada;
ms.openlocfilehash: 72a8729d22a9e76c8076d7a3347619a2163e4f09
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-scale-encoding-with-net-sdk"></a>Skalowanie kodowania za pomocą zestawu .NET SDK
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Omówienie
> [!IMPORTANT]
> Upewnij się przejrzeć [omówienie](media-services-scale-media-processing-overview.md) tematu, aby uzyskać więcej informacji na temat skalowania przetwarzania tematu nośnika.
> 
> 

Aby zmienić typ jednostki zarezerwowane i liczba zastrzeżone jednostki przy użyciu zestawu .NET SDK kodowania, wykonaj następujące czynności:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a>Otwarcie biletu pomocy technicznej
Domyślnie co konto usługi Media Services można skalować do maksymalnie 25, kodowanie i 5 na żądanie jednostek zarezerwowanego przesyłania strumieniowego. Wyższy limit mogą żądać przez otwarcie biletu pomocy technicznej.

### <a name="open-a-support-ticket"></a>Otwórz bilet pomocy technicznej
Aby otworzyć obsługi biletów, wykonaj następujące czynności:

1. Kliknij przycisk [uzyskać pomoc techniczną](https://manage.windowsazure.com/?getsupport=true). Jeśli użytkownik nie jest zalogowany, pojawi się monit o podanie poświadczeń.
2. Wybierz subskrypcję.
3. Wybierz pozycję "Technical" w obszarze typu pomocy technicznej.
4. Kliknij pozycję "Utwórz bilet".
5. Wybierz "Azure Media Services" w liście produktów wyświetlone na następnej stronie.
6. Wybierz opcję "Typ problemu" odpowiednią dla tego problemu.
7. Kliknij przycisk Kontynuuj.
8. Postępuj zgodnie z instrukcjami wyświetlanymi na następnej stronie, a następnie wprowadź szczegóły problemu.
9. Kliknij przycisk Zatwierdź, aby otworzyć bilet.

## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

