---
title: "Usługi Azure event rozpoczęcia zadania wsadowego | Dokumentacja firmy Microsoft"
description: "Odwołanie do zadania wsadowego rozpoczęcia zdarzenia."
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
ms.openlocfilehash: c47ab36c99dddd46a14c15018a2a46bf7f873ffa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="task-start-event"></a>Zdarzenia uruchamiania zadań

 To zdarzenie jest emitowany, gdy zadanie zostało zaplanowane do uruchomienia w węźle obliczeń przez harmonogram. Należy pamiętać, że jeśli zadanie zostanie ponowiona lub umieszczony w kolejce to zdarzenie będzie obliczanie ponownie do tego samego zadania, ale liczby ponownych prób i zadań w systemie zostanie odpowiednio aktualizowany.


 W poniższym przykładzie przedstawiono treść zadania rozpoczęcia zdarzenia.

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "retryCount": 0
    }
}
```

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|Identyfikator zadania|Ciąg|Identyfikator zadania zawierającego zadanie.|
|id|Ciąg|Identyfikator zadania.|
|taskType|Ciąg|Typ zadania. Może to być "JobManager" i wskazujący, że jest to zadanie Menedżer zadania lub "User" i wskazujący, że nie jest zadanie Menedżer zadania.|
|systemTaskVersion|Int32|Jest to licznik ponownych prób wewnętrzny dla zadania. Wewnętrznie usługa partia zadań. Spróbuj ponownie zadania konta dla przejściowych problemów. Te problemy mogą zawierać błędy wewnętrzne planowania lub próbuje odzyskać z węzłami w złym stanie przetwarzania.|
|[nodeInfo](#nodeInfo)|Typ złożony|Zawiera informacje o węźle obliczeń, na którym uruchomiono zadanie.|
|[multiInstanceSettings](#multiInstanceSettings)|Typ złożony|Określa, że zadanie jest wiele wystąpień zadań wymagających wielu węzłów obliczeniowych.  Zobacz [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) szczegółowe informacje.|
|[ograniczenia](#constraints)|Typ złożony|Ograniczenia wykonanie, które są stosowane do tego zadania.|
|[executionInfo](#executionInfo)|Typ złożony|Zawiera informacje dotyczące wykonywania tego zadania.|

###  <a name="nodeInfo"></a>nodeInfo

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|poolId|Ciąg|Identyfikator puli, na którym uruchomiono zadanie.|
|ID. węzła|Ciąg|Identyfikator węzła, na którym uruchomiono zadanie.|

###  <a name="multiInstanceSettings"></a>multiInstanceSettings

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|numberOfInstances|int|Liczba węzłów obliczeń wymagana przez zadanie.|

###  <a name="constraints"></a>ograniczenia

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Maksymalna liczba powtórzeń zadania mogą być ponowiona. Usługa partia zadań ponawia zadanie, jeśli jego kod zakończenia jest różna od zera.<br /><br /> Należy pamiętać, że ta wartość określa, w szczególności liczby ponownych prób. Usługa partia zadań ponowi zadania raz i może następnie ponów próbę wykonania tego limitu. Na przykład jeśli maksymalna liczba ponowień prób partii zadanie 3 do 4 godziny (jedna próba początkowej i 3 ponowne próby).<br /><br /> Jeśli maksymalna liczba ponowień to 0, usługa partia zadań nie ponów próbę wykonania zadania.<br /><br /> Jeśli maksymalna liczba ponowień to -1, usługa partia zadań ponawia próbę zadania bez ograniczeń.<br /><br /> Wartość domyślna to 0 (brak ponownych prób).|

###  <a name="executionInfo"></a>executionInfo

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|retryCount|Int32|Ile razy zadanie było ponawiane przez usługi partia zadań. Zadanie jest podjęta, jeśli kończy działanie z kodem zakończenia różną od zera, do określonego MaxTaskRetryCount|
