---
title: Zarządzanie profilami usługi Azure Traffic Manager | Microsoft Docs
description: Ten artykuł ułatwia tworzenie, wyłączanie, włączanie, usuwanie i wyświetlanie historii profilu usługi Azure Traffic Manager.
services: traffic-manager
documentationcenter: ''
author: sdwheeler
manager: carmonm
editor: ''

ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sewhee

---
# <a name="manage-an-azure-traffic-manager-profile"></a>Zarządzanie profilem usługi Azure Traffic Manager
Profile usługi Traffic Manager wykorzystują metody routingu ruchu do sterowania dystrybucją ruchu do usług w chmurze lub punktów końcowych witryny sieci Web. W tym artykule opisano sposób tworzenia tych profili i zarządzania nimi.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Tworzenie profilu usługi Traffic Manager przy użyciu funkcji Szybkie tworzenie
Profil usługi Traffic Manager można szybko utworzyć przy użyciu funkcji Szybkie tworzenie w klasycznym portalu Azure. Funkcja Szybkie tworzenie umożliwia tworzenie profilów z ustawieniami konfiguracji podstawowej. Funkcji Szybkie tworzenie nie można jednak użyć w przypadku ustawień takich jak zestaw punktów końcowych (usług w chmurze i witryn sieci Web), kolejność przełączania w tryb failover dla metody routingu ruchu w trybie failover lub ustawień monitorowania. Po utworzeniu profilu te ustawienia można skonfigurować w klasycznym portalu Azure. Usługa Traffic Manager obsługuje maksymalnie 200 punktów końcowych dla profilu. Większość scenariuszy użycia wymaga jednak niewielkiej liczby punktów końcowych.

### <a name="to-create-a-traffic-manager-profile"></a>Tworzenie profilu usługi Traffic Manager
1. **Wdróż usługi w chmurze i witryny sieci Web w środowisku produkcyjnym.** Aby uzyskać więcej informacji o usługach w chmurze, zobacz [Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=314074). Aby uzyskać więcej informacji o witrynach sieci Web, zobacz [Websites](http://go.microsoft.com/fwlink/p/?LinkId=393327) (Witryny sieci Web).
2. **Zaloguj się do klasycznego portalu Azure.** Kliknij przycisk **Nowy** w lewym dolnym rogu portalu, kliknij kolejno pozycje **Usługi sieciowe > Traffic Manager**, a następnie kliknij przycisk **Szybkie tworzenie**, aby rozpocząć konfigurowanie profilu.
3. **Skonfiguruj prefiks DNS.** Przypisz swojemu profilowi usługi Traffic Manager unikatowy prefiks nazwy DNS. Można określić tylko prefiks nazwy domeny usługi Traffic Manager.
4. **Wybierz subskrypcję.** Wybierz odpowiednią subskrypcję platformy Azure. Każdy profil jest skojarzony z jedną subskrypcją. Jeśli masz tylko jedną subskrypcję, ta opcja nie jest widoczna.
5. **Wybierz metodę routingu ruchu.** Wybierz metodę routingu ruchu w obszarze **Zasady routingu ruchu**. Aby uzyskać więcej informacji o metodach routingu ruchu, zobacz [Metody routingu ruchu w usłudze Traffic Manager](traffic-manager-routing-methods.md).
6. **Kliknij przycisk „Utwórz”, aby utworzyć profil**. Po zakończeniu konfiguracji profilu można go znaleźć w okienku Traffic Manager w klasycznym portalu Azure.
7. **Skonfiguruj punkty końcowe, monitorowanie i dodatkowe ustawienia w klasycznym portalu Azure.** Opcja Szybkie tworzenie umożliwia skonfigurowanie tylko ustawień podstawowych. Należy skonfigurować dodatkowe ustawienia, takie jak lista punktów końcowych i kolejność trybu failover punktów końcowych.

## <a name="disable,-enable,-or-delete-a-profile"></a>Wyłączanie, włączanie lub usuwanie profilu
Istniejący profil można wyłączyć, aby w usłudze Traffic Manager żądania użytkowników nie odwoływały się do skonfigurowanych punktów końcowych. Po wyłączeniu profilu usługi Traffic Manager profil i informacje w nim zawarte pozostają nienaruszone i można je edytować w interfejsie usługi Traffic Manager.  Działanie odwołań zostaje wznowione po ponownym włączeniu profilu. Po utworzeniu profilu usługi Traffic Manager w klasycznym portalu Azure utworzony profil zostaje automatycznie włączony. Jeśli zdecydujesz, że profil nie będzie już potrzebny, możesz go usunąć.

### <a name="to-disable-a-profile"></a>Aby wyłączyć profil
1. Jeśli używasz niestandardowej nazwy domeny, zmień rekord CNAME na internetowym serwerze DNS, tak aby nie odwoływał się on już do profilu usługi Traffic Manager.
2. Ruch przestanie być kierowany do punktów końcowych za pomocą ustawień profilu usługi Traffic Manager.
3. Wybierz profil, który ma zostać wyłączony. Na stronie usługi Traffic Manager podświetl odpowiedni profil, klikając kolumnę obok nazwy profilu. Kliknięcie nazwy profilu lub znajdującej się obok niej strzałki powoduje przejście na stronę ustawień profilu.
4. Po wybraniu profilu kliknij pozycję **Wyłącz** u dołu strony.

### <a name="to-enable-a-profile"></a>Aby włączyć profil
1. Wybierz profil, który ma zostać wyłączony. Na stronie usługi Traffic Manager podświetl odpowiedni profil, klikając kolumnę obok nazwy profilu. Kliknięcie nazwy profilu lub znajdującej się obok niej strzałki powoduje przejście na stronę ustawień profilu.
2. Po wybraniu profilu kliknij pozycję **Włącz** u dołu strony.
3. Jeśli używasz niestandardowej nazwy domeny, utwórz rekord zasobu CNAME na internetowym serwerze DNS, aby ustawić odwołanie do nazwy domeny profilu usługi Traffic Manager.
4. Ruch zostanie ponownie skierowany do punktów końcowych.

### <a name="to-delete-a-profile"></a>Aby usunąć profil
1. Upewnij się, że rekord zasobu DNS na serwerze DNS w sieci Internet nie używa już rekordu zasobu CNAME, który wskazuje nazwę domeny profilu usługi Traffic Manager.
2. Wybierz profil, który ma zostać wyłączony. Na stronie usługi Traffic Manager podświetl odpowiedni profil, klikając kolumnę obok nazwy profilu. Kliknięcie nazwy profilu lub znajdującej się obok niej strzałki powoduje przejście na stronę ustawień profilu.
3. Po wybraniu profilu kliknij pozycję **Usuń** u dołu strony.

## <a name="view-traffic-manager-profile-change-history"></a>Wyświetlanie historii zmian profilu usługi Traffic Manager
Historię zmian profilu usługi Traffic Manager można wyświetlić w klasycznym portalu Azure w usługach zarządzania.

### <a name="to-view-your-traffic-manager-change-history"></a>Aby wyświetlić historię zmian usługi Traffic Manager
1. W lewym okienku klasycznego portalu Azure kliknij przycisk **Usługi zarządzania**.
2. Na stronie Usługi zarządzania kliknij pozycję **Dzienniki operacji**.
3. Na stronie Dzienniki operacji można filtrować widok w celu wyświetlenia historii zmian dla swojego profilu usługi Traffic Manager. Po wybraniu opcji filtrowania kliknij znacznik wyboru, aby wyświetlić wyniki.
   
   * Aby wyświetlić zmiany dla wszystkich profilów, wybierz subskrypcję i zakres czasu, a następnie wybierz pozycję **Traffic Manager** z menu skrótów **Typ**.
   * Aby filtrować według nazwy profilu, wpisz nazwę profilu w polu **Nazwa usługi** lub wybierz ją z menu skrótów.
   * Aby wyświetlić szczegóły dla poszczególnych zmian, zaznacz wiersz zmiany, którą chcesz wyświetlić, a następnie kliknij przycisk **Szczegóły** u dołu strony. W oknie **Szczegóły operacji** można wyświetlić reprezentację XML obiektu interfejsu API, który został utworzony lub zaktualizowany w ramach operacji.

## <a name="next-steps"></a>Następne kroki
* [Dodawanie punktu końcowego](traffic-manager-endpoints.md)
* [Konfigurowanie metody routingu dla trybu failover](traffic-manager-configure-failover-routing-method.md)
* [Konfigurowanie metody routingu dla działania okrężnego](traffic-manager-configure-round-robin-routing-method.md)
* [Konfigurowanie metody routingu opartego na wydajności](traffic-manager-configure-performance-routing-method.md)
* [Ustawianie firmowej domeny internetowej tak, aby wskazywała nazwę domeny usługi Traffic Manager](traffic-manager-point-internet-domain.md)
* [Rozwiązywanie problemów ze stanem obniżonej wydajności usługi Traffic Manager](traffic-manager-troubleshooting-degraded.md)

<!--HONumber=Oct16_HO3-->


