---
title: "Omówienie usługi Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Opisano podstawowe pojęcia architektury w programie Azure IoT Edge, takie jak bramy, moduły i brokerów."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/04/2017
ms.author: dobett
ms.openlocfilehash: 11f6375c319c8945b0278003ee08215715f7ac42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a>Azure IoT krawędzi architektury pojęcia.

Przed sprawdzić wszelkie przykładowy kod lub utworzyć własne pole bramę, używając krawędzi IoT, zapoznaj się podstawowe pojęcia, które stanowi podstawę do architektury IoT krawędzi.

## <a name="iot-edge-modules"></a>Moduły krawędzi IoT

Tworzenie bramy Azure IoT krawędzi, tworząc i składanie *modułów krawędzi IoT*. Moduły wymieniają między sobą dane za pomocą *komunikatów*. Moduł odbiera komunikat, wykonuje na nim jakąś akcję, opcjonalnie przekształca go w nowy komunikat, a następnie publikuje go w celu przetworzenia przez inne moduły. Niektóre moduły mogą jedynie tworzyć nowe komunikaty i nigdy nie przetwarzają komunikatów przychodzących. Łańcuch modułów tworzy potok przetwarzania danych, w którym każdy moduł wykonuje przekształcenie danych w jednym punkcie tego potoku.

![Łańcuch modułów w bramie utworzonej przy użyciu usługi Azure IoT Edge][1]

Krawędź IoT zawiera następujące składniki:

* Moduły wstępnie napisane wykonać typowe funkcje bramy.
* Interfejsy, za pomocą których deweloperzy mogą pisać moduły niestandardowe.
* Infrastruktura niezbędna do wdrożenia i uruchomienia zestawu modułów.

Zestaw SDK udostępnia warstwę abstrakcji, która umożliwia tworzenie bramy do uruchamiania na różnych systemów operacyjnych i platform.

![Warstwa abstrakcji usługi Azure IoT Edge][2]

## <a name="messages"></a>Komunikaty

Wyobrażenie modułów przekazujących sobie nawzajem komunikaty ułatwia zrozumienie sposobu działania bramy, jednak nie odzwierciedla precyzyjnie tego procesu. Moduły krawędzi IoT umożliwia broker komunikują się ze sobą. Moduły publikowania komunikatów brokera (przy użyciu wzorce obsługi komunikatów, takie jak magistrala lub publikowania/subskrypcji), a następnie umożliwił brokera kierowania wiadomości z modułów dołączone do niego.

Moduł publikuje komunikaty do brokera przy użyciu funkcji **Broker_Publish**. Broker dostarcza komunikaty do modułu za pomocą funkcji wywołania zwrotnego. Komunikat składa się z zestawu właściwości klucz/wartość oraz zawartości przekazywanej w postaci bloku pamięci.

![Rola brokera w usłudze Azure IoT Edge][3]

## <a name="message-routing-and-filtering"></a>Routing i filtrowanie komunikatów

Istnieją dwa sposoby do kierowania wiadomości z poprawną modułów krawędzi IoT:

* Zestaw łącza można przekazać do brokera będzie wówczas traktował brokera źródłowy i odbiorczy dla każdego modułu.
* Moduł można filtrować według właściwości wiadomości.

Moduł powinien podejmować działania dotyczące komunikatu tylko wtedy, gdy komunikat jest przeznaczony dla niego. Łącza i efektywnie filtrowania wiadomości utworzyć potok wiadomości.

## <a name="next-steps"></a>Następne kroki

Aby wyświetlić te pojęcia stosowane w próbce można uruchomić, zobacz [architektura Eksplorowanie usługi Azure IoT krawędzi][lnk-hello-world].

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md
