---
title: "Tworzenie Centrum zdarzeń platformy Azure | Dokumentacja firmy Microsoft"
description: "Tworzenie przestrzeni nazw usługi Azure Event Hubs i Centrum zdarzeń za pomocą portalu Azure"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: ff99e327-c8db-4354-9040-9c60c51a2191
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2017
ms.author: sethm
ms.openlocfilehash: e444c4505d4744c95e08c4ef0d33566356785c81
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Tworzenie przestrzeni nazw usługi Event Hubs i Centrum zdarzeń za pomocą portalu Azure

## <a name="create-an-event-hubs-namespace"></a>Tworzenie przestrzeni nazw usługi Event Hubs
1. Zaloguj się do [portalu Azure][Azure portal]i kliknij przycisk **Utwórz zasób** w lewym górnym rogu ekranu.
1. Kliknij przycisk **Internetu rzeczy**, a następnie kliknij przycisk **usługi Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)
1. W **tworzenie przestrzeni nazw**, wprowadź nazwę przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.
   
    ![](./media/event-hubs-create/create-event-hub1.png)
1. Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz warstwę cenową (Podstawowa lub Standardowa). Ponadto wybierz subskrypcję platformy Azure, grupę zasobów i lokalizację, w której chcesz utworzyć zasób. 
1. Kliknij pozycję **Utwórz**, aby utworzyć przestrzeń nazw. Może być konieczne kilkuminutowe do zapewnienia pełnej zasobów systemu.
2. W portalu listę przestrzeni nazw kliknij nowo utworzonej przestrzeni nazw.
2. Kliknij przycisk **zasady dostępu współużytkowanego**, a następnie kliknij przycisk **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

3. Kliknij przycisk kopiowania, aby skopiować parametry połączenia **RootManageSharedAccessKey** do schowka. Zapisz te parametry połączenia w tymczasowej lokalizacji, takiego jak Notatnik, do użycia w przyszłości.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Tworzenie centrum zdarzeń

1. Na liście przestrzeni nazw usługi Event Hubs kliknij nowo utworzonej przestrzeni nazw.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. W bloku przestrzeni nazw kliknij pozycję **Centra zdarzeń**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

1. Kliknij pozycję **Dodaj centrum zdarzeń** w górnej części bloku.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
1. Wpisz nazwę Centrum zdarzeń, a następnie kliknij przycisk **Utwórz**.
   
    ![](./media/event-hubs-create/create-event-hub5.png)

Utworzono Centrum zdarzeń i uzyskano parametry połączenia potrzebne do wysyłania i odbierania zdarzeń.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat usługi Event Hubs, odwiedź te linki:

* [Omówienie usługi Event Hubs](event-hubs-what-is-event-hubs.md)
* [Omówienie interfejsu API usługi Event Hubs](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/