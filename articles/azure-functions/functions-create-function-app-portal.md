---
title: "Tworzenie aplikacji funkcji przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Utwórz nową aplikację funkcji w usłudze Azure App Service z portalu."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 4f9882ebc86b8119e6e683c7442ee3448a8d111b
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Tworzenie aplikacji funkcji z portalu Azure

Aplikacje platformy Azure funkcja używa infrastruktury usługi Azure App Service. W tym temacie przedstawiono sposób tworzenia aplikacji funkcji w portalu Azure. Aplikacja funkcji jest kontener, który obsługuje wykonywanie poszczególnych funkcji. Po utworzeniu aplikacji funkcji w usłudze App Service, plan hostingu aplikacji funkcji można korzystać ze wszystkich funkcji usługi App Service.

## <a name="create-a-function-app"></a>Tworzenie aplikacji funkcji

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Podczas tworzenia aplikacji funkcji, podaj prawidłową **Nazwa aplikacji**, która może zawierać tylko litery, cyfry i łączniki. Podkreślenie (**_**) nie jest dozwolonym znakiem.

Nazwy kont usługi Magazyn muszą mieć długość od 3 do 24 znaków i mogą zawierać tylko cyfry i małe litery. Nazwa konta magazynu musi być unikatowa w obrębie platformy Azure. 

Po utworzeniu aplikacji funkcji, można utworzyć pojedynczych funkcji w różnych językach jeden lub więcej. Tworzenie funkcji [przy użyciu portalu](functions-create-first-azure-function.md#create-function), [ciągłe wdrażanie](functions-continuous-deployment.md), lub [przekazywanie z FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Plany usługi

Usługa Azure Functions oferuje dwa pakiety różnych usług: plan zużycia i plan usługi aplikacji. Plan zużycie automatycznie przydzieli moc obliczeniową, gdy kod jest uruchomiona, możliwość skalowania w poziomie jako niezbędne do obsługi obciążenia, a następnie skale — w gdy kodu nie jest uruchomiona. Plan usługi aplikacji umożliwia funkcja dostęp aplikacji do wszystkie urządzenia z usługi aplikacji. Musisz wybrać plan usługi po utworzeniu aplikacji funkcji, a obecnie nie można zmienić. Aby uzyskać więcej informacji, zobacz [wybierz usługi Azure Functions plan hostingu](functions-scale.md).

Jeśli planujesz uruchamianie funkcji JavaScript na plan usługi aplikacji, należy wybrać plan o mniejszej liczby rdzeni. Aby uzyskać więcej informacji, zobacz [JavaScript — odwołanie do funkcji](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Wymagania dotyczące konta magazynu

Podczas tworzenia aplikacji funkcji w usłudze App Service, należy utworzyć lub połączyć konto usługi Azure Storage ogólnego przeznaczenia, które obsługuje magazynu obiektów Blob, kolejki i tabeli. Wewnętrznie funkcji używa magazynu dla operacji, takich jak zarządzanie wyzwalaczy i rejestrowanie wykonania funkcji. Niektóre konta magazynu nie obsługują kolejek i tabel, takich jak konta magazynu tylko do obiektów blob, usługa Azure Premium Storage i kont magazynu ogólnego przeznaczenia z replikacją ZRS. Te konta są filtrowane z z bloku konto magazynu podczas tworzenia aplikacji funkcji.

>[!NOTE]
>Korzystając z zużycie plan hostingu, plików konfiguracji kod i powiązania funkcji są przechowywane w magazyn plików Azure w ramach konta głównego magazynu. Podczas usuwania konta magazynu głównego ta zawartość zostanie usunięta i nie może zostać odzyskany.

Aby dowiedzieć się więcej na temat typów kont magazynu, zobacz [wprowadzenie do usług magazynu Azure](../storage/common/storage-introduction.md#introducing-the-azure-storage-services). 

## <a name="next-steps"></a>Następne kroki

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



