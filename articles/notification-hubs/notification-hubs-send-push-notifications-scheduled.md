---
title: "Jak wysyłać powiadomienia zaplanowane | Dokumentacja firmy Microsoft"
description: "W tym temacie opisano, w usłudze Azure Notification Hubs przy użyciu powiadomienia zaplanowane."
services: notification-hubs
documentationcenter: .net
keywords: "powiadomienia wypychane, powiadomień wypychanych, planowania powiadomień wypychanych"
author: ysxu
manager: erikre
editor: 
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: efac6e1ecc00359f1622d380333140bc055c83e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-send-scheduled-notifications"></a>Porady: Wysyłanie powiadomień według harmonogramu
## <a name="overview"></a>Omówienie
Jeśli scenariusz, w którym chcesz wysłać powiadomienie w pewnym momencie w przyszłości, ale nie mają w prosty sposób wake zapasowej swój kod zaplecza, aby wysłać powiadomienie. Centra powiadomień w warstwie standardowa obsługuje funkcja, która umożliwia planowanie powiadomienia w górę do 7 dni w przyszłości.

Podczas wysyłania powiadomienia, po prostu użyć [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) klasy w zestaw SDK usługi Notification Hubs, jak pokazano w poniższym przykładzie:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Ponadto można anulować wcześniej zaplanowanym powiadomienia za pomocą jego notificationId:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Nie ma żadnych limitów liczby zaplanowanych powiadomienia, który można wysłać.

