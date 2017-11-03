---
title: "Dezaktywuj i usuwanie urządzenia StorSimple | Dokumentacja firmy Microsoft"
description: "Opisuje sposób usuwania urządzenia StorSimple z usługi najpierw dezaktywowanie go, a następnie usuwając go."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 155cda38-c5ae-45dc-b7e8-6444494afc9e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c000a642aa088ac80cc7077453b87e9a47f96900
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deactivate-and-delete-a-storsimple-8000-series-device-via-storsimple-manager-service"></a>Dezaktywuj i usuwanie urządzenia StorSimple 8000 serii za pomocą usługi Menedżer StorSimple
## <a name="overview"></a>Omówienie
Możesz wykonać urządzenia StorSimple poza usługi (na przykład, jeśli są zastępowanie lub uaktualnianie urządzenia lub jeśli już używasz StorSimple). Jeśli jest to możliwe, należy dezaktywować urządzenie, aby można było go usunąć. Dezaktywowanie, serwery połączenia między urządzeniem i odpowiedniej usługi Menedżer StorSimple. W tym samouczku opisano sposób usuwania urządzenia StorSimple z usługi dezaktywowanie go, a następnie usuwając go. 

Po dezaktywacji urządzenia żadnych danych, które były przechowywane lokalnie na urządzeniu nie będzie już dostępny. Można odzyskać tylko dane skojarzone z urządzenia, które były przechowywane w chmurze.  

> [!WARNING]
> Dezaktywacja jest operacją trwałe i nie można cofnąć. Nie można zarejestrować dezaktywować urządzenie w usłudze Menedżer StorSimple, chyba że najpierw zostanie zresetowana domyślne ustawienia fabryczne. 
> 
> Fabryka zresetować proces usuwania wszystkich danych, które były przechowywane lokalnie na urządzeniu. Dlatego istotne jest utworzyć migawkę chmury wszystkich danych, aby dezaktywować urządzenie. Pozwoli to na odzyskanie wszystkich danych na późniejszym etapie.
> 
> 

Ten samouczek wyjaśnia, jak:

* Dezaktywować urządzenie i usuwanie danych
* Dezaktywować urządzenie i zachowywać dane

Wyjaśniono również sposób Dezaktywacja i usuwanie działa na urządzeniu wirtualnym StorSimple.

> [!NOTE]
> Przed dezaktywacją urządzenia fizyczne lub wirtualne StorSimple, upewnij się zatrzymać lub usunąć klientów i hosty, które są zależne od tego urządzenia.
> 
> 

## <a name="deactivate-and-delete-data"></a>Dezaktywuj i usuwanie danych
Jeśli planuje się całkowicie usuwania urządzenia i nie chcesz zachować dane na urządzeniu, następnie wykonaj następujące kroki.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Aby dezaktywować urządzenie i usuwanie danych
1. Przed dezaktywowanie urządzenia, należy usunąć wszystkie wolumin kontenerów (oraz na woluminach) skojarzone z urządzeniem. Kontenery woluminów można usunąć tylko po usunięciu skojarzonych kopii zapasowych.
2. Dezaktywować urządzenie w następujący sposób:
   
   1. W usłudze Menedżer StorSimple **urządzeń** wybierz urządzenia, które chcesz zdezaktywować i, w dolnej części strony kliknij **Dezaktywuj**.
   2. Zostanie wyświetlony komunikat potwierdzenia. Kliknij przycisk **tak** aby kontynuować. Proces Dezaktywuj uruchomi i zająć kilka minut.
3. Po dezaktywacji możesz całkowicie usunąć urządzenia. Usuwanie urządzenia spowoduje usunięcie jej z listy urządzeń połączonych z usługą. Usługę można zarządzać nie usunięto urządzenie. Aby usunąć urządzenie, wykonaj następujące kroki:
   
   1. W usłudze Menedżer StorSimple **urządzeń** wybierz dezaktywować urządzenie, które chcesz usunąć.
   2. W dolnej części strony, kliknij polecenie **usunąć**.
   3. Pojawi się monit o potwierdzenie. Kliknij przycisk **tak** aby kontynuować.
      
      Może upłynąć kilka minut, aż do usunięcia urządzenia.

## <a name="deactivate-and-retain-data"></a>Dezaktywuj i Zachowaj dane
Jeśli są zainteresowani usuwania urządzenia, ale mają być przechowywane dane, następnie wykonaj następujące kroki.

#### <a name="to-deactivate-a-device-and-retain-the-data"></a>Aby dezaktywować urządzenie i zachowywać dane
1. Dezaktywować urządzenie. Kontenery woluminów i migawek urządzenia pozostaną.
   
   1. W usłudze Menedżer StorSimple **urządzeń** wybierz urządzenia, które chcesz zdezaktywować i, w dolnej części strony kliknij **Dezaktywuj**.
   2. Zostanie wyświetlony komunikat potwierdzenia. Kliknij przycisk **tak** aby kontynuować. Proces Dezaktywuj uruchomi i zająć kilka minut.
2. Możesz teraz w trybie Failover kontenery woluminów i skojarzone migawki. Procedury, przejdź do [trybu Failover i odzyskiwania po awarii dla urządzenia StorSimple](storsimple-device-failover-disaster-recovery.md).
3. Po dezaktywacji i trybu failover można całkowicie usunąć urządzenie. Usuwanie urządzenia spowoduje usunięcie jej z listy urządzeń połączonych z usługą. Usługę można zarządzać nie usunięto urządzenie. Wykonaj poniższe kroki, aby usunąć urządzenie:
   
   1. W usłudze Menedżer StorSimple **urządzeń** wybierz dezaktywować urządzenie, które chcesz usunąć.
   2. W dolnej części strony, kliknij polecenie **usunąć**.
   3. Pojawi się monit o potwierdzenie. Kliknij przycisk **tak** aby kontynuować.
      
      Może upłynąć kilka minut, aż do usunięcia urządzenia.

## <a name="deactivate-and-delete-a-virtual-device"></a>Zdezaktywować i usunąć urządzenie wirtualne
Dla urządzenia wirtualnego StorSimple dezaktywacji cofa alokację maszyny wirtualnej. Następnie można usunąć maszyny wirtualnej i zasoby utworzone podczas inicjowania jej obsługi. Po dezaktywacji urządzenia wirtualnego nie można go przywrócić do poprzedniego stanu. 

Dezaktywacja spowoduje następujące akcje:

* Urządzenie wirtualne StorSimple jest usuwany.
* Parametr OSDisk i dyski danych utworzone dla urządzenia wirtualnego StorSimple zostały usunięte.
* Usługa hostowana i sieci wirtualnej, które zostały utworzone podczas inicjowania obsługi zostaną zachowane. Jeśli nie używasz te jednostki, należy usunąć je ręcznie.
* Utworzone przez urządzenie wirtualne StorSimple migawki w chmurze są zachowywane.

## <a name="next-steps"></a>Następne kroki
* Aby przywrócić dezaktywować urządzenie do ustawień fabrycznych, przejdź do [zresetowania urządzenia do domyślnych ustawień fabrycznych](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Aby uzyskać pomoc techniczną [skontaktuj się z Microsoft Support](storsimple-contact-microsoft-support.md).
* Aby dowiedzieć się więcej o sposobie używania usługi Menedżer StorSimple, przejdź do [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md). 

