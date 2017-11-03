---
title: "Otwieranie interfejsów API usługi Azure Mobile Engagement usługi za pomocą interfejsu API REST"
description: "Informacje dotyczące używania interfejsów API usługi Azure Mobile Engagement usługi dostępu do interfejsów API REST Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: 
ms.assetid: e8df4897-55ee-45df-b41e-ff187e3d9d12
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 4b21bca6fee7012ce1dba96950ae075eded414f8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a>Dostęp do interfejsów API usługi Azure Mobile Engagement Service za pomocą REST
Azure Mobile Engagement udostępnia [interfejsu API REST Azure Mobile Engagement](https://msdn.microsoft.com/library/azure/mt683754.aspx) do zarządzania urządzeniami, kampanie Zasięgowe lub wypchnąć itp.

> [!NOTE]
> Usługa Azure Mobile Engagement zostanie wycofana w marcu 2018 r. i jest obecnie dostępna wyłącznie dla dotychczasowych klientów. Aby uzyskać więcej informacji, zobacz [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Jeśli nie chcesz korzystać bezpośrednio z interfejsów API REST, firma Microsoft udostępnia również [plik struktury Swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) której można z narzędzia do generowania zestawów SDK preferowany język. Firma Microsoft zaleca używanie [AutoRest](https://github.com/Azure/AutoRest) narzędzie do generowania zestawu SDK z naszych plik struktury Swagger. Zestaw .NET SDK został utworzony w podobny sposób, co pozwala na interakcję z poniższych interfejsów API przy użyciu otoki C# i nie masz negocjacji token uwierzytelniania i Odśwież samodzielnie. Zobacz [przykład zestawu SDK .NET interfejsu API usługi](mobile-engagement-dotnet-sdk-service-api.md) Aby dowiedzieć się, jak używać zestawu SDK .net dla interfejsu API
