---
title: Zarządzanie punktami końcowymi w usłudze Azure Traffic Manager | Microsoft Docs
description: W tym artykule omówiono dodawanie, usuwanie, włączanie i wyłączanie punktów końcowych w usłudze Azure Traffic Manager.
services: traffic-manager
documentationcenter: ''
author: sdwheeler
manager: carmonm
editor: tysonn

ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/17/2016
ms.author: sewhee

---
# Dodawanie, usuwanie, włączanie i wyłączanie punktów końcowych
Funkcja Web Apps w usłudze Azure App Service udostępnia już routing ruchu w trybie failover i działania okrężnego dla witryn sieci Web w centrum danych niezależnie od trybu witryny sieci Web. Korzystając z usługi Azure Traffic Manager, można określić routing ruchu w trybie failover i działania okrężnego dla witryn sieci Web i usług w chmurze w różnych centrach danych. Pierwszym krokiem niezbędnym do obsługi tej funkcjonalności jest dodanie punktu końcowego usługi w chmurze lub witryny sieci Web do usługi Traffic Manager.

> [!NOTE]
> Lokalizacji zewnętrznych lub profilów usługi Traffic Manager nie można dodać jako punktów końcowych przy użyciu klasycznego portalu Azure. Należy użyć operacji [Create Definition](http://go.microsoft.com/fwlink/p/?LinkId=400772) (Utwórz definicję) interfejsu API REST lub polecenia [Add-AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774) programu Windows PowerShell.
> 
> 

Można również wyłączyć poszczególne punkty końcowe, które są częścią profilu usługi Traffic Manager. Punkty końcowe obejmują zarówno usługi w chmurze, jak i witryny sieci Web. Wyłączenie punktu końcowego pozostawia go jako część profilu, ale profil działa tak, jakby punkt końcowy nie był w nim uwzględniony. Ta akcja jest bardzo przydatna do tymczasowego usuwania punktu końcowego, który jest w trybie konserwacji lub ponownie wdrażany. Gdy punkt końcowy zostanie ponownie uruchomiony, można go włączyć.

> [!NOTE]
> Wyłączenie punktu końcowego nie ma nic wspólnego z jego stanem wdrożenia na platformie Azure. Punkt końcowy w dobrej kondycji będzie nadal działać i może odbierać ruch nawet po wyłączeniu w usłudze Traffic Manager. Ponadto wyłączenie punktu końcowego w jednym profilu nie wpływa na jego stan w innym profilu.
> 
> 

## Aby dodać punkt końcowy usługi w chmurze lub witryny sieci Web
1. W okienku Traffic Manager w klasycznym portalu Azure znajdź profil usługi Traffic Manager zawierający ustawienia punktu końcowego, które chcesz zmodyfikować, a następnie kliknij strzałkę po prawej stronie nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
2. W górnej części strony kliknij pozycję **Punkty końcowe**, aby wyświetlić punkty końcowe, które są już częścią konfiguracji.
3. W dolnej części strony kliknij pozycję **Dodaj**, aby uzyskać dostęp do strony **Dodawanie punktów końcowych usługi**. Domyślnie na stronie wyświetlana jest lista usług w chmurze w obszarze **Punkty końcowe usługi**.
4. W przypadku usług w chmurze zaznacz na liście usługi w chmurze, aby włączyć je jako punkty końcowe dla tego profilu. Usunięcie zaznaczenia nazwy usługi w chmurze spowoduje usunięcie jej z listy punktów końcowych.
5. W przypadku witryn sieci Web kliknij listę rozwijaną **Typ usługi**, a następnie wybierz pozycję **Aplikacja sieci Web**.
6. Zaznacz witryny sieci Web na liście, aby dodać je jako punkty końcowe dla tego profilu. Usunięcie zaznaczenia nazwy witryny sieci Web spowoduje usunięcie jej z listy punktów końcowych. Pamiętaj, że dla jednego centrum danych Azure (regionu) można wybrać tylko jedną witrynę sieci Web. Jeśli wybierzesz witrynę sieci Web w centrum danych, które hostuje wiele witryn sieci Web, po wybraniu pierwszej witryny sieci Web pozostałe witryny w tym samym centrum danych staną się niedostępne do wybrania. Należy również zauważyć, że są wyświetlane tylko standardowe witryny sieci Web.
7. Po wybraniu punktów końcowych dla tego profilu, kliknij znacznik wyboru w prawej dolnej części strony, aby zapisać zmiany.

> [!NOTE]
> Jeśli używasz metody routingu ruchu dla *trybu failover*, po dodaniu lub usunięciu punktu końcowego pamiętaj o dostosowaniu listy priorytetów trybu failover, aby odzwierciedlała kolejność pracy w trybie failover dla danej konfiguracji. Aby uzyskać więcej informacji, zobacz [Konfigurowanie routingu ruchu dla trybu failover](traffic-manager-configure-failover-routing-method.md).
> 
> 

## Aby wyłączyć punkt końcowy
1. W okienku Traffic Manager w klasycznym portalu Azure znajdź profil usługi Traffic Manager zawierający ustawienia punktu końcowego, które chcesz zmodyfikować, a następnie kliknij strzałkę po prawej stronie nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
2. W górnej części strony kliknij pozycję **Punkty końcowe**, aby wyświetlić punkty końcowe uwzględnione w konfiguracji.
3. Kliknij punkt końcowy, który chcesz wyłączyć, a następnie kliknij przycisk **Wyłącz** u dołu strony.
4. Ruch przestanie przepływać do punktu końcowego, na podstawie wartości czasu wygaśnięcia (TTL, Time-to-Live) skonfigurowanego dla nazwy domeny usługi Traffic Manager. Czas TTL można zmienić na stronie konfiguracji profilu usługi Traffic Manager.

## Aby włączyć punkt końcowy
1. W okienku Traffic Manager w klasycznym portalu Azure znajdź profil usługi Traffic Manager zawierający ustawienia punktu końcowego, które chcesz zmodyfikować, a następnie kliknij strzałkę po prawej stronie nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
2. W górnej części strony kliknij pozycję **Punkty końcowe**, aby wyświetlić punkty końcowe uwzględnione w konfiguracji.
3. Kliknij punkt końcowy, który chcesz włączyć, a następnie kliknij przycisk **Włącz** u dołu strony.
4. Ruch ponownie zacznie przepływać do usługi zgodnie z ustawieniami profilu.

## Aby usunąć punkt końcowy usługi w chmurze lub witryny sieci Web
1. W okienku Traffic Manager w klasycznym portalu Azure znajdź profil usługi Traffic Manager zawierający ustawienia punktu końcowego, które chcesz zmodyfikować, a następnie kliknij strzałkę po prawej stronie nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
2. W górnej części strony kliknij pozycję **Punkty końcowe**, aby wyświetlić punkty końcowe, które są już częścią konfiguracji.
3. Na stronie Punkty końcowe kliknij nazwę punktu końcowego, który chcesz usunąć z profilu.
4. W dolnej części strony kliknij pozycję **Usuń**.

> [!NOTE]
> Nie można usunąć lokalizacji zewnętrznych lub profilów usługi Traffic Manager jako punktów końcowych przy użyciu klasycznego portalu Azure. Należy użyć programu Windows PowerShell. Aby uzyskać więcej informacji, zobacz polecenie [Remove-AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx).
> 
> 

## Następne kroki
[Konfigurowanie metody routingu dla trybu failover](traffic-manager-configure-failover-routing-method.md)

[Konfigurowanie metody routingu dla działania okrężnego](traffic-manager-configure-round-robin-routing-method.md)

[Konfigurowanie metody routingu opartego na wydajności](traffic-manager-configure-performance-routing-method.md)

[Rozwiązywanie problemów ze stanem obniżonej wydajności usługi Traffic Manager](traffic-manager-troubleshooting-degraded.md)

[Operacje w usłudze Traffic Manager (dokumentacja interfejsu API REST)](http://go.microsoft.com/fwlink/p/?LinkID=313584)

<!--HONumber=Sep16_HO3-->


