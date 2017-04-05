---
title: Tworzenie pierwszej sieci wirtualnej platformy Azure | Microsoft Docs
description: "Dowiedz się, jak utworzyć sieć wirtualną platformy Azure (sieć wirtualną) i podłączyć do niej dwie maszyny wirtualne oraz nawiązać połączenie z tymi maszynami wirtualnymi."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: c731099cb91512f3bf0ecc2ffa5258788c90cd1b
ms.lasthandoff: 03/31/2017


---

# <a name="create-your-first-virtual-network"></a>Tworzenie pierwszej sieci wirtualnej

Dowiedz się, jak utworzyć sieć wirtualną z dwiema podsieciami, utworzyć dwie maszyny wirtualne i podłączyć każdą maszynę wirtualną do jednej podsieci, jak pokazano na poniższej ilustracji:

![Diagram sieci wirtualnej](./media/virtual-network-get-started-vnet-subnet/vnet-diagram.png)

Sieć wirtualna Azure odzwierciedla w chmurze Twoją sieć. Możesz kontrolować ustawienia sieciowe Azure i definiować bloki adresów DHCP, ustawienia DNS, zasady zabezpieczeń oraz routing. Aby dowiedzieć się więcej na temat pojęć związanych z sieciami wirtualnymi, przeczytaj artykuł [Omówienie sieci wirtualnych](virtual-networks-overview.md). Aby utworzyć zasoby pokazane na ilustracji, wykonaj poniższe czynności:

1. [Utwórz sieć wirtualną z dwiema podsieciami](#create-vnet).
2. [Utwórz dwie maszyny wirtualne z interfejsami sieciowymi (kartami sieciowymi)](#create-vms) i skojarz sieciową grupę zabezpieczeń z każdą kartą sieciową.
3. [Nawiąż połączenie z maszynami wirtualnymi](#connect-to-from-vms).
4. [Usuń wszystkie zasoby](#delete-resources). Podczas aprowizowania niektórych zasobów utworzonych w tym ćwiczeniu zostaną naliczone opłaty. Aby je zminimalizować, po ukończeniu ćwiczenia wykonaj czynności opisane w tej sekcji w celu usunięcia utworzonych zasobów.

Po wykonaniu czynności opisanych w tym artykule uzyskasz podstawową wiedzę w zakresie korzystania z sieci wirtualnej. Następne kroki umożliwiają zdobycie dodatkowych informacji i bardziej szczegółowe zapoznanie się z zagadnieniami dotyczącymi używania sieci wirtualnych.

## <a name="create-vnet"></a>Tworzenie sieci wirtualnej z dwiema podsieciami

Aby utworzyć sieć wirtualną z dwiema podsieciami, wykonaj poniższe czynności. Podsieci używa się zwykle do sterowania przepływem ruchu sieciowego.

1. Zaloguj się do witryny [Azure Portal](<https://portal.azure.com>). Jeśli jeszcze nie masz konta, możesz skorzystać z [bezpłatnej miesięcznej wersji próbnej](https://azure.microsoft.com/free). 
2. W okienku **Ulubione** portalu kliknij pozycję **Nowy**.
3. W bloku **Nowy** kliknij pozycję **Sieć**. W bloku **Sieć** kliknij pozycję **Sieć wirtualna**, jak pokazano na poniższej ilustracji:

    ![Diagram sieci wirtualnej](./media/virtual-network-get-started-vnet-subnet/virtual-network.png)

4.  W bloku **Sieć wirtualna** pozostaw wybrany model wdrożenia *Resource Manager* i kliknij pozycję **Utwórz**.
5.  W wyświetlonym oknie **Tworzenie bloku sieci wirtualnej** wprowadź następujące wartości, a następnie kliknij pozycję **Utwórz**:

    |**Ustawienie**|**Wartość**|**Szczegóły**|
    |---|---|---|
    |**Nazwa**|*MojaSiećWirtualna*|Nazwa musi być unikatowa w obrębie grupy zasobów.|
    |**Przestrzeń adresowa**|*10.0.0.0/16*|Można podać dowolną przestrzeń adresową w notacji CIDR.|
    |**Nazwa podsieci**|*Fronton*|Nazwa podsieci musi być unikatowa w obrębie sieci wirtualnej.|
    |**Zakres adresów podsieci**|*10.0.0.0/24*| Podany zakres musi istnieć w przestrzeni adresowej zdefiniowanej dla sieci wirtualnej.|
    |**Subskrypcja**|*[Twoja subskrypcja]*|Wybierz subskrypcję, w której chcesz utworzyć sieć wirtualną. Sieć wirtualna istnieje w jednej subskrypcji.|
    |**Grupa zasobów**|**Utwórz nową:** *MojaGZ*|Utwórz grupę zasobów. Nazwa grupy zasobów musi być unikatowa w obrębie wybranej subskrypcji. Aby dowiedzieć się więcej na temat grup zasobów, zapoznaj się z artykułem [Omówienie usługi Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups).|
    |**Lokalizacja**|*Zachodnie stany USA*| Zazwyczaj wybiera się najbliższą lokalizację.|

    Tworzenie sieci wirtualnej trwa kilka sekund. Po utworzeniu sieci pojawia się pulpit nawigacyjny witryny Azure Portal.

6. Gdy utworzysz sieć wirtualną, w okienku **Ulubione** witryny Azure Portal kliknij pozycję **Wszystkie zasoby**. Kliknij sieć wirtualną **MojaSiećWirtualna** w bloku **Wszystkie zasoby**. Jeśli wybrana subskrypcja zawiera kilka zasobów, możesz wpisać *MojaSiećWirtualna* w polu **Filtruj według nazwy...**, aby łatwo uzyskać dostęp do sieci wirtualnej.
7. Zostanie otwarty blok **MojaSiećWirtualna** z informacjami o sieci wirtualnej, jak pokazano na poniższej ilustracji:

    ![Diagram sieci wirtualnej](./media/virtual-network-get-started-vnet-subnet/myvnet.png)

8. Kliknij pozycję **Podsieci**, aby wyświetlić listę podsieci w sieci wirtualnej, jak pokazano na poprzedniej ilustracji. Widoczna jest tylko podsieć **Fronton**, która została utworzona w kroku 5.
9. W bloku MojaSiećWirtualna — Podsieci kliknij pozycję **+ Podsieć**, aby utworzyć podsieć o następujących parametrach. Kliknij przycisk **OK**:

    |**Ustawienie**|**Wartość**|**Szczegóły**|
    |---|---|---|
    |**Nazwa**|*Zaplecze*|Nazwa musi być unikatowa w obrębie sieci wirtualnej.|
    |**Zakres adresów**|*10.0.1.0/24*|Podany zakres musi istnieć w przestrzeni adresowej zdefiniowanej dla sieci wirtualnej.|
    |**Sieciowa grupa zabezpieczeń** i **Tabela tras**|*Brak* (domyślnie)|Sieciowe grupy zabezpieczeń zostały omówione w dalszej części tego artykułu. Aby dowiedzieć się więcej o trasach definiowanych przez użytkownika, przeczytaj artykuł [Trasy definiowane przez użytkownika](virtual-networks-udr-overview.md).|

10. Po dodaniu nowej podsieci do sieci wirtualnej możesz zamknąć bloki **MojaSiećWirtualna — Podsieci** i **Wszystkie zasoby**.

## <a name="create-vms"></a>Tworzenie maszyn wirtualnych

Po utworzeniu sieci wirtualnej i podsieci możesz utworzyć maszyny wirtualne. W tym ćwiczeniu na obu maszynach wirtualnych zostanie uruchomiony system operacyjny Windows Server. Może to być jednak dowolny system operacyjny obsługiwany przez platformę Azure, łącznie z kilkoma różnymi dystrybucjami systemu Linux.

### <a name="create-web-server-vm"></a>Tworzenie maszyny wirtualnej serwera sieci Web

Aby utworzyć maszynę wirtualną serwera sieci Web, wykonaj następujące czynności:

1. W okienku Ulubione witryny Azure Portal kliknij kolejno pozycje **Nowy**, **Obliczenia** i **Windows Server 2016 Datacenter**.
2. W bloku **Windows Server 2016 Datacenter** kliknij pozycję **Utwórz**.
3. W wyświetlonym bloku **Podstawowe** wprowadź lub wybierz następujące wartości i kliknij przycisk **OK**:

    |**Ustawienie**| **Wartość**|**Szczegóły**|
    |---|---|---|
    |**Nazwa**|*MójSerwerSieciWeb*|Ta maszyna wirtualna służy jako serwer sieci Web, z którym łączą się zasoby internetowe.|
    |**Typ dysku maszyny wirtualnej**|*SSD*|
    |**Nazwa użytkownika**|*Wartość wybrana przez użytkownika*|
    |**Hasło i Potwierdź hasło**|*Wartość wybrana przez użytkownika*|
    | **Subskrypcja**|*<Your subscription>*|Musi to być ta sama subskrypcja, która została wybrana w kroku 5 sekcji [Tworzenie sieci wirtualnej z dwiema podsieciami](#create-vnet) w tym artykule. Sieć wirtualna, do której podłączasz maszynę wirtualną, musi istnieć w tej samej subskrypcji co maszyna wirtualna.|
    |**Grupa zasobów**|**Użyj istniejącej:** wybierz wartość *MojaGZ*|Mimo że jest używana ta sama grupa zasobów co w przypadku sieci wirtualnej, zasoby nie muszą być objęte tą samą grupą zasobów.|
    |**Lokalizacja**|*Zachodnie stany USA*|Musi to być ta sama lokalizacja, która została wybrana w kroku 5 sekcji [Tworzenie sieci wirtualnej z dwiema podsieciami](#create-vnet) w tym artykule. Maszyny wirtualne i sieci wirtualne, z którymi łączą się te maszyny, muszą istnieć w tej samej lokalizacji.|

4. W bloku **Wybierz rozmiar** kliknij pozycję *DS1_V2 Standardowa*, a następnie kliknij pozycję **Wybierz**. Listę wszystkich rozmiarów maszyn wirtualnych z systemem Windows obsługiwanych przez platformę Azure można znaleźć w artykule [Windows VM sizes](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Rozmiary maszyn wirtualnych z systemem Windows).
5. W bloku **Ustawienia** wprowadź lub wybierz następujące wartości i kliknij przycisk **OK**:

    |**Ustawienie**|**Wartość**|**Szczegóły**|
    |---|---|---|
    |**Przestrzeń dyskowa: użyj zarządzanych dysków**|*Tak*||
    |**Sieć wirtualna**| Wybierz wartość *MojaSiećWirtualna*|Możesz wybrać dowolną sieć wirtualną, która znajduje się w tej samej lokalizacji co tworzona maszyna wirtualna. Aby dowiedzieć się więcej na temat sieci wirtualnych i podsieci, przeczytaj artykuł [Sieci wirtualne](virtual-networks-overview.md).|
    |**Podsieć**|Wybierz wartość *Fronton*|Możesz wybrać dowolną podsieć istniejącą w sieci wirtualnej.|
    |**Publiczny adres IP**|Zaakceptuj domyślną wartość|Publiczny adres IP umożliwia nawiązywanie połączeń między Internetem a maszyną wirtualną. Aby dowiedzieć się więcej o publicznych adresach IP, zapoznaj się z artykułem [Adresy IP](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses).|
    |**Sieciowa grupa zabezpieczeń (zapora)**|Zaakceptuj domyślną wartość|Kliknij domyślną sieciową grupę zabezpieczeń **(nowa) MójSerwerSieciWeb — sgz** utworzoną w portalu, aby wyświetlić jej ustawienia. Zwróć uwagę na to, że wyświetlony blok **Tworzenie sieciowej grupy zabezpieczeń** zawiera regułę ruchu przychodzącego, która zezwala na ruch TCP/3389 (RDP) z dowolnego źródłowego adresu IP.|
    |**Wszystkie inne wartości**|Zaakceptuj wartości domyślne|Aby dowiedzieć się więcej o pozostałych ustawieniach, zapoznaj się z artykułem [Informacje o maszynach wirtualnych](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|

    Sieciowe grupy zabezpieczeń umożliwiają tworzenie reguł dla ruchu przychodzącego do maszyn wirtualnych oraz ruchu wychodzącego. Domyślnie cały ruch przychodzący do maszyny wirtualnej jest blokowany. W przypadku serwera sieci Web w środowisku produkcyjnym można dodać reguły ruchu przychodzącego dla portów TCP/80 (HTTP) i TCP/443 (HTTPS). Dla ruchu wychodzącego nie ma reguł, ponieważ domyślnie cały ruch wychodzący jest dozwolony. Można dodawać i usuwać reguły w celu sterowania ruchem w ramach zasad. Aby dowiedzieć się więcej na temat sieciowych grup zabezpieczeń, zapoznaj się z artykułem [Sieciowe grupy zabezpieczeń](virtual-networks-nsg.md).

6.  Sprawdź ustawienia w bloku **Podsumowanie** i kliknij przycisk **OK**, aby utworzyć maszynę wirtualną. Podczas tworzenia maszyny wirtualnej w pulpicie nawigacyjnym portalu jest wyświetlany kafelek stanu. Proces tworzenia może potrwać kilka minut. Nie trzeba czekać na jego zakończenie i można od razu przejść do następnego kroku.

### <a name="create-database-server-vm"></a>Tworzenie maszyny wirtualnej serwera bazy danych

Aby utworzyć maszynę wirtualną serwera bazy danych, wykonaj następujące czynności:

1.  W okienku Ulubione kliknij kolejno pozycje **Nowy**, **Obliczenia** i **Windows Server 2016 Datacenter**.
2.  W bloku **Windows Server 2016 Datacenter** kliknij pozycję **Utwórz**.
3.  W bloku **Podstawowe** wprowadź lub wybierz następujące wartości, a następnie kliknij przycisk **OK**:

    |**Ustawienie**|**Wartość**|**Szczegóły**|
    |---|---|---|
    |**Nazwa**|*MójSerwerBD*|Ta maszyna wirtualna służy jako serwer bazy danych, z którym łączy się serwer sieci Web. Nawiązanie połączenia z tym serwerem bazy danych przez Internet nie jest możliwe.|
    |**Typ dysku maszyny wirtualnej**|*SSD*||
    |**Nazwa użytkownika**|Wartość wybrana przez użytkownika||
    |**Hasło i Potwierdź hasło**|Wartość wybrana przez użytkownika||
    |**Subskrypcja**|<Your subscription>|Musi to być ta sama subskrypcja, która została wybrana w kroku 5 sekcji [Tworzenie sieci wirtualnej z dwiema podsieciami](#create-vnet) w tym artykule.|
    |**Grupa zasobów**|**Użyj istniejącej:** wybierz wartość *MojaGZ*|Mimo że jest używana ta sama grupa zasobów co w przypadku sieci wirtualnej, zasoby nie muszą być objęte tą samą grupą zasobów.|
    |**Lokalizacja**|*Zachodnie stany USA*|Musi to być ta sama lokalizacja, która została wybrana w kroku 5 sekcji [Tworzenie sieci wirtualnej z dwiema podsieciami](#create-vnet) w tym artykule.|

4.  W bloku **Wybierz rozmiar** kliknij pozycję *DS1_V2 Standardowa*, a następnie kliknij pozycję **Wybierz**.
5.  W bloku **Ustawienia** wprowadź lub wybierz następujące wartości i kliknij przycisk **OK**:

    |**Ustawienie**|**Wartość**|**Szczegóły**|
    |----|----|---|
    |**Przestrzeń dyskowa: użyj zarządzanych dysków**|*Tak*||
    |**Sieć wirtualna**|Wybierz wartość *MojaSiećWirtualna*|Możesz wybrać dowolną sieć wirtualną, która znajduje się w tej samej lokalizacji co tworzona maszyna wirtualna.|
    |**Podsieć**|Kliknij pole **Podsieć** i wybierz pozycję *Zaplecze*, a następnie wybierz pozycję **Zaplecze** w bloku **Wybieranie podsieci**|Możesz wybrać dowolną podsieć istniejącą w sieci wirtualnej.|
    |**Publiczny adres IP**|Brak — kliknij adres domyślny, a następnie kliknij pozycję **Brak** w bloku **Wybierz publiczny adres IP**|Jeśli nie podasz publicznego adresu IP, będzie można łączyć się z maszyną wirtualną tylko przy użyciu innej maszyny wirtualnej podłączonej do tej samej sieci wirtualnej. Bezpośrednie łączenie się z maszyną wirtualną przez Internet nie będzie możliwe.|
    |**Sieciowa grupa zabezpieczeń (zapora)**|Zaakceptuj domyślną wartość| Ta sieciowa grupa zabezpieczeń ma tę samą domyślną regułę ruchu przychodzącego co domyślna sieciowa grupa zabezpieczeń utworzona dla maszyny wirtualnej MójSerwerSieciWeb. W przypadku serwera bazy danych można dodać regułę ruchu przychodzącego dla protokołu TCP/1433 (MS SQL). Dla ruchu wychodzącego nie ma reguł, ponieważ domyślnie cały ruch wychodzący jest dozwolony. Można dodawać i usuwać reguły w celu sterowania ruchem w ramach zasad.|
    |**Wszystkie inne wartości**|Zaakceptuj wartości domyślne||

6.  Sprawdź ustawienia w bloku **Podsumowanie** i kliknij przycisk **OK**, aby utworzyć maszynę wirtualną. Podczas tworzenia maszyny wirtualnej w pulpicie nawigacyjnym portalu jest wyświetlany kafelek stanu. Proces tworzenia może potrwać kilka minut. Nie trzeba czekać na jego zakończenie i można od razu przejść do następnego kroku.

## <a name="review"></a>Przeglądanie zasobów

Mimo że zadanie obejmowało utworzenie jednej sieci wirtualnej i dwóch maszyn wirtualnych, w witrynie Azure Portal automatycznie zostało utworzonych kilka dodatkowych zasobów w grupie zasobów MojaGZ. Aby przejrzeć zawartość grupy zasobów MojaGZ, wykonaj następujące czynności:

1. W okienku **Ulubione** kliknij pozycję **Więcej usług**.
2. W okienku **Więcej usług** wpisz *Grupy zasobów* w polu, które zawiera wyraz *Filtruj*. Kliknij pozycję **Grupy zasobów** na wyświetlonej wyfiltrowanej liście.
3. W okienku **Grupy zasobów** kliknij grupę zasobów *MojaGZ*. Jeśli Twoja subskrypcja obejmuje wiele grup zasobów, możesz wpisać *MojaGZ* w polu, które zawiera tekst *Filtruj według nazwy...*, aby szybko uzyskać dostęp do grupy zasobów MojaGZ.
4.  Wyświetlony blok **MojaGZ** zawiera 12 zasobów w grupie zasobów, jak pokazano na poniższej ilustracji:

    ![Zawartość grupy zasobów](./media/virtual-network-get-started-vnet-subnet/resource-group-contents.png)

Aby dowiedzieć się więcej o maszynach wirtualnych, dyskach i kontach magazynu, zapoznaj się z artykułami [Maszyna wirtualna](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Dysk](../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-network%2ftoc.json) i [Konto magazynu](../storage/storage-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) zawierającymi omówienie tych zagadnień. Widoczne są dwie domyślne sieciowe grupy zabezpieczeń automatycznie utworzone przez portal. Widać również, że w portalu zostały utworzone dwa zasoby interfejsów sieciowych (kart sieciowych). Karta sieciowa pozwala maszynie wirtualnej łączyć się z innymi zasobami w sieci wirtualnej. Więcej informacji o kartach sieciowych można znaleźć w artykule [Karty sieciowe](virtual-network-network-interface.md). W portalu utworzono także jeden zasób publicznego adresu IP. Publiczne adresy IP to jedno z ustawień zasobu publicznych adresów IP. Aby dowiedzieć się więcej o publicznych adresach IP, zapoznaj się z artykułem [Adresy IP](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses).

## <a name="connect-to-from-vms"></a>Łączenie się z maszynami wirtualnymi

Po utworzeniu sieci wirtualnej i dwóch maszyn wirtualnych można połączyć się z tymi maszynami wirtualnymi, wykonując czynności opisane w poniższych sekcjach:

### <a name="connect-from-internet"></a>Nawiązywanie połączenia między Internetem a maszyną wirtualną serwera sieci Web

Aby nawiązać połączenie między Internetem a maszyną wirtualną serwera sieci Web, wykonaj następujące czynności:

1. Otwórz grupę zasobów MojaGZ w portalu, wykonując czynności opisane w sekcji [Przeglądanie zasobów](#review) tego artykułu.
2. W bloku **MojaGZ** kliknij maszynę wirtualną **MójSerwerSieciWeb**.
3. W bloku **MójSerwerSieciWeb** kliknij pozycję **Połącz**, jak pokazano na poniższej ilustracji:

    ![Łączenie się z maszyną wirtualną serwera sieci Web](./media/virtual-network-get-started-vnet-subnet/webserver.png)

4. Zezwól przeglądarce na pobranie pliku *MójSerwerSieciWeb.rdp*, a następnie go otwórz.
5. Jeśli pojawi się okno dialogowe informujące o tym, że nie można zweryfikować wydawcy połączenia zdalnego, kliknij pozycję **Połącz**.
6. Podczas wprowadzania poświadczeń pamiętaj o tym, aby zalogować się przy użyciu nazwy użytkownika i hasła podanych w kroku 3 sekcji [Tworzenie maszyny wirtualnej serwera sieci Web](#create-web-server-vm) tego artykułu. Jeśli w wyświetlonym oknie **Zabezpieczenia systemu Windows** nie widać właściwych poświadczeń, może być konieczne kliknięcie kolejno pozycji **Więcej opcji** i **Użyj innego konta** w celu podania prawidłowej nazwy użytkownika i hasła. Kliknij przycisk **OK**, aby połączyć się z maszyną wirtualną.
7. Jeśli pojawi się okno **Podłączanie pulpitu zdalnego** z informacją o tym, że nie można zweryfikować tożsamości komputera zdalnego, kliknij przycisk **Tak**.
8. Masz teraz połączenie z maszyną wirtualną MójSerwerSieciWeb przez Internet. Pozostaw otwarte połączenie pulpitu zdalnego, aby wykonać czynności opisane w następnej sekcji.

Połączenie zdalne korzysta z publicznego adresu IP przypisanego do zasobu publicznego adresu IP, utworzonego w portalu w kroku 5 sekcji [Tworzenie sieci wirtualnej z dwiema podsieciami](#create-vnet) tego artykułu. Połączenie jest dozwolone, ponieważ domyślna reguła utworzona w sieciowej grupie zabezpieczeń **MójSerwerSieciWeb — sgz** zezwala na ruch przychodzący do maszyny wirtualnej przez port TCP/3389 (RDP) z dowolnego źródłowego adresu IP. Jeśli spróbujesz połączyć się z maszyną wirtualną za pośrednictwem innego portu, połączenie nie powiedzie się, chyba że dodasz kolejne reguły ruchu przychodzącego do sieciowej grupy zabezpieczeń w celu zezwolenia na komunikację przy użyciu dodatkowych portów.

>[!NOTE]
>W przypadku dodania kolejnych reguł ruchu przychodzącego do sieciowej grupy zabezpieczeń upewnij się, że te same porty są otwarte w zaporze systemu Windows. Jeśli nie są otwarte, połączenie nie powiedzie się.
>

### <a name="connect-to-internet"></a>Nawiązywanie połączenia między maszyną wirtualną serwera sieci Web a Internetem

Aby nawiązać połączenie między maszyną wirtualną serwera sieci Web a Internetem, wykonaj następujące czynności:

1. Jeśli jeszcze nie masz otwartego połączenia zdalnego z maszyną wirtualną MójSerwerSieciWeb, nawiąż je, wykonując czynności opisane w sekcji [Nawiązywanie połączenia między Internetem a maszyną wirtualną serwera sieci Web](#connect-from-internet) tego artykułu.
2. Otwórz program Internet Explorer na pulpicie systemu Windows. W oknie dialogowym **Instalator programu Internet Explorer 11** kliknij pozycję **Nie używaj zalecanych ustawień**, a następnie kliknij przycisk **OK**. Akceptacja zalecanych ustawień jest wskazana w przypadku serwera produkcyjnego.
3. Na pasku adresu programu Internet Explorer wpisz [bing.com](http:www.bing.com). Jeśli pojawi się okno dialogowe programu Internet Explorer, kliknij pozycję **Dodaj**, kliknij pozycję **Dodaj** w oknie dialogowym **Zaufane witryny**, a następnie kliknij przycisk **Zamknij**. Powtórz te czynności we wszystkich innych oknach dialogowych programu Internet Explorer.
4. Na stronie wyszukiwania serwisu Bing wpisz *whatsmyipaddress*, a następnie kliknij przycisk lupy. Wyszukiwarka Bing zwróci publiczny adres IP przypisany do zasobu publicznego adresu IP utworzonego w portalu podczas tworzenia maszyny wirtualnej. W ustawieniach zasobu **MójSerwerSieciWeb — ip** zobaczysz ten sam adres IP przypisany do zasobu publicznego adresu IP, jak pokazano na poniższej ilustracji. Jednak adres IP przypisany do maszyny wirtualnej jest inny.

    ![Łączenie się z maszyną wirtualną serwera sieci Web](./media/virtual-network-get-started-vnet-subnet/webserver-pip.png)

5.  Pozostaw otwarte połączenie pulpitu zdalnego, aby wykonać czynności opisane w następnej sekcji.

Możesz nawiązać połączenie między maszyną wirtualną a Internetem, ponieważ domyślnie wszystkie połączenia wychodzące z maszyny wirtualnej są dozwolone. Łączność wychodzącą możesz ograniczyć, dodając do sieciowej grupy zabezpieczeń reguły dla karty sieciowej lub dla podsieci, do której jest podłączona karta sieciowa, albo oba typy reguł.

Jeśli maszyna wirtualna pozostaje w stanie zatrzymania (ma cofnięty przydział) w portalu, można zmienić publiczny adres IP. Jeśli jest wymagane, aby publiczny adres IP nigdy się nie zmieniał, można użyć statycznej metody przydziału adresu IP zamiast dynamicznej metody przydziału (domyślnej). Aby dowiedzieć się więcej na temat różnic między metodami przydziału, przeczytaj artykuł [Typy adresów IP i metody alokacji](virtual-network-ip-addresses-overview-arm.md).

### <a name="webserver-to-dbserver"></a>Nawiązywanie połączenia między maszyną wirtualną serwera sieci Web a maszyną wirtualną serwera bazy danych

Aby nawiązać połączenie między maszyną wirtualną serwera sieci Web a maszyną wirtualną serwera bazy danych, wykonaj następujące czynności:

1. Jeśli jeszcze nie masz otwartego połączenia zdalnego z maszyną wirtualną MójSerwerSieciWeb, nawiąż je, wykonując czynności opisane w sekcji [Nawiązywanie połączenia między Internetem a maszyną wirtualną serwera sieci Web](#connect-from-internet) tego artykułu.
2. Kliknij przycisk Start w lewym dolnym rogu pulpitu systemu Windows, a następnie wpisz *pulpit zdalny*. Gdy na liście menu Start pojawi się pozycja **Podłączanie pulpitu zdalnego**, kliknij ją.
3. W oknie dialogowym **Podłączanie pulpitu zdalnego** wpisz nazwę komputera *MójSerwerBD* i kliknij pozycję **Połącz**.
4. Wpisz nazwę użytkownika i hasło wprowadzone w kroku 3 sekcji [Tworzenie maszyny wirtualnej serwera bazy danych](#create-database-server-vm) tego artykułu, a następnie kliknij przycisk **OK**.
5. Jeśli pojawi się okno dialogowe z informacją o tym, że nie można zweryfikować tożsamości komputera zdalnego, kliknij przycisk **Tak**.
6. Pozostaw otwarte połączenie pulpitu zdalnego z obydwoma serwerami, aby wykonać czynności opisane w następnej sekcji.

Nawiązanie połączenia między maszyną wirtualną serwera sieci Web a maszyną wirtualną serwera bazy danych jest możliwe z następujących powodów:

- W domyślnej sieciowej grupie zabezpieczeń, utworzonej w kroku 5 sekcji [Tworzenie maszyny wirtualnej serwera bazy danych](#create-database-server-vm) tego artykułu, połączenia przychodzące TCP/3389 są włączone dla dowolnego źródłowego adresu IP.
- Połączenie zostało zainicjowane na maszynie wirtualnej serwera sieci Web, która jest podłączony do tej samej sieci wirtualnej co maszyna wirtualna serwera bazy danych. Aby połączyć się z maszyną wirtualną, która nie ma przypisanego publicznego adresu IP, musisz nawiązać połączenie na innej maszynie wirtualnej podłączonej do tej samej sieci wirtualnej, nawet jeśli ta maszyna wirtualna jest podłączona do innej podsieci.
- Mimo że maszyny wirtualne są podłączone do różnych podsieci, platforma Azure tworzy trasy domyślne, które umożliwiają łączność między podsieciami. Trasy domyślne można jednak zastąpić własnymi. Aby dowiedzieć się więcej o trasach na platformie Azure, przeczytaj artykuł [Trasy definiowane przez użytkownika](virtual-networks-udr-overview.md).

Jeśli spróbujesz nawiązać połączenie zdalne przez Internet z maszyną wirtualną serwera bazy danych zgodnie z instrukcjami zawartymi w sekcji [Nawiązywanie połączenia między Internetem a maszyną wirtualną serwera sieci Web](#connect-from-internet) tego artykułu, zobaczysz, że opcja **Połącz** jest wyszarzona. Przyczyną jest brak przypisania publicznego adresu IP do maszyny wirtualnej, co uniemożliwia nawiązywanie połączeń przychodzących z Internetu.

### <a name="connect-to-the-internet-from-the-database-server-vm"></a>Nawiązywanie połączenia między maszyną wirtualną serwera bazy danych a Internetem

Aby nawiązać połączenie z Internetem na maszynie wirtualnej serwera bazy danych, wykonaj następujące czynności:

1. Jeśli jeszcze nie masz otwartego połączenia zdalnego między maszyną wirtualną MójSerwerSieciWeb a maszyną wirtualną MójSerwerBD, wykonaj czynności opisane w sekcji [Nawiązywanie połączenia między maszyną wirtualną serwera sieci Web a maszyną wirtualną serwera bazy danych](#webserver-to-dbserver) tego artykułu.
2. Na pulpicie systemu Windows na maszynie wirtualnej MójSerwerBD otwórz program Internet Explorer i w oknach dialogowych wybierz te same opcje, które wybrano w krokach 2 i 3 sekcji [Nawiązywanie połączenia między maszyną wirtualną serwera sieci Web a Internetem](#connect-to-internet) tego artykułu.
3. Na pasku adresu wpisz [bing.com](http:www.bing.com).
4. Kliknij pozycję **Dodaj** w wyświetlonym oknie dialogowym programu Internet Explorer, kliknij pozycję **Dodaj**, a następnie kliknij przycisk **Zamknij** w oknie dialogowym **Zaufane witryny**. Wykonaj te czynności we wszystkich innych oknach dialogowych, które się pojawią.
5. Na stronie wyszukiwania serwisu Bing wpisz *whatsmyipaddress*, a następnie kliknij przycisk lupy. Wyszukiwarka Bing zwraca publiczny adres IP aktualnie przypisany do maszyny wirtualnej w ramach infrastruktury platformy Azure. 6. Na maszynie wirtualnej MójSerwerSieciWeb zamknij połączenie pulpitu zdalnego z maszyną wirtualną MójSerwerBD, a następnie zamknij połączenie zdalne z maszyną wirtualną MójSerwerSieciWeb.

Połączenie wychodzące z Internetem jest dozwolone, ponieważ domyślnie cały ruch wychodzący jest dozwolony, chociaż do maszyny wirtualnej MójSerwerBD nie jest przypisany zasób publicznego adresu IP. Domyślnie wszystkie maszyny wirtualne mogą łączyć się z Internetem — przypisanie zasobu publicznego adresu IP do maszyny wirtualnej nie jest wymagane. Nie możesz jednak łączyć się przez Internet z publicznym adresem IP tak jak na maszynie wirtualnej MójSerwerSieciWeb z przypisanym zasobem publicznego adresu IP.

## <a name="delete-resources"></a>Usuwanie wszystkich zasobów

Aby usunąć wszystkie zasoby utworzone w tym artykule, wykonaj następujące czynności:

1. Aby wyświetlić grupę zasobów MojaGZ utworzoną w tym artykule, wykonaj kroki 1–3 opisane w sekcji [Przeglądanie zasobów](#review). Jeszcze raz przejrzyj zasoby w grupie zasobów. Jeśli grupę zasobów MojaGZ utworzono zgodnie z podanymi instrukcjami, będzie widocznych 12 zasobów, jak pokazano na ilustracji w kroku 4.
2. W bloku MojaGZ kliknij przycisk **Usuń**.
3. Portal wymaga wpisania nazwy grupy zasobów w celu potwierdzenia zamiaru jej usunięcia. Jeśli zobaczysz inne zasoby niż te widoczne w kroku 4 sekcji [Przeglądanie zasobów](#review) tego artykułu, kliknij przycisk **Anuluj**. Jeśli zobaczysz tylko 12 zasobów utworzonych w ramach tego ćwiczenia, wpisz nazwę grupy zasobów *MojaGZ*, a następnie kliknij przycisk **Usuń**. Usunięcie grupy zasobów powoduje usunięcie wszystkich zasobów w niej zawartych, dlatego zawsze należy sprawdzić zawartość grupy zasobów przed jej usunięciem. Portal usuwa wszystkie zasoby zawarte w grupie zasobów, a następnie usuwa tę grupę zasobów. Ten proces trwa kilka minut.

## <a name="next-steps"></a>Następne kroki

W tym ćwiczeniu utworzono sieć wirtualną i dwie maszyny wirtualne. Maszyny wirtualne zostały utworzone przy użyciu zarówno niestandardowych, jak i domyślnych ustawień. Przed wdrożeniem sieci wirtualnych i maszyn wirtualnych w środowisku produkcyjnym zalecamy przeczytanie następujących artykułów w celu zapoznania się ze wszystkimi dostępnymi ustawieniami:

- [Sieci wirtualne](virtual-networks-overview.md)
- [Publiczne adresy IP](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- [Interfejsy sieciowe](virtual-network-network-interface.md)
- [Sieciowe grupy zabezpieczeń](virtual-networks-nsg.md)
- [Maszyny wirtualne](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

