---
title: " Przekazywanie plików na konto usługi Azure Media Services za pomocą witryny Azure Portal | Microsoft Docss"
description: "Ten samouczek przeprowadzi Cię przez kroki przekazywania plików na konto usługi Media Services za pomocą portalu Azure"
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/13/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: ed8ea30b87c8086d41cab879acce82062f08b31c
ms.openlocfilehash: f27ab42ab3c7c704804b9a5493c8b3acd954decb


---
# <a name="upload-files-into-a-media-services-account-using-the-azure-portal"></a>Przekazywanie plików na konto usługi Azure Media Services za pomocą portalu Azure
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 
> [!NOTE]
> Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 


Za pomocą usługi Media Services można przekazać pliki cyfrowe do elementu zawartości. Element zawartości może zawierać pliki wideo, audio, obrazy, kolekcje miniatur, ścieżki tekstowe i pliki napisów (oraz metadane dotyczące tych plików). Po przekazaniu plików zawartość jest bezpiecznie przechowywana w chmurze, na potrzeby dalszego przetwarzania i przesyłania strumieniowego.


## <a name="upload-files"></a>Przekazywanie plików

>[!NOTE]
>Istnieje limit maksymalnego rozmiaru pliku do przetwarzania w usłudze Media Services. Zobacz [ten](media-services-quotas-and-limitations.md) temat, aby uzyskać szczegółowe informacje na temat ograniczeń rozmiarów plików.
>

1. W witrynie [Azure Portal](https://portal.azure.com/) wybierz swoje konto usługi Azure Media Services.
2. W bloku **Ustawienia** kliknij opcję **Elementy zawartości**.
   
    ![Przekazywanie plików](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. Kliknij przycisk **Przekaż**.
   
    Zostanie wyświetlone okno **Przekaż element zawartości wideo**.
   
   > [!NOTE]
   > Nie ma żadnego limitu rozmiaru pliku.
   > 
   > 
4. Przejdź do wybranego pliku wideo na komputerze, zaznacz go, a następnie kliknij przycisk OK.  
   
    Rozpocznie się przekazywanie, a postęp będzie widoczny pod nazwą pliku.  

Po zakończeniu przekazywania na liście w oknie **Elementy zawartości** pojawi się nowy element zawartości. 

## <a name="next-steps"></a>Następne kroki
Teraz możesz zakodować przekazane elementy zawartości. Więcej informacji znajduje się na stronie [Kodowanie elementów zawartości](media-services-portal-encode.md).

Możesz także używać usługi Azure Functions do wyzwalania zadania kodowania na podstawie pliku trafiającego do skonfigurowanego kontenera. Aby uzyskać więcej informacji, zobacz [ten przykład](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!--HONumber=Feb17_HO2-->


