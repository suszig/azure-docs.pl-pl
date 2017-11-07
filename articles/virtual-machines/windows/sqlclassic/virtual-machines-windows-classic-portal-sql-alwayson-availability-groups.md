---
title: "Konfigurowanie zawsze włączonej grupy dostępności w maszynach wirtualnych platformy Azure (klasyczne) | Dokumentacja firmy Microsoft"
description: "Utwórz zawsze włączonej grupy dostępności o maszynach wirtualnych platformy Azure. W tym samouczku używana głównie interfejsu użytkownika i narzędzia zamiast skryptów."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 8d48a3d2-79f8-4ab0-9327-2f30ee0b2ff1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: b360fe9f28eeb9b10c82fce729165b1b572ac3c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-always-on-availability-group-in-azure-virtual-machines-classic"></a>Konfigurowanie zawsze włączonej grupy dostępności w maszynach wirtualnych platformy Azure (klasyczne)
> [!div class="op_single_selector"]
> * [Klasycznym: interfejsu użytkownika](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klasycznym: środowiska PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Przed rozpoczęciem należy wziąć pod uwagę może teraz ukończyć tego zadania w modelu usługi Azure Resource Manager. Firma Microsoft zaleca modelu usługi Azure Resource Manager dla nowych wdrożeń. Zobacz [programu SQL Server zawsze włączonych grup dostępności na maszynach wirtualnych Azure](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT] 
> Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager. Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../../../azure-resource-manager/resource-manager-deployment-model.md). W tym artykule opisano sposób użycia klasycznym modelu wdrażania. 

Aby wykonać to zadanie z modelem usługi Azure Resource Manager, zobacz [programu SQL Server zawsze włączonych grup dostępności na maszynach wirtualnych Azure](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

W tym samouczku end-to-end przedstawiono sposób wdrażania grup dostępności za pomocą programu SQL Server zawsze na uruchomione na maszynach wirtualnych platformy Azure.

Na koniec samouczka rozwiązania programu SQL Server zawsze na platformie Azure będzie składać się z następujących elementów:

* Sieć wirtualna, która zawiera wiele podsieci i frontonu i podsieci wewnętrznej bazy danych
* Kontroler domeny z domeny usługi Active Directory (Azure AD)
* Dwie maszyny wirtualne, które programem SQL Server wdrożony do podsieci wewnętrznej bazy danych i są przyłączone do domeny usługi Azure AD
* Klaster trybu failover węzła trzech modelu kworum Większość węzłów
* Grupy dostępności, która ma dwa zatwierdzania synchronicznego repliki bazy danych dostępności

Na rysunku poniżej przedstawiono graficzną reprezentację rozwiązania.

![Architektura laboratorium testowego dla grup dostępności w systemie Azure](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Należy pamiętać, że jest jedną z możliwych konfiguracji. Na przykład można zminimalizować liczbę maszyn wirtualnych do grupy dostępności dwie repliki. Ta konfiguracja ogranicza godziny obliczeń na platformie Azure przy użyciu kontrolera domeny jako monitor udziału plików kworum w klastrze z dwoma węzłami. Ta metoda zmniejsza liczbę maszyn wirtualnych przez jeden z ilustrowane konfiguracji.

Ten samouczek zakłada następujące czynności:

* Masz już konto platformy Azure.
* Wiesz już, jak użyć graficznego interfejsu użytkownika w galerii maszyn wirtualnych w celu udostępnienia klasyczne maszyny wirtualnej z programem SQL Server.
* Masz już pełny opis zawsze włączonych grup dostępności. Aby uzyskać więcej informacji, zobacz [zawsze włączonych grup dostępności (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Jeśli interesuje Cię przy użyciu zawsze włączonych grup dostępności z programu SharePoint, zobacz też [konfigurowania programu SQL Server 2012 grup dostępności Always On programu SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).
> 
> 

## <a name="create-the-virtual-network-and-domain-controller-server"></a>Utwórz serwer wirtualny Kontroler sieci i domeny
Rozpocznij nowe konto wersji próbnej platformy Azure. Po skonfigurowaniu konta powinno być ekranu głównego z klasycznego portalu Azure.

1. Kliknij przycisk **nowy** przycisku w lewym rogu dolnej części strony, jak pokazano na poniższym zrzucie ekranu.
   
    ![Kliknij przycisk Nowa, w portalu](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)
2. Kliknij przycisk **usług sieciowych** > **sieci wirtualnej** > **Utwórz niestandardowy**, jak pokazano na poniższym zrzucie ekranu.
   
    ![Utwórz sieć wirtualną](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)
3. W **Utwórz sieć WIRTUALNĄ** okna dialogowego Utwórz nową sieć wirtualną krokowe wykonywanie strony i z ustawieniami w poniższej tabeli. 
   
   | Strona | Ustawienia |
   | --- | --- |
   | Szczegóły sieci wirtualnej |**Nazwa = ContosoNET**<br/>**REGION = zachodnie stany USA** |
   | Serwery DNS i połączenie z siecią VPN |Brak |
   | Przestrzeniami adresów sieci wirtualnej |Ustawienia są przedstawione na poniższym zrzucie ekranu: ![Utwórz sieć wirtualną](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png) |
4. Utwórz maszynę wirtualną, która będzie używana jako kontroler domeny (DC). Kliknij przycisk **nowy** > **obliczeniowe** > **maszyny wirtualnej** > **z galerii**, jak pokazano na poniższym zrzucie ekranu.
   
    ![Tworzenie maszyny wirtualnej](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)
5. W **Utwórz MASZYNĘ WIRTUALNĄ A** okna dialogowego Skonfiguruj nową maszynę wirtualną krokowe wykonywanie strony i z ustawieniami w poniższej tabeli. 
   
   | Strona | Ustawienia |
   | --- | --- |
   | Wybierz system operacyjny maszyny wirtualnej |Windows Server 2012 R2 Datacenter |
   | Konfiguracja maszyny wirtualnej |**Data wydania wersji** = (Najnowsza wersja)<br/>**Nazwa maszyny WIRTUALNEJ** = ContosoDC<br/>**WARSTWA** = STANDARD<br/>**ROZMIAR** = A2 (2 rdzenie)<br/>**NOWĄ nazwę użytkownika** = AzureAdmin<br/>**NOWE hasło** = Contoso! 000<br/>**POTWIERDŹ** = Contoso! 000 |
   | Konfiguracja maszyny wirtualnej |**USŁUGI w CHMURZE** = Tworzenie nowej usługi w chmurze<br/>**Nazwa DNS usługi w CHMURZE** = nazwa usługi w chmurze unikatowy<br/>**Nazwa DNS** unikatową nazwę = (przykład: ContosoDC123)<br/>**REGION/grupy KOLIGACJI i WIRTUALNEJ sieci** = ContosoNET<br/>**PODSIECI sieci WIRTUALNEJ** = Back(10.10.2.0/24)<br/>**Konto MAGAZYNU** = Użyj konta usługi storage automatycznie generowanych<br/>**ZESTAW dostępności** = (Brak) |
   | Opcje maszyny wirtualnej |Użyj wartości domyślnych |

Po skonfigurowaniu nowej maszyny wirtualnej, poczekaj na maszynie wirtualnej może być provsioned. Ten proces trwa trochę czasu. Jeśli klikniesz przycisk **maszyny wirtualnej** kartę w klasycznym portalu Azure, można zobaczyć ContosoDC stanów następuje z **uruchamianie (inicjowania obsługi administracyjnej)** do **zatrzymane**, **uruchamianie**, **uruchomiona (inicjowania obsługi administracyjnej)**i w końcu **systemem**.

Teraz pomyślnie obsługiwanej przez serwer kontrolera domeny. Następnie skonfiguruj domeny usługi Active Directory na tym serwerze kontrolera domeny.

## <a name="configure-the-domain-controller"></a>Konfiguracja kontrolera domeny
W poniższych krokach maszyny ContosoDC są konfigurowane jako kontroler domeny corp.contoso.com.

1. W portalu, wybierz **ContosoDC** maszyny. Na **pulpitu nawigacyjnego** , kliknij pozycję **Connect** do otwarcia pulpitu zdalnego (RDP) dostęp do pulpitu zdalnego.
   
    ![Połączyć się z komputerem Vritual](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)
2. Zaloguj się przy użyciu konta administratora skonfigurowane (**\AzureAdmin**) i hasło (**Contoso! 000**).
3. Domyślnie **Menedżera serwera** powinien zostać wyświetlony pulpit nawigacyjny.
4. Kliknij przycisk **Dodaj role i funkcje** łącza na pulpicie nawigacyjnym.
   
    ![W Eksploratorze serwera Dodawanie ról](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)
5. Kliknij przycisk **dalej** aż do **ról serwera** sekcji.
6. Wybierz **usług domenowych w usłudze Active Directory** i **serwera DNS** ról. Po wyświetleniu monitu, Dodaj więcej funkcji, które wymagają tych ról.
   
   > [!NOTE]
   > Zostanie wyświetlone ostrzeżenie, że nie istnieje żaden adres IP weryfikacji. Jeśli testujesz konfigurację, kliknij przycisk **Kontynuuj**. W scenariuszach produkcji [ustawić statyczny adres IP komputera kontrolera domeny przy użyciu programu PowerShell](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   > 
   > 
   
    ![Dodawanie ról okna dialogowego](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)
7. Kliknij przycisk **dalej** aż **potwierdzenie** sekcji. Wybierz **automatycznie ponownie uruchom serwer docelowy, jeśli jest to wymagane** pole wyboru.
8. Kliknij pozycję **Zainstaluj**.
9. Po zainstalowaniu funkcji, wróć do **Menedżera serwera** pulpitu nawigacyjnego.
10. Wybierz nową **usług AD DS** opcji w okienku po lewej stronie.
11. Kliknij przycisk **więcej** łącze na żółty pasek ostrzeżenie.
    
     ![Usługi AD DS okno dialogowe na maszynie wirtualnej serwera DNS](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)
12. W **akcji** kolumny **wszystkie szczegóły zadania serwera** okno dialogowe, kliknij przycisk **podwyższyć ten serwer do poziomu kontrolera domeny**.
13. W **Kreator konfiguracji usług domenowych Active Directory**, użyj następujących wartości:
    
    | Strona | Ustawienie |
    | --- | --- |
    | Konfiguracja wdrażania |**Dodaj nowy las** = wybrane<br/>**Nazwa domeny głównej** = corp.contoso.com |
    | Opcje kontrolera domeny |**Hasło** = Contoso! 000<br/>**Potwierdź hasło** = Contoso! 000 |
14. Kliknij przycisk **dalej** przechodzić przez innych stron w kreatorze. Na **Sprawdzanie wymagań wstępnych** upewnij się, że zostanie wyświetlony następujący komunikat: **sprawdzanie wszystkich wymagań wstępnych zakończyło się pomyślnie**. Należy pamiętać, że należy przejrzeć wszystkie odpowiednie komunikaty ostrzegawcze, ale można kontynuować instalację.
15. Kliknij pozycję **Zainstaluj**. **ContosoDC** maszyny wirtualnej zostanie automatycznie ponownie uruchomiony.

## <a name="configure-domain-accounts"></a>Konfigurowanie kont domeny
Następne kroki konfigurowania kont usługi Active Directory do późniejszego użycia.

1. Zaloguj się ponownie do **ContosoDC** maszyny.
2. W **Menedżera serwera**, kliknij przycisk **narzędzia** > **Centrum administracyjne usługi Active Directory**.
   
    ![Centrum administracyjne usługi Active Directory](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)
3. W **Centrum administracyjne usługi Active Directory**, wybierz pozycję **corp (lokalna)** w okienku po lewej stronie.
4. Po prawej stronie **zadania** okienku, kliknij przycisk **nowy** > **użytkownika**. Użyj następujących ustawień:
   
   | Ustawienie | Wartość |
   | --- | --- |
   | **Imię** |Instalowanie |
   | **SamAccountName użytkownika** |Instalowanie |
   | **Hasło** |Contoso! 000 |
   | **Potwierdź hasło** |Contoso! 000 |
   | **Inne opcje haseł** |Wybrano |
   | **Hasło nigdy nie wygasa** |Zaznaczone |
5. Kliknij przycisk **OK** utworzyć **zainstalować** użytkownika. To konto będzie używane do konfigurowania klastra trybu failover i grupy dostępności.
6. Tworzenie dwóch dodatkowych użytkowników, **CORP\SQLSvc1** i **CORP\SQLSvc2**, ze te same czynności. Te konta będą używane dla wystąpień programu SQL Server. Następnie należy zapewnić **CORP\Install** wystarczających uprawnień, aby skonfigurować klaster pracy awaryjnej systemu Windows.
7. W **Centrum administracyjne usługi Active Directory**, kliknij przycisk **corp (lokalna)** w okienku po lewej stronie. W **zadania** okienku, kliknij przycisk **właściwości**.
   
    ![Właściwości użytkownika CORP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)
8. Wybierz **rozszerzenia**, a następnie kliknij przycisk **zaawansowane** znajdującego się na **zabezpieczeń** kartę.
9. W **Zaawansowane ustawienia zabezpieczeń dla corp** okno dialogowe, kliknij przycisk **Dodaj**.
10. Kliknij przycisk **Wybierz podmiot zabezpieczeń**, wyszukaj **CORP\Install**, a następnie kliknij przycisk **OK**.
11. Wybierz **odczytywanie wszystkich właściwości** i **Tworzenie obiektów komputerów** uprawnienia.
    
     ![Uprawnienia użytkownika Corp](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)
12. Kliknij przycisk **OK**, a następnie kliknij przycisk **OK** ponownie. Zamknij okno właściwości corp.

Teraz, gdy skonfigurowano usługi Active Directory i obiektów użytkowników, możesz utworzyć trzy maszyny wirtualnej programu SQL Server i dołącz je do tej domeny.

## <a name="create-the-sql-server-virtual-machines"></a>Tworzenie maszyn wirtualnych programu SQL Server
Utwórz trzy maszyny wirtualne. Jeden jest węzłem klastra i są dwa dla programu SQL Server. Aby utworzyć poszczególnych maszyn wirtualnych, wróć do klasycznego portalu Azure, kliknij przycisk **nowy** > **obliczeniowe** > **maszyny wirtualnej** > **z galerii**. Następnie należy użyć szablonów w poniższej tabeli ułatwiają tworzenie maszyn wirtualnych.

| Strona | VM1 | MASZYNY VM2 | VM3 |
| --- | --- | --- | --- |
| Wybierz system operacyjny maszyny wirtualnej |**Windows Server 2012 R2 Datacenter** |**SQL Server 2014 Enterprise RTM** |**SQL Server 2014 Enterprise RTM** |
| Konfiguracja maszyny wirtualnej |**Data wydania wersji** = (Najnowsza wersja)<br/>**Nazwa maszyny WIRTUALNEJ** = ContosoWSFCNode<br/>**WARSTWA** = STANDARD<br/>**ROZMIAR** = A2 (2 rdzenie)<br/>**NOWĄ nazwę użytkownika** = AzureAdmin<br/>**NOWE hasło** = Contoso! 000<br/>**POTWIERDŹ** = Contoso! 000 |**Data wydania wersji** = (Najnowsza wersja)<br/>**Nazwa maszyny WIRTUALNEJ** = ContosoSQL1<br/>**WARSTWA** = STANDARD<br/>**ROZMIAR** = A3 (4 rdzenie)<br/>**NOWĄ nazwę użytkownika** = AzureAdmin<br/>**NOWE hasło** = Contoso! 000<br/>**POTWIERDŹ** = Contoso! 000 |**Data wydania wersji** = (Najnowsza wersja)<br/>**Nazwa maszyny WIRTUALNEJ** = ContosoSQL2<br/>**WARSTWA** = STANDARD<br/>**ROZMIAR** = A3 (4 rdzenie)<br/>**NOWĄ nazwę użytkownika** = AzureAdmin<br/>**NOWE hasło** = Contoso! 000<br/>**POTWIERDŹ** = Contoso! 000 |
| Konfiguracja maszyny wirtualnej |**USŁUGI w CHMURZE** = nazwa DNS usługi utworzonej wcześniej unikatowe chmury (przykład: ContosoDC123)<br/>**REGION/grupy KOLIGACJI i WIRTUALNEJ sieci** = ContosoNET<br/>**PODSIECI sieci WIRTUALNEJ** = Back(10.10.2.0/24)<br/>**Konto MAGAZYNU** = Użyj konta usługi storage automatycznie generowanych<br/>**ZESTAWU dostępności** = Tworzenie dostępności ustawić<br/>**NAZWA ZBIORU DOSTĘPNOŚCI** = SQLHADR |**USŁUGI w CHMURZE** = nazwa DNS usługi utworzonej wcześniej unikatowe chmury (przykład: ContosoDC123)<br/>**REGION/grupy KOLIGACJI i WIRTUALNEJ sieci** = ContosoNET<br/>**PODSIECI sieci WIRTUALNEJ** = Back(10.10.2.0/24)<br/>**Konto MAGAZYNU** = Użyj konta usługi storage automatycznie generowanych<br/>**ZESTAWU dostępności** = SQLHADR (można również skonfigurować dostępność ustawić po utworzeniu maszyny. Wszystkie trzy maszyny powinien być przypisany do zestawu dostępności SQLHADR.) |**USŁUGI w CHMURZE** = nazwa DNS usługi utworzonej wcześniej unikatowe chmury (przykład: ContosoDC123)<br/>**REGION/grupy KOLIGACJI i WIRTUALNEJ sieci** = ContosoNET<br/>**PODSIECI sieci WIRTUALNEJ** = Back(10.10.2.0/24)<br/>**Konto MAGAZYNU** = Użyj konta usługi storage automatycznie generowanych<br/>**ZESTAWU dostępności** = SQLHADR (można również skonfigurować dostępność ustawić po utworzeniu maszyny. Wszystkie trzy maszyny powinien być przypisany do zestawu dostępności SQLHADR.) |
| Opcje maszyny wirtualnej |Użyj wartości domyślnych |Użyj wartości domyślnych |Użyj wartości domyślnych |

<br/>

> [!NOTE]
> Poprzednią konfigurację sugeruje maszyn wirtualnych w warstwie standardowa, ponieważ maszyny warstwy BASIC nie obsługuje punktów końcowych z równoważeniem obciążenia. Należy równoważeniem obciążenia punkty końcowe później, aby utworzyć odbiornik grupy dostępności. Ponadto rozmiary maszyny, które są zalecane w tym miejscu są przeznaczone do testowania grup dostępności w maszynach wirtualnych platformy Azure. Aby uzyskać najlepszą wydajność w przypadku obciążeń produkcyjnych, zobacz zalecenia dotyczące programu SQL Server rozmiary i konfiguracji [wydajności najlepsze rozwiązania dotyczące programu SQL Server w usłudze Azure Virtual Machines](../sql/virtual-machines-windows-sql-performance.md).
> 
> 

Po trzy maszyny wirtualne są w pełni zaaprowizowanym, należy połączyć je **corp.contoso.com** domeny i przyznać prawa administracyjne CORP\Install maszyn. Aby to zrobić, użyj następujących kroków dla każdego z trzech maszyn wirtualnych.

1. Najpierw należy zmienić adresu preferowanego serwera DNS. Pobierz plik RDP każdej maszyny wirtualnej do katalogu lokalnego, zaznaczając maszyny wirtualnej na liście i klikając przycisk **Connect** przycisku. Aby wybrać maszynę wirtualną, kliknij w dowolnym miejscu oprócz pierwszej komórki w wierszu, jak pokazano na poniższym zrzucie ekranu.
   
    ![Pobierz plik RDP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)
2. Otwórz plik RDP, który pobrany i zaloguj się do maszyny wirtualnej przy użyciu konta administratora skonfigurowane (**BUILTIN\AzureAdmin**) i hasło (**Contoso! 000**).
3. Po zalogowaniu, powinien zostać wyświetlony **Menedżera serwera** pulpitu nawigacyjnego. Kliknij przycisk **lokalnego serwera** w okienku po lewej stronie.
4. Kliknij przycisk **adres IPv4 przypisany przez serwer DHCP IPv6 włączone** łącza.
5. W **połączenia sieciowe** okna dialogowego kliknij ikonę sieci.
   
    ![Zmień maszyny wirtualnej, preferowany serwer DNS](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)
6. Na pasku poleceń, kliknij przycisk **Zmień ustawienia tego połączenia**. (W zależności od rozmiaru okna, może być konieczne kliknij Podwójna strzałka w prawo, aby wyświetlić to polecenie).
7. Wybierz **protokół internetowy w wersji 4 (TCP/IPv4)**, a następnie kliknij przycisk **właściwości**.
8. Wybierz **Użyj następujących adresów serwerów DNS** , a następnie określ **10.10.2.4** w **preferowany serwer DNS**.
9. **10.10.2.4** adres jest adresem, który jest przypisany do maszyny wirtualnej w podsieci 10.10.2.0/24 w sieci wirtualnej platformy Azure. Czy maszyna wirtualna jest **ContosoDC**. Aby sprawdzić **ContosoDC**na adres IP, użyj **nslookup contosodc** w oknie wiersza polecenia, jak pokazano na poniższym zrzucie ekranu.
   
    ![Użyj polecenia NSLOOKUP, aby znaleźć adres IP dla kontrolera domeny](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)
10. Kliknij przycisk **OK** > **Zamknij** aby zatwierdzić zmiany. Teraz można dołączyć maszyny wirtualnej do **corp.contoso.com**.
11. W **lokalnego serwera** okna, kliknij przycisk **grupy roboczej** łącza.
12. W **nazwy komputera** kliknij **zmiany**.
13. Wybierz **domeny** pole wyboru, typ **corp.contoso.com** w polu tekstowym, a następnie kliknij przycisk **OK**.
14. W **zabezpieczenia systemu Windows** okna dialogowego wprowadź poświadczenia dla konta administratora domeny domyślnej (**CORP\AzureAdmin**) i hasło (**Contoso! 000**).
15. Gdy zostanie wyświetlony komunikat "Zapraszamy do domeny corp.contoso.com", kliknij przycisk **OK**.
16. Kliknij przycisk **Zamknij** > **Uruchom ponownie teraz** w oknie dialogowym.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-virtual-machine"></a>Dodaj użytkownika Corp\Install jako administrator na każdej maszynie wirtualnej
1. Poczekaj, aż do ponownego uruchamiania maszyny wirtualnej, a następnie otwórz protokołu RDP pliku ponownie logować się do maszyny wirtualnej za pomocą **BUILTIN\AzureAdmin** konta.
2. W **Menedżera serwera** kliknij **narzędzia** > **Zarządzanie komputerem**.
   
    ![Zarządzanie komputerem](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)
3. W **Zarządzanie komputerem** okna dialogowego rozwiń **lokalnych użytkowników i grup**, a następnie kliknij przycisk **grup**.
4. Kliknij dwukrotnie **Administratorzy** grupy.
5. W **właściwości Administratorzy** okno dialogowe, kliknij przycisk **Dodaj** przycisku.
6. Wprowadź nazwę danego użytkownika **CORP\Install**, a następnie kliknij przycisk **OK**. Po wyświetleniu monitu o poświadczenia, użyj **AzureAdmin** konta **Contoso! 000** hasła.
7. Kliknij przycisk **OK** zamknąć **właściwości administratora** okno dialogowe.

### <a name="add-the-failover-clustering-feature-to-each-virtual-machine"></a>Dodaj funkcję Klaster trybu Failover do każdej maszyny wirtualnej
1. W **Menedżera serwera** pulpitu nawigacyjnego, kliknij przycisk **Dodaj role i funkcje**.
2. W **Kreatora dodawania ról i funkcji**, kliknij przycisk **dalej** aż do **funkcje** strony.
3. Wybierz **klaster pracy awaryjnej**. Po wyświetleniu monitu, Dodaj inne funkcje zależne.
   
    ![Dodaj funkcję Klaster trybu Failover do maszyny wirtualnej](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)
4. Kliknij przycisk **dalej**, a następnie kliknij przycisk **zainstalować** na **potwierdzenie** strony.
5. Gdy **klaster pracy awaryjnej** instalacja funkcji została zakończona, kliknij przycisk **Zamknij**.
6. Wyloguj się z maszyny wirtualnej.
7. Powtórz kroki opisane w tej sekcji dla wszystkich trzech serwerów: **ContosoWSFCNode**, **ContosoSQL1**, i **ContosoSQL2**.

SQL Server, który teraz przydzielania maszyny wirtualnej i uruchomiona, ale każdy ma domyślne opcje dla programu SQL Server.

## <a name="create-the-failover-cluster"></a>Tworzenie klastra trybu failover
W tej sekcji utworzysz klaster trybu failover, który będzie hostem grupy dostępności, która zostanie utworzona później. Przez teraz należy wykonać następujące polecenie, aby do każdego z trzech maszyn wirtualnych, które będą używane w klastrze trybu failover:

* W pełni zaaprowizowanym maszyny wirtualnej platformy Azure
* Maszyna wirtualna należy do domeny
* Dodaje **CORP\Install** do lokalnej grupy administratorów
* Dodano obsługi klastrów pracy awaryjnej

Wszystkie są wymagania wstępne na każdej maszynie wirtualnej przed dołączeniem jej do klastra trybu failover.

Należy również zauważyć, że sieć wirtualna platformy Azure nie zachowują się w taki sam sposób jak sieci lokalnej. Należy utworzyć klaster w następującej kolejności:

1. Utwórz w jednym węźle klastra z jednym węzłem (**ContosoSQL1**).
2. Zmodyfikuj adres IP klastra nieużywane adresu IP (**10.10.2.101**).
3. Przenieś nazwy klastra w tryb online.
4. Dodawanie innych węzłów (**ContosoSQL2** i **ContosoWSFCNode**).

Wykonaj następujące kroki, aby wykonać zadania, które pełnej konfiguracji klastra.

1. Otwórz plik RDP dla **ContosoSQL1**i zaloguj się przy użyciu konta domeny **CORP\Install**.
2. W **Menedżera serwera** pulpitu nawigacyjnego, kliknij przycisk **narzędzia** > **Menedżera klastra trybu Failover**.
3. W lewym okienku kliknij prawym przyciskiem myszy **Menedżera klastra trybu Failover**, a następnie kliknij przycisk **utworzyć klaster**, jak pokazano na poniższym zrzucie ekranu.
   
    ![Tworzenie klastra](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)
4. W kreatorze tworzenia klastra Tworzenie klastra z jednym węzłem krokowe wykonywanie strony i z ustawieniami w poniższej tabeli:
   
   | Strona | Ustawienia |
   | --- | --- |
   | Przed rozpoczęciem |Użyj wartości domyślnych |
   | Wybierz serwery |Typ **ContosoSQL1** w **wprowadź nazwę serwera** i kliknij przycisk **Dodaj** |
   | Ostrzeżenie dotyczące sprawdzania poprawności |Wybierz **nr nie jest wymagana obsługa firmy Microsoft dla tego klastra i z tego powodu nie chcesz uruchomić testy weryfikacyjne. Po kliknięciu przycisku dalej kontynuować tworzenie klastra**. |
   | Punkt dostępu do administrowania klastrem |Typ **klaster_1** w **nazwa klastra** |
   | Potwierdzenie |Użyj wartości domyślnych, chyba że w przypadku korzystania z funkcji miejsca do magazynowania. Zapoznaj się z ostrzeżeniem poniżej tej tabeli. |
   
   > [!WARNING]
   > Jeśli używasz [miejsca do magazynowania](https://technet.microsoft.com/library/hh831739), który grupuje wiele dysków w pule magazynu, należy wyczyścić **Dodaj wszystkie odpowiednie magazyny do klastra** pole wyboru na **potwierdzenie** strony. Jeśli ta opcja nie zostanie wyczyszczone, dyski wirtualne będzie można odłączyć podczas procesu klastrowania. W związku z tym również nie pojawią się w Menedżerze dysków lub w Eksploratorze dopóki miejsca do magazynowania nie zostaną usunięte z klastra i ponownie nałożona za pomocą programu PowerShell.
   > 
   > 
5. W okienku po lewej stronie rozwiń **Menedżera klastra trybu Failover**, a następnie kliknij przycisk **Cluster1.corp.contoso.com**.
6. W środkowym okienku, przewiń w dół do **zasoby podstawowe klastra** , a następnie rozwiń węzeł **Name: Clutser1** szczegóły. Powinny pojawić się zarówno **nazwa** i **adres IP** zasobów w **niepowodzenie** stanu. Nie można przełączyć zasobu adresu IP do trybu online, ponieważ klastra jest przypisany ten sam adres IP komputera, który jest zduplikowany adres.
7. Kliknij prawym przyciskiem myszy nieudane **adres IP** zasobów, a następnie kliknij przycisk **właściwości**.
   
    ![Właściwości klastra](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)
8. Wybierz **statyczny adres IP**, określ **10.10.2.101** w **adres** polu tekstowym, a następnie kliknij przycisk **OK**.
9. W **zasoby podstawowe klastra** sekcji, kliknij prawym przyciskiem myszy **Name: Klaster1**, a następnie kliknij przycisk **przejdź do trybu Online**. Zaczekaj, aż oba zasoby są w trybie online. Gdy zasób Nazwa klastra do trybu online, z serwerem kontrolera jest aktualizowana o nowe konto komputera usługi Active Directory. To konto usługi Active Directory będzie służyć do uruchamiania usługi w klastrze grupy dostępności później.
10. Dodaj pozostałe węzły do klastra. W drzewie przeglądarki, kliknij prawym przyciskiem myszy **Cluster1.corp.contoso.com**, a następnie kliknij przycisk **Dodaj węzeł**, jak pokazano na poniższym zrzucie ekranu.
    
     ![Dodaj węzeł do klastra](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)
11. W **Kreatora dodawania węzłów**, kliknij przycisk **dalej** na **Wybieranie serwerów** Dodaj **ContosoSQL2** i **ContosoWSFCNode** do listy, wpisując nazwę serwera w **wprowadź nazwę serwera** , a następnie klikając polecenie **Dodaj**. Gdy wszystko będzie gotowe, kliknij przycisk **dalej**.
12. Na **ostrzeżenie dotyczące sprawdzania poprawności** kliknij przycisk **nr**, mimo że w środowisku produkcyjnym, należy wykonać testy sprawdzania poprawności. Następnie kliknij przycisk **Dalej**.
13. Na **potwierdzenie** kliknij przycisk **dalej** można dodać węzłów.
    
    > [!WARNING]
    > Jeśli używasz [miejsca do magazynowania](https://technet.microsoft.com/library/hh831739), który grupuje wiele dysków w pule magazynu, należy wyczyścić **Dodaj wszystkie odpowiednie magazyny do klastra** pole wyboru. Jeśli ta opcja nie zostanie wyczyszczone, dyski wirtualne będzie można odłączyć podczas procesu klastrowania. W związku z tym również nie będą widoczne w Menedżera dysków lub Explorer dopóki miejsca do magazynowania są usuwane z klastra i ponownie nałożona przy użyciu programu PowerShell.
    > 
    > 
14. Po dodaniu węzły do klastra, kliknij przycisk **Zakończ**. Czy klaster ma trzy węzły i wyświetlić je w Menedżerze klastra trybu failover powinna zostać wyświetlona **węzłów** kontenera.
15. Wyloguj się z sesji usług pulpitu zdalnego.

## <a name="prepare-the-sql-server-instances-for-availability-groups"></a>Przygotowanie wystąpień programu SQL Server do grupy dostępności
W tej sekcji będzie wykonywać następujące czynności na obu **ContosoSQL1** i **contosoSQL2**:

* Dodaj dane logowania dla **NT AUTHORITY\System** z niezbędne uprawnienia ustawione na domyślne wystąpienie programu SQL Server.
* Dodaj **CORP\Install** jako rola administratora systemu do domyślnego wystąpienia programu SQL Server.
* Otwórz Zaporę dostępu zdalnego programu SQL Server.
* Włącz zawsze włączone grupy funkcji dostępności.
* Zmień konto usługi programu SQL Server, które **CORP\SQLSvc1** i **CORP\SQLSvc2**odpowiednio.

Te akcje można wykonać w dowolnej kolejności. Niemniej jednak poniższe kroki przeprowadzi je w kolejności. Postępuj zgodnie z instrukcjami dla obu **ContosoSQL1** i **ContosoSQL2**:

1. Jeśli nie nastąpiło wylogowanie sesji usług pulpitu zdalnego dla maszyny wirtualnej, należy to zrobić teraz.
2. Otwieranie plików protokołu RDP dla **ContosoSQL1** i **ContosoSQL2**i zaloguj się jako **BUILTIN\AzureAdmin**.
3. Dodaj **NT AUTHORITY\System** do logowania do programu SQL Server z niezbędne uprawnienia. Otwórz program **SQL Server Management Studio**.
4. Kliknij przycisk **Connect** nawiązać połączenia z domyślnego wystąpienia programu SQL Server.
5. W **Eksplorator obiektów**, rozwiń węzeł **zabezpieczeń**, a następnie rozwiń węzeł **logowania**.
6. Kliknij prawym przyciskiem myszy **NT AUTHORITY\System** logowania, a następnie kliknij przycisk **właściwości**.
7. Na **zabezpieczanych obiektów** strony dla serwera lokalnego, wybierz opcję **Grant** następujące uprawnienia, a następnie kliknij przycisk **OK**.
   
   * Instrukcja ALTER żadnej grupy dostępności
   * Połączenia SQL
   * Widok stanu serwera
8. Dodaj **CORP\Install** jako **sysadmin** roli do domyślnego wystąpienia programu SQL Server. W **Eksplorator obiektów**, kliknij prawym przyciskiem myszy **logowania**, a następnie kliknij przycisk **nowe dane logowania**.
9. Typ **CORP\Install** w **nazwa logowania**.
10. Na **ról serwera** wybierz pozycję **sysadmin**, a następnie kliknij przycisk **OK**. Po utworzeniu nazwy logowania, można to sprawdzić rozwijając **logowania** w **Eksplorator obiektów**.
11. Aby utworzyć regułę zapory dla programu SQL Server na **Start** ekranu, otwórz **Zapora systemu Windows z zabezpieczeniami zaawansowanymi**.
12. W okienku po lewej stronie wybierz **reguły ruchu przychodzącego**. W okienku po prawej stronie kliknij **nową regułę**.
13. Na **typ reguły** kliknij przycisk **Program** > **dalej**.
14. Na **Program** wybierz pozycję **ta ścieżka programu**, typ **%ProgramFiles%\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** w polu tekstowym, a następnie kliknij przycisk **dalej**. Jeśli po te kierunkach, ale przy użyciu programu SQL Server 2012, katalog programu SQL Server jest **MSSQL11. MSSQLSERVER**.
15. Na **akcji** Zachowaj **zezwalały na połączenie** zaznaczone, a następnie kliknij przycisk **dalej**.
16. Na **profilu** Zaakceptuj ustawienia domyślne, a następnie kliknij pozycję **dalej**.
17. Na **nazwa** Określ nazwę reguły, takie jak **programu SQL Server (reguły programu)**w **nazwa** tekstu, a następnie kliknij przycisk **Zakończ**.
18. Aby włączyć **zawsze włączonych grup dostępności** funkcji na **Start** ekranu, otwórz **SQL Server Configuration Manager**.
19. W drzewie w przeglądarce kliknij **usług SQL Server**, kliknij prawym przyciskiem myszy **programu SQL Server (MSSQLSERVER)** usługi, a następnie kliknij przycisk **właściwości**.
20. Kliknij przycisk **zawsze na wysoką dostępność** wybierz opcję **Włącz zawsze włączone grupy dostępności**, jak pokazano w poniższym zrzucie ekranu, a następnie kliknij **Zastosuj**. Kliknij przycisk **OK** w oknie dialogowym i czy nie zamykaj **właściwości** jeszcze — okno dialogowe. Po zmianie konta usługi zostanie uruchomiony ponownie usługę programu SQL Server.
    
     ![Włącz zawsze na grupy dostępności](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)
21. Aby zmienić konto usługi programu SQL Server, kliknij przycisk **logowanie** , wpisz **CORP\SQLSvc1** (dla **ContosoSQL1**) lub **CORP\SQLSvc2** (dla **ContosoSQL2**) w **nazwa konta**, wprowadź i Potwierdź hasło, a następnie kliknij **OK**.
22. W wyświetlonym oknie dialogowym kliknij **tak** ponownie uruchomić usługę SQL Server. Po ponownym uruchomieniu usługi SQL Server, zmian tego wprowadzanych w **właściwości** obowiązują okno dialogowe.
23. Wyloguj się z maszyn wirtualnych.

## <a name="create-the-availability-group"></a>Utworzenie grupy dostępności
Teraz można przystąpić do konfigurowania grupy dostępności. Poniżej znajdują się zarys będzie wykonywać:

* Utwórz nową bazę danych (**MyDB1**) na **ContosoSQL1**.
* Przełączyć pełnej kopii zapasowej i kopii zapasowej dziennika transakcji bazy danych.
* Przywróć pełnego i kopii zapasowych do dziennika **ContosoSQL2** z **NORECOVERY** opcji.
* Utworzenie grupy dostępności (**AG1**) z zatwierdzanie synchroniczne, automatycznej pracy awaryjnej i do odczytu replikach pomocniczych.

### <a name="create-the-mydb1-database-on-contososql1"></a>Utwórz bazę danych MyDB1 na ContosoSQL1
1. Jeśli użytkownik ma już niepodpisana poza sesji pulpitu zdalnego dla **ContosoSQL1** i **ContosoSQL2**, zrób to teraz.
2. Otwórz plik RDP dla **ContosoSQL1**i zaloguj się jako **CORP\Install**.
3. W **Eksploratora plików**w obszarze **C:\\**, Utwórz katalog o nazwie **kopii zapasowej**. Ten katalog użyje kopii zapasowej i przywracanie bazy danych.
4. Kliknij prawym przyciskiem myszy nowy katalog, wskaż pozycję **udostępniać**, a następnie kliknij przycisk **określone osoby**, jak pokazano na poniższym zrzucie ekranu.
   
    ![Tworzenie folderu kopii zapasowej](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)
5. Dodaj **CORP\SQLSvc1**, a następnie nadaj **odczytu/zapisu** uprawnienia. Dodaj **CORP\SQLSvc2**, a następnie nadaj **odczytu** uprawnienia, jak pokazano na poniższym zrzucie ekranu, a następnie kliknij przycisk **udziału**. Po zakończeniu procesu udostępniania plików, kliknij przycisk **gotowe**.
   
    ![Udzielanie uprawnień dla folderu kopii zapasowej](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)
6. Utworzyć bazę danych, z **Start** menu, otwórz **programu SQL Server Management Studio**, a następnie kliknij przycisk **Connect** nawiązać połączenia z domyślnego wystąpienia programu SQL Server.
7. W **Eksplorator obiektów**, kliknij prawym przyciskiem myszy **baz danych**, a następnie kliknij przycisk **nową bazę danych**.
8. W **Nazwa bazy danych**, typ **MyDB1**, a następnie kliknij przycisk **OK**.

### <a name="make-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Utwórz kopię zapasową MyDB1 pełne i przywróć ją na ContosoSQL2
1. Pełnej kopii zapasowej bazy danych, aby w **Eksplorator obiektów**, rozwiń węzeł **baz danych**, kliknij prawym przyciskiem myszy **MyDB1**, wskaż **zadania**, a następnie kliknij przycisk **kopię zapasową**.
2. W **źródła** sekcji, Zachowaj **typ kopii zapasowej** ustawioną **pełne**. W **docelowego** kliknij **Usuń** usunąć domyślna ścieżka pliku dla pliku kopii zapasowej.
3. W **docelowego** kliknij **Dodaj**.
4. W **nazwę pliku** polu tekstowym  **\\ContosoSQL1\backup\MyDB1.bak**, kliknij przycisk **OK**, a następnie kliknij przycisk **OK** ponownie, aby utworzyć kopię zapasową bazy danych. Po zakończeniu operacji tworzenia kopii zapasowej, kliknij **OK** ponownie, aby zamknąć okno dialogowe.
5. Kopię zapasową dziennika transakcji bazy danych, aby w **Eksplorator obiektów**, rozwiń węzeł **baz danych**, kliknij prawym przyciskiem myszy **MyDB1**, wskaż **zadania**, a następnie kliknij przycisk **kopię zapasową**.
6. W **typ kopii zapasowej**, wybierz pozycję **dziennika transakcji**. Zachowaj **docelowego** pliku ścieżka zestawu do jednego określone wcześniej, a następnie kliknij przycisk **OK**. Po zakończeniu operacji tworzenia kopii zapasowej, kliknij przycisk **OK** ponownie.
7. Aby przywrócić transakcji i pełnej kopii zapasowych dziennika **ContosoSQL2**, otwórz plik RDP dla **ContosoSQL2**i zaloguj się jako **CORP\Install**. Pozostaw sesji usług pulpitu zdalnego dla **ContosoSQL1** otworzyć.
8. Z **Start** menu, otwórz **programu SQL Server Management Studio**, a następnie kliknij przycisk **Connect** nawiązać połączenia z domyślnego wystąpienia programu SQL Server.
9. W **Eksplorator obiektów**, kliknij prawym przyciskiem myszy **baz danych**, a następnie kliknij przycisk **Restore Database**.
10. W **źródła** zaznacz **urządzenia**i kliknij przycisk wielokropka **...** Dodaj...
11. W **wybierz urządzenia kopii zapasowej**, kliknij przycisk **Dodaj**.
12. W **lokalizacja pliku kopii zapasowej**, typ  **\\ContosoSQL1\backup**, kliknij przycisk **Odśwież**, wybierz pozycję **MyDB1.bak**, kliknij przycisk **OK**, a następnie kliknij przycisk **OK** ponownie. Powinien zostać wyświetlony pełnej kopii zapasowej i kopii zapasowej dziennika w **zestawy kopii zapasowych do przywrócenia** okienka.
13. Przejdź do **opcje** wybierz pozycję **PRZYWRÓCIĆ WITH NORECOVERY** w **stan odzyskiwania**, a następnie kliknij przycisk **OK** przywrócenie bazy danych. Po zakończeniu operacji przywracania, kliknij przycisk **OK**.

### <a name="create-the-availability-group"></a>Utworzenie grupy dostępności
1. Wróć do sesji pulpitu zdalnego dla **ContosoSQL1**. W **Eksplorator obiektów** w programu SQL Server Management Studio, kliknij prawym przyciskiem myszy **zawsze na wysoką dostępność**, a następnie kliknij przycisk **Kreatora nowej grupy dostępności**, jak pokazano na poniższym zrzucie ekranu.
   
    ![Uruchom Kreatora nowej grupy dostępności](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)
2. Na **wprowadzenie** kliknij przycisk **dalej**. Na **Określ nazwę grupy dostępności** wpisz **AG1** w **Nazwa grupy dostępności**, następnie kliknij przycisk **dalej** ponownie.
   
    ![Kreatora nowej grupy dostępności, nazwa grupy dostępności Określ](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)
3. Na **wybierz baz danych** wybierz **MyDB1**, a następnie kliknij przycisk **dalej**. Bazy danych spełnia wymagania wstępne grupy dostępności, ponieważ co najmniej jedną pełną kopię zapasową wykonano w zamierzonej replice podstawowej.
   
    ![Kreatora nowej grupy Availabilty, wybierz opcję baz danych](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)
4. na **Określanie replik** kliknij przycisk **dodać repliki**.
   
    ![Kreatora nowej grupy Availabilty, określ repliki](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)
5. W **Połącz z serwerem** okno dialogowe, typ **ContosoSQL2** w **nazwy serwera**, a następnie kliknij przycisk **Connect**.
   
    ![Nowa grupa Availabilty kreatora Połącz z serwerem](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)
6. Ponownie **Określanie replik** strony, powinien zostać wyświetlony **ContosoSQL2** na liście **replik dostępności**. Konfigurowanie replik, jak pokazano na poniższym zrzucie ekranu. Gdy skończysz, kliknij przycisk **dalej**.
   
    ![Kreatora nowej grupy Availabilty, określ repliki (Zakończ)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)
7. Na **Wybierz początkową synchronizację danych** wybierz **tylko Dołącz**, a następnie kliknij przycisk **dalej**. Już wykonanego synchronizacji danych ręcznie wprowadzone transakcji i pełne kopie zapasowe na **ContosoSQL1** i przywrócić je na **ContosoSQL2**. Użytkownik może nie kopii zapasowej i przywracanie operacji w bazie danych, a zamiast tego wybrać **pełne** możliwość przeprowadzenia synchronizacji danych można Kreatora nowej grupy dostępności. Jednak zaleca się tę opcję dla bardzo dużych baz danych, które znajdują się w niektóre przedsiębiorstwa.
   
    ![Kreator nowej grupy Availabilty, wybierz początkową synchronizację danych](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)
8. Na **weryfikacji** kliknij przycisk **dalej**. Ta strona powinna wyglądać podobnie do Poniższy zrzut ekranu. Jest ostrzeżenie w konfiguracji odbiornika, ponieważ nie skonfigurowano odbiornik grupy dostępności. Można zignorować to ostrzeżenie, ponieważ w tym samouczku nie konfiguruje odbiornik. Aby skonfigurować odbiornik, po ukończeniu tego samouczka, zobacz [skonfigurować odbiornik ILB dla zawsze włączonych grup dostępności na platformie Azure](../classic/ps-sql-int-listener.md).
   
    ![Availabilty Kreatora nowej grupy sprawdzania poprawności](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)
9. Na **Podsumowanie** kliknij przycisk **Zakończ**, a następnie zaczekaj, aż Kreator skonfiguruje nową grupę dostępności. Na **postępu** strony, możesz kliknąć **szczegółowe** Aby wyświetlić szczegółowe informacje o postępie. Po zakończeniu pracy kreatora, należy sprawdzić **wyniki** Aby sprawdzić, czy grupa dostępności została pomyślnie utworzona, jak pokazano na poniższym zrzucie ekranu, a następnie kliknij przycisk **Zamknij** aby zakończyć pracę kreatora.
   
    ![Availabilty Kreatora nowej grupy wyników](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)
10. W **Eksplorator obiektów**, rozwiń węzeł **zawsze na wysoką dostępność**, a następnie rozwiń węzeł **grup dostępności**. Powinna zostać wyświetlona nowej grupy dostępności w tym kontenerze. Kliknij prawym przyciskiem myszy **AG1 (podstawowe)**, a następnie kliknij przycisk **Pokaż pulpit nawigacyjny**.
    
     ![Pokaż grupy dostępności pulpitu nawigacyjnego](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)
11. Twoje **zawsze na pulpicie nawigacyjnym** powinien być podobny do przedstawionego na poniższym zrzucie ekranu. Widać replik, tryb pracy awaryjnej każdej repliki i stan synchronizacji.
    
     ![Pulpit nawigacyjny grupy dostępności](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)
12. Wróć do **Menedżera serwera**, kliknij przycisk **narzędzia**, a następnie otwórz **Menedżera klastra trybu Failover**.
13. Rozwiń węzeł **Cluster1.corp.contoso.com**, a następnie rozwiń węzeł **usługi i aplikacje**. Wybierz **ról** i należy pamiętać, że **AG1** rola grupy dostępności została utworzona. Należy pamiętać, że AG1 nie ma adresu IP przez bazę danych, która klienci mogą łączyć się z grupy dostępności, ponieważ nie może skonfigurować odbiornik. Można połączyć się bezpośrednio do węzła podstawowego dla operacji odczytu i zapisu i węzła pomocniczego dla zapytań tylko do odczytu.
    
     ![W Menedżerze klastra trybu Failover grupy dostępności](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

> [!WARNING]
> Nie należy próbować awaryjnie grupy dostępności z Menedżera klastra pracy awaryjnej. Należy wykonać wszystkie operacje trybu failover z poziomu **zawsze na pulpicie nawigacyjnym** w programie SQL Server Management Studio. Aby uzyskać więcej informacji, zobacz [ograniczenia na przy użyciu Menedżera klastra trybu Failover z grupami dostępności](https://msdn.microsoft.com/library/ff929171.aspx).
> 
> 

## <a name="next-steps"></a>Następne kroki
Możesz teraz pomyślnie wdrożono programu SQL Server AlwaysOn przez utworzenie grupy dostępności na platformie Azure. Aby skonfigurować odbiornik grupy dostępności, zobacz [skonfigurować odbiornik ILB dla zawsze włączonych grup dostępności na platformie Azure](../classic/ps-sql-int-listener.md).

Aby uzyskać inne informacje o korzystaniu z programu SQL Server na platformie Azure, zobacz [programu SQL Server na maszynach wirtualnych Azure](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

