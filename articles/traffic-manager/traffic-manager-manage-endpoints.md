---
title: "Zarządzanie punktami końcowymi w usłudze Azure Traffic Manager | Microsoft Docs"
description: "W tym artykule omówiono dodawanie, usuwanie, włączanie i wyłączanie punktów końcowych w usłudze Azure Traffic Manager."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: ade2bbc2-35a7-43c5-8001-4698f7254526
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: 52f6d4f3e68e5eb120ee499827cc8549b8e547fd

---

# <a name="add-disable-enable-or-delete-endpoints"></a>Dodawanie, usuwanie, włączanie i wyłączanie punktów końcowych

Funkcja Web Apps w usłudze Azure App Service udostępnia już routing ruchu w trybie failover i działania okrężnego dla witryn sieci Web w centrum danych niezależnie od trybu witryny sieci Web. Korzystając z usługi Azure Traffic Manager, można określić routing ruchu w trybie failover i działania okrężnego dla witryn sieci Web i usług w chmurze w różnych centrach danych. Pierwszym krokiem niezbędnym do obsługi tej funkcjonalności jest dodanie punktu końcowego usługi w chmurze lub witryny sieci Web do usługi Traffic Manager.

> [!NOTE]
> W tym artykule wyjaśniono, jak korzystać z klasycznego portalu. Klasyczny portal Azure obsługuje tylko tworzenie i przypisywanie usług w chmurze i aplikacji sieci Web jako punktów końcowych. Nowy [portal Azure](https://portal.azure.com) stanowi preferowany interfejs.

Można również wyłączyć poszczególne punkty końcowe, które są częścią profilu usługi Traffic Manager. Wyłączenie punktu końcowego pozostawia go jako część profilu, ale profil działa tak, jakby punkt końcowy nie był w nim uwzględniony. Ta akcja przydaje się do tymczasowego usunięcia punktu końcowego, który jest w trybie konserwacji lub jest ponownie wdrażany. Gdy punkt końcowy zostanie ponownie uruchomiony, można go włączyć.

> [!NOTE]
> Wyłączenie punktu końcowego nie ma nic wspólnego z jego stanem wdrożenia na platformie Azure. Punkt końcowy w dobrej kondycji będzie nadal działać i będzie mógł odbierać ruch nawet po wyłączeniu w usłudze Traffic Manager. Ponadto wyłączenie punktu końcowego w jednym profilu nie wpływa na jego stan w innym profilu.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Aby dodać punkt końcowy usługi w chmurze lub witryny sieci Web

1. W okienku Traffic Manager w klasycznym portalu Azure znajdź profil usługi Traffic Manager zawierający ustawienia punktu końcowego, które chcesz zmodyfikować. Aby otworzyć stronę ustawień, kliknij strzałkę po prawej stronie nazwy profilu.
2. W górnej części strony kliknij pozycję **Punkty końcowe**, aby wyświetlić punkty końcowe, które są już częścią konfiguracji.
3. W dolnej części strony kliknij pozycję **Dodaj**, aby uzyskać dostęp do strony **Dodawanie punktów końcowych usługi**. Domyślnie na stronie wyświetlana jest lista usług w chmurze w obszarze **Punkty końcowe usługi**.
4. W przypadku usług w chmurze zaznacz na liście usługi w chmurze, aby dodać je jako punkty końcowe dla tego profilu. Usunięcie zaznaczenia nazwy usługi w chmurze spowoduje usunięcie jej z listy punktów końcowych.
5. W przypadku witryn sieci Web kliknij listę rozwijaną **Typ usługi**, a następnie wybierz pozycję **Aplikacja sieci Web**.
6. Zaznacz witryny sieci Web na liście, aby dodać je jako punkty końcowe dla tego profilu. Usunięcie zaznaczenia nazwy witryny sieci Web spowoduje usunięcie jej z listy punktów końcowych. Dla jednego centrum danych Azure (regionu) można wybrać tylko jedną witrynę sieci Web. Po wybraniu jednej witryny sieci Web zostaje utracona możliwość wybrania innych witryn sieci Web w tym samym centrum danych. Należy również zauważyć, że są wyświetlane tylko standardowe witryny sieci Web.
7. Po wybraniu punktów końcowych dla tego profilu, kliknij znacznik wyboru w prawej dolnej części strony, aby zapisać zmiany.

> [!NOTE]
> Po dodaniu lub usunięciu punktu końcowego z profilu za pomocą metody routingu ruchu dla trybu *failover* lista priorytetów pracy w trybie failover może nie być uporządkowana w sposób pożądany. Kolejność na liście priorytetów pracy w trybie failover można dostosować na stronie konfiguracji. Aby uzyskać więcej informacji, zobacz [Konfigurowanie routingu ruchu dla trybu failover](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Aby wyłączyć punkt końcowy

1. W okienku Traffic Manager w klasycznym portalu Azure znajdź profil usługi Traffic Manager zawierający ustawienia punktu końcowego, które chcesz zmodyfikować. Aby otworzyć stronę ustawień, kliknij strzałkę po prawej stronie nazwy profilu.
2. W górnej części strony kliknij pozycję **Punkty końcowe**, aby wyświetlić punkty końcowe uwzględnione w konfiguracji.
3. Kliknij punkt końcowy, który chcesz wyłączyć, a następnie kliknij przycisk **Wyłącz** u dołu strony.
4. Klienty w dalszym ciągu będą wysyłać ruch do punktu końcowego na czas wygaśnięcia (TTL). Czas wygaśnięcia można zmienić na stronie konfiguracji profilu usługi Traffic Manager.

## <a name="to-enable-an-endpoint"></a>Aby włączyć punkt końcowy

1. W okienku Traffic Manager w klasycznym portalu Azure znajdź profil usługi Traffic Manager zawierający ustawienia punktu końcowego, które chcesz zmodyfikować. Aby otworzyć stronę ustawień, kliknij strzałkę po prawej stronie nazwy profilu.
2. W górnej części strony kliknij pozycję **Punkty końcowe**, aby wyświetlić punkty końcowe uwzględnione w konfiguracji.
3. Kliknij punkt końcowy, który chcesz włączyć, a następnie kliknij przycisk **Włącz** u dołu strony.
4. Klienty są kierowane do włączonego punktu końcowego zgodnie z ustawieniami profilu.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Aby usunąć punkt końcowy usługi w chmurze lub witryny sieci Web

1. W okienku Traffic Manager w klasycznym portalu Azure znajdź profil usługi Traffic Manager zawierający ustawienia punktu końcowego, które chcesz zmodyfikować. Aby otworzyć stronę ustawień, kliknij strzałkę po prawej stronie nazwy profilu.
2. W górnej części strony kliknij pozycję **Punkty końcowe**, aby wyświetlić punkty końcowe, które są już częścią konfiguracji.
3. Na stronie Punkty końcowe kliknij nazwę punktu końcowego, który chcesz usunąć z profilu.
4. W dolnej części strony kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

* [Zarządzanie profilami usługi Traffic Manager](traffic-manager-manage-profiles.md)
* [Konfigurowanie metod routingu](traffic-manager-configure-routing-method.md)
* [Rozwiązywanie problemów ze stanem obniżonej wydajności usługi Traffic Manager](traffic-manager-troubleshooting-degraded.md)
* [Zagadnienia dotyczące wydajności usługi Traffic Manager](traffic-manager-performance-considerations.md)
* [Operacje w usłudze Traffic Manager (dokumentacja interfejsu API REST)](http://go.microsoft.com/fwlink/p/?LinkID=313584)




<!--HONumber=Nov16_HO5-->


