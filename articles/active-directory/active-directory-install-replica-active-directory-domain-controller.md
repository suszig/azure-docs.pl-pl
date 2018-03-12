---
title: "Zainstaluj kontrolery domeny z repliki lokalnej domeny usługi Active Directory na maszynach wirtualnych Azure | Dokumentacja firmy Microsoft"
description: "Jak zainstalować DCs repliki lokalnej domeny usługi Active Directory na maszynach wirtualnych platformy Azure (VM) w sieci wirtualnej platformy Azure."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: f4e64fbc6c2fda026297b69bd54471d49b6785a1
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Instalowanie repliki kontrolera domeny usługi Active Directory w sieci wirtualnej platformy Azure
W tym artykule omówiono sposób instalowania dodatkowych kontrolerów domeny (DC) do użycia jako replik kontrolerów domeny dla domeny usługi Active Directory lokalnego na maszynach wirtualnych platformy Azure (VM) w sieci wirtualnej platformy Azure. Możesz również [zainstalować lasu usługi Active Directory systemu Windows Server w sieci wirtualnej platformy Azure](active-directory-new-forest-virtual-machine.md). Dla instalacji usług domenowych w usłudze Active Directory (AD DS) w sieci wirtualnej platformy Azure, zobacz [wskazówki dotyczące wdrażania systemu Windows Server Active Directory na maszynach wirtualnych Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Diagram scenariusza
W tym scenariuszu użytkownicy zewnętrzni trzeba uzyskiwać dostęp do aplikacji działających na serwerach przyłączonych do domeny. Maszyny wirtualne z systemem serwerów aplikacji, kontrolery domeny repliki są zainstalowane w sieci wirtualnej platformy Azure. Sieci wirtualnej można podłączyć do sieci lokalnej przez [ExpressRoute](../expressroute/expressroute-locations-providers.md), albo użyć [sieci VPN typu lokacja lokacja](../vpn-gateway/vpn-gateway-site-to-site-create.md) połączenia, jak pokazano: 

![Diagram sieci wirtualnej platformy Azure pf repliki kontrolera domeny usługi Active Directory][1]

Serwery aplikacji i kontrolery domeny są wdrażane w ramach usługi w chmurze oddzielne do dystrybucji przetwarzania obliczeń i w obrębie zestawów dostępności ulepszone odporności na uszkodzenia.
Kontrolery domen replikacji ze sobą i z kontrolerów domeny lokalnej za pomocą replikacji usługi Active Directory. Nie są konieczne żadne narzędzia synchronizacji.

## <a name="create-an-on-premises-active-directory-site-for-the-azure-virtual-network"></a>Tworzenie witryny lokalnej usługi Active Directory dla sieci wirtualnej platformy Azure
Można utworzyć lokację w usłudze Active Directory reprezentujące region sieci odpowiadającego do sieci wirtualnej. Ta lokacja może optymalizowania uwierzytelniania, replikacji i innych operacji lokalizacji kontrolera domeny. W poniższych krokach opisano sposób tworzenia lokacji i w tle więcej, zobacz [Dodawanie nowej lokacji](https://technet.microsoft.com/library/cc781496.aspx).

1. Otwórz Lokacje i usługi Active Directory: **Menedżera serwera** > **narzędzia** > **Lokacje i usługi Active Directory**.
2. Utwórz witrynę do reprezentowania regionu, w którym utworzono sieci wirtualnej platformy Azure: kliknij **witryny** > **akcji** > **nowej lokacji** > wpisz nazwę nowej lokacji, takich jak Azure nam zachodnie > Wybierz link lokacji > **OK**.
3. Utwórz podsieć i skojarzyć z nowej lokacji: kliknij dwukrotnie **witryny** > kliknij prawym przyciskiem myszy **podsieci** > **nową podsieć** > wpisz zakres adresów IP sieci wirtualnej (np. 10.1.0.0/16 na diagramie scenariusz) > Wybierz nową witrynę Azure > **OK**.

## <a name="create-an-azure-virtual-network"></a>Tworzenie sieci wirtualnej platformy Azure
Aby utworzyć sieć wirtualną platformy Azure i konfigurowania sieci VPN typu lokacja lokacja, wykonaj kroki zawarte w artykule [utworzyć połączenie lokacja-lokacja](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). 

Można także skonfigurować bramę sieci wirtualnej do utworzenia bezpiecznego połączenia sieci VPN typu lokacja lokacja. Utwórz połączenie sieci VPN lokacja lokacja między nową sieć wirtualną i lokalnego urządzenia sieci VPN. Aby uzyskać instrukcje, zobacz [należy skonfigurować bramę sieci wirtualnej](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="create-azure-vms-for-the-dc-roles"></a>Tworzenie maszyn wirtualnych platformy Azure dla ról kontrolera domeny
Do tworzenia maszyn wirtualnych do obsługi roli kontrolera domeny, powtórz kroki [Utwórz maszynę wirtualną z systemem Windows przy użyciu portalu Azure](../virtual-machines/windows/quick-create-portal.md) zgodnie z potrzebami. Wdrażanie co najmniej dwóch wirtualnych kontrolerów domeny w celu zapewnienia odporności na uszkodzenia i nadmiarowość. Jeśli sieć wirtualna platformy Azure ma co najmniej dwa kontrolery domeny, które podobnie są skonfigurowane, można umieścić te kontrolery domeny z systemem w zestawie ulepszone odporności na uszkodzenia dostępności maszyn wirtualnych.

Aby utworzyć maszyn wirtualnych przy użyciu programu Windows PowerShell zamiast portalu Azure, zobacz [użycia programu Azure PowerShell do tworzenia i wstępnie skonfigurować maszyn wirtualnych z systemem Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Zarezerwuj statyczny adres IP dla maszyn wirtualnych, które będą uruchamiane roli kontrolera domeny. Aby Zarezerwuj statyczny adres IP, Pobierz Instalator platformy sieci Web firmy Microsoft i [zainstalować program Azure PowerShell](/powershell/azure/overview) i uruchom polecenie cmdlet Set-AzureStaticVNetIP. Na przykład:

````
Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM
````
Aby uzyskać więcej informacji na temat ustawiania statycznego adresu IP, zobacz [skonfigurować statyczny adres IP wewnętrznego dla maszyny Wirtualnej,](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Instalowanie usług AD DS na maszynach wirtualnych Azure
Zaloguj się do maszyny Wirtualnej i sprawdź, czy istnieje połączenie przez połączenie sieci VPN i ExpressRoute lokacja lokacja z zasobami w sieci lokalnej. Następnie należy zainstalować usługi AD DS na maszynach wirtualnych Azure. Można użyć tego samego procesu, który umożliwia instalowanie dodatkowego kontrolera domeny w sieci lokalnej (interfejsu użytkownika, środowiska Windows PowerShell lub pliku odpowiedzi). Jak zainstalować usługi AD DS, upewnij się, że określenie nowego woluminu dla lokalizacji bazy danych, dzienników i folderu SYSVOL usługi AD. Jeśli potrzebujesz odświeżacz w instalacji usług AD DS, zobacz [Instalowanie usług Active Directory domeny (poziom 100)](https://technet.microsoft.com/library/hh472162.aspx) lub [instalacji kontrolera domeny repliki systemu Windows Server 2012 w istniejącej domenie (poziom 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Ponownie skonfiguruj serwer DNS dla sieci wirtualnej
1. W celu uzyskania listy nazw sieci wirtualnej w [portalu Azure](https://portal.azure.com), wyszukaj *sieci wirtualnych*, a następnie wybierz pozycję **sieci wirtualnych** do wyświetlania listy. 
2. Otwórz sieci wirtualnej, którymi chcesz zarządzać, a następnie [zmienić konfigurację adresów IP serwerów DNS dla sieci wirtualnej](../virtual-network/manage-virtual-network.md#change-dns-servers) do używania statycznych adresów IP przypisanych do replik kontrolerów domeny zamiast adresów IP dla lokalnych serwerów DNS.
3. Zapewnienie, że wszystkie repliki kontrolera domeny maszyn wirtualnych w sieci wirtualnej skonfigurowano do korzystania z serwerów DNS w sieci wirtualnej:
  1. Wybierz **maszyn wirtualnych**.
  2. Wybierz maszyny wirtualne, a następnie wybierz **ponownego uruchomienia**. 
  3. Poczekaj na Maszynie wirtualnej **systemem** ponownie, a następnie zaloguj się do niego.

## <a name="create-vms-for-application-servers"></a>Tworzenie maszyn wirtualnych dla serwerów aplikacji

Do tworzenia maszyn wirtualnych do obsługi roli serwera aplikacji, powtórz kroki [Utwórz maszynę wirtualną z systemem Windows przy użyciu portalu Azure](../virtual-machines/windows/quick-create-portal.md) zgodnie z potrzebami. Aby utworzyć maszyn wirtualnych przy użyciu Microsoft PowerShell zamiast portalu Azure, zobacz [użycia programu Azure PowerShell do tworzenia i konfigurowania maszyn wirtualnych z systemem Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Poniższa tabela zawiera sugerowane ustawienia.

| Ustawienie | Wartości |
| --- | --- |
|  **Wybierz obraz** | Windows Server 2012 R2 Datacenter |
|  **Konfiguracja maszyny wirtualnej** |<p>Nazwa maszyny wirtualnej: Wpisz nazwy o pojedynczej etykiecie (na przykład AppServer1).</p><p>Nowa nazwa użytkownika: Wpisz nazwę użytkownika. Ten użytkownik będzie członkiem lokalnej grupy Administratorzy na maszynie Wirtualnej. Konieczne będzie tę nazwę, aby zalogować się do maszyny Wirtualnej po raz pierwszy. Wbudowane konto o nazwie Administrator nie będzie działać.</p><p>Nowe hasło/Potwierdź: Wpisz hasło</p> |
|  **Konfiguracja maszyny wirtualnej** |<p>Usługi w chmurze: Wybierz **Utwórz nową usługę w chmurze** dla pierwsza maszyna wirtualna, a następnie wybierz tego samego nazwa usługi w chmurze podczas tworzenia maszyn wirtualnych więcej będzie hosta aplikacji.</p><p>Nazwa DNS usługi w chmurze: Określ globalnie unikatowa nazwa</p><p>Region/grupy koligacji/sieci wirtualnej: Określ nazwę sieci wirtualnej (np. WestUSVNet).</p><p>Konto magazynu: Wybierz **użyć konta magazynu automatycznie generowanych** dla pierwszej maszyny Wirtualnej, a następnie wybierz tej samej nazwy konta magazynu podczas tworzenia więcej maszyn wirtualnych, które będą hosta aplikacji.</p><p>Zestaw dostępności: Wybierz **tworzenia zestawu dostępności**.</p><p>Nazwa zbioru dostępności: wpisz nazwę zestawu dostępności podczas tworzenie pierwszej maszyny Wirtualnej, a następnie wybierz takie same nazwy podczas tworzenia więcej maszyn wirtualnych.</p> |
|  **Konfiguracja maszyny wirtualnej** |<p>Wybierz <b>Zainstaluj agenta maszyny Wirtualnej</b> oraz innych rozszerzeń należy.</p> |
  
Po udostępnieniu każdej maszyny Wirtualnej, zaloguj się i przyłączyć go do domeny. 
1. W **Menedżera serwera** &gt; **lokalnego serwera** &gt; **grupy roboczej** &gt; **zmian...** , wybierz pozycję **domeny**.
2. Wprowadź nazwę domeny lokalnej. 
3. Podaj poświadczenia użytkownika domeny.
4. Uruchom ponownie maszynę wirtualną.

## <a name="additional-resources"></a>Zasoby dodatkowe

* Aby uzyskać więcej informacji o korzystaniu z programu Windows PowerShell, zobacz [wprowadzenie do poleceń cmdlet Azure](/powershell/azure/overview) i [dokumentacji poleceń Cmdlet Azure](/powershell/azure/get-started-azureps).
* [Wskazówki dotyczące wdrażania systemu Windows serwer usługi Active Directory na maszynach wirtualnych Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Jak przekazywać istniejących kontrolerów domeny funkcji Hyper-V lokalnej na platformie Azure przy użyciu programu Azure PowerShell](http://support.microsoft.com/kb/2904015)
* [Instalowanie nowego lasu usługi Active Directory w sieci wirtualnej platformy Azure](active-directory-new-forest-virtual-machine.md)
* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Microsoft Azure IaaS specjalista IT: (01) maszyny wirtualnej podstawy](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IaaS specjalista IT: (05) tworzenie sieci wirtualnych i łączności między lokalizacjami](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Azure PowerShell](/powershell/azure/overview)
* [Polecenia cmdlet do zarządzania platformy Azure](/powershell/module/azurerm.compute/#virtual_machines)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
