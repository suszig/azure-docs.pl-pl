---
title: "Azure zdarzenia rozpoczęcia delete puli partii | Dokumentacja firmy Microsoft"
description: "Odwołanie do usunięcia puli partii rozpoczęcia zdarzenia."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: f8a5241dce422e5c826ab428da6d7bc93284a1cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="pool-delete-start-event"></a>Zdarzenia rozpoczęcia delete puli

 To zdarzenie jest emitowany, gdy rozpoczęto operację usuwania puli. Usuwanie puli jest zdarzenie asynchroniczne, można oczekiwać, że zdarzenie ukończenia usuwania puli być emitowane po ukończeniu operacji usuwania.

 W poniższym przykładzie przedstawiono treści zdarzenia rozpoczęcia delete puli.

```
{
    "id": "myPool1"
}
```

|Element|Typ|Uwagi|
|-------------|----------|-----------|
|id|Ciąg|Identyfikator puli.|