<properties
   pageTitle="Zarządzanie profilami usługi Azure Traffic Manager | Microsoft Azure"
   description="Ten artykuł ułatwia tworzenie, wyłączanie, włączanie, usuwanie i wyświetlanie historii profilu usługi Azure Traffic Manager."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />


# Zarządzanie profilem usługi Azure Traffic Manager

Profil usługi Traffic Manager służy do określania usług w chmurze lub punktów końcowych witryny sieci Web, które będą monitorowane przez usługę Traffic Manager oraz tego, jakie metody routingu ruchu mają być używane do dystrybucji połączeń z tych punktów końcowych.

## Tworzenie profilu usługi Traffic Manager przy użyciu funkcji Szybkie tworzenie

Profil usługi Traffic Manager można szybko utworzyć przy użyciu funkcji Szybkie tworzenie w klasycznym portalu Azure. Funkcja Szybkie tworzenie umożliwia tworzenie profilów z ustawieniami konfiguracji podstawowej. Funkcji Szybkie tworzenie nie można jednak użyć w przypadku ustawień takich jak zestaw punktów końcowych (usług w chmurze i witryn sieci Web), kolejność przełączania w tryb failover dla metody routingu ruchu w trybie failover lub ustawień monitorowania. Po utworzeniu profilu te ustawienia można skonfigurować w klasycznym portalu Azure. Usługa Traffic Manager obsługuje maksymalnie 200 punktów końcowych dla profilu. Jednak większość scenariuszy użycia wymaga niewielkiej liczby punktów końcowych. 

### Aby utworzyć nowy profil usługi Traffic Manager

1. **Wdróż usługi w chmurze i witryny sieci Web w środowisku produkcyjnym.** Aby uzyskać więcej informacji o usługach w chmurze, zobacz [Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=314074). Aby uzyskać informacje o usługach w chmurze, zobacz [Najlepsze rozwiązania](https://msdn.microsoft.com/library/azure/5229dd1c-5a91-4869-8522-bed8597d9cf5#bkmk_TrafficManagerBestPracticesProfile). Aby uzyskać więcej informacji o witrynach sieci Web, zobacz [Websites](http://go.microsoft.com/fwlink/p/?LinkId=393327) (Witryny sieci Web).

2. **Zaloguj się do klasycznego portalu Azure.** Aby utworzyć nowy profil usługi Traffic Manager, kliknij przycisk **Nowy** w lewym dolnym rogu portalu, kliknij pozycje **Usługi sieciowe > Traffic Manager**, a następnie kliknij przycisk **Szybkie tworzenie**, aby rozpocząć konfigurowanie profilu.
3. **Skonfiguruj prefiks DNS.** Przypisz swojemu profilowi usługi Traffic Manager unikatowy prefiks nazwy DNS. Można określić tylko prefiks nazwy domeny usługi Traffic Manager.
4. **Wybierz subskrypcję.** Wybierz odpowiednią subskrypcję platformy Azure. Każdy profil jest skojarzony z jedną subskrypcją. Jeśli masz tylko jedną subskrypcję, ta opcja nie jest widoczna.
5. **Wybierz metodę routingu ruchu.** Wybierz metodę routingu ruchu w obszarze **Zasady routingu ruchu**. Aby uzyskać więcej informacji o metodach routingu ruchu, zobacz [Metody routingu ruchu w usłudze Traffic Manager](traffic-manager-routing-methods.md).
6. **Kliknij przycisk Utwórz, aby utworzyć nowy profil**. Po zakończeniu konfiguracji profilu można go znaleźć w okienku Traffic Manager w klasycznym portalu Azure.
7. **Skonfiguruj punkty końcowe, monitorowanie i dodatkowe ustawienia w klasycznym portalu Azure.** Ponieważ za pomocą funkcji Szybkie tworzenie można skonfigurować tylko podstawowe ustawienia, niezbędne jest skonfigurowanie dodatkowych ustawień, takich jak lista punktów końcowych i kolejność pracy punktów końcowych w trybie failover, aby dokończyć wymaganą konfigurację. 


## Wyłączanie, włączanie lub usuwanie profilu

Istniejący profil usługi Traffic Manager można wyłączyć, aby żądania użytkowników końcowych nie odwoływały się do skonfigurowanych dla niego punktów końcowych. Po wyłączeniu profilu usługi Traffic Manager sam profil i informacje w nim zawarte pozostaną nienaruszone i można je edytować w interfejsie usługi Traffic Manager. Jeśli chcesz ponownie włączyć profil, możesz łatwo to zrobić w klasycznym portalu Azure, co spowoduje wznowienie odwołań. Po utworzeniu profilu usługi Traffic Manager w klasycznym portalu Azure utworzony profil zostaje automatycznie włączony. Jeśli zdecydujesz, że profil nie będzie już potrzebny, możesz go usunąć.

### Aby wyłączyć profil

1. Zmodyfikuj rekord zasobu DNS na serwerze DNS w sieci Internet, aby używał odpowiedniego typu rekordu i wskaźnika do innej nazwy lub adresu IP określonej lokalizacji w Internecie. Innymi słowy należy zmienić rekord zasobu DNS na serwerze DNS w sieci Internet, tak aby nie używał już rekordu zasobu CNAME, który wskazuje nazwę domeny profilu usługi Traffic Manager.
2. Ruch przestanie być kierowany do punktów końcowych za pomocą ustawień profilu usługi Traffic Manager.
3. Wybierz profil, który ma zostać wyłączony. Aby wybrać profil, na stronie usługi Traffic Manager wyróżnij odpowiedni profil, klikając kolumnę obok nazwy profilu. Nie klikaj nazwy profilu ani strzałki obok nazwy, ponieważ spowoduje to przejście na stronę ustawień profilu.
4. Po wybraniu profilu kliknij pozycję **Wyłącz** u dołu strony.

### Aby włączyć profil

1. Wybierz profil, który chcesz włączyć. Aby wybrać profil, na stronie usługi Traffic Manager wyróżnij odpowiedni profil, klikając kolumnę obok nazwy profilu. Nie klikaj nazwy profilu ani strzałki obok nazwy, ponieważ spowoduje to przejście na stronę ustawień profilu.
2. Po wybraniu profilu kliknij pozycję **Włącz** u dołu strony.
3. Zmodyfikuj rekord zasobu DNS na internetowym serwerze DNS w celu używania typu rekordu CNAME, który mapuje nazwę domeny firmowej na nazwę domeny profilu usługi Traffic Manager. Więcej informacji zawiera artykuł [Ustawianie firmowej domeny internetowej tak, aby wskazywała domenę usługi Traffic Manager](traffic-manager-point-internet-domain.md).
4. Ruch ponownie zacznie być kierowany do punktów końcowych.

### Aby usunąć profil

1. Upewnij się, że rekord zasobu DNS na serwerze DNS w sieci Internet nie używa już rekordu zasobu CNAME, który wskazuje nazwę domeny profilu usługi Traffic Manager.
2. Wybierz profil, który chcesz usunąć. Aby wybrać profil, na stronie usługi Traffic Manager wyróżnij odpowiedni profil, klikając kolumnę obok profilu. Nie klikaj nazwy profilu ani strzałki obok nazwy, ponieważ spowoduje to przejście na stronę ustawień profilu.
4. Po wybraniu profilu kliknij pozycję **Usuń** u dołu strony.

## Wyświetlanie historii zmian profilu usługi Traffic Manager

Historię zmian profilu usługi Traffic Manager można wyświetlić w klasycznym portalu Azure w usługach zarządzania.

### Aby wyświetlić historię zmian usługi Traffic Manager

1. W lewym okienku klasycznego portalu Azure kliknij przycisk **Usługi zarządzania**.
2. Na stronie Usługi zarządzania kliknij pozycję **Dzienniki operacji**.
3. Na stronie Dzienniki operacji można filtrować widok w celu wyświetlenia historii zmian dla swojego profilu usługi Traffic Manager. Po wybraniu opcji filtrowania kliknij znacznik wyboru, aby wyświetlić wyniki.
   - Aby wyświetlić zmiany profilu dla wszystkich profilów, wybierz subskrypcję i zakres czasu, a następnie wybierz pozycję **Traffic Manager** z menu skrótów **Typ**.
   - Aby filtrować według nazwy profilu, wpisz nazwę profilu w polu **Nazwa usługi** lub wybierz ją z menu skrótów.
   - Aby wyświetlić szczegóły dla poszczególnych zmian, zaznacz wiersz zmiany, którą chcesz wyświetlić, a następnie kliknij przycisk **Szczegóły** u dołu strony. W oknie **Szczegóły operacji** można wyświetlić reprezentację XML obiektu interfejsu API, który został utworzony lub zaktualizowany w ramach operacji, i skopiować kod XML do schowka.


## Następne kroki

[Dodawanie punktu końcowego](traffic-manager-endpoints.md)

[Konfigurowanie metody routingu dla trybu failover](traffic-manager-configure-failover-routing-method.md)

[Konfigurowanie metody routingu dla działania okrężnego](traffic-manager-configure-round-robin-routing-method.md)

[Konfigurowanie metody routingu opartego na wydajności](traffic-manager-configure-performance-routing-method.md)

[Rozwiązywanie problemów ze stanem obniżonej wydajności usługi Traffic Manager](traffic-manager-troubleshooting-degraded.md)


<!--HONumber=Sep16_HO3-->


