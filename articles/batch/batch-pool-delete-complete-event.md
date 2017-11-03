---
title: "Zdarzenie ukończenia usuwania Azure puli partii | Dokumentacja firmy Microsoft"
description: "Dokumentacja dotycząca puli partii usunąć zdarzenie ukończenia."
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
ms.openlocfilehash: 890f2ba7fda37060c56177868d6214d517d91831
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="pool-delete-complete-event"></a>Zdarzenie ukończenia usuwania puli

 To zdarzenie jest emitowany po ukończeniu operacji usuwania puli.

 W poniższym przykładzie przedstawiono treści puli zdarzenie ukończenia usuwania.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Element|Typ|Uwagi|
|-------------|----------|-----------|
|id|Ciąg|Identyfikator puli.|
|startTime|Data i godzina|Czas, usuń pulę uruchomiona.|
|endTime|Data i godzina|Czas usunąć pulę ukończone.|

## <a name="remarks"></a>Uwagi
Aby uzyskać więcej informacji na temat stanów i kody błędów dla operacji zmiany rozmiaru puli, zobacz [usunąć pulę z konta](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).